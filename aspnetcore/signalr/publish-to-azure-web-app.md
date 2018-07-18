---
title: Publicar un ASP.NET Core SignalR aplicación a aplicación Web de Azure
author: tdykstra
description: Publicar un ASP.NET Core SignalR aplicación a aplicación Web de Azure
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/20/2018
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: b0126771a9ba3a28a7af14adf5b5959c7591e5fb
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095299"
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a>Publicar un ASP.NET Core SignalR aplicación a una aplicación Web de Azure

[Azure Web App](/azure/app-service/app-service-web-overview) es un [Microsoft la informática en nube](https://azure.microsoft.com/) plataforma de servicio para hospedar aplicaciones web, incluido ASP.NET Core.

> [!NOTE]
> Este artículo se refiere a la publicación de una aplicación ASP.NET Core SignalR desde Visual Studio. Visite [SignalR service para Azure](https://azure.microsoft.com/en-gb/services/signalr-service?) para obtener más información sobre el uso de SignalR en Azure.

## <a name="publish-the-app"></a>Publicar la aplicación

Visual Studio proporciona herramientas integradas para la publicación en una aplicación Web de Azure. Puede usar el usuario de Visual Studio Code [CLI de Azure](/cli/azure) comandos para publicar aplicaciones en Azure. Este artículo trata la publicación con las herramientas de Visual Studio. Para publicar una aplicación mediante la CLI de Azure, consulte [publicar una aplicación ASP.NET Core en Azure con herramientas de línea de comandos](xref:tutorials/publish-to-azure-webapp-using-cli).

Haga doble clic en el proyecto en **el Explorador de soluciones** y seleccione **publicar**. Confirme que **crear nuevo** está activada en la **elegir un destino de publicación** cuadro de diálogo y seleccione **publicar**.

![Seleccionar destino de la publicación](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

Escriba la siguiente información en el **crear App Service** cuadro de diálogo y seleccione **crear**.

| Elemento | Descripción |
| ---- | ----------- |
| **Nombre de la aplicación** | Un nombre único de la aplicación. |
| **Suscripción** | La suscripción de Azure que usa la aplicación. |
| **Grupo de recursos** | El grupo de recursos relacionados a la que pertenece la aplicación.  |
| **Plan de hospedaje** | El plan de precios para la aplicación web. |

![Creación de app service](publish-to-azure-web-app/_static/create-app-service-dialog.png)

Visual Studio realiza las tareas siguientes:

* Crea un perfil de publicación que contiene la configuración de publicación.
* Crea o utiliza un archivo *Azure Web App* con los detalles proporcionados.
* Publica la aplicación.
* Inicia un explorador, con la aplicación web publicada cargada.

Tenga en cuenta el formato de la dirección URL de la aplicación se *{nombre de la aplicación}. azurewebsites NET*. Por ejemplo, una aplicación denominada `SignalRChattR` tiene una dirección URL similar a `https://signalrchattr.azurewebsites.net`.

Si se produce un error HTTP 502.2, consulte [versión de vista previa de la implementación de ASP.NET Core en Azure App Service](xref:host-and-deploy/azure-apps/index) para resolverlo.

## <a name="configure-signalr-web-app"></a>Configuración de aplicación web de SignalR

ASP.NET Core SignalR aplicaciones que se publican como una aplicación Web de Azure debe tener [afinidad ARR](https://en.wikipedia.org/wiki/Application_Request_Routing) habilitado. [WebSockets](xref:fundamentals/websockets) debe habilitarse para permitir que el transporte de WebSockets funcione.

En el portal de Azure, vaya a **configuración de la aplicación** para la aplicación web. Establecer **WebSockets** a **en**y compruebe **afinidad ARR** es **en**.

![Configuración de Azure Web app en Azure portal](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 WebSockets y otros transportes [están limitados según el Plan de App Service](/azure/azure-subscription-service-limits#app-service-limits).

## <a name="related-resources"></a>Recursos relacionados

* [Publicar una aplicación ASP.NET Core en Azure con herramientas de línea de comandos](xref:tutorials/publish-to-azure-webapp-using-cli?tabs=windows)
* [Publicar una aplicación ASP.NET Core en Azure con Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs)
* [Hospedar e implementar aplicaciones de la versión preliminar de ASP.NET Core en Azure](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
