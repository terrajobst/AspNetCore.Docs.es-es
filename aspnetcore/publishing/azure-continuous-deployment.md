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
ms.openlocfilehash: b576ef6bce3b211afe7465f33dfe62c25dac1f62
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="continuous-deployment-to-azure-for-aspnet-core-with-visual-studio-and-git"></a><span data-ttu-id="dad9e-104">Implementación continua en Azure para ASP.NET Core con Visual Studio y Git</span><span class="sxs-lookup"><span data-stu-id="dad9e-104">Continuous deployment to Azure for ASP.NET Core, with Visual Studio and Git</span></span>

<span data-ttu-id="dad9e-105">Por [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="dad9e-105">By [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="dad9e-106">En este tutorial se muestra cómo crear una aplicación web de ASP.NET Core con Visual Studio y a implementarla desde Visual Studio en Azure App Service mediante una implementación continua.</span><span class="sxs-lookup"><span data-stu-id="dad9e-106">This tutorial shows you how to create an ASP.NET Core web app using Visual Studio and deploy it from Visual Studio to Azure App Service using continuous deployment.</span></span> 

<span data-ttu-id="dad9e-107">Vea también [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic) (Usar VSTS para crear y publicar una aplicación web de Azure con la implementación continua), donde se muestra cómo configurar un flujo de trabajo de una entrega continua (CD) para [Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) con Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="dad9e-107">See also [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic), which shows how to configure a continuous delivery (CD) workflow for [Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) using Visual Studio Team Services.</span></span> <span data-ttu-id="dad9e-108">La entrega continua de Azure en Team Services simplifica la configuración de una canalización de implementación sólida para publicar actualizaciones de la aplicación en Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="dad9e-108">Azure Continuous Delivery in Team Services simplifies setting up a robust deployment pipeline to publish updates for your app to Azure App Service.</span></span> <span data-ttu-id="dad9e-109">La canalización se puede configurar desde Azure Portal para crear y ejecutar pruebas, implementarlas en un espacio de ensayo y luego implementarlas en un entorno de producción.</span><span class="sxs-lookup"><span data-stu-id="dad9e-109">The pipeline can be configured from the Azure portal to build, run tests, deploy to a staging slot,  and then deploy to production.</span></span>

> [!NOTE]
> <span data-ttu-id="dad9e-110">Para seguir este tutorial necesita una cuenta de Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="dad9e-110">To complete this tutorial, you need a Microsoft Azure account.</span></span> <span data-ttu-id="dad9e-111">Si no tiene una, puede [activar las ventajas como suscriptor a MSDN](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) o [registrarse para obtener una evaluación gratuita](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="dad9e-111">If you don't have an account, you can [activate your MSDN subscriber benefits](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) or [sign up for a free trial](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dad9e-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="dad9e-112">Prerequisites</span></span>

<span data-ttu-id="dad9e-113">En este tutorial se da por hecho que ya ha instalado lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="dad9e-113">This tutorial assumes you have already installed the following:</span></span>

* [<span data-ttu-id="dad9e-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dad9e-114">Visual Studio</span></span>](https://www.visualstudio.com)

* <span data-ttu-id="dad9e-115">[ASP.NET Core](http://go.microsoft.com/fwlink/?LinkId=627627) (tiempo de ejecución y herramientas)</span><span class="sxs-lookup"><span data-stu-id="dad9e-115">[ASP.NET Core](http://go.microsoft.com/fwlink/?LinkId=627627) (runtime and tooling)</span></span>

* <span data-ttu-id="dad9e-116">[Git](http://git-scm.com/downloads) para Windows</span><span class="sxs-lookup"><span data-stu-id="dad9e-116">[Git](http://git-scm.com/downloads) for Windows</span></span>

## <a name="create-an-aspnet-core-web-app"></a><span data-ttu-id="dad9e-117">Crear una aplicación web de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dad9e-117">Create an ASP.NET Core web app</span></span>

1. <span data-ttu-id="dad9e-118">Inicie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dad9e-118">Start Visual Studio.</span></span>

2. <span data-ttu-id="dad9e-119">En el menú **Archivo**, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-119">From the **File** menu, select **New** > **Project**.</span></span>

3. <span data-ttu-id="dad9e-120">Seleccione la plantilla de proyecto **Aplicación web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-120">Select the **ASP.NET Web Application** project template.</span></span> <span data-ttu-id="dad9e-121">Aparece en **Instalados** > **Plantillas** > **Visual C#** > **Web**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-121">It appears under **Installed** > **Templates** > **Visual C#** > **Web**.</span></span> <span data-ttu-id="dad9e-122">Dé un nombre al proyecto `SampleWebAppDemo`.</span><span class="sxs-lookup"><span data-stu-id="dad9e-122">Name the project `SampleWebAppDemo`.</span></span> <span data-ttu-id="dad9e-123">Seleccione la opción **Crear nuevo repositorio de Git** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-123">Select the **Create new Git respository** option and click **OK**.</span></span>

   ![Cuadro de diálogo Nuevo proyecto](azure-continuous-deployment/_static/01-new-project.png)

4. <span data-ttu-id="dad9e-125">En el cuadro de diálogo **Nuevo proyecto ASP.NET**, seleccione la plantilla **Vacía** de ASP.NET Core y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-125">In the **New ASP.NET Project** dialog, select the ASP.NET Core **Empty** template, then click **OK**.</span></span>

   ![Cuadro de diálogo Nuevo proyecto ASP.NET](azure-continuous-deployment/_static/02-web-site-template.png)


### <a name="running-the-web-app-locally"></a><span data-ttu-id="dad9e-127">Ejecutar la aplicación web de forma local</span><span class="sxs-lookup"><span data-stu-id="dad9e-127">Running the web app locally</span></span>

1. <span data-ttu-id="dad9e-128">Cuando Visual Studio haya acabado de crear la aplicación, ejecútela seleccionando **Depurar** -> **Iniciar depuración**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-128">Once Visual Studio finishes creating the app, run the app by selecting **Debug** -> **Start Debugging**.</span></span> <span data-ttu-id="dad9e-129">Como alternativa puede pulsar **F5**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-129">As an alternative, you can press **F5**.</span></span>

   <span data-ttu-id="dad9e-130">Visual Studio y la aplicación nueva pueden tardar un poco en inicializarse.</span><span class="sxs-lookup"><span data-stu-id="dad9e-130">It may take time to initialize Visual Studio and the new app.</span></span> <span data-ttu-id="dad9e-131">Cuando se hayan inicializado, el explorador mostrará la aplicación en ejecución.</span><span class="sxs-lookup"><span data-stu-id="dad9e-131">Once it is complete, the browser will show the running app.</span></span>

   ![Ventana del explorador en la que aparece la aplicación en ejecución que muestra "Hello World!"](azure-continuous-deployment/_static/04-browser-runapp.png)

2. <span data-ttu-id="dad9e-133">Después de revisar la aplicación web en ejecución, cierre el explorador y haga clic en el icono "Detener depuración" de la barra de herramientas de Visual Studio para detener la aplicación.</span><span class="sxs-lookup"><span data-stu-id="dad9e-133">After reviewing the running Web app, close the browser and click the "Stop Debugging" icon in the toolbar of Visual Studio to stop the app.</span></span>

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="dad9e-134">Crear una aplicación web en Azure Portal</span><span class="sxs-lookup"><span data-stu-id="dad9e-134">Create a web app in the Azure Portal</span></span>

<span data-ttu-id="dad9e-135">Los siguientes pasos le guiarán por el proceso de creación de una aplicación web en Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="dad9e-135">The following steps will guide you through creating a web app in the Azure Portal.</span></span>

1. <span data-ttu-id="dad9e-136">Inicie sesión en [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="dad9e-136">Log in to the [Azure Portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="dad9e-137">Pulse **Nuevo** en la parte superior izquierda del portal.</span><span class="sxs-lookup"><span data-stu-id="dad9e-137">TAP **NEW** at the top left of the Portal</span></span>
3. <span data-ttu-id="dad9e-138">Pulse **Web y móvil** > **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-138">TAP **Web + Mobile** > **Web App**</span></span>

    ![Microsoft Azure Portal: Botón Nuevo: Web y móvil en Marketplace: Botón Aplicación web en Aplicaciones destacadas](azure-continuous-deployment/_static/05-azure-newwebapp.png)

4.  <span data-ttu-id="dad9e-140">En la hoja **Aplicación web**, escriba un valor único para el **nombre del servicio de aplicaciones**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-140">In the **Web App** blade, enter a unique value for the **App Service Name**.</span></span>

    ![Hoja Aplicación web](azure-continuous-deployment/_static/06-azure-newappblade.png)

    >[!NOTE]
    ><span data-ttu-id="dad9e-142">El **nombre del servicio de aplicaciones** debe ser único.</span><span class="sxs-lookup"><span data-stu-id="dad9e-142">The **App Service Name** name needs to be unique.</span></span> <span data-ttu-id="dad9e-143">El portal aplicará esta regla al intentar escribir el nombre.</span><span class="sxs-lookup"><span data-stu-id="dad9e-143">The portal will enforce this rule when you attempt to enter the name.</span></span> <span data-ttu-id="dad9e-144">Después de escribir un valor diferente, deberá sustituir dicho valor en todas las apariciones de **SampleWebAppDemo** que vea en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="dad9e-144">After you enter a different value, you'll need to substitute that value for each occurrence of **SampleWebAppDemo** that you see in this tutorial.</span></span>

    &nbsp;
    
    <span data-ttu-id="dad9e-145">También en la hoja **Aplicación web**, seleccione un plan o ubicación existente de App Service o bien cree uno.</span><span class="sxs-lookup"><span data-stu-id="dad9e-145">Also in the **Web App** blade, select an existing **App Service Plan/Location** or create a new one.</span></span> <span data-ttu-id="dad9e-146">Si crea un plan, seleccione el plan de tarifa, la ubicación y otras opciones.</span><span class="sxs-lookup"><span data-stu-id="dad9e-146">If you create a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="dad9e-147">Para más información sobre los planes de App Service, vea [Introducción detallada sobre los planes de Azure App Service](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/).</span><span class="sxs-lookup"><span data-stu-id="dad9e-147">For more information on App Service plans, [Azure App Service plans in-depth overview](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/).</span></span>

5.  <span data-ttu-id="dad9e-148">Haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-148">Click **Create**.</span></span> <span data-ttu-id="dad9e-149">Azure aprovisionará e iniciará la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="dad9e-149">Azure will provision and start your web app.</span></span>

    ![Azure Portal: Hoja de Essentials SampleWebAppDemo01](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="dad9e-151">Habilitar la publicación de Git para la nueva aplicación web</span><span class="sxs-lookup"><span data-stu-id="dad9e-151">Enable Git publishing for the new web app</span></span>

<span data-ttu-id="dad9e-152">Git es un sistema de control de versiones distribuido que puede usar para implementar su aplicación web de Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="dad9e-152">Git is a distributed version control system that you can use to deploy your Azure App Service web app.</span></span> <span data-ttu-id="dad9e-153">Va a almacenar el código que escriba para la aplicación web en un repositorio local de Git y va a implementar el código en Azure insertándolo en un repositorio remoto.</span><span class="sxs-lookup"><span data-stu-id="dad9e-153">You'll store the code you write for your web app in a local Git repository, and you'll deploy your code to Azure by pushing to a remote repository.</span></span>

1. <span data-ttu-id="dad9e-154">Inicie sesión en [Azure Portal](https://portal.azure.com), si aún no lo ha hecho.</span><span class="sxs-lookup"><span data-stu-id="dad9e-154">Log into the [Azure Portal](https://portal.azure.com), if you're not already logged in.</span></span>

2. <span data-ttu-id="dad9e-155">Haga clic en **Examinar**, opción que se encuentra en la parte inferior del panel de navegación.</span><span class="sxs-lookup"><span data-stu-id="dad9e-155">Click **Browse**, located at the bottom of the navigation pane.</span></span>

3. <span data-ttu-id="dad9e-156">Haga clic en **Aplicaciones web** para ver una lista de las aplicaciones web asociadas a su suscripción de Azure.</span><span class="sxs-lookup"><span data-stu-id="dad9e-156">Click **Web Apps** to view a list of the web apps associated with your Azure subscription.</span></span>

4. <span data-ttu-id="dad9e-157">Seleccione la aplicación web que creó en la sección anterior de este tutorial.</span><span class="sxs-lookup"><span data-stu-id="dad9e-157">Select the web app you created in the previous section of this tutorial.</span></span>

5. <span data-ttu-id="dad9e-158">Si no se muestra la hoja **Configuración**, seleccione **Configuración** en la hoja **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-158">If the **Settings** blade is not shown, select **Settings** in the **Web App** blade.</span></span>

6. <span data-ttu-id="dad9e-159">En la hoja **Configuración**, seleccione **Origen de implementación** > **Elegir origen** > **Repositorio de Git local**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-159">In the **Settings** blade, select **Deployment source** > **Choose Source** > **Local Git Repository**.</span></span>

   ![Hoja Configuración: Hoja Origen de implementación: Hoja Elegir origen](azure-continuous-deployment/_static/08-azure-localrepository.png)

7. <span data-ttu-id="dad9e-161">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-161">Click **OK**.</span></span>

8. <span data-ttu-id="dad9e-162">Si no ha configurado antes las credenciales de implementación para publicar una aplicación web u otra aplicación de App Service, configúrelas ahora:</span><span class="sxs-lookup"><span data-stu-id="dad9e-162">If you have not previously set up deployment credentials for publishing a web app or other App Service app, set them up now:</span></span>

   * <span data-ttu-id="dad9e-163">Haga clic en **Configuración** > **Credenciales de implementación**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-163">Click **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="dad9e-164">Se mostrará la hoja **Configurar credenciales de implementación**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-164">The **Set deployment credentials** blade will be displayed.</span></span>

   * <span data-ttu-id="dad9e-165">Cree un nombre de usuario y una contraseña.</span><span class="sxs-lookup"><span data-stu-id="dad9e-165">Create a user name and password.</span></span>  <span data-ttu-id="dad9e-166">Necesitará esta contraseña cuando configure Git.</span><span class="sxs-lookup"><span data-stu-id="dad9e-166">You'll need this password later when setting up Git.</span></span>

   * <span data-ttu-id="dad9e-167">Haga clic en **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-167">Click **Save**.</span></span>

9. <span data-ttu-id="dad9e-168">En la hoja **Aplicación web**, haga clic en **Configuración** > **Propiedades**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-168">In the **Web App** blade, click **Settings** > **Properties**.</span></span> <span data-ttu-id="dad9e-169">La dirección URL del repositorio remoto de Git en el que va a efectuar la implementación aparece en **Dirección URL de Git**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-169">The URL of the remote Git repository that you'll deploy to is shown under **GIT URL**.</span></span>

10. <span data-ttu-id="dad9e-170">Copie el valor de **Dirección URL de Git** para usarlo más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="dad9e-170">Copy the **GIT URL** value for later use in the tutorial.</span></span>

   ![Azure Portal: Hoja Propiedades de la aplicación](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-your-web-app-to-azure-app-service"></a><span data-ttu-id="dad9e-172">Publicar la aplicación web en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="dad9e-172">Publish your web app to Azure App Service</span></span>

<span data-ttu-id="dad9e-173">En esta sección crearemos un repositorio local de Git con Visual Studio y lo insertaremos desde ese repositorio a Azure para implementar la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="dad9e-173">In this section, you will create a local Git repository using Visual Studio and push from that repository to Azure to deploy your web app.</span></span> <span data-ttu-id="dad9e-174">Los pasos son los siguientes:</span><span class="sxs-lookup"><span data-stu-id="dad9e-174">The steps involved include the following:</span></span>

   * <span data-ttu-id="dad9e-175">Agregue la opción del repositorio remoto con el valor de la dirección URL de Git, de modo que pueda implementar el repositorio local en Azure.</span><span class="sxs-lookup"><span data-stu-id="dad9e-175">Add the remote repository setting using your GIT URL value, so you can deploy your local repository to Azure.</span></span>

   * <span data-ttu-id="dad9e-176">Confirme los cambios del proyecto.</span><span class="sxs-lookup"><span data-stu-id="dad9e-176">Commit your project changes.</span></span>

   * <span data-ttu-id="dad9e-177">Inserte los cambios del proyecto del repositorio local en el repositorio remoto de Azure.</span><span class="sxs-lookup"><span data-stu-id="dad9e-177">Push your project changes from your local repository to your remote repository on Azure.</span></span>

&nbsp;
   
1.  <span data-ttu-id="dad9e-178">En el **Explorador de soluciones**, haga clic con el botón derecho en **Solución 'SampleWebAppDemo'** y seleccione **Confirmar**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-178">In **Solution Explorer** right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="dad9e-179">Aparecerá **Team Explorer**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-179">The **Team Explorer** will be displayed.</span></span>

    ![Pestaña Team Explorer: Conexión](azure-continuous-deployment/_static/10-team-explorer.png)

2.  <span data-ttu-id="dad9e-181">En **Team Explorer**, seleccione **Inicio** (icono Inicio) > **Configuración** > **Configuración del repositorio**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-181">In **Team Explorer**, select the **Home** (home icon) > **Settings** > **Repository Settings**.</span></span>

3.  <span data-ttu-id="dad9e-182">En la sección **Remotos** de **Configuración del repositorio**, seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-182">In the **Remotes** section of the **Repository Settings** select **Add**.</span></span> <span data-ttu-id="dad9e-183">Aparecerá el cuadro de diálogo **Agregar remoto**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-183">The **Add Remote** dialog box will be displayed.</span></span>

4.  <span data-ttu-id="dad9e-184">Establezca el **nombre** del repositorio remoto en **Azure-SampleApp**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-184">Set the **Name** of the remote to **Azure-SampleApp**.</span></span>

5.  <span data-ttu-id="dad9e-185">Establezca el valor de **Recuperar** en la **dirección URL de Git** que ha copiado de Azure en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="dad9e-185">Set the value for **Fetch** to the **Git URL** that you copied from Azure earlier in this tutorial.</span></span> <span data-ttu-id="dad9e-186">Tenga en cuenta que esta es la dirección URL que termina en **.git**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-186">Note that this is the URL that ends with **.git**.</span></span>

    ![Cuadro de diálogo Editar Azure-SampleApp remoto](azure-continuous-deployment/_static/11-add-remote.png)

    >[!NOTE]
    ><span data-ttu-id="dad9e-188">Como alternativa, puede especificar el repositorio remoto desde la **ventana de comandos**. Para ello, abra la **ventana de comandos**, vaya al directorio del proyecto y escriba el comando.</span><span class="sxs-lookup"><span data-stu-id="dad9e-188">As an alternative, you can specify the remote repository from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering the command.</span></span> <span data-ttu-id="dad9e-189">Por ejemplo:`git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`</span><span class="sxs-lookup"><span data-stu-id="dad9e-189">For example:`git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`</span></span>

6.  <span data-ttu-id="dad9e-190">Seleccione **Inicio** (icono de Inicio) > **Configuración** > **Configuración global**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-190">Select the **Home** (home icon) > **Settings** > **Global Settings**.</span></span> <span data-ttu-id="dad9e-191">Asegúrese de haber establecido el nombre y la dirección de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="dad9e-191">Make sure you have your name and your email address set.</span></span> <span data-ttu-id="dad9e-192">También deberá seleccionar **Actualizar**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-192">You may also need to select **Update**.</span></span>

7.  <span data-ttu-id="dad9e-193">Seleccione **Inicio** > **Cambios** para volver a la vista **Cambios**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-193">Select **Home** > **Changes** to return to the **Changes** view.</span></span>

8.  <span data-ttu-id="dad9e-194">Escriba un mensaje de confirmación, como **Inserción inicial n.º 1**, y haga clic en **Confirmar**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-194">Enter a commit message, such as **Initial Push #1** and click **Commit**.</span></span> <span data-ttu-id="dad9e-195">Esta acción creará una *confirmación* de forma local.</span><span class="sxs-lookup"><span data-stu-id="dad9e-195">This action will create a *commit* locally.</span></span> <span data-ttu-id="dad9e-196">Luego deberá efectuar una *sincronización* con Azure.</span><span class="sxs-lookup"><span data-stu-id="dad9e-196">Next, you need to *sync* with Azure.</span></span>

    ![Pestaña Team Explorer: Conexión](azure-continuous-deployment/_static/12-initial-commit.png)

    >[!NOTE]
    ><span data-ttu-id="dad9e-198">Como alternativa, puede confirmar los cambios desde la **ventana de comandos**. Para ello, abra la **ventana de comandos**, vaya al directorio del proyecto y escriba los comandos git.</span><span class="sxs-lookup"><span data-stu-id="dad9e-198">As an alternative, you can commit your changes from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering the git commands.</span></span> <span data-ttu-id="dad9e-199">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="dad9e-199">For example:</span></span>
    >
    >`git add .`
    >
    >`git commit -am "Initial Push #1"`

9.  <span data-ttu-id="dad9e-200">Seleccione **Inicio** > **Sincronizar** > **Acciones** > **Abrir símbolo del sistema**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-200">Select **Home** > **Sync** > **Actions** > **Open Command Prompt**.</span></span> <span data-ttu-id="dad9e-201">Se abrirá el símbolo del sistema en el directorio del proyecto.</span><span class="sxs-lookup"><span data-stu-id="dad9e-201">The command prompt will open to your project directory.</span></span>

10.  <span data-ttu-id="dad9e-202">Escriba el siguiente comando en la ventana de comandos:</span><span class="sxs-lookup"><span data-stu-id="dad9e-202">Enter the following command in the command window:</span></span>

    `git push -u Azure-SampleApp master`

11.  <span data-ttu-id="dad9e-203">Escriba la contraseña de sus **credenciales de implementación** de Azure que creó anteriormente en Azure.</span><span class="sxs-lookup"><span data-stu-id="dad9e-203">Enter your Azure **deployment credentials** password that you created earlier in Azure.</span></span>

    >[!NOTE]
    ><span data-ttu-id="dad9e-204">La contraseña no se verá a medida que la escriba.</span><span class="sxs-lookup"><span data-stu-id="dad9e-204">Your password will not be visible as you enter it.</span></span>

    <span data-ttu-id="dad9e-205">Este comando iniciará el proceso de inserción de los archivos locales del proyecto en Azure.</span><span class="sxs-lookup"><span data-stu-id="dad9e-205">This command will start the process of pushing your local project files to Azure.</span></span> <span data-ttu-id="dad9e-206">La salida del comando anterior finaliza con un mensaje que indica que la implementación se efectuó correctamente.</span><span class="sxs-lookup"><span data-stu-id="dad9e-206">The output from the above command ends with a message that deployment was successful.</span></span>
        
    ```
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
    * [new branch]      master -> master
    Branch master set up to track remote branch master from Azure-SampleApp.
    ```
    > [!NOTE]
    > <span data-ttu-id="dad9e-207">Si necesita colaborar en un proyecto, piense en la posibilidad de efectuar una inserción en [GitHub](https://github.com) mientras efectúa una inserción en Azure.</span><span class="sxs-lookup"><span data-stu-id="dad9e-207">If you need to collaborate on a project, you should consider pushing to [GitHub](https://github.com) in between pushing to Azure.</span></span>
 
### <a name="verify-the-active-deployment"></a><span data-ttu-id="dad9e-208">Comprobar la implementación activa</span><span class="sxs-lookup"><span data-stu-id="dad9e-208">Verify the Active Deployment</span></span>

<span data-ttu-id="dad9e-209">Puede comprobar que transfirió correctamente la aplicación web del entorno local a Azure.</span><span class="sxs-lookup"><span data-stu-id="dad9e-209">You can verify that you successfully transferred the web app from your local environment to Azure.</span></span> <span data-ttu-id="dad9e-210">Verá que aparece la implementación correcta.</span><span class="sxs-lookup"><span data-stu-id="dad9e-210">You'll see the listed successful deployment.</span></span>

1. <span data-ttu-id="dad9e-211">En [Azure Portal](https://portal.azure.com), seleccione la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="dad9e-211">In the [Azure Portal](https://portal.azure.com), select your web app.</span></span> <span data-ttu-id="dad9e-212">Luego, seleccione **Configuración** > **Implementación continua**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-212">Then, select **Settings** > **Continuous deployment**.</span></span>

   ![Azure Portal: Hoja Configuración: Hoja Implementaciones donde se muestra una implementación correcta](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a><span data-ttu-id="dad9e-214">Ejecutar la aplicación en Azure</span><span class="sxs-lookup"><span data-stu-id="dad9e-214">Run the app in Azure</span></span>

<span data-ttu-id="dad9e-215">Ahora que ha implementado la aplicación web en Azure, puede ejecutarla.</span><span class="sxs-lookup"><span data-stu-id="dad9e-215">Now that you have deployed your web app to Azure, you can run the app.</span></span>

<span data-ttu-id="dad9e-216">Esto se puede hacer de dos maneras:</span><span class="sxs-lookup"><span data-stu-id="dad9e-216">This can be done in two ways:</span></span>

* <span data-ttu-id="dad9e-217">En Azure Portal, localice la hoja Aplicación web de su aplicación web y haga clic en **Examinar** para ver la aplicación en su explorador predeterminado.</span><span class="sxs-lookup"><span data-stu-id="dad9e-217">In the Azure Portal, locate the web app blade for your web app, and click **Browse** to view your app in your default browser.</span></span>

* <span data-ttu-id="dad9e-218">Abra un explorador y escriba la dirección URL de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="dad9e-218">Open a browser and enter the URL for your web app.</span></span> <span data-ttu-id="dad9e-219">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="dad9e-219">For example:</span></span>

  `http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-your-web-app-and-republish"></a><span data-ttu-id="dad9e-220">Actualizar la aplicación web y volver a publicarla</span><span class="sxs-lookup"><span data-stu-id="dad9e-220">Update your web app and republish</span></span>

<span data-ttu-id="dad9e-221">Después de realizar cambios en el código local, puede volver a publicar la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="dad9e-221">After you make changes to your local code, you can republish.</span></span>

1.  <span data-ttu-id="dad9e-222">En el **Explorador de soluciones** de Visual Studio, abra el archivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="dad9e-222">In **Solution Explorer** of Visual Studio, open the *Startup.cs* file.</span></span>

2.  <span data-ttu-id="dad9e-223">En el método `Configure`, modifique el método `Response.WriteAsync` para que aparezca del siguiente modo:</span><span class="sxs-lookup"><span data-stu-id="dad9e-223">In the `Configure` method, modify the `Response.WriteAsync` method so that it appears as follows:</span></span>

    ```aspx-cs
    await context.Response.WriteAsync("Hello World! Deploy to Azure.");
    ```
3.  <span data-ttu-id="dad9e-224">Guarde los cambios en *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="dad9e-224">Save changes to *Startup.cs*.</span></span>

4.  <span data-ttu-id="dad9e-225">En el **Explorador de soluciones**, haga clic con el botón derecho en **Solución 'SampleWebAppDemo'** y seleccione **Confirmar**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-225">In **Solution Explorer**, right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="dad9e-226">Aparecerá **Team Explorer**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-226">The **Team Explorer** will be displayed.</span></span>

5.  <span data-ttu-id="dad9e-227">Escriba un mensaje de confirmación, como el siguiente:</span><span class="sxs-lookup"><span data-stu-id="dad9e-227">Enter a commit message, such as:</span></span>

    ```none
    Update #2
    ```

6.  <span data-ttu-id="dad9e-228">Presione el botón **Confirmar** para confirmar los cambios del proyecto.</span><span class="sxs-lookup"><span data-stu-id="dad9e-228">Press the **Commit** button to commit the project changes.</span></span>

7.  <span data-ttu-id="dad9e-229">Seleccione **Inicio** > **Sincronizar** > **Acciones** > **Inserción**.</span><span class="sxs-lookup"><span data-stu-id="dad9e-229">Select **Home** > **Sync** > **Actions** > **Push**.</span></span>

>[!NOTE]
><span data-ttu-id="dad9e-230">Como alternativa, puede insertar los cambios desde la **ventana de comandos**. Para ello, abra la **ventana de comandos**, vaya al directorio del proyecto y escriba un comando git.</span><span class="sxs-lookup"><span data-stu-id="dad9e-230">As an alternative, you can push your changes from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering a git command.</span></span> <span data-ttu-id="dad9e-231">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="dad9e-231">For example:</span></span>
>
>`git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a><span data-ttu-id="dad9e-232">Ver la aplicación web actualizada en Azure</span><span class="sxs-lookup"><span data-stu-id="dad9e-232">View the updated web app in Azure</span></span>

<span data-ttu-id="dad9e-233">Para ver la aplicación web actualizada, seleccione **Examinar** en la hoja de la aplicación web de Azure Portal o abra un explorador y escriba la dirección URL de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="dad9e-233">View your updated web app by selecting **Browse** from the web app blade in the Azure Portal or by opening a browser and entering the URL for your web app.</span></span> <span data-ttu-id="dad9e-234">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="dad9e-234">For example:</span></span>

   `http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a><span data-ttu-id="dad9e-235">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="dad9e-235">Additional Resources</span></span>

* [<span data-ttu-id="dad9e-236">Publicación e implementación</span><span class="sxs-lookup"><span data-stu-id="dad9e-236">Publishing and Deployment</span></span>](index.md)

* [<span data-ttu-id="dad9e-237">Proyecto Kudu</span><span class="sxs-lookup"><span data-stu-id="dad9e-237">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
