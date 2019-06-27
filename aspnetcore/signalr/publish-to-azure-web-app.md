---
title: Publicar un ASP.NET Core SignalR app en Azure App Service
author: bradygaster
description: Obtenga información sobre cómo publicar una aplicación de ASP.NET Core SignalR en Azure App Service.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/26/2019
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 87a9c93add373b24e3c473912cdbfcc00bbebf7e
ms.sourcegitcommit: 9bb29f9ba6f0645ee8b9cabda07e3a5aa52cd659
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2019
ms.locfileid: "67406112"
---
# <a name="publish-an-aspnet-core-signalr-app-to-azure-app-service"></a>Publicar un ASP.NET Core SignalR app en Azure App Service

Por [Brady Gaster](https://twitter.com/bradygaster)

[Azure App Service](/azure/app-service/app-service-web-overview) es un [Microsoft la informática en nube](https://azure.microsoft.com/) plataforma de servicio para hospedar aplicaciones web, incluido ASP.NET Core.

> [!NOTE]
> Este artículo se refiere a la publicación de una aplicación ASP.NET Core SignalR desde Visual Studio. Para obtener más información, consulte [SignalR service para Azure](https://azure.microsoft.com/services/signalr-service).

## <a name="publish-the-app"></a>Publicar la aplicación

Este artículo trata la publicación con las herramientas de Visual Studio. Pueden usar los usuarios de Visual Studio Code [CLI de Azure](/cli/azure) comandos para publicar aplicaciones en Azure. Para obtener más información, consulte [publicar una aplicación ASP.NET Core en Azure con herramientas de línea de comandos](/azure/app-service/app-service-web-get-started-dotnet).

1. Desde el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Publicar**.

1. Confirme que **App Service** y **crear nuevo** están seleccionados en el **elegir un destino de publicación** cuadro de diálogo.

1. Seleccione **Crear perfil** desde el **publicar** botón colocar hacia abajo.

   Escriba la información descrita en la siguiente tabla en la **crear App Service** cuadro de diálogo y seleccione **crear**.

   | Elemento               | Descripción |
   | ------------------ | ----------- |
   | **Name**           | Nombre único de la aplicación. |
   | **Suscripción**   | Suscripción de Azure que usa la aplicación. |
   | **Grupo de recursos** | Grupo de recursos relacionados a la que pertenece la aplicación. |
   | **Plan de hospedaje**   | Plan de precios para la aplicación web. |

1. Seleccione el **Azure SignalR Service** en el **dependencias** > **agregar** lista desplegable:

   ![Área de las dependencias que muestra la selección de Azure SignalR Service en la lista desplegable de agregar](publish-to-azure-web-app/_static/signalr-service-dependency.png)

1. En el **Azure SignalR Service** cuadro de diálogo, seleccione **crear una nueva instancia de Azure SignalR Service**.

1. Proporcione un **nombre**, **grupo de recursos**, y **ubicación**. Vuelva a la **Azure SignalR Service** cuadro de diálogo y seleccione **agregar**.

Visual Studio realiza las tareas siguientes:

* Crea un perfil de publicación que contiene la configuración de publicación.
* Crea un *Azure Web App* con los detalles proporcionados.
* Publica la aplicación.
* Inicia un explorador, que carga la aplicación web.

El formato de dirección URL de la aplicación es `{APP SERVICE NAME}.azurewebsites.net`. Por ejemplo, una aplicación denominada `SignalRChatApp` tiene una dirección URL de `https://signalrchatapp.azurewebsites.net`.

Si un HTTP *502.2 - puerta de enlace incorrecta* se produce un error al implementar una aplicación que tenga como destino una versión de .NET Core de versión preliminar, consulte [versión de vista previa de la implementación de ASP.NET Core en Azure App Service](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service) para resolverlo.

## <a name="configure-the-app-in-azure-app-service"></a>Configuración de la aplicación en Azure App Service

> [!NOTE]
> *En esta sección solo se aplica a las aplicaciones no usan Azure SignalR Service.*
>
> Si la aplicación usa Azure SignalR Service, el servicio de aplicación no requiere la configuración de afinidad de enrutamiento de solicitud de aplicaciones (ARR) y Web Sockets descritos en esta sección. Los clientes conectan los Sockets Web a Azure SignalR Service, no directamente a la aplicación.

Para las aplicaciones hospedadas sin Azure SignalR Service, habilitar:

* [Afinidad ARR](https://azure.github.io/AppService/2016/05/16/Disable-Session-affinity-cookie-(ARR-cookie)-for-Azure-web-apps.html) para enrutar las solicitudes de un usuario a la misma instancia de App Service. El valor predeterminado es **en**.
* [Web Sockets](xref:fundamentals/websockets) para permitir que el transporte de Web Sockets para la función. El valor predeterminado es **desactivar**.

1. En el portal de Azure, vaya a la aplicación web en **App Services**.
1. Abra **configuración** > **configuración General**.
1. Establecer **sockets Web** a **en**.
1. Compruebe que **afinidad ARR** está establecido en **en**.

## <a name="app-service-plan-limits"></a>Limita el Plan de App Service

Web Sockets y otros transportes están limitados según el Plan de App Service seleccionado. Para obtener más información, consulte el *límites de Azure Cloud Services* y *límites de App Service* secciones de la [suscripción de Azure y límites de servicio, cuotas y restricciones](/azure/azure-subscription-service-limits#app-service-limits) artículo.

## <a name="additional-resources"></a>Recursos adicionales

* [¿Qué es Azure SignalR Service?](/azure/azure-signalr/signalr-overview)
* <xref:signalr/introduction>
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [Publicar una aplicación ASP.NET Core en Azure con herramientas de línea de comandos](/azure/app-service/app-service-web-get-started-dotnet)
* [Hospedar e implementar aplicaciones de la versión preliminar de ASP.NET Core en Azure](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
