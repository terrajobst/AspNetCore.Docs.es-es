---
title: Depuración de Blazor en ASP.NET Core
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
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78648317"
---
# <a name="debug-aspnet-core-blazor"></a><span data-ttu-id="1e79b-103">Depuración de Blazor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1e79b-103">Debug ASP.NET Core Blazor</span></span>

[<span data-ttu-id="1e79b-104">Daniel Roth</span><span class="sxs-lookup"><span data-stu-id="1e79b-104">Daniel Roth</span></span>](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="1e79b-105">Existe *compatibilidad temprana* con la depuración de WebAssembly de Blazor mediante las herramientas de desarrollo del explorador en exploradores basados en Chromium (Chrome o Edge).</span><span class="sxs-lookup"><span data-stu-id="1e79b-105">*Early* support exists for debugging Blazor WebAssembly using the browser dev tools in Chromium-based browsers (Chrome/Edge).</span></span> <span data-ttu-id="1e79b-106">Hay trabajos en curso para:</span><span class="sxs-lookup"><span data-stu-id="1e79b-106">Work is in progress to:</span></span>

* <span data-ttu-id="1e79b-107">Habilitar la depuración de forma completa en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1e79b-107">Fully enable debugging in Visual Studio.</span></span>
* <span data-ttu-id="1e79b-108">Habilitar la depuración en Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="1e79b-108">Enable debugging in Visual Studio Code.</span></span>

<span data-ttu-id="1e79b-109">Las funcionalidades del depurador son limitadas.</span><span class="sxs-lookup"><span data-stu-id="1e79b-109">Debugger capabilities are limited.</span></span> <span data-ttu-id="1e79b-110">Entre los escenarios disponibles se incluyen los siguientes:</span><span class="sxs-lookup"><span data-stu-id="1e79b-110">Available scenarios include:</span></span>

* <span data-ttu-id="1e79b-111">Establecimiento y eliminación de puntos de interrupción.</span><span class="sxs-lookup"><span data-stu-id="1e79b-111">Set and remove breakpoints.</span></span>
* <span data-ttu-id="1e79b-112">Examen (`F10`) del código una vez o reanudación (`F8`) de la ejecución del código.</span><span class="sxs-lookup"><span data-stu-id="1e79b-112">Single-step (`F10`) through the code or resume (`F8`) code execution.</span></span>
* <span data-ttu-id="1e79b-113">En la pantalla *Variables locales*, observe los valores de las variables locales de tipo `int`, `string` y `bool`.</span><span class="sxs-lookup"><span data-stu-id="1e79b-113">In the *Locals* display, observe the values of any local variables of type `int`, `string`, and `bool`.</span></span>
* <span data-ttu-id="1e79b-114">Vea la pila de llamadas, incluidas las cadenas de llamadas que van desde JavaScript hasta .NET y desde .NET a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1e79b-114">See the call stack, including call chains that go from JavaScript into .NET and from .NET to JavaScript.</span></span>

<span data-ttu-id="1e79b-115">*No* puede:</span><span class="sxs-lookup"><span data-stu-id="1e79b-115">You *can't*:</span></span>

* <span data-ttu-id="1e79b-116">Observar los valores de las variables locales que no sean de tipo `int`, `string` o `bool`.</span><span class="sxs-lookup"><span data-stu-id="1e79b-116">Observe the values of any locals that aren't an `int`, `string`, or `bool`.</span></span>
* <span data-ttu-id="1e79b-117">Observar los valores de ninguna propiedad de clase o campos.</span><span class="sxs-lookup"><span data-stu-id="1e79b-117">Observe the values of any class properties or fields.</span></span>
* <span data-ttu-id="1e79b-118">Mantener el puntero sobre las variables para ver sus valores.</span><span class="sxs-lookup"><span data-stu-id="1e79b-118">Hover over variables to see their values.</span></span>
* <span data-ttu-id="1e79b-119">Evaluar expresiones en la consola.</span><span class="sxs-lookup"><span data-stu-id="1e79b-119">Evaluate expressions in the console.</span></span>
* <span data-ttu-id="1e79b-120">Ejecutar llamadas asincrónicas paso a paso.</span><span class="sxs-lookup"><span data-stu-id="1e79b-120">Step across async calls.</span></span>
* <span data-ttu-id="1e79b-121">Realizar la mayoría del resto de escenarios de depuración convencionales.</span><span class="sxs-lookup"><span data-stu-id="1e79b-121">Perform most other ordinary debugging scenarios.</span></span>

<span data-ttu-id="1e79b-122">El desarrollo de otros escenarios de depuración es un enfoque en curso del equipo de ingeniería.</span><span class="sxs-lookup"><span data-stu-id="1e79b-122">Development of further debugging scenarios is an on-going focus of the engineering team.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1e79b-123">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="1e79b-123">Prerequisites</span></span>

<span data-ttu-id="1e79b-124">La depuración requiere cualquiera de los exploradores siguientes:</span><span class="sxs-lookup"><span data-stu-id="1e79b-124">Debugging requires either of the following browsers:</span></span>

* <span data-ttu-id="1e79b-125">Google Chrome (versión 70 o posterior)</span><span class="sxs-lookup"><span data-stu-id="1e79b-125">Google Chrome (version 70 or later)</span></span>
* <span data-ttu-id="1e79b-126">Versión preliminar de Microsoft Edge ([canal de desarrollo de Edge](https://www.microsoftedgeinsider.com))</span><span class="sxs-lookup"><span data-stu-id="1e79b-126">Microsoft Edge preview ([Edge Dev Channel](https://www.microsoftedgeinsider.com))</span></span>

## <a name="procedure"></a><span data-ttu-id="1e79b-127">Procedimiento</span><span class="sxs-lookup"><span data-stu-id="1e79b-127">Procedure</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="1e79b-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1e79b-128">Visual Studio</span></span>](#tab/visual-studio)

> [!WARNING]
> <span data-ttu-id="1e79b-129">La compatibilidad con la depuración en Visual Studio se encuentra en una fase temprana de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="1e79b-129">Debugging support in Visual Studio is at an early stage of development.</span></span> <span data-ttu-id="1e79b-130">Actualmente no se admite la depuración con **F5**.</span><span class="sxs-lookup"><span data-stu-id="1e79b-130">**F5** debugging isn't currently supported.</span></span>

1. <span data-ttu-id="1e79b-131">Ejecute una aplicación WebAssembly de Blazor en la configuración `Debug` sin depurar (**Ctrl**+**F5** en lugar de **F5**).</span><span class="sxs-lookup"><span data-stu-id="1e79b-131">Run a Blazor WebAssembly app in `Debug` configuration without debugging (**Ctrl**+**F5** instead of **F5**).</span></span>
1. <span data-ttu-id="1e79b-132">Abra las propiedades de depuración de la aplicación (la última entrada del menú **Depurar**) y copie la **dirección URL de la aplicación** HTTP.</span><span class="sxs-lookup"><span data-stu-id="1e79b-132">Open the Debug properties of the app (last entry in the **Debug** menu) and copy the HTTP **App URL**.</span></span> <span data-ttu-id="1e79b-133">Vaya a la dirección HTTP (no a la dirección HTTPS) de la aplicación mediante un explorador basado en Chromium (Edge Beta o Chrome).</span><span class="sxs-lookup"><span data-stu-id="1e79b-133">Browse to the HTTP address (not the HTTPS address) of the app using a Chromium-based browser (Edge Beta or Chrome).</span></span>
1. <span data-ttu-id="1e79b-134">Coloque el foco del teclado en la aplicación en la ventana del explorador, no en el panel de herramientas para desarrolladores.</span><span class="sxs-lookup"><span data-stu-id="1e79b-134">Place the keyboard focus on the app in the browser window, not the developer tools panel.</span></span> <span data-ttu-id="1e79b-135">Para este procedimiento es recomendable mantener cerrado el panel de herramientas para desarrolladores.</span><span class="sxs-lookup"><span data-stu-id="1e79b-135">It's best to keep the developer tools panel closed for this procedure.</span></span> <span data-ttu-id="1e79b-136">Una vez iniciada la depuración, puede volver a abrirlo.</span><span class="sxs-lookup"><span data-stu-id="1e79b-136">After debugging has started, you can re-open the developer tools panel.</span></span>
1. <span data-ttu-id="1e79b-137">Seleccione el siguiente método abreviado de teclado específico de Blazor:</span><span class="sxs-lookup"><span data-stu-id="1e79b-137">Select the following Blazor-specific keyboard shortcut:</span></span>

   * <span data-ttu-id="1e79b-138">`Shift+Alt+D` en Windows</span><span class="sxs-lookup"><span data-stu-id="1e79b-138">`Shift+Alt+D` on Windows</span></span>
   * <span data-ttu-id="1e79b-139">`Shift+Cmd+D` en macOS</span><span class="sxs-lookup"><span data-stu-id="1e79b-139">`Shift+Cmd+D` on macOS</span></span>

   <span data-ttu-id="1e79b-140">Si recibe un mensaje en el que se indica que **no se puede encontrar la pestaña del explorador depurable**, vea [Habilitación de la depuración remota](#enable-remote-debugging).</span><span class="sxs-lookup"><span data-stu-id="1e79b-140">If you receive the **Unable to find debuggable browser tab**, see [Enable remote debugging](#enable-remote-debugging).</span></span>
   
   <span data-ttu-id="1e79b-141">Después de habilitar la depuración remota:</span><span class="sxs-lookup"><span data-stu-id="1e79b-141">After enabling remote debugging:</span></span>
   
   <span data-ttu-id="1e79b-142">1\.</span><span class="sxs-lookup"><span data-stu-id="1e79b-142">1\.</span></span> <span data-ttu-id="1e79b-143">Se abre una nueva ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="1e79b-143">A new browser window opens.</span></span> <span data-ttu-id="1e79b-144">Cierre la ventana anterior.</span><span class="sxs-lookup"><span data-stu-id="1e79b-144">Close the prior window.</span></span>

   <span data-ttu-id="1e79b-145">2\.</span><span class="sxs-lookup"><span data-stu-id="1e79b-145">2\.</span></span> <span data-ttu-id="1e79b-146">Coloque el foco del teclado en la aplicación en la ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="1e79b-146">Place the keyboard focus on the app in the browser window.</span></span>

   <span data-ttu-id="1e79b-147">3\.</span><span class="sxs-lookup"><span data-stu-id="1e79b-147">3\.</span></span> <span data-ttu-id="1e79b-148">Seleccione el método abreviado de teclado específico de Blazor en la nueva ventana del explorador: `Shift+Alt+D` en Windows o `Shift+Cmd+D` en macOS.</span><span class="sxs-lookup"><span data-stu-id="1e79b-148">Select the Blazor-specific keyboard shortcut in the new browser window: `Shift+Alt+D` on Windows or `Shift+Cmd+D` on macOS.</span></span>

   <span data-ttu-id="1e79b-149">4\.</span><span class="sxs-lookup"><span data-stu-id="1e79b-149">4\.</span></span> <span data-ttu-id="1e79b-150">Se abre la pestaña **DevTools** en el explorador.</span><span class="sxs-lookup"><span data-stu-id="1e79b-150">The **DevTools** tab opens in the browser.</span></span> <span data-ttu-id="1e79b-151">**Vuelva a seleccionar la pestaña de la aplicación en la ventana del explorador.**</span><span class="sxs-lookup"><span data-stu-id="1e79b-151">**Reselect the app's tab in the browser window.**</span></span>

   <span data-ttu-id="1e79b-152">Para adjuntar la aplicación a Visual Studio, vea la sección [Asociación al proceso en Visual Studio](#attach-to-process-in-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="1e79b-152">To attach the app to Visual Studio, see the [Attach to process in Visual Studio](#attach-to-process-in-visual-studio) section.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="1e79b-153">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="1e79b-153">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="1e79b-154">Ejecute una aplicación WebAssembly de Blazor en la configuración `Debug` y pase la opción `--configuration Debug` al comando [dotnet run](/dotnet/core/tools/dotnet-run) `dotnet run --configuration Debug`.</span><span class="sxs-lookup"><span data-stu-id="1e79b-154">Run a Blazor WebAssembly app in `Debug` configuration by passing the `--configuration Debug` option to the [dotnet run](/dotnet/core/tools/dotnet-run) command: `dotnet run --configuration Debug`.</span></span>
1. <span data-ttu-id="1e79b-155">Vaya a la aplicación en la dirección URL HTTP que se muestra en la ventana del shell.</span><span class="sxs-lookup"><span data-stu-id="1e79b-155">Navigate to the app at the HTTP URL shown in the shell's window.</span></span>
1. <span data-ttu-id="1e79b-156">Coloque el foco del teclado en la aplicación, no en el panel de herramientas para desarrolladores.</span><span class="sxs-lookup"><span data-stu-id="1e79b-156">Place the keyboard focus on the app, not the developer tools panel.</span></span> <span data-ttu-id="1e79b-157">Para este procedimiento es recomendable mantener cerrado el panel de herramientas para desarrolladores.</span><span class="sxs-lookup"><span data-stu-id="1e79b-157">It's best to keep the developer tools panel closed for this procedure.</span></span> <span data-ttu-id="1e79b-158">Una vez iniciada la depuración, puede volver a abrirlo.</span><span class="sxs-lookup"><span data-stu-id="1e79b-158">After debugging has started, you can re-open the developer tools panel.</span></span>
1. <span data-ttu-id="1e79b-159">Seleccione el siguiente método abreviado de teclado específico de Blazor:</span><span class="sxs-lookup"><span data-stu-id="1e79b-159">Select the following Blazor-specific keyboard shortcut:</span></span>

   * <span data-ttu-id="1e79b-160">`Shift+Alt+D` en Windows</span><span class="sxs-lookup"><span data-stu-id="1e79b-160">`Shift+Alt+D` on Windows</span></span>
   * <span data-ttu-id="1e79b-161">`Shift+Cmd+D` en macOS</span><span class="sxs-lookup"><span data-stu-id="1e79b-161">`Shift+Cmd+D` on macOS</span></span>

   <span data-ttu-id="1e79b-162">Si recibe un mensaje en el que se indica que **no se puede encontrar la pestaña del explorador depurable**, vea [Habilitación de la depuración remota](#enable-remote-debugging).</span><span class="sxs-lookup"><span data-stu-id="1e79b-162">If you receive the **Unable to find debuggable browser tab**, see [Enable remote debugging](#enable-remote-debugging).</span></span>
   
   <span data-ttu-id="1e79b-163">Después de habilitar la depuración remota:</span><span class="sxs-lookup"><span data-stu-id="1e79b-163">After enabling remote debugging:</span></span>
   
   <span data-ttu-id="1e79b-164">1\.</span><span class="sxs-lookup"><span data-stu-id="1e79b-164">1\.</span></span> <span data-ttu-id="1e79b-165">Se abre una nueva ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="1e79b-165">A new browser window opens.</span></span> <span data-ttu-id="1e79b-166">Cierre la ventana anterior.</span><span class="sxs-lookup"><span data-stu-id="1e79b-166">Close the prior window.</span></span>

   <span data-ttu-id="1e79b-167">2\.</span><span class="sxs-lookup"><span data-stu-id="1e79b-167">2\.</span></span> <span data-ttu-id="1e79b-168">Coloque el foco del teclado en la aplicación en la ventana del explorador, no en el panel de herramientas para desarrolladores.</span><span class="sxs-lookup"><span data-stu-id="1e79b-168">Place the keyboard focus on the app in the browser window, not the developer tools panel.</span></span>

   <span data-ttu-id="1e79b-169">3\.</span><span class="sxs-lookup"><span data-stu-id="1e79b-169">3\.</span></span> <span data-ttu-id="1e79b-170">Seleccione el método abreviado de teclado específico de Blazor en la nueva ventana del explorador: `Shift+Alt+D` en Windows o `Shift+Cmd+D` en macOS.</span><span class="sxs-lookup"><span data-stu-id="1e79b-170">Select the Blazor-specific keyboard shortcut in the new browser window: `Shift+Alt+D` on Windows or `Shift+Cmd+D` on macOS.</span></span>

---

## <a name="enable-remote-debugging"></a><span data-ttu-id="1e79b-171">Habilitar la depuración remota</span><span class="sxs-lookup"><span data-stu-id="1e79b-171">Enable remote debugging</span></span>

<span data-ttu-id="1e79b-172">Si la depuración remota está deshabilitada, Chrome genera una página de error en la que se indica que **no se puede encontrar la pestaña del explorador depurable**.</span><span class="sxs-lookup"><span data-stu-id="1e79b-172">If remote debugging is disabled, an **Unable to find debuggable browser tab** error page is generated by Chrome.</span></span> <span data-ttu-id="1e79b-173">La página de error contiene instrucciones para ejecutar Chrome con el puerto de depuración abierto para que el proxy de depuración de Blazor se pueda conectar a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1e79b-173">The error page contains instructions for running Chrome with the debugging port open so that the Blazor debugging proxy can connect to the app.</span></span> <span data-ttu-id="1e79b-174">*Cierre todas las instancias de Chrome* y reinicie dicho explorador como se ha indicado.</span><span class="sxs-lookup"><span data-stu-id="1e79b-174">*Close all Chrome instances* and restart Chrome as instructed.</span></span>

## <a name="debug-the-app"></a><span data-ttu-id="1e79b-175">Depurar la aplicación</span><span class="sxs-lookup"><span data-stu-id="1e79b-175">Debug the app</span></span>

<span data-ttu-id="1e79b-176">Una vez que Chrome se ejecute con la depuración remota habilitada, el método abreviado de teclado de depuración abre una nueva pestaña del depurador. Tras unos instantes, en la pestaña **Orígenes** se mostrará una lista de los ensamblados .NET en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1e79b-176">Once Chrome is running with remote debugging enabled, the debugging keyboard shortcut opens a new debugger tab. After a moment, the **Sources** tab shows a list of the .NET assemblies in the app.</span></span> <span data-ttu-id="1e79b-177">Expanda cada ensamblado y busque los archivos de origen *.cs*/ *.razor* disponibles para la depuración.</span><span class="sxs-lookup"><span data-stu-id="1e79b-177">Expand each assembly and find the *.cs*/*.razor* source files available for debugging.</span></span> <span data-ttu-id="1e79b-178">Establezca puntos de interrupción, vuelva a la pestaña de la aplicación y los puntos de interrupción se alcanzarán cuando se ejecute el código.</span><span class="sxs-lookup"><span data-stu-id="1e79b-178">Set breakpoints, switch back to the app's tab, and the breakpoints are hit when the code executes.</span></span> <span data-ttu-id="1e79b-179">Una vez que se haya alcanzado un punto de interrupción, examine el código una vez (`F10`) o reanude (`F8`) la ejecución del código con normalidad.</span><span class="sxs-lookup"><span data-stu-id="1e79b-179">After a breakpoint is hit, single-step (`F10`) through the code or resume (`F8`) code execution normally.</span></span>

Blazor<span data-ttu-id="1e79b-180"> proporciona un proxy de depuración que implementa el [protocolo Chrome DevTools](https://chromedevtools.github.io/devtools-protocol/) y aumenta el protocolo con información específica de .NET.</span><span class="sxs-lookup"><span data-stu-id="1e79b-180"> provides a debugging proxy that implements the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) and augments the protocol with .NET-specific information.</span></span> <span data-ttu-id="1e79b-181">Cuando se presiona el método abreviado de teclado de depuración, Blazor apunta a Chrome DevTools en el proxy.</span><span class="sxs-lookup"><span data-stu-id="1e79b-181">When debugging keyboard shortcut is pressed, Blazor points the Chrome DevTools at the proxy.</span></span> <span data-ttu-id="1e79b-182">El proxy se conecta a la ventana del explorador que se quiere depurar (de ahí la necesidad de habilitar la depuración remota).</span><span class="sxs-lookup"><span data-stu-id="1e79b-182">The proxy connects to the browser window you're seeking to debug (hence the need to enable remote debugging).</span></span>

## <a name="attach-to-process-in-visual-studio"></a><span data-ttu-id="1e79b-183">Asociación al proceso en Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1e79b-183">Attach to process in Visual Studio</span></span>

<span data-ttu-id="1e79b-184">La asociación al proceso de la aplicación en Visual Studio es un escenario de depuración *temporal* para WebAssembly de Blazor mientras la depuración **F5** se encuentre en fase de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="1e79b-184">Attaching to the app's process in Visual Studio is a *temporary* debugging scenario for Blazor WebAssembly while **F5** debugging is in development.</span></span>

<span data-ttu-id="1e79b-185">Para asociar el proceso de la aplicación en ejecución a Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="1e79b-185">To attach the running app's process to Visual Studio:</span></span>

1. <span data-ttu-id="1e79b-186">En Visual Studio, seleccione **Depurar** > **Asociar al proceso**.</span><span class="sxs-lookup"><span data-stu-id="1e79b-186">In Visual Studio, select **Debug** > **Attach to Process**.</span></span>
1. <span data-ttu-id="1e79b-187">En **Tipo de conexión**, seleccione **WebSocket con protocolo Chrome DevTools (sin autenticación)** .</span><span class="sxs-lookup"><span data-stu-id="1e79b-187">For the **Connection type**, select **Chrome devtools protocol websocket (no authentication)**.</span></span>
1. <span data-ttu-id="1e79b-188">En **Destino de la conexión**, pegue la dirección HTTP (no la dirección HTTPS) de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1e79b-188">For the **Connection target**, paste in the HTTP address (not the HTTPS address) of the app.</span></span>
1. <span data-ttu-id="1e79b-189">Seleccione **Actualizar** para actualizar las entradas en **Procesos disponibles**.</span><span class="sxs-lookup"><span data-stu-id="1e79b-189">Select **Refresh** to refresh the entries under **Available processes**.</span></span>
1. <span data-ttu-id="1e79b-190">Seleccione el proceso del explorador que se va a depurar y, después, **Adjuntar**.</span><span class="sxs-lookup"><span data-stu-id="1e79b-190">Select the browser process to debug and select **Attach**.</span></span>
1. <span data-ttu-id="1e79b-191">En el cuadro de diálogo **Seleccionar tipo de código**, seleccione el tipo de código del explorador específico al que se va a asociar (Edge o Chrome) y, después, seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="1e79b-191">In the **Select Code Type** dialog, select the code type for the specific browser you're attaching to (Edge or Chrome) and then select **OK**.</span></span>

## <a name="browser-source-maps"></a><span data-ttu-id="1e79b-192">Mapas de origen del explorador</span><span class="sxs-lookup"><span data-stu-id="1e79b-192">Browser source maps</span></span>

<span data-ttu-id="1e79b-193">Los mapas de origen del explorador permiten al explorador volver a asignar los archivos compilados a sus archivos de código fuente originales y se suelen usar para la depuración del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="1e79b-193">Browser source maps allow the browser to map compiled files back to their original source files and are commonly used for client-side debugging.</span></span> <span data-ttu-id="1e79b-194">Sin embargo, en la actualidad Blazor no asigna C# directamente a JavaScript o WASM.</span><span class="sxs-lookup"><span data-stu-id="1e79b-194">However, Blazor doesn't currently map C# directly to JavaScript/WASM.</span></span> <span data-ttu-id="1e79b-195">En su lugar, Blazor realiza la interpretación de IL en el explorador, por lo que los mapas de origen no son pertinentes.</span><span class="sxs-lookup"><span data-stu-id="1e79b-195">Instead, Blazor does IL interpretation within the browser, so source maps aren't relevant.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="1e79b-196">Solucionar problemas</span><span class="sxs-lookup"><span data-stu-id="1e79b-196">Troubleshoot</span></span>

<span data-ttu-id="1e79b-197">Si se encuentra con errores, la sugerencia siguiente puede ser útil:</span><span class="sxs-lookup"><span data-stu-id="1e79b-197">If you're running into errors, the following tip may help:</span></span>

<span data-ttu-id="1e79b-198">En la pestaña **Depurador**, abra las herramientas para desarrolladores en el explorador.</span><span class="sxs-lookup"><span data-stu-id="1e79b-198">In the **Debugger** tab, open the developer tools in your browser.</span></span> <span data-ttu-id="1e79b-199">En la consola, ejecute `localStorage.clear()` para quitar los puntos de interrupción.</span><span class="sxs-lookup"><span data-stu-id="1e79b-199">In the console, execute `localStorage.clear()` to remove any breakpoints.</span></span>
