---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Publicar la aplicación en Azure App Service de Azure | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: 5c1a70ceded85681046065881f62c5c95c5d8740
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838439"
---
<a name="publish-the-app-to-azure-azure-app-service"></a><span data-ttu-id="8838d-102">Publicar la aplicación en Azure App Service de Azure</span><span class="sxs-lookup"><span data-stu-id="8838d-102">Publish the App to Azure Azure App Service</span></span>
====================
<span data-ttu-id="8838d-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8838d-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="8838d-104">Descargue el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="8838d-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="8838d-105">Como último paso, publicará la aplicación en Azure.</span><span class="sxs-lookup"><span data-stu-id="8838d-105">As the last step, you will publish the application to Azure.</span></span> <span data-ttu-id="8838d-106">En el Explorador de soluciones, haga clic en el proyecto y seleccione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="8838d-106">In Solution Explorer, right-click the project and select **Publish**.</span></span>

![](part-10/_static/image1.png)

<span data-ttu-id="8838d-107">Al hacer clic en **publicar** invoca el **publicación Web** cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="8838d-107">Clicking **Publish** invokes the **Publish Web** dialog.</span></span> <span data-ttu-id="8838d-108">Si ha activado **Host en la nube** cuando creó por primera vez el proyecto y, a continuación, la conexión y la configuración ya está configurada.</span><span class="sxs-lookup"><span data-stu-id="8838d-108">If you checked **Host in Cloud** when you first created the project, then the connection and settings are already configured.</span></span> <span data-ttu-id="8838d-109">En ese caso, simplemente haga clic en el **configuración** pestaña y compruebe &quot;ejecutar migraciones Code First&quot;.</span><span class="sxs-lookup"><span data-stu-id="8838d-109">In that case, just click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="8838d-110">(Si no comprobara **Host en la nube** al principio, a continuación, siga los pasos descritos en la [siguiente sección](#new-website).)</span><span class="sxs-lookup"><span data-stu-id="8838d-110">(If you didn't check **Host in Cloud** at the beginning, then follow the steps in the [next section](#new-website).)</span></span>

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

<span data-ttu-id="8838d-111">Para implementar la aplicación, haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="8838d-111">To deploy the app, click **Publish**.</span></span> <span data-ttu-id="8838d-112">Puede ver el progreso de la publicación en el **actividad de publicación Web** ventana.</span><span class="sxs-lookup"><span data-stu-id="8838d-112">You can view the publishing progress in the **Web Publish Activity** window.</span></span> <span data-ttu-id="8838d-113">(Desde el **vista** menú, seleccione **Other Windows**, a continuación, seleccione **actividad de publicación Web**.)</span><span class="sxs-lookup"><span data-stu-id="8838d-113">(From the **View** menu, select **Other Windows**, then select **Web Publish Activity**.)</span></span>

![](part-10/_static/image4.png)

<span data-ttu-id="8838d-114">Cuando Visual Studio termine de implementar la aplicación, el explorador predeterminado se abre automáticamente en la dirección URL del sitio Web implementado, y ahora se ejecuta la aplicación que creó en la nube.</span><span class="sxs-lookup"><span data-stu-id="8838d-114">When Visual Studio finishes deploying the app, the default browser automatically opens to the URL of the deployed website, and the application that you created is now running in the cloud.</span></span> <span data-ttu-id="8838d-115">La dirección URL en la barra de direcciones del explorador muestra que el sitio se está cargando desde Internet.</span><span class="sxs-lookup"><span data-stu-id="8838d-115">The URL in the browser address bar shows that the site is being loaded from the Internet.</span></span>

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a><span data-ttu-id="8838d-116">Implementación en un sitio Web nuevo</span><span class="sxs-lookup"><span data-stu-id="8838d-116">Deploying to a New Website</span></span>

<span data-ttu-id="8838d-117">Si no comprobara **Host en la nube** cuando creó por primera vez el proyecto, puede configurar una nueva aplicación web ahora.</span><span class="sxs-lookup"><span data-stu-id="8838d-117">If you did not check **Host in Cloud** when you first created the project, you can configure a new web app now.</span></span> <span data-ttu-id="8838d-118">En el Explorador de soluciones, haga clic en el proyecto y seleccione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="8838d-118">In Solution Explorer, right-click the project and select **Publish**.</span></span> <span data-ttu-id="8838d-119">Seleccione el **perfil** ficha y haga clic en **Microsoft Azure Websites**.</span><span class="sxs-lookup"><span data-stu-id="8838d-119">Select the **Profile** tab and click **Microsoft Azure Websites**.</span></span> <span data-ttu-id="8838d-120">Si actualmente no ha iniciado Azure, se le pedirá que inicie sesión.</span><span class="sxs-lookup"><span data-stu-id="8838d-120">If you aren't currently signed in to Azure, you will be prompted to sign in.</span></span>

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

<span data-ttu-id="8838d-121">En el **sitios Web existentes** cuadro de diálogo, haga clic en **New**.</span><span class="sxs-lookup"><span data-stu-id="8838d-121">In the **Existing Websites** dialog, click **New**.</span></span>

![](part-10/_static/image9.png)

<span data-ttu-id="8838d-122">Escriba un nombre de sitio.</span><span class="sxs-lookup"><span data-stu-id="8838d-122">Enter a site name.</span></span> <span data-ttu-id="8838d-123">Seleccione su suscripción de Azure y la región.</span><span class="sxs-lookup"><span data-stu-id="8838d-123">Select your Azure subscription and the region.</span></span> <span data-ttu-id="8838d-124">En **el servidor de base de datos**, seleccione **crear nuevo servidor**, o seleccione un servidor existente.</span><span class="sxs-lookup"><span data-stu-id="8838d-124">Under **Database server**, select **Create New Server**, or select an existing server.</span></span> <span data-ttu-id="8838d-125">Haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="8838d-125">Click **Create**.</span></span>

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

<span data-ttu-id="8838d-126">Haga clic en el **configuración** pestaña y compruebe &quot;ejecutar migraciones Code First&quot;.</span><span class="sxs-lookup"><span data-stu-id="8838d-126">Click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="8838d-127">A continuación, haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="8838d-127">Then click **Publish**.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="8838d-128">Anterior</span><span class="sxs-lookup"><span data-stu-id="8838d-128">Previous</span></span>](part-9.md)
