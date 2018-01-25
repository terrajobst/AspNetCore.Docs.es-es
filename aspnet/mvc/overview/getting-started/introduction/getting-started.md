---
uid: mvc/overview/getting-started/introduction/getting-started
title: "Introducción a ASP.NET MVC 5 | Documentos de Microsoft"
author: Rick-Anderson
description: "Nota: Una versión actualizada de este tutorial está disponible aquí con Visual Studio 2015. El nuevo tutorial usa ASP.NET Core MVC 6, que proporciona muchas improvem..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 1616b238612fa9f519418f583c40a46b9d81d8ce
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-aspnet-mvc-5"></a>Introducción a ASP.NET MVC 5
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[consider RP](../../../../includes/razor.md)]

 
 Este tutorial le enseñará los aspectos básicos de la creación de una aplicación web de ASP.NET MVC 5 con [2017 de Visual Studio](https://www.visualstudio.com/). Origen final para el tutorial se encuentra en [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)
 
 
 Este tutorial se escribió por [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) ) , y [Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )
 
 Necesita una cuenta de Azure para implementar esta aplicación en Azure:
 
 - También puede [abrir una cuenta de Azure de forma gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -obtendrá créditos puede usar para probar los servicios de Azure de pagados e incluso después de que se utilizan hasta puede mantener la cuenta y libre de usar los servicios de Azure.
 - También puede [activar las ventajas de suscriptor MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -su suscripción a MSDN ofrece créditos cada mes que puede usar para los servicios de Azure de pagados.


## <a name="getting-started"></a>Introducción

Empiece por instalar y ejecutar [2017 de Visual Studio](https://www.visualstudio.com/).

Visual Studio es un entorno de desarrollo integrado o IDE. Al igual que usa Microsoft Word para escribir documentos, deberá usar un IDE para crear aplicaciones. En Visual Studio, hay una lista en la parte inferior muestra distintas opciones disponibles para usted. También hay un menú que proporciona otra manera de realizar las tareas en el IDE. (Por ejemplo, en lugar de seleccionar **nuevo proyecto** desde el **iniciar** página, puede usar el menú y seleccione **archivo** &gt; **denuevoproyecto**.)

   
![](getting-started/_static/image1.png)  
 

## <a name="creating-your-first-application"></a>Crear su primera aplicación

Haga clic en **nuevo proyecto**, a continuación, seleccione Visual C# a la izquierda, a continuación, **Web** y, a continuación, seleccione **aplicación Web de ASP.NET (.NET Framework)**. Denomine el proyecto "MvcMovie" y, a continuación, haga clic en **Aceptar**.

![](getting-started/_static/image2.png)

En el **nuevo proyecto ASP.NET** cuadro de diálogo, haga clic en **MVC** y, a continuación, haga clic en **Aceptar**.

![](getting-started/_static/image3.png)

Visual Studio usa una plantilla predeterminada para el proyecto de MVC de ASP.NET que acaba de crear, por lo que tendrá una aplicación que funciona en este momento sin hacer nada. Se trata de un simple "Hola a todos" proyecto y, de un buen lugar para iniciar la aplicación.

![](getting-started/_static/image4.png)

Haga clic en F5 para iniciar la depuración. F5 hace que Visual Studio iniciar [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) y ejecutar la aplicación web. Visual Studio, a continuación, inicia un explorador y abre la página principal de la aplicación. Observe que la barra de direcciones del explorador indica `localhost:port#` y no algo como `example.com`. Esto es así porque `localhost` siempre apunta a su propio equipo local, que en este caso, se ejecuta la aplicación que acaba de crear. Cuando Visual Studio se ejecuta un proyecto web, se usa un puerto aleatorio para el servidor web. En la imagen siguiente, el número de puerto es 1234. Al ejecutar la aplicación, verá un número de puerto diferente.

![](getting-started/_static/image5.png)

Desde el cuadro de esta plantilla predeterminada proporciona páginas de inicio, póngase en contacto con y sobre. No muestra la imagen anterior del **inicio**, **sobre** y **póngase en contacto con** vínculos. Según el tamaño de la ventana del explorador, tendrá que hacer clic en el icono de navegación para ver estos vínculos.

![](getting-started/_static/image6.png)  

La aplicación también proporciona soporte técnico para registrar e iniciar sesión. El paso siguiente es cambiar el funcionamiento de esta aplicación y aprender un poco sobre ASP.NET MVC. Cierre la aplicación de ASP.NET MVC y vamos a cambiar parte del código.

Para obtener una lista de tutoriales actuales, vea [MVC artículos recomendado](../mvc-learning-sequence.md).

## <a name="see-this-app-running-on-azure"></a>Vea esta aplicación se ejecuta en Azure

¿Desea ver el sitio terminado que se ejecuta como una aplicación web en directo? Puede implementar una versión completa de la aplicación en su cuenta de Azure, simplemente haga clic en el botón siguiente.

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

Necesita una cuenta de Azure para implementar esta solución en Azure. Si no dispone de una cuenta, tiene las siguientes opciones:

- [Abrir una cuenta de Azure de forma gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -obtendrá créditos puede usar para probar los servicios de Azure de pagados e incluso después de que se utilizan hasta puede mantener la cuenta y libre de usar los servicios de Azure.
- [Activar las ventajas de suscriptor MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -su suscripción a MSDN ofrece créditos cada mes que puede usar para los servicios de Azure de pagados.

>[!div class="step-by-step"]
[Siguiente](adding-a-controller.md)
