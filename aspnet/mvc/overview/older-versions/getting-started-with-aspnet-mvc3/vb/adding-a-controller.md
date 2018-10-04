---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
title: Agregar un controlador (VB) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 741259e1-54ac-4f71-b4e8-2bd5560bb950
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: b38f3c4051af426e471568b8a25a471986f07209
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576239"
---
<a name="adding-a-controller-vb"></a>Agregar un controlador (VB)
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
> Un proyecto de Visual Web Developer con código fuente VB.NET está disponible como acompañamiento de este tema. [Descargue la versión VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si lo prefiere C#, cambie a la [C# versión](../cs/adding-a-controller.md) de este tutorial.


MVC es el acrónimo *model-view-controller*. MVC es un patrón para desarrollar aplicaciones de forma que cada parte tiene una responsabilidad independiente:

- Modelo: Los datos de la aplicación.
- Vistas: Los archivos de plantilla de la aplicación usará para generar respuestas HTML de forma dinámica.
- Controladores: Las clases que controlan las solicitudes entrantes de dirección URL a la aplicación, recuperar datos del modelo y, a continuación, especifique las plantillas de vista que representan una respuesta al cliente.

Se va a cubrir todos estos conceptos en este tutorial y le enseñaremos a usarlos para crear una aplicación.

Crear un nuevo controlador haciendo clic con el *controladores* carpeta en **el Explorador de soluciones** y, a continuación, seleccione **Agregar controlador**.

[![AddController](adding-a-controller/_static/image2.png "AddController")](adding-a-controller/_static/image1.png)

Asigne nombre al nuevo controlador &quot;HelloWorldController&quot; y haga clic en **agregar**.

[![2AddEmptyController](adding-a-controller/_static/image4.png "2AddEmptyController")](adding-a-controller/_static/image3.png)

Tenga en cuenta en **el Explorador de soluciones** a la derecha que se creó automáticamente un nuevo archivo denominado *HelloWorldController.cs* y que el archivo está abierto en el IDE.

Dentro de la nueva `public class HelloWorldController` bloquear, cree dos nuevos métodos que tienen un aspecto similar al código siguiente. Usaremos una cadena HTML directamente desde el controlador como un ejemplo.

[!code-vb[Main](adding-a-controller/samples/sample1.vb)]

El controlador se denomina `HelloWorldController` y se llama al nuevo método `Index`. Ejecute la aplicación (presione F5 o CTRL+F5). Una vez que haya iniciado el explorador, anexe &quot;HelloWorld&quot; a la ruta de acceso en la barra de direcciones. (En mi equipo, tiene `http://localhost:43246/HelloWorld`) tendrá un aspecto similar a la siguiente captura de pantalla del explorador. En el método anterior, el código devuelve una cadena directamente. Le dijimos que el sistema solo se devuelva algo de HTML, y así ha sido!

![](adding-a-controller/_static/image5.png)

ASP.NET MVC invoca las clases de controlador diferente (y los métodos de acción diferentes dentro de ellos) según la dirección URL entrante. La lógica de asignación predeterminada utilizada por ASP.NET MVC usa un formato similar al siguiente para controlar qué código se invoca:

`/[Controller]/[ActionName]/[Parameters]`

La primera parte de la dirección URL determina la clase de controlador para ejecutar. Por lo tanto */HelloWorld* se asigna a la `HelloWorldController` clase. La segunda parte de la dirección URL determina el método de acción en la clase que se ejecutará. Por lo tanto */HelloWorld/Index* provocaría el `Index` método de la `HelloWorldController` clase que se ejecutará. Tenga en cuenta que sólo teníamos que visite */HelloWorld* anteriormente y el `Index` usó el método de forma predeterminada. Esto es porque un método denominado `Index` es el método predeterminado que se llamará en un controlador si no se especifica explícitamente.

Vaya a `http://localhost:xxxx/HelloWorld/Welcome`. El `Welcome` método se ejecuta y devuelve la cadena &quot;este es el método de acción bienvenida... &quot;. La asignación de MVC predeterminada es `/[Controller]/[ActionName]/[Parameters]`. Para esta dirección URL, el controlador es `HelloWorld` y `Welcome` es el método. No hemos usado el `[Parameters]` forma parte de la dirección URL todavía.

![](adding-a-controller/_static/image6.png)

Vamos a modificar el ejemplo ligeramente para que nos podemos pasar cierta información del parámetro en desde la dirección URL al controlador (por ejemplo, */HelloWorld/Welcome? nombre = Scott&amp;numtimes = 4*). Cambiar su `Welcome` método para incluir dos parámetros, como se muestra a continuación. Tenga en cuenta que hemos usado la característica de parámetro opcional de VB para indicar que el `numTimes` parámetro, de forma predeterminada en 1 si se pasa ningún valor para ese parámetro.

[!code-vb[Main](adding-a-controller/samples/sample2.vb)]

Ejecute la aplicación y vaya a `http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4` **.** Puede probar valores diferentes para `name` y `numtimes`. El sistema asigna automáticamente los parámetros con nombre de la cadena de consulta en la barra de direcciones para los parámetros del método.

![](adding-a-controller/_static/image7.png)

En estos ejemplos de ambos el controlador ha realizado la parte de VC de MVC, que es el trabajo de vista y controlador. El controlador devuelve HTML directamente. Normalmente no queremos que los controles devuelvan HTML directamente, porque resulta muy complicado de código. En su lugar, vamos a usar normalmente un archivo de plantilla de vista independiente para ayudar a generar la respuesta HTML. Veamos cómo podemos hacer esto.

> [!div class="step-by-step"]
> [Anterior](intro-to-aspnet-mvc-3.md)
> [Siguiente](adding-a-view.md)
