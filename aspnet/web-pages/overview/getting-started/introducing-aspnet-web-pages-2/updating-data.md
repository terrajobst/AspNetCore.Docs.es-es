---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
title: 'Introducción a ASP.NET Web Pages: actualización de la base de datos | Microsoft Docs'
author: Rick-Anderson
description: Este tutorial muestra cómo actualizar la entrada de una base de datos existente (cambiar) al usar ASP.NET Web Pages (Razor). Supone que ha completado la serie th...
ms.author: riande
ms.date: 01/02/2018
ms.assetid: ac86ec9c-6b69-485b-b9e0-8b9127b13e6b
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
msc.type: authoredcontent
ms.openlocfilehash: 206d7e209857aceb3eb92c2405bb73f7ff7dbaeb
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021435"
---
<a name="introducing-aspnet-web-pages---updating-database-data"></a>Introducción a ASP.NET Web Pages: actualización de la base de datos
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial muestra cómo actualizar la entrada de una base de datos existente (cambiar) al usar ASP.NET Web Pages (Razor). Supone que ha completado la serie a través de [introducción de datos por uso de formularios utilizando ASP.NET Web Pages](entering-data.md).
> 
> Lo que aprenderá:
> 
> - Cómo seleccionar un registro individual en el `WebGrid` auxiliar.
> - Cómo leer un único registro de una base de datos.
> - Cómo cargar previamente un formulario con los valores del registro de base de datos.
> - Cómo actualizar un registro existente en una base de datos.
> - Cómo almacenar información en la página sin mostrarlo.
> - Cómo usar un campo oculto para almacenar información.
>   
> 
> Características y tecnologías tratadas:
> 
> - El `WebGrid` auxiliar.
> - El código SQL `Update` comando.
> - El método `Database.Execute`.
> - Campos ocultos (`<input type="hidden">`).


## <a name="what-youll-build"></a>¿Qué va a crear

En el tutorial anterior, ha aprendido a agregar un registro a una base de datos. En este caso, obtendrá información sobre cómo mostrar un registro para editarlo. En el *películas* página, actualizará la `WebGrid` auxiliar para que muestre que TI un **editar** vínculo situado junto a cada película:

![WebGrid mostrar como un vínculo "Editar" para cada película](updating-data/_static/image1.png)

Al hacer clic en el **editar** vínculo le lleva a una página distinta, donde la información de películas ya está en un formulario:

![Editar página de la película que muestra la película se puedan editar](updating-data/_static/image2.png)

Puede cambiar cualquiera de los valores. Al enviar los cambios, el código de la página actualiza la base de datos y lo devuelve a la lista de películas.

Esta parte del proceso funciona casi exactamente igual que el *AddMovie.cshtml* página creada en el tutorial anterior, por lo que gran parte de este tutorial le resultarán familiar.

Hay varias maneras podría implementar una forma de editar una película individual. Se ha elegido el enfoque mostrado porque es fácil de implementar y fácil de entender.

## <a name="adding-an-edit-link-to-the-movie-listing"></a>Agregar un vínculo de edición a la lista de películas

Para empezar, actualizará la *películas* página para que cada película en la lista también contiene un **editar** vínculo.

Abra el *Movies.cshtml* archivo.

En el cuerpo de la página, cambie la `WebGrid` marcado mediante la adición de una columna. Aquí está el marcado modificado:

[!code-html[Main](updating-data/samples/sample1.html?highlight=6)]

Esta es la nueva columna:

[!code-html[Main](updating-data/samples/sample2.html)]

El objetivo de esta columna es mostrar un vínculo (`<a>` elemento) cuyo texto dice "Editar". Lo que tenemos es crear un vínculo con el siguiente aspecto cuando se ejecuta la página, con el `id` valor diferente para cada película:

[!code-css[Main](updating-data/samples/sample3.css)]

Este vínculo invocará una página denominada *EditMovie*, y pasará la cadena de consulta `?id=7` a esa página.

La sintaxis para la nueva columna puede parecer un poco compleja, pero es solo que reúne varios elementos. Cada elemento individual es sencillo. Si concentrarse en la que solo el `<a>` elemento, vea este marcado:

[!code-html[Main](updating-data/samples/sample4.html)]

Conocimientos sobre el funcionamiento de la cuadrícula: la cuadrícula muestra las filas, uno para cada registro de base de datos, y muestra las columnas para cada campo en el registro de base de datos. Mientras se está construyendo cada fila de cuadrícula, la `item` objeto contiene el registro de base de datos (elemento) para esa fila. Esta disposición proporciona una forma en el código para obtener los datos para esa fila. Eso es lo que ve aquí: la expresión `item.ID` es obtener el valor de Id. del elemento de base de datos actual. Se podría obtener cualquiera de los valores de la base de datos (título, género o año) la misma manera mediante `item.Title`, `item.Genre`, o `item.Year`.

La expresión `"~/EditMovie?id=@item.ID` combina la parte modificable de la dirección URL de destino (`~/EditMovie?id=`) con este identificador dinámicamente derivada. (Se vio la `~` operador en el tutorial anterior; es un operador de ASP.NET que representa la raíz del sitio Web actual.)

El resultado es que esta parte del marcado en la columna simplemente genera algo parecido al siguiente marcado en tiempo de ejecución:

[!code-xml[Main](updating-data/samples/sample5.xml)]

Naturalmente, el valor real de `id` será diferente para cada fila.

## <a name="creating-a-custom-display-for-a-grid-column"></a>Crear una visualización personalizada para una columna de cuadrícula

Ahora volvamos a la columna de cuadrícula. Las tres columnas originalmente había en la cuadrícula muestra solo valores de datos (título, género y año). Especificar esta pantalla, pasando el nombre de la columna de base de datos &mdash; por ejemplo, `grid.Column("Title")`.

Esta nueva **editar** columna vínculo es diferente. En lugar de especificar un nombre de columna, que está pasando un `format` parámetro. Este parámetro le permite definir el marcado que el `WebGrid` auxiliar se representará junto con el `item` valor para mostrar los datos de columna en negrita o verde o en el formato que desee. Por ejemplo, si deseara que el título aparece en negrita, podría crear una columna como en este ejemplo:

[!code-html[Main](updating-data/samples/sample6.html)]

(Los diversos `@` caracteres que veas en el `format` propiedad marcar la transición entre el marcado y un valor de código.)

Una vez que sepa sobre el `format` propiedad, resulta más fácil de entender cómo la nueva **editar** elaborado columna vínculo:

[!code-html[Main](updating-data/samples/sample7.html)]

Consta de la columna *sólo* del marcado que representa el vínculo, además de cierta información (identificador) que se extrae de la base de datos registro para la fila.

> [!TIP]
> 
> **Parámetros con nombre y parámetros posicionales para un método**
> 
> Muchas veces cuando se ha llamado a un método y parámetros que se pasan a él, simplemente ha enumerado los valores de parámetros separados por comas. A continuación se muestran un par de ejemplos:
> 
> `db.Execute(insertCommand, title, genre, year)`
> 
> `Validation.RequireField("title", "You must enter a title")`
> 
> No lo hemos mencionado el problema cuando vio primero este código, pero en cada caso, está pasando los parámetros a los métodos en un orden específico &mdash; es decir, el orden en que los parámetros se definen en ese método. Para `db.Execute` y `Validation.RequireFields`, si se combina el orden de los valores pasados, obtendría un mensaje de error cuando se ejecuta la página, o al menos algunos resultados extraños. Claramente, tendrá que saber el orden para pasar los parámetros. (En WebMatrix, IntelliSense puede ayudarle a aprender a averiguar el nombre, tipo y orden de los parámetros.)
> 
> Como alternativa para pasar valores en orden, puede usar *parámetros con nombre*. (Pasar parámetros en orden se conoce como utilización *parámetros posicionales*.) Para los parámetros con nombre, se incluye explícitamente el nombre del parámetro al pasar su valor. Se ha usado los parámetros con nombre ya un número de veces en estos tutoriales. Por ejemplo:
> 
> [!code-csharp[Main](updating-data/samples/sample8.cs)]
> 
> y
> 
> [!code-css[Main](updating-data/samples/sample9.css)]
> 
> Parámetros con nombre son útiles para un par de situaciones, especialmente cuando un método toma el número de parámetros. Uno es cuando desea pasar solo uno o dos parámetros, pero no son los valores que van a pasar entre las posiciones de primera en la lista de parámetros. Otra situación es cuando desea que el código sea más legible, pasando los parámetros en el orden en que más sentido para usted.
> 
> Obviamente, para usar parámetros con nombre, tendrá que conocer los nombres de los parámetros. WebMatrix IntelliSense puede *mostrar* los nombres, pero no puede actualmente rellenarlo para usted.


## <a name="creating-the-edit-page"></a>Creación de la página de edición

Ahora puede crear el *EditMovie* página. Cuando los usuarios hacen clic los **editar** vínculo, terminará en esta página.

Cree una página denominada *EditMovie.cshtml* y sustituir lo que aparece en el archivo con el siguiente marcado:

[!code-cshtml[Main](updating-data/samples/sample10.cshtml)]

Este marcado y el código es similar a lo que tiene en el *AddMovie* página. Hay una pequeña diferencia en el texto para el botón Enviar. Igual que con el *AddMovie* página, hay un `Html.ValidationSummary` llamada que se mostrará los errores de validación si existe alguna. Esta vez nos estamos omitiendo las llamadas a `Validation.Message`, ya que los errores se mostrarán en el resumen de validación. Como se indicó en el tutorial anterior, puede usar el resumen de validación y los mensajes de error individuales en diversas combinaciones.

Observe nuevamente que la `method` atributo de la `<form>` elemento está establecido en `post`. Igual que con el *AddMovie.cshtml* página, esta página realiza cambios en la base de datos. Por lo tanto, debe realizar este formulario un `POST` operación. (Para obtener más información sobre la diferencia entre `GET` y `POST` operaciones, vea el [GET, POST y HTTP verbo seguridad](form-basics.md#GET,_POST,_and_HTTP_Verb_Safety) sidebar en el tutorial en formularios HTML.)

Como se vio en un tutorial anterior, el `value` se establecen los atributos de los cuadros de texto con el código de Razor para cargarlas. Esta vez, sin embargo, usa variables como `title` y `genre` para esa tarea en lugar de `Request.Form["title"]`:

`<input type="text" name="title" value="@title" />`

Como antes, este marcado se cargar previamente los valores del cuadro de texto con los valores de la película. Verá en breve por qué es útil para usar variables en este momento en lugar de usar el `Request` objeto.

También hay un `<input type="hidden">` elemento en esta página. Este elemento almacena el identificador de película sin hacerlo visible en la página. El identificador se pasa inicialmente a la página mediante utilizando un valor de cadena de consulta (`?id=7` o similar en la dirección URL). Al colocar el valor de identificador en un campo oculto, puede asegurarse de que está disponible cuando se envía, incluso si ya no tiene acceso a la dirección URL original que se ha invocado la página con el formulario.

A diferencia de la *AddMovie* página, el código para el *EditMovie* página tiene dos funciones distintas. La primera función es que cuando se muestra la página por primera vez (y *sólo* , a continuación,), el código obtiene el identificador de película de la cadena de consulta. El código, a continuación, usa el identificador para leer la película fuera de la base de datos correspondiente y mostrar (cargar previamente), en los cuadros de texto.

La segunda función es que cuando el usuario hace clic en el **enviar cambios** botón, el código tiene que leer los valores de los cuadros de texto y validarlos. El código también tiene que actualizar el elemento de la base de datos con los nuevos valores. Esta técnica es similar a agregar un registro, como se vio en *AddMovie*.

## <a name="adding-code-to-read-a-single-movie"></a>Agregar código para leer una película única

Para llevar a cabo la primera función, agregue este código a la parte superior de la página:

[!code-cshtml[Main](updating-data/samples/sample11.cshtml)]

La mayor parte de este código es dentro de un bloque que se inicia `if(!IsPost)`. El `!` operador significa "no", por lo que significa que la expresión *si esta solicitud no es un envío de post*, que es una forma indirecta de decir *si esta solicitud es la primera vez que se ha ejecutado esta página*. Como se indicó anteriormente, este código debe ejecutarse *sólo* la primera vez que se ejecuta la página. Si no incluye el código en `if(!IsPost)`, que se ejecute cada vez que se invoca la página, si la primera vez o en respuesta a un botón, haga clic en.

Tenga en cuenta que el código incluye un `else` bloquear este momento. Como hemos dicho cuando introdujimos `if` bloques, a veces desea ejecutar código alternativo si la condición de prueba no es cierto. Ese es el caso aquí. Si se pasa la condición (es decir, si el identificador pasado a la página es correcto), lee una fila de la base de datos. Sin embargo, si no pasa la condición, el `else` bloque se ejecuta y el código establece un mensaje de error.

## <a name="validating-a-value-passed-to-the-page"></a>Validar un valor pasado a la página

El código usa `Request.QueryString["id"]` para obtener el identificador que se pasa a la página. El código se asegura de que realmente se pasó un valor para el identificador. Si se pasa ningún valor, el código establece un error de validación.

Este código muestra otra forma de validar la información. En el tutorial anterior, ha trabajado con la `Validation` auxiliar. Registra los campos para validar y ASP.NET hizo la validación automáticamente y muestra los errores mediante el uso de `Html.ValidationMessage` y `Html.ValidationSummary`. En este caso, sin embargo, realmente no está validando proporcionados por el usuario. En su lugar, que se está validando un valor que se pasó a la página desde cualquier parte. El `Validation` auxiliar no hará eso por usted.

Por lo tanto, compruebe el valor usted mismo, por probarlo con `if(!Request.QueryString["ID"].IsEmpty()`). Si hay un problema, puede mostrar el error mediante `Html.ValidationSummary`, tal y como hizo con la `Validation` auxiliar. Para ello, llame a `Validation.AddFormError` y pasarle un mensaje para mostrar. `Validation.AddFormError` es un método integrado que le permite definir mensajes personalizados que se enlazan con ya está familiarizado con el sistema de validación. (Más adelante en este tutorial hablaremos acerca de cómo realizar este proceso de validación de un poco más robusto.)

Después de asegurarse de que hay un identificador para la película, el código lee la base de datos, buscando sólo un elemento de la base de datos única. (Probablemente haya observado el patrón general para las operaciones de base de datos: abra la base de datos, defina una instrucción SQL y ejecute la instrucción.) Esta vez, el código SQL `Select` instrucción incluye `WHERE ID = @0`. Dado que el identificador es único, se puede devolver un único registro.

La consulta se realiza mediante `db.QuerySingle` (no `db.Query`, tal y como se ha usado para la lista de películas), y el código coloca el resultado en el `row` variable. El nombre `row` es arbitrario; puede designar las variables que desee. Las variables que se inicializan en la parte superior, a continuación, se rellenan con los detalles de la película para que estos valores se pueden mostrar en los cuadros de texto.

## <a name="testing-the-edit-page-so-far"></a>Probar la página de edición (hasta)

Si desea probar la página, ejecute el *películas* página ahora y haga clic en un **editar** vínculo situado junto a cualquier película. Verá el *EditMovie* rellena página con los detalles para la película seleccionada:

![Editar página de la película que muestra la película se puedan editar](updating-data/_static/image3.png)

Tenga en cuenta que la dirección URL de la página incluye algo parecido a `?id=10` (u otro número). Hasta ahora ha probado que **editar** vínculos en el *película* página de trabajo, que la página está leyendo el identificador de la cadena de consulta y que la base de datos de consulta para obtener un registro de película único funciona.

Puede cambiar la información de la película, pero no ocurre nada al hacer clic en **enviar cambios**.

## <a name="adding-code-to-update-the-movie-with-the-users-changes"></a>Agregar código para actualizar la película con los cambios del usuario

En el *EditMovie.cshtml* , para implementar la segunda función (Guardar cambios), agregue el código siguiente justo dentro de la llave de cierre de la `@` bloque. (Si no está seguro de exactamente dónde debe colocar el código, puede mirar la [el código completo de la página Editar película](#Complete_Page_Listing_for_EditMovie) que aparece al final de este tutorial.)

[!code-csharp[Main](updating-data/samples/sample12.cs)]

De nuevo, este marcado y el código es similar al código de *AddMovie*. El código está en un `if(IsPost)` bloquear, dado que este código se ejecuta solo cuando el usuario hace clic en el **enviar cambios** botón &mdash; es decir, cuando (y sólo cuando) se ha registrado el formulario. En este caso, no usa una prueba como `if(IsPost && Validation.IsValid())`, es decir, no es combinar ambas pruebas mediante el uso de and. En esta página, determina primero si hay un envío de formulario (`if(IsPost)`) y registrar solo los campos para la validación. A continuación, puede probar los resultados de validación (`if(Validation.IsValid()`). El flujo es ligeramente diferente a la de la *AddMovie.cshtml* página, pero el efecto es el mismo.

Obtener los valores de los cuadros de texto mediante `Request.Form["title"]` y un código similar para los demás `<input>` elementos. Tenga en cuenta que esta vez, el código obtiene el identificador de película fuera del campo oculto (`<input type="hidden">`). Cuando la página se ha ejecutado la primera vez, el código tiene el identificador de la cadena de consulta. Obtiene el valor del campo oculto para asegurarse de que está obteniendo el Id. de la película que aparecía originalmente, en caso de que la cadena de consulta se modifica de alguna manera desde entonces.

La diferencia entre lo realmente importante la *AddMovie* código y este código es que en este código usa la instrucción SQL `Update` instrucción en lugar de la `Insert Into` instrucción. El ejemplo siguiente muestra la sintaxis de SQL `Update` instrucción:

`UPDATE table SET col1="value", col2="value", col3="value" ... WHERE ID = value`

Puede especificar las columnas en cualquier orden, y no necesariamente deben actualizar todas las columnas durante un `Update` operación. (No se puede actualizar el Id. de sí mismo, porque en vigor que desea guardar el registro como un nuevo registro, y que no se permite para un `Update` operación.)

> [!NOTE] 
> 
> **Importante** el `Where` cláusula con el Id. es muy importante, ya que es la manera en que la base de datos sabe qué base de datos de registro que desea actualizar. Si lo dejó el `Where` cláusula, la base de datos actualizaría *cada* registros en la base de datos. En la mayoría de los casos, eso sería un desastre.


En el código, se pasan los valores para actualizar a la instrucción SQL mediante el uso de marcadores de posición. Repita lo que hemos dicho antes: por motivos de seguridad, *sólo* usar marcadores de posición para pasar valores a una instrucción SQL.

Después de que el código usa `db.Execute` para ejecutar el `Update` instrucción, redirige a la página de lista, donde puede ver los cambios.

> [!TIP] 
> 
> **Instrucciones de SQL diferente, distintos métodos**
> 
> Es posible que haya observado que utiliza métodos ligeramente diferentes para ejecutar instrucciones SQL diferentes. Para ejecutar un `Select` que potencialmente consulta devuelve varios registros, usar el `Query` método. Para ejecutar un `Select` consulta que sabe que devolverá sólo un elemento de la base de datos, utilice el `QuerySingle` método. Para ejecutar comandos que realizar cambios, pero que no devuelven los elementos de la base de datos, utilice el `Execute` método.
> 
> Tendrá que tienen métodos diferentes debido a que cada una de ellas devuelve resultados diferentes, como ya vimos en la diferencia entre `Query` y `QuerySingle`. (El `Execute` método realmente devuelve un valor también &mdash; es decir, el número de filas de la base de datos que se vieron afectados por el comando &mdash; pero se ha estado omitiendo que hasta ahora.)
> 
> Por supuesto, el `Query` podría devolver una única fila de la base de datos. Sin embargo, ASP.NET siempre trata los resultados de la `Query` método como una colección. Incluso si el método devuelve una sola fila, deberá extraer esa única fila de la colección. Por lo tanto, en situaciones donde se *saber* obtendrá una única fila, es un poco más conveniente usar `QuerySingle`.
> 
> Hay algunos otros métodos que realizan determinados tipos de operaciones de base de datos. Puede encontrar una lista de métodos de la base de datos en el [referencia rápida de ASP.NET Web Pages API](../../api-reference/asp-net-web-pages-api-reference.md#Data).


## <a name="making-validation-for-the-id-more-robust"></a>Realizar la validación de Id. de más sólida

La primera vez que se ejecuta la página, obtendrá el identificador de película de la cadena de consulta para que puedan ir obtener esa película de la base de datos. Se ha asegurado de que realmente se ha producido un valor para ir a buscar, que lo hizo utilizando este código:

[!code-csharp[Main](updating-data/samples/sample13.cs)]

Utilice este código para asegurarse de que si un usuario llega a la *EditMovies* página sin seleccionar primero una película en el *películas* página, la página mostrará un mensaje de error descriptivo. (En caso contrario, los usuarios vería un error que probablemente confundirían a ellos).

Sin embargo, esta validación no es muy eficaz. La página también se puede invocar con estos errores:

- El identificador no es un número. Por ejemplo, se pudo invocar la página con una dirección URL como `http://localhost:nnnnn/EditMovie?id=abc`.
- El identificador es un número, pero hace referencia a una película que no existe (por ejemplo, `http://localhost:nnnnn/EditMovie?id=100934`).

Si siente curiosidad por ver los errores resultantes de estas direcciones URL, ejecute el *películas* página. Seleccione una película para editar y, a continuación, cambie la dirección URL de la *EditMovie* página a una dirección URL que contiene un es un carácter alfabético, identificador o el identificador de una película inexistentes.

¿Qué debe hacer? La primera corrección es asegurarse de que no sólo es un identificador pasa a la página, pero que el identificador es un entero. Cambiar el código para el `!IsPost` prueba sea similar a este ejemplo:

[!code-csharp[Main](updating-data/samples/sample14.cs)]

Ha agregado una segunda condición para la `IsEmpty` prueba, vinculado con `&&` (AND lógico):

[!code-csharp[Main](updating-data/samples/sample15.cs)]

Es posible que recuerde de la [Introducción a la programación de ASP.NET Web Pages](../introducing-razor-syntax-c.md) tutorial que métodos como `AsBool` un `AsInt` convertir una cadena de caracteres en algún otro tipo de datos. El `IsInt` método (y otros, como `IsBool` y `IsDateTime`) son similares. Sin embargo, probar solo si se *puede* convertir la cadena, sin tener que realizar la conversión. Así que aquí esencialmente se está diciendo *si el valor de cadena de consulta se puede convertir en un entero de...* .

El posible problema está buscando una película que no existe. El código para obtener una película es similar a este código:

[!code-csharp[Main](updating-data/samples/sample16.cs)]

Si se pasa un `movieId` valor para el `QuerySingle` método que no se corresponde con una película real, se devuelve nada, y las instrucciones que siguen (por ejemplo, `title=row.Title`) producirán errores.

Nuevo hay una solución fácil. Si el `db.QuerySingle` método no devuelve ningún resultado, el `row` variable será null. Por lo que puede comprobar si el `row` variable es null antes de intentar obtener valores a partir de él. El código siguiente agrega un `if` bloque alrededor de las instrucciones que obtienen los valores de la `row` objeto:

[!code-csharp[Main](updating-data/samples/sample17.cs)]

Con estas dos pruebas de validación adicional, la página se vuelve más infalible. El código completo para el `!IsPost` rama ahora es similar a este ejemplo:

[!code-csharp[Main](updating-data/samples/sample18.cs)]

Se le tenga en cuenta una vez más que esta tarea es un buen uso de un `else` bloque. Si no pasan las pruebas, el `else` conjunto de bloques de mensajes de error.

## <a name="adding-a-link-to-return-to-the-movies-page"></a>Agregar un vínculo para volver a la página de películas

Es un detalle final y útil agregar un vínculo a la *películas* página. En el flujo normal de eventos, los usuarios se iniciarán en el *películas* página y haga clic en un **editar** vínculo. Esto les lleva a la *EditMovie* página, donde puede editar la película y haga clic en el botón. Una vez que el código procesa el cambio, redirige a la *películas* página.

Pero:

- El usuario puede decidir no cambia nada.
- El usuario puede aparecer en esta página sin hacer clic en un **editar** vincular en el *películas* página.

En cualquier caso, desea que sea fácil para que puedan volver a la lista principal. Es una solución fácil &mdash; agregue el marcado siguiente justo después del cierre `</form>` etiqueta en el marcado:

[!code-html[Main](updating-data/samples/sample19.html)]

Este marcado utiliza la misma sintaxis para un `<a>` elemento que ha visto en otro lugar. La dirección URL incluye `~` significa "" raíz"del sitio Web.

## <a name="testing-the-movie-update-process"></a>Probar el proceso de actualización de la película

Ahora puede probar. Ejecute el *películas* página y haga clic en **editar** junto a una película. Cuando el *EditMovie* aparece en la página, realizar cambios en la película y haga clic en **enviar cambios**. Cuando aparezca la lista de películas, asegúrese de que se muestran los cambios.

Para asegurarse de que la validación funciona, haga clic en **editar** otra película. Cuando llegue a la *EditMovie* página, desactive la **género** campo (o **año** campo, o ambos) y vuelva a enviar los cambios. Verá un error, como cabría esperar:

![Editar página de la película que muestra los errores de validación](updating-data/_static/image4.png)

Haga clic en el **volver a la lista de películas** vínculo para abandonar los cambios y volver a la *películas* página.

## <a name="coming-up-next"></a>Próxima

En el siguiente tutorial, verá cómo eliminar un registro de la película.

## <a name="complete-listing-for-movie-page-updated-with-edit-links"></a>Lista completa de la página de película (actualizada con vínculos de edición)

[!code-cshtml[Main](updating-data/samples/sample20.cshtml)]

<a id="Complete_Page_Listing_for_EditMovie"></a>
## <a name="complete-page-listing-for-edit-movie-page"></a>Lista de página para editar página de la película completa

[!code-cshtml[Main](updating-data/samples/sample21.cshtml)]

## <a name="additional-resources"></a>Recursos adicionales

- [Introducción a la programación Web de ASP.NET mediante la sintaxis de Razor](../../getting-started/introducing-razor-syntax-c.md)
- [Instrucción UPDATE de SQL](http://www.w3schools.com/sql/sql_update.asp) en el sitio W3Schools

> [!div class="step-by-step"]
> [Anterior](entering-data.md)
> [Siguiente](deleting-data.md)
