---
title: Desarrollar aplicaciones de ASP.NET Core con dotnet watch
author: rick-anderson
description: "Se muestra cómo usar dotnet watch."
keywords: ASP.NET Core, usar dotnet watch
ms.author: riande
manager: wpickett
ms.date: 03/09/2017
ms.topic: article
ms.assetid: 563ffb3f-d369-4aa5-bf0a-7300b4e7832c
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/dotnet-watch
ms.openlocfilehash: 6a8943619e6174dbcd3d901b7bb736ba5d3af95d
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2017
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a><span data-ttu-id="765db-104">Desarrollar aplicaciones de ASP.NET Core con dotnet watch</span><span class="sxs-lookup"><span data-stu-id="765db-104">Developing ASP.NET Core apps using dotnet watch</span></span>


<span data-ttu-id="765db-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="765db-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="765db-106">`dotnet watch` es una herramienta que ejecuta un comando `dotnet` cuando se modifican los archivos de código fuente.</span><span class="sxs-lookup"><span data-stu-id="765db-106">`dotnet watch` is a tool that runs a `dotnet` command when source files change.</span></span> <span data-ttu-id="765db-107">Por ejemplo, un cambio en un archivo puede desencadenar una compilación, pruebas o una implementación.</span><span class="sxs-lookup"><span data-stu-id="765db-107">For example, a file change can trigger compilation, tests, or deployment.</span></span>

<span data-ttu-id="765db-108">En este tutorial usaremos una aplicación de API web existente con dos puntos de conexión: uno que devuelva una suma y otro que devuelva un producto.</span><span class="sxs-lookup"><span data-stu-id="765db-108">In this tutorial, we use an existing Web API app with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="765db-109">El método del producto contiene un error que corregiremos en este mismo tutorial.</span><span class="sxs-lookup"><span data-stu-id="765db-109">The product method contains a bug that we'll fix as part of this tutorial.</span></span>

<span data-ttu-id="765db-110">Descargue la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="765db-110">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="765db-111">Contiene dos proyectos, `WebApp` (una aplicación web), y `WebAppTests` (pruebas unitarias para la aplicación web).</span><span class="sxs-lookup"><span data-stu-id="765db-111">It contains two projects, `WebApp` (a web app) and `WebAppTests` (unit tests for the web app).</span></span>

<span data-ttu-id="765db-112">En una consola, vaya a la carpeta WebApp y ejecute los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="765db-112">In a console, navigate to the WebApp folder and run the following commands:</span></span>

- `dotnet restore`
- `dotnet run`

<span data-ttu-id="765db-113">La salida de la consola mostrará mensajes similares al siguiente (indicando que la aplicación se ejecuta y espera solicitudes):</span><span class="sxs-lookup"><span data-stu-id="765db-113">The console output will show messages similar to the following (indicating that the app is running and waiting for requests):</span></span>

```console
$ dotnet run
Hosting environment: Production
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="765db-114">En un explorador web, vaya a `http://localhost:5000/api/math/sum?a=4&b=5`. Ahí debería ver el resultado `9`.</span><span class="sxs-lookup"><span data-stu-id="765db-114">In a web browser, navigate to `http://localhost:5000/api/math/sum?a=4&b=5`, you should see the result `9`.</span></span>

<span data-ttu-id="765db-115">Vaya a la API del producto (`http://localhost:5000/api/math/product?a=4&b=5`). Devolverá `9` y no `20`, tal como se esperaría.</span><span class="sxs-lookup"><span data-stu-id="765db-115">Navigate to the product API (`http://localhost:5000/api/math/product?a=4&b=5`), it returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="765db-116">Lo corregiremos más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="765db-116">We'll fix that later in the tutorial.</span></span>

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="765db-117">Agregar `dotnet watch` a un proyecto</span><span class="sxs-lookup"><span data-stu-id="765db-117">Add `dotnet watch` to a project</span></span>

- <span data-ttu-id="765db-118">Agregue `Microsoft.DotNet.Watcher.Tools` al archivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="765db-118">Add `Microsoft.DotNet.Watcher.Tools` to the *.csproj* file:</span></span>
 ```xml
 <ItemGroup>
   <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
 </ItemGroup> 
 ```

- <span data-ttu-id="765db-119">Ejecute `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="765db-119">Run `dotnet restore`.</span></span>

## <a name="running-dotnet-commands-using-dotnet-watch"></a><span data-ttu-id="765db-120">Ejecutar comandos `dotnet` mediante `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="765db-120">Running `dotnet` commands using `dotnet watch`</span></span>

<span data-ttu-id="765db-121">Se puede ejecutar cualquier comando `dotnet` con `dotnet watch`. Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="765db-121">Any `dotnet` command can be run with `dotnet watch`, for example:</span></span>

| <span data-ttu-id="765db-122">Comando</span><span class="sxs-lookup"><span data-stu-id="765db-122">Command</span></span> | <span data-ttu-id="765db-123">Comando con watch</span><span class="sxs-lookup"><span data-stu-id="765db-123">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="765db-124">dotnet run</span><span class="sxs-lookup"><span data-stu-id="765db-124">dotnet run</span></span> | <span data-ttu-id="765db-125">dotnet watch run</span><span class="sxs-lookup"><span data-stu-id="765db-125">dotnet watch run</span></span> |
| <span data-ttu-id="765db-126">dotnet run -f net451</span><span class="sxs-lookup"><span data-stu-id="765db-126">dotnet run -f net451</span></span> | <span data-ttu-id="765db-127">dotnet watch run -f net451</span><span class="sxs-lookup"><span data-stu-id="765db-127">dotnet watch run -f net451</span></span> |
| <span data-ttu-id="765db-128">dotnet run -f net451 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="765db-128">dotnet run -f net451 -- --arg1</span></span> | <span data-ttu-id="765db-129">dotnet watch run -f net451 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="765db-129">dotnet watch run -f net451 -- --arg1</span></span> |
| <span data-ttu-id="765db-130">dotnet test</span><span class="sxs-lookup"><span data-stu-id="765db-130">dotnet test</span></span> | <span data-ttu-id="765db-131">dotnet watch test</span><span class="sxs-lookup"><span data-stu-id="765db-131">dotnet watch test</span></span> |

<span data-ttu-id="765db-132">Ejecute `dotnet watch run` en la carpeta `WebApp`.</span><span class="sxs-lookup"><span data-stu-id="765db-132">Run `dotnet watch run` in the `WebApp` folder.</span></span> <span data-ttu-id="765db-133">La salida de la consola indicará que se ha iniciado `watch`.</span><span class="sxs-lookup"><span data-stu-id="765db-133">The console output will indicate `watch` has started.</span></span>

## <a name="making-changes-with-dotnet-watch"></a><span data-ttu-id="765db-134">Efectuar cambios con `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="765db-134">Making changes with `dotnet watch`</span></span>

<span data-ttu-id="765db-135">Asegúrese de que `dotnet watch` se está ejecutando.</span><span class="sxs-lookup"><span data-stu-id="765db-135">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="765db-136">Corrija el error en el método `Product` del `MathController` para que devuelva el producto y no la suma.</span><span class="sxs-lookup"><span data-stu-id="765db-136">Fix the bug in the `Product` method of the `MathController` so it returns the product and not the sum.</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

<span data-ttu-id="765db-137">Guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="765db-137">Save the file.</span></span> <span data-ttu-id="765db-138">La salida de la consola mostrará mensajes que indicarán que `dotnet watch` ha detectado un cambio de archivo y ha reiniciado la aplicación.</span><span class="sxs-lookup"><span data-stu-id="765db-138">The console output will show messages indicating that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="765db-139">Compruebe que `http://localhost:5000/api/math/product?a=4&b=5` devuelve el resultado correcto.</span><span class="sxs-lookup"><span data-stu-id="765db-139">Verify `http://localhost:5000/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="running-tests-using-dotnet-watch"></a><span data-ttu-id="765db-140">Ejecutar pruebas con `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="765db-140">Running tests using `dotnet watch`</span></span>

- <span data-ttu-id="765db-141">Vuelva a cambiar el método `Product` del `MathController` para devolver la suma y guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="765db-141">Change the `Product` method of the `MathController` back to returning the sum and save the file.</span></span>
- <span data-ttu-id="765db-142">En una ventana de comandos, vaya a la carpeta `WebAppTests`.</span><span class="sxs-lookup"><span data-stu-id="765db-142">In a command window, naviagate to the `WebAppTests` folder.</span></span>
- <span data-ttu-id="765db-143">Ejecute `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="765db-143">Run `dotnet restore`</span></span>
- <span data-ttu-id="765db-144">Ejecute `dotnet watch test`.</span><span class="sxs-lookup"><span data-stu-id="765db-144">Run `dotnet watch test`.</span></span> <span data-ttu-id="765db-145">Verá la salida que indicará que se ha producido un error en una prueba y que watcher espera cambios de archivos:</span><span class="sxs-lookup"><span data-stu-id="765db-145">You see output indicating that a test failed and that watcher is waiting for file changes:</span></span>

 ```console
 Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
 Test Run Failed.
  ```
- <span data-ttu-id="765db-146">Corrija el código del método `Product` para que devuelva el producto.</span><span class="sxs-lookup"><span data-stu-id="765db-146">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="765db-147">Guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="765db-147">Save the file.</span></span>

<span data-ttu-id="765db-148">`dotnet watch` detecta el cambio de archivo y vuelve a ejecutar las pruebas.</span><span class="sxs-lookup"><span data-stu-id="765db-148">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="765db-149">La salida de la consola mostrará que se han superado las pruebas.</span><span class="sxs-lookup"><span data-stu-id="765db-149">The console output will show the tests passed.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="765db-150">dotnet-watch en GitHub</span><span class="sxs-lookup"><span data-stu-id="765db-150">dotnet-watch in GitHub</span></span>

<span data-ttu-id="765db-151">dotnet-watch forma parte del [repositorio de DotNetTools](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools) de GitHub.</span><span class="sxs-lookup"><span data-stu-id="765db-151">dotnet-watch is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools).</span></span>

<span data-ttu-id="765db-152">En la [sección MSBuild](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild) del [archivo Léame de dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) se describe cómo se puede configurar dotnet-watch desde el archivo de proyecto de MSBuild que se está inspeccionando.</span><span class="sxs-lookup"><span data-stu-id="765db-152">The [MSBuild section](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild) of the [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) explains how dotnet-watch can be configured from the MSBuild project file being watched.</span></span> <span data-ttu-id="765db-153">El [archivo Léame de dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) contiene información de dotnet-watch que no se trata en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="765db-153">The [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) contains information on dotnet-watch not covered in this tutorial.</span></span>
