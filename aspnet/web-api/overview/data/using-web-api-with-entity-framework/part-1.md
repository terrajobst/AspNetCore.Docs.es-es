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
<a name="using-web-api-2-with-entity-framework-6"></a><span data-ttu-id="9c1c0-104">Usar Web API 2 con Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="9c1c0-104">Using Web API 2 with Entity Framework 6</span></span>
====================

[<span data-ttu-id="9c1c0-105">Descargue el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="9c1c0-105">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

> <span data-ttu-id="9c1c0-106">Este tutorial le enseña los aspectos básicos de la creación de una aplicación web con ASP.NET Web API back-end.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-106">This tutorial teaches you the basics of creating a web application with an ASP.NET Web API back end.</span></span> <span data-ttu-id="9c1c0-107">El tutorial usa Entity Framework 6 para la capa de datos y Knockout.js para la aplicación de JavaScript del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-107">The tutorial uses Entity Framework 6 for the data layer, and Knockout.js for the client-side JavaScript application.</span></span> <span data-ttu-id="9c1c0-108">El tutorial también muestra cómo implementar la aplicación en Azure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-108">The tutorial also shows how to deploy the app to Azure App Service Web Apps.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="9c1c0-109">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="9c1c0-109">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="9c1c0-110">Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="9c1c0-110">Web API 2.1</span></span>
> - <span data-ttu-id="9c1c0-111">Visual Studio 2017 (Descargar Visual Studio 2017 [aquí](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span><span class="sxs-lookup"><span data-stu-id="9c1c0-111">Visual Studio 2017 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span></span>
> - <span data-ttu-id="9c1c0-112">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="9c1c0-112">Entity Framework 6</span></span>
> - <span data-ttu-id="9c1c0-113">.NET 4.7</span><span class="sxs-lookup"><span data-stu-id="9c1c0-113">.NET 4.7</span></span>
> - <span data-ttu-id="9c1c0-114">[Knockout.js](http://knockoutjs.com/) 3.1</span><span class="sxs-lookup"><span data-stu-id="9c1c0-114">[Knockout.js](http://knockoutjs.com/) 3.1</span></span>

<span data-ttu-id="9c1c0-115">Este tutorial usa para crear una aplicación web que se manipula una base de datos back-end ASP.NET Web API 2 con Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-115">This tutorial uses ASP.NET Web API 2 with Entity Framework 6 to create a web application that manipulates a back-end database.</span></span> <span data-ttu-id="9c1c0-116">Aquí es una captura de pantalla de la aplicación que va a crear.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-116">Here is a screen shot of the application that you will create.</span></span>

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

<span data-ttu-id="9c1c0-117">La aplicación utiliza un diseño de la aplicación de página única (SPA).</span><span class="sxs-lookup"><span data-stu-id="9c1c0-117">The app uses a single-page application (SPA) design.</span></span> <span data-ttu-id="9c1c0-118">"Aplicación de página única" es el término general para una aplicación web que se carga una página HTML única y, a continuación, actualiza la página de forma dinámica, en lugar de cargar páginas nuevas.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-118">"Single-page application" is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="9c1c0-119">Después de la carga de página inicial, la aplicación se comunica con el servidor a través de las solicitudes AJAX.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-119">After the initial page load, the app talks with the server through AJAX requests.</span></span> <span data-ttu-id="9c1c0-120">El AJAX solicita devolver datos JSON, que la aplicación usa para actualizar la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-120">The AJAX requests return JSON data, which the app uses to update the UI.</span></span>

<span data-ttu-id="9c1c0-121">AJAX no es nueva, pero hoy en día existen marcos de JavaScript que resulte más fácil crear y mantener una gran aplicación SPA sofisticada.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-121">AJAX isn't new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="9c1c0-122">Este tutorial se usa [Knockout.js](http://knockoutjs.com/), pero puede usar cualquier marco de cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-122">This tutorial uses [Knockout.js](http://knockoutjs.com/), but you can use any JavaScript client framework.</span></span>

<span data-ttu-id="9c1c0-123">Estos son los principales bloques de creación para esta aplicación:</span><span class="sxs-lookup"><span data-stu-id="9c1c0-123">Here are the main building blocks for this app:</span></span>

- <span data-ttu-id="9c1c0-124">ASP.NET MVC se crea la página HTML.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-124">ASP.NET MVC creates the HTML page.</span></span>
- <span data-ttu-id="9c1c0-125">ASP.NET Web API controla las solicitudes AJAX y devuelve datos JSON.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-125">ASP.NET Web API handles the AJAX requests and returns JSON data.</span></span>
- <span data-ttu-id="9c1c0-126">Knockout.js enlaza datos con los elementos HTML a los datos JSON.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-126">Knockout.js data-binds the HTML elements to the JSON data.</span></span>
- <span data-ttu-id="9c1c0-127">Entity Framework se comunica con la base de datos.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-127">Entity Framework talks to the database.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="9c1c0-128">Consulte esta aplicación se ejecuta en Azure</span><span class="sxs-lookup"><span data-stu-id="9c1c0-128">See this app running on Azure</span></span>

<span data-ttu-id="9c1c0-129">¿Desea ver el sitio terminado que se ejecuta como una aplicación web en directo?</span><span class="sxs-lookup"><span data-stu-id="9c1c0-129">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="9c1c0-130">Puede implementar una versión completa de la aplicación en su cuenta de Azure, seleccione el botón siguiente.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-130">You can deploy a complete version of the app to your Azure account by selecting the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

<span data-ttu-id="9c1c0-131">Necesita una cuenta de Azure para implementar esta solución en Azure.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-131">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="9c1c0-132">Si no dispone de una cuenta, tiene las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="9c1c0-132">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="9c1c0-133">[Abrir una cuenta de Azure de forma gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -obtiene crédito puede usar para probar los servicios de Azure de pago e incluso después de que se usan hasta, puede mantener la cuenta y usar servicios gratuitos de Azure.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-133">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="9c1c0-134">[Activar las ventajas de suscriptor MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -su suscripción a MSDN le proporciona crédito todos los meses que puede usar para servicios de Azure de pago.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-134">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="9c1c0-135">Crear el proyecto</span><span class="sxs-lookup"><span data-stu-id="9c1c0-135">Create the project</span></span>

<span data-ttu-id="9c1c0-136">Abra Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-136">Open Visual Studio.</span></span> <span data-ttu-id="9c1c0-137">Desde el **archivo** menú, seleccione **New**, a continuación, seleccione **proyecto**.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-137">From the **File** menu, select **New**, then select **Project**.</span></span> <span data-ttu-id="9c1c0-138">(O seleccione **nuevo proyecto** en la página de inicio.)</span><span class="sxs-lookup"><span data-stu-id="9c1c0-138">(Or select **New Project** on the Start page.)</span></span>

<span data-ttu-id="9c1c0-139">En el **nuevo proyecto** cuadro de diálogo, seleccione **Web** en el panel izquierdo y **aplicación Web ASP.NET (.NET Framework)** en el panel central.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-139">In the **New Project** dialog, select **Web** in the left pane and **ASP.NET Web Application (.NET Framework)** in the middle pane.</span></span> <span data-ttu-id="9c1c0-140">Denomine el proyecto **BookService** y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-140">Name the project **BookService** and select **OK**.</span></span>

[![](part-1/_static/image11.png)](part-1/_static/image11.png)

<span data-ttu-id="9c1c0-141">En el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione el **API Web** plantilla.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-141">In the **New ASP.NET Project** dialog, select the **Web API** template.</span></span>

[![](part-1/_static/image12.png)](part-1/_static/image12.png)


<span data-ttu-id="9c1c0-142">Seleccione **Aceptar** para crear el proyecto.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-142">Select **OK** to create the project.</span></span>

## <a name="configure-azure-settings-optional"></a><span data-ttu-id="9c1c0-143">Configurar valores de Azure (opcionales)</span><span class="sxs-lookup"><span data-stu-id="9c1c0-143">Configure Azure settings (optional)</span></span>

<span data-ttu-id="9c1c0-144">Después de crear el proyecto, puede implementar en Azure App Service Web Apps en cualquier momento.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-144">After you create the project, you can choose to deploy to Azure App Service Web Apps at any time.</span></span> 

1. <span data-ttu-id="9c1c0-145">En el Explorador de soluciones, haga doble clic en el proyecto y seleccione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-145">In Solution Explorer, right-click on your project and select **Publish**.</span></span>

2. <span data-ttu-id="9c1c0-146">En la ventana que aparece, seleccione **iniciar**.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-146">In the window that appears, select **Start**.</span></span> <span data-ttu-id="9c1c0-147">El **elegir un destino de publicación** aparece el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-147">The **Pick a publish target** dialog box appears.</span></span>

   [![](part-1/_static/image14.png)](part-1/_static/image14.png)

3. <span data-ttu-id="9c1c0-148">Seleccione **Crear perfil**.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-148">Select **Create Profile**.</span></span> <span data-ttu-id="9c1c0-149">Aparecerá el cuadro de diálogo **Crear servicio de aplicaciones**.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-149">The **Create App Service** dialog box appears.</span></span>

   [![](part-1/_static/image15.png)](part-1/_static/image15.png)

   <span data-ttu-id="9c1c0-150">Acepte los valores predeterminados o escribir valores diferentes para el nombre de aplicación, el grupo de recursos, hospedaje de plan, la suscripción de Azure y la región geográfica.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-150">Accept the defaults, or enter different values for the application name, resource group, hosting plan, Azure subscription, and geographical region.</span></span> 

4. <span data-ttu-id="9c1c0-151">Seleccione **crear una base de datos SQL**.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-151">Select **Create a SQL database**.</span></span> <span data-ttu-id="9c1c0-152">El **configurar SQL Server** aparece el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-152">The **Configure SQL Server** dialog box appears.</span></span> 

   [![](part-1/_static/image16.png)](part-1/_static/image16.png)

   <span data-ttu-id="9c1c0-153">Acepte los valores predeterminados o escribir valores diferentes.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-153">Accept the defaults or enter different values.</span></span> <span data-ttu-id="9c1c0-154">Escriba un **Adminstrator Username** y **contraseña de administrador** para la base de datos.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-154">Enter an **Adminstrator Username** and **Administrator Password** for your new database.</span></span> <span data-ttu-id="9c1c0-155">Seleccione **Aceptar** cuando haya terminado.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-155">Select **OK** when you're done.</span></span> <span data-ttu-id="9c1c0-156">El **crear App Service** página volverá a aparecer.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-156">The **Create App Service** page reappears.</span></span>

5. <span data-ttu-id="9c1c0-157">Seleccione **crear** para crear su perfil.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-157">Select **Create** to create your profile.</span></span> <span data-ttu-id="9c1c0-158">Aparece un mensaje en la esquina inferior derecha, que indica que la implementación está en curso.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-158">A message appears in the lower-right corner indicating that deployment is in progress.</span></span> <span data-ttu-id="9c1c0-159">Después de unos momentos, el **publicar** ventana vuelve a aparecer.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-159">After a short while, the **Publish** window reappears.</span></span>

    [![](part-1/_static/image17.png)](part-1/_static/image17.png)
   
    <span data-ttu-id="9c1c0-160">El perfil que creó para implementar la aplicación ahora está disponible.</span><span class="sxs-lookup"><span data-stu-id="9c1c0-160">The profile you created to deploy the app is now available.</span></span> 


> [!div class="step-by-step"]
> [<span data-ttu-id="9c1c0-161">Siguiente</span><span class="sxs-lookup"><span data-stu-id="9c1c0-161">Next</span></span>](part-2.md)
