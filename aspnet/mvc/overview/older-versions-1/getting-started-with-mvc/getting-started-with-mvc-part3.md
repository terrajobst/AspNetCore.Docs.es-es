---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
title: Agregar una vista | Documentos de Microsoft
author: shanselman
description: Se trata de un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Crear una aplicación web simple que lee y escribe desde una base de datos.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: e8f1515c-c277-47ff-a23e-224118f13f02
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
msc.type: authoredcontent
ms.openlocfilehash: 978d7980274c072ed559b54ed69ab86245b6c5a7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-view"></a>Agregar una vista
====================
por [Scott Hanselman](https://github.com/shanselman)

> Se trata de un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Se creará una aplicación web simple que lee y escribe desde una base de datos. Visite la [centro de aprendizaje de ASP.NET MVC](../../../index.md) para buscar otros ASP.NET MVC, tutoriales y ejemplos.


En esta sección, vamos a ver cómo podemos tenemos nuestra clase HelloWorldController usar un archivo de plantilla de la vista para encapsular limpiamente generar respuestas HTML a un cliente.

Puede empezar con una plantilla de vista con el método de índice. El método se denomina índice y se encuentra en la HelloWorldController. Actualmente, el método Index() devuelve una cadena con un mensaje que está codificada en la clase de controlador.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample1.cs)]

Vamos a cambiar ahora el método Index en su lugar este aspecto:

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample2.cs)]

Agreguemos ahora una plantilla de vista a nuestro proyecto que podemos usar nuestro método Index(). Para ello, haga clic en con el mouse en algún lugar en el medio el método de índice y haga clic en Agregar vista...

![imagen](getting-started-with-mvc-part3/_static/image1.png)

Se abrirá el cuadro de diálogo "Agregar vista" que nos proporciona algunas opciones para definir cómo desea crear una plantilla de vista que se puede usar el método de índice. Por ahora, no cambia nada y simplemente haga clic en el botón Agregar.

[![Agregar cuadro de diálogo de vista](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)

Después de hacer clic en Agregar, una carpeta nueva y un nuevo archivo aparecerá en la carpeta de soluciones, tal como se muestra aquí. Ahora tengo una carpeta HelloWorld en vistas y un archivo Index.aspx dentro de esa carpeta.

[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)

El nuevo archivo de índice también está ya abierto y listo para su edición. Agregar texto en la primera &lt;h2&gt;índice&lt;/H2&gt; como "Hola mundo".

[!code-html[Main](getting-started-with-mvc-part3/samples/sample3.html)]

Ejecute la aplicación y visite [ `http://localhost:xx/HelloWorld` ](http://localhostxx) nuevo en el explorador. El método Index en nuestro controlador en este ejemplo no realiza ningún trabajo, pero llamó a "devuelto View()", que indica que desea usar un archivo de plantilla de vista para presentar una respuesta al cliente. Dado que no se ha especificado explícitamente el nombre del archivo de plantilla de vista para usar, ASP.NET MVC usado de forma predeterminada el archivo de vista Index.aspx dentro de la carpeta \Views\HelloWorld. Ahora vemos que la cadena que se pueden modificar en la vista.

[![Índice - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)

Parece bastante bueno. Sin embargo, tenga en cuenta que título del explorador dice "Index" y el título grande en la página dice "Mi aplicación MVC". Cambiemos los.

### <a name="changing-views-and-master-pages"></a>Cambiar vistas y las páginas maestras

En primer lugar, vamos a cambiar el texto "Mi aplicación MVC". Que el texto se comparte y aparece en cada página. Aparece realmente en un único lugar en el código, aunque se encuentre en todas las páginas en nuestra aplicación. Vaya a la carpeta /Views/Shared en el Explorador de soluciones y abra el archivo Site.Master. Este archivo se denomina una página maestra y es el shell"compartido" que utilizan el resto de páginas.

Tenga en cuenta algún texto que dice ContentPlaceholder "MainContent" en este archivo.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample4.aspx)]

Ese marcador de posición es donde todas las páginas que cree se mostrarán, "ajustadas" en la página maestra. Pruebe a cambiar el título, a continuación, ejecutar la aplicación y visite varias páginas. Observará que el uno cambio aparece en varias páginas.

[!code-html[Main](getting-started-with-mvc-part3/samples/sample5.html)]

Ahora cada página tendrá el encabezado principal - será H1 - de "Mi aplicación de película MVC". Que controla el texto en blanco en la parte superior hay que se comparte entre todas las páginas.

Este es el Site.Master en su totalidad con nuestro ha cambiado el título:

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample6.aspx)]

Ahora, vamos a cambiar el título de la página de índice.

Open /HelloWorld/Index.aspx. Hay dos que se van a cambiar. En primer lugar, el título que aparece en el título del explorador, el encabezado secundario - que también es H2. Pondré ellos cada cambia ligeramente diferente para que pueda ver qué cantidad de código que parte de la aplicación.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample7.aspx)]

Ejecutar la aplicación y visite /Movies. Tenga en cuenta que el título del explorador, el encabezado principal y los encabezados secundarios han cambiado. Es fácil realizar grandes cambios en su aplicación con pequeños cambios en la vista.

[![Lista de películas - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)

Nuestro poco de "datos" (en este caso el "¡Hello World!" mensaje) era difícil aunque codificada. Tenemos V (vistas) y tenemos C (controladores), pero aún no M (modelo). En breve guiaremos por el proceso de crear una base de datos y recuperar los datos del modelo del mismo.

## <a name="passing-a-viewmodel"></a>Pasar un modelo de vista

Antes de que se vaya a una base de datos y hablar acerca de los modelos, sin embargo, primero se explicará "ViewModels." Se trata de objetos que representan lo que requiere una plantilla de vista para representar una respuesta HTML a un cliente. Normalmente, se crean y pasa una clase de controlador a una plantilla de vista y solo debe contener los datos que requiere la plantilla de vista - y nada más.

Anteriormente con nuestro ejemplo HelloWorld, el método de acción Welcome() tardó un nombre y un parámetro de numTimes y pasar los resultados en el explorador. En lugar de que el controlador continúe representar esta respuesta directamente, vamos a hacer en su lugar una clase pequeña para contener los datos y, a continuación, pasar a través a una plantilla de vista para representar la respuesta HTML usando de nuevo. De esta forma el controlador está relacionado con una cosa y la plantilla de vista de otra, lo que nos permite mantener "separación clara de intereses" dentro de nuestra aplicación.

Vuelva al archivo HelloWorldController.cs y agregue una nueva clase de "WelcomeViewModel" y cambiar el método de bienvenida en el controlador. Este es el HelloWorldController.cs completa con la nueva clase en el mismo archivo.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample8.cs)]

Aunque resulta en varias líneas, el método de bienvenida es en realidad sólo dos instrucciones de código. La primera instrucción empaqueta nuestras dos parámetros en un objeto de modelo de vista y el segundo pasa el objeto resultante en la vista.

Ahora necesitamos una plantilla de vista de página principal. Haga clic en el método principal y seleccione Agregar vista. Esta vez, se deberá comprobar "Crear una vista fuertemente tipada" y seleccione nuestra clase WelcomeViewModel en la lista desplegable. Esta nueva vista solo conocen WelcomeViewModels y ningún otro tipo de objetos.

> *Nota: Necesitará ha compilado una vez después de agregar el WelcomeViewModel para que aparezcan en la lista desplegable.*


Este es el aspecto de su cuadro de diálogo Agregar vista. Haga clic en el botón Agregar. ![Agregar que vista en un círculo](getting-started-with-mvc-part3/_static/image10.png)

Agregue este código bajo el &lt;h2&gt; en su Welcome.aspx nuevo. Se podrá realizar un bucle y le presentamos tantas veces como el usuario indica que se debe!

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample9.aspx)]

Además, tenga en cuenta mientras escribe que ya le dijimos a esta vista sobre el WelcomeViewModel (están casados, recuerde?) que se obtendrá Intellisense útil cada vez que se hacen referencia a nuestro objeto de modelo como se ve en la captura de pantalla siguiente:

[![Código fuente de NumTime](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)

Ejecute la aplicación y visite `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` nuevo. Ahora estamos dando datos desde la dirección URL, se pasa a nuestro controlador automáticamente, nuestro controlador empaqueta los datos en un modelo de vista y pasa ese objeto en la vista. La vista que muestra los datos como HTML al usuario.

[![Bienvenida - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)

Bueno, era un tipo de una "M" para el modelo, pero no el tipo de base de datos. ¡Eche lo que hemos aprendido y crear una base de datos de películas.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part2.md)
> [Siguiente](getting-started-with-mvc-part4.md)
