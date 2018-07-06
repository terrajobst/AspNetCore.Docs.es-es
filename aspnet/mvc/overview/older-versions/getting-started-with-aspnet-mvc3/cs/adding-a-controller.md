---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
title: Agregar un controlador (C#) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express corresponde Pack 1, que creo...
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: 0b8c56b5-fdf3-42dd-a866-98fbe0ab78a0
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 0c13248960384e8d773f9c5ba3d927b6a8502938
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805864"
---
<a name="adding-a-controller-c"></a>Agregar un controlador (C#)
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Hay disponible una versión actualizada de este tutorial [aquí](../../../getting-started/introduction/getting-started.md) que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro y mucho más fácil de seguir y se muestran las características más.
> 
> 
> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio. Antes de empezar, asegúrese de que ha instalado los requisitos previos descritos a continuación. Puede instalar todos ellos haciendo clic en el siguiente vínculo: [instalador de plataforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar individualmente los requisitos previos mediante los vínculos siguientes:
> 
> - [Requisitos previos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tiempo de ejecución de herramientas de soporte técnico +)
> 
> Si usa Visual Studio 2010, en lugar de Visual Web Developer 2010, instale los requisitos previos, haga clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un proyecto de Visual Web Developer con código fuente de C# está disponible como acompañamiento de este tema. [Descargue la versión de C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si prefiere Visual Basic, cambie a la [versión de Visual Basic](../vb/intro-to-aspnet-mvc-3.md) de este tutorial.


MVC es el acrónimo *model-view-controller*. MVC es un patrón para desarrollar aplicaciones que están bien diseñadas y fáciles de mantener. Las aplicaciones basadas en MVC contienen:

- Controladores: Las clases que controlan las solicitudes entrantes a la aplicación, recuperar datos del modelo y, a continuación, especifique las plantillas de vista que devuelven una respuesta al cliente.
- Modelos: Las clases que representan los datos de la aplicación y que usan la lógica de validación para aplicar las reglas de negocios para que los datos.
- Vistas: Archivos de plantilla la aplicación usa para generar respuestas HTML de forma dinámica.

Se va a cubrir todos estos conceptos en esta serie de tutoriales y mostrarle cómo se usan para compilar una aplicación.

Comencemos por crear una clase de controlador. En **el Explorador de soluciones**, haga clic en el *controladores* carpeta y, a continuación, seleccione **Agregar controlador**.

[![](adding-a-controller/_static/image2.png)](adding-a-controller/_static/image1.png)

Asigne un nombre al nuevo controlador "HelloWorldController". Deje la plantilla predeterminada como **controlador vacío** y haga clic en **agregar**.

[![AddHelloWorldController](adding-a-controller/_static/image4.png)](adding-a-controller/_static/image3.png)

Tenga en cuenta en **el Explorador de soluciones** que un nuevo archivo se ha creado con nombre *HelloWorldController.cs*. El archivo está abierto en el IDE.

![](adding-a-controller/_static/image5.png)

Dentro de la `public class HelloWorldController` bloquear, cree dos métodos que tienen un aspecto similar al código siguiente. El controlador devolverá una cadena HTML como un ejemplo.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

El controlador se denomina `HelloWorldController` y el primer método anterior se denomina `Index`. Vamos a invocarlo desde un explorador. Ejecute la aplicación (presione F5 o CTRL+F5). En el explorador, anexe "HelloWorld" a la ruta de acceso en la barra de direcciones. (Por ejemplo, en la ilustración siguiente, su `http://localhost:43246/HelloWorld.`) la página en el explorador será similar a la captura de pantalla siguiente. En el método anterior, el código devuelve una cadena directamente. Dijo el sistema solo se devuelva algo de HTML, y así ha sido!

![](adding-a-controller/_static/image6.png)

ASP.NET MVC invoca las clases de controlador diferente (y los métodos de acción diferentes dentro de ellos) según la dirección URL entrante. La lógica de asignación predeterminada utilizada por ASP.NET MVC usa un formato similar al siguiente para determinar qué código debe invocar:

`/[Controller]/[ActionName]/[Parameters]`

La primera parte de la dirección URL determina la clase de controlador para ejecutar. Por lo tanto */HelloWorld* se asigna a la `HelloWorldController` clase. La segunda parte de la dirección URL determina el método de acción en la clase que se ejecutará. Por lo tanto */HelloWorld/Index* provocaría el `Index` método de la `HelloWorldController` clase que se ejecutará. Tenga en cuenta que sólo teníamos que vaya a */HelloWorld* y `Index` usó el método de forma predeterminada. Esto es porque un método denominado `Index` es el método predeterminado que se llamará en un controlador si no se especifica explícitamente.

Vaya a `http://localhost:xxxx/HelloWorld/Welcome`. El método `Welcome` se ejecuta y devuelve la cadena "This is the Welcome action method..." (Este es el método de acción de bienvenida). La asignación de MVC predeterminada es `/[Controller]/[ActionName]/[Parameters]`. Para esta dirección URL, el controlador es `HelloWorld` y `Welcome` es el método de acción. Todavía no ha usado el elemento `[Parameters]` de la dirección URL.

![](adding-a-controller/_static/image7.png)

Vamos a modificar el ejemplo ligeramente para que pueda pasar cierta información del parámetro de la dirección URL al controlador (por ejemplo, */HelloWorld/Welcome? nombre = Scott&amp;numtimes = 4*). Cambiar su `Welcome` método para incluir dos parámetros, como se muestra a continuación. Tenga en cuenta que el código usa la característica de parámetro opcional de C# para indicar que el `numTimes` parámetro, de forma predeterminada en 1 si se pasa ningún valor para ese parámetro.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Ejecute la aplicación y vaya a la dirección URL de ejemplo (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. Puede probar valores diferentes para `name` y `numtimes` en la dirección URL. El sistema asigna automáticamente los parámetros con nombre de la cadena de consulta en la barra de direcciones para los parámetros del método.

![](adding-a-controller/_static/image8.png)

En estos ejemplos de ambos el controlador ha realizado la parte "VC" de MVC, es decir, el trabajo de vista y controlador. El controlador devuelve HTML directamente. Normalmente no desea que los controles devuelvan HTML directamente, porque resulta muy complicado de código. En su lugar, vamos a usar normalmente un archivo de plantilla de vista independiente para ayudar a generar la respuesta HTML. Echemos un vistazo siguiente en ¿cómo podemos hacer esto.

> [!div class="step-by-step"]
> [Anterior](intro-to-aspnet-mvc-3.md)
> [Siguiente](adding-a-view.md)
