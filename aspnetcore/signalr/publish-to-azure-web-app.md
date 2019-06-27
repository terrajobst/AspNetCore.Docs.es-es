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
# <a name="publish-an-aspnet-core-signalr-app-to-azure-app-service"></a><span data-ttu-id="a4ada-103">Publicar un ASP.NET Core SignalR app en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="a4ada-103">Publish an ASP.NET Core SignalR app to Azure App Service</span></span>

<span data-ttu-id="a4ada-104">Por [Brady Gaster](https://twitter.com/bradygaster)</span><span class="sxs-lookup"><span data-stu-id="a4ada-104">By [Brady Gaster](https://twitter.com/bradygaster)</span></span>

<span data-ttu-id="a4ada-105">[Azure App Service](/azure/app-service/app-service-web-overview) es un [Microsoft la informática en nube](https://azure.microsoft.com/) plataforma de servicio para hospedar aplicaciones web, incluido ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a4ada-105">[Azure App Service](/azure/app-service/app-service-web-overview) is a [Microsoft cloud computing](https://azure.microsoft.com/) platform service for hosting web apps, including ASP.NET Core.</span></span>

> [!NOTE]
> <span data-ttu-id="a4ada-106">Este artículo se refiere a la publicación de una aplicación ASP.NET Core SignalR desde Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a4ada-106">This article refers to publishing an ASP.NET Core SignalR app from Visual Studio.</span></span> <span data-ttu-id="a4ada-107">Para obtener más información, consulte [SignalR service para Azure](https://azure.microsoft.com/services/signalr-service).</span><span class="sxs-lookup"><span data-stu-id="a4ada-107">For more information, see [SignalR service for Azure](https://azure.microsoft.com/services/signalr-service).</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="a4ada-108">Publicar la aplicación</span><span class="sxs-lookup"><span data-stu-id="a4ada-108">Publish the app</span></span>

<span data-ttu-id="a4ada-109">Este artículo trata la publicación con las herramientas de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a4ada-109">This article covers publishing using the tools in Visual Studio.</span></span> <span data-ttu-id="a4ada-110">Pueden usar los usuarios de Visual Studio Code [CLI de Azure](/cli/azure) comandos para publicar aplicaciones en Azure.</span><span class="sxs-lookup"><span data-stu-id="a4ada-110">Visual Studio Code users can use [Azure CLI](/cli/azure) commands to publish apps to Azure.</span></span> <span data-ttu-id="a4ada-111">Para obtener más información, consulte [publicar una aplicación ASP.NET Core en Azure con herramientas de línea de comandos](/azure/app-service/app-service-web-get-started-dotnet).</span><span class="sxs-lookup"><span data-stu-id="a4ada-111">For more information, see [Publish an ASP.NET Core app to Azure with command line tools](/azure/app-service/app-service-web-get-started-dotnet).</span></span>

1. <span data-ttu-id="a4ada-112">Desde el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="a4ada-112">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span>

1. <span data-ttu-id="a4ada-113">Confirme que **App Service** y **crear nuevo** están seleccionados en el **elegir un destino de publicación** cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="a4ada-113">Confirm that **App Service** and **Create new** are selected in the **Pick a publish target** dialog.</span></span>

1. <span data-ttu-id="a4ada-114">Seleccione **Crear perfil** desde el **publicar** botón colocar hacia abajo.</span><span class="sxs-lookup"><span data-stu-id="a4ada-114">Select **Create Profile** from the **Publish** button drop down.</span></span>

   <span data-ttu-id="a4ada-115">Escriba la información descrita en la siguiente tabla en la **crear App Service** cuadro de diálogo y seleccione **crear**.</span><span class="sxs-lookup"><span data-stu-id="a4ada-115">Enter the information described in the following table in the **Create App Service** dialog and select **Create**.</span></span>

   | <span data-ttu-id="a4ada-116">Elemento</span><span class="sxs-lookup"><span data-stu-id="a4ada-116">Item</span></span>               | <span data-ttu-id="a4ada-117">Descripción</span><span class="sxs-lookup"><span data-stu-id="a4ada-117">Description</span></span> |
   | ------------------ | ----------- |
   | <span data-ttu-id="a4ada-118">**Name**</span><span class="sxs-lookup"><span data-stu-id="a4ada-118">**Name**</span></span>           | <span data-ttu-id="a4ada-119">Nombre único de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a4ada-119">Unique name of the app.</span></span> |
   | <span data-ttu-id="a4ada-120">**Suscripción**</span><span class="sxs-lookup"><span data-stu-id="a4ada-120">**Subscription**</span></span>   | <span data-ttu-id="a4ada-121">Suscripción de Azure que usa la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a4ada-121">Azure subscription that the app uses.</span></span> |
   | <span data-ttu-id="a4ada-122">**Grupo de recursos**</span><span class="sxs-lookup"><span data-stu-id="a4ada-122">**Resource Group**</span></span> | <span data-ttu-id="a4ada-123">Grupo de recursos relacionados a la que pertenece la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a4ada-123">Group of related resources to which the app belongs.</span></span> |
   | <span data-ttu-id="a4ada-124">**Plan de hospedaje**</span><span class="sxs-lookup"><span data-stu-id="a4ada-124">**Hosting Plan**</span></span>   | <span data-ttu-id="a4ada-125">Plan de precios para la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="a4ada-125">Pricing plan for the web app.</span></span> |

1. <span data-ttu-id="a4ada-126">Seleccione el **Azure SignalR Service** en el **dependencias** > **agregar** lista desplegable:</span><span class="sxs-lookup"><span data-stu-id="a4ada-126">Select the **Azure SignalR Service** in the **Dependencies** > **Add** drop-down list:</span></span>

   ![Área de las dependencias que muestra la selección de Azure SignalR Service en la lista desplegable de agregar](publish-to-azure-web-app/_static/signalr-service-dependency.png)

1. <span data-ttu-id="a4ada-128">En el **Azure SignalR Service** cuadro de diálogo, seleccione **crear una nueva instancia de Azure SignalR Service**.</span><span class="sxs-lookup"><span data-stu-id="a4ada-128">In the **Azure SignalR Service** dialog, select **Create a new Azure SignalR Service instance**.</span></span>

1. <span data-ttu-id="a4ada-129">Proporcione un **nombre**, **grupo de recursos**, y **ubicación**.</span><span class="sxs-lookup"><span data-stu-id="a4ada-129">Provide a **Name**, **Resource Group**, and **Location**.</span></span> <span data-ttu-id="a4ada-130">Vuelva a la **Azure SignalR Service** cuadro de diálogo y seleccione **agregar**.</span><span class="sxs-lookup"><span data-stu-id="a4ada-130">Return to the **Azure SignalR Service** dialog and select **Add**.</span></span>

<span data-ttu-id="a4ada-131">Visual Studio realiza las tareas siguientes:</span><span class="sxs-lookup"><span data-stu-id="a4ada-131">Visual Studio completes the following tasks:</span></span>

* <span data-ttu-id="a4ada-132">Crea un perfil de publicación que contiene la configuración de publicación.</span><span class="sxs-lookup"><span data-stu-id="a4ada-132">Creates a Publish Profile containing publish settings.</span></span>
* <span data-ttu-id="a4ada-133">Crea un *Azure Web App* con los detalles proporcionados.</span><span class="sxs-lookup"><span data-stu-id="a4ada-133">Creates an *Azure Web App* with the provided details.</span></span>
* <span data-ttu-id="a4ada-134">Publica la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a4ada-134">Publishes the app.</span></span>
* <span data-ttu-id="a4ada-135">Inicia un explorador, que carga la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="a4ada-135">Launches a browser, which loads the web app.</span></span>

<span data-ttu-id="a4ada-136">El formato de dirección URL de la aplicación es `{APP SERVICE NAME}.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="a4ada-136">The format of the app's URL is `{APP SERVICE NAME}.azurewebsites.net`.</span></span> <span data-ttu-id="a4ada-137">Por ejemplo, una aplicación denominada `SignalRChatApp` tiene una dirección URL de `https://signalrchatapp.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="a4ada-137">For example, an app named `SignalRChatApp` has a URL of `https://signalrchatapp.azurewebsites.net`.</span></span>

<span data-ttu-id="a4ada-138">Si un HTTP *502.2 - puerta de enlace incorrecta* se produce un error al implementar una aplicación que tenga como destino una versión de .NET Core de versión preliminar, consulte [versión de vista previa de la implementación de ASP.NET Core en Azure App Service](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service) para resolverlo.</span><span class="sxs-lookup"><span data-stu-id="a4ada-138">If an HTTP *502.2 - Bad Gateway* error occurs when deploying an app that targets a preview .NET Core release, see [Deploy ASP.NET Core preview release to Azure App Service](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service) to resolve it.</span></span>

## <a name="configure-the-app-in-azure-app-service"></a><span data-ttu-id="a4ada-139">Configuración de la aplicación en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="a4ada-139">Configure the app in Azure App Service</span></span>

> [!NOTE]
> <span data-ttu-id="a4ada-140">*En esta sección solo se aplica a las aplicaciones no usan Azure SignalR Service.*</span><span class="sxs-lookup"><span data-stu-id="a4ada-140">*This section only applies to apps not using the Azure SignalR Service.*</span></span>
>
> <span data-ttu-id="a4ada-141">Si la aplicación usa Azure SignalR Service, el servicio de aplicación no requiere la configuración de afinidad de enrutamiento de solicitud de aplicaciones (ARR) y Web Sockets descritos en esta sección.</span><span class="sxs-lookup"><span data-stu-id="a4ada-141">If the app uses the Azure SignalR Service, the App Service doesn't require the configuration of Application Request Routing (ARR) Affinity and Web Sockets described in this section.</span></span> <span data-ttu-id="a4ada-142">Los clientes conectan los Sockets Web a Azure SignalR Service, no directamente a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a4ada-142">Clients connect their Web Sockets to the Azure SignalR Service, not directly to the app.</span></span>

<span data-ttu-id="a4ada-143">Para las aplicaciones hospedadas sin Azure SignalR Service, habilitar:</span><span class="sxs-lookup"><span data-stu-id="a4ada-143">For apps hosted without the Azure SignalR Service, enable:</span></span>

* <span data-ttu-id="a4ada-144">[Afinidad ARR](https://azure.github.io/AppService/2016/05/16/Disable-Session-affinity-cookie-(ARR-cookie)-for-Azure-web-apps.html) para enrutar las solicitudes de un usuario a la misma instancia de App Service.</span><span class="sxs-lookup"><span data-stu-id="a4ada-144">[ARR Affinity](https://azure.github.io/AppService/2016/05/16/Disable-Session-affinity-cookie-(ARR-cookie)-for-Azure-web-apps.html) to route requests from a user back to the same App Service instance.</span></span> <span data-ttu-id="a4ada-145">El valor predeterminado es **en**.</span><span class="sxs-lookup"><span data-stu-id="a4ada-145">The default setting is **On**.</span></span>
* <span data-ttu-id="a4ada-146">[Web Sockets](xref:fundamentals/websockets) para permitir que el transporte de Web Sockets para la función.</span><span class="sxs-lookup"><span data-stu-id="a4ada-146">[Web Sockets](xref:fundamentals/websockets) to allow the Web Sockets transport to function.</span></span> <span data-ttu-id="a4ada-147">El valor predeterminado es **desactivar**.</span><span class="sxs-lookup"><span data-stu-id="a4ada-147">The default setting is **Off**.</span></span>

1. <span data-ttu-id="a4ada-148">En el portal de Azure, vaya a la aplicación web en **App Services**.</span><span class="sxs-lookup"><span data-stu-id="a4ada-148">In the Azure portal, navigate to the web app in **App Services**.</span></span>
1. <span data-ttu-id="a4ada-149">Abra **configuración** > **configuración General**.</span><span class="sxs-lookup"><span data-stu-id="a4ada-149">Open **Configuration** > **General settings**.</span></span>
1. <span data-ttu-id="a4ada-150">Establecer **sockets Web** a **en**.</span><span class="sxs-lookup"><span data-stu-id="a4ada-150">Set **Web sockets** to **On**.</span></span>
1. <span data-ttu-id="a4ada-151">Compruebe que **afinidad ARR** está establecido en **en**.</span><span class="sxs-lookup"><span data-stu-id="a4ada-151">Verify that **ARR affinity** is set to **On**.</span></span>

## <a name="app-service-plan-limits"></a><span data-ttu-id="a4ada-152">Limita el Plan de App Service</span><span class="sxs-lookup"><span data-stu-id="a4ada-152">App Service Plan limits</span></span>

<span data-ttu-id="a4ada-153">Web Sockets y otros transportes están limitados según el Plan de App Service seleccionado.</span><span class="sxs-lookup"><span data-stu-id="a4ada-153">Web Sockets and other transports are limited based on the App Service Plan selected.</span></span> <span data-ttu-id="a4ada-154">Para obtener más información, consulte el *límites de Azure Cloud Services* y *límites de App Service* secciones de la [suscripción de Azure y límites de servicio, cuotas y restricciones](/azure/azure-subscription-service-limits#app-service-limits) artículo.</span><span class="sxs-lookup"><span data-stu-id="a4ada-154">For more information, see the *Azure Cloud Services limits* and *App Service limits* sections of the [Azure subscription and service limits, quotas, and constraints](/azure/azure-subscription-service-limits#app-service-limits) article.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a4ada-155">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="a4ada-155">Additional resources</span></span>

* [<span data-ttu-id="a4ada-156">¿Qué es Azure SignalR Service?</span><span class="sxs-lookup"><span data-stu-id="a4ada-156">What is Azure SignalR Service?</span></span>](/azure/azure-signalr/signalr-overview)
* <xref:signalr/introduction>
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="a4ada-157">Publicar una aplicación ASP.NET Core en Azure con herramientas de línea de comandos</span><span class="sxs-lookup"><span data-stu-id="a4ada-157">Publish an ASP.NET Core app to Azure with command line tools</span></span>](/azure/app-service/app-service-web-get-started-dotnet)
* [<span data-ttu-id="a4ada-158">Hospedar e implementar aplicaciones de la versión preliminar de ASP.NET Core en Azure</span><span class="sxs-lookup"><span data-stu-id="a4ada-158">Host and deploy ASP.NET Core Preview apps on Azure</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
