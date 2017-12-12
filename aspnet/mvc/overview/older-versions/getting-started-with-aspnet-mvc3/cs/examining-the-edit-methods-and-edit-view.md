---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/examining-the-edit-methods-and-edit-view
title: "Examen de los métodos de edición y la vista de edición (C#) | Documentos de Microsoft"
author: Rick-Anderson
description: "Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 1d266bf0-a61e-423b-a3d2-13773d7dafe2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: b80332487e52930f3a75973f714d2532068ed012
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="examining-the-edit-methods-and-edit-view-c"></a>Examen de los métodos de edición y la vista de edición (C#)
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Hay disponible una versión actualizada de este tutorial [aquí](../../../getting-started/introduction/getting-started.md) que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro y mucho más fácil de seguir y se muestra más características.
> 
> 
> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio. Antes de empezar, asegúrese de que ha instalado los requisitos previos descritos a continuación. Puede instalar todas ellas haciendo clic en el siguiente vínculo: [instalador de plataforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar por separado los requisitos previos mediante los siguientes vínculos:
> 
> - [Requisitos previos visuales Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(se admiten en tiempo de ejecución + herramientas)
> 
> Si usa Visual Studio 2010 en lugar de Visual Web Developer 2010, instale los requisitos previos, haga clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un proyecto de Visual Web Developer con el código fuente de C# está disponible como acompañamiento de este tema. [Descargue la versión de C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si prefiere Visual Basic, cambie a la [versión de Visual Basic](../vb/intro-to-aspnet-mvc-3.md) de este tutorial.


En esta sección, podrá examinar los métodos de acción generado y las vistas para el controlador de la película. A continuación, agregará una página de búsqueda personalizada.

Ejecute la aplicación y busque el `Movies` controlador anexando */Movies* a la dirección URL en la barra de direcciones del explorador. Mantenga el puntero del mouse sobre un **editar** vínculo para ver la dirección URL que está vinculado.

[![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image2.png)](examining-the-edit-methods-and-edit-view/_static/image1.png)

El **editar** vínculo generado por la `Html.ActionLink` método en el *Views\Movies\Index.cshtml* vista:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

[![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image4.png)](examining-the-edit-methods-and-edit-view/_static/image3.png)

El `Html` objeto es una aplicación auxiliar que se expone mediante una propiedad en la `WebViewPage` clase base. El `ActionLink` método de aplicación auxiliar resulta muy sencillo generar dinámicamente hipervínculos HTML que se vinculan a los métodos de acción en los controladores. El primer argumento para el `ActionLink` método es el texto del vínculo para representar (por ejemplo, `<a>Edit Me</a>`). El segundo argumento es el nombre del método de acción para invocar. El argumento final es un [objeto anónimo](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) que genera los datos de ruta (en este caso, el identificador de 4).

El vínculo generado se muestra en la imagen anterior es `http://localhost:xxxxx/Movies/Edit/4`. La ruta predeterminada tiene el modelo de dirección URL `{controller}/{action}/{id}`. Por lo tanto, se traduce ASP.NET `http://localhost:xxxxx/Movies/Edit/4` en una solicitud para la `Edit` método de acción de la `Movies` controlador con el parámetro `ID` igual a 4.

También puede pasar parámetros de método de acción mediante una cadena de consulta. Por ejemplo, la dirección URL `http://localhost:xxxxx/Movies/Edit?ID=4` también pasa el parámetro `ID` 4 y la `Edit` método de acción de la `Movies` controlador.

[![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image6.png)](examining-the-edit-methods-and-edit-view/_static/image5.png)

Abra el `Movies` controlador. Los dos `Edit` a continuación se muestran los métodos de acción.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs)]

Observe que el segundo método de acción `Edit` va precedido del atributo `HttpPost`. Este atributo especifica que sobrecarga de la `Edit` método se puede invocar para solicitudes POST. Podría aplicar la `HttpGet` atributo al primer editar método, pero que no es necesario porque es el valor predeterminado. (Nos referiremos a métodos de acción que se le asigna implícitamente la `HttpGet` atributo como `HttpGet` métodos.)

El `HttpGet` `Edit` método toma el parámetro de identificador de película, busca la película con Entity Framework `Find` método y devuelve la película seleccionada a la vista de edición. Cuando el sistema de scaffolding creó la vista de edición, examinó la clase `Movie` y creó código para representar los elementos `<label>` y `<input>` para cada propiedad de la clase. En el ejemplo siguiente se muestra la vista de edición que se ha generado:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

Observe cómo la plantilla de vista tiene un `@model MvcMovie.Models.Movie` instrucción en la parte superior del archivo: Especifica que la vista se espera que el modelo de la plantilla de vista para ser del tipo `Movie`.

El código con scaffolding utiliza varios *métodos auxiliares* para simplificar el formato HTML. El [ `Html.LabelFor` ](https://msdn.microsoft.com/en-us/library/gg401864(VS.98).aspx) auxiliar muestra el nombre del campo ("Title", "ReleaseDate", "Género" o "Price"). El [ `Html.EditorFor` ](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) aplicación auxiliar muestra HTML `<input>` elemento. El [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) aplicación auxiliar muestra los mensajes de validación asociados a esa propiedad.

Ejecute la aplicación y navegue hasta la */Movies* dirección URL. Haga clic en un vínculo **Edit** (Editar). En el explorador, vea el código fuente de la página. El código HTML en la página es similar al ejemplo siguiente. (El marcado de menú se excluyó para mayor claridad).

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample4.html)]

El `<input>` elementos se encuentran en un elemento HTML `<form>` elemento cuyo `action` atributo se establece para registrar el */películas/editar* dirección URL. Los datos del formulario se publicarán en el servidor cuando el **editar** se hace clic en el botón.

## <a name="processing-the-post-request"></a>Procesamiento de la solicitud POST

En la siguiente lista se muestra la versión `HttpPost` del método de acción `Edit`.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs)]

El enlazador de modelos ASP.NET framework toma los valores de formulario expuesto y crea un `Movie` objeto que se pasa como el `movie` parámetro. El `ModelState.IsValid` comprobación en el código comprueba que los datos presentados en el formulario pueden utilizarse para modificar un `Movie` objeto. Si los datos son válidos, el código guarda los datos de la película a la `Movies` colección de la `MovieDBContext` instancia. El código, a continuación, guarda los nuevos datos de la película en la base de datos mediante una llamada a la `SaveChanges` método `MovieDBContext`, que conserva los cambios realizados en la base de datos. Después de guardar los datos, el código redirige al usuario a la `Index` método de acción de la `MoviesController` (clase), que hace que la película actualizada que se mostrará en la lista de películas.

Si los valores registrados no son válidos, se vuelven a mostrar en el formulario. El `Html.ValidationMessageFor` aplicaciones auxiliares en el *Edit.cshtml* vista plantilla ocuparse de presentación de mensajes de error adecuado.

[![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image8.png)](examining-the-edit-methods-and-edit-view/_static/image7.png)

> **Tenga en cuenta acerca de configuraciones regionales** si trabaja habitualmente con una configuración regional distinto del inglés, vea [admiten validación de ASP.NET MVC 3 con configuraciones regionales no inglesas.](https://msdn.microsoft.com/en-us/library/gg674880(VS.98).aspx)


## <a name="making-the-edit-method-more-robust"></a>Hacer que el método Edit más sólida

El `HttpGet` `Edit` método generado por el sistema de scaffolding no comprueba que el identificador que se pasa es válido. Si un usuario quita el segmento de Id. de la dirección URL (`http://localhost:xxxxx/Movies/Edit`), se muestra el siguiente error:

[![Null_ID](examining-the-edit-methods-and-edit-view/_static/image10.png)](examining-the-edit-methods-and-edit-view/_static/image9.png)

Un usuario también podría pasar un identificador que no existe en la base de datos, como `http://localhost:xxxxx/Movies/Edit/1234`. Puede hacer dos cambios en el `HttpGet` `Edit` método de acción para resolver esta limitación. En primer lugar, cambie la `ID` parámetro tenga un valor predeterminado de cero cuando un identificador no se pasa explícitamente. También puede comprobar que la `Find` método encuentra realmente una película antes de devolver el objeto de película a la plantilla de vista. La actualización `Edit` método se muestra a continuación.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cs)]

Si no se encuentra ninguna película, el `HttpNotFound` se llama al método.

Todos los `HttpGet` métodos siguen un patrón similar. Obtienen un objeto de película (o una lista de objetos, en el caso de `Index`) y pasar el modelo a la vista. El `Create` método pasa un objeto de película vacía a la vista de creación. Todos los métodos que crean, editan, eliminan o modifican los datos lo hacen en la sobrecarga `HttpPost` del método. Modificar datos en un método GET de HTTP es un riesgo de seguridad, como se describe en la entrada de blog post [ASP.NET MVC Sugerencia nº 46: no use eliminar vínculos porque crean vulnerabilidades de seguridad](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Modificar datos en un método GET también infringe prácticas recomendadas de HTTP y el modelo de arquitectura REST, que especifica que las solicitudes GET no deben cambiar el estado de la aplicación. En otras palabras, la realización de una operación GET debe ser una operación segura que no tiene efectos secundarios.

## <a name="adding-a-search-method-and-search-view"></a>Agregar un método de búsqueda y la vista de búsqueda

En esta sección agregará un `SearchIndex` método de acción que le permite buscar películas por género o por nombre. Esto estará disponible mediante la */películas/SearchIndex* dirección URL. La solicitud mostrará un formulario HTML que contiene los elementos de entrada que un usuario puede rellenar con el fin de buscar una película. Cuando un usuario envía el formulario, el método de acción obtendrá los valores de búsqueda registrados por el usuario y usar los valores para buscar la base de datos.

## <a name="displaying-the-searchindex-form"></a>Mostrar el formulario SearchIndex

Empiece agregando un `SearchIndex` método de acción a la existente `MoviesController` clase. El método devolverá una vista que contiene un formulario HTML. Este es el código:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cs)]

La primera línea de la `SearchIndex` método crea las siguientes [LINQ](https://msdn.microsoft.com/en-us/library/bb397926.aspx) consulta para seleccionar las películas:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cs)]

La consulta se define en este momento, pero aún no se ha ejecutado en el almacén de datos.

Si el `searchString` parámetro contiene una cadena, se modificará la consulta de películas para filtrar según el valor de la cadena de búsqueda, utilizando el código siguiente:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

Las consultas LINQ no se ejecutan cuando se definen o cuando se modifican mediante una llamada a un método como `Where` o `OrderBy`. En su lugar, se aplaza la ejecución de la consulta, lo que significa que la evaluación de una expresión se retrasa hasta que su valor producida realmente se recorre en iteración o el [ `ToList` ](https://msdn.microsoft.com/en-us/library/bb342261.aspx) se llama al método. En la `SearchIndex` ejemplo, se ejecuta la consulta en la vista SearchIndex. Para más información sobre la ejecución de consultas en diferido, vea [Ejecución de la consulta](https://msdn.microsoft.com/en-us/library/bb738633.aspx).

Ahora puede implementar la `SearchIndex` vista que se mostrará el formulario al usuario. Pulse el botón derecho dentro de la `SearchIndex` método y, a continuación, haga clic en **agregar vista**. En el **agregar vista** diálogo cuadro, especifique que va a pasar un `Movie` objeto a la plantilla de vista como su clase de modelo. En el **plantilla Scaffold** elija **lista**, a continuación, haga clic en **agregar**.

[![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image12.png)](examining-the-edit-methods-and-edit-view/_static/image11.png)

Al hacer clic en el **agregar** botón, el *Views\Movies\SearchIndex.cshtml* se crea la plantilla de vista. Dado que seleccionó **lista** en el **plantilla Scaffold** enumerar, Visual Web Developer genera automáticamente (scaffolding) algún contenido de forma predeterminada en la vista. La técnica scaffolding crea un formulario HTML. También se ha examinado el `Movie` clase y código creado para representar `<label>` elementos para cada propiedad de la clase. La siguiente lista muestra la vista de creación que se ha generado:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

Ejecute la aplicación y vaya a */películas/SearchIndex*. Anexe una cadena de consulta como `?searchString=ghost` a la dirección URL. Se muestran las películas filtradas.

[![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image14.png)](examining-the-edit-methods-and-edit-view/_static/image13.png)

Si cambia la firma de la `SearchIndex` método que tiene un parámetro denominado `id`, el `id` parámetro coincidirá la `{id}` enruta el marcador de posición para el valor predeterminado establecido en el *Global.asax* archivo.

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample11.json)]

Modificados `SearchIndex` método tendría el siguiente aspecto:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample12.cs)]

Ahora puede pasar el título de la búsqueda como datos de ruta (un segmento de dirección URL) en lugar de como un valor de cadena de consulta.

[![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image16.png)](examining-the-edit-methods-and-edit-view/_static/image15.png)

Sin embargo, no se puede esperar que los usuarios modifiquen la dirección URL cada vez que quieran buscar una película. Por lo que ahora se agregará la interfaz de usuario para ayudar a ellos filtrar películas. Si cambia la firma de la `SearchIndex` cambiarlo de método para probar cómo pasar el parámetro de identificador límite de ruta, modo que su `SearchIndex` método toma un parámetro de cadena denominado `searchString`:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample13.cs)]

Abra la *Views\Movies\SearchIndex.cshtml* archivo y, justo después `@Html.ActionLink("Create New", "Create")`, agregue lo siguiente:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample14.cshtml)]

En el ejemplo siguiente se muestra una parte de la *Views\Movies\SearchIndex.cshtml* archivo con el marcado de filtrado agregado.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample15.cshtml)]

El `Html.BeginForm` auxiliar crea apertura `<form>` etiqueta. El `Html.BeginForm` auxiliar hace que el formulario registrar a sí mismo cuando el usuario envía el formulario, haga clic en el **filtro** botón.

Ejecute la aplicación y pruebe a buscar una película.

[![SearchIndxIE9_title](examining-the-edit-methods-and-edit-view/_static/image18.png)](examining-the-edit-methods-and-edit-view/_static/image17.png)

No hay ningún `HttpPost` sobrecarga de la `SearchIndex` método. No es necesario, porque el método no cambia el estado de la aplicación, simplemente filtrar datos.

Después, puede agregar el método `HttpPost SearchIndex` siguiente. En ese caso, coincidiría con el invocador de acción el `HttpPost SearchIndex` método y el `HttpPost SearchIndex` método ejecutaría tal como se muestra en la imagen siguiente.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample16.cs)]

[![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image20.png)](examining-the-edit-methods-and-edit-view/_static/image19.png)

Sin embargo, aunque agregue esta versión de `HttpPost` al método `SearchIndex`, hay una limitación en cómo se ha implementado todo esto. Supongamos que quiere marcar una búsqueda en particular o que quiere enviar un vínculo a sus amigos donde puedan hacer clic para ver la misma lista filtrada de películas. Tenga en cuenta que la dirección URL de la solicitud HTTP POST es el mismo que la dirección URL de la solicitud de obtención (localhost:xxxxx/películas/SearchIndex), no hay información de búsqueda en la propia dirección URL. Derecha ahora, la información de la cadena de búsqueda se envía al servidor como un valor de campo de formulario. Esto significa que no se puede capturar dicha información de búsqueda para marcar o enviar a amigos en una dirección URL.

La solución consiste en utilizar una sobrecarga del `BeginForm` que especifica que la solicitud POST debe agregar la información de búsqueda a la dirección URL y que debería ser enrutado a la versión HttpGet de la `SearchIndex` método. Reemplazar el existente sin parámetros `BeginForm` método con lo siguiente:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample17.cshtml)]

[![BeginFormPost_SM](examining-the-edit-methods-and-edit-view/_static/image22.png)](examining-the-edit-methods-and-edit-view/_static/image21.png)

Ahora cuando se envía una búsqueda, la dirección URL contiene una cadena de consulta de búsqueda. La búsqueda también será dirigida al método de acción `HttpGet SearchIndex`, aunque tenga un método `HttpPost SearchIndex`.

[![SearchIndexWithGetURL](examining-the-edit-methods-and-edit-view/_static/image24.png)](examining-the-edit-methods-and-edit-view/_static/image23.png)

## <a name="adding-search-by-genre"></a>Adición de búsqueda por género

Si agregó la `HttpPost` versión de la `SearchIndex` método elimina ahora.

A continuación, agregará una característica para que los usuarios puedan buscar películas por género. Reemplace el método `SearchIndex` con el código siguiente:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample18.cs)]

Esta versión de la `SearchIndex` método toma un parámetro adicional, es decir, `movieGenre`. Las primeras líneas de código crean un `List` objeto para mantener géneros de película de la base de datos.

El código siguiente es una consulta LINQ que recupera todos los géneros de la base de datos.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample19.cs)]

El código usa el `AddRange` método de la clase genérica `List` colección para agregar todos los géneros distintos a la lista. (Sin el `Distinct` modificador, se agregaría géneros duplicados, por ejemplo, se agregaría Comedia dos veces en nuestro ejemplo). El código, a continuación, almacena la lista de géneros en la `ViewBag` objeto.

El código siguiente muestra cómo comprobar el `movieGenre` parámetro. Si no está vacío el código más restringe la consulta de películas para limitar las películas seleccionadas para el género especificado.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample20.cs)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>Agregar marcas a la vista de SearchIndex para admitir la búsqueda por género

Agregar un `Html.DropDownList` auxiliar para la *Views\Movies\SearchIndex.cshtml* archivo, justo antes del `TextBox` auxiliar. A continuación se muestra el marcado completado:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample21.cshtml)]

Ejecute la aplicación y vaya a */películas/SearchIndex*. Intenta una búsqueda por género, nombre de la película y ambos criterios.

En esta sección se examinan los métodos de acción CRUD y las vistas generadas por el marco de trabajo. Crea un método de acción de búsqueda y una vista que los usuarios pueden buscar por título de la película y el género. En la siguiente sección, examinará cómo agregar una propiedad a la `Movie` modelo y cómo agregar un inicializador que creará automáticamente una base de datos de prueba.

>[!div class="step-by-step"]
[Anterior](accessing-your-models-data-from-a-controller.md)
[Siguiente](adding-a-new-field.md)
