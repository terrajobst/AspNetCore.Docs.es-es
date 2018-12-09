---
title: 'Implementar una aplicación en App Service: DevOps con ASP.NET Core y Azure'
author: CamSoper
description: Implementar una aplicación ASP.NET Core en Azure App Service, el primer paso para DevOps con ASP.NET Core y Azure.
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/deploy-to-app-service
ms.openlocfilehash: 4424d3d15cbd234357c8265fa276834cb9abf352
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121225"
---
# <a name="deploy-an-app-to-app-service"></a><span data-ttu-id="d9c88-103">Implementar una aplicación en App Service</span><span class="sxs-lookup"><span data-stu-id="d9c88-103">Deploy an app to App Service</span></span>

<span data-ttu-id="d9c88-104">[Azure App Service](/azure/app-service/) es plataforma de hospedaje web de Azure.</span><span class="sxs-lookup"><span data-stu-id="d9c88-104">[Azure App Service](/azure/app-service/) is Azure's web hosting platform.</span></span> <span data-ttu-id="d9c88-105">Implementar una aplicación web en Azure App Service puede realizarse manualmente o mediante un proceso automatizado.</span><span class="sxs-lookup"><span data-stu-id="d9c88-105">Deploying a web app to Azure App Service can be done manually or by an automated process.</span></span> <span data-ttu-id="d9c88-106">En esta sección de la guía se describe los métodos de implementación que se pueden desencadenar manualmente o mediante un script mediante la línea de comandos, o desencadenan manualmente con Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d9c88-106">This section of the guide discusses deployment methods that can be triggered manually or by script using the command line, or triggered manually using Visual Studio.</span></span>

<span data-ttu-id="d9c88-107">En esta sección, podrá realizar las tareas siguientes:</span><span class="sxs-lookup"><span data-stu-id="d9c88-107">In this section, you'll accomplish the following tasks:</span></span>

* <span data-ttu-id="d9c88-108">Descargar y compilar la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="d9c88-108">Download and build the sample app.</span></span>
* <span data-ttu-id="d9c88-109">Crear una aplicación Web de Azure App Service mediante Azure Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="d9c88-109">Create an Azure App Service Web App using the Azure Cloud Shell.</span></span>
* <span data-ttu-id="d9c88-110">Implementar la aplicación de ejemplo en Azure con Git.</span><span class="sxs-lookup"><span data-stu-id="d9c88-110">Deploy the sample app to Azure using Git.</span></span>
* <span data-ttu-id="d9c88-111">Implementar un cambio en la aplicación mediante Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d9c88-111">Deploy a change to the app using Visual Studio.</span></span>
* <span data-ttu-id="d9c88-112">Agregar una ranura de ensayo a la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="d9c88-112">Add a staging slot to the web app.</span></span>
* <span data-ttu-id="d9c88-113">Implementar una actualización en la ranura de ensayo.</span><span class="sxs-lookup"><span data-stu-id="d9c88-113">Deploy an update to the staging slot.</span></span>
* <span data-ttu-id="d9c88-114">Intercambiar las ranuras de ensayo y producción.</span><span class="sxs-lookup"><span data-stu-id="d9c88-114">Swap the staging and production slots.</span></span>

## <a name="download-and-test-the-app"></a><span data-ttu-id="d9c88-115">Descargar y probar la aplicación</span><span class="sxs-lookup"><span data-stu-id="d9c88-115">Download and test the app</span></span>

<span data-ttu-id="d9c88-116">La aplicación se usa en esta guía es una aplicación de ASP.NET Core pregenerada [Simple lector de fuentes](https://github.com/Azure-Samples/simple-feed-reader/).</span><span class="sxs-lookup"><span data-stu-id="d9c88-116">The app used in this guide is a pre-built ASP.NET Core app, [Simple Feed Reader](https://github.com/Azure-Samples/simple-feed-reader/).</span></span> <span data-ttu-id="d9c88-117">Es una aplicación de páginas de Razor que usa el `Microsoft.SyndicationFeed.ReaderWriter` API para recuperar una fuente RSS/Atom y mostrar los elementos de noticias de una lista.</span><span class="sxs-lookup"><span data-stu-id="d9c88-117">It's a Razor Pages app that uses the `Microsoft.SyndicationFeed.ReaderWriter` API to retrieve an RSS/Atom feed and display the news items in a list.</span></span>

<span data-ttu-id="d9c88-118">No dude en revisar el código, pero es importante entender que no hay nada especial acerca de esta aplicación.</span><span class="sxs-lookup"><span data-stu-id="d9c88-118">Feel free to review the code, but it's important to understand that there's nothing special about this app.</span></span> <span data-ttu-id="d9c88-119">Es simplemente una sencilla aplicación de ASP.NET Core con fines ilustrativos.</span><span class="sxs-lookup"><span data-stu-id="d9c88-119">It's just a simple ASP.NET Core app for illustrative purposes.</span></span>

<span data-ttu-id="d9c88-120">Desde un shell de comandos, descargar el código, compile el proyecto y ejecútelo como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="d9c88-120">From a command shell, download the code, build the project, and run it as follows.</span></span>

> <span data-ttu-id="d9c88-121">*Nota: Los usuarios de Linux/macOS deben realizar cambios correspondientes de las rutas de acceso, por ejemplo, con barra diagonal (`/`) en lugar de barra diagonal inversa (`\`).*</span><span class="sxs-lookup"><span data-stu-id="d9c88-121">*Note: Linux/macOS users should make appropriate changes for paths, e.g., using forward slash (`/`) rather than back slash (`\`).*</span></span>

1. <span data-ttu-id="d9c88-122">Clone el código en una carpeta en el equipo local.</span><span class="sxs-lookup"><span data-stu-id="d9c88-122">Clone the code to a folder on your local machine.</span></span>

    ```console
    git clone https://github.com/Azure-Samples/simple-feed-reader/
    ```

2. <span data-ttu-id="d9c88-123">Cambiar la carpeta de trabajo para el *lectura de fuentes simple* carpeta que se creó.</span><span class="sxs-lookup"><span data-stu-id="d9c88-123">Change your working folder to the *simple-feed-reader* folder that was created.</span></span>

    ```console
    cd .\simple-feed-reader\SimpleFeedReader
    ```

3. <span data-ttu-id="d9c88-124">Restaure los paquetes y compile la solución.</span><span class="sxs-lookup"><span data-stu-id="d9c88-124">Restore the packages, and build the solution.</span></span>

    ```console
    dotnet build
    ```

4. <span data-ttu-id="d9c88-125">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d9c88-125">Run the app.</span></span>

    ```console
    dotnet run
    ```

    ![El comando dotnet run es correcto](./media/deploying-to-app-service/dotnet-run.png)

5. <span data-ttu-id="d9c88-127">Abra un explorador y vaya a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="d9c88-127">Open a browser and navigate to `http://localhost:5000`.</span></span> <span data-ttu-id="d9c88-128">La aplicación le permite escribir o pegar una dirección URL de fuente de distribución y ver una lista de elementos de noticias.</span><span class="sxs-lookup"><span data-stu-id="d9c88-128">The app allows you to type or paste a syndication feed URL and view a list of news items.</span></span>

     ![La aplicación muestra el contenido de una fuente RSS](./media/deploying-to-app-service/app-in-browser.png)

6. <span data-ttu-id="d9c88-130">Cuando esté satisfecho la aplicación funciona correctamente, cierra presionando **Ctrl**+**C** en el shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="d9c88-130">Once you're satisfied the app is working correctly, shut it down by pressing **Ctrl**+**C** in the command shell.</span></span>

## <a name="create-the-azure-app-service-web-app"></a><span data-ttu-id="d9c88-131">Crear la aplicación Web de Azure App Service</span><span class="sxs-lookup"><span data-stu-id="d9c88-131">Create the Azure App Service Web App</span></span>

<span data-ttu-id="d9c88-132">Para implementar la aplicación, debe crear un servicio de aplicaciones [Web App](/azure/app-service/app-service-web-overview).</span><span class="sxs-lookup"><span data-stu-id="d9c88-132">To deploy the app, you'll need to create an App Service [Web App](/azure/app-service/app-service-web-overview).</span></span> <span data-ttu-id="d9c88-133">Tras la creación de la aplicación Web, deberá implementar en él desde el equipo local mediante Git.</span><span class="sxs-lookup"><span data-stu-id="d9c88-133">After creation of the Web App, you'll deploy to it from your local machine using Git.</span></span>

1. <span data-ttu-id="d9c88-134">Inicie sesión en el [Azure Cloud Shell](https://shell.azure.com/bash).</span><span class="sxs-lookup"><span data-stu-id="d9c88-134">Sign in to the [Azure Cloud Shell](https://shell.azure.com/bash).</span></span> <span data-ttu-id="d9c88-135">Nota: Al iniciar sesión por primera vez, Cloud Shell le insta a crear una cuenta de almacenamiento para archivos de configuración.</span><span class="sxs-lookup"><span data-stu-id="d9c88-135">Note: When you sign in for the first time, Cloud Shell prompts to create a storage account for configuration files.</span></span> <span data-ttu-id="d9c88-136">Acepte los valores predeterminados o proporcione un nombre único.</span><span class="sxs-lookup"><span data-stu-id="d9c88-136">Accept the defaults or provide a unique name.</span></span>

2. <span data-ttu-id="d9c88-137">Usar Cloud Shell para conocer los pasos siguientes.</span><span class="sxs-lookup"><span data-stu-id="d9c88-137">Use the Cloud Shell for the following steps.</span></span>

    <span data-ttu-id="d9c88-138">a.</span><span class="sxs-lookup"><span data-stu-id="d9c88-138">a.</span></span> <span data-ttu-id="d9c88-139">Declare una variable para almacenar el nombre de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="d9c88-139">Declare a variable to store your web app's name.</span></span> <span data-ttu-id="d9c88-140">El nombre debe ser único para su uso en la dirección URL predeterminada.</span><span class="sxs-lookup"><span data-stu-id="d9c88-140">The name must be unique to be used in the default URL.</span></span> <span data-ttu-id="d9c88-141">Mediante el `$RANDOM` función Bash para construir el nombre garantiza la exclusividad y da como resultado el formato `webappname99999`.</span><span class="sxs-lookup"><span data-stu-id="d9c88-141">Using the `$RANDOM` Bash function to construct the name guarantees uniqueness and results in the format `webappname99999`.</span></span>

    ```console
    webappname=mywebapp$RANDOM
    ```

    <span data-ttu-id="d9c88-142">b.</span><span class="sxs-lookup"><span data-stu-id="d9c88-142">b.</span></span> <span data-ttu-id="d9c88-143">Cree un grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="d9c88-143">Create a resource group.</span></span> <span data-ttu-id="d9c88-144">Grupos de recursos proporcionan un medio para agregar los recursos de Azure pueden administrarse como un grupo.</span><span class="sxs-lookup"><span data-stu-id="d9c88-144">Resource groups provide a means to aggregate Azure resources to be managed as a group.</span></span>

    ```azure-cli
    az group create --location centralus --name AzureTutorial
    ```

    <span data-ttu-id="d9c88-145">El `az` comando invoca el [CLI de Azure](/cli/azure/).</span><span class="sxs-lookup"><span data-stu-id="d9c88-145">The `az` command invokes the [Azure CLI](/cli/azure/).</span></span> <span data-ttu-id="d9c88-146">Se puede ejecutar la CLI localmente, pero usarlo en Cloud Shell permite ahorrar tiempo y la configuración.</span><span class="sxs-lookup"><span data-stu-id="d9c88-146">The CLI can be run locally, but using it in the Cloud Shell saves time and configuration.</span></span>

    <span data-ttu-id="d9c88-147">c.</span><span class="sxs-lookup"><span data-stu-id="d9c88-147">c.</span></span> <span data-ttu-id="d9c88-148">Crear un plan de App Service en el nivel S1.</span><span class="sxs-lookup"><span data-stu-id="d9c88-148">Create an App Service plan in the S1 tier.</span></span> <span data-ttu-id="d9c88-149">Un plan de App Service es una agrupación de aplicaciones web que comparten el mismo plan de tarifa.</span><span class="sxs-lookup"><span data-stu-id="d9c88-149">An App Service plan is a grouping of web apps that share the same pricing tier.</span></span> <span data-ttu-id="d9c88-150">El nivel de S1 no está disponible, pero es obligatorio para la característica de espacios de almacenamiento provisional.</span><span class="sxs-lookup"><span data-stu-id="d9c88-150">The S1 tier isn't free, but it's required for the staging slots feature.</span></span>

    ```azure-cli
    az appservice plan create --name $webappname --resource-group AzureTutorial --sku S1
    ```

    <span data-ttu-id="d9c88-151">d.</span><span class="sxs-lookup"><span data-stu-id="d9c88-151">d.</span></span> <span data-ttu-id="d9c88-152">Crear el recurso de aplicación web mediante el plan de App Service en el mismo grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="d9c88-152">Create the web app resource using the App Service plan in the same resource group.</span></span>

    ```azure-cli
    az webapp create --name $webappname --resource-group AzureTutorial --plan $webappname
    ```

    <span data-ttu-id="d9c88-153">e.</span><span class="sxs-lookup"><span data-stu-id="d9c88-153">e.</span></span> <span data-ttu-id="d9c88-154">Establecer las credenciales de implementación.</span><span class="sxs-lookup"><span data-stu-id="d9c88-154">Set the deployment credentials.</span></span> <span data-ttu-id="d9c88-155">Estas credenciales de implementación se aplican a todas las aplicaciones web en su suscripción.</span><span class="sxs-lookup"><span data-stu-id="d9c88-155">These deployment credentials apply to all the web apps in your subscription.</span></span> <span data-ttu-id="d9c88-156">No utilice caracteres especiales en el nombre de usuario.</span><span class="sxs-lookup"><span data-stu-id="d9c88-156">Don't use special characters in the user name.</span></span>

    ```azure-cli
    az webapp deployment user set --user-name REPLACE_WITH_USER_NAME --password REPLACE_WITH_PASSWORD
    ```

    <span data-ttu-id="d9c88-157">f.</span><span class="sxs-lookup"><span data-stu-id="d9c88-157">f.</span></span> <span data-ttu-id="d9c88-158">Configurar la aplicación web para que acepte las implementaciones de local Git y mostrar el *dirección URL de la implementación de Git*.</span><span class="sxs-lookup"><span data-stu-id="d9c88-158">Configure the web app to accept deployments from local Git and display the *Git deployment URL*.</span></span> <span data-ttu-id="d9c88-159">**Tenga en cuenta esta dirección URL para consultarla más tarde**.</span><span class="sxs-lookup"><span data-stu-id="d9c88-159">**Note this URL for reference later**.</span></span>

    ```azure-cli
    echo Git deployment URL: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --query url --output tsv)
    ```

    <span data-ttu-id="d9c88-160">g.</span><span class="sxs-lookup"><span data-stu-id="d9c88-160">g.</span></span> <span data-ttu-id="d9c88-161">Mostrar el *URL de aplicación web*.</span><span class="sxs-lookup"><span data-stu-id="d9c88-161">Display the *web app URL*.</span></span> <span data-ttu-id="d9c88-162">Vaya a esta dirección URL para ver la aplicación web en blanco.</span><span class="sxs-lookup"><span data-stu-id="d9c88-162">Browse to this URL to see the blank web app.</span></span> <span data-ttu-id="d9c88-163">**Tenga en cuenta esta dirección URL para consultarla más tarde**.</span><span class="sxs-lookup"><span data-stu-id="d9c88-163">**Note this URL for reference later**.</span></span>

    ```console
    echo Web app URL: http://$webappname.azurewebsites.net
    ```

3. <span data-ttu-id="d9c88-164">Mediante un shell de comandos en el equipo local, vaya a la carpeta del proyecto de la aplicación web (por ejemplo, `.\simple-feed-reader\SimpleFeedReader`).</span><span class="sxs-lookup"><span data-stu-id="d9c88-164">Using a command shell on your local machine, navigate to the web app's project folder (for example, `.\simple-feed-reader\SimpleFeedReader`).</span></span> <span data-ttu-id="d9c88-165">Ejecute los comandos siguientes para configurar Git para insertar en la dirección URL de implementación:</span><span class="sxs-lookup"><span data-stu-id="d9c88-165">Execute the following commands to set up Git to push to the deployment URL:</span></span>

    <span data-ttu-id="d9c88-166">a.</span><span class="sxs-lookup"><span data-stu-id="d9c88-166">a.</span></span> <span data-ttu-id="d9c88-167">Agregue la dirección URL remota en el repositorio local.</span><span class="sxs-lookup"><span data-stu-id="d9c88-167">Add the remote URL to the local repository.</span></span>

    ```console
    git remote add azure-prod GIT_DEPLOYMENT_URL
    ```

    <span data-ttu-id="d9c88-168">b.</span><span class="sxs-lookup"><span data-stu-id="d9c88-168">b.</span></span> <span data-ttu-id="d9c88-169">Insertar local *maestro* bifurcar a la *azure prod* del remoto *maestro* rama.</span><span class="sxs-lookup"><span data-stu-id="d9c88-169">Push the local *master* branch to the *azure-prod* remote's *master* branch.</span></span>

    ```console
    git push azure-prod master
    ```

    <span data-ttu-id="d9c88-170">Se le pedirá las credenciales de implementación que creó anteriormente.</span><span class="sxs-lookup"><span data-stu-id="d9c88-170">You'll be prompted for the deployment credentials you created earlier.</span></span> <span data-ttu-id="d9c88-171">Observe la salida en el shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="d9c88-171">Observe the output in the command shell.</span></span> <span data-ttu-id="d9c88-172">Azure compila la aplicación de ASP.NET Core de forma remota.</span><span class="sxs-lookup"><span data-stu-id="d9c88-172">Azure builds the ASP.NET Core app remotely.</span></span>

4. <span data-ttu-id="d9c88-173">En un explorador, vaya a la *URL de aplicación Web* y tenga en cuenta la aplicación se ha generado e implementado.</span><span class="sxs-lookup"><span data-stu-id="d9c88-173">In a browser, navigate to the *Web app URL* and note the app has been built and deployed.</span></span> <span data-ttu-id="d9c88-174">Cambios adicionales se pueden confirmados en el repositorio de Git local con `git commit`.</span><span class="sxs-lookup"><span data-stu-id="d9c88-174">Additional changes can be committed to the local Git repository with `git commit`.</span></span> <span data-ttu-id="d9c88-175">Estos cambios se insertan en Azure a la anterior `git push` comando.</span><span class="sxs-lookup"><span data-stu-id="d9c88-175">These changes are pushed to Azure with the preceding `git push` command.</span></span>

## <a name="deployment-with-visual-studio"></a><span data-ttu-id="d9c88-176">Implementación con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d9c88-176">Deployment with Visual Studio</span></span>

> <span data-ttu-id="d9c88-177">*Nota: Esta sección solo se aplica a Windows. Los usuarios de Linux y macOS deben realizar el cambio que se describe en el paso 2 a continuación. Guarde el archivo y confirme el cambio en el repositorio local con `git commit`. Por último, inserte el cambio con `git push`, como en la primera sección.*</span><span class="sxs-lookup"><span data-stu-id="d9c88-177">*Note: This section applies to Windows only. Linux and macOS users should make the change described in step 2 below. Save the file, and commit the change to the local repository with `git commit`. Finally, push the change with `git push`, as in the first section.*</span></span>

<span data-ttu-id="d9c88-178">La aplicación ya se ha implementado desde el shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="d9c88-178">The app has already been deployed from the command shell.</span></span> <span data-ttu-id="d9c88-179">Vamos a usar herramientas integradas de Visual Studio para implementar una actualización en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d9c88-179">Let's use Visual Studio's integrated tools to deploy an update to the app.</span></span> <span data-ttu-id="d9c88-180">En segundo plano, Visual Studio realiza la misma tarea, como las herramientas de línea de comandos, pero dentro de la interfaz de usuario familiar de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d9c88-180">Behind the scenes, Visual Studio accomplishes the same thing as the command line tooling, but within Visual Studio's familiar UI.</span></span>

1. <span data-ttu-id="d9c88-181">Abra *SimpleFeedReader.sln* en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d9c88-181">Open *SimpleFeedReader.sln* in Visual Studio.</span></span>
2. <span data-ttu-id="d9c88-182">En el Explorador de soluciones, abra *Pages\Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d9c88-182">In Solution Explorer, open *Pages\Index.cshtml*.</span></span> <span data-ttu-id="d9c88-183">Cambio `<h2>Simple Feed Reader</h2>` a `<h2>Simple Feed Reader - V2</h2>`.</span><span class="sxs-lookup"><span data-stu-id="d9c88-183">Change `<h2>Simple Feed Reader</h2>` to `<h2>Simple Feed Reader - V2</h2>`.</span></span>
3. <span data-ttu-id="d9c88-184">Presione **Ctrl**+**MAYÚS**+**B** para compilar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d9c88-184">Press **Ctrl**+**Shift**+**B** to build the app.</span></span>
4. <span data-ttu-id="d9c88-185">En el Explorador de soluciones, haga doble clic en el proyecto y haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="d9c88-185">In Solution Explorer, right-click on the project and click **Publish**.</span></span>

    ![Captura de pantalla con el botón derecho, publicar](./media/deploying-to-app-service/publish.png)
5. <span data-ttu-id="d9c88-187">Visual Studio puede crear un nuevo recurso de App Service, pero esta actualización se publicará a través de la implementación existente.</span><span class="sxs-lookup"><span data-stu-id="d9c88-187">Visual Studio can create a new App Service resource, but this update will be published over the existing deployment.</span></span> <span data-ttu-id="d9c88-188">En el **elegir un destino de publicación** cuadro de diálogo, seleccione **App Service** en la lista de la izquierda y, a continuación, seleccione **seleccionar existente**.</span><span class="sxs-lookup"><span data-stu-id="d9c88-188">In the **Pick a publish target** dialog, select **App Service** from the list on the left, and then select **Select Existing**.</span></span> <span data-ttu-id="d9c88-189">Haga clic en **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="d9c88-189">Click **Publish**.</span></span>
6. <span data-ttu-id="d9c88-190">En el **App Service** cuadro de diálogo, confirme que Microsoft o cuenta profesional que se usa para crear la suscripción de Azure se muestra en la esquina superior derecha.</span><span class="sxs-lookup"><span data-stu-id="d9c88-190">In the **App Service** dialog, confirm that the Microsoft or Organizational account used to create your Azure subscription is displayed in the upper right.</span></span> <span data-ttu-id="d9c88-191">Si no es así, haga clic en la lista desplegable y agréguelo.</span><span class="sxs-lookup"><span data-stu-id="d9c88-191">If it's not, click the drop-down and add it.</span></span>
7. <span data-ttu-id="d9c88-192">Confirme que la Azure correcta **suscripción** está seleccionada.</span><span class="sxs-lookup"><span data-stu-id="d9c88-192">Confirm that the correct Azure **Subscription** is selected.</span></span> <span data-ttu-id="d9c88-193">Para **vista**, seleccione **grupo de recursos**.</span><span class="sxs-lookup"><span data-stu-id="d9c88-193">For **View**, select **Resource Group**.</span></span> <span data-ttu-id="d9c88-194">Expanda el **AzureTutorial** grupo de recursos y, a continuación, seleccione la aplicación web existente.</span><span class="sxs-lookup"><span data-stu-id="d9c88-194">Expand the **AzureTutorial** resource group and then select the existing web app.</span></span> <span data-ttu-id="d9c88-195">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="d9c88-195">Click **OK**.</span></span>

    ![Cuadro de diálogo de publicación de servicio de aplicación que muestra la captura de pantalla](./media/deploying-to-app-service/publish-dialog.png)

<span data-ttu-id="d9c88-197">Visual Studio genera e implementa la aplicación en Azure.</span><span class="sxs-lookup"><span data-stu-id="d9c88-197">Visual Studio builds and deploys the app to Azure.</span></span> <span data-ttu-id="d9c88-198">Vaya a la dirección URL de aplicación web.</span><span class="sxs-lookup"><span data-stu-id="d9c88-198">Browse to the web app URL.</span></span> <span data-ttu-id="d9c88-199">Validar que el `<h2>` modificación del elemento está en funcionamiento.</span><span class="sxs-lookup"><span data-stu-id="d9c88-199">Validate that the `<h2>` element modification is live.</span></span>

![La aplicación con el título modificada](./media/deploying-to-app-service/app-v2.png)

## <a name="deployment-slots"></a><span data-ttu-id="d9c88-201">Ranuras de implementación</span><span class="sxs-lookup"><span data-stu-id="d9c88-201">Deployment slots</span></span>

<span data-ttu-id="d9c88-202">Ranuras de implementación admiten el almacenamiento provisional de los cambios sin afectar a la aplicación que se ejecuta en producción.</span><span class="sxs-lookup"><span data-stu-id="d9c88-202">Deployment slots support the staging of changes without impacting the app running in production.</span></span> <span data-ttu-id="d9c88-203">Una vez que un equipo de control de calidad se valida la versión de ensayo de la aplicación, se pueden intercambiar espacios de ensayo y producción.</span><span class="sxs-lookup"><span data-stu-id="d9c88-203">Once the staged version of the app is validated by a quality assurance team, the production and staging slots can be swapped.</span></span> <span data-ttu-id="d9c88-204">La aplicación en ensayo se promueve a producción de esta manera.</span><span class="sxs-lookup"><span data-stu-id="d9c88-204">The app in staging is promoted to production in this manner.</span></span> <span data-ttu-id="d9c88-205">Los siguientes pasos crean una ranura de ensayo, implementación algunos cambios en él e intercambiar la ranura de ensayo a producción después de la comprobación.</span><span class="sxs-lookup"><span data-stu-id="d9c88-205">The following steps create a staging slot, deploy some changes to it, and swap the staging slot with production after verification.</span></span>

1. <span data-ttu-id="d9c88-206">Inicie sesión en el [Azure Cloud Shell](https://shell.azure.com/bash), si no ha iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="d9c88-206">Sign in to the [Azure Cloud Shell](https://shell.azure.com/bash), if not already signed in.</span></span>
2. <span data-ttu-id="d9c88-207">Creación de la ranura de ensayo.</span><span class="sxs-lookup"><span data-stu-id="d9c88-207">Create the staging slot.</span></span>

    <span data-ttu-id="d9c88-208">a.</span><span class="sxs-lookup"><span data-stu-id="d9c88-208">a.</span></span> <span data-ttu-id="d9c88-209">Cree una ranura de implementación con el nombre *ensayo*.</span><span class="sxs-lookup"><span data-stu-id="d9c88-209">Create a deployment slot with the name *staging*.</span></span>

    ```azure-cli
    az webapp deployment slot create --name $webappname --resource-group AzureTutorial --slot staging
    ```

    <span data-ttu-id="d9c88-210">b.</span><span class="sxs-lookup"><span data-stu-id="d9c88-210">b.</span></span> <span data-ttu-id="d9c88-211">Configurar el espacio de ensayo para usar la implementación de local Git y obtenga el **ensayo** dirección URL de implementación.</span><span class="sxs-lookup"><span data-stu-id="d9c88-211">Configure the staging slot to use deployment from local Git and get the **staging** deployment URL.</span></span> <span data-ttu-id="d9c88-212">**Tenga en cuenta esta dirección URL para consultarla más tarde**.</span><span class="sxs-lookup"><span data-stu-id="d9c88-212">**Note this URL for reference later**.</span></span>

    ```azure-cli
    echo Git deployment URL for staging: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --slot staging --query url --output tsv)
    ```

    <span data-ttu-id="d9c88-213">c.</span><span class="sxs-lookup"><span data-stu-id="d9c88-213">c.</span></span> <span data-ttu-id="d9c88-214">Mostrar la dirección URL de la ranura de ensayo.</span><span class="sxs-lookup"><span data-stu-id="d9c88-214">Display the staging slot's URL.</span></span> <span data-ttu-id="d9c88-215">Vaya a la dirección URL para ver el espacio de ensayo vacío.</span><span class="sxs-lookup"><span data-stu-id="d9c88-215">Browse to the URL to see the empty staging slot.</span></span> <span data-ttu-id="d9c88-216">**Tenga en cuenta esta dirección URL para consultarla más tarde**.</span><span class="sxs-lookup"><span data-stu-id="d9c88-216">**Note this URL for reference later**.</span></span>

    ```console
    echo Staging web app URL: http://$webappname-staging.azurewebsites.net
    ```

3. <span data-ttu-id="d9c88-217">En un editor de texto o Visual Studio, modifique *Pages/index.cshtml* nuevo para que el `<h2>` lee el elemento `<h2>Simple Feed Reader - V3</h2>` y guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="d9c88-217">In a text editor or Visual Studio, modify *Pages/Index.cshtml* again so that the `<h2>` element reads `<h2>Simple Feed Reader - V3</h2>` and save the file.</span></span>

4. <span data-ttu-id="d9c88-218">Confirmar el archivo en el repositorio de Git local, mediante el **cambios** página en Visual Studio *Team Explorer* ficha, o escribiendo lo siguiente mediante el shell de comandos del equipo local:</span><span class="sxs-lookup"><span data-stu-id="d9c88-218">Commit the file to the local Git repository, using either the **Changes** page in Visual Studio's *Team Explorer* tab, or by entering the following using the local machine's command shell:</span></span>

    ```console
    git commit -a -m "upgraded to V3"
    ```
5. <span data-ttu-id="d9c88-219">Mediante el shell de comandos de la máquina local, agregue la URL de la implementación de ensayo como un Git remoto e insertar los cambios confirmados:</span><span class="sxs-lookup"><span data-stu-id="d9c88-219">Using the local machine's command shell, add the staging deployment URL as a Git remote and push the committed changes:</span></span>

    <span data-ttu-id="d9c88-220">a.</span><span class="sxs-lookup"><span data-stu-id="d9c88-220">a.</span></span> <span data-ttu-id="d9c88-221">Agregue la dirección URL remota para el almacenamiento provisional en el repositorio de Git local.</span><span class="sxs-lookup"><span data-stu-id="d9c88-221">Add the remote URL for staging to the local Git repository.</span></span>

    ```console
    git remote add azure-staging <Git_staging_deployment_URL>
    ```

    <span data-ttu-id="d9c88-222">b.</span><span class="sxs-lookup"><span data-stu-id="d9c88-222">b.</span></span> <span data-ttu-id="d9c88-223">Insertar local *maestro* bifurcar a la *ensayo de azure* del remoto *maestro* rama.</span><span class="sxs-lookup"><span data-stu-id="d9c88-223">Push the local *master* branch to the *azure-staging* remote's *master* branch.</span></span>

    ```console
    git push azure-staging master
    ```

    <span data-ttu-id="d9c88-224">Espere mientras Azure crea e implementa la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d9c88-224">Wait while Azure builds and deploys the app.</span></span>

6. <span data-ttu-id="d9c88-225">Para comprobar que se ha implementado V3 en la ranura de ensayo, abra dos ventanas del explorador.</span><span class="sxs-lookup"><span data-stu-id="d9c88-225">To verify that V3 has been deployed to the staging slot, open two browser windows.</span></span> <span data-ttu-id="d9c88-226">En una ventana, desplácese a la URL de aplicación web original.</span><span class="sxs-lookup"><span data-stu-id="d9c88-226">In one window, navigate to the original web app URL.</span></span> <span data-ttu-id="d9c88-227">En la ventana, vaya a la URL de aplicación web provisional.</span><span class="sxs-lookup"><span data-stu-id="d9c88-227">In the other window, navigate to the staging web app URL.</span></span> <span data-ttu-id="d9c88-228">La dirección URL de producción actúa V2 de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d9c88-228">The production URL serves V2 of the app.</span></span> <span data-ttu-id="d9c88-229">La URL de ensayo sirve V3 de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d9c88-229">The staging URL serves V3 of the app.</span></span>

    ![Captura de pantalla de comparación de las ventanas del explorador](./media/deploying-to-app-service/ready-to-swap.png)

7. <span data-ttu-id="d9c88-231">En Cloud Shell, colocar la ranura de ensayo comprobado/preparado-up en producción.</span><span class="sxs-lookup"><span data-stu-id="d9c88-231">In the Cloud Shell, swap the verified/warmed-up staging slot into production.</span></span>

    ```azure-cli
    az webapp deployment slot swap --name $webappname --resource-group AzureTutorial --slot staging
    ```

8. <span data-ttu-id="d9c88-232">Compruebe que el intercambio se produjo al actualizar las ventanas del explorador de dos.</span><span class="sxs-lookup"><span data-stu-id="d9c88-232">Verify that the swap occurred by refreshing the two browser windows.</span></span>

    ![Comparación de las ventanas del explorador después del intercambio](./media/deploying-to-app-service/swapped.png)

## <a name="summary"></a><span data-ttu-id="d9c88-234">Resumen</span><span class="sxs-lookup"><span data-stu-id="d9c88-234">Summary</span></span>

<span data-ttu-id="d9c88-235">En esta sección, se completaron las siguientes tareas:</span><span class="sxs-lookup"><span data-stu-id="d9c88-235">In this section, the following tasks were completed:</span></span>

* <span data-ttu-id="d9c88-236">Descargado y compilado la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="d9c88-236">Downloaded and built the sample app.</span></span>
* <span data-ttu-id="d9c88-237">Crea una aplicación Web de Azure App Service mediante Azure Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="d9c88-237">Created an Azure App Service Web App using the Azure Cloud Shell.</span></span>
* <span data-ttu-id="d9c88-238">Implementar la aplicación de ejemplo en Azure mediante Git.</span><span class="sxs-lookup"><span data-stu-id="d9c88-238">Deployed the sample app to Azure using Git.</span></span>
* <span data-ttu-id="d9c88-239">Implementar un cambio en la aplicación mediante Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d9c88-239">Deployed a change to the app using Visual Studio.</span></span>
* <span data-ttu-id="d9c88-240">Agrega una ranura de ensayo a la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="d9c88-240">Added a staging slot to the web app.</span></span>
* <span data-ttu-id="d9c88-241">Implementar una actualización en la ranura de ensayo.</span><span class="sxs-lookup"><span data-stu-id="d9c88-241">Deployed an update to the staging slot.</span></span>
* <span data-ttu-id="d9c88-242">Intercambiar las ranuras de ensayo y producción.</span><span class="sxs-lookup"><span data-stu-id="d9c88-242">Swapped the staging and production slots.</span></span>

<span data-ttu-id="d9c88-243">En la sección siguiente, obtendrá información sobre cómo crear una canalización de DevOps con canalizaciones de Azure.</span><span class="sxs-lookup"><span data-stu-id="d9c88-243">In the next section, you'll learn how to build a DevOps pipeline with Azure Pipelines.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="d9c88-244">Lecturas adicionales</span><span class="sxs-lookup"><span data-stu-id="d9c88-244">Additional reading</span></span>

* [<span data-ttu-id="d9c88-245">Introducción a Web Apps</span><span class="sxs-lookup"><span data-stu-id="d9c88-245">Web Apps overview</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="d9c88-246">Compilar una aplicación web de .NET Core y SQL Database en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="d9c88-246">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* [<span data-ttu-id="d9c88-247">Configurar credenciales de implementación para Azure App Service</span><span class="sxs-lookup"><span data-stu-id="d9c88-247">Configure deployment credentials for Azure App Service</span></span>](/azure/app-service/app-service-deployment-credentials)
* [<span data-ttu-id="d9c88-248">Configurar entornos de ensayo en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="d9c88-248">Set up staging environments in Azure App Service</span></span>](/azure/app-service/web-sites-staged-publishing)
