---
uid: mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
title: Examinar los métodos de edición y vista de edición | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 05/22/2015
ms.assetid: 52a4d5fe-aa31-4471-b3cb-a064f82cb791
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 27c4bcc6dd127fe1a430aaec462e2c19a5fb7851
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577384"
---
<a name="examining-the-edit-methods-and-edit-view"></a>Examinar los métodos de edición y vista de edición
====================
por [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](sample/code-location.md)]

En esta sección, podrá examinar generado `Edit` vistas para el controlador movie y métodos de acción. Pero primero tendrá un breve desvío para hacer que la fecha de lanzamiento tenga mejor aspecto. Abra el *Models\Movie.cs* archivo y agregue las líneas resaltadas se muestra a continuación:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cs?highlight=2,12-14)]

También puede realizar la referencia cultural de la fecha específica similar al siguiente:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs?highlight=3)]

En el próximo tutorial hablaremos de [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx). El atributo [Display](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayattribute.aspx) especifica qué se muestra como nombre de un campo (en este caso, "Release Date" en lugar de "ReleaseDate"). El [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributo especifica el tipo de datos, en este caso es una fecha, por lo que no se muestra la información de hora almacenada en el campo. El [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atributo es necesario para un error en el Explorador de Chrome que representa formatos de fecha incorrectamente.

Ejecute la aplicación y vaya a la `Movies` controlador. Mantenga el puntero del mouse sobre un **editar** vínculo para ver la dirección URL que está vinculado.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

El **editar** vínculo generado por el `Html.ActionLink` método en el *Views\Movies\Index.cshtml* vista:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

El `Html` objeto es una aplicación auxiliar que se expone mediante una propiedad en el [System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) clase base. El `ActionLink` método de la aplicación auxiliar facilita la tarea generar dinámicamente hipervínculos HTML que se vinculan a los métodos de acción en los controladores. El primer argumento para el `ActionLink` método es para representar el texto del vínculo (por ejemplo, `<a>Edit Me</a>`). El segundo argumento es el nombre del método de acción para invocar (en este caso, el `Edit` acción). El argumento final es un [objeto anónimo](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) que genera los datos de ruta (en este caso, el identificador de 4).

El vínculo generado se muestra en la imagen anterior es `http://localhost:1234/Movies/Edit/4`. La ruta predeterminada (establecida en *aplicación\_start\routeconfig*) toma el patrón de URL `{controller}/{action}/{id}`. Por lo tanto, se traduce ASP.NET `http://localhost:1234/Movies/Edit/4` en una solicitud para el `Edit` método de acción de la `Movies` controlador con el parámetro `ID` igual a 4. Examine el código siguiente desde el *aplicación\_start\routeconfig* archivo. El [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) método se utiliza para enrutar las solicitudes HTTP para el método de acción y controlador correcto y proporcione el parámetro de Id. opcional. El [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) también se usa el método por el [HtmlHelpers](https://msdn.microsoft.com/library/system.web.mvc.htmlhelper(v=vs.108).aspx) como `ActionLink` para generar direcciones URL que proporciona el controlador, el método de acción y los datos de ruta.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cs?highlight=7)]

También puede pasar parámetros de método de acción con una cadena de consulta. Por ejemplo, la dirección URL `http://localhost:1234/Movies/Edit?ID=3` también pasa el parámetro `ID` de 3 a la `Edit` método de acción de la `Movies` controlador.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Abra el `Movies` controlador. Los dos `Edit` a continuación se muestran los métodos de acción.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs?highlight=19-21)]

Observe que el segundo método de acción `Edit` va precedido del atributo `HttpPost`. Este atributo especifica que la sobrecarga de la `Edit` se puede invocar el método solo para las solicitudes POST. Puede aplicar el `HttpGet` editar de atributo al primer método, pero que no es necesario porque es el valor predeterminado. (Nos referiremos a métodos de acción que se asignan implícitamente el `HttpGet` atributo como `HttpGet` métodos.) El [enlazar](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) atributo es otro mecanismo de seguridad importantes que impide que los piratas informáticos datos al modelo de publicación excesiva. Solo debe incluir propiedades en el atributo de enlace que desea cambiar. Puede leer acerca de la publicación excesiva y el atributo bind en mi [publicación excesiva nota de seguridad](https://go.microsoft.com/fwlink/?LinkId=317598). En el modelo simple que se usa en este tutorial, realizaremos el enlace todos los datos en el modelo. El [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) atributo se utiliza para impedir la falsificación de una solicitud y se empareja con `@Html.AntiForgeryToken()` en el archivo de vista de edición (*Views\Movies\Edit.cshtml*), a continuación se muestra una parte:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cshtml?highlight=9)]

`@Html.AntiForgeryToken()` genera un token antifalsificación de formulario oculto que debe coincidir en el `Edit` método de la `Movies` controlador. Puede leer más acerca de sitios falsificación (también conocida como XSRF o CSRF) en mi tutorial de solicitud [prevención de XSRF/CSRF en MVC](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md).

El `HttpGet` `Edit` método toma el parámetro de Id. de película, busca la película con Entity Framework `Find` método y devuelve la película seleccionada a la vista de edición. Si no se encuentra una película, [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) se devuelve. Cuando el sistema de scaffolding creó la vista de edición, examinó la clase `Movie` y creó código para representar los elementos `<label>` y `<input>` para cada propiedad de la clase. El ejemplo siguiente muestra la vista de edición que fue generada por el sistema de scaffolding de visual studio:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

Observe cómo la plantilla de vista tiene un `@model MvcMovie.Models.Movie` instrucción en la parte superior del archivo: Especifica que la vista espera que el modelo para la plantilla de vista del tipo `Movie`.

El código con scaffolding usa varios *métodos auxiliares* para simplificar el marcado HTML. El [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) auxiliar muestra el nombre del campo (&quot;título&quot;, &quot;ReleaseDate&quot;, &quot;género&quot;, o &quot;precio &quot;). El [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) aplicación auxiliar representa un elemento HTML `<input>` elemento. El [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) auxiliar muestra los mensajes de validación asociados a esa propiedad.

Ejecute la aplicación y vaya a la */Movies* dirección URL. Haga clic en un vínculo **Edit** (Editar). En el explorador, vea el código fuente de la página. A continuación se muestra el código HTML para el elemento de formulario.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cshtml?highlight=1-2)]

El `<input>` elementos se encuentran en un elemento HTML `<form>` elemento cuyo `action` atributo se establece para publicar en el */Movies/Edit* dirección URL. Los datos del formulario se publicarán en el servidor cuando el **guardar** se hace clic en el botón. La segunda línea muestra el texto oculto [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) token generado por el `@Html.AntiForgeryToken()` llamar.

## <a name="processing-the-post-request"></a>Procesamiento de la solicitud POST

En la siguiente lista se muestra la versión `HttpPost` del método de acción `Edit`.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

El [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) atributo valida el [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) token generado por el `@Html.AntiForgeryToken()` llama en la vista.

El [enlazador de modelos ASP.NET MVC](https://msdn.microsoft.com/library/dd410405.aspx) toma los valores de formulario publicados y crea un `Movie` objeto que se pasa como el `movie` parámetro. El método `ModelState.IsValid` comprueba que los datos presentados en el formulario pueden usarse para modificar (editar o actualizar) un objeto `Movie`. Si los datos son válidos, los datos de la película se guardan en el `Movies` colección de los `db(MovieDBContext` instancia). Los nuevos datos de la película se guardan en la base de datos mediante una llamada a la `SaveChanges` método `MovieDBContext`. Después de guardar los datos, el código redirige al usuario al método de acción `Index` de la clase `MoviesController`, que muestra la colección de películas, incluidos los cambios que se acaban de hacer.

Tan pronto como la validación del lado cliente determina que los valores de un campo no son válidos, se muestra un mensaje de error. Si deshabilita JavaScript, no tendrá la validación del lado cliente, pero el servidor detectará los valores expuestos no son válidos y se volverán a los valores de formulario con los mensajes de error. Más adelante en el tutorial se examina la validación con más detalle.

El `Html.ValidationMessageFor` aplicaciones auxiliares en el *Edit.cshtml* vista plantilla tenga cuidado de mostrar mensajes de error adecuado.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

Todas las `HttpGet` métodos siguen un patrón similar. Obtienen un objeto de película (o una lista de objetos, en el caso de `Index`) y pasar el modelo a la vista. El `Create` método pasa un objeto de película vacío a la vista de creación. Todos los métodos que crean, editan, eliminan o modifican los datos lo hacen en la sobrecarga `HttpPost` del método. Modificación de datos en un método HTTP GET es un riesgo de seguridad, como se describe en la entrada de blog post [ASP.NET MVC sugerencia #46: no use eliminar vínculos porque crean vulnerabilidades de seguridad](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Modificación de datos en un método GET también infringe procedimientos recomendados de HTTP y la arquitectura [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) de patrón, que especifica que las solicitudes GET no deben cambiar el estado de la aplicación. En otras palabras, realizar una operación GET debería ser una operación segura sin efectos secundarios, que no modifica los datos persistentes.

Si usas un equipo de inglés de Estados Unidos, puede omitir esta sección y vaya al siguiente tutorial. Puede descargar la versión Globalize de este tutorial [aquí](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=16475). Para un excelente tutorial de dos partes en la internacionalización, consulte [ASP.NET MVC 5 internacionalización de Nadeem](http://afana.me/post/aspnet-mvc-internationalization.aspx).


> [!NOTE]
> Para admitir la validación de jQuery para configuraciones regionales distintas del inglés que usan una coma (&quot;,&quot;) para un separador decimal y formatos de fecha distintos del inglés de EE. UU., se debe incluir *globalize.js* y específica de su  *Cultures/globalize.Cultures.js* archivo (desde [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) y JavaScript para usar `Globalize.parseFloat`. Puede obtener la validación de jQuery no inglesas de NuGet. (No instale Globalize si usa una configuración regional en inglés).


1. Desde el **herramientas** menú haga clic en **Administrador de paquetes NuGetLibrary**y, a continuación, haga clic en **administrar paquetes de NuGet para la solución**.  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image5.png)
2. En el panel izquierdo, seleccione <strong>examinar *.</strong>* (Consulte la imagen siguiente).
3. En el cuadro de entrada, escriba * Globalize **.  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image6.png) Elija `jQuery.Validation.Globalize`, elija `MvcMovie` y haga clic en **instalar**. El *Scripts\jquery.globalize\globalize.js* archivo se agregará al proyecto. El *Scripts\jquery.globalize\cultures\* carpeta contiene muchos archivos de JavaScript de la referencia cultural. Tenga en cuenta que puede tardar cinco minutos en instalar este paquete.

   El código siguiente muestra las modificaciones en el archivo Views\Movies\Edit.cshtml: 

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

Para evitar repetir este código en cada vista de edición, puede mover el archivo de diseño. Para optimizar la descarga del script, consulte mi tutorial [unión y Minificación](../../performance/bundling-and-minification.md).

Para obtener más información, consulte [internacionalización de ASP.NET MVC 3](http://afana.me/post/aspnet-mvc-internationalization.aspx) y [ASP.NET MVC 3 internacionalización - Part 2 (NerdDinner)](http://afana.me/post/aspnet-mvc-internationalization-part-2.aspx).

Como solución temporal, si no se puede obtener la validación de trabajar en la configuración regional, puede forzar el equipo para utilizar el inglés de Estados Unidos o puede deshabilitar JavaScript en el explorador. Para forzar que el equipo para utilizar el inglés de Estados Unidos, puede agregar el elemento de globalización en la raíz de los proyectos *web.config* archivo. El código siguiente muestra el elemento de globalización con la referencia cultural establecida en inglés de Estados Unidos.

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample11.xml)]

<a id="gettingstarted"></a><a id="jQueryAjaxJSON"></a> En el tutorial siguiente, implementaremos la funcionalidad de búsqueda.

> [!div class="step-by-step"]
> [Anterior](accessing-your-models-data-from-a-controller.md)
> [Siguiente](adding-search.md)
