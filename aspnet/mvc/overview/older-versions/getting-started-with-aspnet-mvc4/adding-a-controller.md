---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
title: "Cómo agregar un controlador | Documentos de Microsoft"
author: Rick-Anderson
description: "Nota: Una versión actualizada de este tutorial está disponible aquí que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro y mucho más fácil de seguir y demostraciones..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 0267d31c-892f-49a1-9e7a-3ae8cc12b2ca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 69af91401e51470fbc0b67103345325201b06723
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-controller"></a>Agregar un controlador
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Hay disponible una versión actualizada de este tutorial [aquí](../../getting-started/introduction/getting-started.md) que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro y mucho más fácil de seguir y se muestra más características.


MVC es el acrónimo *model-view-controller*. MVC es un modelo para desarrollar aplicaciones que están bien diseñada, comprobable y fáciles de mantener. Las aplicaciones basadas en MVC contienen:

- **M** odels: las clases que representan los datos de la aplicación y que usan una lógica de validación para aplicar las reglas de negocios para esos datos.
- **V** iews: archivos de plantilla que la aplicación usa para generar dinámicamente las respuestas HTML.
- **C** ontrollers: las clases que controlan las solicitudes entrantes de explorador, recuperar datos del modelo y, a continuación, especificar plantillas de vista que devuelven una respuesta al explorador.

Comenzaremos ser que cubren todos estos conceptos en esta serie de tutoriales y muestran cómo utilizarlas para crear una aplicación.

Vamos a empezar a crear una clase de controlador. En **el Explorador de soluciones**, haga clic en el *controladores* carpeta y, a continuación, seleccione **Agregar controlador**.

![](adding-a-controller/_static/image1.png)

El nombre de su nuevo controlador &quot;HelloWorldController&quot;. Deje la plantilla predeterminada como **controlador MVC vacío** y haga clic en **agregar**.

![Agregar controlador](adding-a-controller/_static/image2.png)

Observe que en **el Explorador de soluciones** que un nuevo archivo se ha creado con nombre *HelloWorldController.cs*. El archivo está abierto en el IDE.

![](adding-a-controller/_static/image3.png)

Reemplace el contenido del archivo con el código siguiente.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Los métodos de controlador devolverá una cadena HTML como un ejemplo. El controlador se denomina `HelloWorldController` y el primer método anterior se denomina `Index`. Vamos a invocarlo desde un explorador. Ejecute la aplicación (presione F5 o CTRL+F5). En el explorador, anexar &quot;HelloWorld&quot; a la ruta de acceso en la barra de direcciones. (Por ejemplo, en la ilustración siguiente, su `http://localhost:1234/HelloWorld.`) la página en el explorador será similar a la captura de pantalla siguiente. En el método anterior, el código devuelve una cadena directamente. Indicar el sistema para devolver algo de HTML y así ha sido!

![](adding-a-controller/_static/image4.png)

ASP.NET MVC invoca las clases de controlador diferente (y métodos de acción diferentes dentro de ellas) dependiendo de la dirección URL entrante. La lógica de enrutamiento de dirección URL predeterminada utilizada por ASP.NET MVC utiliza un formato similar al siguiente para determinar qué código debe invocar:

`/[Controller]/[ActionName]/[Parameters]`

La primera parte de la dirección URL determina la clase de controlador que se ejecutará. Por lo que */HelloWorld* se asigna a la `HelloWorldController` clase. La segunda parte de la dirección URL determina el método de acción en la clase que se ejecutará. Por lo que */HelloWorld/índice* provocaría que el `Index` método de la `HelloWorldController` clase que se ejecutará. Tenga en cuenta que sólo tuvimos que vaya a */HelloWorld* y `Index` usó el método de forma predeterminada. Esto es porque un método denominado `Index` es el método predeterminado que se llamará en un controlador si no se especifica explícitamente.

Vaya a `http://localhost:xxxx/HelloWorld/Welcome`. El `Welcome` método se ejecuta y devuelve la cadena &quot;éste es el método de acción bienvenida... &quot;. La asignación de MVC predeterminada es `/[Controller]/[ActionName]/[Parameters]`. Para esta dirección URL, el controlador es `HelloWorld` y `Welcome` es el método de acción. Todavía no ha usado el elemento `[Parameters]` de la dirección URL.

![](adding-a-controller/_static/image5.png)

Vamos a modificar el ejemplo ligeramente para que pueda pasar cierta información de parámetro de la dirección URL para el controlador (por ejemplo, */HelloWorld/bienvenida? name = Scott&amp;numtimes = 4*). Cambiar el `Welcome` método debe incluir dos parámetros, tal y como se muestra a continuación. Tenga en cuenta que el código usa la característica de parámetro opcional de C# para indicar que el `numTimes` parámetro, de forma predeterminada en 1 si se pasa ningún valor para ese parámetro.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Ejecute la aplicación y busque la dirección URL de ejemplo (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. Puede probar valores diferentes para `name` y `numtimes` en la dirección URL. El [sistema de enlace de modelo de ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) asigna automáticamente los parámetros con nombre de la cadena de consulta en la barra de direcciones a los parámetros del método.

![](adding-a-controller/_static/image6.png)

En estos ejemplos, el controlador ha realizado la &quot;VC&quot; parte de MVC, es decir, el trabajo de vista y controlador. El controlador devuelve HTML directamente. Normalmente no desea devolver HTML directamente, desde que se vuelve muy complicada al código de controladores. En su lugar, vamos a usar normalmente un archivo de plantilla de vista independiente para ayudar a generar la respuesta HTML. Echemos un vistazo siguiente en cómo podemos hacer esto.

>[!div class="step-by-step"]
[Anterior](intro-to-aspnet-mvc-4.md)
[Siguiente](adding-a-view.md)
