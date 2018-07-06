---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
title: Usar la aplicación auxiliar DropDownList con ASP.NET MVC | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 01/12/2012
ms.assetid: 53767e05-c8ab-42e1-a94b-22d906195200
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 9f16c01515bde80d3994618d26818c2d93f69d87
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828382"
---
<a name="using-the-dropdownlist-helper-with-aspnet-mvc"></a>Usar la aplicación auxiliar DropDownList con ASP.NET MVC
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

Este tutorial le enseñará los aspectos básicos de trabajar con el [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) auxiliar y el [ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) auxiliar en una aplicación Web de ASP.NET MVC. Puede utilizar Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio para seguir el tutorial. Antes de empezar, asegúrese de que ha instalado los requisitos previos descritos a continuación. Puede instalar todos ellos haciendo clic en el siguiente vínculo: [instalador de plataforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar individualmente los requisitos previos mediante los vínculos siguientes:

- [Requisitos previos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack) <a id="post"></a>
- [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tiempo de ejecución de herramientas de soporte técnico +)

Si usa Visual Studio 2010, en lugar de Visual Web Developer 2010, instale los requisitos previos, haga clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack). En este tutorial se supone que ha completado la [Introducción a ASP.NET MVC](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) tutorial o[ASP.NET MVC Music Store](../mvc-music-store/mvc-music-store-part-1.md) tutorial o está familiarizado con el desarrollo de ASP.NET MVC. Este tutorial comienza con un proyecto modificado desde la [ASP.NET MVC Music Store](../mvc-music-store/mvc-music-store-part-1.md) tutorial. Puede descargar el proyecto de inicio con el siguiente vínculo [descargar la versión de C#](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829).

Un proyecto de Visual Web Developer con el tutorial completado de código fuente de C# está disponible como acompañamiento de este tema. [Descargar](https://code.msdn.microsoft.com/Using-the-DropDownList-67f9367d).

### <a name="what-youll-build"></a>¿Qué va a crear

Podrá crear métodos de acción y vistas que usan el [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) auxiliar para seleccionar una categoría. También utilizará **jQuery** para agregar un cuadro de diálogo de la categoría de inserción que se puede usar cuando se necesita una nueva categoría (por ejemplo, el género o un intérprete). A continuación es una captura de pantalla de la vista de creación que se muestran vínculos para agregar un nuevo género y agregar a un nuevo intérprete.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image1.png)

### <a name="skills-youll-learn"></a>Habilidades que aprenderá

Aquí es lo que aprenderá:

- Cómo usar el [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) auxiliar para seleccionar datos de categoría.
- Cómo agregar un **jQuery** cuadro de diálogo para agregar nuevas categorías.

### <a name="getting-started"></a>Introducción

Para comenzar, descargar el proyecto de inicio con el siguiente vínculo: [descargar](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829). En el Explorador de Windows, haga clic en el *DDL\_Starter.zip* de archivo y seleccione Propiedades. En el **DDL\_Starter.zip propiedades** cuadro de diálogo, seleccione desbloquear.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image2.png)

Haga clic con el botón secundario del mouse en el DDL\_Starter.zip archivo y seleccione **extraer todo** para descomprimir el archivo. Abra el *StartMusicStore.sln* archivo con Visual Studio 2010 o Visual Web Developer 2010 Express ("Visual Web Developer" o "VWD" para abreviar).

Presione CTRL+F5 para ejecutar la aplicación y haga clic en el **prueba** vínculo.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image3.png)

Seleccione el **Seleccionar categoría de película (Simple)** vínculo. Se muestra una lista Seleccionar tipo de película, con el valor seleccionado Comedia.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image4.png)

Haga clic con el botón derecho en el explorador y seleccione Ver código fuente. Se muestra el código HTML de la página. El código siguiente muestra el código HTML para el elemento select.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample1.html)]

Puede ver que cada elemento de la lista de selección tiene un valor (0 para la acción, 1 para Drama, 2 para Comedia y 3 para ciencia ficción) y un nombre para mostrar (acción, Drama, Comedia y ciencia ficción). El código anterior es HTML estándar para una lista de selección.

Cambie la lista de selección a Drama del sistema y presione la **enviar** botón. La dirección URL en el explorador es `http://localhost:2468/Home/CategoryChosen?MovieType=1` y muestra la página **seleccionó: 1**.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image5.png)

Abra el *controllers\homecontroller. cs* de archivo y examine el `SelectCategory` método.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample2.cs)]

El [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) auxiliar utilizada para crear una lista de selección HTML requiere un **IEnumerable&lt;SelectListItem &gt;** , explícita o implícitamente. Es decir, puede pasar el **IEnumerable&lt;SelectListItem &gt;**  explícitamente en el [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) auxiliar o puede agregar el **IEnumerable&lt; SelectListItem &gt;**  a la [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) con el mismo nombre para el **SelectListItem** como la propiedad del modelo. Pasando el **SelectListItem** forma implícita y explícita se trata en la siguiente parte del tutorial. El código anterior muestra la manera más sencilla posible para crear un **IEnumerable&lt;SelectListItem &gt;**  y rellenarlo con texto y valores. Tenga en cuenta la `Comedy` [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) tiene la [seleccionados](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.selected.aspx) propiedad establecida en **true;** Esto hará que la lista de selección representada mostrar **Comedia** como el elemento seleccionado en la lista.

El **IEnumerable&lt;SelectListItem &gt;**  creado anteriormente, se agrega a la [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) con el nombre MovieType. Esto indica cómo transferimos los **IEnumerable&lt;SelectListItem &gt;**  implícitamente en el [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) auxiliar que se muestra a continuación.

Abra el *Views\Home\SelectCategory.cshtml* de archivo y examine el marcado.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample3.cshtml)]

En la tercera línea, se establece el diseño en Views/Shared/\_Simple\_Layout.cshtml, que es una versión simplificada del archivo de diseño estándar. No esta opción para mantener la pantalla y representar HTML simple.

En este ejemplo no se está cambiando el estado de la aplicación, por lo que se enviará a los datos mediante un **HTTP GET**, no **HTTP POST**. Consulte la sección de W3C [lista de comprobación rápida para elegir HTTP GET o POST](http://www.w3.org/2001/tag/doc/whenToUseGet.html#checklist). Dado que no estamos cambiando la aplicación y publicar el formulario, se usa el [Html.BeginForm](https://msdn.microsoft.com/library/dd460344.aspx) sobrecarga que nos permite especificar el método de acción, el método de controlador y el formulario (**HTTP POST** o **HTTP GET**). Normalmente, las vistas contienen los [Html.BeginForm](https://msdn.microsoft.com/library/dd505244.aspx) sobrecarga que no toma ningún parámetro. La versión del parámetro no tiene como valor predeterminado para los datos del formulario a la versión de publicación del mismo método de acción y controlador de registro.

La línea siguiente

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample4.cshtml)]

pasa un argumento de cadena para la **DropDownList** auxiliar. Esta cadena, "MovieType" en nuestro ejemplo, hace dos cosas:

- Proporciona la clave para el **DropDownList** auxiliar para buscar un **IEnumerable&lt;SelectListItem &gt;**  en el **ViewBag**.
- Se está enlazado a datos para el elemento de formulario MovieType. Si el método de envío es **HTTP GET**, `MovieType` será una cadena de consulta. Si el método de envío es **HTTP POST**, `MovieType` se agregará en el cuerpo del mensaje. La siguiente imagen muestra la cadena de consulta con el valor de 1.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image6.png)

El código siguiente muestra el `CategoryChosen` se envíe el formulario del método.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample5.cs)]

Vaya a la página de prueba y seleccione el **HTML SelectList** vínculo. La página HTML representa un elemento select similar a la página de prueba simple de ASP.NET MVC. Haga clic en la ventana del explorador y seleccione **ver código fuente**. El marcado HTML de la lista de selección es básicamente idéntico. Página de prueba HTML, funciona como el método de acción de MVC de ASP.NET y la vista que hemos probado previamente.

### <a name="improving-the-movie-select-list-with-enums"></a>Mejora de la lista de selección de la película con las enumeraciones

Si las categorías de la aplicación son fijos y no cambiará, puede aprovechar las enumeraciones para que el código sea más sólido y más fácil de extender. Cuando se agrega una nueva categoría, se genera el valor de la categoría correcta. Al agregar una nueva categoría, pero se olvide de actualizar el valor de categoría, se evitan errores de copiar y pegar.

Abra el *controllers\homecontroller. cs* de archivo y examine el código siguiente:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample6.cs)]

El [enum](https://msdn.microsoft.com/library/sbbt4032(VS.80).aspx) `eMovieCategories` captura los tipos de cuatro película. El `SetViewBagMovieType` método crea el **IEnumerable&lt;SelectListItem &gt;**  desde el `eMovieCategories` **enum**y establece el `Selected` propiedad desde la `selectedMovie` parámetro. El `SelectCategoryEnum` método de acción utiliza la misma vista como el `SelectCategory` método de acción.

Vaya a la página de prueba y haga clic en el `Select Movie Category (Enum)` vínculo. Esta vez, en lugar de un valor (número) que se muestra, se muestra una cadena que representa la enumeración.

### <a name="posting-enum-values"></a>Registrar los valores de enumeración

Formularios HTML normalmente se usan para publicar datos en el servidor. El siguiente código muestra la `HTTP GET` y `HTTP POST` versiones de la `SelectCategoryEnumPost` método.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample7.cs)]

Al pasar un `eMovieCategories` enum para la `POST` método, podemos extraer el valor de enumeración y la cadena de la enumeración. Ejecutar el ejemplo y navegue hasta la página de prueba. Haga clic en el `Select Movie Category(Enum Post)` vínculo. Seleccione un tipo de película y, a continuación, presione el botón Enviar. La pantalla muestra el valor y el nombre del tipo movie.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image7.png)

### <a name="creating-a-multiple-section-select-element"></a>Creación de un elemento de sección selecciones múltiples

El [ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) aplicación auxiliar HTML representa el código HTML `<select>` elemento con el `multiple` atributo, que permite a los usuarios a hacer selecciones múltiples. Navegue hasta el vínculo de prueba y luego seleccione el **Multi Seleccionar país** vínculo. La interfaz de usuario presentado le permite seleccionar varios países. En la imagen siguiente, se seleccionan Canadá y China.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image8.png)

### <a name="examining-the-multiselectcountry-code"></a>Examinar el código de MultiSelectCountry

Examine el código siguiente desde el *controllers\homecontroller. cs* archivo.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample8.cs)]

El `GetCountries` método crea una lista de países, a continuación, pasa a la `MultiSelectList` constructor. El `MultiSelectList` sobrecarga del constructor en el `GetCountries` anterior método toma cuatro parámetros:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample9.cs)]

1. *elementos*: un [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) que contiene los elementos de la lista. En el ejemplo anterior, la lista de países.
2. *dataValueField*: el nombre de la propiedad en el **IEnumerable** lista que contiene el valor. En el ejemplo anterior, el `ID` propiedad.
3. *dataTextField*: el nombre de la propiedad en el **IEnumerable** lista que contiene la información para mostrar. En el ejemplo anterior, el `name` propiedad.
4. *selectedValues*: la lista de valores seleccionados.

En el ejemplo anterior, el `MultiSelectCountry` método pasa un `null` valor para los países seleccionados, por lo que no países estarán seleccionadas cuando se muestra la interfaz de usuario. El código siguiente muestra el marcado de Razor que se usa para representar el `MultiSelectCountry` vista.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample10.cshtml)]

La aplicación auxiliar HTML [ListBox](https://msdn.microsoft.com/library/dd470200.aspx) método utiliza antes toman dos parámetros, el nombre de la propiedad de enlace de modelo y la [MultiSelectList](https://msdn.microsoft.com/library/system.web.mvc.multiselectlist.aspx) que contiene los valores y seleccione opciones. El `ViewBag.YouSelected` código anterior se usa para mostrar los valores de los países que seleccionó al enviar el formulario. Examine la sobrecarga de HTTP POST de la `MultiSelectCountry` método.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample11.cs)]

El `ViewBag.YouSelected` propiedad dinámica que contiene los países seleccionados, obtenidos para el `Countries` entrada en la colección de formulario. En esta versión, el método GetCountries se pasa una lista de los países seleccionados, por lo que cuando el `MultiSelectCountry` se muestra la vista, se seleccionan los países seleccionados en la interfaz de usuario.

### <a name="making-a-select-element-friendly-with-the-harvest-chosen-jquery-plugin"></a>Realizar un seleccione elemento descriptivo con el complemento de jQuery Harvest elegido

La recopilación [elegido](http://harvesthq.github.com/chosen/) jQuery complemento puede agregarse a un elemento HTML &lt;seleccione&gt; elemento que se va a crear un usuario de la interfaz de usuario descriptivo. Las siguientes imágenes muestran la cosecha [elegido](http://harvesthq.github.com/chosen/) complemento de jQuery con `MultiSelectCountry` vista.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image9.png)

En las dos imágenes siguientes, **Canadá** está seleccionada.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image10.png)

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image11.png)

En la imagen anterior, se selecciona Canadá, y contiene un **x** puede hacer clic para quitar la selección. La imagen siguiente muestra Canadá, China, y Japón seleccionado.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image12.png)

### <a name="hooking-up-the-harvest-chosen-jquery-plugin"></a>Enlazar el complemento de jQuery Harvest elegido

La siguiente sección es más fácil de seguir si tiene alguna experiencia con jQuery. Si nunca ha usado jQuery antes, es posible que desee pruebe uno de los siguientes tutoriales de jQuery.

- [Cómo funciona jQuery](http://docs.jquery.com/Tutorials:How_jQuery_Works) por [John Resig](http://ejohn.org/)
- [Introducción a jQuery](http://docs.jquery.com/Tutorials:Getting_Started_with_jQuery) por [Jörn Zaefferer](http://bassistance.de/)
- [Ejemplos de jQuery de Live](http://codylindley.com/blogstuff/js/jquery/#) por [Cody Lindley](http://codylindley.com/)

El complemento elegido se incluye en los proyectos de ejemplo completo que acompañan a este tutorial y starter. En este tutorial sólo deberá usar jQuery para enlazarlo con la interfaz de usuario. Para usar el complemento de jQuery Harvest elegido en un proyecto de MVC de ASP.NET, debe:

1. Descargar el complemento seleccionado desde [github](https://github.com/harvesthq/chosen/). Este paso se ha realizado automáticamente.
2. Agregue la carpeta elegida para el proyecto de ASP.NET MVC. Agregue los recursos desde el complemento elegido que descargó en el paso anterior a la carpeta elegida. Este paso se ha realizado automáticamente.
3. El complemento elegido para enlazar el **DropDownList** o **ListBox** aplicación auxiliar HTML.

### <a name="hooking-up-the-chosen-plugin-to-the-multiselectcountry-view"></a>Enlazar el complemento seleccionado a la vista MultiSelectCountry.

Abra el *Views\Home\MultiSelectCountry.cshtml* archivo y agregue un `htmlAttributes` parámetro para el `Html.ListBox`. El parámetro que se va a agregar contiene un nombre de clase para la lista de selección (`@class = "chzn-select"`). El código completo se muestra a continuación:

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample12.cshtml)]

En el código anterior, estamos agregando el atributo HTML y el valor del atributo `class = "chzn-select"`. El carácter @ delante de la clase tiene nada que ver con el motor de vistas Razor. `class` es un [palabra clave de C#](https://msdn.microsoft.com/library/x53a06bb.aspx). Palabras clave de C# no se puede usar como identificadores a menos que incluyan "@" como prefijo. En el ejemplo anterior, `@class` es un identificador válido, pero **clase** no es porque **clase** es una palabra clave.

Agregue referencias a la *Chosen/chosen.jquery.js* y *Chosen/chosen.css* archivos. El *Chosen/chosen.jquery.js* e implementa la funcionalidad del complemento seleccionado. El *Chosen/chosen.css* archivo proporciona el estilo. Agregue estas referencias a la parte inferior de la *Views\Home\MultiSelectCountry.cshtml* archivo. El código siguiente muestra cómo hacer referencia el complemento elegido.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample13.cshtml)]

Active el complemento elegido con el nombre de clase usado en el **Html.ListBox** código. En el ejemplo anterior, el nombre de clase es `chzn-select`. Agregue la siguiente línea a la parte inferior de la *Views\Home\MultiSelectCountry.cshtml* ver el archivo. Esta línea activa el complemento elegido.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample14.html)]

La siguiente línea es la sintaxis para llamar a la función ready de jQuery, que selecciona el elemento DOM con el nombre de la clase `chzn-select`.

[!code-powershell[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample15.ps1)]

El valor ajustado devuelva el conjunto de la llamada anterior, a continuación, aplica el método elegido (`.chosen();`), que enlaza el complemento elegido.

El código siguiente muestra el completado *Views\Home\MultiSelectCountry.cshtml* ver el archivo.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample16.cshtml)]

Ejecute la aplicación y vaya a la `MultiSelectCountry` vista. Pruebe a agregar y eliminar los países. La descarga de ejemplo proporcionada también contiene un `MultiCountryVM` método y la vista que implementa la funcionalidad de MultiSelectCountry mediante una vista de modelo en lugar de un **ViewBag**.

En la sección siguiente verá cómo funciona el mecanismo de scaffolding de ASP.NET MVC con la **DropDownList** auxiliar.

> [!div class="step-by-step"]
> [Siguiente](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
