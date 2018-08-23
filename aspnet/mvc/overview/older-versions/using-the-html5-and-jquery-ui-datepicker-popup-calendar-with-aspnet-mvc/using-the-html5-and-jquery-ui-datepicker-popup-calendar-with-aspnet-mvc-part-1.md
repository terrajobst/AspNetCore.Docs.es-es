---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
title: Uso de HTML5 y calendario emergente de la interfaz de usuario Datepicker de jQuery con ASP.NET MVC - parte 1 | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de cómo trabajar con plantillas de editor, las plantillas de presentación y el calendario emergente de jQuery UI datepicker en una máquina virtual de ASP.NET...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: c23d27f7-b0cf-44f2-8445-fb69e045c674
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
msc.type: authoredcontent
ms.openlocfilehash: c98c77d5e9e965fb82efbe6ed88700c89bc67b4c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835906"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-1"></a>Uso de HTML5 y calendario emergente de la interfaz de usuario Datepicker de jQuery con ASP.NET MVC - parte 1
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial le enseñará los aspectos básicos de cómo trabajar con plantillas de editor, las plantillas de presentación y el calendario emergente de jQuery UI datepicker en una aplicación Web de ASP.NET MVC.


Este tutorial le enseñará los aspectos básicos de cómo trabajar con plantillas de editor, las plantillas de presentación y jQuery [calendario emergente de la interfaz de usuario datepicker](http://plugins.jquery.com/project/datepicker) en una aplicación Web de ASP.NET MVC. Para este tutorial, puede utilizar Microsoft Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer&quot;), que es una versión gratuita de Microsoft Visual Studio, o puede usar Visual Studio 2010 SP1 si ya la tiene.

Antes de empezar, asegúrese de que ha instalado los requisitos previos descritos a continuación. Puede instalar todos ellos haciendo clic en el siguiente vínculo: [instalador de plataforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar el software necesario mediante los vínculos siguientes individualmente:

- [Requisitos previos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tiempo de ejecución de herramientas de soporte técnico +)

Si usa Visual Studio 2010, en lugar de Visual Web Developer, instale los requisitos previos, haga clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

En este tutorial se supone que ha completado la [Introducción a MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) tutorial o que está familiarizado con el desarrollo de ASP.NET MVC. Este tutorial comienza con el proyecto completado de la [Introducción a MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) tutorial.

En este tutorial se muestra código en C#. Sin embargo, el [proyecto inicial](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800) y proyecto completado también están disponibles en Visual Basic.

Un proyecto de Visual Studio con código fuente de C# y Visual Basic está disponible como acompañamiento de este tema: [descargar](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800).

### <a name="what-youll-build"></a>¿Qué va a crear

Deberá agregar plantillas (en concreto, editar y mostrar las plantillas) a la aplicación simple de la lista de películas que se creó en el [Introducción a MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) tutorial. También agregará un [datepicker de jQuery UI](http://jqueryui.com/demos/datepicker/) calendario emergente para simplificar el proceso de especificación de fechas. Captura de pantalla siguiente muestra la aplicación modificada con el calendario emergente de jQuery UI datepicker muestra.

![termine de jQuery](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image1.png)

### <a name="skills-youll-learn"></a>Habilidades que aprenderá

Aquí es lo que aprenderá:

- Cómo usar los atributos de la [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) espacio de nombres para controlar el formato de datos cuando se muestre y cuando está en modo de edición.
- Creación de plantillas (Editar y mostrar las plantillas) para controlar el formato de datos.
- Cómo agregar el [datepicker de jQuery UI](http://jqueryui.com/demos/datepicker/) como una manera de especificar los campos de fecha.

### <a name="getting-started"></a>Introducción

Si no dispone de la aplicación de la lista de películas desde el proyecto de inicio, descargarlo mediante el siguiente vínculo: [descargar](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800). A continuación, en el Explorador de Windows, haga clic en el *MvcMovie.zip* de archivo y seleccione **propiedades**. En el **MvcMovie.zip propiedades** cuadro de diálogo, seleccione **desbloquear**. (Desbloqueo evita recibir una advertencia de seguridad que se produce cuando se intenta usar un *.zip* archivo que ha descargado desde el web.)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image2.png)

Haga clic en el *MvcMovie.zip* de archivo y seleccione **extraer todo** para descomprimir el archivo. En Visual Studio 2010 o en Visual Web Developer, abra el *MvcMovieCS\_TU.sln* archivo.

En **el Explorador de soluciones**, haga doble clic en el *Views\Shared\\_Layout.cshtml* para abrirlo. Cambiar el `H1` encabezado desde **MVC Movie App** a **película jQuery**. Presione CTRL+F5 para ejecutar la aplicación y haga clic en el **inicio** ficha, que le llevará a la `Index` método del controlador de películas. Para probar la aplicación, seleccione la **editar** vínculo y el **detalles** vínculo para una de las películas. Tenga en cuenta que en el índice, editar, y las vistas de detalles, la fecha de publicación y precio bien tienen el formato:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image3.png)

El formato de la fecha y el precio es el resultado del uso de la [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atributo en las propiedades de la `Movie` clase.

Abra el *Movie.cs* de archivos y marque como comentario el `DisplayFormat` atributo el `ReleaseDate` y `Price` propiedades. Resultante `Movie` clase tiene este aspecto:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample1.cs)]

Presione CTRL+F5 para ejecutar la aplicación y seleccione el **inicio** pestaña para ver la lista de películas. Esta vez la fecha de lanzamiento muestra la fecha y hora, y el campo de precio ya no muestra el símbolo de moneda. El cambio en el `Movie` clase deshacer el formato más bonito que vio anteriormente, pero se corregirá en un momento.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image4.png)

### <a name="using-the-dataannotations-datatype-attribute-to-specify-the-data-type"></a>Usar el atributo DataAnnotations DataType para especificar el tipo de datos

Reemplace la comentada `DisplayFormat` atributo para el `ReleaseDate` propiedad con el [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atributo, utilizando el `Date` enumeración. Reemplace el `DisplayFormat` atributo para el `Price` propiedad con el [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atributo nuevo, esta vez mediante el `Currency` enumeración. Este es el aspecto que tiene el código completo:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample2.cs)]

Ejecute la aplicación. Ahora la fecha de lanzamiento y las propiedades de precio se formatean correctamente (es decir, con los formatos de fecha y moneda adecuados). El [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atributo proporciona metadatos de tipo para el MVC de ASP.NET integrados plantillas para que los campos que se representan en el formato correcto. Mediante el `DataType` atributo es preferible al uso la `DisplayFormat` atributo que se encontraba originalmente en el código, porque el `DataType` atributo hace que el modelo más limpio y más flexible para propósitos como la internacionalización.

En la sección siguiente verá cómo hacer que las plantillas personalizadas para mostrar los campos de fecha.

> [!div class="step-by-step"]
> [Siguiente](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
