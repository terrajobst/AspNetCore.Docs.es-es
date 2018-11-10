---
uid: mvc/overview/getting-started/introduction/getting-started
title: Introducción a ASP.NET MVC 5 | Microsoft Docs
author: Rick-Anderson
ms.author: riande
ms.date: 10/04/2018
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 462583a42f20126ef8f8b5927268c20ec1ceab89
ms.sourcegitcommit: edb9d2d78c9a4d68b397e74ae2aff088b325a143
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2018
ms.locfileid: "51505809"
---
<a name="getting-started-with-aspnet-mvc-5"></a>Introducción a ASP.NET MVC 5
====================
por [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [consider RP](../../../../includes/razor.md)]

Este tutorial le enseña los aspectos básicos de la creación de una aplicación web de ASP.NET MVC 5 con [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017). El código fuente final para el tutorial se encuentra en [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie).

En este tutorial se escribió por [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) ) , y [Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )

Necesita una cuenta de Azure para implementar esta aplicación en Azure:

- También puede [abrir una cuenta de Azure de forma gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -obtenga créditos puede usar para probar los servicios de Azure de pago e incluso después de que se usan hasta, puede mantener la cuenta y usar servicios gratuitos de Azure.
- También puede [activar las ventajas de suscriptor MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -su suscripción a MSDN le proporciona crédito todos los meses que puede usar para servicios de Azure de pago.

## <a name="get-started"></a>Primeros pasos

Empiece por [instalar Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017). A continuación, abra Visual Studio.

Visual Studio es un entorno de desarrollo integrado o IDE. Al igual que utiliza Microsoft Word para escribir documentos, usará un IDE para crear aplicaciones. En Visual Studio, hay una lista en la parte inferior muestra distintas opciones disponibles para usted. También hay un menú que proporciona otra manera de realizar las tareas en el IDE. Por ejemplo, en lugar de seleccionar **nuevo proyecto** en el **página de inicio**, puede usar la barra de menús y seleccione **archivo** > **denuevoproyecto**.

![](getting-started/_static/image1.png)

## <a name="create-your-first-app"></a>Crear la primera aplicación

En el **página de inicio**, seleccione **nuevo proyecto**. En el **nuevo proyecto** cuadro de diálogo, seleccione el **Visual C#** categoría a la izquierda y, a continuación, **Web**y, a continuación, seleccione el **aplicación Web ASP.NET (.NET Framework)**  plantilla de proyecto. Nombre del proyecto "MvcMovie" y, a continuación, elija **Aceptar**.

![](getting-started/_static/image2.png)

En el **nueva aplicación Web ASP.NET** cuadro de diálogo, elija **MVC** y, a continuación, elija **Aceptar**.

![](getting-started/_static/image3.png)

Visual Studio usa una plantilla predeterminada para el proyecto de ASP.NET MVC que acaba de crear, por lo que tiene ahora una aplicación de trabajo sin hacer nada! Se trata de un simple "Hello World!" proyecto y, de un buen lugar para iniciar la aplicación.

![](getting-started/_static/image4.png)

Presiona **F5** para iniciar la depuración. Al presionar **F5**, Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación web. A continuación, Visual Studio inicia un explorador y abre la página principal de la aplicación. Tenga en cuenta que la barra de direcciones del explorador dice `localhost:port#` y no algo como `example.com`. Eso es porque `localhost` siempre apunta a su propio equipo local, que en este caso, se ejecuta la aplicación que acaba de crear. Cuando Visual Studio se ejecuta un proyecto web, se usa un puerto aleatorio para el servidor web. En la imagen siguiente, el número de puerto es 1234. Al ejecutar la aplicación, verá un número de puerto diferente.

![](getting-started/_static/image5.png)

Desde el comienzo esta plantilla predeterminada proporciona `Home`, `Contact`, y `About` páginas. No se muestra la imagen siguiente el **inicio**, **sobre**, y **póngase en contacto con** vínculos. Según el tamaño de la ventana del explorador, es posible que deba haga clic en el icono de navegación para ver estos vínculos.

![](getting-started/_static/image6.png)

La aplicación también proporciona compatibilidad para registrar e inicie sesión. El siguiente paso es cambiar el funcionamiento de esta aplicación y aprender un poco sobre ASP.NET MVC. Cierre la aplicación de ASP.NET MVC y vamos a cambiar algo de código.

Para obtener una lista de tutoriales actuales, consulte [MVC artículos recomendado](../mvc-learning-sequence.md).

## <a name="see-this-app-running-on-azure"></a>Consulte esta aplicación se ejecuta en Azure

¿Desea ver el sitio terminado que se ejecuta como una aplicación web en directo? Puede implementar una versión completa de la aplicación en su cuenta de Azure, simplemente haga clic en el botón siguiente.

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

Necesita una cuenta de Azure para implementar esta solución en Azure. Si aún no tiene una cuenta, use una de las opciones siguientes para crear uno:

- [Abrir una cuenta de Azure de forma gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -obtiene crédito puede usar para probar los servicios de Azure de pago e incluso después de que se usan hasta, puede mantener la cuenta y usar servicios gratuitos de Azure.
- [Activar los beneficios de suscriptor de Visual Studio](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers) -suscripción Your Visual Studio le proporciona crédito todos los meses que puede usar para servicios de Azure de pago.

> [!div class="step-by-step"]
> [Siguiente](adding-a-controller.md)
