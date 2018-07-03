---
title: Introducción a ASP.NET Core
author: rick-anderson
description: Tutorial rápido que crea y ejecuta una aplicación Hola mundo sencilla mediante ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 22e9c982921cc03d89506e18ff99bf481027dda6
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077665"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="6f28b-103">Introducción a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6f28b-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="6f28b-104">Instale el [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="6f28b-104">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="6f28b-105">Cree un proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6f28b-105">Create an ASP.NET Core project.</span></span> <span data-ttu-id="6f28b-106">Abra un shell de comandos y escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="6f28b-106">Open a command shell and enter the following command:</span></span>

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    <span data-ttu-id="6f28b-107">[!INCLUDE [](~/includes/webapp-alias-notice.md) [](~/includes/webapp-alias-notice.md)]</span><span class="sxs-lookup"><span data-stu-id="6f28b-107">[!INCLUDE [](~/includes/webapp-alias-notice.md) [](~/includes/webapp-alias-notice.md)]</span></span>

3. <span data-ttu-id="6f28b-108">Confíe en el certificado de desarrollo HTTPS:</span><span class="sxs-lookup"><span data-stu-id="6f28b-108">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="6f28b-109">Windows</span><span class="sxs-lookup"><span data-stu-id="6f28b-109">Windows</span></span>](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following dialog:

    ![Security warning dialog](_static/cert.png)

    Select **Yes** if you agree to trust the development certificate.

# <a name="macostabmacos"></a>[<span data-ttu-id="6f28b-110">macOS</span><span class="sxs-lookup"><span data-stu-id="6f28b-110">macOS</span></span>](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following message:

    *Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:*
    `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`
    *This command might prompt you for your password to install the certificate on the system keychain.
    Password:*

    Enter your password if you agree to trust the development certificate.

# <a name="linuxtablinux"></a>[<span data-ttu-id="6f28b-111">Linux</span><span class="sxs-lookup"><span data-stu-id="6f28b-111">Linux</span></span>](#tab/linux)

    See the documentation for your Linux distribution on how to trust the HTTPS development certificate
---

4. <span data-ttu-id="6f28b-112">Ejecute la aplicación:</span><span class="sxs-lookup"><span data-stu-id="6f28b-112">Run the app:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="6f28b-113">Vaya a [http://localhost:5001](http://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="6f28b-113">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="6f28b-114">Haga clic en **Aceptar** para aceptar la política de privacidad y de cookies.</span><span class="sxs-lookup"><span data-stu-id="6f28b-114">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="6f28b-115">Esta aplicación no conserva información de carácter personal.</span><span class="sxs-lookup"><span data-stu-id="6f28b-115">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="6f28b-116">Abra *Pages/About.cshtml* y modifique la página con el siguiente marcado resaltado:</span><span class="sxs-lookup"><span data-stu-id="6f28b-116">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. <span data-ttu-id="6f28b-117">Vaya a [http://localhost:5001/About](http://localhost:5001/About) y confirme que los cambios aparecen reflejados.</span><span class="sxs-lookup"><span data-stu-id="6f28b-117">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="6f28b-118">Instale el [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="6f28b-118">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="6f28b-119">Cree un proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6f28b-119">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="6f28b-120">Abra un shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="6f28b-120">Open a command shell.</span></span> <span data-ttu-id="6f28b-121">Escriba el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="6f28b-121">Enter the following command:</span></span>

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="6f28b-122">Ejecute la aplicación con los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="6f28b-122">Run the app with the following commands:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="6f28b-123">Vaya a [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="6f28b-123">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="6f28b-124">Abra *Pages/About.cshtml* y modifique la página para que muestre el mensaje "¡Hola, mundo!".</span><span class="sxs-lookup"><span data-stu-id="6f28b-124">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="6f28b-125">La hora del servidor es @DateTime.Now" :</span><span class="sxs-lookup"><span data-stu-id="6f28b-125">The time on the server is @DateTime.Now":</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="6f28b-126">Vaya a [http://localhost:5000/About](http://localhost:5000/About) y compruebe los cambios.</span><span class="sxs-lookup"><span data-stu-id="6f28b-126">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="6f28b-127">Instale el **instalador del SDK** de .NET Core para SDK 1.0.4 desde la [página de descargas de .NET Core](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="6f28b-127">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="6f28b-128">Cree una carpeta para un nuevo proyecto de ASP .NET Core.</span><span class="sxs-lookup"><span data-stu-id="6f28b-128">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="6f28b-129">Abra un shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="6f28b-129">Open a command shell.</span></span> <span data-ttu-id="6f28b-130">Escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="6f28b-130">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="6f28b-131">Si ha instalado una versión posterior del SDK en el equipo, cree un archivo *global.json* para seleccionar la versión 1.0.4 del SDK.</span><span class="sxs-lookup"><span data-stu-id="6f28b-131">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="6f28b-132">Cree un proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6f28b-132">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="6f28b-133">Restaure los paquetes.</span><span class="sxs-lookup"><span data-stu-id="6f28b-133">Restore the packages.</span></span>

    ```console
    dotnet restore
    ```

6. <span data-ttu-id="6f28b-134">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6f28b-134">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="6f28b-135">El comando [dotnet run](/dotnet/core/tools/dotnet-run) compila primero la aplicación en caso de que sea necesario.</span><span class="sxs-lookup"><span data-stu-id="6f28b-135">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="6f28b-136">Vaya a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="6f28b-136">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
