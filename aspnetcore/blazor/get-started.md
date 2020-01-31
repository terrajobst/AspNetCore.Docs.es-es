---
title: Introducción a ASP.NET Core Blazor
author: guardrex
description: Para empezar a trabajar con Blazor, cree una aplicación Blazor con las herramientas que prefiera.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/28/2019
no-loc:
- Blazor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: bd33d874b3d6122f2ab820e9b147b0e62ba03a58
ms.sourcegitcommit: fe41cff0b99f3920b727286944e5b652ca301640
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76869585"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="ed7a7-103">Introducción a ASP.NET Core extraordinarias</span><span class="sxs-lookup"><span data-stu-id="ed7a7-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="ed7a7-104">Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ed7a7-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="ed7a7-105">Introducción a más increíble:</span><span class="sxs-lookup"><span data-stu-id="ed7a7-105">Get started with Blazor:</span></span>

1. <span data-ttu-id="ed7a7-106">Instale el [SDK de .net Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="ed7a7-106">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="ed7a7-107">Opcionalmente, instale la plantilla de [Webassembler de increíbles](xref:blazor/hosting-models#blazor-webassembly) :</span><span class="sxs-lookup"><span data-stu-id="ed7a7-107">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="ed7a7-108">Instale el [SDK de .net Core 3,1 o posterior (versión preliminar)](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="ed7a7-108">Install the [.NET Core 3.1 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="ed7a7-109">Ejecute el siguiente comando en un shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-109">Run the following command in a command shell.</span></span> <span data-ttu-id="ed7a7-110">El paquete [Microsoft. AspNetCore. extraordinarias. templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) tiene una versión preliminar, mientras que el webassembly de extraordinarias está en versión preliminar.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-110">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.2.0-preview1.20073.1
   ```

1. <span data-ttu-id="ed7a7-111">Siga las instrucciones para su elección de herramientas:</span><span class="sxs-lookup"><span data-stu-id="ed7a7-111">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ed7a7-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ed7a7-112">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="ed7a7-113">1 \.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-113">1\.</span></span> <span data-ttu-id="ed7a7-114">Instale [Visual Studio 2019 versión 16,4 o posterior](https://visualstudio.microsoft.com/vs/preview/) con la carga de trabajo de **desarrollo web y ASP.net** .</span><span class="sxs-lookup"><span data-stu-id="ed7a7-114">Install [Visual Studio 2019 version 16.4 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="ed7a7-115">2 \.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-115">2\.</span></span> <span data-ttu-id="ed7a7-116">Cree un nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-116">Create a new project.</span></span>

   <span data-ttu-id="ed7a7-117">3 \.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-117">3\.</span></span> <span data-ttu-id="ed7a7-118">Seleccione **aplicación extraordinaria**.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-118">Select **Blazor App**.</span></span> <span data-ttu-id="ed7a7-119">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-119">Select **Next**.</span></span>

   <span data-ttu-id="ed7a7-120">4 \.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-120">4\.</span></span> <span data-ttu-id="ed7a7-121">Proporcione un nombre para el proyecto en el campo **Nombre del proyecto** o acepte el predeterminado.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-121">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="ed7a7-122">Confirme que la entrada de **Ubicación** es correcta o proporcione una ubicación para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-122">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="ed7a7-123">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-123">Select **Create**.</span></span>

   <span data-ttu-id="ed7a7-124">5 \.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-124">5\.</span></span> <span data-ttu-id="ed7a7-125">Para disfrutar de una experiencia de webassembly increíblemente rápida, elija la plantilla de **aplicación de Webassemble más brillante** .</span><span class="sxs-lookup"><span data-stu-id="ed7a7-125">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="ed7a7-126">Para obtener una experiencia de servidor increíblemente rápida, elija la plantilla de **aplicación de servidor de extraordinarias** .</span><span class="sxs-lookup"><span data-stu-id="ed7a7-126">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="ed7a7-127">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-127">Select **Create**.</span></span> <span data-ttu-id="ed7a7-128">Para obtener más información sobre los dos modelos de hospedaje más increíbles, el *servidor* y el *webensamblado*increíble, consulte <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-128">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="ed7a7-129">6 \.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-129">6\.</span></span> <span data-ttu-id="ed7a7-130">Presione **Ctrl**+**F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-130">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="ed7a7-131">Si ha instalado la extensión de Visual Studio de extraordinarias para una versión preliminar anterior de ASP.NET Core extraordinaria (versión preliminar 6 o anterior), puede desinstalar la extensión.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-131">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="ed7a7-132">Instalar las plantillas de extraordinarias en un shell de comandos ahora es suficiente para mostrar las plantillas en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-132">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ed7a7-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ed7a7-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="ed7a7-134">1 \.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-134">1\.</span></span> <span data-ttu-id="ed7a7-135">Instale [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="ed7a7-135">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="ed7a7-136">2 \.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-136">2\.</span></span> <span data-ttu-id="ed7a7-137">Instale la [ C# extensión de Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)más reciente.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-137">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="ed7a7-138">3 \.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-138">3\.</span></span> <span data-ttu-id="ed7a7-139">En el caso de una experiencia de webassembler extraordinaria, ejecute el siguiente comando en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="ed7a7-139">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="ed7a7-140">Para obtener una experiencia de servidor extraordinaria, ejecute el siguiente comando en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="ed7a7-140">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="ed7a7-141">Para obtener más información sobre los dos modelos de hospedaje más increíbles, el *servidor* y el *webensamblado*increíble, consulte <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-141">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="ed7a7-142">4 \.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-142">4\.</span></span> <span data-ttu-id="ed7a7-143">Abra la carpeta *WebApplication1* en Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-143">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="ed7a7-144">5 \.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-144">5\.</span></span> <span data-ttu-id="ed7a7-145">En el caso de un proyecto de servidor más rápido, el IDE solicita que agregue recursos para compilar y depurar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-145">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="ed7a7-146">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-146">Select **Yes**.</span></span>

   <span data-ttu-id="ed7a7-147">6 \.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-147">6\.</span></span> <span data-ttu-id="ed7a7-148">Si usa una aplicación de servidor más brillante, ejecute la aplicación con el depurador de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-148">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="ed7a7-149">Si usa una aplicación de webassembly extraordinaria, ejecute `dotnet run` desde la carpeta de proyecto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-149">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="ed7a7-150">7 \.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-150">7\.</span></span> <span data-ttu-id="ed7a7-151">En un navegador, vaya a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-151">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ed7a7-152">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="ed7a7-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="ed7a7-153">1 \.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-153">1\.</span></span> <span data-ttu-id="ed7a7-154">Instale [Visual Studio para Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="ed7a7-154">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

   <span data-ttu-id="ed7a7-155">2 \.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-155">2\.</span></span> <span data-ttu-id="ed7a7-156">Seleccione **archivo** > **nueva solución** o cree un **nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-156">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="ed7a7-157">3 \.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-157">3\.</span></span> <span data-ttu-id="ed7a7-158">En la barra lateral, seleccione **.net Core** > **aplicación**.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-158">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="ed7a7-159">4 \.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-159">4\.</span></span> <span data-ttu-id="ed7a7-160">Seleccione la plantilla **aplicación de servidor increíbles** .</span><span class="sxs-lookup"><span data-stu-id="ed7a7-160">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="ed7a7-161">En este momento, solo está disponible en Visual Studio para Mac la plantilla de servidor de extraordinarias.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-161">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="ed7a7-162">Para disfrutar de una experiencia de webassembly extraordinaria, siga las instrucciones de la pestaña **CLI de .net Core** . Después de seleccionar la plantilla de servidor de extraordinarias, seleccione **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-162">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="ed7a7-163">Para obtener más información sobre los dos modelos de hospedaje más increíbles, el *servidor* y el *webensamblado*increíble, consulte <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-163">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="ed7a7-164">5 \.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-164">5\.</span></span> <span data-ttu-id="ed7a7-165">Establezca la **plataforma de destino** en **.net Core 3,1** y seleccione **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-165">Set the **Target Framework** to **.NET Core 3.1** and select **Next**.</span></span>

   <span data-ttu-id="ed7a7-166">6 \.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-166">6\.</span></span> <span data-ttu-id="ed7a7-167">En el campo **nombre del proyecto** , asigne un nombre a la aplicación `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-167">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="ed7a7-168">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-168">Select **Create**.</span></span>

   <span data-ttu-id="ed7a7-169">7 \.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-169">7\.</span></span> <span data-ttu-id="ed7a7-170">Seleccione **ejecutar** > **ejecutar sin depurar** para ejecutar la aplicación *sin el depurador*.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-170">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="ed7a7-171">Ejecute la aplicación con **iniciar depuración** para ejecutar la aplicación *con el depurador*.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-171">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

   <span data-ttu-id="ed7a7-172">Si aparece un mensaje que confía en el certificado de desarrollo, confíe en el certificado y continúe.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-172">If a prompt appears to trust the development certificate, trust the certificate and continue.</span></span>

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ed7a7-173">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="ed7a7-173">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="ed7a7-174">En el caso de una experiencia de webassembler extraordinaria, ejecute los siguientes comandos en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="ed7a7-174">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="ed7a7-175">Para obtener una experiencia de servidor extraordinaria, ejecute los siguientes comandos en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="ed7a7-175">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="ed7a7-176">Para obtener más información sobre los dos modelos de hospedaje más increíbles, el *servidor* y el *webensamblado*increíble, consulte <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-176">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="ed7a7-177">En un navegador, vaya a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-177">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

<span data-ttu-id="ed7a7-178">Hay varias páginas disponibles en las pestañas de la barra lateral:</span><span class="sxs-lookup"><span data-stu-id="ed7a7-178">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="ed7a7-179">Página de inicio de</span><span class="sxs-lookup"><span data-stu-id="ed7a7-179">Home</span></span>
* <span data-ttu-id="ed7a7-180">Contador</span><span class="sxs-lookup"><span data-stu-id="ed7a7-180">Counter</span></span>
* <span data-ttu-id="ed7a7-181">Capturar datos</span><span class="sxs-lookup"><span data-stu-id="ed7a7-181">Fetch data</span></span>

<span data-ttu-id="ed7a7-182">En la página Contador, seleccione el botón **Click me** para aumentar el contador sin una actualización de página.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-182">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="ed7a7-183">El incremento de un contador en una página web normalmente requiere la escritura de JavaScript, pero con C#Blazor puede usar.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-183">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="ed7a7-184">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="ed7a7-184">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="ed7a7-185">Una solicitud de `/counter` en el explorador, tal y como se especifica en la Directiva de `@page` en la parte superior, hace que el componente `Counter` represente su contenido.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-185">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="ed7a7-186">Los componentes se representan en una representación en memoria del árbol de representación que se puede usar para actualizar la interfaz de usuario de una forma flexible y eficaz.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-186">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="ed7a7-187">Cada vez que se selecciona el botón **click me** :</span><span class="sxs-lookup"><span data-stu-id="ed7a7-187">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="ed7a7-188">Se desencadena el evento `onclick`.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-188">The `onclick` event is fired.</span></span>
* <span data-ttu-id="ed7a7-189">Se llama al método `IncrementCount`.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-189">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="ed7a7-190">Se incrementa el `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-190">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="ed7a7-191">El componente se representará de nuevo.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-191">The component is rendered again.</span></span>

<span data-ttu-id="ed7a7-192">El tiempo de ejecución compara el nuevo contenido con el contenido anterior y solo aplica el contenido cambiado al Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="ed7a7-192">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="ed7a7-193">Agregar un componente a otro componente mediante la sintaxis HTML.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-193">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="ed7a7-194">Por ejemplo, agregue el componente `Counter` a la Página principal de la aplicación agregando un elemento `<Counter />` al componente `Index`.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-194">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="ed7a7-195">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="ed7a7-195">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="ed7a7-196">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-196">Run the app.</span></span> <span data-ttu-id="ed7a7-197">La Página principal tiene su propio contador proporcionado por el componente de `Counter`.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-197">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="ed7a7-198">Los parámetros de componente se especifican mediante atributos o [contenido secundario](xref:blazor/components#child-content), que permiten establecer propiedades en el componente secundario.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-198">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="ed7a7-199">Para agregar un parámetro al componente de `Counter`, actualice el bloque de `@code` del componente:</span><span class="sxs-lookup"><span data-stu-id="ed7a7-199">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="ed7a7-200">Agregue una propiedad pública para `IncrementAmount` con un atributo de `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-200">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="ed7a7-201">Cambie el método `IncrementCount` para usar `IncrementAmount` al aumentar el valor de `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-201">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="ed7a7-202">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="ed7a7-202">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="ed7a7-203">Especifique el `IncrementAmount` en el elemento `<Counter>` del componente de `Index` mediante un atributo.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-203">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="ed7a7-204">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="ed7a7-204">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="ed7a7-205">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-205">Run the app.</span></span> <span data-ttu-id="ed7a7-206">El componente de `Index` tiene su propio contador que se incrementa en diez cada vez que se selecciona el botón **click me** .</span><span class="sxs-lookup"><span data-stu-id="ed7a7-206">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="ed7a7-207">El componente de `Counter` (*Counter. Razor*) en `/counter` continúa aumentando en uno.</span><span class="sxs-lookup"><span data-stu-id="ed7a7-207">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ed7a7-208">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="ed7a7-208">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="ed7a7-209">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="ed7a7-209">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
