---
title: Introducción a ASP.NET Core
author: rick-anderson
description: Tutorial rápido que crea y ejecuta una aplicación Hola mundo sencilla mediante ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 7ab9f303d74786c4ac76f002d0f2c66371e78cb8
ms.sourcegitcommit: b4c7b1a4c48dec0865f27874275c73da1f75e918
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228587"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="d2015-103">Introducción a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d2015-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="d2015-104">Instale el [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="d2015-104">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="d2015-105">Cree un proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d2015-105">Create an ASP.NET Core project.</span></span> <span data-ttu-id="d2015-106">Abra un shell de comandos y escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="d2015-106">Open a command shell and enter the following command:</span></span>

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    [!INCLUDE [](~/includes/webapp-alias-notice.md)]

3. <span data-ttu-id="d2015-107">Confíe en el certificado de desarrollo HTTPS:</span><span class="sxs-lookup"><span data-stu-id="d2015-107">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="d2015-108">Windows</span><span class="sxs-lookup"><span data-stu-id="d2015-108">Windows</span></span>](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

   <span data-ttu-id="d2015-109">El comando anterior muestra el siguiente cuadro de diálogo:</span><span class="sxs-lookup"><span data-stu-id="d2015-109">The preceding command displays the following dialog:</span></span>

   ![Cuadro de diálogo de advertencia de seguridad](_static/cert.png)

   <span data-ttu-id="d2015-111">Si acepta confiar en el certificado de desarrollo, seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="d2015-111">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="d2015-112">macOS</span><span class="sxs-lookup"><span data-stu-id="d2015-112">macOS</span></span>](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

   <span data-ttu-id="d2015-113">El comando anterior muestra el siguiente mensaje:</span><span class="sxs-lookup"><span data-stu-id="d2015-113">The preceding command displays the following message:</span></span>

   <span data-ttu-id="d2015-114">*Se ha solicitado que confíe en el certificado de desarrollo HTTPS. Si el certificado todavía no es de confianza, se ejecutará el siguiente comando:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'` *Es posible que este comando le solicite su contraseña para instalar el certificado en la cadena de claves del sistema.    Contraseña:*</span><span class="sxs-lookup"><span data-stu-id="d2015-114">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'` *This command might prompt you for your password to install the certificate on the system keychain.    Password:*</span></span>

   <span data-ttu-id="d2015-115">Si acepta confiar en el certificado de desarrollo, escriba la contraseña.</span><span class="sxs-lookup"><span data-stu-id="d2015-115">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="d2015-116">Linux</span><span class="sxs-lookup"><span data-stu-id="d2015-116">Linux</span></span>](#tab/linux)

   <a name="see-the-documentation-for-your-linux-distribution-on-how-to-trust-the-https-development-certificate"></a><span data-ttu-id="d2015-117">Consulte la documentación correspondiente a su distribución Linux sobre cómo confiar en el certificado de desarrollo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d2015-117">See the documentation for your Linux distribution on how to trust the HTTPS development certificate</span></span>
---

4. <span data-ttu-id="d2015-118">Ejecute la aplicación:</span><span class="sxs-lookup"><span data-stu-id="d2015-118">Run the app:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="d2015-119">Vaya a [http://localhost:5001](http://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="d2015-119">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="d2015-120">Haga clic en **Aceptar** para aceptar la política de privacidad y de cookies.</span><span class="sxs-lookup"><span data-stu-id="d2015-120">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="d2015-121">Esta aplicación no conserva información de carácter personal.</span><span class="sxs-lookup"><span data-stu-id="d2015-121">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="d2015-122">Abra *Pages/About.cshtml* y modifique la página con el siguiente marcado resaltado:</span><span class="sxs-lookup"><span data-stu-id="d2015-122">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. <span data-ttu-id="d2015-123">Vaya a [http://localhost:5001/About](http://localhost:5001/About) y confirme que los cambios aparecen reflejados.</span><span class="sxs-lookup"><span data-stu-id="d2015-123">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="d2015-124">Instale el [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="d2015-124">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="d2015-125">Cree un proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d2015-125">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="d2015-126">Abra un shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="d2015-126">Open a command shell.</span></span> <span data-ttu-id="d2015-127">Escriba el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="d2015-127">Enter the following command:</span></span>

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="d2015-128">Ejecute la aplicación con los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="d2015-128">Run the app with the following commands:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="d2015-129">Vaya a [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="d2015-129">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="d2015-130">Abra *Pages/About.cshtml* y modifique la página para que muestre el mensaje "¡Hola, mundo!".</span><span class="sxs-lookup"><span data-stu-id="d2015-130">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="d2015-131">La hora del servidor es @DateTime.Now" :</span><span class="sxs-lookup"><span data-stu-id="d2015-131">The time on the server is @DateTime.Now":</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="d2015-132">Vaya a [http://localhost:5000/About](http://localhost:5000/About) y compruebe los cambios.</span><span class="sxs-lookup"><span data-stu-id="d2015-132">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="d2015-133">Instale el **instalador del SDK** de .NET Core para SDK 1.0.4 desde la [página de descargas de .NET Core](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="d2015-133">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="d2015-134">Cree una carpeta para un nuevo proyecto de ASP .NET Core.</span><span class="sxs-lookup"><span data-stu-id="d2015-134">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="d2015-135">Abra un shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="d2015-135">Open a command shell.</span></span> <span data-ttu-id="d2015-136">Escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="d2015-136">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="d2015-137">Si ha instalado una versión posterior del SDK en el equipo, cree un archivo *global.json* para seleccionar la versión 1.0.4 del SDK.</span><span class="sxs-lookup"><span data-stu-id="d2015-137">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="d2015-138">Cree un proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d2015-138">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="d2015-139">Restaure los paquetes.</span><span class="sxs-lookup"><span data-stu-id="d2015-139">Restore the packages.</span></span>

    ```console
    dotnet restore
    ```

6. <span data-ttu-id="d2015-140">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d2015-140">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="d2015-141">El comando [dotnet run](/dotnet/core/tools/dotnet-run) compila primero la aplicación en caso de que sea necesario.</span><span class="sxs-lookup"><span data-stu-id="d2015-141">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="d2015-142">Vaya a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="d2015-142">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
