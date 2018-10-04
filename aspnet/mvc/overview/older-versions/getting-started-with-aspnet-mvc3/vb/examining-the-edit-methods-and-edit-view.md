---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/examining-the-edit-methods-and-edit-view
title: Examinar los métodos de edición y la vista Edit (VB) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 5cb3c59b-1e96-464b-b3a8-c55607201872
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 509f362a15990486cc5fa4f2f666c3d0de2434dc
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578124"
---
<a name="examining-the-edit-methods-and-edit-view-vb"></a>Examinar los métodos de edición y la vista Edit (VB)
====================
por [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio. Antes de empezar, asegúrese de que ha instalado los requisitos previos descritos a continuación. Puede instalar todos ellos haciendo clic en el siguiente vínculo: [instalador de plataforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar individualmente los requisitos previos mediante los vínculos siguientes:
> 
> - [Requisitos previos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tiempo de ejecución de herramientas de soporte técnico +)
> 
> Si usa Visual Studio 2010, en lugar de Visual Web Developer 2010, instale los requisitos previos, haga clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un proyecto de Visual Web Developer con código fuente VB.NET está disponible como acompañamiento de este tema. [Descargue la versión VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si lo prefiere C#, cambie a la [C# versión](../cs/examining-the-edit-methods-and-edit-view.md) de este tutorial.


En esta sección, podrá examinar los métodos de acción generado y vistas para el controlador de película. A continuación, agregará una página de búsqueda personalizada.

Ejecute la aplicación y vaya a la `Movies` controlador anexando */Movies* a la dirección URL en la barra de direcciones del explorador. Mantenga el puntero del mouse sobre un **editar** vínculo para ver la dirección URL que está vinculado.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

El **editar** vínculo generado por el `Html.ActionLink` método en el *Views\Movies\Index.vbhtml* vista:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

[![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image3.png)](examining-the-edit-methods-and-edit-view/_static/image2.png)

El `Html` objeto es una aplicación auxiliar que se expone mediante una propiedad en el `WebViewPage` clase base. El `ActionLink` método de la aplicación auxiliar facilita la tarea generar dinámicamente hipervínculos HTML que se vinculan a los métodos de acción en los controladores. El primer argumento para el `ActionLink` método es para representar el texto del vínculo (por ejemplo, `<a>Edit Me</a>`). El segundo argumento es el nombre del método de acción para invocar. El argumento final es un [objeto anónimo](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) que genera los datos de ruta (en este caso, el identificador de 4).

El vínculo generado se muestra en la imagen anterior es `http://localhost:xxxxx/Movies/Edit/4`. La ruta predeterminada tiene el patrón de URL `{controller}/{action}/{id}`. Por lo tanto, se traduce ASP.NET `http://localhost:xxxxx/Movies/Edit/4` en una solicitud para el `Edit` método de acción de la `Movies` controlador con el parámetro `ID` igual a 4.

También puede pasar parámetros de método de acción con una cadena de consulta. Por ejemplo, la dirección URL `http://localhost:xxxxx/Movies/Edit?ID=4` también pasa el parámetro `ID` de 4 a la `Edit` método de acción de la `Movies` controlador.

[![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image5.png)](examining-the-edit-methods-and-edit-view/_static/image4.png)

Abra el `Movies` controlador. Los dos `Edit` a continuación se muestran los métodos de acción.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample3.vb)]

Observe que el segundo método de acción `Edit` va precedido del atributo `HttpPost`. Este atributo especifica esa sobrecarga de la `Edit` se puede invocar el método solo para las solicitudes POST. Puede aplicar el `HttpGet` editar de atributo al primer método, pero que no es necesario porque es el valor predeterminado. (Nos referiremos a métodos de acción que se asignan implícitamente el `HttpGet` atributo como `HttpGet` métodos.)

El `HttpGet` `Edit` método toma el parámetro de Id. de película, busca la película con Entity Framework `Find` método y devuelve la película seleccionada a la vista de edición. Cuando el sistema de scaffolding creó la vista de edición, examinó la clase `Movie` y creó código para representar los elementos `<label>` y `<input>` para cada propiedad de la clase. El ejemplo siguiente muestra la vista de edición que se ha generado:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample4.vbhtml)]

Observe cómo la plantilla de vista tiene un `@ModelType MvcMovie.Models.Movie` instrucción en la parte superior del archivo: Especifica que la vista espera que el modelo para la plantilla de vista del tipo `Movie`.

El código con scaffolding usa varios *métodos auxiliares* para simplificar el marcado HTML. El [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) auxiliar muestra el nombre del campo (&quot;título&quot;, &quot;ReleaseDate&quot;, &quot;género&quot;, o &quot;precio &quot;). El [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) muestra de aplicación auxiliar HTML `<input>` elemento. El [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) auxiliar muestra los mensajes de validación asociados a esa propiedad.

Ejecute la aplicación y vaya a la */Movies* dirección URL. Haga clic en un vínculo **Edit** (Editar). En el explorador, vea el código fuente de la página. El código HTML en la página es similar al ejemplo siguiente. (El marcado de menú se excluyó para mayor claridad).

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample5.html)]

El `<input>` elementos se encuentran en un elemento HTML `<form>` elemento cuyo `action` atributo se establece para publicar en el */Movies/Edit* dirección URL. Los datos del formulario se publicarán en el servidor cuando el **editar** se hace clic en el botón.

## <a name="processing-the-post-request"></a>Procesamiento de la solicitud POST

En la siguiente lista se muestra la versión `HttpPost` del método de acción `Edit`.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample6.vb)]

El enlazador de modelos ASP.NET framework toma los valores de formulario publicados y crea un `Movie` objeto que se pasa como el `movie` parámetro. El `ModelState.IsValid` proceso en el código comprueba que los datos presentados en el formulario se pueden usar para modificar un `Movie` objeto. Si los datos son válidos, el código guarda los datos de la película a la `Movies` colección de los `MovieDBContext` instancia. El código, a continuación, guarda los nuevos datos de película en la base de datos mediante una llamada a la `SaveChanges` método `MovieDBContext`, que conserva los cambios en la base de datos. Después de guardar los datos, el código redirige al usuario a la `Index` método de acción de la `MoviesController` (clase), lo que hace que la película actualizada que se mostrará en la lista de películas.

Si los valores expuestos no son válidos, se vuelven a mostrar en el formulario. El `Html.ValidationMessageFor` aplicaciones auxiliares en el *Edit.vbhtml* vista plantilla tenga cuidado de mostrar mensajes de error adecuado.

[![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image7.png)](examining-the-edit-methods-and-edit-view/_static/image6.png)

> **Nota sobre las configuraciones regionales** si normalmente trabaja con una configuración regional distinto del inglés, vea [que admiten validación de ASP.NET MVC 3 con configuraciones regionales de inglés.](https://msdn.microsoft.com/library/gg674880(VS.98).aspx)


## <a name="making-the-edit-method-more-robust"></a>Hacer que el método de edición más sólido

El `HttpGet` `Edit` método generado por el sistema de scaffolding no comprueba que el identificador que se pasa es válido. Si un usuario elimina el segmento de Id. de la dirección URL (`http://localhost:xxxxx/Movies/Edit`), se muestra el error siguiente:

[![Null_ID](examining-the-edit-methods-and-edit-view/_static/image9.png)](examining-the-edit-methods-and-edit-view/_static/image8.png)

Un usuario también podría pasar un identificador que no existe en la base de datos, como `http://localhost:xxxxx/Movies/Edit/1234`. Puede realizar dos cambios a la `HttpGet` `Edit` método de acción para abordar esta limitación. En primer lugar, cambie el `ID` parámetro tenga un valor predeterminado de cero cuando un identificador no se pasa explícitamente. También puede comprobar que el `Find` método realmente encuentra una película antes de devolver el objeto de película a la plantilla de vista. La actualización `Edit` método se muestra a continuación.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample7.vb)]

Si no se encuentra ninguna película, el `HttpNotFound` se llama al método.

Todas las `HttpGet` métodos siguen un patrón similar. Obtienen un objeto de película (o una lista de objetos, en el caso de `Index`) y pasar el modelo a la vista. El `Create` método pasa un objeto de película vacío a la vista de creación. Todos los métodos que crean, editan, eliminan o modifican los datos lo hacen en la sobrecarga `HttpPost` del método. Modificación de datos en un método HTTP GET es un riesgo de seguridad, como se describe en la entrada de blog post [ASP.NET MVC sugerencia #46: no use eliminar vínculos porque crean vulnerabilidades de seguridad](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Modificación de datos en un método GET también infringe procedimientos recomendados de HTTP y el patrón de arquitectura REST, que especifica que las solicitudes GET no deben cambiar el estado de la aplicación. En otras palabras, realizar una operación GET debería ser una operación segura que no tiene efectos secundarios.

## <a name="adding-a-search-method-and-search-view"></a>Agregar un método de búsqueda y la vista de búsqueda

En esta sección agregará un `SearchIndex` método de acción que le permite buscar películas por género o un nombre. Esto estará disponible mediante la */Movies/SearchIndex* dirección URL. La solicitud mostrará un formulario HTML que contiene los elementos de entrada que un usuario puede rellenar con el fin de buscar una película. Cuando un usuario envía el formulario, el método de acción obtendrá los valores de búsqueda enviados por el usuario y usar los valores para buscar la base de datos.

![SearchIndx_SM](examining-the-edit-methods-and-edit-view/_static/image10.png)

## <a name="displaying-the-searchindex-form"></a>Mostrando el formulario SearchIndex

Empiece agregando un `SearchIndex` método de acción existente `MoviesController` clase. El método devolverá una vista que contiene un formulario HTML. Este es el código:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample8.vb)]

La primera línea de la `SearchIndex` método crea los siguientes [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) consulta para seleccionar las películas:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample9.vb)]

La consulta se define en este momento, pero aún no se ha ejecutado en el almacén de datos.

Si el `searchString` parámetro contiene una cadena, la consulta de películas se modifica para filtrar según el valor de la cadena de búsqueda, utilizando el código siguiente:

Si no es así, a continuación, String.IsNullOrEmpty(searchString)   
 películas = películas. Dónde (o funciones s.Title.Contains(searchString))   
 End If

Las consultas LINQ no se ejecutan cuando se definen o cuando se modifican mediante una llamada a un método como `Where` o `OrderBy`. En su lugar, se aplaza la ejecución de la consulta, lo que significa que la evaluación de una expresión se retrasa hasta que su valor realizado se repita realmente o [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) se llama al método. En el `SearchIndex` ejemplo, la consulta se ejecuta en la vista SearchIndex. Para más información sobre la ejecución de consultas en diferido, vea [Ejecución de la consulta](https://msdn.microsoft.com/library/bb738633.aspx).

Ahora puede implementar la `SearchIndex` vista que se mostrará el formulario al usuario. Haga clic en el `SearchIndex` método y, a continuación, haga clic en **agregar vista**. En el **agregar vista** diálogo cuadro, especifique que va a pasar un `Movie` objeto a la plantilla de vista como su clase de modelo. En el **plantilla Scaffold** elija **lista**, a continuación, haga clic en **agregar**.

[![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image12.png)](examining-the-edit-methods-and-edit-view/_static/image11.png)

Al hacer clic en el **agregar** botón, el *Views\Movies\SearchIndex.vbhtml* se crea la plantilla de vista. Dado que seleccionó **lista** en el **plantilla Scaffold** enumerar, Visual Web Developer genera automáticamente (scaffolding) algún contenido de forma predeterminada en la vista. La técnica de scaffolding crea un formulario HTML. También se ha examinado el `Movie` clase y código creado para representar `<label>` elementos para cada propiedad de la clase. La siguiente lista muestra la vista de creación que se ha generado:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.vbhtml)]

Ejecute la aplicación y vaya a */Movies/SearchIndex*. Anexe una cadena de consulta como `?searchString=ghost` a la dirección URL. Se muestran las películas filtradas.

[![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image14.png)](examining-the-edit-methods-and-edit-view/_static/image13.png)

Si cambia la firma de la `SearchIndex` método tenga un parámetro denominado `id`, el `id` parámetro coincidirá la `{id}` enruta el marcador de posición para el valor predeterminado establecido en el *Global.asax* archivo.

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample11.json)]

Modificado `SearchIndex` método tendría el aspecto siguiente:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample12.vb)]

Ahora puede pasar el título de la búsqueda como datos de ruta (un segmento de dirección URL) en lugar de como un valor de cadena de consulta.

[![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image16.png)](examining-the-edit-methods-and-edit-view/_static/image15.png)

Sin embargo, no se puede esperar que los usuarios modifiquen la dirección URL cada vez que quieran buscar una película. Ahora va a agregar la interfaz de usuario para ayudar a ellos filtrar las películas. Si ha cambiado la firma de la `SearchIndex` método para probar cómo pasar el parámetro de identificador enlazado a una ruta, cámbielo para que su `SearchIndex` método toma un parámetro de cadena denominado `searchString`:

Abra el *Views\Movies\SearchIndex.vbhtml* archivo y, justo después `@Html.ActionLink("Create New", "Create")`, agregue lo siguiente:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample13.vbhtml)]

El `Html.BeginForm` auxiliar crea una apertura `<form>` etiqueta. El `Html.BeginForm` auxiliar hace que el formulario registrar a sí mismo cuando el usuario envía el formulario, haga clic en el **filtro** botón.

Ejecute la aplicación y pruebe a buscar una película.

[![SearchIndxIE9_title](examining-the-edit-methods-and-edit-view/_static/image18.png)](examining-the-edit-methods-and-edit-view/_static/image17.png)

No hay ningún `HttpPost` sobrecarga de la `SearchIndex` método. No es necesario, porque el método no cambia el estado de la aplicación, simplemente filtra los datos. Si ha agregado la siguiente `HttpPost` `SearchIndex` método, el invocador de acción coincidiría con el `HttpPost` `SearchIndex` método y el `HttpPost` `SearchIndex` método ejecutaría tal como se muestra en la imagen siguiente.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample14.vb)]

[![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image20.png)](examining-the-edit-methods-and-edit-view/_static/image19.png)

## <a name="adding-search-by-genre"></a>Adición de búsqueda por género

Si ha agregado el `HttpPost` versión de la `SearchIndex` método, elimínelo.

A continuación, agregará una característica para permitir que los usuarios buscar películas por género. Reemplace el método `SearchIndex` con el código siguiente:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample15.vb)]

Esta versión de la `SearchIndex` método toma un parámetro adicional, es decir, `movieGenre`. Las primeras líneas de código crean un `List` objeto para contener los géneros de película de la base de datos.

El código siguiente es una consulta LINQ que recupera todos los géneros de la base de datos.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample16.vb)]

El código usa el `AddRange` método de la clase genérica `List` colección para agregar los distintos géneros a la lista. (Sin el `Distinct` modificador, se agregaría géneros duplicados, por ejemplo, Comedia se agregaría dos veces en nuestro ejemplo). El código, a continuación, almacena la lista de géneros de la `ViewBag` objeto.

El código siguiente muestra cómo comprobar el `movieGenre` parámetro. Si no está vacío el código más restringe la consulta de películas para limitar las películas seleccionadas para el género especificado.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample17.vb)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>Agregar marcas a la vista de SearchIndex para admitir la búsqueda por género

Agregar un `Html.DropDownList` auxiliar para el *Views\Movies\SearchIndex.vbhtml* archivo, justo antes del `TextBox` auxiliar. A continuación se muestra el marcado completado:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample18.vbhtml)]

Ejecute la aplicación y vaya a */Movies/SearchIndex*. Pruebe una búsqueda por género, nombre de la película y ambos criterios.

En esta sección se examinan los métodos de acción de CRUD y las vistas generadas por el marco de trabajo. Crea un método de acción de búsqueda y la vista que permite a los usuarios buscar por título de la película y género. En la siguiente sección, echaremos un vistazo a cómo agregar una propiedad a la `Movie` modelo y cómo agregar un inicializador que creará automáticamente una base de datos de prueba.

> [!div class="step-by-step"]
> [Anterior](accessing-your-models-data-from-a-controller.md)
> [Siguiente](adding-a-new-field.md)
