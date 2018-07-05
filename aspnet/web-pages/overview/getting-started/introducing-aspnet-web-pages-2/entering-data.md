---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
title: 'Introducción a las páginas Web ASP.NET: introducción de datos de base de datos mediante el uso de formularios | Microsoft Docs'
author: tfitzmac
description: Este tutorial muestra cómo crear un formulario de entrada y, a continuación, escriba los datos que se obtiene desde el formulario en una tabla de base de datos al usar ASP.NET Web Pages (...)
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: d37c93fc-25fd-4e94-8671-0d437beef206
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
msc.type: authoredcontent
ms.openlocfilehash: 41122b3bca5a3d3162a66be163642610b8349cc5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386412"
---
<a name="introducing-aspnet-web-pages---entering-database-data-by-using-forms"></a>Introducción a las páginas Web ASP.NET: introducción de datos de base de datos mediante el uso de formularios
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial muestra cómo crear un formulario de entrada y, a continuación, escriba los datos que se obtiene desde el formulario en una tabla de base de datos al usar ASP.NET Web Pages (Razor). Supone que ha completado la serie a través de [conceptos básicos de los formularios HTML en ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251581).
> 
> Lo que aprenderá:
> 
> - Más información acerca de cómo procesar los formularios de entrada.
> - Cómo agregar datos (insert) en una base de datos.
> - Cómo asegurarse de que los usuarios han escrito un valor requerido en un formulario (cómo validar entradas de usuario).
> - Cómo mostrar errores de validación.
> - Cómo saltar a otra página de la página actual.
>   
> 
> Características y tecnologías tratadas:
> 
> - Método `Database.Execute`.
> - El código SQL `Insert Into` instrucción
> - El `Validation` auxiliar.
> - Método `Response.Redirect`.


## <a name="what-youll-build"></a>¿Qué va a crear

En el tutorial anterior que mostraba cómo crear una base de datos, escribió la base de datos mediante la edición de la base de datos directamente en WebMatrix, trabajar el **base de datos** área de trabajo. En la mayoría de las aplicaciones que no es una forma práctica para colocar datos en la base de datos, sin embargo. Por lo que en este tutorial, creará una interfaz basada en web que permite a usted o alguien escribir datos y guardarlo en la base de datos.

Creará una página donde puede especificar nuevas películas. La página contendrá un formulario de entrada que tiene campos (cuadros de texto), donde puede escribir un título de la película, el género y año. La página se parecerá a esta página:

![Página "Agregar la película" en el explorador](entering-data/_static/image1.png)

Los cuadros de texto será HTML `<input>` elementos que tendrá un aspecto como este marcado:

`<input type="text" name="genre" value="" />`

## <a name="creating-the-basic-entry-form"></a>Crear el formulario de entrada básico

Cree una página denominada *AddMovie.cshtml*.

Reemplazar lo que está en el archivo con el marcado siguiente. Sobrescribir todo... en breve agregará un bloque de código en la parte superior.

[!code-cshtml[Main](entering-data/samples/sample1.cshtml)]

Este ejemplo muestra para la creación de un formulario HTML típico. Lo usa `<input>` elementos para los cuadros de texto y para el botón Enviar. Los títulos de los cuadros de texto se crean mediante el estándar `<label>` elementos. El `<fieldset>` y `<legend>` elementos situar un agradable cuadro alrededor del formulario.

Tenga en cuenta que en esta página, el `<form>` elemento utiliza `post` como el valor de la `method` atributo. En el tutorial anterior, creó un formulario que utiliza el `get` método. Que fue correcta, porque aunque el formulario envía al servidor los valores, la solicitud no realizó ningún cambio. Todo lo que hice fue recuperar datos de maneras diferentes. Sin embargo, en esta página le *le* realizar cambios, va a agregar nuevos registros de base de datos. Por lo tanto, debe usar este formulario el `post` método. (Para obtener más información sobre la diferencia entre `GET` y `POST` operaciones, vea el[GET, POST y HTTP verbo seguridad](https://go.microsoft.com/fwlink/?LinkId=251581#GET,_POST,_and_HTTP_Verb_Safety) sidebar en el tutorial anterior.)

Tenga en cuenta que cada cuadro de texto tiene un `name` elemento (`title`, `genre`, `year`). Como vimos en el tutorial anterior, estos nombres son importantes porque esos nombres debe tener por lo que puede obtener la entrada del usuario más tarde. Puede utilizar cualquier nombre. Resulta útil usar nombres descriptivos que le ayudarán a recordar qué datos se trabaja con.

El `value` atributo de cada `<input>` elemento contiene un fragmento de código de Razor (por ejemplo, `Request.Form["title"]`). Una versión de este truco ha aprendido en el tutorial anterior para conservar el valor especificado en el cuadro de texto (si existe) después de enviar el formulario.

## <a name="getting-the-form-values"></a>Obtener los valores de formulario

A continuación, agregue código que procesa el formulario. En el esquema, deberá hacer lo siguiente:

1. Compruebe si se está publicando la página (se ha enviado). Desea el código para ejecutarse únicamente cuando los usuarios hacen clic en el botón, no cuando la página se ejecuta por primera vez.
2. Obtener los valores que el usuario especificado en los cuadros de texto. En este caso, porque el formulario usa el `POST` verbo, obtendrá los valores de formulario de la `Request.Form` colección.
3. Inserte los valores como un nuevo registro en el *películas* tabla de base de datos.

En la parte superior del archivo, agregue el código siguiente:

[!code-cshtml[Main](entering-data/samples/sample2.cshtml)]

Las primeras líneas crean variables (`title`, `genre`, y `year`) para contener los valores de los cuadros de texto. La línea `if(IsPost)` asegura que las variables se establecen *solo* cuando los usuarios hacen clic los **película agregar** botón, es decir, cuando se ha registrado el formulario.

Como se vio en un tutorial anterior, obtener el valor de un cuadro de texto mediante el uso de una expresión como `Request.Form["name"]`, donde *nombre* es el nombre de la `<input>` elemento.

Los nombres de las variables (`title`, `genre`, y `year`) son arbitrarios. Al igual que los nombres que se asignan a `<input>` elementos, puede llamar a los que desee. (Los nombres de las variables no tienen que coincidir con los atributos de nombre de `<input>` elementos en el formulario.) Pero al igual que con el `<input>` elementos, es una buena idea usar nombres de variable que reflejen los datos que contienen. Cuando escribe código, con nombres coherentes resultará más fácil recordar los datos que está trabajando con.

## <a name="adding-data-to-the-database"></a>Agregar datos a la base de datos

En el código de bloque que recién agregada, simplemente *dentro de* la llave de cierre ( `}` ) de la `if` bloque (no solo dentro del bloque de código), agregue el código siguiente:

[!code-csharp[Main](entering-data/samples/sample3.cs)]

En este ejemplo es similar al código que usó en un tutorial anterior a la captura y la presentación de datos. La línea que empieza con `db =` se abre la base de datos, como antes y la línea siguiente define nuevo de una instrucción SQL, como hemos visto antes. Sin embargo, esta vez define una instancia de SQL `Insert Into` instrucción. El ejemplo siguiente muestra la sintaxis general de la `Insert Into` instrucción:

`INSERT INTO table (column1, column2, column3, ...) VALUES (value1, value2, value3, ...)`

En otras palabras, especifique la tabla para insertar, a continuación, incluya las columnas que se va a insertar en y, a continuación, se enumeran los valores que se va a insertar. (Como se indicó antes, SQL no distingue mayúsculas de minúsculas, pero algunas personas aprovechar las palabras clave para que sea más fácil de leer el comando).

Las columnas que se están insertando ya aparecen en el comando: `(Title, Genre, Year)`. La parte interesante es cómo obtener los valores de los cuadros de texto en el `VALUES` parte del comando. En lugar de los valores reales, verá `@0`, `@1`, y `@2`, que por supuesto son marcadores de posición. Al ejecutar el comando (en la `db.Execute` línea), pasar los valores que obtuvo en los cuadros de texto.

**¡Importante!** Recuerde que la única manera de nunca debe incluir datos en línea escritos por un usuario en una instrucción SQL es usar marcadores de posición, como puede ver aquí (`VALUES(@0, @1, @2)`). Si concatenar proporcionados por el usuario en una instrucción SQL, estará abierto a un ataque de inyección de código SQL, como se explica en [conceptos básicos de formularios en ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251581) (el tutorial anterior).

Todavía en el `if` en bloques, agregue la siguiente línea después de la `db.Execute` línea:

[!code-css[Main](entering-data/samples/sample4.css)]

Después de la nueva película se ha insertado en la base de datos, esta línea se (redirecciones) salta a la *películas* página para que pueda ver la película que acaba de escribir. El `~` operador significa "" raíz"del sitio Web. (El `~` operador solo funciona en las páginas ASP.NET, no en HTML con carácter general.)

El bloque de código completo es similar a este ejemplo:

[!code-cshtml[Main](entering-data/samples/sample5.cshtml)]

## <a name="testing-the-insert-command-so-far"></a>Probar el comando de inserción (hasta)

Aún no ha terminado, pero ahora es un buen momento para probar.

En la vista de árbol de archivos en WebMatrix, haga clic en el *AddMovie.cshtml* página y, a continuación, haga clic en **iniciar en el explorador**.

![Página "Agregar la película" en el explorador](entering-data/_static/image2.png)

(Si termina con una página diferente en el explorador, asegúrese de que la dirección URL es `http://localhost:nnnnn/AddMovie`), donde *nnnnn* es el número de puerto que esté usando.)

¿Ha recibido una página de error? Si es así, léalo atentamente y asegurarse de que el código es exactamente lo que se muestra anteriormente.

Especifique una película en el formulario &mdash; por ejemplo, use "Ciudadanos Kane", "Drama" y "1941". (O cualquier otra cosa.) A continuación, haga clic en **película agregar**.

Si todo va bien, se le redirigirá a la *películas* página. Asegúrese de que aparece la nueva película.

![Página de películas que muestra recién agregado película](entering-data/_static/image3.png)

## <a name="validating-user-input"></a>Validar la entrada del usuario

Vuelva a la *AddMovie* página o vuelva a ejecutar. Escriba otra película, pero esta vez, especifique solo el título &mdash; por ejemplo, escriba "Cantando ' en el Rain". A continuación, haga clic en **película agregar**.

Se le redirigirá a la *películas* página nuevo. Puede encontrar la nueva película, pero está incompleto.

![Página de películas que muestra nueva película que falta algunos valores](entering-data/_static/image4.png)

Cuando creó la *películas* tabla, explícitamente dijiste que ninguno de los campos podría ser nulo. Aquí tiene un formulario de entrada de nuevas películas, y se dejan los campos en blanco. Es un error.

En este caso, realmente no provoca la base de datos (o *throw*) un error. No ha proporcionado un género o un año, por lo que el código en el *AddMovie* página trata estos valores como llamados *cadenas vacías*. Cuando el código SQL `Insert Into` se ejecutó el comando, los campos de género y año no tienen datos útiles en ellos, pero no eran null.

Obviamente, no desea que los usuarios puedan especificar información de la película medio vacío en la base de datos. La solución consiste en validar la entrada del usuario. Inicialmente, la validación simplemente asegurará que el usuario ha escrito un valor para todos los campos (es decir, que ninguna de ellas contiene una cadena vacía).

> [!TIP]
> 
> **Cadenas NULL y vacías**
> 
> En la programación, hay una diferencia entre diferentes nociones de "sin valor". En general, es un valor *null* si nunca se ha establecido o inicializarse de ninguna manera. En cambio, se puede establecer una variable que se espera que los datos de caracteres (cadenas) en un *una cadena vacía*. En ese caso, el valor no es null; solo se han se establece explícitamente en una cadena de caracteres cuya longitud es cero. Estas dos instrucciones muestran la diferencia:
> 
> [!code-csharp[Main](entering-data/samples/sample6.cs)]
> 
> Es un poco más complicado que eso, pero lo importante es que `null` representa un tipo de estado indeterminado.
> 
> Ahora y, a continuación, es importante entender exactamente cuando un valor es null y cuándo es simplemente una cadena vacía. En el código para el *AddMovie* página, obtener los valores de los cuadros de texto mediante `Request.Form["title"]` y así sucesivamente. Cuando la página ejecuta primero (antes de hacer clic en el botón), el valor de `Request.Form["title"]` es null. Pero cuando se envía el formulario, `Request.Form["title"]` Obtiene el valor de la `title` cuadro de texto. No es obvio, pero un cuadro de texto vacío no es null; solo se tiene una cadena vacía en ella. Cuando se ejecuta el código en respuesta al botón, haga clic en, `Request.Form["title"]` tiene una cadena vacía en ella.
> 
> ¿Por qué es importante esta distinción? Cuando creó la *películas* tabla, explícitamente dijiste que ninguno de los campos podría ser nulo. Pero aquí tiene un formulario de entrada de nuevas películas, y se dejan los campos en blanco. Cabe esperar la base de datos se quejará cuando intentó guardar nuevas películas que no tienen valores para el género o un año. Pero eso es el punto de &mdash; incluso si se deja en blanco los cuadros de texto, los valores no null; están las cadenas vacías. Como resultado, podemos guardar películas nuevas en la base de datos con estas columnas vacías &mdash; pero no es null. valores &mdash;. Por lo tanto, tendrá que asegurarse de que los usuarios no enviar una cadena vacía, lo que puede hacer mediante la validación de la entrada del usuario.


### <a name="the-validation-helper"></a>Aplicación auxiliar de validación

Las páginas Web ASP.NET incluye una aplicación auxiliar &mdash; el `Validation` auxiliar &mdash; que puede usar para asegurarse de que los usuarios introduzcan datos que cumpla sus requisitos. El `Validation` auxiliar es una de las aplicaciones auxiliares que se basa ASP.NET Web Pages, por lo que no tiene que instalar como un paquete mediante el uso de NuGet, la manera en que instaló la aplicación auxiliar de Gravatar en un tutorial anterior.

Para validar la entrada del usuario, deberá hacer lo siguiente:

- Usar código para especificar que desea que requieren valores en los cuadros de texto en la página.
- Coloque una prueba en el código para que la información de la película se agrega a la base de datos solo si todo se valida correctamente.
- Agregue el código en el marcado para mostrar los mensajes de error.

En el bloque de código en el *AddMovie* página, derecha arriba en la parte superior antes de las declaraciones de variable, agregue el código siguiente:

[!code-csharp[Main](entering-data/samples/sample7.cs)]

Se llama a `Validation.RequireField` una vez para cada campo (`<input>` elemento) donde desea requerir una entrada. También puede agregar un mensaje de error personalizado para cada llamada, tal como se muestra aquí. (Se varían los mensajes solo para mostrar que puede colocar cualquier cosa que no existe).

Si hay un problema, puede evitar la nueva información de la película desde el que se inserta en la base de datos. En el `if(IsPost)` en bloques, utilice `&&` (AND lógico) para agregar otra condición que prueba `Validation.IsValid()`. Cuando haya terminado, la totalidad `if(IsPost)` bloque parece este código:

[!code-csharp[Main](entering-data/samples/sample8.cs)]

Si se produce un error de validación con cualquiera de los campos que registraron usando el `Validation` auxiliar, el `Validation.IsValid` método devuelve false. Y en ese caso, ninguno de los códigos en ese bloque se ejecutará, por lo que no hay entradas de la película no válido se insertará en la base de datos. Y por supuesto no le redirigirá a la *películas* página.

El bloque de código completo, incluido el código de validación, se parece ahora a este ejemplo:

[!code-cshtml[Main](entering-data/samples/sample9.cshtml?highlight=10)]

## <a name="displaying-validation-errors"></a>Mostrar errores de validación

El último paso es mostrar los mensajes de error. Puede mostrar los mensajes individuales para cada error de validación, o puede mostrar un resumen, o ambos. Para este tutorial, llevará a cabo tanto por lo que puede ver cómo funciona.

Junto a cada `<input>` elemento que se está validando, llamada la `Html.ValidationMessage` método y pásele el nombre de la `<input>` elemento que se está validando. Coloca el `Html.ValidationMessage` método justo donde desea que aparezca el mensaje de error. Cuando se ejecuta la página, el `Html.ValidationMessage` método representa un `<span>` elemento dónde irá el error de validación. (Si no hay ningún error, el `<span>` se representa el elemento, pero no hay ningún texto en él.)

Cambie el marcado en la página para que incluya un `Html.ValidationMessage` método para cada una de las tres `<input>` elementos en la página, como en este ejemplo:

[!code-cshtml[Main](entering-data/samples/sample10.cshtml?highlight=3,8,13)]

Para ver cómo funciona el resumen, también agregue el siguiente marcado y código justo después de la `<h1>Add a Movie</h1>` elemento en la página:

[!code-cshtml[Main](entering-data/samples/sample11.cshtml)]

De forma predeterminada, el `Html.ValidationSummary` método muestra todos los mensajes de validación en una lista (un `<ul>` elemento que está dentro de un `<div>` elemento). Igual que con el `Html.ValidationMessage` método, siempre se representa el marcado para el resumen de validación; si no hay ningún error, no hay elementos de lista se representan.

El resumen puede ser una manera alternativa de mostrar mensajes de validación en lugar de mediante el `Html.ValidationMessage` método para mostrar cada error específico del campo. O bien, puede usar un resumen y los detalles. O bien puede usar el `Html.ValidationSummary` método para mostrar un error genérico y, a continuación, usar individuales `Html.ValidationMessage` llamadas para mostrar los detalles.

La página completa se parece ahora a este ejemplo:

[!code-cshtml[Main](entering-data/samples/sample12.cshtml)]

Ya está. Ahora puede probar la página mediante la adición de una película pero dejando fuera uno o varios de los campos. Al hacerlo, se ve el error siguiente:

![Agregar página de la película que muestra los mensajes de error de validación](entering-data/_static/image5.png)

## <a name="styling-the-validation-error-messages"></a>Aplicar estilos a los mensajes de Error de validación

Puede ver que hay mensajes de error, pero no realmente destacarse muy bien. Hay una manera fácil para definir el estilo de los mensajes de error, sin embargo.

Para definir el estilo de los mensajes de errores individuales que se muestran de forma `Html.ValidationMessage`, cree una clase de estilo CSS denominada `field-validation-error`. Para definir el aspecto para el resumen de validación, cree una clase de estilo CSS denominada `validation-summary-errors`.

Para ver cómo funciona esta técnica, agregue un `<style>` elemento dentro de la `<head>` sección de la página. A continuación, defina las clases de estilo denominadas `field-validation-error` y `validation-summary-errors` que contienen las siguientes reglas:

[!code-cshtml[Main](entering-data/samples/sample13.cshtml?highlight=4-17)]

Normalmente, probablemente colocaría la información de estilo en otro *.css* archivo, pero por motivos de simplicidad puede colocarlas en la página por ahora. (Más adelante en esta serie de tutoriales, podrá mover las reglas de CSS a otro *.css* archivo.)

Si se produce un error de validación, el `Html.ValidationMessage` método representa un `<span>` elemento que incluya `class="field-validation-error"`. Mediante la adición de una definición de estilo para esa clase, puede configurar el aspecto que tiene el mensaje. Si hay errores, el `ValidationSummary` método igualmente dinámicamente representa el atributo `class="validation-summary-errors"`.

Vuelva a ejecutar la página y olvidó deliberadamente un par de los campos. Los errores son ahora más fáciles. (De hecho, se usa en exceso, pero que es simplemente para demostrar lo que puede hacer).

![Agregar página de la película que muestra los errores de validación que se han aplicado un estilo](entering-data/_static/image6.png)

## <a name="adding-a-link-to-the-movies-page"></a>Agregar un vínculo a la página de películas

Un paso final es que resulte conveniente llegar a la *AddMovie* página desde la lista de películas original.

Abra el *películas* página nuevo. Después del cierre `</div>` etiqueta que sigue el `WebGrid` auxiliar, agregue el siguiente marcado:

[!code-cshtml[Main](entering-data/samples/sample14.cshtml)]

Como vimos antes, ASP.NET interpreta el `~` operador como la raíz del sitio Web. No tiene que usar el `~` operador; podría usar el marcado `<a href="./AddMovie">Add a movie</a>` o alguna otra forma de definir la ruta de acceso que entiende de HTML. Pero la `~` operador es un buen enfoque general al crear vínculos a páginas de Razor, ya que hace que el sitio más flexible, si mueve la página actual en una subcarpeta, el vínculo se dirigirán a la *AddMovie* página. (Recuerde que el `~` operador solo funciona *.cshtml* páginas. ASP.NET lo entiende, pero no es HTML estándar).

Cuando haya terminado, ejecute el *películas* página. Será similar a esta página:

![Página de películas con vínculo a la página 'Agregar películas'](entering-data/_static/image7.png)

Haga clic en el **agregar una película** vínculo para asegurarse de que va a la *AddMovie* página.

## <a name="coming-up-next"></a>Próxima

En el siguiente tutorial, obtendrá información sobre cómo permitir que los usuarios editar los datos que ya está en la base de datos.

## <a name="complete-listing-for-addmovie-page"></a>Lista completa de página AddMovie

[!code-cshtml[Main](entering-data/samples/sample15.cshtml)]

## <a name="additional-resources"></a>Recursos adicionales

- [Introducción a la programación Web de ASP.NET mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Instrucción SQL INSERT INTO](http://www.w3schools.com/sql/sql_insert.asp) en el sitio W3Schools
- [Validar la entrada del usuario en ASP.NET Web Pages sitios](https://go.microsoft.com/fwlink/?LinkId=253002). Para obtener más información sobre cómo trabajar con el `Validation` auxiliar.

> [!div class="step-by-step"]
> [Anterior](form-basics.md)
> [Siguiente](updating-data.md)
