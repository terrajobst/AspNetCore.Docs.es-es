---
title: Publicación de una aplicación de ASP.NET Core en Azure con Visual Studio Code
author: ricardoserradas
description: Obtenga información sobre cómo publicar una aplicación de ASP.NET Core en Azure App Service con Visual Studio Code.
ms.author: ricardoserradas
ms.custom: mvc
ms.date: 04/16/2019
uid: tutorials/publish-to-azure-webapp-using-vscode
ms.openlocfilehash: 64d82835f6a47a458802692c99658b964c07f807
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59711318"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-visual-studio-code"></a><span data-ttu-id="2532f-103">Publicación de una aplicación de ASP.NET Core en Azure con Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2532f-103">Publish an ASP.NET Core app to Azure with Visual Studio Code</span></span>

<span data-ttu-id="2532f-104">Autor: [Ricardo Serradas](https://twitter.com/ricardoserradas)</span><span class="sxs-lookup"><span data-stu-id="2532f-104">By [Ricardo Serradas](https://twitter.com/ricardoserradas)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="2532f-105">Para solucionar un problema de implementación de App Service, vea <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="2532f-105">To troubleshoot an App Service deployment issue, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>

## <a name="intro"></a><span data-ttu-id="2532f-106">Introducción</span><span class="sxs-lookup"><span data-stu-id="2532f-106">Intro</span></span>

<span data-ttu-id="2532f-107">En este tutorial aprenderá a crear una aplicación MVC ASP.Net Core y a implementarla en Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="2532f-107">With this tutorial, you'll learn how to create an ASP.Net Core MVC Application and deploy it within Visual Studio Code.</span></span>

## <a name="set-up"></a><span data-ttu-id="2532f-108">Configurar</span><span class="sxs-lookup"><span data-stu-id="2532f-108">Set up</span></span>

- <span data-ttu-id="2532f-109">Abra una [cuenta gratuita de Azure](https://azure.microsoft.com/free/dotnet/) si no tiene una.</span><span class="sxs-lookup"><span data-stu-id="2532f-109">Open a [free Azure account](https://azure.microsoft.com/free/dotnet/) if you don't have one.</span></span>
- <span data-ttu-id="2532f-110">Instale el [SDK de .NET Core](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="2532f-110">Install [.NET Core SDK](https://dotnet.microsoft.com/download)</span></span>
- <span data-ttu-id="2532f-111">Instale [Visual Studio Code](https://code.visualstudio.com/Download).</span><span class="sxs-lookup"><span data-stu-id="2532f-111">Install [Visual Studio Code](https://code.visualstudio.com/Download)</span></span>
  - <span data-ttu-id="2532f-112">Instale la [extensión de C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) en Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="2532f-112">Install the [C# Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) to Visual Studio Code</span></span>
  - <span data-ttu-id="2532f-113">Instale la [extensión de Azure App Service](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) en Visual Studio Code y configúrela antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="2532f-113">Instal the [Azure App Service Extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) to Visual Studio Code and configure it before proceeding</span></span>

## <a name="create-an-aspnet-core-mvc-project"></a><span data-ttu-id="2532f-114">Creación de un proyecto MVC ASP.Net Core</span><span class="sxs-lookup"><span data-stu-id="2532f-114">Create an ASP.Net Core MVC project</span></span>

<span data-ttu-id="2532f-115">Mediante un terminal, navegue a la carpeta donde quiera crear el proyecto y use el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="2532f-115">Using a terminal, navigate to the folder you want the project to be created on and use the following command:</span></span>

```cmd
> dotnet new mvc
```

<span data-ttu-id="2532f-116">Tendrá una estructura de carpetas similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="2532f-116">You'll have a folder structure similar to the following:</span></span>

```cmd
      appsettings.Development.json
      appsettings.json
<DIR> Controllers
<DIR> Models
      netcore-vscode.csproj
<DIR> obj
      Program.cs
<DIR> Properties
      Startup.cs
<DIR> Views
<DIR> wwwroot
```

## <a name="open-it-with-visual-studio-code"></a><span data-ttu-id="2532f-117">Apertura con Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2532f-117">Open it with Visual Studio Code</span></span>

<span data-ttu-id="2532f-118">Una vez que haya creado el proyecto, podrá abrirlo con Visual Studio Code con una de las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="2532f-118">After your project is created, you can open it with Visual Studio Code by using one of the options below:</span></span>

### <a name="through-the-command-line"></a><span data-ttu-id="2532f-119">Mediante la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="2532f-119">Through the command line</span></span>

<span data-ttu-id="2532f-120">Use el comando siguiente en la carpeta en la que ha creado el proyecto:</span><span class="sxs-lookup"><span data-stu-id="2532f-120">Use the following command within the folder you created the project:</span></span>

```cmd
> code .
```

<span data-ttu-id="2532f-121">Si el comando siguiente no funciona, compruebe si la instalación está configurada adecuadamente de acuerdo al contenido de [este vínculo](https://code.visualstudio.com/docs/setup/setup-overview#_cross-platform).</span><span class="sxs-lookup"><span data-stu-id="2532f-121">If the command below does not work, check if your installation is configured properly by referencing [this link](https://code.visualstudio.com/docs/setup/setup-overview#_cross-platform).</span></span>

### <a name="through-visual-studio-code-interface"></a><span data-ttu-id="2532f-122">Mediante la interfaz de Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2532f-122">Through Visual Studio Code interface</span></span>

- <span data-ttu-id="2532f-123">Abra Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="2532f-123">Open Visual Studio Code</span></span>
- <span data-ttu-id="2532f-124">En el menú, seleccione `File > Open Folder`.</span><span class="sxs-lookup"><span data-stu-id="2532f-124">On the menu, select `File > Open Folder`</span></span>
- <span data-ttu-id="2532f-125">Seleccione la raíz de la carpeta en la que ha creado el proyecto MVC.</span><span class="sxs-lookup"><span data-stu-id="2532f-125">Select the root of the folder you created the MVC Project</span></span>

<span data-ttu-id="2532f-126">Al abrir la carpeta del proyecto, recibirá un mensaje en el que se indica que faltan los recursos requeridos para realizar la compilación y la depuración.</span><span class="sxs-lookup"><span data-stu-id="2532f-126">When you open the project folder, you'll receive a message saying that required assets to build and debug are missing.</span></span> <span data-ttu-id="2532f-127">Acepte la ayuda para agregarlos.</span><span class="sxs-lookup"><span data-stu-id="2532f-127">Accept the help to add them.</span></span>

![Interfaz de Visual Studio Code con un proyecto cargado](publish-to-azure-webapp-using-vscode/_static/folder-structure-restore-netcore.jpg)

<span data-ttu-id="2532f-129">Se creará una carpeta `.vscode` en la estructura del proyecto.</span><span class="sxs-lookup"><span data-stu-id="2532f-129">A `.vscode` folder will be created under the project structure.</span></span> <span data-ttu-id="2532f-130">Contendrá los siguientes archivos:</span><span class="sxs-lookup"><span data-stu-id="2532f-130">It will contain the following files:</span></span>

```cmd
launch.json
tasks.json
```

<span data-ttu-id="2532f-131">Estos son archivos de utilidad que le ayudarán a compilar y depurar su aplicación web .NET Core.</span><span class="sxs-lookup"><span data-stu-id="2532f-131">These are utility files to help you build and debug your .NET Core Web App.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="2532f-132">Ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="2532f-132">Run the app</span></span>

<span data-ttu-id="2532f-133">Antes de implementar la aplicación en Azure, asegúrese de que se ejecuta correctamente en la máquina local.</span><span class="sxs-lookup"><span data-stu-id="2532f-133">Before we deploy the app to Azure, make sure it is running properly on your local machine.</span></span>

- <span data-ttu-id="2532f-134">Presione F5 para ejecutar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="2532f-134">Press F5 to run the project</span></span>

<span data-ttu-id="2532f-135">La aplicación web se ejecutará en una pestaña nueva del explorador predeterminado.</span><span class="sxs-lookup"><span data-stu-id="2532f-135">Your web app will start running on a new tab of your default browser.</span></span> <span data-ttu-id="2532f-136">Es posible que se muestre una advertencia de privacidad en cuanto se inicie.</span><span class="sxs-lookup"><span data-stu-id="2532f-136">You may notice a privacy warning as soon as it starts.</span></span> <span data-ttu-id="2532f-137">Esto se debe a que la aplicación se iniciará con HTTP o HTTPS y navegará al punto de conexión HTTPS de manera predeterminada.</span><span class="sxs-lookup"><span data-stu-id="2532f-137">This is because your app will start either using HTTP and HTTPS, and it navigates to the HTTPS endpoint by default.</span></span>

![Advertencia de privacidad al depurar la aplicación localmente](publish-to-azure-webapp-using-vscode/_static/run-webapp-https-warning.jpg)

<span data-ttu-id="2532f-139">Para mantener la sesión de depuración, haga clic en `Advanced` y luego en `Continue to localhost (unsafe)`.</span><span class="sxs-lookup"><span data-stu-id="2532f-139">To keep the debugging session, click `Advanced` and then `Continue to localhost (unsafe)`.</span></span>

## <a name="generate-the-deployment-package-locally"></a><span data-ttu-id="2532f-140">Generación del paquete de implementación localmente</span><span class="sxs-lookup"><span data-stu-id="2532f-140">Generate the deployment package locally</span></span>

- <span data-ttu-id="2532f-141">Abra un terminal de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="2532f-141">Open Visual Studio Code terminal</span></span>
- <span data-ttu-id="2532f-142">Use el comando siguiente para generar un paquete `Release` en una subcarpeta denominada `publish`:</span><span class="sxs-lookup"><span data-stu-id="2532f-142">Use the following command to generate a `Release` package to a sub folder called `publish`:</span></span>
  - `dotnet publish -c Release -o ./publish`
- <span data-ttu-id="2532f-143">Se creará una nueva carpeta `publish` en la estructura del proyecto.</span><span class="sxs-lookup"><span data-stu-id="2532f-143">A new `publish` folder will be created under the project structure</span></span>

![Estructura de carpetas de publicación](publish-to-azure-webapp-using-vscode/_static/publish-folder.jpg)

## <a name="publish-to-azure-app-service"></a><span data-ttu-id="2532f-145">Publicación en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="2532f-145">Publish to Azure App Service</span></span>

<span data-ttu-id="2532f-146">Aproveche la extensión de Azure App Service para Visual Studio Code y siga los pasos que se indican a continuación para publicar el sitio web directamente en Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="2532f-146">Leveraging the Azure App Service extension for Visual Studio Code, follow the steps below to publish the website directly to the Azure App Service.</span></span>

### <a name="if-youre-creating-a-new-web-app"></a><span data-ttu-id="2532f-147">Si está creando una aplicación web</span><span class="sxs-lookup"><span data-stu-id="2532f-147">If you're creating a new Web App</span></span>

- <span data-ttu-id="2532f-148">Haga clic con el botón derecho en la carpeta `publish` y seleccione `Deploy to Web App...`.</span><span class="sxs-lookup"><span data-stu-id="2532f-148">Right click the `publish` folder and select `Deploy to Web App...`</span></span>
- <span data-ttu-id="2532f-149">Seleccione la suscripción que quiera crear en la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="2532f-149">Select the subscription you want to create the Web App</span></span>
- <span data-ttu-id="2532f-150">Seleccione `Create New Web App`.</span><span class="sxs-lookup"><span data-stu-id="2532f-150">Select `Create New Web App`</span></span>
- <span data-ttu-id="2532f-151">Escriba un nombre para la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="2532f-151">Enter a name for the Web App</span></span>

<span data-ttu-id="2532f-152">La extensión creará la nueva aplicación web e iniciará automáticamente la implementación del paquete.</span><span class="sxs-lookup"><span data-stu-id="2532f-152">The extension will create the new Web App and will automatically start deploying the package to it.</span></span> <span data-ttu-id="2532f-153">Una vez que se haya terminado la implementación, haga clic en `Browse Website` para validar la implementación.</span><span class="sxs-lookup"><span data-stu-id="2532f-153">Once the deployment is finished, click `Browse Website` to validate the deployment.</span></span>

![Mensaje en el que se indica que la implementación se ha realizado correctamente](publish-to-azure-webapp-using-vscode/_static/deployment-succeeded-message.jpg)

<span data-ttu-id="2532f-155">Al hacer clic en `Browse Website`, navegará ahí con su explorador predeterminado:</span><span class="sxs-lookup"><span data-stu-id="2532f-155">Once you click `Browse Website`, you'll navigate to it using your default browser:</span></span>

![Nueva aplicación web implementada correctamente](publish-to-azure-webapp-using-vscode/_static/new-webapp-deployed.jpg)

### <a name="if-youre-deploying-to-an-existing-web-app"></a><span data-ttu-id="2532f-157">Si va a realizar la implementación en una aplicación web existente</span><span class="sxs-lookup"><span data-stu-id="2532f-157">If you're deploying to an existing Web App</span></span>

- <span data-ttu-id="2532f-158">Haga clic con el botón derecho en la carpeta `publish` y seleccione `Deploy to Web App...`.</span><span class="sxs-lookup"><span data-stu-id="2532f-158">Right click the `publish` folder and select `Deploy to Web App...`</span></span>
- <span data-ttu-id="2532f-159">Seleccione la suscripción en la que reside la aplicación web existente.</span><span class="sxs-lookup"><span data-stu-id="2532f-159">Select the subscription the existing Web App resides</span></span>
- <span data-ttu-id="2532f-160">Seleccione la aplicación web de la lista.</span><span class="sxs-lookup"><span data-stu-id="2532f-160">Select the Web App from the list</span></span>
- <span data-ttu-id="2532f-161">Visual Studio Code le preguntará si quiere sobrescribir el contenido existente.</span><span class="sxs-lookup"><span data-stu-id="2532f-161">Visual Studio Code will ask you if you want to overwrite the existing content.</span></span> <span data-ttu-id="2532f-162">Haga clic en `Deploy` para confirmar.</span><span class="sxs-lookup"><span data-stu-id="2532f-162">Click `Deploy` to confirm</span></span>

<span data-ttu-id="2532f-163">La extensión implementará el contenido actualizado en la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="2532f-163">The extension will deploy the updated content to the Web App.</span></span> <span data-ttu-id="2532f-164">Cuando termine, haga clic en `Browse Website` para validar la implementación.</span><span class="sxs-lookup"><span data-stu-id="2532f-164">Once it's done, click `Browse Website` to validate the deployment.</span></span>

![Aplicación web existente implementada correctamente.](publish-to-azure-webapp-using-vscode/_static/existing-webapp-deployed.jpg)

## <a name="next-steps"></a><span data-ttu-id="2532f-166">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="2532f-166">Next steps</span></span>

- <span data-ttu-id="2532f-167">[Create your first Azure DevOps pipeline](/azure/devops/pipelines/create-first-pipeline) (Creación de su primera canalización de Azure DevOps)</span><span class="sxs-lookup"><span data-stu-id="2532f-167">[Create your first Azure DevOps pipeline](/azure/devops/pipelines/create-first-pipeline)</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2532f-168">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="2532f-168">Additional resources</span></span>

- [<span data-ttu-id="2532f-169">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="2532f-169">Azure App Service</span></span>](/azure/app-service/app-service-web-overview)
- [<span data-ttu-id="2532f-170">Grupos de recursos de Azure</span><span class="sxs-lookup"><span data-stu-id="2532f-170">Azure resource groups</span></span>](/azure/azure-resource-manager/resource-group-overview#resource-groups)