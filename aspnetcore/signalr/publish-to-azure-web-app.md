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
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a><span data-ttu-id="4dddc-103">Publicar un núcleo de ASP.NET SignalR aplicación a una aplicación Web de Azure</span><span class="sxs-lookup"><span data-stu-id="4dddc-103">Publish an ASP.NET Core SignalR app to an Azure Web App</span></span>

<span data-ttu-id="4dddc-104">[Aplicación Web de Azure](/azure/app-service/app-service-web-overview) es un [Microsoft la informática en nube](https://azure.microsoft.com/) servicio de plataforma para hospedar las aplicaciones web, incluido ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4dddc-104">[Azure Web App](/azure/app-service/app-service-web-overview) is a [Microsoft cloud computing](https://azure.microsoft.com/) platform service for hosting web apps, including ASP.NET Core.</span></span>

> [!NOTE]
> <span data-ttu-id="4dddc-105">En este artículo se refiere a la publicación de una aplicación de ASP.NET Core SignalR desde Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4dddc-105">This article refers to publishing an ASP.NET Core SignalR app from Visual Studio.</span></span> <span data-ttu-id="4dddc-106">Visite [SignalR servicio para Azure](https://azure.microsoft.com/en-gb/services/signalr-service?) para obtener más información sobre el uso de SignalR en Azure.</span><span class="sxs-lookup"><span data-stu-id="4dddc-106">Visit [SignalR service for Azure](https://azure.microsoft.com/en-gb/services/signalr-service?) for more information about using SignalR on Azure.</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="4dddc-107">Publicar la aplicación</span><span class="sxs-lookup"><span data-stu-id="4dddc-107">Publish the app</span></span>

<span data-ttu-id="4dddc-108">Visual Studio proporciona herramientas integradas para la publicación en una aplicación Web de Azure.</span><span class="sxs-lookup"><span data-stu-id="4dddc-108">Visual Studio provides built-in tools for publishing to an Azure Web App.</span></span> <span data-ttu-id="4dddc-109">Puede usar el usuario de Visual Studio Code [CLI de Azure](/cli/azure) comandos para publicar aplicaciones para Azure.</span><span class="sxs-lookup"><span data-stu-id="4dddc-109">Visual Studio Code user can use [Azure CLI](/cli/azure) commands to publish apps to Azure.</span></span> <span data-ttu-id="4dddc-110">Este artículo trata la publicación con las herramientas de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4dddc-110">This article covers publishing using the tools in Visual Studio.</span></span> <span data-ttu-id="4dddc-111">Para publicar una aplicación mediante la CLI de Azure, consulte [publicar una aplicación de ASP.NET Core en Azure con las herramientas de línea de comandos](xref:tutorials/publish-to-azure-webapp-using-cli).</span><span class="sxs-lookup"><span data-stu-id="4dddc-111">To publish an app using Azure CLI, see [Publish an ASP.NET Core app to Azure with command line tools](xref:tutorials/publish-to-azure-webapp-using-cli).</span></span>

<span data-ttu-id="4dddc-112">Haga doble clic en el proyecto en **el Explorador de soluciones** y seleccione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="4dddc-112">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span> <span data-ttu-id="4dddc-113">Confirme que **crear nuevo** está activada en la **elegir un destino de publicación** cuadro de diálogo y seleccione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="4dddc-113">Confirm that **Create new** is checked in the **Pick a publish target** dialog, and select **Publish**.</span></span>

![Selección de destino de publicación](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

<span data-ttu-id="4dddc-115">Escriba la siguiente información en el **crear servicio en la aplicación** cuadro de diálogo y seleccione **crear**.</span><span class="sxs-lookup"><span data-stu-id="4dddc-115">Enter the following information in the **Create App Service** dialog and select **Create**.</span></span>

| <span data-ttu-id="4dddc-116">Elemento</span><span class="sxs-lookup"><span data-stu-id="4dddc-116">Item</span></span> | <span data-ttu-id="4dddc-117">Descripción</span><span class="sxs-lookup"><span data-stu-id="4dddc-117">Description</span></span> |
| ---- | ----------- |
| <span data-ttu-id="4dddc-118">**Nombre de la aplicación**</span><span class="sxs-lookup"><span data-stu-id="4dddc-118">**App name**</span></span> | <span data-ttu-id="4dddc-119">Un nombre único de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4dddc-119">A unique name of the app.</span></span> |
| <span data-ttu-id="4dddc-120">**Suscripción**</span><span class="sxs-lookup"><span data-stu-id="4dddc-120">**Subscription**</span></span> | <span data-ttu-id="4dddc-121">La suscripción de Azure que usa la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4dddc-121">The Azure subscription that the app uses.</span></span> |
| <span data-ttu-id="4dddc-122">**Grupo de recursos**</span><span class="sxs-lookup"><span data-stu-id="4dddc-122">**Resource Group**</span></span> | <span data-ttu-id="4dddc-123">El grupo de recursos relacionados a la que pertenece la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4dddc-123">The group of related resources to which the app belongs.</span></span>  |
| <span data-ttu-id="4dddc-124">**Plan de hospedaje**</span><span class="sxs-lookup"><span data-stu-id="4dddc-124">**Hosting Plan**</span></span> | <span data-ttu-id="4dddc-125">El plan de precios de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="4dddc-125">The pricing plan for the web app.</span></span> |

![Crear servicio de aplicaciones](publish-to-azure-web-app/_static/create-app-service-dialog.png)

<span data-ttu-id="4dddc-127">Visual Studio realiza las tareas siguientes:</span><span class="sxs-lookup"><span data-stu-id="4dddc-127">Visual Studio completes the following tasks:</span></span>

* <span data-ttu-id="4dddc-128">Crea un perfil de publicación que contiene la configuración de publicación.</span><span class="sxs-lookup"><span data-stu-id="4dddc-128">Creates a Publish Profile containing publish settings.</span></span>
* <span data-ttu-id="4dddc-129">Crea o utiliza un archivo *aplicación Web de Azure* con los detalles proporcionados.</span><span class="sxs-lookup"><span data-stu-id="4dddc-129">Creates or uses an existing *Azure Web App* with the provided details.</span></span>
* <span data-ttu-id="4dddc-130">Publica la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4dddc-130">Publishes the app.</span></span>
* <span data-ttu-id="4dddc-131">Inicia un explorador, con la aplicación web publicada cargada.</span><span class="sxs-lookup"><span data-stu-id="4dddc-131">Launches a browser, with the published web app loaded.</span></span>

<span data-ttu-id="4dddc-132">Tenga en cuenta el formato de la dirección URL para la aplicación es *.azurewebsites {nombre de la aplicación} .net*.</span><span class="sxs-lookup"><span data-stu-id="4dddc-132">Notice the format of the URL for the app is *{app name}.azurewebsites.net*.</span></span> <span data-ttu-id="4dddc-133">Por ejemplo, una aplicación denominada `SignalRChattR` tiene una dirección URL similar a `https://signalrchattr.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="4dddc-133">For example, an app named `SignalRChattR` has a URL that looks like `https://signalrchattr.azurewebsites.net`.</span></span>

<span data-ttu-id="4dddc-134">Si se produce un error de HTTP 502.2, consulte [versión de vista previa de implementar ASP.NET Core para el servicio de aplicaciones de Azure](xref:host-and-deploy/azure-apps/index) para resolverlo.</span><span class="sxs-lookup"><span data-stu-id="4dddc-134">If an HTTP 502.2 error occurs, see [Deploy ASP.NET Core preview release to Azure App Service](xref:host-and-deploy/azure-apps/index) to resolve it.</span></span>

## <a name="configure-signalr-web-app"></a><span data-ttu-id="4dddc-135">Configurar la aplicación web de SignalR</span><span class="sxs-lookup"><span data-stu-id="4dddc-135">Configure SignalR web app</span></span>

<span data-ttu-id="4dddc-136">Las aplicaciones ASP.NET SignalR Core que se publican como una aplicación Web de Azure debe tener [ARR afinidad](https://en.wikipedia.org/wiki/Application_Request_Routing) habilitado.</span><span class="sxs-lookup"><span data-stu-id="4dddc-136">ASP.NET Core SignalR apps that are published as an Azure Web App must have [ARR Affinity](https://en.wikipedia.org/wiki/Application_Request_Routing) enabled.</span></span> <span data-ttu-id="4dddc-137">[WebSockets](xref:fundamentals/websockets) debe habilitarse para permitir que el transporte de WebSockets a función.</span><span class="sxs-lookup"><span data-stu-id="4dddc-137">[WebSockets](xref:fundamentals/websockets) should be enabled, to allow the WebSockets transport to function.</span></span>

<span data-ttu-id="4dddc-138">En el portal de Azure, vaya a **configuración de la aplicación** para su aplicación web.</span><span class="sxs-lookup"><span data-stu-id="4dddc-138">In the Azure portal, navigate to **App Settings** for your web app.</span></span> <span data-ttu-id="4dddc-139">Establecer **WebSockets** a **en**y compruebe **ARR afinidad** es **en**.</span><span class="sxs-lookup"><span data-stu-id="4dddc-139">Set **WebSockets** to **On**, and verify **ARR Affinity** is **On**.</span></span>

![Configuración de la aplicación Web Azure en el portal de Azure](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 <span data-ttu-id="4dddc-141">WebSockets y otros transportes [están limitados según el Plan de servicio de aplicaciones](/azure/azure-subscription-service-limits#app-service-limits).</span><span class="sxs-lookup"><span data-stu-id="4dddc-141">WebSockets and other transports [are limited based on the App Service Plan](/azure/azure-subscription-service-limits#app-service-limits).</span></span>

## <a name="related-resources"></a><span data-ttu-id="4dddc-142">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="4dddc-142">Related resources</span></span>

* [<span data-ttu-id="4dddc-143">Publicar una aplicación de ASP.NET Core en Azure con las herramientas de línea de comandos</span><span class="sxs-lookup"><span data-stu-id="4dddc-143">Publish an ASP.NET Core app to Azure with command line tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli?tabs=windows)
* [<span data-ttu-id="4dddc-144">Publicar una aplicación de ASP.NET Core en Azure con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4dddc-144">Publish an ASP.NET Core app to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)
* [<span data-ttu-id="4dddc-145">Hospedar e implementar aplicaciones de ASP.NET Core Preview en Azure</span><span class="sxs-lookup"><span data-stu-id="4dddc-145">Host and deploy ASP.NET Core Preview apps on Azure</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
