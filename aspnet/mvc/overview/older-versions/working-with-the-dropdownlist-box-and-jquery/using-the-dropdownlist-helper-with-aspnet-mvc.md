---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
title: "Usar la aplicación auxiliar DropDownList con ASP.NET MVC | Documentos de Microsoft"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 53767e05-c8ab-42e1-a94b-22d906195200
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: b8393e1503cb562a46a00f49b51c0cb64ff2cfdc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="using-the-dropdownlist-helper-with-aspnet-mvc"></a>Usar la aplicación auxiliar DropDownList con ASP.NET MVC
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

Este tutorial le enseñará los conceptos básicos sobre cómo trabajar con el [DropDownList](https://msdn.microsoft.com/en-us/library/dd492948.aspx) auxiliar y [ListBox](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.selectextensions.listbox.aspx) auxiliar en una aplicación Web de MVC de ASP.NET. Puede utilizar Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio para seguir el tutorial. Antes de empezar, asegúrese de que ha instalado los requisitos previos descritos a continuación. Puede instalar todas ellas haciendo clic en el siguiente vínculo: [instalador de plataforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar por separado los requisitos previos mediante los siguientes vínculos:

- [Los requisitos previos visuales Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)<a id="post"></a>
- [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(se admiten en tiempo de ejecución + herramientas)

Si usa Visual Studio 2010 en lugar de Visual Web Developer 2010, instale los requisitos previos, haga clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack). Este tutorial se da por supuesto que ha completado la [Introducción a ASP.NET MVC](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) tutorial o[tienda de música de ASP.NET MVC](../mvc-music-store/mvc-music-store-part-1.md) tutorial o está familiarizados con el desarrollo de ASP.NET MVC. Este tutorial comienza con un proyecto modificado desde la [tienda de música de ASP.NET MVC](../mvc-music-store/mvc-music-store-part-1.md) tutorial. Puede descargar el proyecto de inicio con el siguiente vínculo [descargar la versión de C#](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829).

Un proyecto de Visual Web Developer con el código fuente de C# del tutorial completado está disponible como acompañamiento de este tema. [Descargar](https://code.msdn.microsoft.com/Using-the-DropDownList-67f9367d).

### <a name="what-youll-build"></a>Lo que vamos a compilar

Podrá crear métodos de acción y vistas que usan el [DropDownList](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) auxiliar para seleccionar una categoría. También se utiliza **jQuery** para agregar un cuadro de diálogo de la categoría de inserción que se puede usar cuando se necesita una nueva categoría (por ejemplo, género o intérprete). A continuación se muestra una captura de pantalla de la vista de creación que se muestran vínculos para agregar un nuevo género y agregar a un intérprete nuevo.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image1.png)

### <a name="skills-youll-learn"></a>Obtendrá información de conocimientos

Esto es lo aprenderá:

- Cómo utilizar el [DropDownList](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) auxiliar para seleccionar datos de categoría.
- Cómo agregar un **jQuery** cuadro de diálogo para agregar nuevas categorías.

### <a name="getting-started"></a>Introducción

Empiece por descargar el proyecto de inicio con el siguiente vínculo: [descargar](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829). En el Explorador de Windows, haga clic en el *DDL\_Starter.zip* de archivo y seleccione Propiedades. En el **DDL\_Starter.zip propiedades** cuadro de diálogo, seleccione desbloquear.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image2.png)

Haga clic en el archivo DDL\_Starter.zip archivo y seleccione **extraer todo** para descomprimir el archivo. Abra la *StartMusicStore.sln* archivo con Visual Studio 2010 o Visual Web Developer 2010 Express ("Visual Web Developer" o "VWD" para abreviar).

Presione CTRL + F5 para ejecutar la aplicación y haga clic en el **prueba** vínculo.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image3.png)

Seleccione el **Seleccionar categoría de película (Simple)** vínculo. Se muestra una lista Select de tipo de película, con Comedia el valor seleccionado.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image4.png)

Haga clic con el botón derecho en el explorador y seleccione Ver código fuente. Se muestra el código HTML de la página. El código siguiente muestra el código HTML para el elemento select.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample1.html)]

Puede ver que cada elemento de la lista de selección tiene un valor (0 para la acción, 1 para series, 2 para Comedia y 3 para ciencia ficción) y un nombre para mostrar (acción, series, Comedia y ciencia ficción). El código anterior es HTML estándar para una lista de selección.

Cambiar la lista de selección para series del sistema y presione la **enviar** botón. La dirección URL en el explorador es `http://localhost:2468/Home/CategoryChosen?MovieType=1` y muestra la página **seleccionó: 1**.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image5.png)

Abra el *controllers\homecontroller* archivo y examine la `SelectCategory` método.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample2.cs)]

El [DropDownList](https://msdn.microsoft.com/en-us/library/dd492738.aspx) auxiliar utilizada para crear una lista de selección HTML requiere un **IEnumerable&lt;SelectListItem &gt;** , explícita o implícitamente. Es decir, puede pasar el **IEnumerable&lt;SelectListItem &gt;**  explícitamente en el [DropDownList](https://msdn.microsoft.com/en-us/library/dd492738.aspx) auxiliar o puede agregar el **IEnumerable&lt; SelectListItem &gt;**  a la [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) con el mismo nombre para el **SelectListItem** como la propiedad de modelo. Pasando el **SelectListItem** forma implícita y explícita se trata en la siguiente parte del tutorial. El código anterior muestra la manera más sencilla posible para crear una **IEnumerable&lt;SelectListItem &gt;**  y rellenarlo con texto y valores. Tenga en cuenta el `Comedy` [SelectListItem](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlistitem.aspx) tiene la [seleccionados](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlistitem.selected.aspx) propiedad establecida en **true;** Esto hará que la lista de selección representada mostrar **Comedia** como el elemento seleccionado en la lista.

El **IEnumerable&lt;SelectListItem &gt;**  creado anteriormente, se agrega a la [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) con el nombre MovieType. Se trata cómo pasamos el **IEnumerable&lt;SelectListItem &gt;**  implícitamente a la [DropDownList](https://msdn.microsoft.com/en-us/library/dd492738.aspx) auxiliar se muestra a continuación.

Abra la *Views\Home\SelectCategory.cshtml* archivo y examine el marcado.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample3.cshtml)]

En la tercera línea, se establece el diseño en vistas/compartida/\_Simple\_Layout.cshtml, que es una versión simplificada del archivo de diseño estándar. No esta opción para mantener la pantalla y representar HTML simple.

En este ejemplo no se está cambiando el estado de la aplicación, por lo que se vaya a enviar los datos mediante un **HTTP GET**, no **HTTP POST**. Vea la sección de W3C [lista de comprobación rápida para elegir HTTP GET o POST](http://www.w3.org/2001/tag/doc/whenToUseGet.html#checklist). Debido a que no estamos cambiando la aplicación y el formulario de registro, usamos el [Html.BeginForm](https://msdn.microsoft.com/en-us/library/dd460344.aspx) sobrecarga que nos permite especificar el método de acción, el método de controlador y formato (**HTTP POST** o **GET de HTTP**). Normalmente contienen vistas el [Html.BeginForm](https://msdn.microsoft.com/en-us/library/dd505244.aspx) sobrecarga que no toma ningún parámetro. La versión de parámetro no tiene como valor predeterminado para enviar los datos del formulario a la versión posterior del mismo método de acción y controlador.

La línea siguiente

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample4.cshtml)]

pasa un argumento de cadena para el **DropDownList** auxiliar. Esta cadena, "MovieType" en nuestro ejemplo, hace dos cosas:

- Proporciona la clave para la **DropDownList** auxiliar para buscar un **IEnumerable&lt;SelectListItem &gt;**  en el **ViewBag**.
- Se está enlazado a datos para el elemento de formulario MovieType. Si el método de envío es **HTTP GET**, `MovieType` será una cadena de consulta. Si el método de envío es **HTTP POST**, `MovieType` se agregará en el cuerpo del mensaje. La siguiente imagen muestra la cadena de consulta con el valor de 1.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image6.png)

El código siguiente muestra el `CategoryChosen` método se envíe el formulario.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample5.cs)]

Vaya a la página de prueba y seleccione el **HTML SelectList** vínculo. La página HTML representa un elemento select similar a la página de prueba de MVC de ASP.NET simple. Haga clic en la ventana del explorador y seleccione **ver código fuente**. El marcado HTML de la lista de selección es básicamente idéntico. Página de prueba HTML, funciona como el método de acción de MVC de ASP.NET y la vista que hemos probado previamente.

### <a name="improving-the-movie-select-list-with-enums"></a>Mejora de la lista de selección de la película con las enumeraciones

Si las categorías de la aplicación son fijos y no va a cambiar, puede sacar partido de las enumeraciones que el código sea más compacto y más fácil de extender. Cuando se agrega una nueva categoría, se genera el valor de categoría correcta. El evita los errores de copiar y pegar cuando se agrega una nueva categoría pero se olvida de actualizar el valor de categoría.

Abra la *controllers\homecontroller* de archivos y examine el código siguiente:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample6.cs)]

El [enum](https://msdn.microsoft.com/en-us/library/sbbt4032(VS.80).aspx) `eMovieCategories` captura los tipos de cuatro película. El `SetViewBagMovieType` método crea el **IEnumerable&lt;SelectListItem &gt;**  desde el `eMovieCategories` **enum**y establece el `Selected` propiedad desde el `selectedMovie` parámetro. El `SelectCategoryEnum` método de acción utiliza la misma vista que la `SelectCategory` método de acción.

Vaya a la página de prueba y haga clic en el `Select Movie Category (Enum)` vínculo. Esta vez, en lugar de un valor (número) que se muestra, se muestra una cadena que representa la enumeración.

### <a name="posting-enum-values"></a>Registrar los valores de enumeración

Formularios HTML se usan normalmente para publicar datos en el servidor. El código siguiente muestra el `HTTP GET` y `HTTP POST` versiones de la `SelectCategoryEnumPost` método.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample7.cs)]

Si se pasa un `eMovieCategories` enum para el `POST` método, podemos extraer el valor de enumeración y la cadena de la enumeración. Ejecutar el ejemplo y navegue hasta la página de prueba. Haga clic en el `Select Movie Category(Enum Post)` vínculo. Seleccione un tipo de película y, a continuación, pulse el botón de envío. La pantalla muestra el valor y el nombre del tipo de película.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image7.png)

### <a name="creating-a-multiple-section-select-element"></a>Crear un elemento de sección selecciones múltiples

El [ListBox](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.selectextensions.listbox.aspx) aplicación auxiliar HTML representa el código HTML `<select>` elemento con la `multiple` atributo, que permite a los usuarios hacer selecciones múltiples. Navegue hasta el vínculo de prueba, a continuación, seleccione la **múltiples Seleccionar país** vínculo. La interfaz de usuario representado permite seleccionar varios países. En la imagen siguiente, se seleccionan Canadá y China.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image8.png)

### <a name="examining-the-multiselectcountry-code"></a>Examinar el código de MultiSelectCountry

Examine el código siguiente desde la *controllers\homecontroller* archivo.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample8.cs)]

El `GetCountries` método crea una lista de países, y lo pasa a la `MultiSelectList` constructor. El `MultiSelectList` sobrecarga de constructor utilizada en el `GetCountries` método anterior toma cuatro parámetros:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample9.cs)]

1. *elementos*: un [IEnumerable](https://msdn.microsoft.com/en-us/library/system.collections.ienumerable.aspx) que contiene los elementos de la lista. En el ejemplo anterior, la lista de países.
2. *dataValueField*: el nombre de la propiedad en el **IEnumerable** lista que contiene el valor. En el ejemplo anterior, el `ID` propiedad.
3. *dataTextField*: el nombre de la propiedad en el **IEnumerable** lista que contiene la información que se va a mostrar. En el ejemplo anterior, el `name` propiedad.
4. *selectedValues*: la lista de valores seleccionados.

En el ejemplo anterior, el `MultiSelectCountry` método pasa una `null` valor para los países seleccionados, por lo que no países estarán seleccionadas cuando se muestra la interfaz de usuario. El código siguiente muestra el marcado de Razor que se usa para representar el `MultiSelectCountry` vista.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample10.cshtml)]

La aplicación auxiliar HTML [ListBox](https://msdn.microsoft.com/en-us/library/dd470200.aspx) método empleado anteriormente toman dos parámetros, el nombre de la propiedad de enlace de modelo y la [MultiSelectList](https://msdn.microsoft.com/en-us/library/system.web.mvc.multiselectlist.aspx) que contiene los valores y seleccione opciones. La `ViewBag.YouSelected` código anterior se utiliza para mostrar los valores de los países que seleccionó al enviar el formulario. Examine la sobrecarga de HTTP POST de la `MultiSelectCountry` método.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample11.cs)]

El `ViewBag.YouSelected` propiedad dinámica que contiene los países seleccionados, obtenidos para el `Countries` entrada de la colección de formulario. En esta versión, el método GetCountries se pasa una lista de los países seleccionados, por lo que cuando el `MultiSelectCountry` se muestra, se seleccionan los países seleccionados en la interfaz de usuario.

### <a name="making-a-select-element-friendly-with-the-harvest-chosen-jquery-plugin"></a>Realizar una seleccione elemento descriptivo con el complemento de jQuery cosecha elegido

La cosecha [elegido](http://harvesthq.github.com/chosen/) jQuery complemento se puede agregar a un elemento HTML &lt;seleccione&gt; elemento que se va a crear un usuario de interfaz de usuario descriptivo. Las siguientes imágenes muestran la cosecha [elegido](http://harvesthq.github.com/chosen/) complemento jQuery con `MultiSelectCountry` vista.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image9.png)

En las dos imágenes siguientes, **Canadá** está seleccionada.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image10.png)

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image11.png)

En la imagen anterior, se selecciona Canadá, y contiene un **x** puede hacer clic para eliminar la selección. La imagen siguiente muestra Canadá, China, y Japón seleccionado.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image12.png)

### <a name="hooking-up-the-harvest-chosen-jquery-plugin"></a>Enlazar el complemento de jQuery cosecha elegido

En la siguiente sección es más fácil de seguir si tiene cierta experiencia con jQuery. Si no ha usado nunca jQuery antes, puede intentar uno de los siguientes tutoriales de jQuery.

- [Funcionamiento de jQuery](http://docs.jquery.com/Tutorials:How_jQuery_Works) por [John Resig](http://ejohn.org/)
- [Introducción a jQuery](http://docs.jquery.com/Tutorials:Getting_Started_with_jQuery) por [Jörn Zaefferer](http://bassistance.de/)
- [Ejemplos de jQuery de Live](http://codylindley.com/blogstuff/js/jquery/#) por [Cody Lindley](http://codylindley.com/)

El complemento elegido se incluye en el inicio y proyectos de ejemplo completo que contiene este tutorial. Para este tutorial solo debe usar jQuery para conectarlo a la interfaz de usuario. Para usar el complemento de jQuery cosecha elegido en un proyecto de MVC de ASP.NET, debe:

1. Descargar el complemento seleccionado desde [github](https://github.com/harvesthq/chosen/). Este paso se ha realizado automáticamente.
2. Agregue la carpeta elegida para el proyecto de MVC de ASP.NET. Agregue los activos desde el complemento elegido que descargó en el paso anterior a la carpeta elegida. Este paso se ha realizado automáticamente.
3. Enlazar el complemento elegido para la **DropDownList** o **ListBox** aplicación auxiliar HTML.

### <a name="hooking-up-the-chosen-plugin-to-the-multiselectcountry-view"></a>Enlazar el complemento elegido a la vista de MultiSelectCountry.

Abra la *Views\Home\MultiSelectCountry.cshtml* de archivos y agregar un `htmlAttributes` parámetro para el `Html.ListBox`. El parámetro que se va a agregar contiene un nombre de clase para la lista de selección (`@class = "chzn-select"`). El código completo se muestra a continuación:

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample12.cshtml)]

En el código anterior, vamos a agregar el atributo HTML y el valor del atributo `class = "chzn-select"`. El carácter @ delante de la clase tiene nada que ver con el motor de vista Razor. `class`es un [palabra clave de C#](https://msdn.microsoft.com/en-us/library/x53a06bb.aspx). Palabras clave de C# no se pueden utilizar como identificadores a menos que incluyan como prefijo. En el ejemplo anterior, `@class` es un identificador válido pero **clase** no es porque **clase** es una palabra clave.

Agregue referencias a los *Chosen/chosen.jquery.js* y *Chosen/chosen.css* archivos. El *Chosen/chosen.jquery.js* e implementa la funcionalidad del complemento seleccionado. El *Chosen/chosen.css* archivo proporciona el estilo. Agregue estas referencias a la parte inferior de la *Views\Home\MultiSelectCountry.cshtml* archivo. El código siguiente muestra cómo hacer referencia el complemento elegido.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample13.cshtml)]

Activar el complemento elegido con el nombre de clase usado en el **Html.ListBox** código. En el ejemplo anterior, el nombre de clase es `chzn-select`. Agregue la siguiente línea a la parte inferior de la *Views\Home\MultiSelectCountry.cshtml* ver el archivo. Esta línea activa el complemento elegido.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample14.html)]

La siguiente línea es la sintaxis para llamar a la función lista de jQuery, que selecciona el elemento DOM con nombre de la clase `chzn-select`.

[!code-powershell[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample15.ps1)]

El conjunto devuelto por la llamada anterior, a continuación, ajustada aplica el método elegido (`.chosen();`), que enlaza el complemento elegido.

El código siguiente muestra el completado *Views\Home\MultiSelectCountry.cshtml* ver el archivo.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample16.cshtml)]

Ejecute la aplicación y navegue hasta la `MultiSelectCountry` vista. Intente agregar y eliminar países. El ejemplo de la descarga proporciona también contiene un `MultiCountryVM` método y la vista que implementa la funcionalidad de MultiSelectCountry mediante una vista de modelo en lugar de un **ViewBag**.

En la siguiente sección, verá cómo funciona el mecanismo de scaffolding de ASP.NET MVC con el **DropDownList** auxiliar.

>[!div class="step-by-step"]
[Siguiente](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
