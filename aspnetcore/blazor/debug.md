---
title: Depurar ASP.NET Core Blazor
author: guardrex
description: Obtenga información sobre cómo depurar aplicaciones Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
no-loc:
- Blazor
uid: blazor/debug
ms.openlocfilehash: 3096ad9b3a6904804f239d61f374adcd4d851978
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963144"
---
# <a name="debug-aspnet-core-opno-locblazor"></a><span data-ttu-id="7a753-103">Depurar ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="7a753-103">Debug ASP.NET Core Blazor</span></span>

[<span data-ttu-id="7a753-104">Daniel Roth</span><span class="sxs-lookup"><span data-stu-id="7a753-104">Daniel Roth</span></span>](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="7a753-105">Existe compatibilidad *temprana* con la depuración Blazor aplicaciones webassembly que se ejecutan en webassembly en Chrome.</span><span class="sxs-lookup"><span data-stu-id="7a753-105">*Early* support exists for debugging Blazor WebAssembly apps running on WebAssembly in Chrome.</span></span>

<span data-ttu-id="7a753-106">Las funcionalidades del depurador están limitadas.</span><span class="sxs-lookup"><span data-stu-id="7a753-106">Debugger capabilities are limited.</span></span> <span data-ttu-id="7a753-107">Entre los escenarios disponibles se incluyen:</span><span class="sxs-lookup"><span data-stu-id="7a753-107">Available scenarios include:</span></span>

* <span data-ttu-id="7a753-108">Establecer y quitar puntos de interrupción.</span><span class="sxs-lookup"><span data-stu-id="7a753-108">Set and remove breakpoints.</span></span>
* <span data-ttu-id="7a753-109">Un solo paso (`F10`) a través de la ejecución de código de código o de reanudación (`F8`).</span><span class="sxs-lookup"><span data-stu-id="7a753-109">Single-step (`F10`) through the code or resume (`F8`) code execution.</span></span>
* <span data-ttu-id="7a753-110">En la pantalla *variables locales* , observe los valores de las variables locales de tipo `int`, `string` y `bool`.</span><span class="sxs-lookup"><span data-stu-id="7a753-110">In the *Locals* display, observe the values of any local variables of type `int`, `string`, and `bool`.</span></span>
* <span data-ttu-id="7a753-111">Vea la pila de llamadas, incluidas las cadenas de llamadas que van desde JavaScript hasta .NET y desde .NET a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7a753-111">See the call stack, including call chains that go from JavaScript into .NET and from .NET to JavaScript.</span></span>

<span data-ttu-id="7a753-112">*No*se puede:</span><span class="sxs-lookup"><span data-stu-id="7a753-112">You *can't*:</span></span>

* <span data-ttu-id="7a753-113">Observe los valores de las variables locales que no son `int`, `string` o `bool`.</span><span class="sxs-lookup"><span data-stu-id="7a753-113">Observe the values of any locals that aren't an `int`, `string`, or `bool`.</span></span>
* <span data-ttu-id="7a753-114">Observe los valores de cualquier propiedad o campo de clase.</span><span class="sxs-lookup"><span data-stu-id="7a753-114">Observe the values of any class properties or fields.</span></span>
* <span data-ttu-id="7a753-115">Mantenga el mouse sobre las variables para ver sus valores.</span><span class="sxs-lookup"><span data-stu-id="7a753-115">Hover over variables to see their values.</span></span>
* <span data-ttu-id="7a753-116">Evaluar expresiones en la consola.</span><span class="sxs-lookup"><span data-stu-id="7a753-116">Evaluate expressions in the console.</span></span>
* <span data-ttu-id="7a753-117">Paso a través de las llamadas asincrónicas.</span><span class="sxs-lookup"><span data-stu-id="7a753-117">Step across async calls.</span></span>
* <span data-ttu-id="7a753-118">Realice la mayoría de los demás escenarios de depuración normales.</span><span class="sxs-lookup"><span data-stu-id="7a753-118">Perform most other ordinary debugging scenarios.</span></span>

<span data-ttu-id="7a753-119">El desarrollo de otros escenarios de depuración es un enfoque continuo del equipo de ingeniería.</span><span class="sxs-lookup"><span data-stu-id="7a753-119">Development of further debugging scenarios is an on-going focus of the engineering team.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7a753-120">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="7a753-120">Prerequisites</span></span>

<span data-ttu-id="7a753-121">La depuración requiere cualquiera de los siguientes exploradores:</span><span class="sxs-lookup"><span data-stu-id="7a753-121">Debugging requires either of the following browsers:</span></span>

* <span data-ttu-id="7a753-122">Google Chrome (versión 70 o posterior)</span><span class="sxs-lookup"><span data-stu-id="7a753-122">Google Chrome (version 70 or later)</span></span>
* <span data-ttu-id="7a753-123">Microsoft Edge Preview ([canal de desarrollo perimetral](https://www.microsoftedgeinsider.com))</span><span class="sxs-lookup"><span data-stu-id="7a753-123">Microsoft Edge preview ([Edge Dev Channel](https://www.microsoftedgeinsider.com))</span></span>

## <a name="procedure"></a><span data-ttu-id="7a753-124">Procedimiento</span><span class="sxs-lookup"><span data-stu-id="7a753-124">Procedure</span></span>

1. <span data-ttu-id="7a753-125">Ejecute una aplicación Blazor webassembly en `Debug` configuración.</span><span class="sxs-lookup"><span data-stu-id="7a753-125">Run a Blazor WebAssembly app in `Debug` configuration.</span></span> <span data-ttu-id="7a753-126">Pase la opción `--configuration Debug` al comando [dotnet Run](/dotnet/core/tools/dotnet-run) : `dotnet run --configuration Debug`.</span><span class="sxs-lookup"><span data-stu-id="7a753-126">Pass the `--configuration Debug` option to the [dotnet run](/dotnet/core/tools/dotnet-run) command: `dotnet run --configuration Debug`.</span></span>
1. <span data-ttu-id="7a753-127">Acceda a la aplicación en el explorador.</span><span class="sxs-lookup"><span data-stu-id="7a753-127">Access the app in the browser.</span></span>
1. <span data-ttu-id="7a753-128">Coloque el foco de teclado en la aplicación, no en el panel herramientas de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="7a753-128">Place the keyboard focus on the app, not the developer tools panel.</span></span> <span data-ttu-id="7a753-129">Se puede cerrar el panel herramientas de desarrollo cuando se inicia la depuración.</span><span class="sxs-lookup"><span data-stu-id="7a753-129">The developer tools panel can be closed when debugging is initiated.</span></span>
1. <span data-ttu-id="7a753-130">Seleccione el siguiente método abreviado de teclado específico de Blazor:</span><span class="sxs-lookup"><span data-stu-id="7a753-130">Select the following Blazor-specific keyboard shortcut:</span></span>
   * <span data-ttu-id="7a753-131">`Shift+Alt+D` en Windows/Linux</span><span class="sxs-lookup"><span data-stu-id="7a753-131">`Shift+Alt+D` on Windows/Linux</span></span>
   * <span data-ttu-id="7a753-132">`Shift+Cmd+D` en macOS</span><span class="sxs-lookup"><span data-stu-id="7a753-132">`Shift+Cmd+D` on macOS</span></span>
1. <span data-ttu-id="7a753-133">Siga los pasos indicados en la pantalla para reiniciar el explorador con la depuración remota habilitada.</span><span class="sxs-lookup"><span data-stu-id="7a753-133">Follow the steps listed on the screen to restart the browser with remote debugging enabled.</span></span>
1. <span data-ttu-id="7a753-134">Vuelva a seleccionar el siguiente método abreviado de teclado específico de Blazorpara iniciar la sesión de depuración:</span><span class="sxs-lookup"><span data-stu-id="7a753-134">Select the following Blazor-specific keyboard shortcut once again to start the debug session:</span></span>
   * <span data-ttu-id="7a753-135">`Shift+Alt+D` en Windows/Linux</span><span class="sxs-lookup"><span data-stu-id="7a753-135">`Shift+Alt+D` on Windows/Linux</span></span>
   * <span data-ttu-id="7a753-136">`Shift+Cmd+D` en macOS</span><span class="sxs-lookup"><span data-stu-id="7a753-136">`Shift+Cmd+D` on macOS</span></span>

## <a name="enable-remote-debugging"></a><span data-ttu-id="7a753-137">Habilitar depuración remota</span><span class="sxs-lookup"><span data-stu-id="7a753-137">Enable remote debugging</span></span>

<span data-ttu-id="7a753-138">Si la depuración remota está deshabilitada, Chrome genera una página de error **no se puede encontrar la pestaña del explorador depurable** .</span><span class="sxs-lookup"><span data-stu-id="7a753-138">If remote debugging is disabled, an **Unable to find debuggable browser tab** error page is generated by Chrome.</span></span> <span data-ttu-id="7a753-139">La página de error contiene instrucciones para ejecutar Chrome con el puerto de depuración abierto para que el Blazor proxy de depuración pueda conectarse a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7a753-139">The error page contains instructions for running Chrome with the debugging port open so that the Blazor debugging proxy can connect to the app.</span></span> <span data-ttu-id="7a753-140">*Cierre todas las instancias de Chrome* y reinicie Chrome tal y como se indica.</span><span class="sxs-lookup"><span data-stu-id="7a753-140">*Close all Chrome instances* and restart Chrome as instructed.</span></span>

## <a name="debug-the-app"></a><span data-ttu-id="7a753-141">Depurar la aplicación</span><span class="sxs-lookup"><span data-stu-id="7a753-141">Debug the app</span></span>

<span data-ttu-id="7a753-142">Una vez que Chrome se ejecuta con la depuración remota habilitada, el método abreviado de teclado de depuración abre una nueva pestaña del depurador. Después de un momento, la pestaña **orígenes** muestra una lista de los ensamblados .net en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7a753-142">Once Chrome is running with remote debugging enabled, the debugging keyboard shortcut opens a new debugger tab. After a moment, the **Sources** tab shows a list of the .NET assemblies in the app.</span></span> <span data-ttu-id="7a753-143">Expanda cada ensamblado y busque los archivos de origen *. cs*/ *. Razor* disponibles para la depuración.</span><span class="sxs-lookup"><span data-stu-id="7a753-143">Expand each assembly and find the *.cs*/*.razor* source files available for debugging.</span></span> <span data-ttu-id="7a753-144">Establezca puntos de interrupción, vuelva a la pestaña de la aplicación y los puntos de interrupción se alcanzarán cuando se ejecute el código.</span><span class="sxs-lookup"><span data-stu-id="7a753-144">Set breakpoints, switch back to the app's tab, and the breakpoints are hit when the code executes.</span></span> <span data-ttu-id="7a753-145">Una vez que se visita un punto de interrupción, se ejecuta el código de un solo paso (`F10`) a través de la ejecución de código de código o de reanudación (`F8`) con normalidad.</span><span class="sxs-lookup"><span data-stu-id="7a753-145">After a breakpoint is hit, single-step (`F10`) through the code or resume (`F8`) code execution normally.</span></span>

Blazor<span data-ttu-id="7a753-146"> proporciona un proxy de depuración que implementa el [Protocolo Chrome DevTools](https://chromedevtools.github.io/devtools-protocol/) y aumenta el protocolo con. Información específica de la red.</span><span class="sxs-lookup"><span data-stu-id="7a753-146"> provides a debugging proxy that implements the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) and augments the protocol with .NET-specific information.</span></span> <span data-ttu-id="7a753-147">Cuando se presiona el método abreviado de teclado de depuración, Blazor señala el DevTools de cromo en el proxy.</span><span class="sxs-lookup"><span data-stu-id="7a753-147">When debugging keyboard shortcut is pressed, Blazor points the Chrome DevTools at the proxy.</span></span> <span data-ttu-id="7a753-148">El proxy se conecta a la ventana del explorador que busca depurar (por lo tanto, la necesidad de habilitar la depuración remota).</span><span class="sxs-lookup"><span data-stu-id="7a753-148">The proxy connects to the browser window you're seeking to debug (hence the need to enable remote debugging).</span></span>

## <a name="browser-source-maps"></a><span data-ttu-id="7a753-149">Mapas de origen del explorador</span><span class="sxs-lookup"><span data-stu-id="7a753-149">Browser source maps</span></span>

<span data-ttu-id="7a753-150">Los mapas de origen del explorador permiten al explorador volver a asignar los archivos compilados a sus archivos de código fuente originales y se suelen usar para la depuración del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="7a753-150">Browser source maps allow the browser to map compiled files back to their original source files and are commonly used for client-side debugging.</span></span> <span data-ttu-id="7a753-151">Sin embargo, Blazor actualmente no C# se asigna directamente a JavaScript/WASM.</span><span class="sxs-lookup"><span data-stu-id="7a753-151">However, Blazor doesn't currently map C# directly to JavaScript/WASM.</span></span> <span data-ttu-id="7a753-152">En su lugar, Blazor realiza la interpretación de IL en el explorador, por lo que los mapas de origen no son relevantes.</span><span class="sxs-lookup"><span data-stu-id="7a753-152">Instead, Blazor does IL interpretation within the browser, so source maps aren't relevant.</span></span>

## <a name="troubleshooting-tip"></a><span data-ttu-id="7a753-153">Sugerencia para la solución de problemas</span><span class="sxs-lookup"><span data-stu-id="7a753-153">Troubleshooting tip</span></span>

<span data-ttu-id="7a753-154">Si se encuentra con errores, la sugerencia siguiente puede ser útil:</span><span class="sxs-lookup"><span data-stu-id="7a753-154">If you're running into errors, the following tip may help:</span></span>

<span data-ttu-id="7a753-155">En la pestaña **depurador** , abra las herramientas de desarrollo en el explorador.</span><span class="sxs-lookup"><span data-stu-id="7a753-155">In the **Debugger** tab, open the developer tools in your browser.</span></span> <span data-ttu-id="7a753-156">En la consola de, ejecute `localStorage.clear()` para quitar los puntos de interrupción.</span><span class="sxs-lookup"><span data-stu-id="7a753-156">In the console, execute `localStorage.clear()` to remove any breakpoints.</span></span>
