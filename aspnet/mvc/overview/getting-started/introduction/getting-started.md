---
uid: mvc/overview/getting-started/introduction/getting-started
title: Introducción a ASP.NET MVC 5 | Microsoft Docs
author: Rick-Anderson
description: 'Nota: Una versión actualizada de este tutorial está disponible aquí con Visual Studio 2015. El nuevo tutorial usa ASP.NET Core MVC 6, que proporciona muchas improvem...'
ms.author: riande
ms.date: 05/28/2015
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 8417be945e16ed99e655f628134c9190429f1d67
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837644"
---
<a name="getting-started-with-aspnet-mvc-5"></a>Introducción a ASP.NET MVC 5
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [consider RP](../../../../includes/razor.md)]

 Este tutorial le enseñará los aspectos básicos de la creación de una aplicación web de ASP.NET MVC 5 con [Visual Studio 2017](https://www.visualstudio.com/). Origen final para el tutorial se encuentra en [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)


 En este tutorial se escribió por [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) ) , y [Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )

 Necesita una cuenta de Azure para implementar esta aplicación en Azure:

 - También puede [abrir una cuenta de Azure de forma gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -obtenga créditos puede usar para probar los servicios de Azure de pago e incluso después de que se usan hasta, puede mantener la cuenta y usar servicios gratuitos de Azure.
 - También puede [activar las ventajas de suscriptor MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -su suscripción a MSDN le proporciona crédito todos los meses que puede usar para servicios de Azure de pago.


## <a name="getting-started"></a>Introducción

Comience por instalar y ejecutar [Visual Studio 2017](https://www.visualstudio.com/).

Visual Studio es un entorno de desarrollo integrado o IDE. Al igual que utiliza Microsoft Word para escribir documentos, usará un IDE para crear aplicaciones. En Visual Studio, hay una lista en la parte inferior muestra distintas opciones disponibles para usted. También hay un menú que proporciona otra manera de realizar las tareas en el IDE. (Por ejemplo, en lugar de seleccionar **nuevo proyecto** desde el **iniciar** página, puede usar el menú y seleccione **archivo** &gt; **denuevoproyecto**.)


![](getting-started/_static/image1.png)  


## <a name="creating-your-first-application"></a>Crear su primera aplicación

Haga clic en **nuevo proyecto**, a continuación, seleccione Visual C# a la izquierda y, a continuación, **Web** y, a continuación, seleccione **aplicación Web ASP.NET (.NET Framework)**. Nombre del proyecto "MvcMovie" y, a continuación, haga clic en **Aceptar**.

![](getting-started/_static/image2.png)

En el **nuevo proyecto ASP.NET** cuadro de diálogo, haga clic en **MVC** y, a continuación, haga clic en **Aceptar**.

![](getting-started/_static/image3.png)

Visual Studio usa una plantilla predeterminada para el proyecto de ASP.NET MVC que acaba de crear, por lo que tiene ahora una aplicación de trabajo sin hacer nada! Se trata de un simple "Hello World!" proyecto y, de un buen lugar para iniciar la aplicación.

![](getting-started/_static/image4.png)

Haga clic en F5 para iniciar la depuración. F5 hace que Visual Studio iniciar [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) y ejecutar la aplicación web. A continuación, Visual Studio inicia un explorador y abre la página principal de la aplicación. Tenga en cuenta que la barra de direcciones del explorador dice `localhost:port#` y no algo como `example.com`. Eso es porque `localhost` siempre apunta a su propio equipo local, que en este caso, se ejecuta la aplicación que acaba de crear. Cuando Visual Studio se ejecuta un proyecto web, se usa un puerto aleatorio para el servidor web. En la imagen siguiente, el número de puerto es 1234. Al ejecutar la aplicación, verá un número de puerto diferente.

![](getting-started/_static/image5.png)

Desde el comienzo esta plantilla predeterminada proporciona páginas principal, contactos y sobre. No se muestra la imagen anterior el **inicio**, **sobre** y **póngase en contacto con** vínculos. Según el tamaño de la ventana del explorador, es posible que deba haga clic en el icono de navegación para ver estos vínculos.

![](getting-started/_static/image6.png)  

La aplicación también proporciona compatibilidad para registrar e inicie sesión. El siguiente paso es cambiar el funcionamiento de esta aplicación y aprender un poco sobre ASP.NET MVC. Cierre la aplicación de ASP.NET MVC y vamos a cambiar algo de código.

Para obtener una lista de tutoriales actuales, consulte [MVC artículos recomendado](../mvc-learning-sequence.md).

## <a name="see-this-app-running-on-azure"></a>Consulte esta aplicación se ejecuta en Azure

¿Desea ver el sitio terminado que se ejecuta como una aplicación web en directo? Puede implementar una versión completa de la aplicación en su cuenta de Azure, simplemente haga clic en el botón siguiente.

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

Necesita una cuenta de Azure para implementar esta solución en Azure. Si no dispone de una cuenta, tiene las siguientes opciones:

- [Abrir una cuenta de Azure de forma gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -obtiene crédito puede usar para probar los servicios de Azure de pago e incluso después de que se usan hasta, puede mantener la cuenta y usar servicios gratuitos de Azure.
- [Activar las ventajas de suscriptor MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -su suscripción a MSDN le proporciona crédito todos los meses que puede usar para servicios de Azure de pago.

> [!div class="step-by-step"]
> [Siguiente](adding-a-controller.md)
