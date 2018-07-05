---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
title: Agregar una vista | Microsoft Docs
author: Rick-Anderson
description: 'Nota: Una versión actualizada de este tutorial está disponible aquí que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro y mucho más fácil de seguir y demostraciones...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: dde851d7-882e-4d99-9b96-cf96daed81cc
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 0974cd2e06ed86c736214944a29a5a1eab8af50b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391514"
---
<a name="adding-a-view"></a>Agregar una vista
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Hay disponible una versión actualizada de este tutorial [aquí](../../getting-started/introduction/getting-started.md) que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro y mucho más fácil de seguir y se muestran las características más.


En esta sección va a modificar el `HelloWorldController` clase para usar la vista archivos de plantilla correctamente encapsulan el proceso de generar respuestas HTML a un cliente.

Creará un archivo de plantilla de vista mediante el [motor de vistas Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) introducidos con ASP.NET MVC 3. Las plantillas de vista Razor tienen una *.cshtml* la extensión de archivo y proporcionan una manera elegante para crear el código HTML de salida con C#. Minimiza el número de caracteres y pulsaciones de teclas necesarias cuando se escribe una plantilla de vista Razor y permite un rápido, fluido flujo de trabajo de codificación.

Actualmente, el método `Index` devuelve una cadena con un mensaje que está codificado de forma rígida en la clase de controlador. Cambiar el `Index` método devuelva un `View` de objeto, como se muestra en el código siguiente:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

El `Index` método anterior usa una plantilla de vista para generar una respuesta HTML al explorador. Los métodos de controlador (también conocido como [métodos de acción](http://rachelappel.com/asp.net-mvc-actionresults-explained)), como el `Index` método anterior, suelen devolver un [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (o una clase derivada de [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), los tipos no primitivos como cadena.

En el proyecto, agregue una plantilla de vista que puede usar con el `Index` método. Para ello, haga clic en el `Index` método y haga clic en **agregar vista**.

![](adding-a-view/_static/image1.png)

El **agregar vista** aparece el cuadro de diálogo. Deje los valores predeterminados de la manera son y haga clic en el **agregar** botón:

![](adding-a-view/_static/image2.png)

El *MvcMovie\Views\HelloWorld* carpeta y el *MvcMovie\Views\HelloWorld\Index.cshtml* se crean archivos. Puede verlos en **el Explorador de soluciones**:

![](adding-a-view/_static/image3.png)

La siguiente muestra la *Index.cshtml* archivo creado:

![HelloWorldIndex](adding-a-view/_static/image4.png)

Agregue el siguiente código HTML bajo el `<h2>` etiqueta.

[!code-html[Main](adding-a-view/samples/sample2.html)]

La completa *MvcMovie\Views\HelloWorld\Index.cshtml* archivo se muestra a continuación.

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=7-8)]

Si utiliza Visual Studio 2012, en el Explorador de soluciones, haga clic la *Index.cshtml* de archivo y seleccione **ver en Inspector de página**.

![PI](adding-a-view/_static/image5.png)

El [tutorial del Inspector de página](../../views/using-page-inspector-in-aspnet-mvc.md) tiene más información sobre esta nueva herramienta.

Como alternativa, ejecute la aplicación y vaya a la `HelloWorld` controlador (`http://localhost:xxxx/HelloWorld`). El `Index` método en el controlador no hacen mucho trabajo; simplemente ejecutó la instrucción `return View()`, que especifica que el método debe utilizar un archivo de plantilla de vista para presentar una respuesta al explorador. Dado que no especifica explícitamente el nombre del archivo de plantilla de vista para usar, ASP.NET MVC usa de forma predeterminada el *Index.cshtml* los archivos de vista el *\Views\HelloWorld* carpeta. La siguiente imagen muestra la cadena &quot;Hola desde nuestra plantilla de vista!&quot; codificado de forma rígida en la vista.

![](adding-a-view/_static/image6.png)

Parece bastante bueno. Sin embargo, tenga en cuenta que se muestra la barra de título del explorador &quot;indizar mi A ASP.NET&quot; y el vínculo grande en la parte superior de la página se lee &quot;su logotipo aquí.&quot; A continuación el &quot;su logotipo aquí.&quot; vínculo son aproximadamente de registro y registro en vínculos y, a continuación que se vincula a la página principal y póngase en contacto con las páginas. Vamos a cambiar algunas de ellas.

## <a name="changing-views-and-layout-pages"></a>Cambiar vistas y páginas de diseño

En primer lugar, desea cambiar la &quot;su logotipo aquí.&quot; título en la parte superior de la página. Es común a todas las páginas que el texto. En realidad se implementa en un único lugar en el proyecto, aunque aparezca en todas las páginas en la aplicación. Vaya a la */Views/Shared* carpeta **el Explorador de soluciones** y abra el  *\_Layout.cshtml* archivo. Este archivo se denomina un *página de diseño* y es el recurso compartido &quot;shell&quot; que usan todas las demás páginas.

![_LayoutCshtml](adding-a-view/_static/image7.png)

Las plantillas de diseño permiten especificar el diseño del contenedor HTML del sitio en un solo lugar y, a continuación, aplicarla en varias páginas del sitio. Busque la línea `@RenderBody()`. `RenderBody` es un marcador de posición donde se mostrarán todas las páginas específicas de vista que cree, &quot;encapsuladas&quot; en la página de diseño. Por ejemplo, si selecciona el vínculo acerca la *Views\Home\About.cshtml* vista se representa dentro de la `RenderBody` método.

Cambie el encabezado de título del sitio en la plantilla de diseño de &quot;su logotipo aquí&quot; a &quot;MVC Movie&quot;.

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Reemplace el contenido del elemento de título con el marcado siguiente:

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

Ejecute la aplicación y observe que ahora dice &quot;MVC Movie &quot;. Haga clic en el **sobre** vínculo y ver cómo se muestra esa página &quot;MVC Movie&quot;, demasiado. Fuimos capaces de realizar el cambio una vez en la plantilla de diseño y dispone de todas las páginas del sitio refleja el nuevo título.

![](adding-a-view/_static/image8.png)

Ahora, vamos a cambiar el título de la vista de índice.

Abra *MvcMovie\Views\HelloWorld\Index.cshtml*. Hay dos lugares para realizar un cambio: en primer lugar, el texto que aparece en el título del explorador y, a continuación, en el encabezado secundario (el `<h2>` elemento). Haremos que sean algo diferentes para que pueda ver qué parte del código cambia cada área de la aplicación.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml)]

Para indicar el título HTML para mostrar el código anterior establece un `Title` propiedad de la `ViewBag` objeto (que se encuentra en la *Index.cshtml* plantilla de vista). Si repasa el código fuente de la plantilla de diseño, observará que la plantilla utiliza este valor en el `<title>` elemento como parte de la `<head>` sección del archivo HTML que se modificó anteriormente. Mediante este `ViewBag` enfoque, puede pasar fácilmente otros parámetros entre la plantilla de vista y el archivo de diseño.

Ejecute la aplicación y vaya a `http://localhost:xx/HelloWorld`. Tenga en cuenta que el título del explorador, el encabezado principal y los encabezados secundarios han cambiado. (Si no ve los cambios en el explorador, es posible que esté viendo contenido almacenado en caché. Presione Ctrl+F5 en el explorador para forzar que se cargue la respuesta del servidor). El título del explorador se crea con el `ViewBag.Title` se establece el *Index.cshtml* ver la plantilla y el adicionales &quot;-Movie App&quot; agregado en el archivo de diseño.

Observe también cómo el contenido en el *Index.cshtml* plantilla de vista se combinó con el  *\_Layout.cshtml* se envió la plantilla de vista y una única respuesta HTML al explorador. Con las plantillas de diseño es realmente fácil hacer cambios para que se apliquen en todas las páginas de la aplicación.

![](adding-a-view/_static/image9.png)

Nuestra pequeña cantidad de &quot;datos&quot; (en este caso el &quot;Hola desde nuestra plantilla de vista!&quot; mensaje) está codificado de forma rígida, sin embargo. La aplicación de MVC tiene un &quot;V&quot; (vista) y tiene un &quot;C&quot; (controlador), pero no &quot;M&quot; (model) todavía. En breve, le guiaremos a través de cómo crear una base de datos y recuperar los datos del modelo del mismo.

## <a name="passing-data-from-the-controller-to-the-view"></a>Pasar datos del controlador a la vista

Antes de hablar acerca de los modelos y vaya a una base de datos, sin embargo, en primer lugar hablemos acerca de cómo pasar información desde el controlador a una vista. Las clases de controlador se invocan en respuesta a una solicitud de dirección URL entrante. Una clase de controlador es donde se escribe el código que controla el explorador entrante solicita, recupera datos de una base de datos y, en última instancia decida qué tipo de respuesta para enviar al explorador. Plantillas de vista, a continuación, pueden utilizarse desde un controlador para generar y dar formato a una respuesta HTML al explorador.

Los controladores son responsables de proporcionar los datos u objetos son necesarios para una plantilla de vista presentar una respuesta al explorador. Una práctica recomendada: **una plantilla de vista nunca debe realizar la lógica de negocios ni interactuar directamente con una base de datos**. En su lugar, una plantilla de vista deban trabajar solo con los datos que se proporcionan el controlador. Esto mantiene &quot;separación de preocupaciones&quot; ayuda a mantener el código limpio, fácil de probar y más fácil de mantener.

Actualmente, el `Welcome` método de acción en el `HelloWorldController` clase toma un `name` y un `numTimes` parámetro y, a continuación, obtiene los valores directamente en el explorador. En lugar de que el controlador represente esta respuesta como una cadena, vamos a cambiar el controlador para que use una plantilla de vista en su lugar. La plantilla de vista generará una respuesta dinámica, lo que significa que debe pasar las partes de datos adecuadas desde el controlador a la vista para que se genere la respuesta. Para ello, indique al controlador que coloque los datos dinámicos (parámetros) que necesita la plantilla de vista en un `ViewBag` objeto que puede tener acceso la plantilla de vista.

Vuelva a la *HelloWorldController.cs* y cambie el `Welcome` método para agregar un `Message` y `NumTimes` valor para el `ViewBag` objeto. `ViewBag` es un objeto dinámico, lo que significa que puede colocar todo lo desee en él; la `ViewBag` objeto no tiene ninguna propiedad definida hasta que coloca algo dentro de él. El [sistema de enlace de modelos ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) asigna automáticamente los parámetros con nombre (`name` y `numTimes`) de la cadena de consulta en la barra de direcciones para los parámetros del método. El archivo *HelloWorldController.cs* completo tiene este aspecto:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

Ahora el `ViewBag` objeto contiene los datos que se pasará automáticamente a la vista.

A continuación, necesita una plantilla de vista principal. En el **compilar** menú, seleccione **MvcMovie compilación** para asegurarse de que se compila el proyecto.

A continuación, haga clic en el `Welcome` método y haga clic en **agregar vista**.

![](adding-a-view/_static/image10.png)

Esto es lo que el **agregar vista** cuadro de diálogo es similar a:

![](adding-a-view/_static/image11.png)

Haga clic en **agregar**y, a continuación, agregue el siguiente código bajo el `<h2>` elemento en el nuevo *Welcome.cshtml* archivo. Se creará un bucle que dice &quot;Hello&quot; tantas veces como el usuario indica que debería. La completa *Welcome.cshtml* archivo se muestra a continuación.

[!code-cshtml[Main](adding-a-view/samples/sample8.cshtml)]

Ejecute la aplicación y vaya a la dirección URL siguiente:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Ahora los datos se toman de la dirección URL y pasa al controlador mediante la [enlazador de modelos](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx). El controlador empaqueta los datos en un `ViewBag` objeto y pasa ese objeto a la vista. La vista, a continuación, muestra los datos como HTML al usuario.

![](adding-a-view/_static/image12.png)

En el ejemplo anterior, hemos usado un `ViewBag` objeto que se va a pasar datos a una vista del controlador. Esta última en el tutorial, usaremos un modelo de vista para pasar datos de un controlador a una vista. El enfoque del modelo de vista para pasar los datos es generalmente mucho más conveniente el enfoque de contenedor de vista. Consulte la entrada de blog [vistas dinámicas de V fuertemente tipados](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) para obtener más información.

Bueno, eso era un tipo de un &quot;M&quot; para el modelo, pero no el tipo de base de datos. Vamos a aprovechar lo que hemos aprendido para crear una base de datos de películas.

> [!div class="step-by-step"]
> [Anterior](adding-a-controller.md)
> [Siguiente](adding-a-model.md)
