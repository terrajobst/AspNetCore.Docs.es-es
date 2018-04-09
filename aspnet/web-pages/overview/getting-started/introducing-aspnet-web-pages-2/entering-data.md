---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
title: 'Introducción a ASP.NET Web Pages: escribir datos de la base de datos mediante el uso de formularios | Documentos de Microsoft'
author: tfitzmac
description: Este tutorial muestra cómo crear un formulario de entrada y, a continuación, escriba los datos que se obtienen de la forma en una tabla de base de datos cuando se usa ASP.NET Web Pages (...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: d37c93fc-25fd-4e94-8671-0d437beef206
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
msc.type: authoredcontent
ms.openlocfilehash: bbccf8134e90c19e29efaa5afe1e46e15320c189
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="introducing-aspnet-web-pages---entering-database-data-by-using-forms"></a>Introducción a ASP.NET Web Pages: escribir datos de la base de datos mediante el uso de formularios
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial muestra cómo crear un formulario de entrada y, a continuación, escriba los datos que se obtienen de la forma en una tabla de base de datos cuando se usa ASP.NET Web Pages (Razor). Supone que ha completado la serie a través de [conceptos básicos de los formularios HTML en ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251581).
> 
> Lo que aprenderá:
> 
> - Más información acerca de cómo procesar los formularios de entrada.
> - Cómo agregar datos (inserción) en una base de datos.
> - Cómo asegurarse de que los usuarios han escrito un valor necesario en un formulario (cómo validar proporcionados por el usuario).
> - Cómo mostrar errores de validación.
> - Cómo saltar a otra página de la página actual.
>   
> 
> Características y tecnologías que se tratan:
> 
> - Método `Database.Execute`.
> - La instrucción SQL `Insert Into` instrucción
> - El `Validation` auxiliar.
> - Método `Response.Redirect`.


## <a name="what-youll-build"></a>Lo que vamos a compilar

En el tutorial anterior que se ha explicado cómo crear una base de datos, especificar la base de datos mediante la edición de la base de datos directamente en WebMatrix, trabajar la **base de datos** área de trabajo. En la mayoría de las aplicaciones, que no es una forma práctica para colocar datos en la base de datos, aunque. Por lo que en este tutorial, creará una interfaz basada en web que permite a usted o cualquiera que escriba los datos y la guarda en la base de datos.

Podrá crear una página donde puede escribir nuevas películas. La página contendrá un formulario de entrada que tiene campos (cuadros de texto), donde puede escribir un título de la película, género y año. La página tendrá un aspecto similar a esta página:

![Página 'Agregar película' en el explorador](entering-data/_static/image1.png)

Los cuadros de texto será HTML `<input>` elementos que tendrá un aspecto como este marcado:

`<input type="text" name="genre" value="" />`

## <a name="creating-the-basic-entry-form"></a>Crear el formulario de entrada básico

Crear una página denominada *AddMovie.cshtml*.

Sustituir lo que aparece en el archivo con el siguiente marcado. Sobrescribir todos los elementos; en breve, podrá agregar un bloque de código en la parte superior.

[!code-cshtml[Main](entering-data/samples/sample1.cshtml)]

Este ejemplo muestra HTML típico para crear un formulario. Usa `<input>` elementos para los cuadros de texto y para el botón Enviar. Los títulos de los cuadros de texto se crean mediante estándar `<label>` elementos. El `<fieldset>` y `<legend>` elementos colocan un cuadro nice alrededor del formulario.

Observe que en esta página, el `<form>` elemento usa `post` como el valor de la `method` atributo. En el tutorial anterior, creó un formulario que utiliza el `get` método. Que es correcto, porque aunque valores para el servidor envía el formulario, la solicitud no hizo ningún cambio. Todo lo que hicimos fue recuperar datos de maneras diferentes. Sin embargo, en esta página se *le* realizar cambios, va a agregar nuevos registros de base de datos. Por lo tanto, debe usar este formulario el `post` método. (Para obtener más información acerca de las diferencias entre `GET` y `POST` operaciones, consulte el[GET, POST y HTTP verbo seguridad](https://go.microsoft.com/fwlink/?LinkId=251581#GET,_POST,_and_HTTP_Verb_Safety) sidebar en el tutorial anterior.)

Tenga en cuenta que cada cuadro de texto tiene un `name` elemento (`title`, `genre`, `year`). Como vimos en el tutorial anterior, estos nombres son importantes porque deben tener los nombres para que puedas más adelante la entrada del usuario. Puede utilizar cualquier nombre. Resulta útil usar nombres descriptivos que le ayudarán a recordar qué datos se trabaja con.

El `value` atributo de cada `<input>` elemento contiene un poco de código Razor (por ejemplo, `Request.Form["title"]`). Una versión de este truco también ha aprendido en el tutorial anterior para conservar el valor especificado en el cuadro de texto (si existe) después de que se ha enviado el formulario.

## <a name="getting-the-form-values"></a>Obtener los valores de formulario

A continuación, agregue código que procesa el formulario. En el esquema, deberá hacer lo siguiente:

1. Compruebe si se está publicando la página (envió). Desea el código para ejecutarse únicamente cuando los usuarios hacen clic en el botón, no cuando la página se ejecuta por primera vez.
2. Obtiene los valores que el usuario ha escrito en los cuadros de texto. En este caso, porque el formulario está usando la `POST` verbo, obtendrá los valores del formulario de la `Request.Form` colección.
3. Inserte los valores como un nuevo registro en el *películas* tabla de base de datos.

En la parte superior del archivo, agregue el código siguiente:

[!code-cshtml[Main](entering-data/samples/sample2.cshtml)]

Las primeras líneas crean variables (`title`, `genre`, y `year`) para almacenar los valores de los cuadros de texto. La línea `if(IsPost)` se asegura de que se establecen las variables *sólo* cuando los usuarios hacen clic el **película agregar** botón, es decir, cuando se ha registrado el formulario.

Como se vio en un tutorial anterior, obtendrá el valor de un cuadro de texto mediante el uso de una expresión como `Request.Form["name"]`, donde *nombre* es el nombre de la `<input>` elemento.

Los nombres de las variables (`title`, `genre`, y `year`) son arbitrarios. Al igual que los nombres que se asignan a `<input>` elementos, puede llamarlos que desee. (Los nombres de las variables no deben coincidir con los atributos de nombre de `<input>` elementos en el formulario.) Pero al igual que con el `<input>` elementos, es una buena idea usar nombres de variable que reflejen los datos que contienen. Cuando se escribe código, con nombres coherentes resulte más fácil de recordar qué datos se trabaja con.

## <a name="adding-data-to-the-database"></a>Agregar datos a la base de datos

En el código de bloque que acaba de agregar, simplemente *en* la llave de cierre ( `}` ) de la `if` bloquear (no solo dentro del bloque de código), agregue el código siguiente:

[!code-csharp[Main](entering-data/samples/sample3.cs)]

En este ejemplo es similar al código que se usa en un tutorial anterior para captura y mostrar los datos. La línea que comienza con `db =` se abre la base de datos, al igual que antes y la línea siguiente define nuevo de una instrucción SQL, como hemos visto antes. Sin embargo, este tiempo define una instancia de SQL `Insert Into` instrucción. En el ejemplo siguiente se muestra la sintaxis general de la `Insert Into` instrucción:

`INSERT INTO table (column1, column2, column3, ...) VALUES (value1, value2, value3, ...)`

En otras palabras, se especifican a continuación, muestre las columnas que se va a insertar en la tabla para insertar en y, a continuación, se enumeran los valores para insertar. (Como se indicó antes, SQL no distingue mayúsculas de minúsculas, pero algunas personas aprovechar las palabras clave para que sea más fácil de leer el comando).

Las columnas que se inserta en ya figuran en el comando: `(Title, Genre, Year)`. La parte interesante es cómo obtener los valores de los cuadros de texto en el `VALUES` parte del comando. En lugar de valores reales, verá `@0`, `@1`, y `@2`, que son, por supuesto, los marcadores de posición. Al ejecutar el comando (en la `db.Execute` línea), se pasan los valores que obtuvo en los cuadros de texto.

**¡Importante!** Recuerde que la única manera de nunca debe incluir datos en línea introducidos por un usuario en una instrucción SQL es usar marcadores de posición, como puede ver aquí (`VALUES(@0, @1, @2)`). Si se unen proporcionados por el usuario en una instrucción SQL, abrir usted mismo a un ataque de inyección de código SQL, como se explica en [conceptos básicos de formularios en ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251581) (el tutorial anterior).

Todavía en el `if` bloquear, agregue la siguiente línea después de la `db.Execute` línea:

[!code-css[Main](entering-data/samples/sample4.css)]

Después de la película nuevo se ha insertado en la base de datos, esta línea se (redirecciones) salta a la *películas* página para que pueda ver la película que acaba de escribir. El `~` operador significa "raíz del sitio Web". (El `~` operador solo funciona en las páginas ASP.NET, no en HTML generalmente.)

El bloque de código completo es similar a este ejemplo:

[!code-cshtml[Main](entering-data/samples/sample5.cshtml)]

## <a name="testing-the-insert-command-so-far"></a>Probar el comando de inserción (hasta ahora)

Aún no ha terminado, pero ahora es un buen momento para probar.

En la vista de árbol de archivos en WebMatrix, haga clic en el *AddMovie.cshtml* página y, a continuación, haga clic en **iniciar en un explorador**.

![Página 'Agregar película' en el explorador](entering-data/_static/image2.png)

(Si finalmente con otra página en el explorador, asegúrese de que la dirección URL es `http://localhost:nnnnn/AddMovie`), donde *nnnnn* es el número de puerto que está usando.)

¿Si obtuvo una página de error? Si es así, léalo atentamente y asegurarse de que el código tiene exactamente lo que se incluyó en versiones anteriores.

Especifique una película en el formulario &mdash; por ejemplo, use "Ciudadano Kane", "Series" y "1941". (O cualquier otra.) A continuación, haga clic en **película agregar**.

Si todo funciona correctamente, se le redirigirá a la *películas* página. Asegúrese de que aparece la película nuevo.

![Página de películas que muestra recién agregado película](entering-data/_static/image3.png)

## <a name="validating-user-input"></a>Validar la entrada del usuario

Vuelva a la *AddMovie* página o vuelva a ejecutarlo. Escriba otra película, pero esta vez, escriba solo el título &mdash; por ejemplo, escriba "Iniciar ' en el raicilla". A continuación, haga clic en **película agregar**.

Se le redirigirá a la *películas* página de nuevo. Puede encontrar la película nuevo, pero está incompleta.

![Película nueva que muestra que falta algunos valores de página de películas](entering-data/_static/image4.png)

Al crear el *películas* tabla, que explícitamente dijo que ninguno de los campos podría ser null. Aquí tiene un formulario de entrada de películas nuevas y se ausenta de campos en blanco. Es un error.

En este caso, realmente no provoca la base de datos (o *throw*) un error. No ha proporcionado un género y año, por lo que el código en el *AddMovie* página trata dichos valores como denominadas *cadenas vacías*. Cuando la instrucción SQL `Insert Into` comando se ejecutó, el género y año los campos no tienen datos útiles en ellos, pero que no estaban null.

Obviamente, no desea que los usuarios puedan especificar información de la película de medio en blanco en la base de datos. La solución consiste en validar la entrada del usuario. Inicialmente, la validación simplemente asegurará que el usuario ha escrito un valor para todos los campos (es decir, que ninguna de ellas contiene una cadena vacía).

> [!TIP]
> 
> **Cadenas NULL y vacías**
> 
> En la programación, hay una diferencia entre diferentes nociones de "sin valor". En general, es un valor *null* si nunca se ha establecido o inicializado de forma alguna. En cambio, se puede establecer una variable que espera datos de caracteres (cadenas) en un *una cadena vacía*. En ese caso, el valor no es null; solo se han se establece explícitamente en una cadena de caracteres cuya longitud es cero. Estas dos instrucciones muestran la diferencia:
> 
> [!code-csharp[Main](entering-data/samples/sample6.cs)]
> 
> Es un poco más complicado que, pero lo importante es que `null` representa un tipo de estado indeterminado.
> 
> Ahora y, a continuación, es importante entender exactamente cuando un valor es null y cuándo es simplemente una cadena vacía. En el código de la *AddMovie* página, se obtienen los valores de los cuadros de texto mediante el uso de `Request.Form["title"]` y así sucesivamente. Cuando primero se ejecuta la página (antes de hacer clic en el botón), el valor de `Request.Form["title"]` es null. Pero cuando se envía el formulario, `Request.Form["title"]` Obtiene el valor de la `title` cuadro de texto. No es obvio, pero un cuadro de texto vacío no es null; solo tiene una cadena vacía en ella. Cuando se ejecuta el código en respuesta al botón, haga clic en, `Request.Form["title"]` tiene una cadena vacía en ella.
> 
> ¿Por qué es importante esta distinción? Al crear el *películas* tabla, que explícitamente dijo que ninguno de los campos podría ser null. Pero aquí tiene un formulario de entrada de películas nuevas y se ausenta de campos en blanco. Se podría esperar razonablemente la base de datos se quejan al intentar guardar películas nuevas que no tienen valores para el género o año. Pero eso es el punto de &mdash; incluso si deja en blanco los cuadros de texto, los valores no null; están las cadenas vacías. Como resultado, usted puede guardar películas nuevas en la base de datos con estas columnas vacías &mdash; pero no es null. valores &mdash;. Por tanto, tendrá que asegurarse de que los usuarios no enviar una cadena vacía, lo que puede hacer mediante la validación de la entrada del usuario.


### <a name="the-validation-helper"></a>La aplicación auxiliar Validation

Páginas Web de ASP.NET incluye una aplicación auxiliar &mdash; el `Validation` auxiliar &mdash; que puede usar para asegurarse de que los usuarios escriben los datos que se adapte a sus necesidades. El `Validation` auxiliar es una de las aplicaciones auxiliares que se compila en ASP.NET Web Pages, por lo que no es necesario instalarlo como un paquete mediante el uso de NuGet, la manera en que instaló la aplicación auxiliar Gravatar en un tutorial anterior.

Para validar la entrada del usuario, deberá llevar a cabo lo siguiente:

- Utilice el código para especificar que desea que requieren valores en los cuadros de texto en la página.
- Colocar una prueba en el código para que la información de la película se agrega a la base de datos solo si todos los elementos se valida correctamente.
- Agregue código en el marcado para mostrar mensajes de error.

En el bloque de código en el *AddMovie* página, derecha hacia arriba en la parte superior antes de las declaraciones de variable, agregue el código siguiente:

[!code-csharp[Main](entering-data/samples/sample7.cs)]

Se llama a `Validation.RequireField` una vez para cada campo (`<input>` elemento) donde desea requerir una entrada. También puede agregar un mensaje de error personalizado para cada llamada, como puede ver aquí. (Se variar los mensajes solo para mostrar que puede colocar cualquier cosa que no existe).

Si hay un problema, puede evitar la nueva información de la película desde que se va a insertar en la base de datos. En el `if(IsPost)` bloquear, use `&&` (AND lógico) para agregar otra condición que prueba `Validation.IsValid()`. Cuando haya terminado, todo el `if(IsPost)` bloque parecido al este código:

[!code-csharp[Main](entering-data/samples/sample8.cs)]

Si se produce un error de validación con cualquiera de los campos que registró mediante el `Validation` auxiliar, el `Validation.IsValid` método devuelve false. Y en ese caso, ninguno de los códigos dentro de dicho bloque se ejecutará, por lo que no hay entradas de película no válido se insertará en la base de datos. Y por supuesto no le redirige a la *películas* página.

El bloque de código completo, incluido el código de validación, ahora es similar a este ejemplo:

[!code-cshtml[Main](entering-data/samples/sample9.cshtml?highlight=10)]

## <a name="displaying-validation-errors"></a>Mostrar errores de validación

Es el último paso mostrar los mensajes de error. Puede mostrar los mensajes individuales para cada error de validación, o se puede mostrar un resumen, o ambas cosas. Para este tutorial, llevará a cabo tanto para que pueda ver cómo funciona.

Junto a cada `<input>` elemento que se está validando, llame a la `Html.ValidationMessage` método y pásele el nombre de la `<input>` elemento que se está validando. Coloca el `Html.ValidationMessage` método justo donde desea que aparezca el mensaje de error. Cuando se ejecuta la página, el `Html.ValidationMessage` método representa un `<span>` elemento dónde se escribirá el error de validación. (Si no hay ningún error, el `<span>` se representa el elemento, pero no hay ningún texto en él.)

Cambiar el marcado en la página para que incluya un `Html.ValidationMessage` método para cada una de las tres `<input>` elementos en la página, al igual que en este ejemplo:

[!code-cshtml[Main](entering-data/samples/sample10.cshtml?highlight=3,8,13)]

Para ver cómo funciona el resumen, también agregue el siguiente marcado y código justo después de la `<h1>Add a Movie</h1>` elemento de la página:

[!code-cshtml[Main](entering-data/samples/sample11.cshtml)]

De forma predeterminada, el `Html.ValidationSummary` método muestra todos los mensajes de validación en una lista (un `<ul>` elemento que está dentro de un `<div>` elemento). Al igual que con el `Html.ValidationMessage` método, siempre se representa el marcado para el resumen de validación; si no hay ningún error, no hay elementos de lista se representan.

El resumen puede ser una manera alternativa de mostrar mensajes de validación en lugar de mediante el `Html.ValidationMessage` método para mostrar cada error específico del campo. O bien, puede usar un resumen y los detalles. O bien puede usar el `Html.ValidationSummary` método para mostrar un error genérico y, a continuación, utilizar individuales `Html.ValidationMessage` llamadas para mostrar los detalles.

Ahora, la página completa es similar a este ejemplo:

[!code-cshtml[Main](entering-data/samples/sample12.cshtml)]

Ya está. Ahora puede probar la página mediante la adición de una película pero dejando fuera uno o varios de los campos. Al hacerlo, verá el siguiente error mostrar:

![Agregar página de la película que muestra los mensajes de error de validación](entering-data/_static/image5.png)

## <a name="styling-the-validation-error-messages"></a>Aplicar un estilo a los mensajes de Error de validación

Puede ver que hay mensajes de error, pero no destaquen muy bien. No hay una manera sencilla de estilo aunque los mensajes de error.

Para definir el estilo de los mensajes de error individuales que se muestran por `Html.ValidationMessage`, cree una clase de estilo CSS denominada `field-validation-error`. Para definir el aspecto para el resumen de validación, cree una clase de estilo CSS denominada `validation-summary-errors`.

Para ver cómo funciona esta técnica, agregue un `<style>` elemento dentro de la `<head>` sección de la página. A continuación, definir las clases de estilo denominadas `field-validation-error` y `validation-summary-errors` que contienen las siguientes reglas:

[!code-cshtml[Main](entering-data/samples/sample13.cshtml?highlight=4-17)]

Normalmente puede probablemente colocar la información de estilo en otro *.css* archivo, pero para simplificar el trabajo se puede colocar en la página por ahora. (Más adelante en este tutorial conjunto, se moverá las reglas de CSS para otro *.css* archivo.)

Si se produce un error de validación, el `Html.ValidationMessage` método representa un `<span>` elemento que incluye `class="field-validation-error"`. Al agregar una definición de estilo para esa clase, puede configurar el aspecto que tiene el mensaje. Si hay errores, el `ValidationSummary` método igualmente dinámicamente representa el atributo `class="validation-summary-errors"`.

Vuelva a ejecutar la página y omitir deliberadamente un par de los campos. Los errores son ahora más evidentes. (De hecho, se usa en exceso, pero que es solo mostrar lo que puede hacer).

![Agregar página que muestra los errores de validación que se ha aplicado el estilo de la película](entering-data/_static/image6.png)

## <a name="adding-a-link-to-the-movies-page"></a>Si agrega un vínculo a la página de películas

Es un paso final para que sea conveniente llegar a la *AddMovie* página de la lista original de la película.

Abra la *películas* página de nuevo. Después del cierre `</div>` etiqueta que sigue a la `WebGrid` auxiliar, agregue el siguiente marcado:

[!code-cshtml[Main](entering-data/samples/sample14.cshtml)]

Como hemos visto antes, ASP.NET interpreta la `~` operador como la raíz del sitio Web. No tiene que usar el `~` operador; podría utilizar el marcado `<a href="./AddMovie">Add a movie</a>` o alguna otra forma de definir la ruta de acceso que entienda de HTML. Pero la `~` operador es un buen enfoque general al crear vínculos a páginas de Razor, ya que hace que el sitio sea más flexible, si mueve la página actual en una subcarpeta, el vínculo se dirigirán a la *AddMovie* página. (Recuerde que el `~` operador solo funciona *.cshtml* páginas. ASP.NET lo entiende, pero no es HTML estándar).

Cuando haya terminado, ejecute el *películas* página. Será similar a esta página:

![Página de películas con un vínculo a la página 'Agregar películas'](entering-data/_static/image7.png)

Haga clic en el **agregar una película** vínculo para asegurarse de que va a la *AddMovie* página.

## <a name="coming-up-next"></a>Próxima

En el siguiente tutorial, aprenderá cómo permitir a los usuarios editar los datos que ya está en la base de datos.

## <a name="complete-listing-for-addmovie-page"></a>Muestra la lista completa de página AddMovie

[!code-cshtml[Main](entering-data/samples/sample15.cshtml)]

## <a name="additional-resources"></a>Recursos adicionales

- [Introducción a la programación Web de ASP.NET mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Instrucción SQL INSERT INTO](http://www.w3schools.com/sql/sql_insert.asp) en el sitio W3Schools
- [Validar la entrada del usuario en ASP.NET Web Pages sitios](https://go.microsoft.com/fwlink/?LinkId=253002). Para obtener más información sobre cómo trabajar con el `Validation` auxiliar.

> [!div class="step-by-step"]
> [Anterior](form-basics.md)
> [Siguiente](updating-data.md)
