---
title: Agregar una vista a una aplicación MVC
author: Rick-Anderson
description: Agregar una vista a una aplicación MVC
ms.author: riande
manager: wpickett
ms.date: 09/1721/2017
ms.topic: article
ms.technology: dotnet-mvc
ms.prod: .net-framework
uid: mvc/overview/getting-started/introduction/adding-a-view
ms.openlocfilehash: 21db97e635b5db580df31f46ca7f8b60a80d6f94
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2018
ms.locfileid: "30873435"
---
<a name="adding-a-view"></a>Agregar una vista
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

En esta sección va a modificar el `HelloWorldController` clase para usar la vista archivos de plantilla a correctamente encapsulan el proceso de generar las respuestas HTML a un cliente. 

Se creará un archivo de plantilla de la vista mediante el [motor de vista Razor](../../../../web-pages/overview/getting-started/introducing-razor-syntax-c.md). Plantillas de vista Razor tienen un *.cshtml* la extensión de archivo y proporcionan una forma elegante para crear HTML con C# de salida. Razor minimiza el número de caracteres y pulsaciones de teclas necesarias al escribir una plantilla de vista y permite un fluido rápido, flujo de trabajo de codificación.

Actualmente, el método `Index` devuelve una cadena con un mensaje que está codificado de forma rígida en la clase de controlador. Cambiar el `Index` método para devolver un `View` de objeto, como se muestra en el código siguiente:

[!code-csharp[Main](adding-a-view/samples/sample1.cs?highlight=1,3)]

El `Index` método anterior usa una plantilla de vista para generar una respuesta HTML al explorador. Los métodos de controlador (también conocido como [métodos de acción](http://rachelappel.com/asp.net-mvc-actionresults-explained)), como el `Index` método anterior, generalmente devuelven un [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (o una clase derivada de [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), los tipos no primitivos como cadena.

Haga clic con el *Views\HelloWorld* carpeta y haga clic en **agregar**, a continuación, haga clic en **página de vista de MVC 5 con diseño (Razor)**.
  
![](adding-a-view/_static/image1.png)   
  
En el **especificar nombre para el elemento** diálogo cuadro, escriba *índice*y, a continuación, haga clic en **Aceptar**.  
  
![](adding-a-view/_static/image2.png)  
  
En el **seleccionar una página de diseño** cuadro de diálogo, acepte el valor predeterminado  **\_Layout.cshtml** y haga clic en **Aceptar**.  
  
![](adding-a-view/_static/image3.png)  
  
En el cuadro de diálogo anterior, el *Views\Shared* se selecciona la carpeta en el panel izquierdo. Si tuviera un archivo de diseño personalizado en otra carpeta, pudieran seleccionarlo. Hablaremos sobre el archivo de diseño más adelante en el tutorial

El *MvcMovie\Views\HelloWorld\Index.cshtml* se crea el archivo.

![](adding-a-view/_static/image4.png)

Agregue el siguiente marcado resaltado.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml?highlight=4-11)]

Haga clic con el *Index.cshtml* de archivos y seleccione **ver en el explorador**.

![PI](adding-a-view/_static/image5.png)

También puede hacer haga clic el *Index.cshtml* de archivos y seleccione **ver en Inspector de página.** Consulte la [tutorial de Inspector de página](../../views/using-page-inspector-in-aspnet-mvc.md) para obtener más información.

O bien, ejecute la aplicación y busque el `HelloWorld` controlador (`http://localhost:xxxx/HelloWorld`). El `Index` método en el controlador no hacen mucho trabajo; simplemente se ejecutó la instrucción `return View()`, que especifica que el método debe utilizar un archivo de plantilla de la vista para presentar una respuesta al explorador. Dado que no especifica explícitamente el nombre del archivo de plantilla de vista para usar, ASP.NET MVC usado de forma predeterminada el *Index.cshtml* archivo de vista en el *\Views\HelloWorld* carpeta. La imagen siguiente muestra la cadena &quot;Hola de nuestra plantilla vista!&quot; codificado de forma rígida en la vista.

![](adding-a-view/_static/image6.png)

Parece bastante bueno. Sin embargo, tenga en cuenta que la barra de título del explorador muestra &quot;índice - mi Fac de ASP.NET "y el vínculo grande en la parte superior de la página dice"Nombre de la aplicación". Dependiendo de cómo pequeño que la ventana del explorador, tendrá que hacer clic en las tres barras en la esquina superior derecha para ver la a la **inicio**, **sobre**, **póngase en contacto con**, **Registrar** y **sesión** vínculos.

## <a name="changing-views-and-layout-pages"></a>Cambiar vistas y páginas de diseño

En primer lugar, desea cambiar la &quot;nombre de la aplicación&quot; vínculo en la parte superior de la página. Que el texto es común a todas las páginas. En realidad se implementa en un único lugar en el proyecto, aunque aparezca en todas las páginas en la aplicación. Vaya a la */vistas/Shared* carpeta **el Explorador de soluciones** y abra el  *\_Layout.cshtml* archivo. Este archivo se denomina un *página de diseño* y se encuentra en la carpeta compartida que utilizan todas las demás páginas.

![_LayoutCshtml](adding-a-view/_static/image7.png)

Las plantillas de diseño permiten especificar el diseño de contenedor HTML del sitio en un solo lugar y, a continuación, se aplican a través de varias páginas en el sitio. Busque la línea `@RenderBody()`. `RenderBody` es un marcador de posición donde se mostrarán todas las páginas específicas de vista que cree, &quot;encapsuladas&quot; en la página de diseño. Por ejemplo, si selecciona el **sobre** vínculo, el *Views\Home\About.cshtml* vista se representa dentro de la `RenderBody` método.

Cambie el contenido del elemento de título. Cambiar el [ActionLink](https://msdn.microsoft.com/library/dd504972(v=vs.108).aspx) en la plantilla de diseño de &quot;nombre de la aplicación&quot; a &quot;MVC película&quot; y el controlador de `Home` a `Movies`. El archivo de diseño completo se muestra a continuación:

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=6,20)]

Ejecutar la aplicación y observe que ahora dice &quot;MVC película &quot;. Haga clic en el **sobre** vínculo y ver cómo se muestra esa página &quot;MVC película&quot;, demasiado. Hemos podidos realizar el cambio una vez en la plantilla de diseño y ha todas las páginas en el sitio de reflejar el nuevo título.

![](adding-a-view/_static/image8.png)

Cuando se crea por primera vez el *Views\HelloWorld\Index.cshtml* archivo, contiene el código siguiente:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

El código Razor anterior establece explícitamente la página de diseño. Examine la *vistas\\_ViewStart.cshtml* archivo, contiene el marcado de Razor mismo exacto. El *[vistas\\_ViewStart.cshtml](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)* archivo define el diseño común que se va a usar todas las vistas, por lo tanto, puede anule o quitar ese código desde el *Views\HelloWorld\ Index.cshtml* archivo.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml?highlight=1-3)]

Puede usar la propiedad `Layout` para establecer una vista de diseño diferente o establecerla en `null` para que no se use ningún archivo de diseño.

Ahora, vamos a cambiar el título de la vista de índice.

Abra *MvcMovie\Views\HelloWorld\Index.cshtml*. Hay dos lugares para realizar un cambio: en primer lugar, el texto que aparece en el título del explorador y, a continuación, en el encabezado secundario (el `<h2>` elemento). Haremos que sean algo diferentes para que pueda ver qué parte del código cambia cada área de la aplicación.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml?highlight=2,5)]

Para indicar el título HTML para mostrar, el código anterior establece una `Title` propiedad de la `ViewBag` objeto (que está en el *Index.cshtml* plantilla de vista). Tenga en cuenta que la plantilla de diseño ( *Views\Shared\\_Layout.cshtml* ) usa este valor en el `<title>` elemento como parte de la `<head>` sección del HTML que hemos modificado anteriormente.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml?highlight=6)]

Mediante este `ViewBag` enfoque, puede pasar fácilmente otros parámetros entre la plantilla de vista y el archivo de diseño.

Ejecute la aplicación. Tenga en cuenta que el título del explorador, el encabezado principal y los encabezados secundarios han cambiado. (Si no ve los cambios en el explorador, es posible que esté viendo contenido almacenado en caché. Presione Ctrl+F5 en el explorador para forzar que se cargue la respuesta del servidor). El título del explorador se crea con el `ViewBag.Title` se establece el *Index.cshtml* ver la plantilla y la adición &quot;-aplicación de película&quot; agregado en el archivo de diseño.

Observe también cómo el contenido en el *Index.cshtml* plantilla de vista se combinó con el  *\_Layout.cshtml* plantilla de vista y una única respuesta HTML se envía al explorador. Con las plantillas de diseño es realmente fácil hacer cambios para que se apliquen en todas las páginas de la aplicación.

![](adding-a-view/_static/image9.png)

Nuestro poco de &quot;datos&quot; (en este caso el &quot;Hola de nuestra plantilla vista!&quot; mensaje) está codificado de forma rígida, aunque. La aplicación de MVC tiene un &quot;V&quot; (vista) y tienes un &quot;C&quot; (controlador), pero no &quot;M&quot; (modelo) todavía. En breve, le mostraremos cómo crear una base de datos y recuperar los datos del modelo del mismo.

## <a name="passing-data-from-the-controller-to-the-view"></a>Pasar datos del controlador a la vista

Antes de que se vaya a una base de datos y hablar acerca de los modelos, sin embargo, en primer lugar hablemos acerca de cómo pasar información desde el controlador a una vista. Las clases de controlador se invocan en respuesta a una solicitud de dirección URL entrante. Una clase de controlador es donde se escribe el código que controla el explorador entrante solicita, recupera datos de una base de datos y en última instancia decida qué tipo de respuesta para enviar al explorador. Plantillas de vista, a continuación, pueden utilizarse desde un controlador para generar y dar formato a una respuesta HTML al explorador.

Los controladores son responsables de proporcionar los datos u objetos necesarios para una plantilla de vista presentar una respuesta al explorador. Una práctica recomendada: **una plantilla de vista nunca debe realizar la lógica de negocios o interactuar directamente con una base de datos**. En su lugar, una plantilla de vista debe funcionar solo con los datos que están indicados por el controlador. Mantenimiento de todo esto &quot;separación de intereses&quot; ayuda a mantener el código limpio, comprobable y más fácil de mantener.

Actualmente, el `Welcome` método de acción en el `HelloWorldController` clase toma un `name` y un `numTimes` parámetro y, a continuación, los valores directamente en el Explorador de salidas. En lugar de tener el controlador de representar esta respuesta como una cadena, vamos a cambiar el controlador para usar una plantilla de vista en su lugar. La plantilla de vista generará una respuesta dinámica, lo que significa que debe pasar las partes de datos adecuadas desde el controlador a la vista para que se genere la respuesta. Puede hacerlo haciendo que el controlador de colocar los datos dinámicos (parámetros) que necesita la plantilla de vista en un `ViewBag` objeto que puede tener acceso la plantilla de vista.

Vuelva a la *HelloWorldController.cs* y cambie el `Welcome` método para agregar un `Message` y `NumTimes` valor para el `ViewBag` objeto. `ViewBag` es un objeto dinámico, lo que significa que puede colocar todo lo que desees la `ViewBag` objeto no tiene ninguna propiedad definida hasta que escriba algo dentro de él. El [sistema de enlace de modelo de ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) asigna automáticamente los parámetros con nombre (`name` y `numTimes`) de la cadena de consulta en la barra de direcciones a los parámetros del método. El archivo *HelloWorldController.cs* completo tiene este aspecto:

[!code-csharp[Main](adding-a-view/samples/sample8.cs)]

Ahora el `ViewBag` objeto contiene los datos que se pasará automáticamente a la vista. A continuación, hay una plantilla de vista de página principal. En el **generar** menú, seleccione **generar solución** (o Ctrl + Mayús + B) para asegurarse de que se compila el proyecto. Haga clic con el *Views\HelloWorld* carpeta y haga clic en **agregar**, a continuación, haga clic en **página de vista de MVC 5 con diseño (Razor)**.
  
![](adding-a-view/_static/image10.png)   
  
En el **especificar nombre para el elemento** diálogo cuadro, escriba *bienvenida*y, a continuación, haga clic en **Aceptar**.   
  
En el **seleccionar una página de diseño** cuadro de diálogo, acepte el valor predeterminado  **\_Layout.cshtml** y haga clic en **Aceptar**.  
  
![](adding-a-view/_static/image11.png)   

El *MvcMovie\Views\HelloWorld\Welcome.cshtml* se crea el archivo.

Reemplace el marcado en el *Welcome.cshtml* archivo. Se creará un bucle que dice &quot;Hello&quot; tantas veces como el usuario indica que debería. La sección completa *Welcome.cshtml* archivo se muestra a continuación.

[!code-cshtml[Main](adding-a-view/samples/sample9.cshtml)]

Ejecute la aplicación y vaya a la dirección URL siguiente:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Ahora es procedente de la dirección URL y se pasa el controlador con datos la [enlazador de modelos](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx). El controlador empaqueta los datos en un `ViewBag` objeto y pasa ese objeto a la vista. La vista, a continuación, muestra los datos como HTML al usuario.

![](adding-a-view/_static/image12.png)

En el ejemplo anterior, hemos usado un `ViewBag` objeto que se va a pasar datos desde el controlador a una vista. Más adelante en el tutorial usaremos un modelo de vista para pasar datos de un controlador a una vista. El enfoque de modelo de vista para pasar datos es generalmente mucho más preferible sobre el enfoque de contenedor de vista. Vea la entrada de blog [V fuertemente tipados las vistas dinámicas](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) para obtener más información. 

Bueno, que era un tipo de un &quot;M&quot; para el modelo, pero no el tipo de base de datos. Vamos a aprovechar lo que hemos aprendido para crear una base de datos de películas.

> [!div class="step-by-step"]
> [Anterior](adding-a-controller.md)
> [Siguiente](adding-a-model.md)
