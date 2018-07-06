---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Usar Web API 2 con Entity Framework 6 | Microsoft Docs
author: MikeWasson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación web con ASP.NET Web API back-end. Este tutorial usa Entity Framework 6 para el diseño de datos...
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: bc853413e814e6ef1a44841d114853003d7328f9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827238"
---
<a name="using-web-api-2-with-entity-framework-6"></a>Usar Web API 2 con Entity Framework 6
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargue el proyecto completado](https://github.com/MikeWasson/BookService)

> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación web con ASP.NET Web API back-end. El tutorial usa Entity Framework 6 para la capa de datos y Knockout.js para la aplicación de JavaScript del lado cliente. El tutorial también muestra cómo implementar la aplicación en Azure App Service Web Apps.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - Web API 2.1
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> - [Knockout.js](http://knockoutjs.com/) 3.1


Este tutorial usa para crear una aplicación web que se manipula una base de datos back-end ASP.NET Web API 2 con Entity Framework 6. Aquí es una captura de pantalla de la aplicación que va a crear.

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

La aplicación utiliza un diseño de la aplicación de página única (SPA). "Aplicación de página única" es el término general para una aplicación web que se carga una página HTML única y, a continuación, actualiza la página de forma dinámica, en lugar de cargar páginas nuevas. Después de la carga de página inicial, la aplicación se comunica con el servidor a través de las solicitudes AJAX. El AJAX solicita devolver datos JSON, que la aplicación usa para actualizar la interfaz de usuario.

AJAX no es nueva, pero hoy en día existen marcos de JavaScript que resulte más fácil crear y mantener una gran aplicación SPA sofisticada. Este tutorial se usa [Knockout.js](http://knockoutjs.com/), pero puede usar cualquier marco de cliente de JavaScript.

Estos son los principales bloques de creación para esta aplicación:

- ASP.NET MVC se crea la página HTML.
- ASP.NET Web API controla las solicitudes AJAX y devuelve datos JSON.
- Knockout.js enlaza datos con los elementos HTML a los datos JSON.
- Entity Framework se comunica con la base de datos.

## <a name="see-this-app-running-on-azure"></a>Consulte esta aplicación se ejecuta en Azure

¿Desea ver el sitio terminado que se ejecuta como una aplicación web en directo? Puede implementar una versión completa de la aplicación en su cuenta de Azure, simplemente haga clic en el botón siguiente.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

Necesita una cuenta de Azure para implementar esta solución en Azure. Si no dispone de una cuenta, tiene las siguientes opciones:

- [Abrir una cuenta de Azure de forma gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -obtiene crédito puede usar para probar los servicios de Azure de pago e incluso después de que se usan hasta, puede mantener la cuenta y usar servicios gratuitos de Azure.
- [Activar las ventajas de suscriptor MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -su suscripción a MSDN le proporciona crédito todos los meses que puede usar para servicios de Azure de pago.

## <a name="create-the-project"></a>Crear el proyecto

Abra Visual Studio. Desde el **archivo** menú, seleccione **New**, a continuación, seleccione **proyecto**. (O haga clic en **nuevo proyecto** en la página de inicio.)

En el **nuevo proyecto** cuadro de diálogo, haga clic en **Web** en el panel izquierdo y **aplicación Web ASP.NET** en el panel central. Denomine el proyecto BookService y haga clic en **Aceptar**.

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

En el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione el **API Web** plantilla.

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

Si desea hospedar el proyecto en un Azure App Service, deje el **Host en la nube** casilla activada.

Haga clic en **Aceptar** para crear el proyecto.

## <a name="configure-azure-settings-optional"></a>Configurar valores de Azure (opcionales)

Si deja el **Host en la nube** opción activada, Visual Studio le pedirá que inicie sesión en Microsoft Azure

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

Después de iniciar sesión Azure, Visual Studio le pedirá que configure la aplicación web. Escriba un nombre para el sitio, seleccione su suscripción de Azure y seleccione una región geográfica. En **el servidor de base de datos**, seleccione **crear nuevo servidor**. Escriba un nombre de usuario de administrador y una contraseña.

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

> [!div class="step-by-step"]
> [Siguiente](part-2.md)
