---
title: "Implementación continua en Azure con Visual Studio y Git"
author: rick-anderson
description: "Aprenda a crear una aplicación web de ASP.NET Core con Visual Studio y a implementarla en Azure App Service con Git para una implementación continua."
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 2707c7a8-2350-4304-9856-fda58e5c0a16
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/azure-continuous-deployment
ms.openlocfilehash: f7ea2e76fdee19a3d964e42053f0060a0a505e5b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="continuous-deployment-to-azure-for-aspnet-core-with-visual-studio-and-git"></a><span data-ttu-id="92a67-104">Implementación continua en Azure para ASP.NET Core con Visual Studio y Git</span><span class="sxs-lookup"><span data-stu-id="92a67-104">Continuous deployment to Azure for ASP.NET Core, with Visual Studio and Git</span></span>

<span data-ttu-id="92a67-105">Por [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="92a67-105">By [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="92a67-106">En este tutorial se muestra cómo crear una aplicación web de ASP.NET Core con Visual Studio y a implementarla desde Visual Studio en Azure App Service mediante una implementación continua.</span><span class="sxs-lookup"><span data-stu-id="92a67-106">This tutorial shows you how to create an ASP.NET Core web app using Visual Studio and deploy it from Visual Studio to Azure App Service using continuous deployment.</span></span> 

<span data-ttu-id="92a67-107">Vea también [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic) (Usar VSTS para crear y publicar una aplicación web de Azure con la implementación continua), donde se muestra cómo configurar un flujo de trabajo de una entrega continua (CD) para [Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) con Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="92a67-107">See also [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic), which shows how to configure a continuous delivery (CD) workflow for [Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) using Visual Studio Team Services.</span></span> <span data-ttu-id="92a67-108">La entrega continua de Azure en Team Services simplifica la configuración de una canalización de implementación sólida para publicar actualizaciones de la aplicación en Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="92a67-108">Azure Continuous Delivery in Team Services simplifies setting up a robust deployment pipeline to publish updates for your app to Azure App Service.</span></span> <span data-ttu-id="92a67-109">La canalización se puede configurar desde Azure Portal para crear y ejecutar pruebas, implementarlas en un espacio de ensayo y luego implementarlas en un entorno de producción.</span><span class="sxs-lookup"><span data-stu-id="92a67-109">The pipeline can be configured from the Azure portal to build, run tests, deploy to a staging slot,  and then deploy to production.</span></span>

> [!NOTE]
> <span data-ttu-id="92a67-110">Para seguir este tutorial necesita una cuenta de Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="92a67-110">To complete this tutorial, you need a Microsoft Azure account.</span></span> <span data-ttu-id="92a67-111">Si no tiene una, puede [activar las ventajas como suscriptor a MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) o [registrarse para obtener una evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="92a67-111">If you don't have an account, you can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="92a67-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="92a67-112">Prerequisites</span></span>

<span data-ttu-id="92a67-113">En este tutorial se da por hecho que ya ha instalado lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="92a67-113">This tutorial assumes you have already installed the following:</span></span>

* [<span data-ttu-id="92a67-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="92a67-114">Visual Studio</span></span>](https://www.visualstudio.com)

* <span data-ttu-id="92a67-115">[ASP.NET Core](https://www.microsoft.com/net/download/core) (tiempo de ejecución y herramientas)</span><span class="sxs-lookup"><span data-stu-id="92a67-115">[ASP.NET Core](https://www.microsoft.com/net/download/core) (runtime and tooling)</span></span>

* <span data-ttu-id="92a67-116">[Git](https://git-scm.com/downloads) para Windows</span><span class="sxs-lookup"><span data-stu-id="92a67-116">[Git](https://git-scm.com/downloads) for Windows</span></span>

## <a name="create-an-aspnet-core-web-app"></a><span data-ttu-id="92a67-117">Crear una aplicación web de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="92a67-117">Create an ASP.NET Core web app</span></span>

1. <span data-ttu-id="92a67-118">Inicie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="92a67-118">Start Visual Studio.</span></span>

2. <span data-ttu-id="92a67-119">En el menú **Archivo**, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="92a67-119">From the **File** menu, select **New** > **Project**.</span></span>

3. <span data-ttu-id="92a67-120">Seleccione la plantilla de proyecto **Aplicación web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="92a67-120">Select the **ASP.NET Core Web Application** project template.</span></span> <span data-ttu-id="92a67-121">Aparece en **Instalados** > **Plantillas** > **Visual C#** > **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="92a67-121">It appears under **Installed** > **Templates** > **Visual C#** > **.NET Core**.</span></span> <span data-ttu-id="92a67-122">Dé un nombre al proyecto `SampleWebAppDemo`.</span><span class="sxs-lookup"><span data-stu-id="92a67-122">Name the project `SampleWebAppDemo`.</span></span> <span data-ttu-id="92a67-123">Seleccione la opción **Crear nuevo repositorio de Git** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="92a67-123">Select the **Create new Git respository** option and click **OK**.</span></span>

   ![Cuadro de diálogo Nuevo proyecto](azure-continuous-deployment/_static/01-new-project.png)

4. <span data-ttu-id="92a67-125">En el cuadro de diálogo **Nuevo proyecto ASP.NET Core**, seleccione la plantilla **Vacía** de ASP.NET Core y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="92a67-125">In the **New ASP.NET Core Project** dialog, select the ASP.NET Core **Empty** template, then click **OK**.</span></span>

   ![Cuadro de diálogo Nuevo proyecto ASP.NET](azure-continuous-deployment/_static/02-web-site-template.png)

>[!NOTE]
    ><span data-ttu-id="92a67-127">La última versión de .NET Core es la 2.0</span><span class="sxs-lookup"><span data-stu-id="92a67-127">Most recent release of .NET Core is 2.0</span></span>

### <a name="running-the-web-app-locally"></a><span data-ttu-id="92a67-128">Ejecutar la aplicación web de forma local</span><span class="sxs-lookup"><span data-stu-id="92a67-128">Running the web app locally</span></span>

1. <span data-ttu-id="92a67-129">Cuando Visual Studio haya acabado de crear la aplicación, ejecútela seleccionando **Depurar** -> **Iniciar depuración**.</span><span class="sxs-lookup"><span data-stu-id="92a67-129">Once Visual Studio finishes creating the app, run the app by selecting **Debug** -> **Start Debugging**.</span></span> <span data-ttu-id="92a67-130">Como alternativa puede pulsar **F5**.</span><span class="sxs-lookup"><span data-stu-id="92a67-130">As an alternative, you can press **F5**.</span></span>

   <span data-ttu-id="92a67-131">Visual Studio y la aplicación nueva pueden tardar un poco en inicializarse.</span><span class="sxs-lookup"><span data-stu-id="92a67-131">It may take time to initialize Visual Studio and the new app.</span></span> <span data-ttu-id="92a67-132">Cuando se hayan inicializado, el explorador mostrará la aplicación en ejecución.</span><span class="sxs-lookup"><span data-stu-id="92a67-132">Once it is complete, the browser will show the running app.</span></span>

   ![Ventana del explorador en la que aparece la aplicación en ejecución que muestra "Hello World!"](azure-continuous-deployment/_static/04-browser-runapp.png)

2. <span data-ttu-id="92a67-134">Después de revisar la aplicación web en ejecución, cierre el explorador y haga clic en el icono "Detener depuración" de la barra de herramientas de Visual Studio para detener la aplicación.</span><span class="sxs-lookup"><span data-stu-id="92a67-134">After reviewing the running Web app, close the browser and click the "Stop Debugging" icon in the toolbar of Visual Studio to stop the app.</span></span>

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="92a67-135">Crear una aplicación web en Azure Portal</span><span class="sxs-lookup"><span data-stu-id="92a67-135">Create a web app in the Azure Portal</span></span>

<span data-ttu-id="92a67-136">Los siguientes pasos le guiarán por el proceso de creación de una aplicación web en Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="92a67-136">The following steps will guide you through creating a web app in the Azure Portal.</span></span>

1. <span data-ttu-id="92a67-137">Inicie sesión en [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="92a67-137">Log in to the [Azure Portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="92a67-138">Pulse **Nuevo** en la parte superior izquierda del portal.</span><span class="sxs-lookup"><span data-stu-id="92a67-138">TAP **NEW** at the top left of the Portal</span></span>
3. <span data-ttu-id="92a67-139">Pulse **Web y móvil** > **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="92a67-139">TAP **Web + Mobile** > **Web App**</span></span>

    ![Microsoft Azure Portal: Botón Nuevo: Web y móvil en Marketplace: Botón Aplicación web en Aplicaciones destacadas](azure-continuous-deployment/_static/05-azure-newwebapp.png)

4.  <span data-ttu-id="92a67-141">En la hoja **Aplicación web**, escriba un valor único para el **nombre del servicio de aplicaciones**.</span><span class="sxs-lookup"><span data-stu-id="92a67-141">In the **Web App** blade, enter a unique value for the **App Service Name**.</span></span>

    ![Hoja Aplicación web](azure-continuous-deployment/_static/06-azure-newappblade.png)

    >[!NOTE]
    ><span data-ttu-id="92a67-143">El **nombre del servicio de aplicaciones** debe ser único.</span><span class="sxs-lookup"><span data-stu-id="92a67-143">The **App Service Name** name needs to be unique.</span></span> <span data-ttu-id="92a67-144">El portal aplicará esta regla al intentar escribir el nombre.</span><span class="sxs-lookup"><span data-stu-id="92a67-144">The portal will enforce this rule when you attempt to enter the name.</span></span> <span data-ttu-id="92a67-145">Después de escribir un valor diferente, deberá sustituir dicho valor en todas las apariciones de **SampleWebAppDemo** que vea en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="92a67-145">After you enter a different value, you'll need to substitute that value for each occurrence of **SampleWebAppDemo** that you see in this tutorial.</span></span>

    &nbsp;
    
    <span data-ttu-id="92a67-146">También en la hoja **Aplicación web**, seleccione un plan o ubicación existente de App Service o bien cree uno.</span><span class="sxs-lookup"><span data-stu-id="92a67-146">Also in the **Web App** blade, select an existing **App Service Plan/Location** or create a new one.</span></span> <span data-ttu-id="92a67-147">Si crea un plan, seleccione el plan de tarifa, la ubicación y otras opciones.</span><span class="sxs-lookup"><span data-stu-id="92a67-147">If you create a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="92a67-148">Para más información sobre los planes de App Service, vea [Introducción detallada sobre los planes de Azure App Service](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/).</span><span class="sxs-lookup"><span data-stu-id="92a67-148">For more information on App Service plans, [Azure App Service plans in-depth overview](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/).</span></span>

5.  <span data-ttu-id="92a67-149">Haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="92a67-149">Click **Create**.</span></span> <span data-ttu-id="92a67-150">Azure aprovisionará e iniciará la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="92a67-150">Azure will provision and start your web app.</span></span>

    ![Azure Portal: Hoja de Essentials SampleWebAppDemo01](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="92a67-152">Habilitar la publicación de Git para la nueva aplicación web</span><span class="sxs-lookup"><span data-stu-id="92a67-152">Enable Git publishing for the new web app</span></span>

<span data-ttu-id="92a67-153">Git es un sistema de control de versiones distribuido que puede usar para implementar su aplicación web de Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="92a67-153">Git is a distributed version control system that you can use to deploy your Azure App Service web app.</span></span> <span data-ttu-id="92a67-154">Va a almacenar el código que escriba para la aplicación web en un repositorio local de Git y va a implementar el código en Azure insertándolo en un repositorio remoto.</span><span class="sxs-lookup"><span data-stu-id="92a67-154">You'll store the code you write for your web app in a local Git repository, and you'll deploy your code to Azure by pushing to a remote repository.</span></span>

1. <span data-ttu-id="92a67-155">Inicie sesión en [Azure Portal](https://portal.azure.com), si aún no lo ha hecho.</span><span class="sxs-lookup"><span data-stu-id="92a67-155">Log into the [Azure Portal](https://portal.azure.com), if you're not already logged in.</span></span>

2. <span data-ttu-id="92a67-156">Haga clic en **App Services** para ver una lista los servicios de aplicaciones asociados a su suscripción de Azure.</span><span class="sxs-lookup"><span data-stu-id="92a67-156">Click **App Services** to view a list of the app services associated with your Azure subscription.</span></span>

3. <span data-ttu-id="92a67-157">Seleccione la aplicación web que creó en la sección anterior de este tutorial.</span><span class="sxs-lookup"><span data-stu-id="92a67-157">Select the web app you created in the previous section of this tutorial.</span></span>

4. <span data-ttu-id="92a67-158">En la hoja **Implementación**, seleccione **Opciones de implementación** > **Elegir origen** > **Repositorio de Git local**.</span><span class="sxs-lookup"><span data-stu-id="92a67-158">In the **Deployment** blade, select **Deployment options** > **Choose Source** > **Local Git Repository**.</span></span>

   ![Hoja Configuración: Hoja Origen de implementación: Hoja Elegir origen](azure-continuous-deployment/_static/deployment-options.png)

5. <span data-ttu-id="92a67-160">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="92a67-160">Click **OK**.</span></span>

6. <span data-ttu-id="92a67-161">Si no ha configurado antes las credenciales de implementación para publicar una aplicación web u otra aplicación de App Service, configúrelas ahora:</span><span class="sxs-lookup"><span data-stu-id="92a67-161">If you have not previously set up deployment credentials for publishing a web app or other App Service app, set them up now:</span></span>

   * <span data-ttu-id="92a67-162">Haga clic en **Configuración** > **Credenciales de implementación**.</span><span class="sxs-lookup"><span data-stu-id="92a67-162">Click **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="92a67-163">Se mostrará la hoja **Configurar credenciales de implementación**.</span><span class="sxs-lookup"><span data-stu-id="92a67-163">The **Set deployment credentials** blade will be displayed.</span></span>

   * <span data-ttu-id="92a67-164">Cree un nombre de usuario y una contraseña.</span><span class="sxs-lookup"><span data-stu-id="92a67-164">Create a user name and password.</span></span>  <span data-ttu-id="92a67-165">Necesitará esta contraseña cuando configure Git.</span><span class="sxs-lookup"><span data-stu-id="92a67-165">You'll need this password later when setting up Git.</span></span>

   * <span data-ttu-id="92a67-166">Haga clic en **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="92a67-166">Click **Save**.</span></span>

7. <span data-ttu-id="92a67-167">En la hoja **Aplicación web**, haga clic en **Configuración** > **Propiedades**.</span><span class="sxs-lookup"><span data-stu-id="92a67-167">In the **Web App** blade, click **Settings** > **Properties**.</span></span> <span data-ttu-id="92a67-168">La dirección URL del repositorio remoto de Git en el que va a efectuar la implementación aparece en **Dirección URL de Git**.</span><span class="sxs-lookup"><span data-stu-id="92a67-168">The URL of the remote Git repository that you'll deploy to is shown under **GIT URL**.</span></span>

8. <span data-ttu-id="92a67-169">Copie el valor de **Dirección URL de Git** para usarlo más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="92a67-169">Copy the **GIT URL** value for later use in the tutorial.</span></span>

   ![Azure Portal: Hoja Propiedades de la aplicación](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-your-web-app-to-azure-app-service"></a><span data-ttu-id="92a67-171">Publicar la aplicación web en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="92a67-171">Publish your web app to Azure App Service</span></span>

<span data-ttu-id="92a67-172">En esta sección crearemos un repositorio local de Git con Visual Studio y lo insertaremos desde ese repositorio a Azure para implementar la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="92a67-172">In this section, you will create a local Git repository using Visual Studio and push from that repository to Azure to deploy your web app.</span></span> <span data-ttu-id="92a67-173">Los pasos son los siguientes:</span><span class="sxs-lookup"><span data-stu-id="92a67-173">The steps involved include the following:</span></span>

   * <span data-ttu-id="92a67-174">Agregue la opción del repositorio remoto con el valor de la dirección URL de Git, de modo que pueda implementar el repositorio local en Azure.</span><span class="sxs-lookup"><span data-stu-id="92a67-174">Add the remote repository setting using your GIT URL value, so you can deploy your local repository to Azure.</span></span>

   * <span data-ttu-id="92a67-175">Confirme los cambios del proyecto.</span><span class="sxs-lookup"><span data-stu-id="92a67-175">Commit your project changes.</span></span>

   * <span data-ttu-id="92a67-176">Inserte los cambios del proyecto del repositorio local en el repositorio remoto de Azure.</span><span class="sxs-lookup"><span data-stu-id="92a67-176">Push your project changes from your local repository to your remote repository on Azure.</span></span>

&nbsp;
   
1.  <span data-ttu-id="92a67-177">En el **Explorador de soluciones**, haga clic con el botón derecho en **Solución 'SampleWebAppDemo'** y seleccione **Confirmar**.</span><span class="sxs-lookup"><span data-stu-id="92a67-177">In **Solution Explorer** right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="92a67-178">Aparecerá **Team Explorer**.</span><span class="sxs-lookup"><span data-stu-id="92a67-178">The **Team Explorer** will be displayed.</span></span>

    ![Pestaña Team Explorer: Conexión](azure-continuous-deployment/_static/10-team-explorer.png)

2.  <span data-ttu-id="92a67-180">En **Team Explorer**, seleccione **Inicio** (icono Inicio) > **Configuración** > **Configuración del repositorio**.</span><span class="sxs-lookup"><span data-stu-id="92a67-180">In **Team Explorer**, select the **Home** (home icon) > **Settings** > **Repository Settings**.</span></span>

3.  <span data-ttu-id="92a67-181">En la sección **Remotos** de **Configuración del repositorio**, seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="92a67-181">In the **Remotes** section of the **Repository Settings** select **Add**.</span></span> <span data-ttu-id="92a67-182">Aparecerá el cuadro de diálogo **Agregar remoto**.</span><span class="sxs-lookup"><span data-stu-id="92a67-182">The **Add Remote** dialog box will be displayed.</span></span>

4.  <span data-ttu-id="92a67-183">Establezca el **nombre** del repositorio remoto en **Azure-SampleApp**.</span><span class="sxs-lookup"><span data-stu-id="92a67-183">Set the **Name** of the remote to **Azure-SampleApp**.</span></span>

5.  <span data-ttu-id="92a67-184">Establezca el valor de **Recuperar** en la **dirección URL de Git** que ha copiado de Azure en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="92a67-184">Set the value for **Fetch** to the **Git URL** that you copied from Azure earlier in this tutorial.</span></span> <span data-ttu-id="92a67-185">Tenga en cuenta que esta es la dirección URL que termina en **.git**.</span><span class="sxs-lookup"><span data-stu-id="92a67-185">Note that this is the URL that ends with **.git**.</span></span>

    ![Cuadro de diálogo Editar Azure-SampleApp remoto](azure-continuous-deployment/_static/11-add-remote.png)

    >[!NOTE]
    ><span data-ttu-id="92a67-187">Como alternativa, puede especificar el repositorio remoto desde la **ventana de comandos**. Para ello, abra la **ventana de comandos**, vaya al directorio del proyecto y escriba el comando.</span><span class="sxs-lookup"><span data-stu-id="92a67-187">As an alternative, you can specify the remote repository from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering the command.</span></span> <span data-ttu-id="92a67-188">Por ejemplo:`git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`</span><span class="sxs-lookup"><span data-stu-id="92a67-188">For example:`git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`</span></span>

6.  <span data-ttu-id="92a67-189">Seleccione **Inicio** (icono de Inicio) > **Configuración** > **Configuración global**.</span><span class="sxs-lookup"><span data-stu-id="92a67-189">Select the **Home** (home icon) > **Settings** > **Global Settings**.</span></span> <span data-ttu-id="92a67-190">Asegúrese de haber establecido el nombre y la dirección de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="92a67-190">Make sure you have your name and your email address set.</span></span> <span data-ttu-id="92a67-191">También deberá seleccionar **Actualizar**.</span><span class="sxs-lookup"><span data-stu-id="92a67-191">You may also need to select **Update**.</span></span>

7.  <span data-ttu-id="92a67-192">Seleccione **Inicio** > **Cambios** para volver a la vista **Cambios**.</span><span class="sxs-lookup"><span data-stu-id="92a67-192">Select **Home** > **Changes** to return to the **Changes** view.</span></span>

8.  <span data-ttu-id="92a67-193">Escriba un mensaje de confirmación, como **Inserción inicial n.º 1**, y haga clic en **Confirmar**.</span><span class="sxs-lookup"><span data-stu-id="92a67-193">Enter a commit message, such as **Initial Push #1** and click **Commit**.</span></span> <span data-ttu-id="92a67-194">Esta acción creará una *confirmación* de forma local.</span><span class="sxs-lookup"><span data-stu-id="92a67-194">This action will create a *commit* locally.</span></span> <span data-ttu-id="92a67-195">Luego deberá efectuar una *sincronización* con Azure.</span><span class="sxs-lookup"><span data-stu-id="92a67-195">Next, you need to *sync* with Azure.</span></span>

    ![Pestaña Team Explorer: Conexión](azure-continuous-deployment/_static/12-initial-commit.png)

    >[!NOTE]
    ><span data-ttu-id="92a67-197">Como alternativa, puede confirmar los cambios desde la **ventana de comandos**. Para ello, abra la **ventana de comandos**, vaya al directorio del proyecto y escriba los comandos git.</span><span class="sxs-lookup"><span data-stu-id="92a67-197">As an alternative, you can commit your changes from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering the git commands.</span></span> <span data-ttu-id="92a67-198">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="92a67-198">For example:</span></span>
    >
    >`git add .`
    >
    >`git commit -am "Initial Push #1"`

9.  <span data-ttu-id="92a67-199">Seleccione **Inicio** > **Sincronizar** > **Acciones** > **Abrir símbolo del sistema**.</span><span class="sxs-lookup"><span data-stu-id="92a67-199">Select **Home** > **Sync** > **Actions** > **Open Command Prompt**.</span></span> <span data-ttu-id="92a67-200">Se abrirá el símbolo del sistema en el directorio del proyecto.</span><span class="sxs-lookup"><span data-stu-id="92a67-200">The command prompt will open to your project directory.</span></span>

10.  <span data-ttu-id="92a67-201">Escriba el siguiente comando en la ventana de comandos:</span><span class="sxs-lookup"><span data-stu-id="92a67-201">Enter the following command in the command window:</span></span>

    `git push -u Azure-SampleApp master`

11.  <span data-ttu-id="92a67-202">Escriba la contraseña de sus **credenciales de implementación** de Azure que creó anteriormente en Azure.</span><span class="sxs-lookup"><span data-stu-id="92a67-202">Enter your Azure **deployment credentials** password that you created earlier in Azure.</span></span>

    >[!NOTE]
    ><span data-ttu-id="92a67-203">La contraseña no se verá a medida que la escriba.</span><span class="sxs-lookup"><span data-stu-id="92a67-203">Your password will not be visible as you enter it.</span></span>

    <span data-ttu-id="92a67-204">Este comando iniciará el proceso de inserción de los archivos locales del proyecto en Azure.</span><span class="sxs-lookup"><span data-stu-id="92a67-204">This command will start the process of pushing your local project files to Azure.</span></span> <span data-ttu-id="92a67-205">La salida del comando anterior finaliza con un mensaje que indica que la implementación se efectuó correctamente.</span><span class="sxs-lookup"><span data-stu-id="92a67-205">The output from the above command ends with a message that deployment was successful.</span></span>
        
    ```
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
    * [new branch]      master -> master
    Branch master set up to track remote branch master from Azure-SampleApp.
    ```
    > [!NOTE]
    > <span data-ttu-id="92a67-206">Si necesita colaborar en un proyecto, piense en la posibilidad de efectuar una inserción en [GitHub](https://github.com) mientras efectúa una inserción en Azure.</span><span class="sxs-lookup"><span data-stu-id="92a67-206">If you need to collaborate on a project, you should consider pushing to [GitHub](https://github.com) in between pushing to Azure.</span></span>
 
### <a name="verify-the-active-deployment"></a><span data-ttu-id="92a67-207">Comprobar la implementación activa</span><span class="sxs-lookup"><span data-stu-id="92a67-207">Verify the Active Deployment</span></span>

<span data-ttu-id="92a67-208">Puede comprobar que transfirió correctamente la aplicación web del entorno local a Azure.</span><span class="sxs-lookup"><span data-stu-id="92a67-208">You can verify that you successfully transferred the web app from your local environment to Azure.</span></span> <span data-ttu-id="92a67-209">Verá que aparece la implementación correcta.</span><span class="sxs-lookup"><span data-stu-id="92a67-209">You'll see the listed successful deployment.</span></span>

1. <span data-ttu-id="92a67-210">En [Azure Portal](https://portal.azure.com), seleccione la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="92a67-210">In the [Azure Portal](https://portal.azure.com), select your web app.</span></span> <span data-ttu-id="92a67-211">Después, seleccione **Implementación** > **Opciones de implementación**.</span><span class="sxs-lookup"><span data-stu-id="92a67-211">Then, select **Deployment** > **Deployment options**.</span></span>

   ![Azure Portal: Hoja Configuración: Hoja Implementaciones donde se muestra una implementación correcta](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a><span data-ttu-id="92a67-213">Ejecutar la aplicación en Azure</span><span class="sxs-lookup"><span data-stu-id="92a67-213">Run the app in Azure</span></span>

<span data-ttu-id="92a67-214">Ahora que ha implementado la aplicación web en Azure, puede ejecutarla.</span><span class="sxs-lookup"><span data-stu-id="92a67-214">Now that you have deployed your web app to Azure, you can run the app.</span></span>

<span data-ttu-id="92a67-215">Esto se puede hacer de dos maneras:</span><span class="sxs-lookup"><span data-stu-id="92a67-215">This can be done in two ways:</span></span>

* <span data-ttu-id="92a67-216">En Azure Portal, localice la hoja Aplicación web de su aplicación web y haga clic en **Examinar** para ver la aplicación en su explorador predeterminado.</span><span class="sxs-lookup"><span data-stu-id="92a67-216">In the Azure Portal, locate the web app blade for your web app, and click **Browse** to view your app in your default browser.</span></span>

* <span data-ttu-id="92a67-217">Abra un explorador y escriba la dirección URL de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="92a67-217">Open a browser and enter the URL for your web app.</span></span> <span data-ttu-id="92a67-218">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="92a67-218">For example:</span></span>

  `http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-your-web-app-and-republish"></a><span data-ttu-id="92a67-219">Actualizar la aplicación web y volver a publicarla</span><span class="sxs-lookup"><span data-stu-id="92a67-219">Update your web app and republish</span></span>

<span data-ttu-id="92a67-220">Después de realizar cambios en el código local, puede volver a publicar la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="92a67-220">After you make changes to your local code, you can republish.</span></span>

1.  <span data-ttu-id="92a67-221">En el **Explorador de soluciones** de Visual Studio, abra el archivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="92a67-221">In **Solution Explorer** of Visual Studio, open the *Startup.cs* file.</span></span>

2.  <span data-ttu-id="92a67-222">En el método `Configure`, modifique el método `Response.WriteAsync` para que aparezca del siguiente modo:</span><span class="sxs-lookup"><span data-stu-id="92a67-222">In the `Configure` method, modify the `Response.WriteAsync` method so that it appears as follows:</span></span>

    ```aspx-cs
    await context.Response.WriteAsync("Hello World! Deploy to Azure.");
    ```
3.  <span data-ttu-id="92a67-223">Guarde los cambios en *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="92a67-223">Save changes to *Startup.cs*.</span></span>

4.  <span data-ttu-id="92a67-224">En el **Explorador de soluciones**, haga clic con el botón derecho en **Solución 'SampleWebAppDemo'** y seleccione **Confirmar**.</span><span class="sxs-lookup"><span data-stu-id="92a67-224">In **Solution Explorer**, right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="92a67-225">Aparecerá **Team Explorer**.</span><span class="sxs-lookup"><span data-stu-id="92a67-225">The **Team Explorer** will be displayed.</span></span>

5.  <span data-ttu-id="92a67-226">Escriba un mensaje de confirmación, como el siguiente:</span><span class="sxs-lookup"><span data-stu-id="92a67-226">Enter a commit message, such as:</span></span>

    ```none
    Update #2
    ```

6.  <span data-ttu-id="92a67-227">Presione el botón **Confirmar** para confirmar los cambios del proyecto.</span><span class="sxs-lookup"><span data-stu-id="92a67-227">Press the **Commit** button to commit the project changes.</span></span>

7.  <span data-ttu-id="92a67-228">Seleccione **Inicio** > **Sincronizar** > **Acciones** > **Inserción**.</span><span class="sxs-lookup"><span data-stu-id="92a67-228">Select **Home** > **Sync** > **Actions** > **Push**.</span></span>

>[!NOTE]
><span data-ttu-id="92a67-229">Como alternativa, puede insertar los cambios desde la **ventana de comandos**. Para ello, abra la **ventana de comandos**, vaya al directorio del proyecto y escriba un comando git.</span><span class="sxs-lookup"><span data-stu-id="92a67-229">As an alternative, you can push your changes from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering a git command.</span></span> <span data-ttu-id="92a67-230">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="92a67-230">For example:</span></span>
>
>`git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a><span data-ttu-id="92a67-231">Ver la aplicación web actualizada en Azure</span><span class="sxs-lookup"><span data-stu-id="92a67-231">View the updated web app in Azure</span></span>

<span data-ttu-id="92a67-232">Para ver la aplicación web actualizada, seleccione **Examinar** en la hoja de la aplicación web de Azure Portal o abra un explorador y escriba la dirección URL de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="92a67-232">View your updated web app by selecting **Browse** from the web app blade in the Azure Portal or by opening a browser and entering the URL for your web app.</span></span> <span data-ttu-id="92a67-233">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="92a67-233">For example:</span></span>

   `http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a><span data-ttu-id="92a67-234">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="92a67-234">Additional Resources</span></span>

* [<span data-ttu-id="92a67-235">Publicación e implementación</span><span class="sxs-lookup"><span data-stu-id="92a67-235">Publishing and Deployment</span></span>](index.md)

* [<span data-ttu-id="92a67-236">Proyecto Kudu</span><span class="sxs-lookup"><span data-stu-id="92a67-236">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
