---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
title: Examinar los métodos de edición y vista de edición | Microsoft Docs
author: Rick-Anderson
description: 'Nota: Una versión actualizada de este tutorial está disponible aquí que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro y mucho más fácil de seguir y demostraciones...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 41eb99ca-e88f-4720-ae6d-49a958da8116
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: ea8156fc2ea334be37e7c3f02ac15d168cfc6843
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838267"
---
<a name="examining-the-edit-methods-and-edit-view"></a>Examinar los métodos de edición y vista de edición
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Hay disponible una versión actualizada de este tutorial [aquí](../../getting-started/introduction/getting-started.md) que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro y mucho más fácil de seguir y se muestran las características más.


En esta sección, podrá examinar los métodos de acción generado y vistas para el controlador de película. A continuación, agregará una página de búsqueda personalizada.

Ejecute la aplicación y vaya a la `Movies` controlador anexando */Movies* a la dirección URL en la barra de direcciones del explorador. Mantenga el puntero del mouse sobre un **editar** vínculo para ver la dirección URL que está vinculado.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

El **editar** vínculo generado por el `Html.ActionLink` método en el *Views\Movies\Index.cshtml* vista:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

El `Html` objeto es una aplicación auxiliar que se expone mediante una propiedad en el [System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) clase base. El `ActionLink` método de la aplicación auxiliar facilita la tarea generar dinámicamente hipervínculos HTML que se vinculan a los métodos de acción en los controladores. El primer argumento para el `ActionLink` método es para representar el texto del vínculo (por ejemplo, `<a>Edit Me</a>`). El segundo argumento es el nombre del método de acción para invocar. El argumento final es un [objeto anónimo](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) que genera los datos de ruta (en este caso, el identificador de 4).

El vínculo generado se muestra en la imagen anterior es `http://localhost:xxxxx/Movies/Edit/4`. La ruta predeterminada (establecida en *aplicación\_start\routeconfig*) toma el patrón de URL `{controller}/{action}/{id}`. Por lo tanto, se traduce ASP.NET `http://localhost:xxxxx/Movies/Edit/4` en una solicitud para el `Edit` método de acción de la `Movies` controlador con el parámetro `ID` igual a 4. Examine el código siguiente desde el *aplicación\_start\routeconfig* archivo.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs)]

También puede pasar parámetros de método de acción con una cadena de consulta. Por ejemplo, la dirección URL `http://localhost:xxxxx/Movies/Edit?ID=4` también pasa el parámetro `ID` de 4 a la `Edit` método de acción de la `Movies` controlador.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Abra el `Movies` controlador. Los dos `Edit` a continuación se muestran los métodos de acción.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cs)]

Observe que el segundo método de acción `Edit` va precedido del atributo `HttpPost`. Este atributo especifica esa sobrecarga de la `Edit` se puede invocar el método solo para las solicitudes POST. Puede aplicar el `HttpGet` editar de atributo al primer método, pero que no es necesario porque es el valor predeterminado. (Nos referiremos a métodos de acción que se asignan implícitamente el `HttpGet` atributo como `HttpGet` métodos.)

El `HttpGet` `Edit` método toma el parámetro de Id. de película, busca la película con Entity Framework `Find` método y devuelve la película seleccionada a la vista de edición. Especifica el parámetro ID un [valor predeterminado](https://msdn.microsoft.com/library/dd264739.aspx) de cero si el `Edit` se llama al método sin parámetros. Si no se encuentra una película, [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) se devuelve. Cuando el sistema de scaffolding creó la vista de edición, examinó la clase `Movie` y creó código para representar los elementos `<label>` y `<input>` para cada propiedad de la clase. El ejemplo siguiente muestra la vista de edición que se ha generado:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cshtml)]

Observe cómo la plantilla de vista tiene un `@model MvcMovie.Models.Movie` instrucción en la parte superior del archivo: Especifica que la vista espera que el modelo para la plantilla de vista del tipo `Movie`.

El código con scaffolding usa varios *métodos auxiliares* para simplificar el marcado HTML. El [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) auxiliar muestra el nombre del campo (&quot;título&quot;, &quot;ReleaseDate&quot;, &quot;género&quot;, o &quot;precio &quot;). El [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) aplicación auxiliar representa un elemento HTML `<input>` elemento. El [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) auxiliar muestra los mensajes de validación asociados a esa propiedad.

Ejecute la aplicación y vaya a la */Movies* dirección URL. Haga clic en un vínculo **Edit** (Editar). En el explorador, vea el código fuente de la página. A continuación se muestra el código HTML para el elemento de formulario.

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample5.html?highlight=7,10-11)]

El `<input>` elementos se encuentran en un elemento HTML `<form>` elemento cuyo `action` atributo se establece para publicar en el */Movies/Edit* dirección URL. Los datos del formulario se publicarán en el servidor cuando el **editar** se hace clic en el botón.

## <a name="processing-the-post-request"></a>Procesamiento de la solicitud POST

En la siguiente lista se muestra la versión `HttpPost` del método de acción `Edit`.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cs)]

El [enlazador de modelos ASP.NET MVC](https://msdn.microsoft.com/magazine/hh781022.aspx) toma los valores de formulario publicados y crea un `Movie` objeto que se pasa como el `movie` parámetro. El método `ModelState.IsValid` comprueba que los datos presentados en el formulario pueden usarse para modificar (editar o actualizar) un objeto `Movie`. Si los datos son válidos, los datos de la película se guardan en el `Movies` colección de los `db(MovieDBContext` instancia). Los nuevos datos de la película se guardan en la base de datos mediante una llamada a la `SaveChanges` método `MovieDBContext`. Después de guardar los datos, el código redirige al usuario a la `Index` método de acción de la `MoviesController` clase, que muestra el de la colección de películas, incluidos los cambios que acaba de crear.

Si los valores expuestos no son válidos, se vuelven a mostrar en el formulario. El `Html.ValidationMessageFor` aplicaciones auxiliares en el *Edit.cshtml* vista plantilla tenga cuidado de mostrar mensajes de error adecuado.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

> [!NOTE]
> para admitir la validación de jQuery para configuraciones regionales distintas del inglés que usan una coma (&quot;,&quot;) para un punto decimal, se debe incluir *globalize.js* y específica de su *cultures/globalize.cultures.js* archivo (desde [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) y JavaScript para usar `Globalize.parseFloat`. El código siguiente muestra las modificaciones en el archivo Views\Movies\Edit.cshtml para trabajar con el &quot;fr-FR&quot; referencia cultural:


[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

El campo decimal puede requerir una coma, no es un separador decimal. Como solución temporal, puede agregar el elemento de globalización en el archivo web.config de raíz de los proyectos. El código siguiente muestra el elemento de globalización con la referencia cultural establecida en inglés de Estados Unidos.

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.xml)]

Todas las `HttpGet` métodos siguen un patrón similar. Obtienen un objeto de película (o una lista de objetos, en el caso de `Index`) y pasar el modelo a la vista. El `Create` método pasa un objeto de película vacío a la vista de creación. Todos los métodos que crean, editan, eliminan o modifican los datos lo hacen en la sobrecarga `HttpPost` del método. Modificación de datos en un método HTTP GET es un riesgo de seguridad, como se describe en la entrada de blog post [ASP.NET MVC sugerencia #46: no use eliminar vínculos porque crean vulnerabilidades de seguridad](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Modificación de datos en un método GET también infringe procedimientos recomendados de HTTP y la arquitectura [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) de patrón, que especifica que las solicitudes GET no deben cambiar el estado de la aplicación. En otras palabras, realizar una operación GET debería ser una operación segura sin efectos secundarios, que no modifica los datos persistentes.

## <a name="adding-a-search-method-and-search-view"></a>Agregar un método de búsqueda y la vista de búsqueda

En esta sección agregará un `SearchIndex` método de acción que le permite buscar películas por género o un nombre. Esto estará disponible mediante la */Movies/SearchIndex* dirección URL. La solicitud mostrará un formulario HTML que contiene los elementos de entrada que un usuario puede escribir para buscar una película. Cuando un usuario envía el formulario, el método de acción obtendrá los valores de búsqueda enviados por el usuario y usar los valores para buscar la base de datos.

## <a name="displaying-the-searchindex-form"></a>Mostrando el formulario SearchIndex

Empiece agregando un `SearchIndex` método de acción existente `MoviesController` clase. El método devolverá una vista que contiene un formulario HTML. Este es el código:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

La primera línea de la `SearchIndex` método crea los siguientes [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) consulta para seleccionar las películas:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cs)]

La consulta se define en este momento, pero aún no se ha ejecutado en el almacén de datos.

Si el `searchString` parámetro contiene una cadena, la consulta de películas se modifica para filtrar según el valor de la cadena de búsqueda, utilizando el código siguiente:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample11.cs)]

El código `s => s.Title` anterior es una [expresión Lambda](https://msdn.microsoft.com/library/bb397687.aspx). Las lambdas se usan en basadas en métodos [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) consultas como argumentos a los métodos de operador de consulta estándar como el [donde](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) método utilizado en el código anterior. Las consultas LINQ no se ejecutan cuando se definen o cuando se modifican mediante una llamada a un método como `Where` o `OrderBy`. En su lugar, se aplaza la ejecución de la consulta, lo que significa que la evaluación de una expresión se retrasa hasta que su valor realizado se repita realmente o [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) se llama al método. En el `SearchIndex` ejemplo, la consulta se ejecuta en la vista SearchIndex. Para más información sobre la ejecución de consultas en diferido, vea [Ejecución de la consulta](https://msdn.microsoft.com/library/bb738633.aspx).

Ahora puede implementar la `SearchIndex` vista que se mostrará el formulario al usuario. Haga clic en el `SearchIndex` método y, a continuación, haga clic en **agregar vista**. En el **agregar vista** diálogo cuadro, especifique que va a pasar un `Movie` objeto a la plantilla de vista como su clase de modelo. En el **plantilla Scaffold** elija **lista**, a continuación, haga clic en **agregar**.

![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image5.png)

Al hacer clic en el **agregar** botón, el *Views\Movies\SearchIndex.cshtml* se crea la plantilla de vista. Dado que seleccionó **lista** en el **plantilla Scaffold** enumerar, Visual Studio genera automáticamente (scaffolding) algún marcado de forma predeterminada en la vista. La técnica de scaffolding crea un formulario HTML. También se ha examinado el `Movie` clase y código creado para representar `<label>` elementos para cada propiedad de la clase. La siguiente lista muestra la vista de creación que se ha generado:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample12.cshtml)]

Ejecute la aplicación y vaya a */Movies/SearchIndex*. Anexe una cadena de consulta como `?searchString=ghost` a la dirección URL. Se muestran las películas filtradas.

![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image6.png)

Si cambia la firma de la `SearchIndex` método tenga un parámetro denominado `id`, el `id` parámetro coincidirá la `{id}` enruta el marcador de posición para el valor predeterminado establecido en el *Global.asax* archivo.

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample13.json)]

La versión original `SearchIndex` método aspecto::

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample14.cs)]

Modificado `SearchIndex` método tendría el aspecto siguiente:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample15.cs?highlight=1,3)]

Ahora puede pasar el título de la búsqueda como datos de ruta (un segmento de dirección URL) en lugar de como un valor de cadena de consulta.

![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image7.png)

Sin embargo, no se puede esperar que los usuarios modifiquen la dirección URL cada vez que quieran buscar una película. Ahora va a agregar la interfaz de usuario para ayudar a ellos filtrar las películas. Si ha cambiado la firma de la `SearchIndex` método para probar cómo pasar el parámetro de identificador enlazado a una ruta, cámbielo para que su `SearchIndex` método toma un parámetro de cadena denominado `searchString`:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample16.cs)]

Abra el *Views\Movies\SearchIndex.cshtml* archivo y, justo después `@Html.ActionLink("Create New", "Create")`, agregue lo siguiente:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample17.cshtml?highlight=2)]

El ejemplo siguiente muestra una parte de la *Views\Movies\SearchIndex.cshtml* archivo con el marcado de filtrado se ha agregado.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample18.cshtml?highlight=12-15)]

El `Html.BeginForm` auxiliar crea una apertura `<form>` etiqueta. El `Html.BeginForm` auxiliar hace que el formulario registrar a sí mismo cuando el usuario envía el formulario, haga clic en el **filtro** botón.

Ejecute la aplicación y pruebe a buscar una película.

![](examining-the-edit-methods-and-edit-view/_static/image8.png)

No hay ningún `HttpPost` sobrecarga de la `SearchIndex` método. No es necesario, porque el método no cambia el estado de la aplicación, simplemente filtra los datos.

Después, puede agregar el método `HttpPost SearchIndex` siguiente. En ese caso, coincidiría con el invocador de acción el `HttpPost SearchIndex` método y el `HttpPost SearchIndex` método ejecutaría tal como se muestra en la imagen siguiente.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample19.cs)]

![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image9.png)

Sin embargo, aunque agregue esta versión de `HttpPost` al método `SearchIndex`, hay una limitación en cómo se ha implementado todo esto. Supongamos que quiere marcar una búsqueda en particular o que quiere enviar un vínculo a sus amigos donde puedan hacer clic para ver la misma lista filtrada de películas. Tenga en cuenta que la dirección URL de la solicitud HTTP POST es el mismo que la dirección URL de la solicitud GET (localhost: xxxxx/Movies/SearchIndex), no hay ninguna información de búsqueda en la propia dirección URL. Derecha ahora, la información de la cadena de búsqueda se envía al servidor como un valor de campo de formulario. Esto significa que no se puede capturar dicha información para marcar o enviar a amigos en una dirección URL.

La solución consiste en utilizar una sobrecarga de `BeginForm` que especifica que la solicitud POST debe agregar la información de búsqueda a la dirección URL y que deben enrutarse a la versión de HttpGet del `SearchIndex` método. Reemplazar el existente sin parámetros `BeginForm` método con lo siguiente:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample20.cshtml)]

![BeginFormPost_SM](examining-the-edit-methods-and-edit-view/_static/image10.png)

Ahora cuando se envía una búsqueda, la dirección URL contiene una cadena de consulta de búsqueda. La búsqueda también será dirigida al método de acción `HttpGet SearchIndex`, aunque tenga un método `HttpPost SearchIndex`.

![SearchIndexWithGetURL](examining-the-edit-methods-and-edit-view/_static/image11.png)

## <a name="adding-search-by-genre"></a>Adición de búsqueda por género

Si ha agregado el `HttpPost` versión de la `SearchIndex` método, elimínelo.

A continuación, agregará una característica para permitir que los usuarios buscar películas por género. Reemplace el método `SearchIndex` con el código siguiente:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample21.cs)]

Esta versión de la `SearchIndex` método toma un parámetro adicional, es decir, `movieGenre`. Las primeras líneas de código crean un `List` objeto para contener los géneros de película de la base de datos.

El código siguiente es una consulta LINQ que recupera todos los géneros de la base de datos.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample22.cs)]

El código usa el `AddRange` método de la clase genérica `List` colección para agregar los distintos géneros a la lista. (Sin el `Distinct` modificador, se agregaría géneros duplicados, por ejemplo, Comedia se agregaría dos veces en nuestro ejemplo). El código, a continuación, almacena la lista de géneros de la `ViewBag` objeto.

El código siguiente muestra cómo comprobar el `movieGenre` parámetro. Si no está vacía, el código más restringe la consulta de películas para limitar las películas seleccionadas para el género especificado.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample23.cs)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>Agregar marcas a la vista de SearchIndex para admitir la búsqueda por género

Agregar un `Html.DropDownList` auxiliar para el *Views\Movies\SearchIndex.cshtml* archivo, justo antes del `TextBox` auxiliar. A continuación se muestra el marcado completado:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample24.cshtml?highlight=4)]

Ejecute la aplicación y vaya a */Movies/SearchIndex*. Pruebe una búsqueda por género, nombre de la película y ambos criterios.

![](examining-the-edit-methods-and-edit-view/_static/image12.png)

En esta sección se examinan los métodos de acción de CRUD y las vistas generadas por el marco de trabajo. Crea un método de acción de búsqueda y la vista que permite a los usuarios buscar por título de la película y género. En la siguiente sección, echaremos un vistazo a cómo agregar una propiedad a la `Movie` modelo y cómo agregar un inicializador que creará automáticamente una base de datos de prueba.

> [!div class="step-by-step"]
> [Anterior](accessing-your-models-data-from-a-controller.md)
> [Siguiente](adding-a-new-field-to-the-movie-model-and-table.md)
