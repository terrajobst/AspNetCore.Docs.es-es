---
title: Introducción a ASP.NET Core
author: rick-anderson
description: Tutorial rápido que crea y ejecuta una aplicación Hola mundo sencilla mediante ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: e814277663ff5a964171a71ebb6e0f094e0ddc60
ms.sourcegitcommit: 3d071fabaf90e32906df97b08a8d00e602db25c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/10/2018
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="e5f4a-103">Introducción a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e5f4a-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.0"

1. <span data-ttu-id="e5f4a-104">Instale el [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="e5f4a-104">Install the [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="e5f4a-105">Cree un nuevo proyecto de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e5f4a-105">Create a new .NET Core project.</span></span>

   <span data-ttu-id="e5f4a-106">En macOS y Linux, abra una ventana de terminal.</span><span class="sxs-lookup"><span data-stu-id="e5f4a-106">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="e5f4a-107">En Windows, abra un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="e5f4a-107">On Windows, open a command prompt.</span></span> <span data-ttu-id="e5f4a-108">Escriba el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="e5f4a-108">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="e5f4a-109">Ejecute la aplicación con los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="e5f4a-109">Run the app with the following commands:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="e5f4a-110">Vaya a [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="e5f4a-110">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="e5f4a-111">Abra *Pages/About.cshtml* y modifique la página para que muestre el mensaje "¡Hola, mundo!".</span><span class="sxs-lookup"><span data-stu-id="e5f4a-111">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="e5f4a-112">La hora del servidor es @DateTime.Now" :</span><span class="sxs-lookup"><span data-stu-id="e5f4a-112">The time on the server is @DateTime.Now":</span></span>

    <span data-ttu-id="e5f4a-113">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="e5f4a-113">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span></span>

6. <span data-ttu-id="e5f4a-114">Vaya a [http://localhost:5000/About](http://localhost:5000/About) y compruebe los cambios.</span><span class="sxs-lookup"><span data-stu-id="e5f4a-114">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="e5f4a-115">Instale el **instalador del SDK** de .NET Core para SDK 1.0.4 desde la [página de descargas de .NET Core](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="e5f4a-115">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="e5f4a-116">Cree una carpeta para un nuevo proyecto de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e5f4a-116">Create a folder for a new .NET Core project.</span></span>

   <span data-ttu-id="e5f4a-117">En macOS y Linux, abra una ventana de terminal.</span><span class="sxs-lookup"><span data-stu-id="e5f4a-117">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="e5f4a-118">En Windows, abra un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="e5f4a-118">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="e5f4a-119">Si ha instalado una versión posterior del SDK en el equipo, cree un archivo *global.json* para seleccionar la versión 1.0.4 del SDK.</span><span class="sxs-lookup"><span data-stu-id="e5f4a-119">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="e5f4a-120">Cree un nuevo proyecto de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e5f4a-120">Create a new .NET Core project.</span></span>

   ```terminal
   dotnet new web
   ```

5. <span data-ttu-id="e5f4a-121">Restaure los paquetes.</span><span class="sxs-lookup"><span data-stu-id="e5f4a-121">Restore the packages.</span></span>

    ```terminal
    dotnet restore
    ```

6. <span data-ttu-id="e5f4a-122">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e5f4a-122">Run the app.</span></span>

   ```terminal
   dotnet run
   ```

   <span data-ttu-id="e5f4a-123">El comando [dotnet run](/dotnet/core/tools/dotnet-run) compila primero la aplicación en caso de que sea necesario.</span><span class="sxs-lookup"><span data-stu-id="e5f4a-123">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="e5f4a-124">Vaya a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e5f4a-124">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end