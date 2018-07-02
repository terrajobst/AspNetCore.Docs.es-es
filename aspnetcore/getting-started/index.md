---
title: Introducción a ASP.NET Core
author: rick-anderson
description: Tutorial rápido que crea y ejecuta una aplicación Hola mundo sencilla mediante ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: eb049dea2800cf2e12c044b88d1664ee80bb95a5
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296772"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="abe02-103">Introducción a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="abe02-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="abe02-104">Instale el [!INCLUDE[](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="abe02-104">Install the [!INCLUDE[](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="abe02-105">Cree un proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="abe02-105">Create an ASP.NET Core project.</span></span> <span data-ttu-id="abe02-106">Abra un shell de comandos y escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="abe02-106">Open a command shell and enter the following command:</span></span>

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

3. <span data-ttu-id="abe02-108">Confíe en el certificado de desarrollo HTTPS:</span><span class="sxs-lookup"><span data-stu-id="abe02-108">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="abe02-109">Windows</span><span class="sxs-lookup"><span data-stu-id="abe02-109">Windows</span></span>](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following dialog:

    ![Security warning dialog](_static/cert.png)

    Select **Yes** if you agree to trust the development certificate.

# <a name="macostabmacos"></a>[<span data-ttu-id="abe02-110">macOS</span><span class="sxs-lookup"><span data-stu-id="abe02-110">macOS</span></span>](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following message:

    *Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:*
    `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`
    *This command might prompt you for your password to install the certificate on the system keychain.
    Password:*

    Enter your password if you agree to trust the development certificate.

# <a name="linuxtablinux"></a>[<span data-ttu-id="abe02-111">Linux</span><span class="sxs-lookup"><span data-stu-id="abe02-111">Linux</span></span>](#tab/linux)

    See the documentation for your Linux distribution on how to trust the HTTPS development certificate
---

4. <span data-ttu-id="abe02-112">Ejecute la aplicación:</span><span class="sxs-lookup"><span data-stu-id="abe02-112">Run the app:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="abe02-113">Vaya a [http://localhost:5001](http://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="abe02-113">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="abe02-114">Haga clic en **Aceptar** para aceptar la política de privacidad y de cookies.</span><span class="sxs-lookup"><span data-stu-id="abe02-114">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="abe02-115">Esta aplicación no conserva información de carácter personal.</span><span class="sxs-lookup"><span data-stu-id="abe02-115">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="abe02-116">Abra *Pages/About.cshtml* y modifique la página con el siguiente marcado resaltado:</span><span class="sxs-lookup"><span data-stu-id="abe02-116">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

    <span data-ttu-id="abe02-117">[!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]</span><span class="sxs-lookup"><span data-stu-id="abe02-117">[!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]</span></span>

7. <span data-ttu-id="abe02-118">Vaya a [http://localhost:5001/About](http://localhost:5001/About) y confirme que los cambios aparecen reflejados.</span><span class="sxs-lookup"><span data-stu-id="abe02-118">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="abe02-119">Instale el [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="abe02-119">Install the [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="abe02-120">Cree un proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="abe02-120">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="abe02-121">Abra un shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="abe02-121">Open a command shell.</span></span> <span data-ttu-id="abe02-122">Escriba el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="abe02-122">Enter the following command:</span></span>

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="abe02-123">Ejecute la aplicación con los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="abe02-123">Run the app with the following commands:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="abe02-124">Vaya a [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="abe02-124">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="abe02-125">Abra *Pages/About.cshtml* y modifique la página para que muestre el mensaje "¡Hola, mundo!".</span><span class="sxs-lookup"><span data-stu-id="abe02-125">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="abe02-126">La hora del servidor es @DateTime.Now" :</span><span class="sxs-lookup"><span data-stu-id="abe02-126">The time on the server is @DateTime.Now":</span></span>

    <span data-ttu-id="abe02-127">[!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="abe02-127">[!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span></span>

6. <span data-ttu-id="abe02-128">Vaya a [http://localhost:5000/About](http://localhost:5000/About) y compruebe los cambios.</span><span class="sxs-lookup"><span data-stu-id="abe02-128">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="abe02-129">Instale el **instalador del SDK** de .NET Core para SDK 1.0.4 desde la [página de descargas de .NET Core](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="abe02-129">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="abe02-130">Cree una carpeta para un nuevo proyecto de ASP .NET Core.</span><span class="sxs-lookup"><span data-stu-id="abe02-130">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="abe02-131">Abra un shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="abe02-131">Open a command shell.</span></span> <span data-ttu-id="abe02-132">Escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="abe02-132">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="abe02-133">Si ha instalado una versión posterior del SDK en el equipo, cree un archivo *global.json* para seleccionar la versión 1.0.4 del SDK.</span><span class="sxs-lookup"><span data-stu-id="abe02-133">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="abe02-134">Cree un proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="abe02-134">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="abe02-135">Restaure los paquetes.</span><span class="sxs-lookup"><span data-stu-id="abe02-135">Restore the packages.</span></span>

    ```console
    dotnet restore
    ```

6. <span data-ttu-id="abe02-136">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="abe02-136">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="abe02-137">El comando [dotnet run](/dotnet/core/tools/dotnet-run) compila primero la aplicación en caso de que sea necesario.</span><span class="sxs-lookup"><span data-stu-id="abe02-137">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="abe02-138">Vaya a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="abe02-138">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
