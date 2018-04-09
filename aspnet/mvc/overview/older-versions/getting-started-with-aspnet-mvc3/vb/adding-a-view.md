---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
title: Agregar una vista (VB) | Documentos de Microsoft
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: d3633f64-5d3c-45c9-ae4b-cb1563e3739f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: c9675eb7776116ecbe910d5515abfe9b4391df22
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-view-vb"></a>Agregar una vista (VB)
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio. Antes de empezar, asegúrese de que ha instalado los requisitos previos descritos a continuación. Puede instalar todas ellas haciendo clic en el siguiente vínculo: [instalador de plataforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar por separado los requisitos previos mediante los siguientes vínculos:
> 
> - [Requisitos previos visuales Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(se admiten en tiempo de ejecución + herramientas)
> 
> Si usa Visual Studio 2010 en lugar de Visual Web Developer 2010, instale los requisitos previos, haga clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un proyecto de Visual Web Developer con el código fuente VB.NET está disponible como acompañamiento de este tema. [Descargue la versión VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si lo prefiere C#, cambie a la [versión de C#](../cs/adding-a-view.md) de este tutorial.


En esta sección, vamos a modificar el `HelloWorldController` clase se debe utilizar un archivo de plantilla de la vista para correctamente encapsular el proceso de generar las respuestas HTML a un cliente.

Puede empezar con una plantilla de vista con el `Index` método en la `HelloWorldController` clase. Actualmente el `Index` método devuelve una cadena con un mensaje que está codificado de forma rígida dentro de la clase de controlador. Cambiar el `Index` método para devolver un `View` de objeto, como se muestra en la siguiente:

[!code-vb[Main](adding-a-view/samples/sample1.vb)]

Ahora agreguemos una plantilla de vista a nuestro proyecto que se puede invocar con el `Index` método. Para ello, pulse el botón derecho dentro de la `Index` método y haga clic en **agregar vista**.

[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)

El **agregar vista** aparece el cuadro de diálogo. Deje las entradas predeterminadas y haga clic en el **agregar** botón.

[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)

El *MvcMovie\Views\HelloWorld* carpeta y el *MvcMovie\Views\HelloWorld\Index.vbhtml* se crea el archivo. Podrá verlas en **el Explorador de soluciones**:

[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)

Agregar algún HTML en la `<h2>` etiqueta. Modificados *MvcMovie\Views\HelloWorld\Index.vbhtml* archivo se muestra a continuación.

[!code-vbhtml[Main](adding-a-view/samples/sample2.vbhtml)]

Ejecute la aplicación y busque el &quot;Hola a todos&quot; controlador (`http://localhost:xxxx/HelloWorld`). El `Index` método en el controlador no hacen mucho trabajo; simplemente se ejecutó la instrucción `return View()`, el cual indica que deseamos utilizar un archivo de plantilla de la vista para presentar una respuesta al cliente. Dado que no se ha especificado explícitamente el nombre del archivo de plantilla de vista para usar, ASP.NET MVC usado de forma predeterminada el *Index.vbhtml* Ver archivo dentro de la *\Views\HelloWorld* carpeta. La imagen siguiente muestra la cadena codificada de forma rígida en la vista.

[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)

Parece bastante bueno. Sin embargo, tenga en cuenta que la barra de título del explorador dice &quot;índice&quot; e indica el título grande en la página &quot;mi aplicación MVC.&quot; Cambiemos los.

## <a name="changing-views-and-layout-pages"></a>Cambiar vistas y páginas de diseño

En primer lugar, vamos a cambiar el texto &quot;mi aplicación MVC.&quot; Que el texto se comparte y aparece en cada página. Aparece realmente en un único lugar en el proyecto, aunque se encuentre en todas las páginas en nuestra aplicación. Vaya a la */vistas/Shared* carpeta **el Explorador de soluciones** y abra el  *\_Layout.vbhtml* archivo. Este archivo se denomina una página de diseño y es el recurso compartido &quot;shell&quot; que usan todas las demás páginas.

Tenga en cuenta la `@RenderBody()` línea de código en la parte inferior del archivo. `RenderBody` es un marcador de posición donde se muestran todas las páginas que cree, &quot;ajustado&quot; en la página de diseño. Cambiar el `<h1>` encabezado de **&quot;** mi aplicación MVC&quot; a &quot;aplicación de MVC película&quot;.

[!code-html[Main](adding-a-view/samples/sample3.html)]

Ejecute la aplicación y observe que ahora dice &quot;aplicación de MVC película&quot;. Haga clic en el **sobre** link y que se muestra en la página &quot;aplicación de MVC película&quot;, demasiado.

La sección completa  *\_Layout.vbhtml* archivo se muestra a continuación:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Ahora, vamos a cambiar el título de la página de índice (vista).

[!code-vbhtml[Main](adding-a-view/samples/sample5.vbhtml)]

Open *MvcMovie\Views\HelloWorld\Index.vbhtml*. Hay dos lugares para realizar un cambio: en primer lugar, el texto que aparece en el título del explorador y, a continuación, en el encabezado secundario (el `<h2>` elemento). Nos aseguraremos de hacer ellos ligeramente diferente para que pueda ver qué bit de código que cambia qué parte de la aplicación.

Ejecute la aplicación y vaya a`http://localhost:xx/HelloWorld`. Tenga en cuenta que el título del explorador, el encabezado principal y los encabezados secundarios han cambiado. Es fácil realizar grandes cambios en la aplicación con pequeños cambios en una vista. (Si no ve los cambios en el explorador, es posible que esté viendo contenido almacenado en caché. Presione Ctrl+F5 en el explorador para forzar que se cargue la respuesta del servidor).

[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)

Nuestro poco de &quot;datos&quot; (en este caso el &quot;Hello World!&quot; mensaje) está codificado de forma rígida, aunque. Nuestra aplicación de MVC tiene V (vistas) y tenemos C (controladores), pero aún no M (modelo). En breve, le mostraremos cómo crear una base de datos y recuperar los datos del modelo del mismo.

## <a name="passing-data-from-the-controller-to-the-view"></a>Pasar datos del controlador a la vista

Antes de que se vaya a una base de datos y hablar acerca de los modelos, sin embargo, en primer lugar hablemos acerca de cómo pasar información desde el controlador a una vista. Desea pasar a lo que requiere una plantilla de vista para representar una respuesta HTML a un cliente. Estos objetos se crean normalmente y se pasa una clase de controlador a una plantilla de vista, y deben contener únicamente los datos que requiere la plantilla de vista y no hay más.

Anteriormente con el `HelloWorldController` (clase), el `Welcome` tardó de método de acción un `name` y un `numTimes` parámetro y salida, a continuación, los valores de parámetro en el explorador. En su lugar de que el controlador continúe representar esta respuesta directamente, vamos a en su lugar deberá colocamos esos datos en un contenedor para la vista. Controladores y vistas pueden usar un `ViewBag` objeto para contener los datos. Que se se pasa automáticamente a una plantilla de vista y usa para representar la respuesta HTML con el contenido de la bolsa de como datos. De este modo el controlador está relacionado con una cosa y la plantilla de vista con otro, lo que nos permite mantener limpia &quot;separación de intereses&quot; dentro de la aplicación.

Como alternativa, se podríamos definir una clase personalizada, a continuación, crear una instancia de ese objeto en nuestro propio, rellenar con datos y pasar a la vista. Que se suele denominar un ViewModel, porque es un modelo para la vista personalizado. Para pequeñas cantidades de datos, sin embargo, el elemento ViewBag funciona bien.

Vuelva a la *HelloWorldController.vb* cambio en un archivo la `Welcome` método en el controlador para poner el mensaje y NumTimes en el elemento ViewBag. El elemento ViewBag es un objeto dinámico. Esto significa que puede colocar todo lo que desees a él. ViewBag no tiene ninguna propiedad definida hasta que escriba algo dentro de él.

La sección completa `HelloWorldController.vb` con la nueva clase en el mismo archivo.

[!code-vb[Main](adding-a-view/samples/sample6.vb)]

Nuestro ViewBag contiene ahora datos que se pasará a través a la vista automáticamente. De nuevo, o bien se podríamos transcurrieron en nuestro propio objeto similar al siguiente si le gustó:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

Ahora necesitamos un `WelcomeView` plantilla! Ejecute la aplicación, por lo que el código nuevo se compilará. Cierre el explorador, haga clic en dentro de la `Welcome` método y, a continuación, haga clic en **agregar vista**.

Esto es lo que su **agregar vista** cuadro de diálogo es similar.

[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)

Agregue el código siguiente bajo la `<h2>` elemento en el nuevo <em>bienvenida.</em> archivo VBHTML. Se podrá realizar un bucle y diga &quot;Hello&quot; tantas veces como el usuario indica que deberíamos!

[!code-vbhtml[Main](adding-a-view/samples/sample8.vbhtml)]

Ejecute la aplicación y vaya a `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Ahora los datos se toma de la dirección URL y pasa automáticamente al controlador de. El controlador empaqueta los datos en un `Model` objeto y pasa ese objeto a la vista. La vista que muestra los datos como HTML al usuario.

[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)

Bueno, que era un tipo de un &quot;M&quot; para el modelo, pero no el tipo de base de datos. Vamos a aprovechar lo que hemos aprendido para crear una base de datos de películas.

> [!div class="step-by-step"]
> [Anterior](adding-a-controller.md)
> [Siguiente](adding-a-model.md)
