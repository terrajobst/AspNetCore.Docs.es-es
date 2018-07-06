---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
title: Agregar una vista (C#) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es...
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: abc7c78d-cb09-4a4c-a887-61bc401d40e3
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 4a4eacfdcb0b53da377e9b6812a7ce50aa94b551
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831711"
---
<a name="adding-a-view-c"></a>Agregar una vista (C#)
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Hay disponible una versión actualizada de este tutorial [aquí](../../../getting-started/introduction/getting-started.md) que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro y mucho más fácil de seguir y se muestran las características más.
> 
> 
> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio. Antes de empezar, asegúrese de que ha instalado los requisitos previos descritos a continuación. Puede instalar todos ellos haciendo clic en el siguiente vínculo: [instalador de plataforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar individualmente los requisitos previos mediante los vínculos siguientes:
> 
> - [Requisitos previos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tiempo de ejecución de herramientas de soporte técnico +)
> 
> Si usa Visual Studio 2010, en lugar de Visual Web Developer 2010, instale los requisitos previos, haga clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un proyecto de Visual Web Developer con código fuente de C# está disponible como acompañamiento de este tema. [Descargue la versión de C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si prefiere Visual Basic, cambie a la [versión de Visual Basic](../vb/intro-to-aspnet-mvc-3.md) de este tutorial.


En esta sección va a modificar el `HelloWorldController` clase para usar la vista archivos de plantilla correctamente encapsulan el proceso de generar respuestas HTML a un cliente.

Se creará un archivo de plantilla de vista con el nuevo [motor de vistas Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) introducidos con ASP.NET MVC 3. Las plantillas de vista Razor tienen una *.cshtml* la extensión de archivo y proporcionan una manera elegante para crear el código HTML de salida con C#. Minimiza el número de caracteres y pulsaciones de teclas necesarias cuando se escribe una plantilla de vista Razor y permite un rápido, fluido flujo de trabajo de codificación.

Iniciar con una plantilla de vista con el `Index` método en el `HelloWorldController` clase. Actualmente, el método `Index` devuelve una cadena con un mensaje que está codificado de forma rígida en la clase de controlador. Cambiar el `Index` método devuelva un `View` de objeto, como se muestra en la siguiente:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

Este código usa una plantilla de vista para generar una respuesta HTML al explorador. En el proyecto, agregue una plantilla de vista que puede usar con el `Index` método. Para ello, haga clic en el `Index` método y haga clic en **agregar vista**.

![](adding-a-view/_static/image1.png)

El **agregar vista** aparece el cuadro de diálogo. Deje los valores predeterminados de la manera son y haga clic en el **agregar** botón:

![](adding-a-view/_static/image2.png)

El *MvcMovie\Views\HelloWorld* carpeta y el *MvcMovie\Views\HelloWorld\Index.cshtml* se crean archivos. Puede verlos en **el Explorador de soluciones**:

![](adding-a-view/_static/image3.png)

La siguiente muestra la *Index.cshtml* archivo creado:

[![HelloWorldIndex](adding-a-view/_static/image5.png)](adding-a-view/_static/image4.png)

Agregar algún HTML bajo el `<h2>` etiqueta. Modificado *MvcMovie\Views\HelloWorld\Index.cshtml* archivo se muestra a continuación.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml)]

Ejecute la aplicación y vaya a la `HelloWorld` controlador (`http://localhost:xxxx/HelloWorld`). El `Index` método en el controlador no hacen mucho trabajo; simplemente ejecutó la instrucción `return View()`, que especifica que el método debe utilizar un archivo de plantilla de vista para presentar una respuesta al explorador. Dado que no especifica explícitamente el nombre del archivo de plantilla de vista para usar, ASP.NET MVC usa de forma predeterminada el *Index.cshtml* los archivos de vista el *\Views\HelloWorld* carpeta. La siguiente imagen muestra la cadena codificada en la vista.

![](adding-a-view/_static/image6.png)

Parece bastante bueno. Sin embargo, tenga en cuenta que la barra del explorador título dice "Index" y el título en la página big dice "Mi aplicación MVC." Vamos a cambiar aquellos.

## <a name="changing-views-and-layout-pages"></a>Cambiar vistas y páginas de diseño

En primer lugar, desea cambiar el título "Mi aplicación de MVC" en la parte superior de la página. Es común a todas las páginas que el texto. En realidad se implementa en un único lugar en el proyecto, aunque aparezca en todas las páginas en la aplicación. Vaya a la */Views/Shared* carpeta **el Explorador de soluciones** y abra el  *\_Layout.cshtml* archivo. Este archivo se denomina un *página de diseño* y es el shell"compartido" que usan todas las demás páginas.

[![_LayoutCshtml](adding-a-view/_static/image8.png)](adding-a-view/_static/image7.png)

Las plantillas de diseño permiten especificar el diseño del contenedor HTML del sitio en un solo lugar y, a continuación, aplicarla en varias páginas del sitio. Tenga en cuenta la `@RenderBody()` línea cerca de la parte inferior del archivo. `RenderBody` es un marcador de posición donde todas las páginas de vista específica que cree se muestran, "encapsuladas" en la página de diseño. Cambie el encabezado de título de la plantilla de diseño de "Mi aplicación de MVC" a "MVC Movie App".

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml)]

Ejecute la aplicación y observe que ahora dice "MVC Movie App". Haga clic en el **sobre** vínculo y ver cómo esa página muestra "MVC Movie App", demasiado. Fuimos capaces de realizar el cambio una vez en la plantilla de diseño y dispone de todas las páginas del sitio refleja el nuevo título.

![](adding-a-view/_static/image9.png)

La completa  *\_Layout.cshtml* archivo se muestra a continuación:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Ahora, vamos a cambiar el título de la página de índice (vista).

Abra *MvcMovie\Views\HelloWorld\Index.cshtml*. Hay dos lugares para realizar un cambio: en primer lugar, el texto que aparece en el título del explorador y, a continuación, en el encabezado secundario (el `<h2>` elemento). Haremos que sean algo diferentes para que pueda ver qué parte del código cambia cada área de la aplicación.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

Para indicar el título HTML para mostrar el código anterior establece un `Title` propiedad de la `ViewBag` objeto (que se encuentra en la *Index.cshtml* plantilla de vista). Si repasa el código fuente de la plantilla de diseño, observará que la plantilla utiliza este valor en el `<title>` elemento como parte de la `<head>` sección del archivo HTML. Con este enfoque, puede pasar fácilmente otros parámetros entre la plantilla de vista y el archivo de diseño.

Ejecute la aplicación y vaya a `http://localhost:xx/HelloWorld`. Tenga en cuenta que el título del explorador, el encabezado principal y los encabezados secundarios han cambiado. (Si no ve los cambios en el explorador, es posible que esté viendo contenido almacenado en caché. Presione Ctrl+F5 en el explorador para forzar que se cargue la respuesta del servidor).

Observe también cómo el contenido en el *Index.cshtml* plantilla de vista se combinó con el  *\_Layout.cshtml* se envió la plantilla de vista y una única respuesta HTML al explorador. Con las plantillas de diseño es realmente fácil hacer cambios para que se apliquen en todas las páginas de la aplicación.

![](adding-a-view/_static/image10.png)

Nuestra pequeña cantidad de "datos", en este caso, el mensaje "Hello from our View Template!" (Hola desde nuestra plantilla de vista), están codificados de forma rígida. La aplicación de MVC tiene una "V" (vista) y ha obtenido una "C" (controlador), pero todavía no tiene una "M" (modelo). En breve, le guiaremos a través de cómo crear una base de datos y recuperar los datos del modelo del mismo.

## <a name="passing-data-from-the-controller-to-the-view"></a>Pasar datos del controlador a la vista

Antes de hablar acerca de los modelos y vaya a una base de datos, sin embargo, en primer lugar hablemos acerca de cómo pasar información desde el controlador a una vista. Las clases de controlador se invocan en respuesta a una solicitud de dirección URL entrante. Una clase de controlador es donde se escribe el código que controla los parámetros de entrada, recupera datos de una base de datos y, en última instancia decida qué tipo de respuesta para enviar al explorador. Plantillas de vista, a continuación, pueden utilizarse desde un controlador para generar y dar formato a una respuesta HTML al explorador.

Los controladores son responsables de proporcionar los datos u objetos son necesarios para una plantilla de vista presentar una respuesta al explorador. Una plantilla de vista nunca debe realizar la lógica de negocios ni interactuar directamente con una base de datos. En su lugar, debería funcionar sólo con los datos que se proporcionan el controlador. Mantener esta "separación de intereses" ayuda a mantener su código limpio y más fácil de mantener.

Actualmente, el `Welcome` método de acción en el `HelloWorldController` clase toma un `name` y un `numTimes` parámetro y, a continuación, obtiene los valores directamente en el explorador. En lugar de que el controlador represente esta respuesta como una cadena, vamos a cambiar el controlador para que use una plantilla de vista en su lugar. La plantilla de vista generará una respuesta dinámica, lo que significa que debe pasar las partes de datos adecuadas desde el controlador a la vista para que se genere la respuesta. Para ello, indique al controlador que coloque los datos dinámicos que necesita la plantilla de vista en un `ViewBag` objeto que puede tener acceso la plantilla de vista.

Vuelva a la *HelloWorldController.cs* y cambie el `Welcome` método para agregar un `Message` y `NumTimes` valor para el `ViewBag` objeto. `ViewBag` es un objeto dinámico, lo que significa que puede colocar todo lo desee en él; la `ViewBag` objeto no tiene ninguna propiedad definida hasta que coloca algo dentro de él. El archivo *HelloWorldController.cs* completo tiene este aspecto:

[!code-csharp[Main](adding-a-view/samples/sample6.cs)]

Ahora el `ViewBag` objeto contiene los datos que se pasará automáticamente a la vista.

A continuación, necesita una plantilla de vista principal. En el **depurar** menú, seleccione **MvcMovie compilación** para asegurarse de que se compila el proyecto.

[![BuildHelloWorld](adding-a-view/_static/image12.png)](adding-a-view/_static/image11.png)

A continuación, haga clic en el `Welcome` método y haga clic en **agregar vista**. Esto es lo que el **agregar vista** cuadro de diálogo es similar a:

![](adding-a-view/_static/image13.png)

Haga clic en **agregar**y, a continuación, agregue el siguiente código bajo el `<h2>` elemento en el nuevo *Welcome.cshtml* archivo. Creará un bucle que dice "Hello" tantas veces como el usuario indica que debería. La completa *Welcome.cshtml* archivo se muestra a continuación.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml)]

Ejecute la aplicación y vaya a la dirección URL siguiente:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Ahora los datos se toman de la dirección URL y pasa automáticamente al controlador. El controlador empaqueta los datos en un `ViewBag` objeto y pasa ese objeto a la vista. La vista, a continuación, muestra los datos como HTML al usuario.

![](adding-a-view/_static/image14.png)

Bueno, todo esto era un tipo de "M" para el modelo, pero no el tipo de base de datos. Vamos a aprovechar lo que hemos aprendido para crear una base de datos de películas.

> [!div class="step-by-step"]
> [Anterior](adding-a-controller.md)
> [Siguiente](adding-a-model.md)
