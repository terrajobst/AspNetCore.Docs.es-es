---
title: Depuración ASP.NET Core extraordinaria
author: guardrex
description: Obtenga información sobre cómo depurar aplicaciones increíbles.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: blazor/debug
ms.openlocfilehash: 9fc3f1d2dd7dc79d2ba3d64bff6e0f92ac2cf6dc
ms.sourcegitcommit: 35a86ce48041caaf6396b1e88b0472578ba24483
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72391184"
---
# <a name="debug-aspnet-core-blazor"></a><span data-ttu-id="839ed-103">Depuración ASP.NET Core extraordinaria</span><span class="sxs-lookup"><span data-stu-id="839ed-103">Debug ASP.NET Core Blazor</span></span>

[<span data-ttu-id="839ed-104">Daniel Roth</span><span class="sxs-lookup"><span data-stu-id="839ed-104">Daniel Roth</span></span>](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="839ed-105">Existe compatibilidad *temprana* para depurar aplicaciones webassembly increíblemente que se ejecutan en webassembly en Chrome.</span><span class="sxs-lookup"><span data-stu-id="839ed-105">*Early* support exists for debugging Blazor WebAssembly apps running on WebAssembly in Chrome.</span></span>

<span data-ttu-id="839ed-106">Las funcionalidades del depurador están limitadas.</span><span class="sxs-lookup"><span data-stu-id="839ed-106">Debugger capabilities are limited.</span></span> <span data-ttu-id="839ed-107">Entre los escenarios disponibles se incluyen:</span><span class="sxs-lookup"><span data-stu-id="839ed-107">Available scenarios include:</span></span>

* <span data-ttu-id="839ed-108">Establecer y quitar puntos de interrupción.</span><span class="sxs-lookup"><span data-stu-id="839ed-108">Set and remove breakpoints.</span></span>
* <span data-ttu-id="839ed-109">Un solo paso (`F10`) a través de la ejecución de código de código o de reanudación (`F8`).</span><span class="sxs-lookup"><span data-stu-id="839ed-109">Single-step (`F10`) through the code or resume (`F8`) code execution.</span></span>
* <span data-ttu-id="839ed-110">En la pantalla *variables locales* , observe los valores de las variables locales de tipo `int`, `string` y `bool`.</span><span class="sxs-lookup"><span data-stu-id="839ed-110">In the *Locals* display, observe the values of any local variables of type `int`, `string`, and `bool`.</span></span>
* <span data-ttu-id="839ed-111">Vea la pila de llamadas, incluidas las cadenas de llamadas que van desde JavaScript hasta .NET y desde .NET a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="839ed-111">See the call stack, including call chains that go from JavaScript into .NET and from .NET to JavaScript.</span></span>

<span data-ttu-id="839ed-112">*No*se puede:</span><span class="sxs-lookup"><span data-stu-id="839ed-112">You *can't*:</span></span>

* <span data-ttu-id="839ed-113">Observe los valores de las variables locales que no son `int`, `string` o `bool`.</span><span class="sxs-lookup"><span data-stu-id="839ed-113">Observe the values of any locals that aren't an `int`, `string`, or `bool`.</span></span>
* <span data-ttu-id="839ed-114">Observe los valores de cualquier propiedad o campo de clase.</span><span class="sxs-lookup"><span data-stu-id="839ed-114">Observe the values of any class properties or fields.</span></span>
* <span data-ttu-id="839ed-115">Mantenga el mouse sobre las variables para ver sus valores.</span><span class="sxs-lookup"><span data-stu-id="839ed-115">Hover over variables to see their values.</span></span>
* <span data-ttu-id="839ed-116">Evaluar expresiones en la consola.</span><span class="sxs-lookup"><span data-stu-id="839ed-116">Evaluate expressions in the console.</span></span>
* <span data-ttu-id="839ed-117">Paso a través de las llamadas asincrónicas.</span><span class="sxs-lookup"><span data-stu-id="839ed-117">Step across async calls.</span></span>
* <span data-ttu-id="839ed-118">Realice la mayoría de los demás escenarios de depuración normales.</span><span class="sxs-lookup"><span data-stu-id="839ed-118">Perform most other ordinary debugging scenarios.</span></span>

<span data-ttu-id="839ed-119">El desarrollo de otros escenarios de depuración es un enfoque continuo del equipo de ingeniería.</span><span class="sxs-lookup"><span data-stu-id="839ed-119">Development of further debugging scenarios is an on-going focus of the engineering team.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="839ed-120">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="839ed-120">Prerequisites</span></span>

<span data-ttu-id="839ed-121">La depuración requiere cualquiera de los siguientes exploradores:</span><span class="sxs-lookup"><span data-stu-id="839ed-121">Debugging requires either of the following browsers:</span></span>

* <span data-ttu-id="839ed-122">Google Chrome (versión 70 o posterior)</span><span class="sxs-lookup"><span data-stu-id="839ed-122">Google Chrome (version 70 or later)</span></span>
* <span data-ttu-id="839ed-123">Microsoft Edge Preview ([canal de desarrollo perimetral](https://www.microsoftedgeinsider.com))</span><span class="sxs-lookup"><span data-stu-id="839ed-123">Microsoft Edge preview ([Edge Dev Channel](https://www.microsoftedgeinsider.com))</span></span>

## <a name="procedure"></a><span data-ttu-id="839ed-124">Procedimiento</span><span class="sxs-lookup"><span data-stu-id="839ed-124">Procedure</span></span>

1. <span data-ttu-id="839ed-125">Ejecute una aplicación de webassembly increíblemente alta en la configuración de `Debug`.</span><span class="sxs-lookup"><span data-stu-id="839ed-125">Run a Blazor WebAssembly app in `Debug` configuration.</span></span> <span data-ttu-id="839ed-126">Pase la opción `--configuration Debug` al comando [dotnet Run](/dotnet/core/tools/dotnet-run) : `dotnet run --configuration Debug`.</span><span class="sxs-lookup"><span data-stu-id="839ed-126">Pass the `--configuration Debug` option to the [dotnet run](/dotnet/core/tools/dotnet-run) command: `dotnet run --configuration Debug`.</span></span>
1. <span data-ttu-id="839ed-127">Acceda a la aplicación en el explorador.</span><span class="sxs-lookup"><span data-stu-id="839ed-127">Access the app in the browser.</span></span>
1. <span data-ttu-id="839ed-128">Coloque el foco de teclado en la aplicación, no en el panel herramientas de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="839ed-128">Place the keyboard focus on the app, not the developer tools panel.</span></span> <span data-ttu-id="839ed-129">Se puede cerrar el panel herramientas de desarrollo cuando se inicia la depuración.</span><span class="sxs-lookup"><span data-stu-id="839ed-129">The developer tools panel can be closed when debugging is initiated.</span></span>
1. <span data-ttu-id="839ed-130">Seleccione el siguiente método abreviado de teclado:</span><span class="sxs-lookup"><span data-stu-id="839ed-130">Select the following Blazor-specific keyboard shortcut:</span></span>
   * <span data-ttu-id="839ed-131">`Shift+Alt+D` en Windows/Linux</span><span class="sxs-lookup"><span data-stu-id="839ed-131">`Shift+Alt+D` on Windows/Linux</span></span>
   * <span data-ttu-id="839ed-132">`Shift+Cmd+D` en macOS</span><span class="sxs-lookup"><span data-stu-id="839ed-132">`Shift+Cmd+D` on macOS</span></span>
1. <span data-ttu-id="839ed-133">Siga los pasos indicados en la pantalla para reiniciar el explorador con la depuración remota habilitada.</span><span class="sxs-lookup"><span data-stu-id="839ed-133">Follow the steps listed on the screen to restart the browser with remote debugging enabled.</span></span>
1. <span data-ttu-id="839ed-134">Para iniciar la sesión de depuración, seleccione de nuevo el siguiente método abreviado de teclado específico de increíbles:</span><span class="sxs-lookup"><span data-stu-id="839ed-134">Select the following Blazor-specific keyboard shortcut once again to start the debug session:</span></span>
   * <span data-ttu-id="839ed-135">`Shift+Alt+D` en Windows/Linux</span><span class="sxs-lookup"><span data-stu-id="839ed-135">`Shift+Alt+D` on Windows/Linux</span></span>
   * <span data-ttu-id="839ed-136">`Shift+Cmd+D` en macOS</span><span class="sxs-lookup"><span data-stu-id="839ed-136">`Shift+Cmd+D` on macOS</span></span>

## <a name="enable-remote-debugging"></a><span data-ttu-id="839ed-137">Habilitar depuración remota</span><span class="sxs-lookup"><span data-stu-id="839ed-137">Enable remote debugging</span></span>

<span data-ttu-id="839ed-138">Si la depuración remota está deshabilitada, Chrome genera una página de error **no se puede encontrar la pestaña del explorador depurable** .</span><span class="sxs-lookup"><span data-stu-id="839ed-138">If remote debugging is disabled, an **Unable to find debuggable browser tab** error page is generated by Chrome.</span></span> <span data-ttu-id="839ed-139">La página de error contiene instrucciones para ejecutar Chrome con el puerto de depuración abierto para que el proxy de depuración increíblemente rápido pueda conectarse a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="839ed-139">The error page contains instructions for running Chrome with the debugging port open so that the Blazor debugging proxy can connect to the app.</span></span> <span data-ttu-id="839ed-140">*Cierre todas las instancias de Chrome* y reinicie Chrome tal y como se indica.</span><span class="sxs-lookup"><span data-stu-id="839ed-140">*Close all Chrome instances* and restart Chrome as instructed.</span></span>

## <a name="debug-the-app"></a><span data-ttu-id="839ed-141">Depurar la aplicación</span><span class="sxs-lookup"><span data-stu-id="839ed-141">Debug the app</span></span>

<span data-ttu-id="839ed-142">Una vez que Chrome se ejecuta con la depuración remota habilitada, el método abreviado de teclado de depuración abre una nueva pestaña del depurador. Después de un momento, la pestaña **orígenes** muestra una lista de los ensamblados .net en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="839ed-142">Once Chrome is running with remote debugging enabled, the debugging keyboard shortcut opens a new debugger tab. After a moment, the **Sources** tab shows a list of the .NET assemblies in the app.</span></span> <span data-ttu-id="839ed-143">Expanda cada ensamblado y busque los archivos de origen *. cs*/ *. Razor* disponibles para la depuración.</span><span class="sxs-lookup"><span data-stu-id="839ed-143">Expand each assembly and find the *.cs*/*.razor* source files available for debugging.</span></span> <span data-ttu-id="839ed-144">Establezca puntos de interrupción, vuelva a la pestaña de la aplicación y los puntos de interrupción se alcanzarán cuando se ejecute el código.</span><span class="sxs-lookup"><span data-stu-id="839ed-144">Set breakpoints, switch back to the app's tab, and the breakpoints are hit when the code executes.</span></span> <span data-ttu-id="839ed-145">Una vez que se visita un punto de interrupción, se ejecuta el código de un solo paso (`F10`) a través de la ejecución de código de código o de reanudación (`F8`) con normalidad.</span><span class="sxs-lookup"><span data-stu-id="839ed-145">After a breakpoint is hit, single-step (`F10`) through the code or resume (`F8`) code execution normally.</span></span>

<span data-ttu-id="839ed-146">Increíbles proporciona un proxy de depuración que implementa el [Protocolo Chrome DevTools](https://chromedevtools.github.io/devtools-protocol/) y aumenta el protocolo con. Información específica de la red.</span><span class="sxs-lookup"><span data-stu-id="839ed-146">Blazor provides a debugging proxy that implements the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) and augments the protocol with .NET-specific information.</span></span> <span data-ttu-id="839ed-147">Cuando se presiona el método abreviado de teclado de depuración, el DevTools de cromo apunta al proxy.</span><span class="sxs-lookup"><span data-stu-id="839ed-147">When debugging keyboard shortcut is pressed, Blazor points the Chrome DevTools at the proxy.</span></span> <span data-ttu-id="839ed-148">El proxy se conecta a la ventana del explorador que busca depurar (por lo tanto, la necesidad de habilitar la depuración remota).</span><span class="sxs-lookup"><span data-stu-id="839ed-148">The proxy connects to the browser window you're seeking to debug (hence the need to enable remote debugging).</span></span>

## <a name="browser-source-maps"></a><span data-ttu-id="839ed-149">Mapas de origen del explorador</span><span class="sxs-lookup"><span data-stu-id="839ed-149">Browser source maps</span></span>

<span data-ttu-id="839ed-150">Los mapas de origen del explorador permiten al explorador volver a asignar los archivos compilados a sus archivos de código fuente originales y se suelen usar para la depuración del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="839ed-150">Browser source maps allow the browser to map compiled files back to their original source files and are commonly used for client-side debugging.</span></span> <span data-ttu-id="839ed-151">Sin embargo, increíble no se asigna C# directamente a JavaScript/WASM.</span><span class="sxs-lookup"><span data-stu-id="839ed-151">However, Blazor doesn't currently map C# directly to JavaScript/WASM.</span></span> <span data-ttu-id="839ed-152">En su lugar, increíble es una interpretación de IL en el explorador, por lo que los mapas de origen no son relevantes.</span><span class="sxs-lookup"><span data-stu-id="839ed-152">Instead, Blazor does IL interpretation within the browser, so source maps aren't relevant.</span></span>

## <a name="troubleshooting-tip"></a><span data-ttu-id="839ed-153">Sugerencia para la solución de problemas</span><span class="sxs-lookup"><span data-stu-id="839ed-153">Troubleshooting tip</span></span>

<span data-ttu-id="839ed-154">Si se encuentra con errores, la sugerencia siguiente puede ser útil:</span><span class="sxs-lookup"><span data-stu-id="839ed-154">If you're running into errors, the following tip may help:</span></span>

<span data-ttu-id="839ed-155">En la pestaña **depurador** , abra las herramientas de desarrollo en el explorador.</span><span class="sxs-lookup"><span data-stu-id="839ed-155">In the **Debugger** tab, open the developer tools in your browser.</span></span> <span data-ttu-id="839ed-156">En la consola de, ejecute `localStorage.clear()` para quitar los puntos de interrupción.</span><span class="sxs-lookup"><span data-stu-id="839ed-156">In the console, execute `localStorage.clear()` to remove any breakpoints.</span></span>
