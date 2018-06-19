---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Publique la aplicación en Azure servicio de aplicaciones de Azure | Documentos de Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: cc8a9199144e9fac041435938ea8899374ea199f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30867819"
---
<a name="publish-the-app-to-azure-azure-app-service"></a><span data-ttu-id="8db95-102">Publique la aplicación en Azure servicio de aplicaciones de Azure</span><span class="sxs-lookup"><span data-stu-id="8db95-102">Publish the App to Azure Azure App Service</span></span>
====================
<span data-ttu-id="8db95-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8db95-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="8db95-104">Descargar el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="8db95-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="8db95-105">Como último paso, publicará la aplicación en Azure.</span><span class="sxs-lookup"><span data-stu-id="8db95-105">As the last step, you will publish the application to Azure.</span></span> <span data-ttu-id="8db95-106">En el Explorador de soluciones, haga clic en el proyecto y seleccione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="8db95-106">In Solution Explorer, right-click the project and select **Publish**.</span></span>

![](part-10/_static/image1.png)

<span data-ttu-id="8db95-107">Haga clic en **publicar** , se invoca el **Publicar Web** cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="8db95-107">Clicking **Publish** invokes the **Publish Web** dialog.</span></span> <span data-ttu-id="8db95-108">Si ha activado **Host en la nube** cuando se crea por primera vez el proyecto y, a continuación, la conexión y configuración ya está configurada.</span><span class="sxs-lookup"><span data-stu-id="8db95-108">If you checked **Host in Cloud** when you first created the project, then the connection and settings are already configured.</span></span> <span data-ttu-id="8db95-109">En ese caso, simplemente haga clic en el **configuración** ficha y compruebe &quot;ejecutar Code First Migrations&quot;.</span><span class="sxs-lookup"><span data-stu-id="8db95-109">In that case, just click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="8db95-110">(Si no activó **Host en la nube** al principio, a continuación, siga los pasos descritos en la [próxima sección](#new-website).)</span><span class="sxs-lookup"><span data-stu-id="8db95-110">(If you didn't check **Host in Cloud** at the beginning, then follow the steps in the [next section](#new-website).)</span></span>

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

<span data-ttu-id="8db95-111">Para implementar la aplicación, haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="8db95-111">To deploy the app, click **Publish**.</span></span> <span data-ttu-id="8db95-112">Puede ver el progreso de la publicación en el **Web publicar actividad** ventana.</span><span class="sxs-lookup"><span data-stu-id="8db95-112">You can view the publishing progress in the **Web Publish Activity** window.</span></span> <span data-ttu-id="8db95-113">(Desde el **vista** menú, seleccione **otras ventanas**, a continuación, seleccione **Web publicar actividad**.)</span><span class="sxs-lookup"><span data-stu-id="8db95-113">(From the **View** menu, select **Other Windows**, then select **Web Publish Activity**.)</span></span>

![](part-10/_static/image4.png)

<span data-ttu-id="8db95-114">Cuando Visual Studio finaliza la implementación de la aplicación, el explorador predeterminado se abre automáticamente en la dirección URL del sitio Web implementado y ahora se ejecuta la aplicación que creó en la nube.</span><span class="sxs-lookup"><span data-stu-id="8db95-114">When Visual Studio finishes deploying the app, the default browser automatically opens to the URL of the deployed website, and the application that you created is now running in the cloud.</span></span> <span data-ttu-id="8db95-115">La dirección URL en la barra de direcciones del explorador, se muestra que el sitio se está cargando desde Internet.</span><span class="sxs-lookup"><span data-stu-id="8db95-115">The URL in the browser address bar shows that the site is being loaded from the Internet.</span></span>

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a><span data-ttu-id="8db95-116">Implementar en un sitio Web nuevo</span><span class="sxs-lookup"><span data-stu-id="8db95-116">Deploying to a New Website</span></span>

<span data-ttu-id="8db95-117">Si no activó **Host en la nube** cuando creó el proyecto por primera vez, puede configurar una nueva aplicación web ahora.</span><span class="sxs-lookup"><span data-stu-id="8db95-117">If you did not check **Host in Cloud** when you first created the project, you can configure a new web app now.</span></span> <span data-ttu-id="8db95-118">En el Explorador de soluciones, haga clic en el proyecto y seleccione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="8db95-118">In Solution Explorer, right-click the project and select **Publish**.</span></span> <span data-ttu-id="8db95-119">Seleccione el **perfil** ficha y haga clic en **sitios Web de Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="8db95-119">Select the **Profile** tab and click **Microsoft Azure Websites**.</span></span> <span data-ttu-id="8db95-120">Si no están actualmente inició sesión Azure, se le pedirá que inicie sesión en.</span><span class="sxs-lookup"><span data-stu-id="8db95-120">If you aren't currently signed in to Azure, you will be prompted to sign in.</span></span>

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

<span data-ttu-id="8db95-121">En el **sitios Web existentes** cuadro de diálogo, haga clic en **nuevo**.</span><span class="sxs-lookup"><span data-stu-id="8db95-121">In the **Existing Websites** dialog, click **New**.</span></span>

![](part-10/_static/image9.png)

<span data-ttu-id="8db95-122">Escriba un nombre de sitio.</span><span class="sxs-lookup"><span data-stu-id="8db95-122">Enter a site name.</span></span> <span data-ttu-id="8db95-123">Seleccione la suscripción de Azure y la región.</span><span class="sxs-lookup"><span data-stu-id="8db95-123">Select your Azure subscription and the region.</span></span> <span data-ttu-id="8db95-124">En **servidor de base de datos**, seleccione **crear nuevo servidor**, o seleccione un servidor existente.</span><span class="sxs-lookup"><span data-stu-id="8db95-124">Under **Database server**, select **Create New Server**, or select an existing server.</span></span> <span data-ttu-id="8db95-125">Haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="8db95-125">Click **Create**.</span></span>

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

<span data-ttu-id="8db95-126">Haga clic en el **configuración** ficha y compruebe &quot;ejecutar Code First Migrations&quot;.</span><span class="sxs-lookup"><span data-stu-id="8db95-126">Click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="8db95-127">A continuación, haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="8db95-127">Then click **Publish**.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="8db95-128">Anterior</span><span class="sxs-lookup"><span data-stu-id="8db95-128">Previous</span></span>](part-9.md)
