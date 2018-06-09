---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
title: Agregar una vista (C#) | Documentos de Microsoft
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: abc7c78d-cb09-4a4c-a887-61bc401d40e3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 50ce4a2024ffd9e2bbb5526717052d486689ff38
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2018
ms.locfileid: "30873175"
---
<a name="adding-a-view-c"></a>Agregar una vista (C#)
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

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


En esta sección va a modificar el `HelloWorldController` clase para usar la vista archivos de plantilla a correctamente encapsulan el proceso de generar las respuestas HTML a un cliente.

Se creará un archivo de plantilla de la vista con el nuevo [motor de vista Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) introducidos con ASP.NET MVC 3. Plantillas de vista Razor tienen un *.cshtml* la extensión de archivo y proporcionan una forma elegante para crear HTML con C# de salida. Razor minimiza el número de caracteres y pulsaciones de teclas necesarias al escribir una plantilla de vista y permite un fluido rápido, flujo de trabajo de codificación.

Iniciar con una plantilla de vista con el `Index` método en la `HelloWorldController` clase. Actualmente, el método `Index` devuelve una cadena con un mensaje que está codificado de forma rígida en la clase de controlador. Cambiar el `Index` método para devolver un `View` de objeto, como se muestra en la siguiente:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

Este código usa una plantilla de vista para generar una respuesta HTML al explorador. En el proyecto, agregue una plantilla de vista que puede usar con el `Index` método. Para ello, pulse el botón derecho dentro de la `Index` método y haga clic en **agregar vista**.

![](adding-a-view/_static/image1.png)

El **agregar vista** aparece el cuadro de diálogo. Deje los valores predeterminados de la manera son y haga clic en el **agregar** botón:

![](adding-a-view/_static/image2.png)

El *MvcMovie\Views\HelloWorld* carpeta y el *MvcMovie\Views\HelloWorld\Index.cshtml* se crea el archivo. Podrá verlas en **el Explorador de soluciones**:

![](adding-a-view/_static/image3.png)

La siguiente muestra la *Index.cshtml* archivo que se creó:

[![HelloWorldIndex](adding-a-view/_static/image5.png)](adding-a-view/_static/image4.png)

Agregar algún HTML en la `<h2>` etiqueta. Modificados *MvcMovie\Views\HelloWorld\Index.cshtml* archivo se muestra a continuación.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml)]

Ejecute la aplicación y busque el `HelloWorld` controlador (`http://localhost:xxxx/HelloWorld`). El `Index` método en el controlador no hacen mucho trabajo; simplemente se ejecutó la instrucción `return View()`, que especifica que el método debe utilizar un archivo de plantilla de la vista para presentar una respuesta al explorador. Dado que no especifica explícitamente el nombre del archivo de plantilla de vista para usar, ASP.NET MVC usado de forma predeterminada el *Index.cshtml* archivo de vista en el *\Views\HelloWorld* carpeta. La imagen siguiente muestra la cadena codificada de forma rígida en la vista.

![](adding-a-view/_static/image6.png)

Parece bastante bueno. Sin embargo, tenga en cuenta que barra de título del explorador dice "Index" y el título grande en la página dice "Mi aplicación MVC". Cambiemos los.

## <a name="changing-views-and-layout-pages"></a>Cambiar vistas y páginas de diseño

En primer lugar, desea cambiar el título "Mi aplicación de MVC" en la parte superior de la página. Que el texto es común a todas las páginas. En realidad se implementa en un único lugar en el proyecto, aunque aparezca en todas las páginas en la aplicación. Vaya a la */vistas/Shared* carpeta **el Explorador de soluciones** y abra el  *\_Layout.cshtml* archivo. Este archivo se denomina un *página de diseño* y es el shell"compartido" que utilizan todas las demás páginas.

[![_LayoutCshtml](adding-a-view/_static/image8.png)](adding-a-view/_static/image7.png)

Las plantillas de diseño permiten especificar el diseño de contenedor HTML del sitio en un solo lugar y, a continuación, se aplican a través de varias páginas en el sitio. Tenga en cuenta la `@RenderBody()` línea en la parte inferior del archivo. `RenderBody` es un marcador de posición donde todas las páginas de vista específicos que creas mostrarán, "ajustadas" en la página de diseño. Cambie el encabezado de título en la plantilla de diseño de "Mi aplicación de MVC" a "Aplicación de película de MVC".

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml)]

Ejecute la aplicación y observe que ahora dice "Aplicación de película de MVC". Haga clic en el **sobre** vínculo y verá cómo esa página muestra "MVC de aplicación de película," demasiado. Hemos podidos realizar el cambio una vez en la plantilla de diseño y ha todas las páginas en el sitio de reflejar el nuevo título.

![](adding-a-view/_static/image9.png)

La sección completa  *\_Layout.cshtml* archivo se muestra a continuación:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Ahora, vamos a cambiar el título de la página de índice (vista).

Abra *MvcMovie\Views\HelloWorld\Index.cshtml*. Hay dos lugares para realizar un cambio: en primer lugar, el texto que aparece en el título del explorador y, a continuación, en el encabezado secundario (el `<h2>` elemento). Haremos que sean algo diferentes para que pueda ver qué parte del código cambia cada área de la aplicación.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

Para indicar el título HTML para mostrar, el código anterior establece una `Title` propiedad de la `ViewBag` objeto (que está en el *Index.cshtml* plantilla de vista). Si volver atrás en el código fuente de la plantilla de diseño, observará que la plantilla utiliza este valor en el `<title>` elemento como parte de la `<head>` sección del HTML. Con este enfoque, puede pasar otros parámetros fácilmente entre la plantilla de vista y el archivo de diseño.

Ejecute la aplicación y vaya a `http://localhost:xx/HelloWorld`. Tenga en cuenta que el título del explorador, el encabezado principal y los encabezados secundarios han cambiado. (Si no ve los cambios en el explorador, es posible que esté viendo contenido almacenado en caché. Presione Ctrl+F5 en el explorador para forzar que se cargue la respuesta del servidor).

Observe también cómo el contenido en el *Index.cshtml* plantilla de vista se combinó con el  *\_Layout.cshtml* plantilla de vista y una única respuesta HTML se envía al explorador. Con las plantillas de diseño es realmente fácil hacer cambios para que se apliquen en todas las páginas de la aplicación.

![](adding-a-view/_static/image10.png)

Nuestra pequeña cantidad de "datos", en este caso, el mensaje "Hello from our View Template!" (Hola desde nuestra plantilla de vista), están codificados de forma rígida. La aplicación de MVC tiene una "V" (vista) y ha obtenido una "C" (controlador), pero todavía no tiene una "M" (modelo). En breve, le mostraremos cómo crear una base de datos y recuperar los datos del modelo del mismo.

## <a name="passing-data-from-the-controller-to-the-view"></a>Pasar datos del controlador a la vista

Antes de que se vaya a una base de datos y hablar acerca de los modelos, sin embargo, en primer lugar hablemos acerca de cómo pasar información desde el controlador a una vista. Las clases de controlador se invocan en respuesta a una solicitud de dirección URL entrante. Una clase de controlador es donde se escribe el código que controla los parámetros de entrada, recupera datos de una base de datos y, en última instancia decida qué tipo de respuesta para enviar al explorador. Plantillas de vista, a continuación, pueden utilizarse desde un controlador para generar y dar formato a una respuesta HTML al explorador.

Los controladores son responsables de proporcionar los datos u objetos necesarios para una plantilla de vista presentar una respuesta al explorador. Una plantilla de vista nunca debe realizar la lógica de negocios o interactuar directamente con una base de datos. En su lugar, debe funcionar solo con los datos que están indicados por el controlador. Mantener esta "separación de aspectos" le ayuda a mantener el código limpio y más fácil de mantener.

Actualmente, el `Welcome` método de acción en el `HelloWorldController` clase toma un `name` y un `numTimes` parámetro y, a continuación, los valores directamente en el Explorador de salidas. En lugar de tener el controlador de representar esta respuesta como una cadena, vamos a cambiar el controlador para usar una plantilla de vista en su lugar. La plantilla de vista generará una respuesta dinámica, lo que significa que debe pasar las partes de datos adecuadas desde el controlador a la vista para que se genere la respuesta. Puede hacerlo haciendo que el controlador de colocar los datos dinámicos que necesita la plantilla de vista en un `ViewBag` objeto que puede tener acceso la plantilla de vista.

Vuelva a la *HelloWorldController.cs* y cambie el `Welcome` método para agregar un `Message` y `NumTimes` valor para el `ViewBag` objeto. `ViewBag` es un objeto dinámico, lo que significa que puede colocar todo lo que desees la `ViewBag` objeto no tiene ninguna propiedad definida hasta que escriba algo dentro de él. El archivo *HelloWorldController.cs* completo tiene este aspecto:

[!code-csharp[Main](adding-a-view/samples/sample6.cs)]

Ahora el `ViewBag` objeto contiene los datos que se pasará automáticamente a la vista.

A continuación, hay una plantilla de vista de página principal. En el **depurar** menú, seleccione **MvcMovie generar** para asegurarse de que se compila el proyecto.

[![BuildHelloWorld](adding-a-view/_static/image12.png)](adding-a-view/_static/image11.png)

A continuación, haga clic en dentro de la `Welcome` método y haga clic en **agregar vista**. Esto es lo que la **agregar vista** cuadro de diálogo es similar:

![](adding-a-view/_static/image13.png)

Haga clic en **agregar**y, a continuación, agregue el código siguiente bajo la `<h2>` elemento en el nuevo *Welcome.cshtml* archivo. Se creará un bucle que dice "Hello" tantas veces como el usuario indica que debería. La sección completa *Welcome.cshtml* archivo se muestra a continuación.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml)]

Ejecute la aplicación y vaya a la dirección URL siguiente:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Ahora los datos se toma de la dirección URL y pasa automáticamente al controlador de. El controlador empaqueta los datos en un `ViewBag` objeto y pasa ese objeto a la vista. La vista, a continuación, muestra los datos como HTML al usuario.

![](adding-a-view/_static/image14.png)

Bueno, todo esto era un tipo de "M" para el modelo, pero no el tipo de base de datos. Vamos a aprovechar lo que hemos aprendido para crear una base de datos de películas.

> [!div class="step-by-step"]
> [Anterior](adding-a-controller.md)
> [Siguiente](adding-a-model.md)
