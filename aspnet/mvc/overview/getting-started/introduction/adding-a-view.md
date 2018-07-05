---
title: Agregar una vista a una aplicación MVC
author: Rick-Anderson
description: Agregar una vista a una aplicación MVC
ms.author: riande
manager: wpickett
ms.date: 09/1721/2017
ms.topic: article
ms.technology: dotnet-mvc
uid: mvc/overview/getting-started/introduction/adding-a-view
ms.openlocfilehash: a6046d467dbd36f4f3ca5721a3b7c7424e18e00a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370288"
---
<a name="adding-a-view"></a>Agregar una vista
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

En esta sección va a modificar el `HelloWorldController` clase para usar la vista archivos de plantilla correctamente encapsulan el proceso de generar respuestas HTML a un cliente. 

Creará un archivo de plantilla de vista mediante el [motor de vistas Razor](../../../../web-pages/overview/getting-started/introducing-razor-syntax-c.md). Las plantillas de vista Razor tienen una *.cshtml* la extensión de archivo y proporcionan una manera elegante para crear el código HTML de salida con C#. Minimiza el número de caracteres y pulsaciones de teclas necesarias cuando se escribe una plantilla de vista Razor y permite un rápido, fluido flujo de trabajo de codificación.

Actualmente, el método `Index` devuelve una cadena con un mensaje que está codificado de forma rígida en la clase de controlador. Cambiar el `Index` método devuelva un `View` de objeto, como se muestra en el código siguiente:

[!code-csharp[Main](adding-a-view/samples/sample1.cs?highlight=1,3)]

El `Index` método anterior usa una plantilla de vista para generar una respuesta HTML al explorador. Los métodos de controlador (también conocido como [métodos de acción](http://rachelappel.com/asp.net-mvc-actionresults-explained)), como el `Index` método anterior, suelen devolver un [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (o una clase derivada de [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), los tipos no primitivos como cadena.

Haga clic en el *Views\HelloWorld* carpeta y haga clic en **agregar**, a continuación, haga clic en **página de vista de MVC 5 con diseño (Razor)**.
  
![](adding-a-view/_static/image1.png)   
  
En el **especificar nombre para el elemento** diálogo cuadro, escriba *índice*y, a continuación, haga clic en **Aceptar**.  
  
![](adding-a-view/_static/image2.png)  
  
En el **seleccionar una página de diseño** cuadro de diálogo, acepte el valor predeterminado  **\_Layout.cshtml** y haga clic en **Aceptar**.  
  
![](adding-a-view/_static/image3.png)  
  
En el cuadro de diálogo anterior, el *Views\Shared* carpeta está seleccionada en el panel izquierdo. Si tenía un archivo de diseño personalizado en otra carpeta, se puede seleccionar. Hablaremos sobre el archivo de diseño más adelante en el tutorial

El *MvcMovie\Views\HelloWorld\Index.cshtml* se crea el archivo.

![](adding-a-view/_static/image4.png)

Agregue el siguiente marcado resaltado.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml?highlight=4-11)]

Haga clic en el *Index.cshtml* de archivo y seleccione **ver en el explorador**.

![PI](adding-a-view/_static/image5.png)

También puede derecho en el *Index.cshtml* de archivo y seleccione **ver en Inspector de página.** Consulte la [tutorial del Inspector de página](../../views/using-page-inspector-in-aspnet-mvc.md) para obtener más información.

Como alternativa, ejecute la aplicación y vaya a la `HelloWorld` controlador (`http://localhost:xxxx/HelloWorld`). El `Index` método en el controlador no hacen mucho trabajo; simplemente ejecutó la instrucción `return View()`, que especifica que el método debe utilizar un archivo de plantilla de vista para presentar una respuesta al explorador. Dado que no especifica explícitamente el nombre del archivo de plantilla de vista para usar, ASP.NET MVC usa de forma predeterminada el *Index.cshtml* los archivos de vista el *\Views\HelloWorld* carpeta. La siguiente imagen muestra la cadena &quot;Hola desde nuestra plantilla de vista!&quot; codificado de forma rígida en la vista.

![](adding-a-view/_static/image6.png)

Parece bastante bueno. Sin embargo, tenga en cuenta que se muestra la barra de título del explorador &quot;Index - Mis fac ASP.NET "y el vínculo grande en la parte superior de la página lee"Application name". Dependiendo de cómo realizar la ventana del explorador, es posible que deba haga clic en las tres barras en la esquina superior derecha para ver el para el **inicio**, **sobre**, **póngase en contacto con**, **Registrar** y **iniciarla** vínculos.

## <a name="changing-views-and-layout-pages"></a>Cambiar vistas y páginas de diseño

En primer lugar, desea cambiar la &quot;nombre de la aplicación&quot; vínculo en la parte superior de la página. Es común a todas las páginas que el texto. En realidad se implementa en un único lugar en el proyecto, aunque aparezca en todas las páginas en la aplicación. Vaya a la */Views/Shared* carpeta **el Explorador de soluciones** y abra el  *\_Layout.cshtml* archivo. Este archivo se denomina un *página de diseño* y se encuentra en la carpeta compartida que usan todas las demás páginas.

![_LayoutCshtml](adding-a-view/_static/image7.png)

Las plantillas de diseño permiten especificar el diseño del contenedor HTML del sitio en un solo lugar y, a continuación, aplicarla en varias páginas del sitio. Busque la línea `@RenderBody()`. `RenderBody` es un marcador de posición donde se mostrarán todas las páginas específicas de vista que cree, &quot;encapsuladas&quot; en la página de diseño. Por ejemplo, si selecciona el **sobre** vínculo, el *Views\Home\About.cshtml* vista se representa dentro de la `RenderBody` método.

Cambie el contenido del elemento de título. Cambiar el [ActionLink](https://msdn.microsoft.com/library/dd504972(v=vs.108).aspx) en la plantilla de diseño de &quot;nombre de la aplicación&quot; a &quot;MVC Movie&quot; y el controlador de `Home` a `Movies`. El archivo de diseño completo se muestra a continuación:

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=6,20)]

Ejecute la aplicación y observe que ahora dice &quot;MVC Movie &quot;. Haga clic en el **sobre** vínculo y ver cómo se muestra esa página &quot;MVC Movie&quot;, demasiado. Fuimos capaces de realizar el cambio una vez en la plantilla de diseño y dispone de todas las páginas del sitio refleja el nuevo título.

![](adding-a-view/_static/image8.png)

Cuando se crea por primera vez el *Views\HelloWorld\Index.cshtml* archivo, contiene el código siguiente:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

El código Razor anterior establece explícitamente la página de diseño. Examine el *vistas\\_ViewStart.cshtml* archivo, contiene el mismo marcado exacto de Razor. El *[vistas\\_ViewStart.cshtml](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)* archivo define el diseño común que se va a usar todas las vistas, por lo tanto, puede comentar o elimine dicho código desde el *Views\HelloWorld\ Index.cshtml* archivo.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml?highlight=1-3)]

Puede usar la propiedad `Layout` para establecer una vista de diseño diferente o establecerla en `null` para que no se use ningún archivo de diseño.

Ahora, vamos a cambiar el título de la vista de índice.

Abra *MvcMovie\Views\HelloWorld\Index.cshtml*. Hay dos lugares para realizar un cambio: en primer lugar, el texto que aparece en el título del explorador y, a continuación, en el encabezado secundario (el `<h2>` elemento). Haremos que sean algo diferentes para que pueda ver qué parte del código cambia cada área de la aplicación.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml?highlight=2,5)]

Para indicar el título HTML para mostrar el código anterior establece un `Title` propiedad de la `ViewBag` objeto (que se encuentra en la *Index.cshtml* plantilla de vista). Tenga en cuenta que la plantilla de diseño ( *Views\Shared\\_Layout.cshtml* ) usa este valor en el `<title>` elemento como parte de la `<head>` sección del archivo HTML que se modificó anteriormente.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml?highlight=6)]

Mediante este `ViewBag` enfoque, puede pasar fácilmente otros parámetros entre la plantilla de vista y el archivo de diseño.

Ejecute la aplicación. Tenga en cuenta que el título del explorador, el encabezado principal y los encabezados secundarios han cambiado. (Si no ve los cambios en el explorador, es posible que esté viendo contenido almacenado en caché. Presione Ctrl+F5 en el explorador para forzar que se cargue la respuesta del servidor). El título del explorador se crea con el `ViewBag.Title` se establece el *Index.cshtml* ver la plantilla y el adicionales &quot;-Movie App&quot; agregado en el archivo de diseño.

Observe también cómo el contenido en el *Index.cshtml* plantilla de vista se combinó con el  *\_Layout.cshtml* se envió la plantilla de vista y una única respuesta HTML al explorador. Con las plantillas de diseño es realmente fácil hacer cambios para que se apliquen en todas las páginas de la aplicación.

![](adding-a-view/_static/image9.png)

Nuestra pequeña cantidad de &quot;datos&quot; (en este caso el &quot;Hola desde nuestra plantilla de vista!&quot; mensaje) está codificado de forma rígida, sin embargo. La aplicación de MVC tiene un &quot;V&quot; (vista) y tiene un &quot;C&quot; (controlador), pero no &quot;M&quot; (model) todavía. En breve, le guiaremos a través de cómo crear una base de datos y recuperar los datos de modelo de.

## <a name="passing-data-from-the-controller-to-the-view"></a>Pasar datos del controlador a la vista

Antes de hablar acerca de los modelos y vaya a una base de datos, sin embargo, en primer lugar hablemos acerca de cómo pasar información desde el controlador a una vista. Las clases de controlador se invocan en respuesta a una solicitud de dirección URL entrante. Una clase de controlador es donde se escribe el código que controla el explorador entrante solicita, recupera datos de una base de datos y, en última instancia decida qué tipo de respuesta para enviar al explorador. Plantillas de vista, a continuación, pueden utilizarse desde un controlador para generar y dar formato a una respuesta HTML al explorador.

Los controladores son responsables de proporcionar los datos u objetos son necesarios para una plantilla de vista presentar una respuesta al explorador. Una práctica recomendada: **una plantilla de vista nunca debe realizar la lógica de negocios ni interactuar directamente con una base de datos**. En su lugar, una plantilla de vista deban trabajar solo con los datos que se proporcionan el controlador. Esto mantiene &quot;separación de preocupaciones&quot; ayuda a mantener el código limpio, fácil de probar y más fácil de mantener.

Actualmente, el `Welcome` método de acción en el `HelloWorldController` clase toma un `name` y un `numTimes` parámetro y, a continuación, obtiene los valores directamente en el explorador. En lugar de que el controlador represente esta respuesta como una cadena, vamos a cambiar el controlador para que use una plantilla de vista en su lugar. La plantilla de vista generará una respuesta dinámica, lo que significa que debe pasar las partes de datos adecuadas desde el controlador a la vista para que se genere la respuesta. Para ello, indique al controlador que coloque los datos dinámicos (parámetros) que necesita la plantilla de vista en un `ViewBag` objeto que puede tener acceso la plantilla de vista.

Vuelva a la *HelloWorldController.cs* y cambie el `Welcome` método para agregar un `Message` y `NumTimes` valor para el `ViewBag` objeto. `ViewBag` es un objeto dinámico, lo que significa que puede colocar todo lo desee en él; la `ViewBag` objeto no tiene ninguna propiedad definida hasta que coloca algo dentro de él. El [sistema de enlace de modelos ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) asigna automáticamente los parámetros con nombre (`name` y `numTimes`) de la cadena de consulta en la barra de direcciones para los parámetros del método. El archivo *HelloWorldController.cs* completo tiene este aspecto:

[!code-csharp[Main](adding-a-view/samples/sample8.cs)]

Ahora el `ViewBag` objeto contiene los datos que se pasará automáticamente a la vista. A continuación, necesita una plantilla de vista principal. En el **compilar** menú, seleccione **compilar solución** (o Ctrl + Mayús + B) para asegurarse de que se compila el proyecto. Haga clic en el *Views\HelloWorld* carpeta y haga clic en **agregar**, a continuación, haga clic en **página de vista de MVC 5 con diseño (Razor)**.
  
![](adding-a-view/_static/image10.png)   
  
En el **especificar nombre para el elemento** diálogo cuadro, escriba *bienvenida*y, a continuación, haga clic en **Aceptar**.   
  
En el **seleccionar una página de diseño** cuadro de diálogo, acepte el valor predeterminado  **\_Layout.cshtml** y haga clic en **Aceptar**.  
  
![](adding-a-view/_static/image11.png)   

El *MvcMovie\Views\HelloWorld\Welcome.cshtml* se crea el archivo.

Reemplace el marcado en el *Welcome.cshtml* archivo. Se creará un bucle que dice &quot;Hello&quot; tantas veces como el usuario indica que debería. La completa *Welcome.cshtml* archivo se muestra a continuación.

[!code-cshtml[Main](adding-a-view/samples/sample9.cshtml)]

Ejecute la aplicación y vaya a la dirección URL siguiente:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Ahora los datos se toman de la dirección URL y pasa al controlador mediante la [enlazador de modelos](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx). El controlador empaqueta los datos en un `ViewBag` objeto y pasa ese objeto a la vista. La vista, a continuación, muestra los datos como HTML al usuario.

![](adding-a-view/_static/image12.png)

En el ejemplo anterior, hemos usado un `ViewBag` objeto que se va a pasar datos a una vista del controlador. Más adelante en el tutorial usaremos un modelo de vista para pasar datos de un controlador a una vista. El enfoque del modelo de vista para pasar los datos es generalmente mucho más conveniente el enfoque de contenedor de vista. Consulte la entrada de blog [vistas dinámicas de V fuertemente tipados](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) para obtener más información. 

Bueno, eso era un tipo de un &quot;M&quot; para el modelo, pero no el tipo de base de datos. Vamos a aprovechar lo que hemos aprendido para crear una base de datos de películas.

> [!div class="step-by-step"]
> [Anterior](adding-a-controller.md)
> [Siguiente](adding-a-model.md)
