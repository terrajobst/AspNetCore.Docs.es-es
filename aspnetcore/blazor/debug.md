---
title: Depurar ASP.NET Core Blazor
author: guardrex
description: Obtenga información sobre cómo depurar aplicaciones Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/16/2020
no-loc:
- Blazor
- SignalR
uid: blazor/debug
ms.openlocfilehash: 1b0035af48b82807a6ae14835a41a1ecbef06bb6
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2020
ms.locfileid: "76159994"
---
# <a name="debug-aspnet-core-opno-locblazor"></a><span data-ttu-id="1f663-103">Depurar ASP.NET Core [!OP.NO-LOC(Blazor)]</span><span class="sxs-lookup"><span data-stu-id="1f663-103">Debug ASP.NET Core [!OP.NO-LOC(Blazor)]</span></span>

[<span data-ttu-id="1f663-104">Daniel Roth</span><span class="sxs-lookup"><span data-stu-id="1f663-104">Daniel Roth</span></span>](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="1f663-105">Existe compatibilidad *temprana* para depurar [!OP.NO-LOC(Blazor)] webassembly con las herramientas de desarrollo del explorador en exploradores basados en cromo (Chrome/ Microsoft Edge).</span><span class="sxs-lookup"><span data-stu-id="1f663-105">*Early* support exists for debugging [!OP.NO-LOC(Blazor)] WebAssembly using the browser dev tools in Chromium-based browsers (Chrome/Edge).</span></span> <span data-ttu-id="1f663-106">El trabajo está en curso para:</span><span class="sxs-lookup"><span data-stu-id="1f663-106">Work is in progress to:</span></span>

* <span data-ttu-id="1f663-107">Habilite totalmente la depuración en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1f663-107">Fully enable debugging in Visual Studio.</span></span>
* <span data-ttu-id="1f663-108">Habilite la depuración en Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="1f663-108">Enable debugging in Visual Studio Code.</span></span>

<span data-ttu-id="1f663-109">Las funcionalidades del depurador están limitadas.</span><span class="sxs-lookup"><span data-stu-id="1f663-109">Debugger capabilities are limited.</span></span> <span data-ttu-id="1f663-110">Entre los escenarios disponibles se incluyen:</span><span class="sxs-lookup"><span data-stu-id="1f663-110">Available scenarios include:</span></span>

* <span data-ttu-id="1f663-111">Establecer y quitar puntos de interrupción.</span><span class="sxs-lookup"><span data-stu-id="1f663-111">Set and remove breakpoints.</span></span>
* <span data-ttu-id="1f663-112">Un solo paso (`F10`) a través de la ejecución de código de código o de reanudación (`F8`).</span><span class="sxs-lookup"><span data-stu-id="1f663-112">Single-step (`F10`) through the code or resume (`F8`) code execution.</span></span>
* <span data-ttu-id="1f663-113">En la pantalla *variables locales* , observe los valores de las variables locales de tipo `int`, `string`y `bool`.</span><span class="sxs-lookup"><span data-stu-id="1f663-113">In the *Locals* display, observe the values of any local variables of type `int`, `string`, and `bool`.</span></span>
* <span data-ttu-id="1f663-114">Vea la pila de llamadas, incluidas las cadenas de llamadas que van desde JavaScript hasta .NET y desde .NET a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1f663-114">See the call stack, including call chains that go from JavaScript into .NET and from .NET to JavaScript.</span></span>

<span data-ttu-id="1f663-115">*No*se puede:</span><span class="sxs-lookup"><span data-stu-id="1f663-115">You *can't*:</span></span>

* <span data-ttu-id="1f663-116">Observe los valores de las variables locales que no son `int`, `string`o `bool`.</span><span class="sxs-lookup"><span data-stu-id="1f663-116">Observe the values of any locals that aren't an `int`, `string`, or `bool`.</span></span>
* <span data-ttu-id="1f663-117">Observe los valores de cualquier propiedad o campo de clase.</span><span class="sxs-lookup"><span data-stu-id="1f663-117">Observe the values of any class properties or fields.</span></span>
* <span data-ttu-id="1f663-118">Mantenga el mouse sobre las variables para ver sus valores.</span><span class="sxs-lookup"><span data-stu-id="1f663-118">Hover over variables to see their values.</span></span>
* <span data-ttu-id="1f663-119">Evaluar expresiones en la consola.</span><span class="sxs-lookup"><span data-stu-id="1f663-119">Evaluate expressions in the console.</span></span>
* <span data-ttu-id="1f663-120">Paso a través de las llamadas asincrónicas.</span><span class="sxs-lookup"><span data-stu-id="1f663-120">Step across async calls.</span></span>
* <span data-ttu-id="1f663-121">Realice la mayoría de los demás escenarios de depuración normales.</span><span class="sxs-lookup"><span data-stu-id="1f663-121">Perform most other ordinary debugging scenarios.</span></span>

<span data-ttu-id="1f663-122">El desarrollo de otros escenarios de depuración es un enfoque continuo del equipo de ingeniería.</span><span class="sxs-lookup"><span data-stu-id="1f663-122">Development of further debugging scenarios is an on-going focus of the engineering team.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1f663-123">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="1f663-123">Prerequisites</span></span>

<span data-ttu-id="1f663-124">La depuración requiere cualquiera de los siguientes exploradores:</span><span class="sxs-lookup"><span data-stu-id="1f663-124">Debugging requires either of the following browsers:</span></span>

* <span data-ttu-id="1f663-125">Google Chrome (versión 70 o posterior)</span><span class="sxs-lookup"><span data-stu-id="1f663-125">Google Chrome (version 70 or later)</span></span>
* <span data-ttu-id="1f663-126">Microsoft Edge Preview ([canal de desarrollo perimetral](https://www.microsoftedgeinsider.com))</span><span class="sxs-lookup"><span data-stu-id="1f663-126">Microsoft Edge preview ([Edge Dev Channel](https://www.microsoftedgeinsider.com))</span></span>

## <a name="procedure"></a><span data-ttu-id="1f663-127">Procedimiento</span><span class="sxs-lookup"><span data-stu-id="1f663-127">Procedure</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1f663-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1f663-128">Visual Studio</span></span>](#tab/visual-studio)

> [!WARNING]
> <span data-ttu-id="1f663-129">La compatibilidad con la depuración en Visual Studio se encuentra en una fase temprana del desarrollo.</span><span class="sxs-lookup"><span data-stu-id="1f663-129">Debugging support in Visual Studio is at an early stage of development.</span></span> <span data-ttu-id="1f663-130">La depuración **F5** no se admite actualmente.</span><span class="sxs-lookup"><span data-stu-id="1f663-130">**F5** debugging isn't currently supported.</span></span>

1. <span data-ttu-id="1f663-131">Ejecute una aplicación [!OP.NO-LOC(Blazor)] webassembly en `Debug` configuración sin depurar (**Ctrl**+**F5** en lugar de **F5**).</span><span class="sxs-lookup"><span data-stu-id="1f663-131">Run a [!OP.NO-LOC(Blazor)] WebAssembly app in `Debug` configuration without debugging (**Ctrl**+**F5** instead of **F5**).</span></span>
1. <span data-ttu-id="1f663-132">Abra las propiedades de depuración de la aplicación (última entrada en el menú **depurar** ) y copie la **dirección URL**de la aplicación http.</span><span class="sxs-lookup"><span data-stu-id="1f663-132">Open the Debug properties of the app (last entry in the **Debug** menu) and copy the HTTP **App URL**.</span></span> <span data-ttu-id="1f663-133">Vaya a la dirección HTTP (no a la dirección HTTPS) de la aplicación mediante un explorador basado en cromo (versión beta o Chrome de Microsoft Edge).</span><span class="sxs-lookup"><span data-stu-id="1f663-133">Browse to the HTTP address (not the HTTPS address) of the app using a Chromium-based browser (Edge Beta or Chrome).</span></span>
1. <span data-ttu-id="1f663-134">Coloque el foco de teclado en la aplicación en la ventana del explorador, no en el panel herramientas de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="1f663-134">Place the keyboard focus on the app in the browser window, not the developer tools panel.</span></span> <span data-ttu-id="1f663-135">Es mejor mantener el panel herramientas de desarrollo cerrado para este procedimiento.</span><span class="sxs-lookup"><span data-stu-id="1f663-135">It's best to keep the developer tools panel closed for this procedure.</span></span> <span data-ttu-id="1f663-136">Una vez iniciada la depuración, puede volver a abrir el panel herramientas de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="1f663-136">After debugging has started, you can re-open the developer tools panel.</span></span>
1. <span data-ttu-id="1f663-137">Seleccione el siguiente método abreviado de teclado específico de [!OP.NO-LOC(Blazor)]:</span><span class="sxs-lookup"><span data-stu-id="1f663-137">Select the following [!OP.NO-LOC(Blazor)]-specific keyboard shortcut:</span></span>

   * <span data-ttu-id="1f663-138">`Shift+Alt+D` en Windows</span><span class="sxs-lookup"><span data-stu-id="1f663-138">`Shift+Alt+D` on Windows</span></span>
   * <span data-ttu-id="1f663-139">`Shift+Cmd+D` en macOS</span><span class="sxs-lookup"><span data-stu-id="1f663-139">`Shift+Cmd+D` on macOS</span></span>

   <span data-ttu-id="1f663-140">Si recibe la **pestaña no se puede encontrar el explorador depurable**, vea [Habilitar depuración remota](#enable-remote-debugging).</span><span class="sxs-lookup"><span data-stu-id="1f663-140">If you receive the **Unable to find debuggable browser tab**, see [Enable remote debugging](#enable-remote-debugging).</span></span>
   
   <span data-ttu-id="1f663-141">Después de habilitar la depuración remota:</span><span class="sxs-lookup"><span data-stu-id="1f663-141">After enabling remote debugging:</span></span>
   
   <span data-ttu-id="1f663-142">1 \.</span><span class="sxs-lookup"><span data-stu-id="1f663-142">1\.</span></span> <span data-ttu-id="1f663-143">Se abre una nueva ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="1f663-143">A new browser window opens.</span></span> <span data-ttu-id="1f663-144">Cierre la ventana anterior.</span><span class="sxs-lookup"><span data-stu-id="1f663-144">Close the prior window.</span></span>

   <span data-ttu-id="1f663-145">2 \.</span><span class="sxs-lookup"><span data-stu-id="1f663-145">2\.</span></span> <span data-ttu-id="1f663-146">Coloque el foco de teclado en la aplicación en la ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="1f663-146">Place the keyboard focus on the app in the browser window.</span></span>

   <span data-ttu-id="1f663-147">3 \.</span><span class="sxs-lookup"><span data-stu-id="1f663-147">3\.</span></span> <span data-ttu-id="1f663-148">Seleccione el método abreviado de teclado específico de [!OP.NO-LOC(Blazor)]en la ventana nuevo explorador: `Shift+Alt+D` en Windows o `Shift+Cmd+D` en macOS.</span><span class="sxs-lookup"><span data-stu-id="1f663-148">Select the [!OP.NO-LOC(Blazor)]-specific keyboard shortcut in the new browser window: `Shift+Alt+D` on Windows or `Shift+Cmd+D` on macOS.</span></span>

   <span data-ttu-id="1f663-149">4 \.</span><span class="sxs-lookup"><span data-stu-id="1f663-149">4\.</span></span> <span data-ttu-id="1f663-150">Se abre la pestaña **DevTools** en el explorador.</span><span class="sxs-lookup"><span data-stu-id="1f663-150">The **DevTools** tab opens in the browser.</span></span> <span data-ttu-id="1f663-151">**Reseleccione la pestaña de la aplicación en la ventana del explorador.**</span><span class="sxs-lookup"><span data-stu-id="1f663-151">**Reselect the app's tab in the browser window.**</span></span>

   <span data-ttu-id="1f663-152">Para adjuntar la aplicación a Visual Studio, consulte la sección [asociar al proceso en Visual Studio](#attach-to-process-in-visual-studio) .</span><span class="sxs-lookup"><span data-stu-id="1f663-152">To attach the app to Visual Studio, see the [Attach to process in Visual Studio](#attach-to-process-in-visual-studio) section.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1f663-153">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="1f663-153">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="1f663-154">Ejecute una aplicación [!OP.NO-LOC(Blazor)] webassembly en `Debug` configuración; para ello, pase la opción `--configuration Debug` al comando [dotnet Run](/dotnet/core/tools/dotnet-run) : `dotnet run --configuration Debug`.</span><span class="sxs-lookup"><span data-stu-id="1f663-154">Run a [!OP.NO-LOC(Blazor)] WebAssembly app in `Debug` configuration by passing the `--configuration Debug` option to the [dotnet run](/dotnet/core/tools/dotnet-run) command: `dotnet run --configuration Debug`.</span></span>
1. <span data-ttu-id="1f663-155">Vaya a la aplicación en la dirección URL HTTP que se muestra en la ventana del shell.</span><span class="sxs-lookup"><span data-stu-id="1f663-155">Navigate to the app at the HTTP URL shown in the shell's window.</span></span>
1. <span data-ttu-id="1f663-156">Coloque el foco de teclado en la aplicación, no en el panel herramientas de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="1f663-156">Place the keyboard focus on the app, not the developer tools panel.</span></span> <span data-ttu-id="1f663-157">Es mejor mantener el panel herramientas de desarrollo cerrado para este procedimiento.</span><span class="sxs-lookup"><span data-stu-id="1f663-157">It's best to keep the developer tools panel closed for this procedure.</span></span> <span data-ttu-id="1f663-158">Una vez iniciada la depuración, puede volver a abrir el panel herramientas de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="1f663-158">After debugging has started, you can re-open the developer tools panel.</span></span>
1. <span data-ttu-id="1f663-159">Seleccione el siguiente método abreviado de teclado específico de [!OP.NO-LOC(Blazor)]:</span><span class="sxs-lookup"><span data-stu-id="1f663-159">Select the following [!OP.NO-LOC(Blazor)]-specific keyboard shortcut:</span></span>

   * <span data-ttu-id="1f663-160">`Shift+Alt+D` en Windows</span><span class="sxs-lookup"><span data-stu-id="1f663-160">`Shift+Alt+D` on Windows</span></span>
   * <span data-ttu-id="1f663-161">`Shift+Cmd+D` en macOS</span><span class="sxs-lookup"><span data-stu-id="1f663-161">`Shift+Cmd+D` on macOS</span></span>

   <span data-ttu-id="1f663-162">Si recibe la **pestaña no se puede encontrar el explorador depurable**, vea [Habilitar depuración remota](#enable-remote-debugging).</span><span class="sxs-lookup"><span data-stu-id="1f663-162">If you receive the **Unable to find debuggable browser tab**, see [Enable remote debugging](#enable-remote-debugging).</span></span>
   
   <span data-ttu-id="1f663-163">Después de habilitar la depuración remota:</span><span class="sxs-lookup"><span data-stu-id="1f663-163">After enabling remote debugging:</span></span>
   
   <span data-ttu-id="1f663-164">1 \.</span><span class="sxs-lookup"><span data-stu-id="1f663-164">1\.</span></span> <span data-ttu-id="1f663-165">Se abre una nueva ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="1f663-165">A new browser window opens.</span></span> <span data-ttu-id="1f663-166">Cierre la ventana anterior.</span><span class="sxs-lookup"><span data-stu-id="1f663-166">Close the prior window.</span></span>

   <span data-ttu-id="1f663-167">2 \.</span><span class="sxs-lookup"><span data-stu-id="1f663-167">2\.</span></span> <span data-ttu-id="1f663-168">Coloque el foco de teclado en la aplicación en la ventana del explorador, no en el panel herramientas de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="1f663-168">Place the keyboard focus on the app in the browser window, not the developer tools panel.</span></span>

   <span data-ttu-id="1f663-169">3 \.</span><span class="sxs-lookup"><span data-stu-id="1f663-169">3\.</span></span> <span data-ttu-id="1f663-170">Seleccione el método abreviado de teclado específico de [!OP.NO-LOC(Blazor)]en la ventana nuevo explorador: `Shift+Alt+D` en Windows o `Shift+Cmd+D` en macOS.</span><span class="sxs-lookup"><span data-stu-id="1f663-170">Select the [!OP.NO-LOC(Blazor)]-specific keyboard shortcut in the new browser window: `Shift+Alt+D` on Windows or `Shift+Cmd+D` on macOS.</span></span>

---

## <a name="enable-remote-debugging"></a><span data-ttu-id="1f663-171">Habilitar la depuración remota</span><span class="sxs-lookup"><span data-stu-id="1f663-171">Enable remote debugging</span></span>

<span data-ttu-id="1f663-172">Si la depuración remota está deshabilitada, Chrome genera una página de error **no se puede encontrar la pestaña del explorador depurable** .</span><span class="sxs-lookup"><span data-stu-id="1f663-172">If remote debugging is disabled, an **Unable to find debuggable browser tab** error page is generated by Chrome.</span></span> <span data-ttu-id="1f663-173">La página de error contiene instrucciones para ejecutar Chrome con el puerto de depuración abierto para que el Blazor proxy de depuración pueda conectarse a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1f663-173">The error page contains instructions for running Chrome with the debugging port open so that the Blazor debugging proxy can connect to the app.</span></span> <span data-ttu-id="1f663-174">*Cierre todas las instancias de Chrome* y reinicie Chrome tal y como se indica.</span><span class="sxs-lookup"><span data-stu-id="1f663-174">*Close all Chrome instances* and restart Chrome as instructed.</span></span>

## <a name="debug-the-app"></a><span data-ttu-id="1f663-175">Depurar la aplicación</span><span class="sxs-lookup"><span data-stu-id="1f663-175">Debug the app</span></span>

<span data-ttu-id="1f663-176">Una vez que Chrome se ejecuta con la depuración remota habilitada, el método abreviado de teclado de depuración abre una nueva pestaña del depurador. Después de un momento, la pestaña **orígenes** muestra una lista de los ensamblados .net en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1f663-176">Once Chrome is running with remote debugging enabled, the debugging keyboard shortcut opens a new debugger tab. After a moment, the **Sources** tab shows a list of the .NET assemblies in the app.</span></span> <span data-ttu-id="1f663-177">Expanda cada ensamblado y busque los archivos de origen *. cs*/ *. Razor* disponibles para la depuración.</span><span class="sxs-lookup"><span data-stu-id="1f663-177">Expand each assembly and find the *.cs*/*.razor* source files available for debugging.</span></span> <span data-ttu-id="1f663-178">Establezca puntos de interrupción, vuelva a la pestaña de la aplicación y los puntos de interrupción se alcanzarán cuando se ejecute el código.</span><span class="sxs-lookup"><span data-stu-id="1f663-178">Set breakpoints, switch back to the app's tab, and the breakpoints are hit when the code executes.</span></span> <span data-ttu-id="1f663-179">Una vez que se visita un punto de interrupción, se realiza un solo paso (`F10`) a través de la ejecución de código de código o de reanudación (`F8`) con normalidad.</span><span class="sxs-lookup"><span data-stu-id="1f663-179">After a breakpoint is hit, single-step (`F10`) through the code or resume (`F8`) code execution normally.</span></span>

Blazor<span data-ttu-id="1f663-180"> proporciona un proxy de depuración que implementa el [Protocolo Chrome DevTools](https://chromedevtools.github.io/devtools-protocol/) y aumenta el protocolo con. Información específica de la red.</span><span class="sxs-lookup"><span data-stu-id="1f663-180"> provides a debugging proxy that implements the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) and augments the protocol with .NET-specific information.</span></span> <span data-ttu-id="1f663-181">Cuando se presiona el método abreviado de teclado de depuración, Blazor señala el DevTools de cromo en el proxy.</span><span class="sxs-lookup"><span data-stu-id="1f663-181">When debugging keyboard shortcut is pressed, Blazor points the Chrome DevTools at the proxy.</span></span> <span data-ttu-id="1f663-182">El proxy se conecta a la ventana del explorador que busca depurar (por lo tanto, la necesidad de habilitar la depuración remota).</span><span class="sxs-lookup"><span data-stu-id="1f663-182">The proxy connects to the browser window you're seeking to debug (hence the need to enable remote debugging).</span></span>

## <a name="attach-to-process-in-visual-studio"></a><span data-ttu-id="1f663-183">Asociar al proceso en Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1f663-183">Attach to process in Visual Studio</span></span>

<span data-ttu-id="1f663-184">Adjuntar al proceso de la aplicación en Visual Studio es un escenario de depuración *temporal* para Blazor webassembly mientras la depuración **F5** está en desarrollo.</span><span class="sxs-lookup"><span data-stu-id="1f663-184">Attaching to the app's process in Visual Studio is a *temporary* debugging scenario for Blazor WebAssembly while **F5** debugging is in development.</span></span>

<span data-ttu-id="1f663-185">Para asociar el proceso de la aplicación en ejecución a Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="1f663-185">To attach the running app's process to Visual Studio:</span></span>

1. <span data-ttu-id="1f663-186">En Visual Studio, seleccione **Depurar** > **adjuntar al proceso**.</span><span class="sxs-lookup"><span data-stu-id="1f663-186">In Visual Studio, select **Debug** > **Attach to Process**.</span></span>
1. <span data-ttu-id="1f663-187">Para el **tipo de conexión**, seleccione **Protocolo de WebSocket DevTools de Chrome (sin autenticación)** .</span><span class="sxs-lookup"><span data-stu-id="1f663-187">For the **Connection type**, select **Chrome devtools protocol websocket (no authentication)**.</span></span>
1. <span data-ttu-id="1f663-188">En destino de la **conexión**, pegue la dirección http (no la dirección https) de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1f663-188">For the **Connection target**, paste in the HTTP address (not the HTTPS address) of the app.</span></span>
1. <span data-ttu-id="1f663-189">Seleccione **Actualizar** para actualizar las entradas en **procesos disponibles**.</span><span class="sxs-lookup"><span data-stu-id="1f663-189">Select **Refresh** to refresh the entries under **Available processes**.</span></span>
1. <span data-ttu-id="1f663-190">Seleccione el proceso del explorador para depurar y seleccione **adjuntar**.</span><span class="sxs-lookup"><span data-stu-id="1f663-190">Select the browser process to debug and select **Attach**.</span></span>
1. <span data-ttu-id="1f663-191">En el cuadro de diálogo **Seleccionar tipo de código** , seleccione el tipo de código para el explorador específico al que se va a asociar (Microsoft Edge o Chrome) y, después, seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="1f663-191">In the **Select Code Type** dialog, select the code type for the specific browser you're attaching to (Edge or Chrome) and then select **OK**.</span></span>

## <a name="browser-source-maps"></a><span data-ttu-id="1f663-192">Mapas de origen del explorador</span><span class="sxs-lookup"><span data-stu-id="1f663-192">Browser source maps</span></span>

<span data-ttu-id="1f663-193">Los mapas de origen del explorador permiten al explorador volver a asignar los archivos compilados a sus archivos de código fuente originales y se suelen usar para la depuración del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="1f663-193">Browser source maps allow the browser to map compiled files back to their original source files and are commonly used for client-side debugging.</span></span> <span data-ttu-id="1f663-194">Sin embargo, Blazor actualmente no C# se asigna directamente a JavaScript/WASM.</span><span class="sxs-lookup"><span data-stu-id="1f663-194">However, Blazor doesn't currently map C# directly to JavaScript/WASM.</span></span> <span data-ttu-id="1f663-195">En su lugar, Blazor realiza la interpretación de IL en el explorador, por lo que los mapas de origen no son relevantes.</span><span class="sxs-lookup"><span data-stu-id="1f663-195">Instead, Blazor does IL interpretation within the browser, so source maps aren't relevant.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="1f663-196">Solucionar problemas</span><span class="sxs-lookup"><span data-stu-id="1f663-196">Troubleshoot</span></span>

<span data-ttu-id="1f663-197">Si se encuentra con errores, la sugerencia siguiente puede ser útil:</span><span class="sxs-lookup"><span data-stu-id="1f663-197">If you're running into errors, the following tip may help:</span></span>

<span data-ttu-id="1f663-198">En la pestaña **depurador** , abra las herramientas de desarrollo en el explorador.</span><span class="sxs-lookup"><span data-stu-id="1f663-198">In the **Debugger** tab, open the developer tools in your browser.</span></span> <span data-ttu-id="1f663-199">En la consola de, ejecute `localStorage.clear()` para quitar los puntos de interrupción.</span><span class="sxs-lookup"><span data-stu-id="1f663-199">In the console, execute `localStorage.clear()` to remove any breakpoints.</span></span>
