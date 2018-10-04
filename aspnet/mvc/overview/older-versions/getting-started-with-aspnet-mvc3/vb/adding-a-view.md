---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
title: Agregar una vista (VB) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: d3633f64-5d3c-45c9-ae4b-cb1563e3739f
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 0d6311cac4fe01b2e21b300ff3841b3f2ca6fcf5
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577071"
---
<a name="adding-a-view-vb"></a>Agregar una vista (VB)
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
> Un proyecto de Visual Web Developer con código fuente VB.NET está disponible como acompañamiento de este tema. [Descargue la versión VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si lo prefiere C#, cambie a la [C# versión](../cs/adding-a-view.md) de este tutorial.


En esta sección, vamos a modificar el `HelloWorldController` clase para usar un archivo de plantilla de vista a claramente encapsulé el proceso de generar respuestas HTML a un cliente.

Comencemos con una plantilla de vista con el `Index` método en el `HelloWorldController` clase. Actualmente la `Index` método devuelve una cadena con un mensaje que está codificado dentro de la clase de controlador. Cambiar el `Index` método devuelva un `View` de objeto, como se muestra en la siguiente:

[!code-vb[Main](adding-a-view/samples/sample1.vb)]

Ahora agreguemos una plantilla de vista a nuestro proyecto que se puede invocar con la `Index` método. Para ello, haga clic en el `Index` método y haga clic en **agregar vista**.

[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)

El **agregar vista** aparece el cuadro de diálogo. Deje las entradas predeterminadas y haga clic en el **agregar** botón.

[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)

El *MvcMovie\Views\HelloWorld* carpeta y el *MvcMovie\Views\HelloWorld\Index.vbhtml* se crean archivos. Puede verlos en **el Explorador de soluciones**:

[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)

Agregar algún HTML bajo el `<h2>` etiqueta. Modificado *MvcMovie\Views\HelloWorld\Index.vbhtml* archivo se muestra a continuación.

[!code-vbhtml[Main](adding-a-view/samples/sample2.vbhtml)]

Ejecute la aplicación y vaya a la &quot;Hola mundo&quot; controlador (`http://localhost:xxxx/HelloWorld`). El `Index` método en el controlador no hacen mucho trabajo; simplemente ejecutó la instrucción `return View()`, el cual indica que deseamos utilizar un archivo de plantilla de vista para presentar una respuesta al cliente. Dado que no se ha especificado explícitamente el nombre del archivo de plantilla de vista para usar, ASP.NET MVC usa de forma predeterminada el *Index.vbhtml* Ver archivo dentro de la *\Views\HelloWorld* carpeta. La siguiente imagen muestra la cadena codificada en la vista.

[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)

Parece bastante bueno. Sin embargo, tenga en cuenta que la barra de título del explorador dice &quot;índice&quot; e indica el título en la página big &quot;mi aplicación MVC.&quot; Vamos a cambiar aquellos.

## <a name="changing-views-and-layout-pages"></a>Cambiar vistas y páginas de diseño

En primer lugar, vamos a cambiar el texto &quot;mi aplicación MVC.&quot; Que el texto se comparte y aparece en cada página. Aparece realmente en un único lugar en el proyecto, incluso aunque esté en cada página en nuestra aplicación. Vaya a la */Views/Shared* carpeta **el Explorador de soluciones** y abra el  *\_Layout.vbhtml* archivo. Este archivo se llama a una página de diseño y es el recurso compartido &quot;shell&quot; que usan todas las demás páginas.

Tenga en cuenta la `@RenderBody()` línea de código cerca de la parte inferior del archivo. `RenderBody` es un marcador de posición donde se muestran todas las páginas que crea, &quot;ajustado&quot; en la página de diseño. Cambiar el `<h1>` encabezado desde **&quot;** mi aplicación MVC&quot; a &quot;MVC Movie App&quot;.

[!code-html[Main](adding-a-view/samples/sample3.html)]

Ejecute la aplicación y tenga en cuenta ahora dice &quot;MVC Movie App&quot;. Haga clic en el **sobre** vínculo y que se muestra en la página &quot;MVC Movie App&quot;, demasiado.

La completa  *\_Layout.vbhtml* archivo se muestra a continuación:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Ahora, vamos a cambiar el título de la página de índice (vista).

[!code-vbhtml[Main](adding-a-view/samples/sample5.vbhtml)]

Abra *MvcMovie\Views\HelloWorld\Index.vbhtml*. Hay dos lugares para realizar un cambio: en primer lugar, el texto que aparece en el título del explorador y, a continuación, en el encabezado secundario (el `<h2>` elemento). Haremos ellos ligeramente diferente para que pueda ver qué fragmento de código cambia qué parte de la aplicación.

Ejecute la aplicación y vaya a`http://localhost:xx/HelloWorld`. Tenga en cuenta que el título del explorador, el encabezado principal y los encabezados secundarios han cambiado. Es fácil realizar grandes cambios en la aplicación con pequeños cambios en una vista. (Si no ve los cambios en el explorador, es posible que esté viendo contenido almacenado en caché. Presione Ctrl+F5 en el explorador para forzar que se cargue la respuesta del servidor).

[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)

Nuestra pequeña cantidad de &quot;datos&quot; (en este caso el &quot;Hello World!&quot; mensaje) está codificado de forma rígida, sin embargo. Nuestra aplicación de MVC tiene V (vistas) y tenemos C (controladores), pero aún no hay M (modelo). En breve, le guiaremos a través de cómo crear una base de datos y recuperar los datos del modelo del mismo.

## <a name="passing-data-from-the-controller-to-the-view"></a>Pasar datos del controlador a la vista

Antes de hablar acerca de los modelos y vaya a una base de datos, sin embargo, en primer lugar hablemos acerca de cómo pasar información desde el controlador a una vista. Van a pasar a lo que requiere una plantilla de vista para representar una respuesta HTML a un cliente. Estos objetos normalmente se crean y se pasa una clase de controlador a una plantilla de vista, y deben contener únicamente los datos que requiere la plantilla de vista y no más.

Anteriormente con el `HelloWorldController` (clase), el `Welcome` tardó el método de acción un `name` y un `numTimes` parámetro y salida, a continuación, los valores de parámetro en el explorador. En su lugar a que el controlador continúan generándose directamente esta respuesta, vamos a en su lugar, pondremos esos datos en un contenedor para la vista. Controladores y vistas que pueden usar un `ViewBag` objeto para contener los datos. Que se pasa automáticamente a una plantilla de vista y usa para representar la respuesta HTML con el contenido del contenedor como datos. De este modo el controlador se ocupa de una cosa y la plantilla de vista con otro, lo que nos permite mantener limpia &quot;separación de preocupaciones&quot; dentro de la aplicación.

Como alternativa, podríamos definir una clase personalizada, a continuación, cree una instancia de ese objeto en nuestro propio, rellenarlo con datos y pasarla a la vista. Que se suele denominar un ViewModel, porque es un modelo personalizado para la vista. Para pequeñas cantidades de datos, sin embargo, el elemento ViewBag funciona muy bien.

Vuelva a la *HelloWorldController.vb* cambio de archivo la `Welcome` método en el controlador para poner el mensaje y NumTimes en ViewBag. El elemento ViewBag es un objeto dinámico. Esto significa que puede colocar todo lo desee a él. ViewBag no tiene ninguna propiedad definida hasta que coloca algo dentro de él.

La completa `HelloWorldController.vb` con la nueva clase en el mismo archivo.

[!code-vb[Main](adding-a-view/samples/sample6.vb)]

Ahora nuestra ViewBag contiene datos que se pasa a través de la vista automáticamente. De nuevo, o bien, podríamos han pasado en nuestro propio objeto similar al siguiente si nos gustó:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

Ahora necesitamos un `WelcomeView` plantilla! Ejecute la aplicación, por lo que se compila el nuevo código. Cierre el explorador, haga clic en el `Welcome` método y, a continuación, haga clic en **agregar vista**.

Esto es lo que su **agregar vista** cuadro de diálogo es similar a.

[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)

Agregue el siguiente código bajo el `<h2>` elemento en el nuevo <em>Bienvenido.</em> archivo VBHTML. Se deberá realizar un bucle y diga &quot;Hello&quot; tantas veces como el usuario dice que deberíamos!

[!code-vbhtml[Main](adding-a-view/samples/sample8.vbhtml)]

Ejecute la aplicación y vaya a `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Ahora los datos se toman de la dirección URL y pasa automáticamente al controlador. El controlador empaqueta los datos en un `Model` objeto y pasa ese objeto a la vista. La vista que muestra los datos como HTML al usuario.

[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)

Bueno, eso era un tipo de un &quot;M&quot; para el modelo, pero no el tipo de base de datos. Vamos a aprovechar lo que hemos aprendido para crear una base de datos de películas.

> [!div class="step-by-step"]
> [Anterior](adding-a-controller.md)
> [Siguiente](adding-a-model.md)
