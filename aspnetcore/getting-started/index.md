---
title: Introducción a ASP.NET Core
author: rick-anderson
description: Tutorial rápido que crea y ejecuta una aplicación Hola mundo sencilla mediante ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: a6a5023594aec01370143e7d1f35fb45c109122a
ms.sourcegitcommit: 13940eb53c68664b11a2d685ee17c78faab1945d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2018
ms.locfileid: "47860945"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="3ebdb-103">Introducción a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3ebdb-103">Get started with ASP.NET Core</span></span>

<span data-ttu-id="3ebdb-104">Este documento proporciona los pasos necesarios para crear y ejecutar una aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3ebdb-104">This document provides steps for creating and running an ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="3ebdb-105">Instale el [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="3ebdb-105">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="3ebdb-106">Cree un proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3ebdb-106">Create an ASP.NET Core project.</span></span> <span data-ttu-id="3ebdb-107">Abra un shell de comandos y escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="3ebdb-107">Open a command shell and enter the following command:</span></span>

   ```console
   dotnet new webapp -o aspnetcoreapp
   ```

3. <span data-ttu-id="3ebdb-108">Confíe en el certificado de desarrollo HTTPS:</span><span class="sxs-lookup"><span data-stu-id="3ebdb-108">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="3ebdb-109">Windows</span><span class="sxs-lookup"><span data-stu-id="3ebdb-109">Windows</span></span>](#tab/windows)

  ```console
  dotnet dev-certs https --trust
  ```

  <span data-ttu-id="3ebdb-110">El comando anterior muestra el siguiente cuadro de diálogo:</span><span class="sxs-lookup"><span data-stu-id="3ebdb-110">The preceding command displays the following dialog:</span></span>

  ![Cuadro de diálogo de advertencia de seguridad](_static/cert.png)

  <span data-ttu-id="3ebdb-112">Si acepta confiar en el certificado de desarrollo, seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="3ebdb-112">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="3ebdb-113">macOS</span><span class="sxs-lookup"><span data-stu-id="3ebdb-113">macOS</span></span>](#tab/macos)

  ```console
  dotnet dev-certs https --trust
  ```

  <span data-ttu-id="3ebdb-114">El comando anterior muestra el siguiente mensaje:</span><span class="sxs-lookup"><span data-stu-id="3ebdb-114">The preceding command displays the following message:</span></span>

  <span data-ttu-id="3ebdb-115">*Se ha solicitado que confíe en el certificado de desarrollo HTTPS. Si el certificado todavía no es de confianza, no se ejecutará el comando siguiente:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span><span class="sxs-lookup"><span data-stu-id="3ebdb-115">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span></span>  
  <span data-ttu-id="3ebdb-116">\* Este comando podría solicitarle la contraseña para instalar el certificado en la cadena de claves del sistema.</span><span class="sxs-lookup"><span data-stu-id="3ebdb-116">\*This command might prompt you for your password to install the certificate on the system keychain.</span></span>
  
  <span data-ttu-id="3ebdb-117">Contraseña:\*</span><span class="sxs-lookup"><span data-stu-id="3ebdb-117">Password:\*</span></span>

  <span data-ttu-id="3ebdb-118">Si acepta confiar en el certificado de desarrollo, escriba la contraseña.</span><span class="sxs-lookup"><span data-stu-id="3ebdb-118">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="3ebdb-119">Linux</span><span class="sxs-lookup"><span data-stu-id="3ebdb-119">Linux</span></span>](#tab/linux)

  <span data-ttu-id="3ebdb-120">Consulte la documentación correspondiente a su distribución Linux sobre cómo confiar en el certificado de desarrollo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3ebdb-120">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>
   
---

4. <span data-ttu-id="3ebdb-121">Ejecute la aplicación:</span><span class="sxs-lookup"><span data-stu-id="3ebdb-121">Run the app:</span></span>

   ```console
   cd aspnetcoreapp
   dotnet run
   ```

5. <span data-ttu-id="3ebdb-122">Vaya a [http://localhost:5001](http://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="3ebdb-122">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="3ebdb-123">Haga clic en **Aceptar** para aceptar la política de privacidad y de cookies.</span><span class="sxs-lookup"><span data-stu-id="3ebdb-123">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="3ebdb-124">Esta aplicación no conserva información de carácter personal.</span><span class="sxs-lookup"><span data-stu-id="3ebdb-124">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="3ebdb-125">Abra *Pages/About.cshtml* y modifique la página con el siguiente marcado resaltado:</span><span class="sxs-lookup"><span data-stu-id="3ebdb-125">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

   [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. <span data-ttu-id="3ebdb-126">Vaya a [http://localhost:5001/About](http://localhost:5001/About) y confirme que los cambios aparecen reflejados.</span><span class="sxs-lookup"><span data-stu-id="3ebdb-126">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="3ebdb-127">Instale el [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="3ebdb-127">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="3ebdb-128">Cree un proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3ebdb-128">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="3ebdb-129">Abra un shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="3ebdb-129">Open a command shell.</span></span> <span data-ttu-id="3ebdb-130">Escriba el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="3ebdb-130">Enter the following command:</span></span>

   ```console
   dotnet new razor -o aspnetcoreapp
   ```

3. <span data-ttu-id="3ebdb-131">Ejecute la aplicación con los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="3ebdb-131">Run the app with the following commands:</span></span>

   ```console
   cd aspnetcoreapp
   dotnet run
   ```

4. <span data-ttu-id="3ebdb-132">Vaya a [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="3ebdb-132">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="3ebdb-133">Abra *Pages/About.cshtml* y modifique la página para que muestre el mensaje "¡Hola, mundo!".</span><span class="sxs-lookup"><span data-stu-id="3ebdb-133">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="3ebdb-134">La hora del servidor es @DateTime.Now" :</span><span class="sxs-lookup"><span data-stu-id="3ebdb-134">The time on the server is @DateTime.Now":</span></span>

   [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="3ebdb-135">Vaya a [http://localhost:5000/About](http://localhost:5000/About) y compruebe los cambios.</span><span class="sxs-lookup"><span data-stu-id="3ebdb-135">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="3ebdb-136">Instale el **instalador del SDK** de .NET Core para SDK 1.0.4 desde la [página de descargas de .NET Core](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="3ebdb-136">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="3ebdb-137">Cree una carpeta para un nuevo proyecto de ASP .NET Core.</span><span class="sxs-lookup"><span data-stu-id="3ebdb-137">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="3ebdb-138">Abra un shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="3ebdb-138">Open a command shell.</span></span> <span data-ttu-id="3ebdb-139">Escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="3ebdb-139">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="3ebdb-140">Si ha instalado una versión posterior del SDK en el equipo, cree un archivo *global.json* para seleccionar la versión 1.0.4 del SDK.</span><span class="sxs-lookup"><span data-stu-id="3ebdb-140">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="3ebdb-141">Cree un proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3ebdb-141">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="3ebdb-142">Restaure los paquetes.</span><span class="sxs-lookup"><span data-stu-id="3ebdb-142">Restore the packages.</span></span>

   ```console
   dotnet restore
   ```

6. <span data-ttu-id="3ebdb-143">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3ebdb-143">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="3ebdb-144">El comando [dotnet run](/dotnet/core/tools/dotnet-run) compila primero la aplicación en caso de que sea necesario.</span><span class="sxs-lookup"><span data-stu-id="3ebdb-144">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="3ebdb-145">Vaya a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="3ebdb-145">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end
