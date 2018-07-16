---
title: Publicar una aplicación ASP.NET Core en Azure con las herramientas de línea de comandos
author: camsoper
description: Obtenga información sobre cómo publicar una aplicación de ASP.NET Core en Azure App Service con el cliente de línea de comandos de Git.
ms.author: casoper
ms.custom: mvc
ms.date: 11/03/2017
services: multiple
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 526ceef469d473706f39cdc3ee645280e99315b1
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2018
ms.locfileid: "38126965"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-command-line-tools"></a><span data-ttu-id="4ce37-103">Publicar una aplicación ASP.NET Core en Azure con las herramientas de línea de comandos</span><span class="sxs-lookup"><span data-stu-id="4ce37-103">Publish an ASP.NET Core app to Azure with command line tools</span></span>

<span data-ttu-id="4ce37-104">Por [Cam Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="4ce37-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="4ce37-105">En este tutorial se explica cómo compilar e implementar una aplicación de ASP.NET Core en Microsoft Azure App Service con las herramientas de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="4ce37-105">This tutorial will show you how to build and deploy an ASP.NET Core app to Microsoft Azure App Service using command line tools.</span></span> <span data-ttu-id="4ce37-106">Cuando termine, tendrá una aplicación web de Razor Pages integrada en ASP.NET Core y hospedada como una aplicación web de Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="4ce37-106">When finished, you'll have a Razor Pages web app built in ASP.NET Core hosted as an Azure App Service Web App.</span></span> <span data-ttu-id="4ce37-107">Este tutorial se escribe con herramientas de línea de comandos de Windows, pero también puede aplicarse a entornos de macOS y Linux.</span><span class="sxs-lookup"><span data-stu-id="4ce37-107">This tutorial is written using Windows command line tools, but can be applied to macOS and Linux environments, as well.</span></span>

<span data-ttu-id="4ce37-108">En este tutorial aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="4ce37-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4ce37-109">Crear un sitio web en Azure App Service con la CLI de Azure</span><span class="sxs-lookup"><span data-stu-id="4ce37-109">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="4ce37-110">Implementación de una aplicación de ASP.NET Core en Azure App Service con la herramienta de línea de comandos de Git</span><span class="sxs-lookup"><span data-stu-id="4ce37-110">Deploy an ASP.NET Core app to Azure App Service using the Git command line tool</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4ce37-111">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="4ce37-111">Prerequisites</span></span>

<span data-ttu-id="4ce37-112">Para completar este tutorial, necesita:</span><span class="sxs-lookup"><span data-stu-id="4ce37-112">To complete this tutorial, you'll need:</span></span>

* <span data-ttu-id="4ce37-113">Una [suscripción de Microsoft Azure](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="4ce37-113">A [Microsoft Azure subscription](https://azure.microsoft.com/free/)</span></span>
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="4ce37-114">Un cliente de línea de comandos de [Git](https://www.git-scm.com/)</span><span class="sxs-lookup"><span data-stu-id="4ce37-114">[Git](https://www.git-scm.com/) command line client</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="4ce37-115">Creación de una aplicación web</span><span class="sxs-lookup"><span data-stu-id="4ce37-115">Create a web app</span></span>

<span data-ttu-id="4ce37-116">Cree un directorio para la aplicación web, cree una aplicación de Razor Pages de ASP.NET Core y, luego, ejecute el sitio web de forma local.</span><span class="sxs-lookup"><span data-stu-id="4ce37-116">Create a new directory for the web app, create a new ASP.NET Core Razor Pages app, and then run the website locally.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="4ce37-117">Windows</span><span class="sxs-lookup"><span data-stu-id="4ce37-117">Windows</span></span>](#tab/windows)

::: moniker range=">= aspnetcore-2.1"

```console
REM Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the app
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
REM Create a new ASP.NET Core Razor Pages app
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the app
dotnet run
```

::: moniker-end

# <a name="othertabother"></a>[<span data-ttu-id="4ce37-118">Otros problemas</span><span class="sxs-lookup"><span data-stu-id="4ce37-118">Other</span></span>](#tab/other)

::: moniker range=">= aspnetcore-2.1"

```bash
# Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the app
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```bash
# Create a new ASP.NET Core Razor Pages app
dotnet new razor -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the app
dotnet run
```

::: moniker-end

---

![Salida de la línea de comandos](publish-to-azure-webapp-using-cli/_static/new_prj.png)

<span data-ttu-id="4ce37-120">Navegue a `http://localhost:5000` para probar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4ce37-120">Test the app by browsing to `http://localhost:5000`.</span></span>

![Sitio web en ejecución en el entorno local](publish-to-azure-webapp-using-cli/_static/app_test.png)

## <a name="create-the-azure-app-service-instance"></a><span data-ttu-id="4ce37-122">Creación de la instancia de Azure App Service</span><span class="sxs-lookup"><span data-stu-id="4ce37-122">Create the Azure App Service instance</span></span>

<span data-ttu-id="4ce37-123">Con [Azure Cloud Shell](/azure/cloud-shell/quickstart), cree un grupo de recursos, un plan de App Service y una aplicación web de App Service.</span><span class="sxs-lookup"><span data-stu-id="4ce37-123">Using the [Azure Cloud Shell](/azure/cloud-shell/quickstart), create a resource group, App Service plan, and an App Service web app.</span></span>

```azurecli-interactive
# Generate a unique Web App name
let randomNum=$RANDOM*$RANDOM
webappname=tutorialApp$randomNum

# Create the DotNetAzureTutorial resource group
az group create --name DotNetAzureTutorial --location EastUS

# Create an App Service plan.
az appservice plan create --name $webappname --resource-group DotNetAzureTutorial --sku FREE

# Create the Web App
az webapp create --name $webappname --resource-group DotNetAzureTutorial --plan $webappname
```

<span data-ttu-id="4ce37-124">Antes de la implementación, defina las credenciales de implementación a nivel de cuenta con el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="4ce37-124">Before deployment, set the account-level deployment credentials using the following command:</span></span>

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

<span data-ttu-id="4ce37-125">Se necesita una dirección URL de implementación para implementar la aplicación con Git.</span><span class="sxs-lookup"><span data-stu-id="4ce37-125">A deployment URL is needed to deploy the app using Git.</span></span> <span data-ttu-id="4ce37-126">Recupere una dirección URL como esta.</span><span class="sxs-lookup"><span data-stu-id="4ce37-126">Retrieve the URL like this.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```

<span data-ttu-id="4ce37-127">Anote la dirección URL mostrada que termina en `.git`.</span><span class="sxs-lookup"><span data-stu-id="4ce37-127">Note the displayed URL ending in `.git`.</span></span> <span data-ttu-id="4ce37-128">Se utiliza en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="4ce37-128">It's used in the next step.</span></span>

## <a name="deploy-the-app-using-git"></a><span data-ttu-id="4ce37-129">Implementación de la aplicación con Git</span><span class="sxs-lookup"><span data-stu-id="4ce37-129">Deploy the app using Git</span></span>

<span data-ttu-id="4ce37-130">Está listo para implementar desde el equipo local mediante Git.</span><span class="sxs-lookup"><span data-stu-id="4ce37-130">You're ready to deploy from your local machine using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="4ce37-131">Es seguro pasar por alto las advertencias de Git sobre los finales de línea.</span><span class="sxs-lookup"><span data-stu-id="4ce37-131">It's safe to ignore any warnings from Git about line endings.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="4ce37-132">Windows</span><span class="sxs-lookup"><span data-stu-id="4ce37-132">Windows</span></span>](#tab/windows)

```cmd
REM Initialize the local Git repository
git init

REM Add the contents of the working directory to the repo
git add --all

REM Commit the changes to the local repo
git commit -a -m "Initial commit"

REM Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

REM Push the local repository to the remote
git push azure master
```

# <a name="othertabother"></a>[<span data-ttu-id="4ce37-133">Otros problemas</span><span class="sxs-lookup"><span data-stu-id="4ce37-133">Other</span></span>](#tab/other)

```bash
# Initialize the local Git repository
git init

# Add the contents of the working directory to the repo
git add --all

# Commit the changes to the local repo
git commit -a -m "Initial commit"

# Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

# Push the local repository to the remote
git push azure master
```

---

<span data-ttu-id="4ce37-134">Git solicitará las credenciales de implementación establecidas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="4ce37-134">Git prompts for the deployment credentials that were set earlier.</span></span> <span data-ttu-id="4ce37-135">Tras la autenticación, la aplicación se inserta en la ubicación remota, se compila y se implementa.</span><span class="sxs-lookup"><span data-stu-id="4ce37-135">After authenticating, the app will be pushed to the remote location, built, and deployed.</span></span>

![Resultado de la implementación de Git](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-app"></a><span data-ttu-id="4ce37-137">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="4ce37-137">Test the app</span></span>

<span data-ttu-id="4ce37-138">Navegue a `https://<web app name>.azurewebsites.net` para probar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4ce37-138">Test the app by browsing to `https://<web app name>.azurewebsites.net`.</span></span> <span data-ttu-id="4ce37-139">Para mostrar la dirección en Cloud Shell o en la CLI de Azure, se usa lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="4ce37-139">To display the address in the Cloud Shell (or Azure CLI), use the following:</span></span>

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![La aplicación en ejecución en Azure](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a><span data-ttu-id="4ce37-141">Limpieza</span><span class="sxs-lookup"><span data-stu-id="4ce37-141">Clean up</span></span>

<span data-ttu-id="4ce37-142">Cuando termine de probar la aplicación y de inspeccionar el código y los recursos, elimine la aplicación web y el plan con la eliminación del grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="4ce37-142">When finished testing the app and inspecting the code and resources, delete the web app and plan by deleting the resource group.</span></span>

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a><span data-ttu-id="4ce37-143">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="4ce37-143">Next steps</span></span>

<span data-ttu-id="4ce37-144">En este tutorial ha aprendido a:</span><span class="sxs-lookup"><span data-stu-id="4ce37-144">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4ce37-145">Crear un sitio web en Azure App Service con la CLI de Azure</span><span class="sxs-lookup"><span data-stu-id="4ce37-145">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="4ce37-146">Implementación de una aplicación de ASP.NET Core en Azure App Service con la herramienta de línea de comandos de Git</span><span class="sxs-lookup"><span data-stu-id="4ce37-146">Deploy an ASP.NET Core app to Azure App Service using the Git command line tool</span></span>

<span data-ttu-id="4ce37-147">A continuación, aprenda a usar la línea de comandos para implementar una aplicación web existente que usa Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="4ce37-147">Next, learn to use the command line to deploy an existing web app that uses CosmosDB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="4ce37-148">Implementación en Azure desde la línea de comandos con .NET Core</span><span class="sxs-lookup"><span data-stu-id="4ce37-148">Deploy to Azure from the command line with .NET Core</span></span>](/dotnet/azure/dotnet-quickstart-xplat)
