---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Usar Web API 2 con Entity Framework 6 | Documentos de Microsoft
author: MikeWasson
description: Este tutorial le enseñará a los conceptos básicos de creación de una aplicación web con ASP.NET Web API back-end. El tutorial usa Entity Framework 6 para el diseño de datos...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 8e6d381509a121e3036ca3af91ea3b9bd0be33c2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="using-web-api-2-with-entity-framework-6"></a><span data-ttu-id="6c6a8-104">Usar Web API 2 con Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="6c6a8-104">Using Web API 2 with Entity Framework 6</span></span>
====================
<span data-ttu-id="6c6a8-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6c6a8-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="6c6a8-106">Descargar el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="6c6a8-106">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

> <span data-ttu-id="6c6a8-107">Este tutorial le enseñará a los conceptos básicos de creación de una aplicación web con ASP.NET Web API back-end.</span><span class="sxs-lookup"><span data-stu-id="6c6a8-107">This tutorial will teach you the basics of creating a web application with an ASP.NET Web API back end.</span></span> <span data-ttu-id="6c6a8-108">El tutorial usa Entity Framework 6 para la capa de datos y Knockout.js para la aplicación de JavaScript del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="6c6a8-108">The tutorial uses Entity Framework 6 for the data layer, and Knockout.js for the client-side JavaScript application.</span></span> <span data-ttu-id="6c6a8-109">El tutorial también muestra cómo implementar la aplicación para aplicaciones de Web del servicio de aplicación de Azure.</span><span class="sxs-lookup"><span data-stu-id="6c6a8-109">The tutorial also shows how to deploy the app to Azure App Service Web Apps.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6c6a8-110">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="6c6a8-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="6c6a8-111">Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="6c6a8-111">Web API 2.1</span></span>
> - [<span data-ttu-id="6c6a8-112">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="6c6a8-112">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="6c6a8-113">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="6c6a8-113">Entity Framework 6</span></span>
> - <span data-ttu-id="6c6a8-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="6c6a8-114">.NET 4.5</span></span>
> - <span data-ttu-id="6c6a8-115">[Knockout.js](http://knockoutjs.com/) 3.1</span><span class="sxs-lookup"><span data-stu-id="6c6a8-115">[Knockout.js](http://knockoutjs.com/) 3.1</span></span>


<span data-ttu-id="6c6a8-116">Este tutorial usa ASP.NET Web API 2 con Entity Framework 6 para crear una aplicación web que se manipula una base de datos back-end.</span><span class="sxs-lookup"><span data-stu-id="6c6a8-116">This tutorial uses ASP.NET Web API 2 with Entity Framework 6 to create a web application that manipulates a back-end database.</span></span> <span data-ttu-id="6c6a8-117">Aquí se muestra una captura de pantalla de la aplicación que va a crear.</span><span class="sxs-lookup"><span data-stu-id="6c6a8-117">Here is a screen shot of the application that you will create.</span></span>

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

<span data-ttu-id="6c6a8-118">La aplicación utiliza un diseño de la aplicación de página (SPA).</span><span class="sxs-lookup"><span data-stu-id="6c6a8-118">The app uses a single-page application (SPA) design.</span></span> <span data-ttu-id="6c6a8-119">"Aplicación de página" es el término general para una aplicación web que se carga una página HTML única y, a continuación, actualiza la página de forma dinámica, en lugar de cargar páginas nuevas.</span><span class="sxs-lookup"><span data-stu-id="6c6a8-119">"Single-page application" is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="6c6a8-120">Después de la carga de la página inicial, la aplicación se comunica con el servidor a través de solicitudes de AJAX.</span><span class="sxs-lookup"><span data-stu-id="6c6a8-120">After the initial page load, the app talks with the server through AJAX requests.</span></span> <span data-ttu-id="6c6a8-121">El AJAX solicita devolver datos JSON, que utiliza la aplicación para actualizar la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="6c6a8-121">The AJAX requests return JSON data, which the app uses to update the UI.</span></span>

<span data-ttu-id="6c6a8-122">AJAX no es nueva, pero en la actualidad existen marcos de JavaScript que resulten más fácil generar y mantener una gran aplicación SPA sofisticada.</span><span class="sxs-lookup"><span data-stu-id="6c6a8-122">AJAX isn't new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="6c6a8-123">Este tutorial usa [Knockout.js](http://knockoutjs.com/), pero puede usar cualquier marco de cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6c6a8-123">This tutorial uses [Knockout.js](http://knockoutjs.com/), but you can use any JavaScript client framework.</span></span>

<span data-ttu-id="6c6a8-124">Estos son los principales bloques de creación para esta aplicación:</span><span class="sxs-lookup"><span data-stu-id="6c6a8-124">Here are the main building blocks for this app:</span></span>

- <span data-ttu-id="6c6a8-125">ASP.NET MVC se crea la página HTML.</span><span class="sxs-lookup"><span data-stu-id="6c6a8-125">ASP.NET MVC creates the HTML page.</span></span>
- <span data-ttu-id="6c6a8-126">API Web de ASP.NET controla las solicitudes de AJAX y devuelve datos JSON.</span><span class="sxs-lookup"><span data-stu-id="6c6a8-126">ASP.NET Web API handles the AJAX requests and returns JSON data.</span></span>
- <span data-ttu-id="6c6a8-127">Knockout.js enlaza datos con los elementos HTML a los datos JSON.</span><span class="sxs-lookup"><span data-stu-id="6c6a8-127">Knockout.js data-binds the HTML elements to the JSON data.</span></span>
- <span data-ttu-id="6c6a8-128">Entity Framework se comunica con la base de datos.</span><span class="sxs-lookup"><span data-stu-id="6c6a8-128">Entity Framework talks to the database.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="6c6a8-129">Vea esta aplicación se ejecuta en Azure</span><span class="sxs-lookup"><span data-stu-id="6c6a8-129">See this App Running on Azure</span></span>

<span data-ttu-id="6c6a8-130">¿Desea ver el sitio terminado que se ejecuta como una aplicación web en directo?</span><span class="sxs-lookup"><span data-stu-id="6c6a8-130">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="6c6a8-131">Puede implementar una versión completa de la aplicación en su cuenta de Azure, simplemente haga clic en el botón siguiente.</span><span class="sxs-lookup"><span data-stu-id="6c6a8-131">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

<span data-ttu-id="6c6a8-132">Necesita una cuenta de Azure para implementar esta solución en Azure.</span><span class="sxs-lookup"><span data-stu-id="6c6a8-132">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="6c6a8-133">Si no dispone de una cuenta, tiene las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="6c6a8-133">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="6c6a8-134">[Abrir una cuenta de Azure de forma gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -obtendrá créditos puede usar para probar los servicios de Azure de pagados e incluso después de que se utilizan hasta puede mantener la cuenta y libre de usar los servicios de Azure.</span><span class="sxs-lookup"><span data-stu-id="6c6a8-134">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="6c6a8-135">[Activar las ventajas de suscriptor MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -su suscripción a MSDN ofrece créditos cada mes que puede usar para los servicios de Azure de pagados.</span><span class="sxs-lookup"><span data-stu-id="6c6a8-135">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="6c6a8-136">Crear el proyecto</span><span class="sxs-lookup"><span data-stu-id="6c6a8-136">Create the Project</span></span>

<span data-ttu-id="6c6a8-137">Abra Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6c6a8-137">Open Visual Studio.</span></span> <span data-ttu-id="6c6a8-138">Desde el **archivo** menú, seleccione **New**, a continuación, seleccione **proyecto**.</span><span class="sxs-lookup"><span data-stu-id="6c6a8-138">From the **File** menu, select **New**, then select **Project**.</span></span> <span data-ttu-id="6c6a8-139">(O haga clic en **nuevo proyecto** en la página de inicio.)</span><span class="sxs-lookup"><span data-stu-id="6c6a8-139">(Or click **New Project** on the Start page.)</span></span>

<span data-ttu-id="6c6a8-140">En el **nuevo proyecto** cuadro de diálogo, haga clic en **Web** en el panel izquierdo y **aplicación Web ASP.NET** en el panel central.</span><span class="sxs-lookup"><span data-stu-id="6c6a8-140">In the **New Project** dialog, click **Web** in the left pane and **ASP.NET Web Application** in the middle pane.</span></span> <span data-ttu-id="6c6a8-141">Denomine el proyecto BookService y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="6c6a8-141">Name the project BookService and click **OK**.</span></span>

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

<span data-ttu-id="6c6a8-142">En el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione la **API Web** plantilla.</span><span class="sxs-lookup"><span data-stu-id="6c6a8-142">In the **New ASP.NET Project** dialog, select the **Web API** template.</span></span>

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

<span data-ttu-id="6c6a8-143">Si desea hospedar el proyecto en un servicio de aplicaciones de Azure, deje el **Host en la nube** casilla activada.</span><span class="sxs-lookup"><span data-stu-id="6c6a8-143">If you want to host the project in a Azure App Service, leave the **Host in the cloud** box checked.</span></span>

<span data-ttu-id="6c6a8-144">Haga clic en **Aceptar** para crear el proyecto.</span><span class="sxs-lookup"><span data-stu-id="6c6a8-144">Click **OK** to create the project.</span></span>

## <a name="configure-azure-settings-optional"></a><span data-ttu-id="6c6a8-145">Configurar Azure (opcional)</span><span class="sxs-lookup"><span data-stu-id="6c6a8-145">Configure Azure Settings (Optional)</span></span>

<span data-ttu-id="6c6a8-146">Si deja el **Host en la nube** activa la opción, Visual Studio le pedirá que inicie sesión en Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="6c6a8-146">If you left the **Host in Cloud** option checked, Visual Studio will prompt you to sign in to Microsoft Azure</span></span>

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

<span data-ttu-id="6c6a8-147">Después de iniciar sesión Azure, Visual Studio le pide que configure la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="6c6a8-147">After you sign in to Azure, Visual Studio prompts you to configure the web app.</span></span> <span data-ttu-id="6c6a8-148">Escriba un nombre para el sitio, seleccione la suscripción de Azure y seleccione una región geográfica.</span><span class="sxs-lookup"><span data-stu-id="6c6a8-148">Enter a name for the site, select your Azure subscription, and select a geographical region.</span></span> <span data-ttu-id="6c6a8-149">En **servidor de base de datos**, seleccione **crear nuevo servidor**.</span><span class="sxs-lookup"><span data-stu-id="6c6a8-149">Under **Database server**, select **Create new server**.</span></span> <span data-ttu-id="6c6a8-150">Escriba un nombre de usuario de administrador y una contraseña.</span><span class="sxs-lookup"><span data-stu-id="6c6a8-150">Enter an administrator username and password.</span></span>

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

> [!div class="step-by-step"]
> [<span data-ttu-id="6c6a8-151">Siguiente</span><span class="sxs-lookup"><span data-stu-id="6c6a8-151">Next</span></span>](part-2.md)
