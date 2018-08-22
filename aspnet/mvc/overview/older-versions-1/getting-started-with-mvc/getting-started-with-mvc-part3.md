---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
title: Agregar una vista | Microsoft Docs
author: shanselman
description: Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Cree una aplicación web simple que lee y escribe desde una base de datos.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: e8f1515c-c277-47ff-a23e-224118f13f02
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
msc.type: authoredcontent
ms.openlocfilehash: f55e558dd056e86bdd2310894959aef02a9d8de2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838774"
---
<a name="adding-a-view"></a>Agregar una vista
====================
por [Scott Hanselman](https://github.com/shanselman)

> Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Creará una aplicación web simple que lee y escribe desde una base de datos. Visite el [centro de aprendizaje de ASP.NET MVC](../../../index.md) para buscar otros ASP.NET MVC, tutoriales y ejemplos.


En esta sección vamos a ver cómo podemos tenemos nuestra clase HelloWorldController usar un archivo de plantilla de vista para encapsular limpiamente generar respuestas HTML a un cliente.

Comencemos con una plantilla de vista con el método de índice. El método se denomina índice y se encuentra en la HelloWorldController. Actualmente, nuestro método Index() devuelve una cadena con un mensaje que está codificado dentro de la clase de controlador.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample1.cs)]

Ahora cambiemos el método Index en su lugar este aspecto:

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample2.cs)]

Ahora agreguemos una plantilla de vista a nuestro proyecto que podemos usar nuestro método Index(). Para ello, haga doble clic con el mouse en algún lugar en medio del método Index y haga clic en Agregar vista...

![imagen](getting-started-with-mvc-part3/_static/image1.png)

Se abrirá el cuadro de diálogo "Agregar vista", lo que nos proporciona algunas opciones para ver cómo queremos crear una plantilla de vista que se puede usar nuestro método Index. Por ahora, no cambia nada y, simplemente haga clic en el botón Agregar.

[![Agregar cuadro de diálogo de vista](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)

Tras hacer clic en Agregar, una nueva carpeta y un nuevo archivo se mostrarán en la carpeta de soluciones, tal como se muestra aquí. Ahora tengo una carpeta HelloWorld en vistas y un archivo Index.aspx dentro de esa carpeta.

[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)

El nuevo archivo de índice también está ya abierto y listo para su edición. Agregue texto debajo del primer &lt;h2&gt;índice&lt;/H2&gt; como "Hello World".

[!code-html[Main](getting-started-with-mvc-part3/samples/sample3.html)]

Ejecute la aplicación y visite [ `http://localhost:xx/HelloWorld` ](http://localhostxx) nuevo en el explorador. El método Index en nuestro controlador en este ejemplo no realiza ningún trabajo, pero llamó a "return View()", lo que indica que deseamos utilizar un archivo de plantilla de vista para presentar una respuesta al cliente. Dado que no se ha especificado explícitamente el nombre del archivo de plantilla de vista para usar, ASP.NET MVC usa de forma predeterminada el archivo de vista Index.aspx dentro de la carpeta \Views\HelloWorld. Ahora vemos que la cadena que se pueden modificar en nuestra vista.

[![Índice - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)

Parece bastante bueno. Sin embargo, tenga en cuenta que el título del explorador dice "Index" y el título en la página big dice "Mi aplicación MVC." Vamos a cambiar aquellos.

### <a name="changing-views-and-master-pages"></a>Cambiar vistas y páginas maestras

En primer lugar, vamos a cambiar el texto "Mi aplicación MVC." Que el texto se comparte y aparece en cada página. Aparece realmente en un único lugar en nuestro código, incluso aunque esté en cada página en nuestra aplicación. Vaya a la carpeta /Views/Shared en el Explorador de soluciones y abra el archivo Site.Master. Este archivo se llama a una página maestra y es el shell"compartido" que usan todas nuestras páginas.

Tenga en cuenta algún texto que dice "MainContent" ContentPlaceholder en este archivo.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample4.aspx)]

Marcador de posición es donde todas las páginas que crea se mostrarán, "encapsuladas" en la página maestra. Pruebe a cambiar el título, a continuación, ejecute la aplicación y visite varias páginas. Observará que el uno cambio aparece en varias páginas.

[!code-html[Main](getting-started-with-mvc-part3/samples/sample5.html)]

Ahora cada página tendrá el encabezado principal - es H1 - de "Mi aplicación de película MVC." Que controla el texto en blanco en la parte superior hay que se comparte entre todas las páginas.

Este es el Site.Master en su totalidad con el título modificado:

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample6.aspx)]

Ahora, vamos a cambiar el título de la página de índice.

Abra /HelloWorld/Index.aspx. Hay dos lugares a cambiar. En primer lugar, el título que aparece en el título del explorador y, a continuación, el encabezado secundario - que también está H2. Haré que ellos cada ligeramente diferente para que pueda ver qué fragmento de código cambia qué parte de la aplicación.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample7.aspx)]

Ejecute la aplicación y visite /Movies. Tenga en cuenta que el título del explorador, el encabezado principal y los encabezados secundarios han cambiado. Es fácil realizar grandes cambios en su aplicación con pequeños cambios en la vista.

[![Lista de películas - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)

Nuestra pequeña cantidad de "datos" (en este caso, "Hello World!" mensaje) fue difícil aunque codificada. Tenemos V (vistas) y tenemos C (controladores), pero aún no hay M (modelo). En breve analizaremos cómo crear una base de datos y recuperar los datos del modelo del mismo.

## <a name="passing-a-viewmodel"></a>Pasar una clase ViewModel

Antes de hablar acerca de los modelos y vaya a una base de datos, sin embargo, primero explicaremos "ViewModel". Estos son objetos que representan lo que requiere una plantilla de vista para presentar una respuesta HTML a un cliente. Normalmente se crean y pasa una clase de controlador a una plantilla de vista y solo debe contener los datos que necesita la plantilla de vista - y nada más.

Anteriormente con nuestro ejemplo HelloWorld, nuestro método de acción Welcome() tardó un nombre y un parámetro numTimes y pasar los resultados al explorador. En lugar de que el controlador continúan generándose directamente esta respuesta, vamos a hacer en su lugar una clase pequeña para contener los datos y, a continuación, pasar a través de a una plantilla de vista para representar vuelta la respuesta HTML con él. De este modo el controlador se ocupa de una cosa y la plantilla de vista de otra, lo que nos permite mantener "separación clara de intereses" dentro de nuestra aplicación.

Vuelva al archivo HelloWorldController.cs y agregue una nueva clase "WelcomeViewModel" y cambie el método principal dentro del controlador. Este es el HelloWorldController.cs completa con la nueva clase en el mismo archivo.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample8.cs)]

Incluso aunque esté en varias líneas, nuestro método bienvenida realmente es solo dos instrucciones de código. La primera instrucción empaqueta nuestros dos parámetros en un objeto ViewModel y el segundo pasa el objeto resultante en la vista.

Ahora tenemos una plantilla de vista principal. Haga clic con el botón derecho en el método principal y seleccione Agregar vista. Esta vez, se deberá comprobar "Crear una vista fuertemente tipada" y seleccione nuestra clase WelcomeViewModel en la lista desplegable. Esta nueva vista sabrá solo sobre WelcomeViewModels y ningún otro tipo de objetos.

> *Nota: Deberá ha compilado una vez después de agregar su WelcomeViewModel para que aparezcan en la lista desplegable.*


Este es el aspecto que debería su cuadro de diálogo Agregar vista. Haga clic en el botón Agregar. ![Agregar que vista en un círculo](getting-started-with-mvc-part3/_static/image10.png)

Agregue este código bajo el &lt;h2&gt; en su nuevo Welcome.aspx. Crearemos realizar un bucle y saluda tantas veces como el usuario dice que deberíamos!

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample9.aspx)]

Además, tenga en cuenta mientras escribe que dado que indicamos a esta vista sobre el WelcomeViewModel (están casados, recuerde?) que obtenemos útil Intellisense cada vez que se hace referencia el objeto del modelo como se muestra en la captura de pantalla siguiente:

[![Código fuente NumTime](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)

Ejecute la aplicación y visite `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` nuevo. Ahora le estamos redirigiendo los datos de la dirección URL, se pasa automáticamente a nuestro controlador, nuestro controlador empaqueta los datos en una clase ViewModel y pasa ese objeto en nuestra vista. La vista que muestra los datos como HTML al usuario.

[![Página principal - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)

Bueno, eso fue una especie de "M" para el modelo, pero no el tipo de base de datos. Veamos lo que hemos aprendido y crear una base de datos de películas.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part2.md)
> [Siguiente](getting-started-with-mvc-part4.md)
