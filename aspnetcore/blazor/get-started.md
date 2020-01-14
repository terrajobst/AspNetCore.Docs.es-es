---
title: Introducción a ASP.NET Core Blazor
author: guardrex
description: Para empezar a trabajar con Blazor, cree una aplicación Blazor con las herramientas que prefiera.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/09/2019
no-loc:
- Blazor
uid: blazor/get-started
ms.openlocfilehash: 554f4daff92a0839ee7679287a4618e9b51e0fe5
ms.sourcegitcommit: 925cdbd94613243f33bc7613a62ea34006219931
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/13/2020
ms.locfileid: "75921309"
---
# <a name="get-started-with-aspnet-core-opno-locblazor"></a><span data-ttu-id="8ac1e-103">Introducción a ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="8ac1e-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="8ac1e-104">Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8ac1e-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="8ac1e-105">Introducción a Blazor:</span><span class="sxs-lookup"><span data-stu-id="8ac1e-105">Get started with Blazor:</span></span>

::: moniker range=">= aspnetcore-3.1"

1. <span data-ttu-id="8ac1e-106">Instale el [SDK de .net Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="8ac1e-106">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="8ac1e-107">Opcionalmente, instale la plantilla [Blazor Webassembly](xref:blazor/hosting-models#blazor-webassembly) :</span><span class="sxs-lookup"><span data-stu-id="8ac1e-107">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="8ac1e-108">Instale el [SDK de .net Core 3,1 o posterior (versión preliminar)](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="8ac1e-108">Install the [.NET Core 3.1 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="8ac1e-109">Ejecute el siguiente comando en un shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-109">Run the following command in a command shell.</span></span> <span data-ttu-id="8ac1e-110">[Microsoft.AspNetCore.Blazor.](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/)El paquete de plantillas tiene una versión preliminar mientras Blazor Webassembly está en versión preliminar.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-110">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
   ```

1. <span data-ttu-id="8ac1e-111">Siga las instrucciones para su elección de herramientas:</span><span class="sxs-lookup"><span data-stu-id="8ac1e-111">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8ac1e-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8ac1e-112">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="8ac1e-113">1 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-113">1\.</span></span> <span data-ttu-id="8ac1e-114">Instale [Visual Studio 16,4 o posterior](https://visualstudio.microsoft.com/vs/preview/) con la carga de trabajo de **desarrollo web y ASP.net** .</span><span class="sxs-lookup"><span data-stu-id="8ac1e-114">Install [Visual Studio 16.4 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="8ac1e-115">2 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-115">2\.</span></span> <span data-ttu-id="8ac1e-116">Cree un nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-116">Create a new project.</span></span>

   <span data-ttu-id="8ac1e-117">3 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-117">3\.</span></span> <span data-ttu-id="8ac1e-118">Seleccione **Blazor aplicación**.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-118">Select **Blazor App**.</span></span> <span data-ttu-id="8ac1e-119">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-119">Select **Next**.</span></span>

   <span data-ttu-id="8ac1e-120">4 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-120">4\.</span></span> <span data-ttu-id="8ac1e-121">Proporcione un nombre para el proyecto en el campo **Nombre del proyecto** o acepte el predeterminado.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-121">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="8ac1e-122">Confirme que la entrada de **Ubicación** es correcta o proporcione una ubicación para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-122">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="8ac1e-123">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-123">Select **Create**.</span></span>

   <span data-ttu-id="8ac1e-124">5 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-124">5\.</span></span> <span data-ttu-id="8ac1e-125">Para obtener una experiencia de webassembly Blazor, elija la plantilla **aplicación WebassemblyBlazor** .</span><span class="sxs-lookup"><span data-stu-id="8ac1e-125">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="8ac1e-126">Para obtener una experiencia de servidor Blazor, elija la plantilla **aplicación deBlazor Server** .</span><span class="sxs-lookup"><span data-stu-id="8ac1e-126">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="8ac1e-127">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-127">Select **Create**.</span></span> <span data-ttu-id="8ac1e-128">Para obtener información sobre los dos Blazor modelos de hospedaje, *Blazor Server* y *Blazor webassembly*, vea <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-128">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="8ac1e-129">6 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-129">6\.</span></span> <span data-ttu-id="8ac1e-130">Presione **Ctrl**+**F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-130">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="8ac1e-131">Si ha instalado el Blazor extensión de Visual Studio para una versión preliminar anterior de ASP.NET Core Blazor (versión preliminar 6 o anterior), puede desinstalar la extensión.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-131">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="8ac1e-132">La instalación de las plantillas de Blazor en un shell de comandos ahora es suficiente para mostrar las plantillas en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-132">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8ac1e-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8ac1e-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="8ac1e-134">1 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-134">1\.</span></span> <span data-ttu-id="8ac1e-135">Instale [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="8ac1e-135">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="8ac1e-136">2 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-136">2\.</span></span> <span data-ttu-id="8ac1e-137">Instale la [ C# extensión de Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)más reciente.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-137">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="8ac1e-138">3 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-138">3\.</span></span> <span data-ttu-id="8ac1e-139">Para obtener una experiencia de webassembly Blazor, ejecute el siguiente comando en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="8ac1e-139">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="8ac1e-140">Para obtener una experiencia de servidor Blazor, ejecute el siguiente comando en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="8ac1e-140">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="8ac1e-141">Para obtener información sobre los dos Blazor modelos de hospedaje, *Blazor Server* y *Blazor webassembly*, vea <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-141">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="8ac1e-142">4 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-142">4\.</span></span> <span data-ttu-id="8ac1e-143">Abra la carpeta *WebApplication1* en Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-143">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="8ac1e-144">5 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-144">5\.</span></span> <span data-ttu-id="8ac1e-145">Para un proyecto de servidor de Blazor, el IDE solicita que agregue recursos para compilar y depurar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-145">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="8ac1e-146">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-146">Select **Yes**.</span></span>

   <span data-ttu-id="8ac1e-147">6 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-147">6\.</span></span> <span data-ttu-id="8ac1e-148">Si usa una aplicación de Blazor Server, ejecute la aplicación con el depurador de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-148">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="8ac1e-149">Si usa una aplicación Blazor webassembly, ejecute `dotnet run` desde la carpeta de proyecto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-149">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="8ac1e-150">7 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-150">7\.</span></span> <span data-ttu-id="8ac1e-151">En un navegador, vaya a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-151">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8ac1e-152">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="8ac1e-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="8ac1e-153">1 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-153">1\.</span></span> <span data-ttu-id="8ac1e-154">Instale [Visual Studio para Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="8ac1e-154">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

   <span data-ttu-id="8ac1e-155">2 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-155">2\.</span></span> <span data-ttu-id="8ac1e-156">Seleccione **archivo** > **nueva solución** o cree un **nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-156">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="8ac1e-157">3 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-157">3\.</span></span> <span data-ttu-id="8ac1e-158">En la barra lateral, seleccione **.net Core** > **aplicación**.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-158">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="8ac1e-159">4 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-159">4\.</span></span> <span data-ttu-id="8ac1e-160">Seleccione la plantilla **aplicación deBlazor Server** .</span><span class="sxs-lookup"><span data-stu-id="8ac1e-160">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="8ac1e-161">En este momento solo está disponible la plantilla de Blazor Server en Visual Studio para Mac.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-161">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="8ac1e-162">Para obtener una experiencia de webassembly Blazor, siga las instrucciones de la pestaña **CLI de .net Core** . Después de seleccionar la plantilla de servidor de Blazor, seleccione **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-162">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="8ac1e-163">Para obtener información sobre los dos Blazor modelos de hospedaje, *Blazor Server* y *Blazor webassembly*, vea <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-163">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="8ac1e-164">5 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-164">5\.</span></span> <span data-ttu-id="8ac1e-165">Establezca la **plataforma de destino** en **.net Core 3,1** y seleccione **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-165">Set the **Target Framework** to **.NET Core 3.1** and select **Next**.</span></span>

   <span data-ttu-id="8ac1e-166">6 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-166">6\.</span></span> <span data-ttu-id="8ac1e-167">En el campo **nombre del proyecto** , asigne un nombre a la aplicación `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-167">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="8ac1e-168">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-168">Select **Create**.</span></span>

   <span data-ttu-id="8ac1e-169">7 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-169">7\.</span></span> <span data-ttu-id="8ac1e-170">Seleccione **ejecutar** > **ejecutar sin depurar** para ejecutar la aplicación *sin el depurador*.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-170">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="8ac1e-171">Ejecute la aplicación con **iniciar depuración** para ejecutar la aplicación *con el depurador*.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-171">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

       If a prompt appears to trust the development certificate, trust the certificate and continue.

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8ac1e-172">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="8ac1e-172">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="8ac1e-173">Para obtener una experiencia de webassembly Blazor, ejecute los siguientes comandos en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="8ac1e-173">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="8ac1e-174">Para obtener una experiencia de servidor Blazor, ejecute los siguientes comandos en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="8ac1e-174">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="8ac1e-175">Para obtener información sobre los dos Blazor modelos de hospedaje, *Blazor Server* y *Blazor webassembly*, vea <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-175">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="8ac1e-176">En un navegador, vaya a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-176">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

::: moniker range="< aspnetcore-3.1"

1. <span data-ttu-id="8ac1e-177">Instale el [SDK de .net Core 3,0](https://dotnet.microsoft.com/download/dotnet-core/3.0)más reciente.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-177">Install the latest [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0).</span></span>

1. <span data-ttu-id="8ac1e-178">Opcionalmente, instale la plantilla [Blazor Webassembly](xref:blazor/hosting-models#blazor-webassembly) :</span><span class="sxs-lookup"><span data-stu-id="8ac1e-178">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="8ac1e-179">Instale el [SDK de .net Core 3,1 o posterior (versión preliminar)](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="8ac1e-179">Install the [.NET Core 3.1 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="8ac1e-180">Ejecute el siguiente comando en un shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-180">Run the following command in a command shell.</span></span> <span data-ttu-id="8ac1e-181">[Microsoft.AspNetCore.Blazor.](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/)El paquete de plantillas tiene una versión preliminar mientras Blazor Webassembly está en versión preliminar.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-181">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
   ```

1. <span data-ttu-id="8ac1e-182">Siga las instrucciones para su elección de herramientas:</span><span class="sxs-lookup"><span data-stu-id="8ac1e-182">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8ac1e-183">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8ac1e-183">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="8ac1e-184">1 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-184">1\.</span></span> <span data-ttu-id="8ac1e-185">Instale la versión más reciente de [Visual Studio](https://visualstudio.com/vs/) con la carga de trabajo de **desarrollo web y ASP.net** .</span><span class="sxs-lookup"><span data-stu-id="8ac1e-185">Install the latest [Visual Studio](https://visualstudio.com/vs/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="8ac1e-186">2 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-186">2\.</span></span> <span data-ttu-id="8ac1e-187">Opcionalmente, instale [Visual Studio 16,4 Preview 2 o posterior](https://visualstudio.microsoft.com/vs/preview/) con la carga Blazor de trabajo de **desarrollo web y ASP.net** para el desarrollo de aplicaciones de webassembly.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-187">Optionally install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload for Blazor WebAssembly app development.</span></span>

   <span data-ttu-id="8ac1e-188">3 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-188">3\.</span></span> <span data-ttu-id="8ac1e-189">Cree un nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-189">Create a new project.</span></span>

   <span data-ttu-id="8ac1e-190">4 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-190">4\.</span></span> <span data-ttu-id="8ac1e-191">Seleccione **Blazor aplicación**.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-191">Select **Blazor App**.</span></span> <span data-ttu-id="8ac1e-192">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-192">Select **Next**.</span></span>

   <span data-ttu-id="8ac1e-193">5 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-193">5\.</span></span> <span data-ttu-id="8ac1e-194">Proporcione un nombre para el proyecto en el campo **Nombre del proyecto** o acepte el predeterminado.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-194">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="8ac1e-195">Confirme que la entrada de **Ubicación** es correcta o proporcione una ubicación para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-195">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="8ac1e-196">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-196">Select **Create**.</span></span>

   <span data-ttu-id="8ac1e-197">6 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-197">6\.</span></span> <span data-ttu-id="8ac1e-198">Para obtener una experiencia de webassembly Blazor, elija la plantilla **aplicación WebassemblyBlazor** .</span><span class="sxs-lookup"><span data-stu-id="8ac1e-198">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="8ac1e-199">Para obtener una experiencia de servidor Blazor, elija la plantilla **aplicación deBlazor Server** .</span><span class="sxs-lookup"><span data-stu-id="8ac1e-199">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="8ac1e-200">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-200">Select **Create**.</span></span> <span data-ttu-id="8ac1e-201">Para obtener información sobre los dos Blazor modelos de hospedaje, *Blazor Server* y *Blazor webassembly*, vea <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-201">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="8ac1e-202">7 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-202">7\.</span></span> <span data-ttu-id="8ac1e-203">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-203">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="8ac1e-204">Si ha instalado el Blazor extensión de Visual Studio para una versión preliminar anterior de ASP.NET Core Blazor (versión preliminar 6 o anterior), puede desinstalar la extensión.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-204">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="8ac1e-205">La instalación de las plantillas de Blazor en un shell de comandos ahora es suficiente para mostrar las plantillas en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-205">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8ac1e-206">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8ac1e-206">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="8ac1e-207">1 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-207">1\.</span></span> <span data-ttu-id="8ac1e-208">Instale [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="8ac1e-208">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="8ac1e-209">2 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-209">2\.</span></span> <span data-ttu-id="8ac1e-210">Instale la [ C# extensión de Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)más reciente.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-210">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="8ac1e-211">3 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-211">3\.</span></span> <span data-ttu-id="8ac1e-212">Para obtener una experiencia de webassembly Blazor, ejecute el siguiente comando en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="8ac1e-212">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="8ac1e-213">Para obtener una experiencia de servidor Blazor, ejecute el siguiente comando en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="8ac1e-213">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="8ac1e-214">Para obtener información sobre los dos Blazor modelos de hospedaje, *Blazor Server* y *Blazor webassembly*, vea <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-214">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="8ac1e-215">4 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-215">4\.</span></span> <span data-ttu-id="8ac1e-216">Abra la carpeta *WebApplication1* en Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-216">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="8ac1e-217">5 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-217">5\.</span></span> <span data-ttu-id="8ac1e-218">Para un proyecto de servidor de Blazor, el IDE solicita que agregue recursos para compilar y depurar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-218">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="8ac1e-219">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-219">Select **Yes**.</span></span>

   <span data-ttu-id="8ac1e-220">6 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-220">6\.</span></span> <span data-ttu-id="8ac1e-221">Si usa una aplicación de Blazor Server, ejecute la aplicación con el depurador de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-221">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="8ac1e-222">Si usa una aplicación Blazor webassembly, ejecute `dotnet run` desde la carpeta de proyecto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-222">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="8ac1e-223">7 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-223">7\.</span></span> <span data-ttu-id="8ac1e-224">En un navegador, vaya a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-224">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8ac1e-225">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="8ac1e-225">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="8ac1e-226">1 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-226">1\.</span></span> <span data-ttu-id="8ac1e-227">Instale [Visual Studio para Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="8ac1e-227">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span> <span data-ttu-id="8ac1e-228">Cambie el [canal de actualización a vista previa](/visualstudio/mac/install-preview).</span><span class="sxs-lookup"><span data-stu-id="8ac1e-228">Switch the [Update channel to Preview](/visualstudio/mac/install-preview).</span></span>

   <span data-ttu-id="8ac1e-229">2 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-229">2\.</span></span> <span data-ttu-id="8ac1e-230">Seleccione **archivo** > **nueva solución** o cree un **nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-230">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="8ac1e-231">3 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-231">3\.</span></span> <span data-ttu-id="8ac1e-232">En la barra lateral, seleccione **.net Core** > **aplicación**.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-232">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="8ac1e-233">4 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-233">4\.</span></span> <span data-ttu-id="8ac1e-234">Seleccione la plantilla **aplicación deBlazor Server** .</span><span class="sxs-lookup"><span data-stu-id="8ac1e-234">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="8ac1e-235">En este momento solo está disponible la plantilla de Blazor Server en Visual Studio para Mac.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-235">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="8ac1e-236">Para obtener una experiencia de webassembly Blazor, siga las instrucciones de la pestaña **CLI de .net Core** . Después de seleccionar la plantilla de servidor de Blazor, seleccione **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-236">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="8ac1e-237">Para obtener información sobre los dos Blazor modelos de hospedaje, *Blazor Server* y *Blazor webassembly*, vea <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-237">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="8ac1e-238">5 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-238">5\.</span></span> <span data-ttu-id="8ac1e-239">Establezca la **plataforma de destino** en **.net Core 3,0** y seleccione **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-239">Set the **Target Framework** to **.NET Core 3.0** and select **Next**.</span></span>

   <span data-ttu-id="8ac1e-240">6 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-240">6\.</span></span> <span data-ttu-id="8ac1e-241">En el campo **nombre del proyecto** , asigne un nombre a la aplicación `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-241">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="8ac1e-242">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-242">Select **Create**.</span></span>

   <span data-ttu-id="8ac1e-243">7 \.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-243">7\.</span></span> <span data-ttu-id="8ac1e-244">Seleccione **ejecutar** > **ejecutar sin depurar** para ejecutar la aplicación *sin el depurador*.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-244">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="8ac1e-245">Ejecute la aplicación con **iniciar depuración** para ejecutar la aplicación *con el depurador*.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-245">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

       If a prompt appears to trust the development certificate, trust the certificate and continue.

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8ac1e-246">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="8ac1e-246">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="8ac1e-247">Para obtener una experiencia de webassembly Blazor, ejecute los siguientes comandos en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="8ac1e-247">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="8ac1e-248">Para obtener una experiencia de servidor Blazor, ejecute los siguientes comandos en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="8ac1e-248">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="8ac1e-249">Para obtener información sobre los dos Blazor modelos de hospedaje, *Blazor Server* y *Blazor webassembly*, vea <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-249">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="8ac1e-250">En un navegador, vaya a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-250">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

<span data-ttu-id="8ac1e-251">Hay varias páginas disponibles en las pestañas de la barra lateral:</span><span class="sxs-lookup"><span data-stu-id="8ac1e-251">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="8ac1e-252">Página de inicio de</span><span class="sxs-lookup"><span data-stu-id="8ac1e-252">Home</span></span>
* <span data-ttu-id="8ac1e-253">Contador</span><span class="sxs-lookup"><span data-stu-id="8ac1e-253">Counter</span></span>
* <span data-ttu-id="8ac1e-254">Capturar datos</span><span class="sxs-lookup"><span data-stu-id="8ac1e-254">Fetch data</span></span>

<span data-ttu-id="8ac1e-255">En la página Contador, seleccione el botón **Click me** para aumentar el contador sin una actualización de página.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-255">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="8ac1e-256">El incremento de un contador en una página web normalmente requiere la escritura de JavaScript, pero con C#Blazor puede usar.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-256">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="8ac1e-257">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="8ac1e-257">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="8ac1e-258">Una solicitud de `/counter` en el explorador, tal y como se especifica en la Directiva de `@page` en la parte superior, hace que el componente `Counter` represente su contenido.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-258">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="8ac1e-259">Los componentes se representan en una representación en memoria del árbol de representación que se puede usar para actualizar la interfaz de usuario de una forma flexible y eficaz.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-259">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="8ac1e-260">Cada vez que se selecciona el botón **click me** :</span><span class="sxs-lookup"><span data-stu-id="8ac1e-260">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="8ac1e-261">Se desencadena el evento `onclick`.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-261">The `onclick` event is fired.</span></span>
* <span data-ttu-id="8ac1e-262">Se llama al método `IncrementCount`.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-262">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="8ac1e-263">Se incrementa el `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-263">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="8ac1e-264">El componente se representará de nuevo.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-264">The component is rendered again.</span></span>

<span data-ttu-id="8ac1e-265">El tiempo de ejecución compara el nuevo contenido con el contenido anterior y solo aplica el contenido cambiado al Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="8ac1e-265">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="8ac1e-266">Agregar un componente a otro componente mediante la sintaxis HTML.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-266">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="8ac1e-267">Por ejemplo, agregue el componente `Counter` a la Página principal de la aplicación agregando un elemento `<Counter />` al componente `Index`.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-267">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="8ac1e-268">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="8ac1e-268">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="8ac1e-269">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-269">Run the app.</span></span> <span data-ttu-id="8ac1e-270">La Página principal tiene su propio contador proporcionado por el componente de `Counter`.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-270">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="8ac1e-271">Los parámetros de componente se especifican mediante atributos o [contenido secundario](xref:blazor/components#child-content), que permiten establecer propiedades en el componente secundario.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-271">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="8ac1e-272">Para agregar un parámetro al componente de `Counter`, actualice el bloque de `@code` del componente:</span><span class="sxs-lookup"><span data-stu-id="8ac1e-272">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="8ac1e-273">Agregue una propiedad pública para `IncrementAmount` con un atributo de `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-273">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="8ac1e-274">Cambie el método `IncrementCount` para usar `IncrementAmount` al aumentar el valor de `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-274">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="8ac1e-275">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="8ac1e-275">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="8ac1e-276">Especifique el `IncrementAmount` en el elemento `<Counter>` del componente de `Index` mediante un atributo.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-276">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="8ac1e-277">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="8ac1e-277">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="8ac1e-278">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-278">Run the app.</span></span> <span data-ttu-id="8ac1e-279">El componente de `Index` tiene su propio contador que se incrementa en diez cada vez que se selecciona el botón **click me** .</span><span class="sxs-lookup"><span data-stu-id="8ac1e-279">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="8ac1e-280">El componente de `Counter` (*Counter. Razor*) en `/counter` continúa aumentando en uno.</span><span class="sxs-lookup"><span data-stu-id="8ac1e-280">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8ac1e-281">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="8ac1e-281">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="8ac1e-282">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="8ac1e-282">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
