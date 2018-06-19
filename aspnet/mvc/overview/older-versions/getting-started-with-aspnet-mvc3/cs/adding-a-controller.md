---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
title: Cómo agregar un controlador (C#) | Documentos de Microsoft
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express servicio Pack 1, qué i...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 0b8c56b5-fdf3-42dd-a866-98fbe0ab78a0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 963d3bbbadf408d7045c50bfd693069e4097d45d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868300"
---
<a name="adding-a-controller-c"></a>Cómo agregar un controlador (C#)
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


MVC es el acrónimo *model-view-controller*. MVC es un modelo para desarrollar aplicaciones que están bien diseñada y fáciles de mantener. Las aplicaciones basadas en MVC contienen:

- Controladores: Las clases que controlan las solicitudes entrantes a la aplicación, recuperar datos del modelo y, a continuación, especificar plantillas de vista que devuelven una respuesta al cliente.
- Modelos de clases que representan los datos de la aplicación y que usan una lógica de validación para aplicar las reglas de negocios para esos datos.
- Vistas: Archivos de plantilla que la aplicación usa para generar dinámicamente las respuestas HTML.

Comenzaremos ser que cubren todos estos conceptos en esta serie de tutoriales y muestran cómo utilizarlas para crear una aplicación.

Vamos a empezar a crear una clase de controlador. En **el Explorador de soluciones**, haga clic en el *controladores* carpeta y, a continuación, seleccione **Agregar controlador**.

[![](adding-a-controller/_static/image2.png)](adding-a-controller/_static/image1.png)

Nombre el nuevo controlador de "HelloWorldController". Deje la plantilla predeterminada como **controlador en blanco** y haga clic en **agregar**.

[![AddHelloWorldController](adding-a-controller/_static/image4.png)](adding-a-controller/_static/image3.png)

Observe que en **el Explorador de soluciones** que un nuevo archivo se ha creado con nombre *HelloWorldController.cs*. El archivo está abierto en el IDE.

![](adding-a-controller/_static/image5.png)

Dentro de la `public class HelloWorldController` de bloque, cree dos métodos que tengan un aspecto similar al código siguiente. El controlador devolverá una cadena HTML como un ejemplo.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

El controlador se denomina `HelloWorldController` y el primer método anterior se denomina `Index`. Vamos a invocarlo desde un explorador. Ejecute la aplicación (presione F5 o CTRL+F5). En el explorador, anexe "HelloWorld" a la ruta de acceso en la barra de direcciones. (Por ejemplo, en la ilustración siguiente, su `http://localhost:43246/HelloWorld.`) la página en el explorador será similar a la captura de pantalla siguiente. En el método anterior, el código devuelve una cadena directamente. Indicar el sistema para devolver algo de HTML y así ha sido!

![](adding-a-controller/_static/image6.png)

ASP.NET MVC invoca las clases de controlador diferente (y métodos de acción diferentes dentro de ellas) dependiendo de la dirección URL entrante. La lógica de asignación predeterminada usa ASP.NET MVC utiliza un formato similar al siguiente para determinar qué código debe invocar:

`/[Controller]/[ActionName]/[Parameters]`

La primera parte de la dirección URL determina la clase de controlador que se ejecutará. Por lo que */HelloWorld* se asigna a la `HelloWorldController` clase. La segunda parte de la dirección URL determina el método de acción en la clase que se ejecutará. Por lo que */HelloWorld/índice* provocaría que el `Index` método de la `HelloWorldController` clase que se ejecutará. Tenga en cuenta que sólo tuvimos que vaya a */HelloWorld* y `Index` usó el método de forma predeterminada. Esto es porque un método denominado `Index` es el método predeterminado que se llamará en un controlador si no se especifica explícitamente.

Vaya a `http://localhost:xxxx/HelloWorld/Welcome`. El método `Welcome` se ejecuta y devuelve la cadena "This is the Welcome action method..." (Este es el método de acción de bienvenida). La asignación de MVC predeterminada es `/[Controller]/[ActionName]/[Parameters]`. Para esta dirección URL, el controlador es `HelloWorld` y `Welcome` es el método de acción. Todavía no ha usado el elemento `[Parameters]` de la dirección URL.

![](adding-a-controller/_static/image7.png)

Vamos a modificar el ejemplo ligeramente para que pueda pasar cierta información de parámetro de la dirección URL para el controlador (por ejemplo, */HelloWorld/bienvenida? name = Scott&amp;numtimes = 4*). Cambiar el `Welcome` método debe incluir dos parámetros, tal y como se muestra a continuación. Tenga en cuenta que el código usa la característica de parámetro opcional de C# para indicar que el `numTimes` parámetro, de forma predeterminada en 1 si se pasa ningún valor para ese parámetro.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Ejecute la aplicación y busque la dirección URL de ejemplo (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. Puede probar valores diferentes para `name` y `numtimes` en la dirección URL. El sistema asigna automáticamente los parámetros con nombre de la cadena de consulta en la barra de direcciones a los parámetros del método.

![](adding-a-controller/_static/image8.png)

En estos ejemplos de ambos el controlador ha realizado la parte "VC" de MVC, es decir, el trabajo de vista y controlador. El controlador devuelve HTML directamente. Normalmente no desea devolver HTML directamente, desde que se vuelve muy complicada al código de controladores. En su lugar, vamos a usar normalmente un archivo de plantilla de vista independiente para ayudar a generar la respuesta HTML. Echemos un vistazo siguiente en cómo podemos hacer esto.

> [!div class="step-by-step"]
> [Anterior](intro-to-aspnet-mvc-3.md)
> [Siguiente](adding-a-view.md)
