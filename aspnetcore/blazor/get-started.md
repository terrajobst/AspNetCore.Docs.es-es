---
title: Introducción a ASP.NET Core extraordinarias
author: guardrex
description: Para empezar a trabajar con más increíble, cree una aplicación increíblemente con las herramientas de su elección.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/21/2019
uid: blazor/get-started
ms.openlocfilehash: 80ff7b42a44e722dd27bc4fde53a066863448e10
ms.sourcegitcommit: 810d5831169770ee240d03207d6671dabea2486e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2019
ms.locfileid: "72779124"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="d9222-103">Introducción a ASP.NET Core extraordinarias</span><span class="sxs-lookup"><span data-stu-id="d9222-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="d9222-104">Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d9222-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="d9222-105">Introducción a más increíble:</span><span class="sxs-lookup"><span data-stu-id="d9222-105">Get started with Blazor:</span></span>

::: moniker range=">= aspnetcore-3.1"

1. <span data-ttu-id="d9222-106">Instale el [SDK de la versión preliminar de .net Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="d9222-106">Install the [.NET Core 3.1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="d9222-107">Instale la plantilla de [Webassembly de extraordinarias](xref:blazor/hosting-models#blazor-webassembly) ejecutando el siguiente comando en un shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="d9222-107">Install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by running the following command in a command shell.</span></span> <span data-ttu-id="d9222-108">El paquete [Microsoft. AspNetCore. extraordinarias. templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) tiene una versión preliminar, mientras que el webassembly de extraordinarias está en versión preliminar.</span><span class="sxs-lookup"><span data-stu-id="d9222-108">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview1.19508.20
   ```

1. <span data-ttu-id="d9222-109">Siga las instrucciones para su elección de herramientas:</span><span class="sxs-lookup"><span data-stu-id="d9222-109">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d9222-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d9222-110">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="d9222-111">1 \.</span><span class="sxs-lookup"><span data-stu-id="d9222-111">1\.</span></span> <span data-ttu-id="d9222-112">Instale [Visual Studio 16,4 Preview 2 o posterior](https://visualstudio.microsoft.com/vs/preview/) con la carga de trabajo de **desarrollo web y ASP.net** .</span><span class="sxs-lookup"><span data-stu-id="d9222-112">Install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="d9222-113">2 \.</span><span class="sxs-lookup"><span data-stu-id="d9222-113">2\.</span></span> <span data-ttu-id="d9222-114">Cree un nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="d9222-114">Create a new project.</span></span>

   <span data-ttu-id="d9222-115">3 \.</span><span class="sxs-lookup"><span data-stu-id="d9222-115">3\.</span></span> <span data-ttu-id="d9222-116">Seleccione **aplicación extraordinaria**.</span><span class="sxs-lookup"><span data-stu-id="d9222-116">Select **Blazor App**.</span></span> <span data-ttu-id="d9222-117">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="d9222-117">Select **Next**.</span></span>

   <span data-ttu-id="d9222-118">4 \.</span><span class="sxs-lookup"><span data-stu-id="d9222-118">4\.</span></span> <span data-ttu-id="d9222-119">Proporcione un nombre para el proyecto en el campo **Nombre del proyecto** o acepte el predeterminado.</span><span class="sxs-lookup"><span data-stu-id="d9222-119">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="d9222-120">Confirme que la entrada de **Ubicación** es correcta o proporcione una ubicación para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="d9222-120">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="d9222-121">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="d9222-121">Select **Create**.</span></span>

   <span data-ttu-id="d9222-122">5 \.</span><span class="sxs-lookup"><span data-stu-id="d9222-122">5\.</span></span> <span data-ttu-id="d9222-123">Para disfrutar de una experiencia de webassembly increíblemente rápida, elija la plantilla de **aplicación de Webassemble más brillante** .</span><span class="sxs-lookup"><span data-stu-id="d9222-123">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="d9222-124">Para obtener una experiencia de servidor increíblemente rápida, elija la plantilla de **aplicación de servidor de extraordinarias** .</span><span class="sxs-lookup"><span data-stu-id="d9222-124">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="d9222-125">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="d9222-125">Select **Create**.</span></span> <span data-ttu-id="d9222-126">Para obtener más información sobre los dos modelos de hospedaje más increíbles, el *servidor* y el *webensamblado*increíble, consulte <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d9222-126">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="d9222-127">6 \.</span><span class="sxs-lookup"><span data-stu-id="d9222-127">6\.</span></span> <span data-ttu-id="d9222-128">Presione **Ctrl**+**F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d9222-128">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="d9222-129">Si ha instalado la extensión de Visual Studio de extraordinarias para una versión preliminar anterior de ASP.NET Core extraordinaria (versión preliminar 6 o anterior), puede desinstalar la extensión.</span><span class="sxs-lookup"><span data-stu-id="d9222-129">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="d9222-130">Instalar las plantillas de extraordinarias en un shell de comandos ahora es suficiente para mostrar las plantillas en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d9222-130">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d9222-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d9222-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="d9222-132">1 \.</span><span class="sxs-lookup"><span data-stu-id="d9222-132">1\.</span></span> <span data-ttu-id="d9222-133">Instale [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="d9222-133">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="d9222-134">2 \.</span><span class="sxs-lookup"><span data-stu-id="d9222-134">2\.</span></span> <span data-ttu-id="d9222-135">Instale la [ C# extensión de Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)más reciente.</span><span class="sxs-lookup"><span data-stu-id="d9222-135">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="d9222-136">3 \.</span><span class="sxs-lookup"><span data-stu-id="d9222-136">3\.</span></span> <span data-ttu-id="d9222-137">En el caso de una experiencia de webassembler extraordinaria, ejecute el siguiente comando en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="d9222-137">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="d9222-138">Para obtener una experiencia de servidor extraordinaria, ejecute el siguiente comando en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="d9222-138">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="d9222-139">Para obtener más información sobre los dos modelos de hospedaje más increíbles, el *servidor* y el *webensamblado*increíble, consulte <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d9222-139">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="d9222-140">4 \.</span><span class="sxs-lookup"><span data-stu-id="d9222-140">4\.</span></span> <span data-ttu-id="d9222-141">Abra la carpeta *WebApplication1* en Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d9222-141">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="d9222-142">5 \.</span><span class="sxs-lookup"><span data-stu-id="d9222-142">5\.</span></span> <span data-ttu-id="d9222-143">En el caso de un proyecto de servidor más rápido, el IDE solicita que agregue recursos para compilar y depurar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="d9222-143">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="d9222-144">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="d9222-144">Select **Yes**.</span></span>

   <span data-ttu-id="d9222-145">6 \.</span><span class="sxs-lookup"><span data-stu-id="d9222-145">6\.</span></span> <span data-ttu-id="d9222-146">Si usa una aplicación de servidor más brillante, ejecute la aplicación con el depurador de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d9222-146">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="d9222-147">Si usa una aplicación de webassembly increíblemente alta, ejecute `dotnet run` desde la carpeta de proyecto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d9222-147">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="d9222-148">7 \.</span><span class="sxs-lookup"><span data-stu-id="d9222-148">7\.</span></span> <span data-ttu-id="d9222-149">En un navegador, vaya a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="d9222-149">In a browser, navigate to `https://localhost:5001`.</span></span>

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor Server experience, select the **Blazor Server App** template. For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d9222-150">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="d9222-150">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="d9222-151">En el caso de una experiencia de webassembler extraordinaria, ejecute los siguientes comandos en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="d9222-151">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="d9222-152">Para obtener una experiencia de servidor extraordinaria, ejecute los siguientes comandos en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="d9222-152">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="d9222-153">Para obtener más información sobre los dos modelos de hospedaje más increíbles, el *servidor* y el *webensamblado*increíble, consulte <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d9222-153">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="d9222-154">En un navegador, vaya a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="d9222-154">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

::: moniker range="< aspnetcore-3.1"

1. <span data-ttu-id="d9222-155">Instale la versión más reciente del [SDK de .net Core 3,0](https://dotnet.microsoft.com/download/dotnet-core/3.0) .</span><span class="sxs-lookup"><span data-stu-id="d9222-155">Install the latest [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>

1. <span data-ttu-id="d9222-156">Si lo desea, puede instalar la plantilla de [Webassembly de extraordinarias](xref:blazor/hosting-models#blazor-webassembly) mediante la instalación del [SDK de .net Core 3,1 Preview](https://dotnet.microsoft.com/download/dotnet-core/3.1) y, después, ejecutar el siguiente comando en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="d9222-156">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by installing the [.NET Core 3.1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1) and then running the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview1.19508.20
   ```

1. <span data-ttu-id="d9222-157">Siga las instrucciones para su elección de herramientas:</span><span class="sxs-lookup"><span data-stu-id="d9222-157">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d9222-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d9222-158">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="d9222-159">1 \.</span><span class="sxs-lookup"><span data-stu-id="d9222-159">1\.</span></span> <span data-ttu-id="d9222-160">Instale la versión más reciente de [Visual Studio](https://visualstudio.com/vs/) con la carga de trabajo de **desarrollo web y ASP.net** .</span><span class="sxs-lookup"><span data-stu-id="d9222-160">Install the latest [Visual Studio](https://visualstudio.com/vs/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="d9222-161">2 \.</span><span class="sxs-lookup"><span data-stu-id="d9222-161">2\.</span></span> <span data-ttu-id="d9222-162">Opcionalmente, instale [Visual Studio 16,4 Preview 2 o posterior](https://visualstudio.microsoft.com/vs/preview/) con la carga de trabajo de **desarrollo web y ASP.net** para el desarrollo de aplicaciones webassembly.</span><span class="sxs-lookup"><span data-stu-id="d9222-162">Optionally install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload for Blazor WebAssembly app development.</span></span>

   <span data-ttu-id="d9222-163">3 \.</span><span class="sxs-lookup"><span data-stu-id="d9222-163">3\.</span></span> <span data-ttu-id="d9222-164">Cree un nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="d9222-164">Create a new project.</span></span>

   <span data-ttu-id="d9222-165">4 \.</span><span class="sxs-lookup"><span data-stu-id="d9222-165">4\.</span></span> <span data-ttu-id="d9222-166">Seleccione **aplicación extraordinaria**.</span><span class="sxs-lookup"><span data-stu-id="d9222-166">Select **Blazor App**.</span></span> <span data-ttu-id="d9222-167">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="d9222-167">Select **Next**.</span></span>

   <span data-ttu-id="d9222-168">5 \.</span><span class="sxs-lookup"><span data-stu-id="d9222-168">5\.</span></span> <span data-ttu-id="d9222-169">Proporcione un nombre para el proyecto en el campo **Nombre del proyecto** o acepte el predeterminado.</span><span class="sxs-lookup"><span data-stu-id="d9222-169">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="d9222-170">Confirme que la entrada de **Ubicación** es correcta o proporcione una ubicación para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="d9222-170">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="d9222-171">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="d9222-171">Select **Create**.</span></span>

   <span data-ttu-id="d9222-172">6 \.</span><span class="sxs-lookup"><span data-stu-id="d9222-172">6\.</span></span> <span data-ttu-id="d9222-173">Para disfrutar de una experiencia de webassembly increíblemente rápida, elija la plantilla de **aplicación de Webassemble más brillante** .</span><span class="sxs-lookup"><span data-stu-id="d9222-173">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="d9222-174">Para obtener una experiencia de servidor increíblemente rápida, elija la plantilla de **aplicación de servidor de extraordinarias** .</span><span class="sxs-lookup"><span data-stu-id="d9222-174">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="d9222-175">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="d9222-175">Select **Create**.</span></span> <span data-ttu-id="d9222-176">Para obtener más información sobre los dos modelos de hospedaje más increíbles, el *servidor* y el *webensamblado*increíble, consulte <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d9222-176">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="d9222-177">7 \.</span><span class="sxs-lookup"><span data-stu-id="d9222-177">7\.</span></span> <span data-ttu-id="d9222-178">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d9222-178">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="d9222-179">Si ha instalado la extensión de Visual Studio de extraordinarias para una versión preliminar anterior de ASP.NET Core extraordinaria (versión preliminar 6 o anterior), puede desinstalar la extensión.</span><span class="sxs-lookup"><span data-stu-id="d9222-179">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="d9222-180">Instalar las plantillas de extraordinarias en un shell de comandos ahora es suficiente para mostrar las plantillas en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d9222-180">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d9222-181">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d9222-181">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="d9222-182">1 \.</span><span class="sxs-lookup"><span data-stu-id="d9222-182">1\.</span></span> <span data-ttu-id="d9222-183">Instale [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="d9222-183">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="d9222-184">2 \.</span><span class="sxs-lookup"><span data-stu-id="d9222-184">2\.</span></span> <span data-ttu-id="d9222-185">Instale la [ C# extensión de Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)más reciente.</span><span class="sxs-lookup"><span data-stu-id="d9222-185">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="d9222-186">3 \.</span><span class="sxs-lookup"><span data-stu-id="d9222-186">3\.</span></span> <span data-ttu-id="d9222-187">En el caso de una experiencia de webassembler extraordinaria, ejecute el siguiente comando en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="d9222-187">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="d9222-188">Para obtener una experiencia de servidor extraordinaria, ejecute el siguiente comando en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="d9222-188">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="d9222-189">Para obtener más información sobre los dos modelos de hospedaje más increíbles, el *servidor* y el *webensamblado*increíble, consulte <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d9222-189">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="d9222-190">4 \.</span><span class="sxs-lookup"><span data-stu-id="d9222-190">4\.</span></span> <span data-ttu-id="d9222-191">Abra la carpeta *WebApplication1* en Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d9222-191">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="d9222-192">5 \.</span><span class="sxs-lookup"><span data-stu-id="d9222-192">5\.</span></span> <span data-ttu-id="d9222-193">En el caso de un proyecto de servidor más rápido, el IDE solicita que agregue recursos para compilar y depurar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="d9222-193">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="d9222-194">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="d9222-194">Select **Yes**.</span></span>

   <span data-ttu-id="d9222-195">6 \.</span><span class="sxs-lookup"><span data-stu-id="d9222-195">6\.</span></span> <span data-ttu-id="d9222-196">Si usa una aplicación de servidor más brillante, ejecute la aplicación con el depurador de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d9222-196">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="d9222-197">Si usa una aplicación de webassembly increíblemente alta, ejecute `dotnet run` desde la carpeta de proyecto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d9222-197">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="d9222-198">7 \.</span><span class="sxs-lookup"><span data-stu-id="d9222-198">7\.</span></span> <span data-ttu-id="d9222-199">En un navegador, vaya a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="d9222-199">In a browser, navigate to `https://localhost:5001`.</span></span>

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor Server experience, select the **Blazor Server App** template. For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d9222-200">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="d9222-200">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="d9222-201">En el caso de una experiencia de webassembler extraordinaria, ejecute los siguientes comandos en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="d9222-201">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="d9222-202">Para obtener una experiencia de servidor extraordinaria, ejecute los siguientes comandos en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="d9222-202">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="d9222-203">Para obtener más información sobre los dos modelos de hospedaje más increíbles, el *servidor* y el *webensamblado*increíble, consulte <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d9222-203">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="d9222-204">En un navegador, vaya a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="d9222-204">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

<span data-ttu-id="d9222-205">Hay varias páginas disponibles en las pestañas de la barra lateral:</span><span class="sxs-lookup"><span data-stu-id="d9222-205">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="d9222-206">Página principal</span><span class="sxs-lookup"><span data-stu-id="d9222-206">Home</span></span>
* <span data-ttu-id="d9222-207">Contador</span><span class="sxs-lookup"><span data-stu-id="d9222-207">Counter</span></span>
* <span data-ttu-id="d9222-208">Capturar datos</span><span class="sxs-lookup"><span data-stu-id="d9222-208">Fetch data</span></span>

<span data-ttu-id="d9222-209">En la página Contador, seleccione el botón **Click me** para aumentar el contador sin una actualización de página.</span><span class="sxs-lookup"><span data-stu-id="d9222-209">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="d9222-210">El incremento de un contador en una página web normalmente requiere la escritura de JavaScript, pero con el C#aumento de la carga que puede usar.</span><span class="sxs-lookup"><span data-stu-id="d9222-210">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="d9222-211">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="d9222-211">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="d9222-212">Una solicitud de `/counter` en el explorador, tal como la especifica la Directiva `@page` en la parte superior, hace que el componente `Counter` represente su contenido.</span><span class="sxs-lookup"><span data-stu-id="d9222-212">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="d9222-213">Los componentes se representan en una representación en memoria del árbol de representación que se puede usar para actualizar la interfaz de usuario de una forma flexible y eficaz.</span><span class="sxs-lookup"><span data-stu-id="d9222-213">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="d9222-214">Cada vez que se selecciona el botón **click me** :</span><span class="sxs-lookup"><span data-stu-id="d9222-214">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="d9222-215">Se desencadena el evento `onclick`.</span><span class="sxs-lookup"><span data-stu-id="d9222-215">The `onclick` event is fired.</span></span>
* <span data-ttu-id="d9222-216">Se llama al método `IncrementCount` .</span><span class="sxs-lookup"><span data-stu-id="d9222-216">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="d9222-217">Se incrementa el `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="d9222-217">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="d9222-218">El componente se representará de nuevo.</span><span class="sxs-lookup"><span data-stu-id="d9222-218">The component is rendered again.</span></span>

<span data-ttu-id="d9222-219">El tiempo de ejecución compara el nuevo contenido con el contenido anterior y solo aplica el contenido cambiado al Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="d9222-219">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="d9222-220">Agregar un componente a otro componente mediante la sintaxis HTML.</span><span class="sxs-lookup"><span data-stu-id="d9222-220">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="d9222-221">Por ejemplo, agregue el componente `Counter` a la Página principal de la aplicación agregando un elemento `<Counter />` al componente `Index`.</span><span class="sxs-lookup"><span data-stu-id="d9222-221">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="d9222-222">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="d9222-222">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="d9222-223">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d9222-223">Run the app.</span></span> <span data-ttu-id="d9222-224">La Página principal tiene su propio contador proporcionado por el componente `Counter`.</span><span class="sxs-lookup"><span data-stu-id="d9222-224">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="d9222-225">Los parámetros de componente se especifican mediante atributos o [contenido secundario](xref:blazor/components#child-content), que permiten establecer propiedades en el componente secundario.</span><span class="sxs-lookup"><span data-stu-id="d9222-225">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="d9222-226">Para agregar un parámetro al componente `Counter`, actualice el bloque `@code` del componente:</span><span class="sxs-lookup"><span data-stu-id="d9222-226">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="d9222-227">Agregue una propiedad pública para `IncrementAmount` con un atributo de `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="d9222-227">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="d9222-228">Cambie el método `IncrementCount` para usar `IncrementAmount` al aumentar el valor de `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="d9222-228">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="d9222-229">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="d9222-229">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="d9222-230">Especifique el `IncrementAmount` en el elemento `<Counter>` del componente `Index` mediante un atributo.</span><span class="sxs-lookup"><span data-stu-id="d9222-230">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="d9222-231">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="d9222-231">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="d9222-232">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d9222-232">Run the app.</span></span> <span data-ttu-id="d9222-233">El componente `Index` tiene su propio contador que se incrementa en diez cada vez que se selecciona el botón **click me** .</span><span class="sxs-lookup"><span data-stu-id="d9222-233">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="d9222-234">El componente `Counter` (*Counter. Razor*) en `/counter` continúa aumentando en uno.</span><span class="sxs-lookup"><span data-stu-id="d9222-234">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d9222-235">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="d9222-235">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="d9222-236">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="d9222-236">Additional resources</span></span>

* <xref:signalr/introduction>
