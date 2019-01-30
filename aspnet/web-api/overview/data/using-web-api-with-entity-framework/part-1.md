---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Usar Web API 2 con Entity Framework 6 | Microsoft Docs
author: MikeWasson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación web con ASP.NET Web API back-end. Este tutorial usa Entity Framework 6 para el diseño de datos...
ms.author: riande
ms.date: 01/17/2019
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 266c808e3525787181038d2de473194989039e02
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236528"
---
<a name="using-web-api-2-with-entity-framework-6"></a>Usar Web API 2 con Entity Framework 6
====================

[Descargue el proyecto completado](https://github.com/MikeWasson/BookService)

> Este tutorial le enseña los aspectos básicos de la creación de una aplicación web con ASP.NET Web API back-end. El tutorial usa Entity Framework 6 para la capa de datos y Knockout.js para la aplicación de JavaScript del lado cliente. El tutorial también muestra cómo implementar la aplicación en Azure App Service Web Apps.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
>
> - Web API 2.1
> - Visual Studio 2017 (Descargar Visual Studio 2017 [aquí](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))
> - Entity Framework 6
> - .NET 4.7
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

¿Desea ver el sitio terminado que se ejecuta como una aplicación web en directo? Puede implementar una versión completa de la aplicación en su cuenta de Azure, seleccione el botón siguiente.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

Necesita una cuenta de Azure para implementar esta solución en Azure. Si no dispone de una cuenta, tiene las siguientes opciones:

- [Abrir una cuenta de Azure de forma gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -obtiene crédito puede usar para probar los servicios de Azure de pago e incluso después de que se usan hasta, puede mantener la cuenta y usar servicios gratuitos de Azure.
- [Activar las ventajas de suscriptor MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -su suscripción a MSDN le proporciona crédito todos los meses que puede usar para servicios de Azure de pago.

## <a name="create-the-project"></a>Crear el proyecto

Abra Visual Studio. Desde el **archivo** menú, seleccione **New**, a continuación, seleccione **proyecto**. (O seleccione **nuevo proyecto** en la página de inicio.)

En el **nuevo proyecto** cuadro de diálogo, seleccione **Web** en el panel izquierdo y **aplicación Web ASP.NET (.NET Framework)** en el panel central. Denomine el proyecto **BookService** y seleccione **Aceptar**.

[![](part-1/_static/image11.png)](part-1/_static/image11.png)

En el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione el **API Web** plantilla.

[![](part-1/_static/image12.png)](part-1/_static/image12.png)


Seleccione **Aceptar** para crear el proyecto.

## <a name="configure-azure-settings-optional"></a>Configurar valores de Azure (opcionales)

Después de crear el proyecto, puede implementar en Azure App Service Web Apps en cualquier momento. 

1. En el Explorador de soluciones, haga doble clic en el proyecto y seleccione **publicar**.

2. En la ventana que aparece, seleccione **iniciar**. El **elegir un destino de publicación** aparece el cuadro de diálogo.

   [![](part-1/_static/image14.png)](part-1/_static/image14.png)

3. Seleccione **Crear perfil**. Aparecerá el cuadro de diálogo **Crear servicio de aplicaciones**.

   [![](part-1/_static/image15.png)](part-1/_static/image15.png)

   Acepte los valores predeterminados o escribir valores diferentes para el nombre de aplicación, el grupo de recursos, hospedaje de plan, la suscripción de Azure y la región geográfica. 

4. Seleccione **crear una base de datos SQL**. El **configurar SQL Server** aparece el cuadro de diálogo. 

   [![](part-1/_static/image16.png)](part-1/_static/image16.png)

   Acepte los valores predeterminados o escribir valores diferentes. Escriba un **Adminstrator Username** y **contraseña de administrador** para la base de datos. Seleccione **Aceptar** cuando haya terminado. El **crear App Service** página volverá a aparecer.

5. Seleccione **crear** para crear su perfil. Aparece un mensaje en la esquina inferior derecha, que indica que la implementación está en curso. Después de unos momentos, el **publicar** ventana vuelve a aparecer.

    [![](part-1/_static/image17.png)](part-1/_static/image17.png)
   
    El perfil que creó para implementar la aplicación ahora está disponible. 


> [!div class="step-by-step"]
> [Siguiente](part-2.md)
