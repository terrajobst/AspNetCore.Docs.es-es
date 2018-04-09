---
title: Implementación continua a Azure con Visual Studio y Git con ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo crear una aplicación web de ASP.NET Core con Visual Studio e implementarla en Azure App Service con Git para una implementación continua.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/azure-continuous-deployment
ms.openlocfilehash: 4de1893e8c1f7f2f4d9af7278a110067ea777c61
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
# <a name="continuous-deployment-to-azure-with-visual-studio-and-git-with-aspnet-core"></a><span data-ttu-id="09ec0-103">Implementación continua a Azure con Visual Studio y Git con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="09ec0-103">Continuous deployment to Azure with Visual Studio and Git with ASP.NET Core</span></span>

<span data-ttu-id="09ec0-104">Por [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="09ec0-104">By [Erik Reitan](https://github.com/Erikre)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="09ec0-105">Este tutorial muestra cómo crear una aplicación web de ASP.NET Core con Visual Studio e implementarla desde Visual Studio en el servicio de aplicaciones de Azure mediante la implementación continua.</span><span class="sxs-lookup"><span data-stu-id="09ec0-105">This tutorial shows how to create an ASP.NET Core web app using Visual Studio and deploy it from Visual Studio to Azure App Service using continuous deployment.</span></span>

<span data-ttu-id="09ec0-106">Vea también [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic) (Usar VSTS para crear y publicar una aplicación web de Azure con la implementación continua), donde se muestra cómo configurar un flujo de trabajo de una entrega continua (CD) para [Azure App Service](/azure/app-service/app-service-web-overview) con Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="09ec0-106">See also [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), which shows how to configure a continuous delivery (CD) workflow for [Azure App Service](/azure/app-service/app-service-web-overview) using Visual Studio Team Services.</span></span> <span data-ttu-id="09ec0-107">La entrega continua Azure en Team Services simplifica la configuración una canalización de implementación sólida para publicar actualizaciones para las aplicaciones hospedadas en el servicio de aplicación de Azure.</span><span class="sxs-lookup"><span data-stu-id="09ec0-107">Azure Continuous Delivery in Team Services simplifies setting up a robust deployment pipeline to publish updates for apps hosted in Azure App Service.</span></span> <span data-ttu-id="09ec0-108">La canalización se puede configurar desde el portal de Azure para crear, ejecutar pruebas, implementar en una ranura de ensayo y, a continuación, implementarse en producción.</span><span class="sxs-lookup"><span data-stu-id="09ec0-108">The pipeline can be configured from the Azure portal to build, run tests, deploy to a staging slot, and then deploy to production.</span></span>

> [!NOTE]
> <span data-ttu-id="09ec0-109">Para completar este tutorial, se requiere una cuenta de Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="09ec0-109">To complete this tutorial, a Microsoft Azure account is required.</span></span> <span data-ttu-id="09ec0-110">Para obtener una cuenta, [activar las ventajas de suscriptor MSDN](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) o [registrarse para obtener una prueba gratuita de](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="09ec0-110">To obtain an account, [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="09ec0-111">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="09ec0-111">Prerequisites</span></span>

<span data-ttu-id="09ec0-112">Este tutorial se da por supuesto que está instalado el siguiente software:</span><span class="sxs-lookup"><span data-stu-id="09ec0-112">This tutorial assumes the following software is installed:</span></span>

* [<span data-ttu-id="09ec0-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="09ec0-113">Visual Studio</span></span>](https://www.visualstudio.com)
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="09ec0-114">[Git](https://git-scm.com/downloads) para Windows</span><span class="sxs-lookup"><span data-stu-id="09ec0-114">[Git](https://git-scm.com/downloads) for Windows</span></span>

## <a name="create-an-aspnet-core-web-app"></a><span data-ttu-id="09ec0-115">Crear una aplicación web de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="09ec0-115">Create an ASP.NET Core web app</span></span>

1. <span data-ttu-id="09ec0-116">Inicie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="09ec0-116">Start Visual Studio.</span></span>

1. <span data-ttu-id="09ec0-117">En el menú **Archivo**, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="09ec0-117">From the **File** menu, select **New** > **Project**.</span></span>

1. <span data-ttu-id="09ec0-118">Seleccione la plantilla de proyecto **Aplicación web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="09ec0-118">Select the **ASP.NET Core Web Application** project template.</span></span> <span data-ttu-id="09ec0-119">Aparece en **Instalados** > **Plantillas** > **Visual C#** > **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="09ec0-119">It appears under **Installed** > **Templates** > **Visual C#** > **.NET Core**.</span></span> <span data-ttu-id="09ec0-120">Dé un nombre al proyecto `SampleWebAppDemo`.</span><span class="sxs-lookup"><span data-stu-id="09ec0-120">Name the project `SampleWebAppDemo`.</span></span> <span data-ttu-id="09ec0-121">Seleccione el **crear nuevo repositorio de Git** opción y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="09ec0-121">Select the **Create new Git repository** option and click **OK**.</span></span>

   ![Cuadro de diálogo Nuevo proyecto](azure-continuous-deployment/_static/01-new-project.png)

1. <span data-ttu-id="09ec0-123">En el cuadro de diálogo **Nuevo proyecto ASP.NET Core**, seleccione la plantilla **Vacía** de ASP.NET Core y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="09ec0-123">In the **New ASP.NET Core Project** dialog, select the ASP.NET Core **Empty** template, then click **OK**.</span></span>

   ![Cuadro de diálogo Nuevo proyecto ASP.NET](azure-continuous-deployment/_static/02-web-site-template.png)

> [!NOTE]
> <span data-ttu-id="09ec0-125">La versión más reciente de .NET Core es 2.0.</span><span class="sxs-lookup"><span data-stu-id="09ec0-125">The most recent release of .NET Core is 2.0.</span></span>

### <a name="running-the-web-app-locally"></a><span data-ttu-id="09ec0-126">Ejecutar la aplicación web de forma local</span><span class="sxs-lookup"><span data-stu-id="09ec0-126">Running the web app locally</span></span>

1. <span data-ttu-id="09ec0-127">Cuando Visual Studio haya acabado de crear la aplicación, ejecútela seleccionando **Depurar** > **Iniciar depuración**.</span><span class="sxs-lookup"><span data-stu-id="09ec0-127">Once Visual Studio finishes creating the app, run the app by selecting **Debug** > **Start Debugging**.</span></span> <span data-ttu-id="09ec0-128">Como alternativa, presione **F5**.</span><span class="sxs-lookup"><span data-stu-id="09ec0-128">As an alternative, press **F5**.</span></span>

   <span data-ttu-id="09ec0-129">Visual Studio y la aplicación nueva pueden tardar un poco en inicializarse.</span><span class="sxs-lookup"><span data-stu-id="09ec0-129">It may take time to initialize Visual Studio and the new app.</span></span> <span data-ttu-id="09ec0-130">Una vez completada, el explorador muestra la aplicación en ejecución.</span><span class="sxs-lookup"><span data-stu-id="09ec0-130">Once it's complete, the browser shows the running app.</span></span>

   ![Ventana del explorador en la que aparece la aplicación en ejecución que muestra "Hello World!"](azure-continuous-deployment/_static/04-browser-runapp.png)

1. <span data-ttu-id="09ec0-132">Después de revisar la aplicación Web en ejecución, cierre el explorador y seleccione el icono "Detener depuración" en la barra de herramientas de Visual Studio para detener la aplicación.</span><span class="sxs-lookup"><span data-stu-id="09ec0-132">After reviewing the running Web app, close the browser and select the "Stop Debugging" icon in the toolbar of Visual Studio to stop the app.</span></span>

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="09ec0-133">Crear una aplicación web en Azure Portal</span><span class="sxs-lookup"><span data-stu-id="09ec0-133">Create a web app in the Azure Portal</span></span>

<span data-ttu-id="09ec0-134">Los pasos siguientes para crear una aplicación web en el Portal de Azure:</span><span class="sxs-lookup"><span data-stu-id="09ec0-134">The following steps create a web app in the Azure Portal:</span></span>

1. <span data-ttu-id="09ec0-135">Inicie sesión en el [Portal de Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="09ec0-135">Log in to the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="09ec0-136">Seleccione **NEW** en la parte superior izquierda de la interfaz del portal.</span><span class="sxs-lookup"><span data-stu-id="09ec0-136">Select **NEW** at the top left of the portal interface.</span></span>

1. <span data-ttu-id="09ec0-137">Seleccione **Web y móvil** > **aplicación Web**.</span><span class="sxs-lookup"><span data-stu-id="09ec0-137">Select **Web + Mobile** > **Web App**.</span></span>

   ![Microsoft Azure Portal: Botón Nuevo: Web y móvil en Marketplace: Botón Aplicación web en Aplicaciones destacadas](azure-continuous-deployment/_static/05-azure-newwebapp.png)

1. <span data-ttu-id="09ec0-139">En la hoja **Aplicación web**, escriba un valor único para el **nombre del servicio de aplicaciones**.</span><span class="sxs-lookup"><span data-stu-id="09ec0-139">In the **Web App** blade, enter a unique value for the **App Service Name**.</span></span>

   ![Hoja Aplicación web](azure-continuous-deployment/_static/06-azure-newappblade.png)

   > [!NOTE]
   > <span data-ttu-id="09ec0-141">El **nombre servicio de aplicaciones** nombre debe ser único.</span><span class="sxs-lookup"><span data-stu-id="09ec0-141">The **App Service Name** name must be unique.</span></span> <span data-ttu-id="09ec0-142">El portal aplica esta regla cuando se proporciona el nombre.</span><span class="sxs-lookup"><span data-stu-id="09ec0-142">The portal enforces this rule when the name is provided.</span></span> <span data-ttu-id="09ec0-143">Si proporciona un valor diferente, sustituya ese valor por cada aparición de **SampleWebAppDemo** en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="09ec0-143">If providing a different value, substitute that value for each occurrence of **SampleWebAppDemo** in this tutorial.</span></span>

   <span data-ttu-id="09ec0-144">También en la hoja **Aplicación web**, seleccione un plan o ubicación existente de App Service o bien cree uno.</span><span class="sxs-lookup"><span data-stu-id="09ec0-144">Also in the **Web App** blade, select an existing **App Service Plan/Location** or create a new one.</span></span> <span data-ttu-id="09ec0-145">Si crea un nuevo plan, seleccione el nivel de precios, la ubicación y otras opciones.</span><span class="sxs-lookup"><span data-stu-id="09ec0-145">If creating a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="09ec0-146">Para obtener más información sobre los planes de servicio de aplicaciones, consulte [información general detallada de planes de servicio de aplicaciones de Azure](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span><span class="sxs-lookup"><span data-stu-id="09ec0-146">For more information on App Service plans, see [Azure App Service plans in-depth overview](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span></span>

1. <span data-ttu-id="09ec0-147">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="09ec0-147">Select **Create**.</span></span> <span data-ttu-id="09ec0-148">Aprovisionar Azure e iniciar la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="09ec0-148">Azure will provision and start the web app.</span></span>

   ![Azure Portal: Hoja de Essentials SampleWebAppDemo01](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="09ec0-150">Habilitar la publicación de Git para la nueva aplicación web</span><span class="sxs-lookup"><span data-stu-id="09ec0-150">Enable Git publishing for the new web app</span></span>

<span data-ttu-id="09ec0-151">GIT es un sistema de control de versiones distribuidas que puede usarse para implementar una aplicación web de servicio de aplicaciones de Azure.</span><span class="sxs-lookup"><span data-stu-id="09ec0-151">Git is a distributed version control system that can be used to deploy an Azure App Service web app.</span></span> <span data-ttu-id="09ec0-152">Código de la aplicación Web se almacena en un repositorio de Git local y el código se implementa en Azure mediante la aplicación en un repositorio remoto.</span><span class="sxs-lookup"><span data-stu-id="09ec0-152">Web app code is stored in a local Git repository, and the code is deployed to Azure by pushing to a remote repository.</span></span>

1. <span data-ttu-id="09ec0-153">Inicie sesión en el [Portal de Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="09ec0-153">Log into the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="09ec0-154">Seleccione **servicios de aplicaciones** para ver una lista de los servicios de aplicación asociados a la suscripción de Azure.</span><span class="sxs-lookup"><span data-stu-id="09ec0-154">Select **App Services** to view a list of the app services associated with the Azure subscription.</span></span>

1. <span data-ttu-id="09ec0-155">Seleccione la aplicación web que creó en la sección anterior de este tutorial.</span><span class="sxs-lookup"><span data-stu-id="09ec0-155">Select the web app created in the previous section of this tutorial.</span></span>

1. <span data-ttu-id="09ec0-156">En la hoja **Implementación**, seleccione **Opciones de implementación** > **Elegir origen** > **Repositorio de Git local**.</span><span class="sxs-lookup"><span data-stu-id="09ec0-156">In the **Deployment** blade, select **Deployment options** > **Choose Source** > **Local Git Repository**.</span></span>

   ![Hoja Configuración: Hoja Origen de implementación: Hoja Elegir origen](azure-continuous-deployment/_static/deployment-options.png)

1. <span data-ttu-id="09ec0-158">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="09ec0-158">Select **OK**.</span></span>

1. <span data-ttu-id="09ec0-159">Si las credenciales de implementación para la publicación de una aplicación web u otra aplicación de servicio de aplicaciones todavía no ha configurado previamente, configurarlos ahora:</span><span class="sxs-lookup"><span data-stu-id="09ec0-159">If deployment credentials for publishing a web app or other App Service app haven't previously been set up, set them up now:</span></span>

   * <span data-ttu-id="09ec0-160">Seleccione **configuración** > **las credenciales de implementación**.</span><span class="sxs-lookup"><span data-stu-id="09ec0-160">Select **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="09ec0-161">El **configurar credenciales de implementación** hoja se muestra.</span><span class="sxs-lookup"><span data-stu-id="09ec0-161">The **Set deployment credentials** blade is displayed.</span></span>
   * <span data-ttu-id="09ec0-162">Cree un nombre de usuario y una contraseña.</span><span class="sxs-lookup"><span data-stu-id="09ec0-162">Create a user name and password.</span></span> <span data-ttu-id="09ec0-163">Guarde la contraseña para su uso posterior al configurar Git.</span><span class="sxs-lookup"><span data-stu-id="09ec0-163">Save the password for later use when setting up Git.</span></span>
   * <span data-ttu-id="09ec0-164">Seleccione **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="09ec0-164">Select **Save**.</span></span>

1. <span data-ttu-id="09ec0-165">En el **aplicación Web** hoja, seleccione **configuración** > **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="09ec0-165">In the **Web App** blade, select **Settings** > **Properties**.</span></span> <span data-ttu-id="09ec0-166">Se muestra la dirección URL del repositorio de Git remoto para implementar en **dirección URL de GIT**.</span><span class="sxs-lookup"><span data-stu-id="09ec0-166">The URL of the remote Git repository to deploy to is shown under **GIT URL**.</span></span>

1. <span data-ttu-id="09ec0-167">Copie el valor de **Dirección URL de Git** para usarlo más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="09ec0-167">Copy the **GIT URL** value for later use in the tutorial.</span></span>

   ![Azure Portal: Hoja Propiedades de la aplicación](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-the-web-app-to-azure-app-service"></a><span data-ttu-id="09ec0-169">Publicar la aplicación web en el servicio de aplicaciones de Azure</span><span class="sxs-lookup"><span data-stu-id="09ec0-169">Publish the web app to Azure App Service</span></span>

<span data-ttu-id="09ec0-170">En esta sección, creará un repositorio Git local mediante Visual Studio e inserción desde ese repositorio en Azure para implementar la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="09ec0-170">In this section, create a local Git repository using Visual Studio and push from that repository to Azure to deploy the web app.</span></span> <span data-ttu-id="09ec0-171">Los pasos son los siguientes:</span><span class="sxs-lookup"><span data-stu-id="09ec0-171">The steps involved include the following:</span></span>

* <span data-ttu-id="09ec0-172">Agrega la opción de repositorio remoto utilizando el valor de la dirección URL de GIT, por lo que el repositorio local se puede implementar en Azure.</span><span class="sxs-lookup"><span data-stu-id="09ec0-172">Add the remote repository setting using the GIT URL value, so the local repository can be deployed to Azure.</span></span>
* <span data-ttu-id="09ec0-173">Confirmar cambios en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="09ec0-173">Commit project changes.</span></span>
* <span data-ttu-id="09ec0-174">Inserte los cambios de proyecto desde el repositorio local en el repositorio remoto en Azure.</span><span class="sxs-lookup"><span data-stu-id="09ec0-174">Push project changes from the local repository to the remote repository on Azure.</span></span>

1. <span data-ttu-id="09ec0-175">En el **Explorador de soluciones**, haga clic con el botón derecho en **Solución 'SampleWebAppDemo'** y seleccione **Confirmar**.</span><span class="sxs-lookup"><span data-stu-id="09ec0-175">In **Solution Explorer** right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="09ec0-176">El **Team Explorer** se muestra.</span><span class="sxs-lookup"><span data-stu-id="09ec0-176">The **Team Explorer** is displayed.</span></span>

   ![Pestaña Team Explorer: Conexión](azure-continuous-deployment/_static/10-team-explorer.png)

1. <span data-ttu-id="09ec0-178">En **Team Explorer**, seleccione **Inicio** (icono Inicio) > **Configuración** > **Configuración del repositorio**.</span><span class="sxs-lookup"><span data-stu-id="09ec0-178">In **Team Explorer**, select the **Home** (home icon) > **Settings** > **Repository Settings**.</span></span>

1. <span data-ttu-id="09ec0-179">En el **controles remotos** sección de la **configuración del repositorio**, seleccione **agregar**.</span><span class="sxs-lookup"><span data-stu-id="09ec0-179">In the **Remotes** section of the **Repository Settings**, select **Add**.</span></span> <span data-ttu-id="09ec0-180">El **agregar remoto** se muestra el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="09ec0-180">The **Add Remote** dialog box is displayed.</span></span>

1. <span data-ttu-id="09ec0-181">Establezca el **nombre** del repositorio remoto en **Azure-SampleApp**.</span><span class="sxs-lookup"><span data-stu-id="09ec0-181">Set the **Name** of the remote to **Azure-SampleApp**.</span></span>

1. <span data-ttu-id="09ec0-182">Establezca el valor de **capturar** a la **dirección URL de Git** que copiado desde Azure anteriormente en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="09ec0-182">Set the value for **Fetch** to the **Git URL** that copied from Azure earlier in this tutorial.</span></span> <span data-ttu-id="09ec0-183">Tenga en cuenta que esta es la dirección URL que termina en **.git**.</span><span class="sxs-lookup"><span data-stu-id="09ec0-183">Note that this is the URL that ends with **.git**.</span></span>

   ![Cuadro de diálogo Editar Azure-SampleApp remoto](azure-continuous-deployment/_static/11-add-remote.png)

   > [!NOTE]
   > <span data-ttu-id="09ec0-185">Como alternativa, especifique el repositorio remoto desde el **ventana de comandos** abriendo el **ventana de comandos**, cambiar al directorio del proyecto y escriba el comando.</span><span class="sxs-lookup"><span data-stu-id="09ec0-185">As an alternative, specify the remote repository from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the command.</span></span> <span data-ttu-id="09ec0-186">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="09ec0-186">Example:</span></span>
   >
   > `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

1. <span data-ttu-id="09ec0-187">Seleccione **Inicio** (icono de Inicio) > **Configuración** > **Configuración global**.</span><span class="sxs-lookup"><span data-stu-id="09ec0-187">Select the **Home** (home icon) > **Settings** > **Global Settings**.</span></span> <span data-ttu-id="09ec0-188">Confirme que se establecen el nombre y direcciones de correo.</span><span class="sxs-lookup"><span data-stu-id="09ec0-188">Confirm that the name and email address are set.</span></span> <span data-ttu-id="09ec0-189">Seleccione **actualización** si es necesario.</span><span class="sxs-lookup"><span data-stu-id="09ec0-189">Select **Update** if required.</span></span>

1. <span data-ttu-id="09ec0-190">Seleccione **Inicio** > **Cambios** para volver a la vista **Cambios**.</span><span class="sxs-lookup"><span data-stu-id="09ec0-190">Select **Home** > **Changes** to return to the **Changes** view.</span></span>

1. <span data-ttu-id="09ec0-191">Escriba un mensaje de confirmación, como **inicial Push #1** y seleccione **confirmación**.</span><span class="sxs-lookup"><span data-stu-id="09ec0-191">Enter a commit message, such as **Initial Push #1** and select **Commit**.</span></span> <span data-ttu-id="09ec0-192">Esta acción crea un *confirmación* localmente.</span><span class="sxs-lookup"><span data-stu-id="09ec0-192">This action creates a *commit* locally.</span></span>

   ![Pestaña Team Explorer: Conexión](azure-continuous-deployment/_static/12-initial-commit.png)

   > [!NOTE]
   > <span data-ttu-id="09ec0-194">Como alternativa, guardar los cambios de la **ventana de comandos** abriendo el **ventana de comandos**, cambiar al directorio del proyecto y escriba los comandos de git.</span><span class="sxs-lookup"><span data-stu-id="09ec0-194">As an alternative, commit changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the git commands.</span></span> <span data-ttu-id="09ec0-195">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="09ec0-195">Example:</span></span>
   >
   > `git add .`
   >
   > `git commit -am "Initial Push #1"`

1. <span data-ttu-id="09ec0-196">Seleccione **Inicio** > **Sincronizar** > **Acciones** > **Abrir símbolo del sistema**.</span><span class="sxs-lookup"><span data-stu-id="09ec0-196">Select **Home** > **Sync** > **Actions** > **Open Command Prompt**.</span></span> <span data-ttu-id="09ec0-197">El símbolo del sistema se abrirá en el directorio del proyecto.</span><span class="sxs-lookup"><span data-stu-id="09ec0-197">The command prompt opens to the project directory.</span></span>

1. <span data-ttu-id="09ec0-198">Escriba el siguiente comando en la ventana de comandos:</span><span class="sxs-lookup"><span data-stu-id="09ec0-198">Enter the following command in the command window:</span></span>

   `git push -u Azure-SampleApp master`

1. <span data-ttu-id="09ec0-199">Escriba Azure **las credenciales de implementación** contraseña que creó anteriormente en Azure.</span><span class="sxs-lookup"><span data-stu-id="09ec0-199">Enter the Azure **deployment credentials** password created earlier in Azure.</span></span>

   <span data-ttu-id="09ec0-200">Este comando inicia el proceso de inserción de los archivos de proyecto local a Azure.</span><span class="sxs-lookup"><span data-stu-id="09ec0-200">This command starts the process of pushing the local project files to Azure.</span></span> <span data-ttu-id="09ec0-201">El resultado del comando anterior finaliza con el mensaje que indica que la implementación se realizó correctamente.</span><span class="sxs-lookup"><span data-stu-id="09ec0-201">The output from the above command ends with a message that the deployment was successful.</span></span>

   ```
   remote: Finished successfully.
   remote: Running post deployment command(s)...
   remote: Deployment successful.
   To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
   * [new branch]      master -> master
   Branch master set up to track remote branch master from Azure-SampleApp.
   ```

   > [!NOTE]
   > <span data-ttu-id="09ec0-202">Si se requiere la colaboración en el proyecto, considere la posibilidad de insertar en [GitHub](https://github.com) antes de insertar en Azure.</span><span class="sxs-lookup"><span data-stu-id="09ec0-202">If collaboration on the project is required, consider pushing to [GitHub](https://github.com) before pushing to Azure.</span></span>
 
### <a name="verify-the-active-deployment"></a><span data-ttu-id="09ec0-203">Comprobar la implementación activa</span><span class="sxs-lookup"><span data-stu-id="09ec0-203">Verify the Active Deployment</span></span>

<span data-ttu-id="09ec0-204">Compruebe que la transferencia de la aplicación web desde el entorno local a Azure es correcta.</span><span class="sxs-lookup"><span data-stu-id="09ec0-204">Verify that the web app transfer from the local environment to Azure is successful.</span></span>

<span data-ttu-id="09ec0-205">En el [Portal de Azure](https://portal.azure.com), seleccione la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="09ec0-205">In the [Azure Portal](https://portal.azure.com), select the web app.</span></span> <span data-ttu-id="09ec0-206">Seleccione **implementación** > **opciones de implementación**.</span><span class="sxs-lookup"><span data-stu-id="09ec0-206">Select **Deployment** > **Deployment options**.</span></span>

![Azure Portal: Hoja Configuración: Hoja Implementaciones donde se muestra una implementación correcta](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a><span data-ttu-id="09ec0-208">Ejecutar la aplicación en Azure</span><span class="sxs-lookup"><span data-stu-id="09ec0-208">Run the app in Azure</span></span>

<span data-ttu-id="09ec0-209">Ahora que la aplicación web se implementa en Azure, ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="09ec0-209">Now that the web app is deployed to Azure, run the app.</span></span>

<span data-ttu-id="09ec0-210">Esto puede realizarse de dos maneras:</span><span class="sxs-lookup"><span data-stu-id="09ec0-210">This can be accomplished in two ways:</span></span>

* <span data-ttu-id="09ec0-211">En el Portal de Azure, localice la hoja de la aplicación web para la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="09ec0-211">In the Azure Portal, locate the web app blade for the web app.</span></span> <span data-ttu-id="09ec0-212">Seleccione **examinar** para ver la aplicación en el explorador predeterminado.</span><span class="sxs-lookup"><span data-stu-id="09ec0-212">Select **Browse** to view the app in the default browser.</span></span>
* <span data-ttu-id="09ec0-213">Abra un explorador y escriba la dirección URL de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="09ec0-213">Open a browser and enter the URL for the web app.</span></span> <span data-ttu-id="09ec0-214">Ejemplo: `http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="09ec0-214">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="update-the-web-app-and-republish"></a><span data-ttu-id="09ec0-215">Actualización de la aplicación web y volver a publicar</span><span class="sxs-lookup"><span data-stu-id="09ec0-215">Update the web app and republish</span></span>

<span data-ttu-id="09ec0-216">Después de realizar cambios en el código local, vuelva a publicar:</span><span class="sxs-lookup"><span data-stu-id="09ec0-216">After making changes to the local code, republish:</span></span>

1. <span data-ttu-id="09ec0-217">En el **Explorador de soluciones** de Visual Studio, abra el archivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="09ec0-217">In **Solution Explorer** of Visual Studio, open the *Startup.cs* file.</span></span>

1. <span data-ttu-id="09ec0-218">En el método `Configure`, modifique el método `Response.WriteAsync` para que aparezca del siguiente modo:</span><span class="sxs-lookup"><span data-stu-id="09ec0-218">In the `Configure` method, modify the `Response.WriteAsync` method so that it appears as follows:</span></span>

   ```csharp
   await context.Response.WriteAsync("Hello World! Deploy to Azure.");
   ```

1. <span data-ttu-id="09ec0-219">Guardar los cambios realizados en *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="09ec0-219">Save the changes to *Startup.cs*.</span></span>

1. <span data-ttu-id="09ec0-220">En el **Explorador de soluciones**, haga clic con el botón derecho en **Solución 'SampleWebAppDemo'** y seleccione **Confirmar**.</span><span class="sxs-lookup"><span data-stu-id="09ec0-220">In **Solution Explorer**, right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="09ec0-221">El **Team Explorer** se muestra.</span><span class="sxs-lookup"><span data-stu-id="09ec0-221">The **Team Explorer** is displayed.</span></span>

1. <span data-ttu-id="09ec0-222">Escriba un mensaje de confirmación, como `Update #2`.</span><span class="sxs-lookup"><span data-stu-id="09ec0-222">Enter a commit message, such as `Update #2`.</span></span>

1. <span data-ttu-id="09ec0-223">Presione el botón **Confirmar** para confirmar los cambios del proyecto.</span><span class="sxs-lookup"><span data-stu-id="09ec0-223">Press the **Commit** button to commit the project changes.</span></span>

1. <span data-ttu-id="09ec0-224">Seleccione **Inicio** > **Sincronizar** > **Acciones** > **Inserción**.</span><span class="sxs-lookup"><span data-stu-id="09ec0-224">Select **Home** > **Sync** > **Actions** > **Push**.</span></span>

> [!NOTE]
> <span data-ttu-id="09ec0-225">Como alternativa, inserte los cambios desde el **ventana de comandos** abriendo el **ventana de comandos**, cambiar al directorio del proyecto y escriba un comando de git.</span><span class="sxs-lookup"><span data-stu-id="09ec0-225">As an alternative, push the changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering a git command.</span></span> <span data-ttu-id="09ec0-226">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="09ec0-226">Example:</span></span>
> 
> `git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a><span data-ttu-id="09ec0-227">Ver la aplicación web actualizada en Azure</span><span class="sxs-lookup"><span data-stu-id="09ec0-227">View the updated web app in Azure</span></span>

<span data-ttu-id="09ec0-228">Ver la aplicación web se actualizó seleccionando **examinar** desde la hoja de la aplicación web en el Portal de Azure, o bien, abra un explorador y escriba la dirección URL de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="09ec0-228">View the updated web app by selecting **Browse** from the web app blade in the Azure Portal or by opening a browser and entering the URL for the web app.</span></span> <span data-ttu-id="09ec0-229">Ejemplo: `http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="09ec0-229">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="additional-resources"></a><span data-ttu-id="09ec0-230">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="09ec0-230">Additional resources</span></span>

* [<span data-ttu-id="09ec0-231">Usar VSTS para generar y publicar en una aplicación Web de Azure con una implementación continua</span><span class="sxs-lookup"><span data-stu-id="09ec0-231">Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment</span></span>](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic)
* [<span data-ttu-id="09ec0-232">Proyecto Kudu</span><span class="sxs-lookup"><span data-stu-id="09ec0-232">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
