---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
title: "Cómo agregar un controlador (VB) | Documentos de Microsoft"
author: Rick-Anderson
description: "Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 741259e1-54ac-4f71-b4e8-2bd5560bb950
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 74113d76a74b1da27a7f9a33a13038a0c36ad036
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-controller-vb"></a>Cómo agregar un controlador (VB)
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio. Antes de empezar, asegúrese de que ha instalado los requisitos previos descritos a continuación. Puede instalar todas ellas haciendo clic en el siguiente vínculo: [instalador de plataforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar por separado los requisitos previos mediante los siguientes vínculos:
> 
> - [Requisitos previos visuales Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(se admiten en tiempo de ejecución + herramientas)
> 
> Si usa Visual Studio 2010 en lugar de Visual Web Developer 2010, instale los requisitos previos, haga clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un proyecto de Visual Web Developer con el código fuente VB.NET está disponible como acompañamiento de este tema. [Descargue la versión VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si lo prefiere C#, cambie a la [versión de C#](../cs/adding-a-controller.md) de este tutorial.


MVC es el acrónimo *model-view-controller*. MVC es un modelo para desarrollar aplicaciones de forma que cada parte tiene una responsabilidad independiente:

- : Los datos del modelo Para la aplicación.
- Vistas: Los archivos de plantilla de la aplicación utilizará para generar dinámicamente las respuestas HTML.
- Controladores: Las clases que controlan las solicitudes entrantes de dirección URL a la aplicación, recuperar datos del modelo y, a continuación, especificar plantillas de vista que representan una respuesta al cliente.

Comenzaremos ser que cubren todos estos conceptos en este tutorial y muestran cómo utilizarlas para crear una aplicación.

Crear un nuevo controlador haciendo clic en el *controladores* carpeta **el Explorador de soluciones** y, a continuación, seleccione **Agregar controlador**.

[![AddController](adding-a-controller/_static/image2.png "AddController")](adding-a-controller/_static/image1.png)

El nombre de su nuevo controlador &quot;HelloWorldController&quot; y haga clic en **agregar**.

[![2AddEmptyController](adding-a-controller/_static/image4.png "2AddEmptyController")](adding-a-controller/_static/image3.png)

Observe que en **el Explorador de soluciones** a la derecha que se ha creado un nuevo archivo automáticamente con el nombre *HelloWorldController.cs* y que el archivo está abierto en el IDE.

Dentro de la nueva `public class HelloWorldController` bloquear, cree dos nuevos métodos que tengan un aspecto similar al código siguiente. Se tendrá que volver una cadena HTML directamente desde el controlador como un ejemplo.

[!code-vb[Main](adding-a-controller/samples/sample1.vb)]

El controlador se denomina `HelloWorldController` y se llama al nuevo método `Index`. Ejecute la aplicación (presione F5 o CTRL+F5). Una vez que el explorador se ha iniciado, anexar &quot;HelloWorld&quot; a la ruta de acceso en la barra de direcciones. (En mi equipo, tiene `http://localhost:43246/HelloWorld`) el explorador será similar a la captura de pantalla siguiente. En el método anterior, el código devuelve una cadena directamente. Le dijimos el sistema para devolver algo de HTML y así ha sido!

![](adding-a-controller/_static/image5.png)

ASP.NET MVC invoca las clases de controlador diferente (y métodos de acción diferentes dentro de ellas) dependiendo de la dirección URL entrante. La lógica de asignación predeterminada usa ASP.NET MVC utiliza un formato similar al siguiente para controlar qué código se invoca:

`/[Controller]/[ActionName]/[Parameters]`

La primera parte de la dirección URL determina la clase de controlador que se ejecutará. Por lo que */HelloWorld* se asigna a la `HelloWorldController` clase. La segunda parte de la dirección URL determina el método de acción en la clase que se ejecutará. Por lo que */HelloWorld/índice* provocaría que el `Index` método de la `HelloWorldController` clase que se ejecutará. Tenga en cuenta que sólo tuvimos que visite */HelloWorld* anteriormente y el `Index` usó el método de forma predeterminada. Esto es porque un método denominado `Index` es el método predeterminado que se llamará en un controlador si no se especifica explícitamente.

Vaya a `http://localhost:xxxx/HelloWorld/Welcome`. El `Welcome` método se ejecuta y devuelve la cadena &quot;éste es el método de acción bienvenida... &quot;. La asignación de MVC predeterminada es `/[Controller]/[ActionName]/[Parameters]`. Para esta dirección URL, el controlador es `HelloWorld` y `Welcome` es el método. No hemos usado la `[Parameters]` forma parte de la dirección URL todavía.

![](adding-a-controller/_static/image6.png)

Vamos a modificar el ejemplo ligeramente para que para pasar información de parámetro en desde la dirección URL para el controlador (por ejemplo, */HelloWorld/bienvenida? name = Scott&amp;numtimes = 4*). Cambiar el `Welcome` método debe incluir dos parámetros, tal y como se muestra a continuación. Tenga en cuenta que hemos usado la característica de parámetro opcional de VB para indicar que el `numTimes` parámetro, de forma predeterminada en 1 si se pasa ningún valor para ese parámetro.

[!code-vb[Main](adding-a-controller/samples/sample2.vb)]

Ejecute la aplicación y vaya a `http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4` **.** Puede probar valores diferentes para `name` y `numtimes`. El sistema asigna automáticamente los parámetros con nombre de la cadena de consulta en la barra de direcciones a los parámetros del método.

![](adding-a-controller/_static/image7.png)

En estos ejemplos de ambos el controlador ha realizado la parte de VC de MVC, que es el trabajo de vista y controlador. El controlador devuelve HTML directamente. Normalmente no es aconsejable controladores devolver HTML directamente, desde que se vuelve muy complicada al código. En su lugar, vamos a usar normalmente un archivo de plantilla de vista independiente para ayudar a generar la respuesta HTML. Echemos un vistazo a cómo podemos hacer esto.

>[!div class="step-by-step"]
[Anterior](intro-to-aspnet-mvc-3.md)
[Siguiente](adding-a-view.md)
