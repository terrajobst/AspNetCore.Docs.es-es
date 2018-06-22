---
title: Publicar un núcleo de ASP.NET SignalR aplicación a aplicación Web de Azure
author: rachelappel
description: Publicar un núcleo de ASP.NET SignalR aplicación a aplicación Web de Azure
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/20/2018
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 0d98c6b24b9695c0af0170173f13902bac5f55ed
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36271923"
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a>Publicar un núcleo de ASP.NET SignalR aplicación a una aplicación Web de Azure

[Aplicación Web de Azure](/azure/app-service/app-service-web-overview) es un [Microsoft la informática en nube](https://azure.microsoft.com/) servicio de plataforma para hospedar las aplicaciones web, incluido ASP.NET Core.

> [!NOTE]
> En este artículo se refiere a la publicación de una aplicación de ASP.NET Core SignalR desde Visual Studio. Visite [SignalR servicio para Azure](https://azure.microsoft.com/en-gb/services/signalr-service?) para obtener más información sobre el uso de SignalR en Azure.

## <a name="publish-the-app"></a>Publicar la aplicación

Visual Studio proporciona herramientas integradas para la publicación en una aplicación Web de Azure. Puede usar el usuario de Visual Studio Code [CLI de Azure](/cli/azure) comandos para publicar aplicaciones para Azure. Este artículo trata la publicación con las herramientas de Visual Studio. Para publicar una aplicación mediante la CLI de Azure, consulte [publicar una aplicación de ASP.NET Core en Azure con las herramientas de línea de comandos](xref:tutorials/publish-to-azure-webapp-using-cli).

Haga doble clic en el proyecto en **el Explorador de soluciones** y seleccione **publicar**. Confirme que **crear nuevo** está activada en la **elegir un destino de publicación** cuadro de diálogo y seleccione **publicar**.

![Selección de destino de publicación](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

Escriba la siguiente información en el **crear servicio en la aplicación** cuadro de diálogo y seleccione **crear**.

| Elemento | Descripción |
| ---- | ----------- |
| **Nombre de la aplicación** | Un nombre único de la aplicación. |
| **Suscripción** | La suscripción de Azure que usa la aplicación. |
| **Grupo de recursos** | El grupo de recursos relacionados a la que pertenece la aplicación.  |
| **Plan de hospedaje** | El plan de precios de la aplicación web. |

![Crear servicio de aplicaciones](publish-to-azure-web-app/_static/create-app-service-dialog.png)

Visual Studio realiza las tareas siguientes:

* Crea un perfil de publicación que contiene la configuración de publicación.
* Crea o utiliza un archivo *aplicación Web de Azure* con los detalles proporcionados.
* Publica la aplicación.
* Inicia un explorador, con la aplicación web publicada cargada.

Tenga en cuenta el formato de la dirección URL para la aplicación es *.azurewebsites {nombre de la aplicación} .net*. Por ejemplo, una aplicación denominada `SignalRChattR` tiene una dirección URL similar a `https://signalrchattr.azurewebsites.net`.

Si se produce un error de HTTP 502.2, consulte [versión de vista previa de implementar ASP.NET Core para el servicio de aplicaciones de Azure](xref:host-and-deploy/azure-apps/index) para resolverlo.

## <a name="configure-signalr-web-app"></a>Configurar la aplicación web de SignalR

Las aplicaciones ASP.NET SignalR Core que se publican como una aplicación Web de Azure debe tener [ARR afinidad](https://en.wikipedia.org/wiki/Application_Request_Routing) habilitado. [WebSockets](xref:fundamentals/websockets) debe habilitarse para permitir que el transporte de WebSockets a función.

En el portal de Azure, vaya a **configuración de la aplicación** para su aplicación web. Establecer **WebSockets** a **en**y compruebe **ARR afinidad** es **en**.

![Configuración de la aplicación Web Azure en el portal de Azure](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 WebSockets y otros transportes [están limitados según el Plan de servicio de aplicaciones](/azure/azure-subscription-service-limits#app-service-limits).

## <a name="related-resources"></a>Recursos relacionados

* [Publicar una aplicación de ASP.NET Core en Azure con las herramientas de línea de comandos](xref:tutorials/publish-to-azure-webapp-using-cli?tabs=windows)
* [Publicar una aplicación de ASP.NET Core en Azure con Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs)
* [Hospedar e implementar aplicaciones de ASP.NET Core Preview en Azure](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
