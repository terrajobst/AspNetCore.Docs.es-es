---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Usar Web API 2 con Entity Framework 6 | Microsoft Docs
author: MikeWasson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación web con ASP.NET Web API back-end. Este tutorial usa Entity Framework 6 para el diseño de datos...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: d65c0ea35ec766ef9d9093c6502230f9de72a3f3
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795219"
---
<a name="using-web-api-2-with-entity-framework-6"></a><span data-ttu-id="913ed-104">Usar Web API 2 con Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="913ed-104">Using Web API 2 with Entity Framework 6</span></span>
====================
<span data-ttu-id="913ed-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="913ed-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="913ed-106">Descargue el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="913ed-106">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

> <span data-ttu-id="913ed-107">Este tutorial le enseñará los aspectos básicos de la creación de una aplicación web con ASP.NET Web API back-end.</span><span class="sxs-lookup"><span data-stu-id="913ed-107">This tutorial will teach you the basics of creating a web application with an ASP.NET Web API back end.</span></span> <span data-ttu-id="913ed-108">El tutorial usa Entity Framework 6 para la capa de datos y Knockout.js para la aplicación de JavaScript del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="913ed-108">The tutorial uses Entity Framework 6 for the data layer, and Knockout.js for the client-side JavaScript application.</span></span> <span data-ttu-id="913ed-109">El tutorial también muestra cómo implementar la aplicación en Azure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="913ed-109">The tutorial also shows how to deploy the app to Azure App Service Web Apps.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="913ed-110">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="913ed-110">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="913ed-111">Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="913ed-111">Web API 2.1</span></span>
> - <span data-ttu-id="913ed-112">Visual Studio 2013 (Descargar Visual Studio 2017 [aquí](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span><span class="sxs-lookup"><span data-stu-id="913ed-112">Visual Studio 2013 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span></span>
> - <span data-ttu-id="913ed-113">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="913ed-113">Entity Framework 6</span></span>
> - <span data-ttu-id="913ed-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="913ed-114">.NET 4.5</span></span>
> - <span data-ttu-id="913ed-115">[Knockout.js](http://knockoutjs.com/) 3.1</span><span class="sxs-lookup"><span data-stu-id="913ed-115">[Knockout.js](http://knockoutjs.com/) 3.1</span></span>

<span data-ttu-id="913ed-116">Este tutorial usa para crear una aplicación web que se manipula una base de datos back-end ASP.NET Web API 2 con Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="913ed-116">This tutorial uses ASP.NET Web API 2 with Entity Framework 6 to create a web application that manipulates a back-end database.</span></span> <span data-ttu-id="913ed-117">Aquí es una captura de pantalla de la aplicación que va a crear.</span><span class="sxs-lookup"><span data-stu-id="913ed-117">Here is a screen shot of the application that you will create.</span></span>

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

<span data-ttu-id="913ed-118">La aplicación utiliza un diseño de la aplicación de página única (SPA).</span><span class="sxs-lookup"><span data-stu-id="913ed-118">The app uses a single-page application (SPA) design.</span></span> <span data-ttu-id="913ed-119">"Aplicación de página única" es el término general para una aplicación web que se carga una página HTML única y, a continuación, actualiza la página de forma dinámica, en lugar de cargar páginas nuevas.</span><span class="sxs-lookup"><span data-stu-id="913ed-119">"Single-page application" is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="913ed-120">Después de la carga de página inicial, la aplicación se comunica con el servidor a través de las solicitudes AJAX.</span><span class="sxs-lookup"><span data-stu-id="913ed-120">After the initial page load, the app talks with the server through AJAX requests.</span></span> <span data-ttu-id="913ed-121">El AJAX solicita devolver datos JSON, que la aplicación usa para actualizar la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="913ed-121">The AJAX requests return JSON data, which the app uses to update the UI.</span></span>

<span data-ttu-id="913ed-122">AJAX no es nueva, pero hoy en día existen marcos de JavaScript que resulte más fácil crear y mantener una gran aplicación SPA sofisticada.</span><span class="sxs-lookup"><span data-stu-id="913ed-122">AJAX isn't new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="913ed-123">Este tutorial se usa [Knockout.js](http://knockoutjs.com/), pero puede usar cualquier marco de cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="913ed-123">This tutorial uses [Knockout.js](http://knockoutjs.com/), but you can use any JavaScript client framework.</span></span>

<span data-ttu-id="913ed-124">Estos son los principales bloques de creación para esta aplicación:</span><span class="sxs-lookup"><span data-stu-id="913ed-124">Here are the main building blocks for this app:</span></span>

- <span data-ttu-id="913ed-125">ASP.NET MVC se crea la página HTML.</span><span class="sxs-lookup"><span data-stu-id="913ed-125">ASP.NET MVC creates the HTML page.</span></span>
- <span data-ttu-id="913ed-126">ASP.NET Web API controla las solicitudes AJAX y devuelve datos JSON.</span><span class="sxs-lookup"><span data-stu-id="913ed-126">ASP.NET Web API handles the AJAX requests and returns JSON data.</span></span>
- <span data-ttu-id="913ed-127">Knockout.js enlaza datos con los elementos HTML a los datos JSON.</span><span class="sxs-lookup"><span data-stu-id="913ed-127">Knockout.js data-binds the HTML elements to the JSON data.</span></span>
- <span data-ttu-id="913ed-128">Entity Framework se comunica con la base de datos.</span><span class="sxs-lookup"><span data-stu-id="913ed-128">Entity Framework talks to the database.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="913ed-129">Consulte esta aplicación se ejecuta en Azure</span><span class="sxs-lookup"><span data-stu-id="913ed-129">See this App Running on Azure</span></span>

<span data-ttu-id="913ed-130">¿Desea ver el sitio terminado que se ejecuta como una aplicación web en directo?</span><span class="sxs-lookup"><span data-stu-id="913ed-130">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="913ed-131">Puede implementar una versión completa de la aplicación en su cuenta de Azure, simplemente haga clic en el botón siguiente.</span><span class="sxs-lookup"><span data-stu-id="913ed-131">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

<span data-ttu-id="913ed-132">Necesita una cuenta de Azure para implementar esta solución en Azure.</span><span class="sxs-lookup"><span data-stu-id="913ed-132">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="913ed-133">Si no dispone de una cuenta, tiene las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="913ed-133">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="913ed-134">[Abrir una cuenta de Azure de forma gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -obtiene crédito puede usar para probar los servicios de Azure de pago e incluso después de que se usan hasta, puede mantener la cuenta y usar servicios gratuitos de Azure.</span><span class="sxs-lookup"><span data-stu-id="913ed-134">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="913ed-135">[Activar las ventajas de suscriptor MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -su suscripción a MSDN le proporciona crédito todos los meses que puede usar para servicios de Azure de pago.</span><span class="sxs-lookup"><span data-stu-id="913ed-135">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="913ed-136">Crear el proyecto</span><span class="sxs-lookup"><span data-stu-id="913ed-136">Create the Project</span></span>

<span data-ttu-id="913ed-137">Abra Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="913ed-137">Open Visual Studio.</span></span> <span data-ttu-id="913ed-138">Desde el **archivo** menú, seleccione **New**, a continuación, seleccione **proyecto**.</span><span class="sxs-lookup"><span data-stu-id="913ed-138">From the **File** menu, select **New**, then select **Project**.</span></span> <span data-ttu-id="913ed-139">(O haga clic en **nuevo proyecto** en la página de inicio.)</span><span class="sxs-lookup"><span data-stu-id="913ed-139">(Or click **New Project** on the Start page.)</span></span>

<span data-ttu-id="913ed-140">En el **nuevo proyecto** cuadro de diálogo, haga clic en **Web** en el panel izquierdo y **aplicación Web ASP.NET** en el panel central.</span><span class="sxs-lookup"><span data-stu-id="913ed-140">In the **New Project** dialog, click **Web** in the left pane and **ASP.NET Web Application** in the middle pane.</span></span> <span data-ttu-id="913ed-141">Denomine el proyecto BookService y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="913ed-141">Name the project BookService and click **OK**.</span></span>

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

<span data-ttu-id="913ed-142">En el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione el **API Web** plantilla.</span><span class="sxs-lookup"><span data-stu-id="913ed-142">In the **New ASP.NET Project** dialog, select the **Web API** template.</span></span>

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

<span data-ttu-id="913ed-143">Si desea hospedar el proyecto en un Azure App Service, deje el **Host en la nube** casilla activada.</span><span class="sxs-lookup"><span data-stu-id="913ed-143">If you want to host the project in a Azure App Service, leave the **Host in the cloud** box checked.</span></span>

<span data-ttu-id="913ed-144">Haga clic en **Aceptar** para crear el proyecto.</span><span class="sxs-lookup"><span data-stu-id="913ed-144">Click **OK** to create the project.</span></span>

## <a name="configure-azure-settings-optional"></a><span data-ttu-id="913ed-145">Configurar valores de Azure (opcionales)</span><span class="sxs-lookup"><span data-stu-id="913ed-145">Configure Azure Settings (Optional)</span></span>

<span data-ttu-id="913ed-146">Si deja el **Host en la nube** opción activada, Visual Studio le pedirá que inicie sesión en Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="913ed-146">If you left the **Host in Cloud** option checked, Visual Studio will prompt you to sign in to Microsoft Azure</span></span>

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

<span data-ttu-id="913ed-147">Después de iniciar sesión Azure, Visual Studio le pedirá que configure la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="913ed-147">After you sign in to Azure, Visual Studio prompts you to configure the web app.</span></span> <span data-ttu-id="913ed-148">Escriba un nombre para el sitio, seleccione su suscripción de Azure y seleccione una región geográfica.</span><span class="sxs-lookup"><span data-stu-id="913ed-148">Enter a name for the site, select your Azure subscription, and select a geographical region.</span></span> <span data-ttu-id="913ed-149">En **el servidor de base de datos**, seleccione **crear nuevo servidor**.</span><span class="sxs-lookup"><span data-stu-id="913ed-149">Under **Database server**, select **Create new server**.</span></span> <span data-ttu-id="913ed-150">Escriba un nombre de usuario de administrador y una contraseña.</span><span class="sxs-lookup"><span data-stu-id="913ed-150">Enter an administrator username and password.</span></span>

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

> [!div class="step-by-step"]
> [<span data-ttu-id="913ed-151">Siguiente</span><span class="sxs-lookup"><span data-stu-id="913ed-151">Next</span></span>](part-2.md)
