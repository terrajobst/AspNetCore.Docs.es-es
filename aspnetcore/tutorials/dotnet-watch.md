---
title: Desarrollar aplicaciones ASP.NET Core con un monitor de archivos
author: rick-anderson
description: Este tutorial muestra cómo instalar y usar la herramienta de monitor de archivos (dotnet watch) de la CLI de .NET Core en una aplicación de ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 05/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/dotnet-watch
ms.openlocfilehash: 016ee107ae646ed43d8a98e97fd2d5b41356910e
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/12/2018
ms.locfileid: "35341852"
---
# <a name="develop-aspnet-core-apps-using-a-file-watcher"></a><span data-ttu-id="d8317-103">Desarrollar aplicaciones ASP.NET Core con un monitor de archivos</span><span class="sxs-lookup"><span data-stu-id="d8317-103">Develop ASP.NET Core apps using a file watcher</span></span>

<span data-ttu-id="d8317-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="d8317-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="d8317-105">`dotnet watch` es una herramienta que ejecuta un comando de la [CLI de .NET Core](/dotnet/core/tools) cuando se modifican los archivos de código fuente.</span><span class="sxs-lookup"><span data-stu-id="d8317-105">`dotnet watch` is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="d8317-106">Por ejemplo, un cambio en un archivo puede desencadenar una compilación, una ejecución de prueba o una implementación.</span><span class="sxs-lookup"><span data-stu-id="d8317-106">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="d8317-107">En este tutorial usaremos una API web existente con dos puntos de conexión: uno que devuelve una suma y otro que devuelve un producto.</span><span class="sxs-lookup"><span data-stu-id="d8317-107">This tutorial uses an existing web API with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="d8317-108">El método Product contiene un error, que se ha corregido en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="d8317-108">The product method has a bug, which is fixed in this tutorial.</span></span>

<span data-ttu-id="d8317-109">Descargue la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="d8317-109">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="d8317-110">Consta de dos proyectos: *WebApp* (una ASP.NET Core Web API) y *WebAppTests* (pruebas unitarias para la API web).</span><span class="sxs-lookup"><span data-stu-id="d8317-110">It consists of two projects: *WebApp* (an ASP.NET Core web API) and *WebAppTests* (unit tests for the web API).</span></span>

<span data-ttu-id="d8317-111">En un shell de comandos, desplácese hasta la carpeta *WebApp*.</span><span class="sxs-lookup"><span data-stu-id="d8317-111">In a command shell, navigate to the *WebApp* folder.</span></span> <span data-ttu-id="d8317-112">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="d8317-112">Run the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="d8317-113">La salida de la consola muestra mensajes similares al siguiente (indicando que la aplicación se ejecuta y espera solicitudes):</span><span class="sxs-lookup"><span data-stu-id="d8317-113">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="d8317-114">En un explorador web, vaya a `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span><span class="sxs-lookup"><span data-stu-id="d8317-114">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="d8317-115">Debería ver el resultado de `9`.</span><span class="sxs-lookup"><span data-stu-id="d8317-115">You should see the result of `9`.</span></span>

<span data-ttu-id="d8317-116">Navegue a la API del producto (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span><span class="sxs-lookup"><span data-stu-id="d8317-116">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="d8317-117">Devuelve `9`, no `20` tal como se esperaría.</span><span class="sxs-lookup"><span data-stu-id="d8317-117">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="d8317-118">Ese problema se corregirá más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="d8317-118">That problem is fixed later in the tutorial.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="d8317-119">Agregar `dotnet watch` a un proyecto</span><span class="sxs-lookup"><span data-stu-id="d8317-119">Add `dotnet watch` to a project</span></span>

<span data-ttu-id="d8317-120">La herramienta de monitor de archivos `dotnet watch` se incluye con la versión 2.1.300 del SDK de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="d8317-120">The `dotnet watch` file watcher tool is included with version 2.1.300 of the .NET Core SDK.</span></span> <span data-ttu-id="d8317-121">Si se usa una versión anterior del SDK de .NET Core, será necesario realizar los siguientes pasos.</span><span class="sxs-lookup"><span data-stu-id="d8317-121">The following steps are required when using an earlier version of the .NET Core SDK.</span></span>

1. <span data-ttu-id="d8317-122">Agregue una referencia de paquete `Microsoft.DotNet.Watcher.Tools` al archivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="d8317-122">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup>
    ```

1. <span data-ttu-id="d8317-123">Instale el paquete `Microsoft.DotNet.Watcher.Tools` mediante la ejecución del comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="d8317-123">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>

    ```console
    dotnet restore
    ```

::: moniker-end

## <a name="run-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="d8317-124">Ejecutar los comandos de la CLI de .NET Core con `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="d8317-124">Run .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="d8317-125">Cualquier [comando de la CLI de .NET Core](/dotnet/core/tools#cli-commands) se puede ejecutar con `dotnet watch`.</span><span class="sxs-lookup"><span data-stu-id="d8317-125">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="d8317-126">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d8317-126">For example:</span></span>

| <span data-ttu-id="d8317-127">Comando</span><span class="sxs-lookup"><span data-stu-id="d8317-127">Command</span></span> | <span data-ttu-id="d8317-128">Comando con watch</span><span class="sxs-lookup"><span data-stu-id="d8317-128">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="d8317-129">dotnet run</span><span class="sxs-lookup"><span data-stu-id="d8317-129">dotnet run</span></span> | <span data-ttu-id="d8317-130">dotnet watch run</span><span class="sxs-lookup"><span data-stu-id="d8317-130">dotnet watch run</span></span> |
| <span data-ttu-id="d8317-131">dotnet run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="d8317-131">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="d8317-132">dotnet watch run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="d8317-132">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="d8317-133">dotnet run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="d8317-133">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="d8317-134">dotnet watch run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="d8317-134">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="d8317-135">dotnet test</span><span class="sxs-lookup"><span data-stu-id="d8317-135">dotnet test</span></span> | <span data-ttu-id="d8317-136">dotnet watch test</span><span class="sxs-lookup"><span data-stu-id="d8317-136">dotnet watch test</span></span> |

<span data-ttu-id="d8317-137">Ejecute `dotnet watch run` en la carpeta *WebApp*.</span><span class="sxs-lookup"><span data-stu-id="d8317-137">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="d8317-138">La salida de la consola indica que se ha iniciado `watch`.</span><span class="sxs-lookup"><span data-stu-id="d8317-138">The console output indicates `watch` has started.</span></span>

## <a name="make-changes-with-dotnet-watch"></a><span data-ttu-id="d8317-139">Realizar cambios con `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="d8317-139">Make changes with `dotnet watch`</span></span>

<span data-ttu-id="d8317-140">Asegúrese de que `dotnet watch` se está ejecutando.</span><span class="sxs-lookup"><span data-stu-id="d8317-140">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="d8317-141">Corrija el error en el método `Product` de *MathController.cs* para que devuelva el producto y no la suma:</span><span class="sxs-lookup"><span data-stu-id="d8317-141">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
}
```

<span data-ttu-id="d8317-142">Guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="d8317-142">Save the file.</span></span> <span data-ttu-id="d8317-143">La salida de la consola muestra que `dotnet watch` ha detectado un cambio de archivo y ha reiniciado la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d8317-143">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="d8317-144">Compruebe que `http://localhost:<port number>/api/math/product?a=4&b=5` devuelve el resultado correcto.</span><span class="sxs-lookup"><span data-stu-id="d8317-144">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="run-tests-using-dotnet-watch"></a><span data-ttu-id="d8317-145">Ejecutar pruebas con `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="d8317-145">Run tests using `dotnet watch`</span></span>

1. <span data-ttu-id="d8317-146">Vuelva a cambiar el método `Product` de *MathController.cs* para devolver la suma.</span><span class="sxs-lookup"><span data-stu-id="d8317-146">Change the `Product` method of *MathController.cs* back to returning the sum.</span></span> <span data-ttu-id="d8317-147">Guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="d8317-147">Save the file.</span></span>
1. <span data-ttu-id="d8317-148">En un shell de comandos, desplácese hasta la carpeta *WebAppTests*.</span><span class="sxs-lookup"><span data-stu-id="d8317-148">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="d8317-149">Ejecute [dotnet restore](/dotnet/core/tools/dotnet-restore).</span><span class="sxs-lookup"><span data-stu-id="d8317-149">Run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span>
1. <span data-ttu-id="d8317-150">Ejecute `dotnet watch test`.</span><span class="sxs-lookup"><span data-stu-id="d8317-150">Run `dotnet watch test`.</span></span> <span data-ttu-id="d8317-151">La salida que indica que se ha producido un error en una prueba y que el monitor espera cambios de archivos:</span><span class="sxs-lookup"><span data-stu-id="d8317-151">Its output indicates that a test failed and that the watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="d8317-152">Corrija el código del método `Product` para que devuelva el producto.</span><span class="sxs-lookup"><span data-stu-id="d8317-152">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="d8317-153">Guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="d8317-153">Save the file.</span></span>

<span data-ttu-id="d8317-154">`dotnet watch` detecta el cambio de archivo y vuelve a ejecutar las pruebas.</span><span class="sxs-lookup"><span data-stu-id="d8317-154">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="d8317-155">La salida de la consola indica que se han superado las pruebas.</span><span class="sxs-lookup"><span data-stu-id="d8317-155">The console output indicates the tests passed.</span></span>

## <a name="customize-files-list-to-watch"></a><span data-ttu-id="d8317-156">Personalizar la lista de archivos que inspeccionar</span><span class="sxs-lookup"><span data-stu-id="d8317-156">Customize files list to watch</span></span>

<span data-ttu-id="d8317-157">`dotnet-watch` realiza un seguimiento de forma predeterminada de todos los archivos que coincidan con los siguientes patrones globales:</span><span class="sxs-lookup"><span data-stu-id="d8317-157">By default, `dotnet-watch` tracks all files matching the following glob patterns:</span></span>

* `**/*.cs`
* `*.csproj`
* `**/*.resx`

<span data-ttu-id="d8317-158">Se pueden agregar más elementos a la lista de control inspección editando el archivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="d8317-158">More items can be added to the watch list by editing the *.csproj* file.</span></span> <span data-ttu-id="d8317-159">Los elementos se pueden especificar individualmente o usando patrones globales.</span><span class="sxs-lookup"><span data-stu-id="d8317-159">Items can be specified individually or by using glob patterns.</span></span>

```xml
<ItemGroup>
    <!-- extends watching group to include *.js files -->
    <Watch Include="**\*.js" Exclude="node_modules\**\*;**\*.js.map;obj\**\*;bin\**\*" />
</ItemGroup>
```

## <a name="opt-out-of-files-to-be-watched"></a><span data-ttu-id="d8317-160">Descartar archivos de la inspección</span><span class="sxs-lookup"><span data-stu-id="d8317-160">Opt-out of files to be watched</span></span>

<span data-ttu-id="d8317-161">`dotnet-watch` se puede configurar para pasar por alto su configuración predeterminada.</span><span class="sxs-lookup"><span data-stu-id="d8317-161">`dotnet-watch` can be configured to ignore its default settings.</span></span> <span data-ttu-id="d8317-162">Para omitir archivos concretos, agregue el atributo `Watch="false"` a la definición de un elemento en el archivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="d8317-162">To ignore specific files, add the `Watch="false"` attribute to an item's definition in the *.csproj* file:</span></span>

```xml
<ItemGroup>
    <!-- exclude Generated.cs from dotnet-watch -->
    <Compile Include="Generated.cs" Watch="false" />

    <!-- exclude Strings.resx from dotnet-watch -->
    <EmbeddedResource Include="Strings.resx" Watch="false" />

    <!-- exclude changes in this referenced project -->
    <ProjectReference Include="..\ClassLibrary1\ClassLibrary1.csproj" Watch="false" />
</ItemGroup>
```

## <a name="custom-watch-projects"></a><span data-ttu-id="d8317-163">Proyectos de inspección personalizados</span><span class="sxs-lookup"><span data-stu-id="d8317-163">Custom watch projects</span></span>

<span data-ttu-id="d8317-164">`dotnet-watch` no queda restringido exclusivamente a proyectos de C#,</span><span class="sxs-lookup"><span data-stu-id="d8317-164">`dotnet-watch` isn't restricted to C# projects.</span></span> <span data-ttu-id="d8317-165">sino que se pueden crear proyectos de inspección personalizados para controlar distintos escenarios.</span><span class="sxs-lookup"><span data-stu-id="d8317-165">Custom watch projects can be created to handle different scenarios.</span></span> <span data-ttu-id="d8317-166">Veamos el siguiente diseño de proyecto:</span><span class="sxs-lookup"><span data-stu-id="d8317-166">Consider the following project layout:</span></span>

* <span data-ttu-id="d8317-167">**test/**</span><span class="sxs-lookup"><span data-stu-id="d8317-167">**test/**</span></span>
  * <span data-ttu-id="d8317-168">*UnitTests/UnitTests.csproj*</span><span class="sxs-lookup"><span data-stu-id="d8317-168">*UnitTests/UnitTests.csproj*</span></span>
  * <span data-ttu-id="d8317-169">*IntegrationTests/IntegrationTests.csproj*</span><span class="sxs-lookup"><span data-stu-id="d8317-169">*IntegrationTests/IntegrationTests.csproj*</span></span>

<span data-ttu-id="d8317-170">Si el objetivo es inspeccionar los dos proyectos, cree un archivo de proyecto personalizado configurado para supervisar ambos proyectos:</span><span class="sxs-lookup"><span data-stu-id="d8317-170">If the goal is to watch both projects, create a custom project file configured to watch both projects:</span></span>

```xml
<Project>
    <ItemGroup>
        <TestProjects Include="**\*.csproj" />
        <Watch Include="**\*.cs" />
    </ItemGroup>

    <Target Name="Test">
        <MSBuild Targets="VSTest" Projects="@(TestProjects)" />
    </Target>

    <Import Project="$(MSBuildExtensionsPath)\Microsoft.Common.targets" />
</Project>
```

<span data-ttu-id="d8317-171">Para empezar a inspeccionar archivos en ambos proyectos, cambie a la carpeta *test*.</span><span class="sxs-lookup"><span data-stu-id="d8317-171">To start file watching on both projects, change to the *test* folder.</span></span> <span data-ttu-id="d8317-172">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="d8317-172">Execute the following command:</span></span>

```console
dotnet watch msbuild /t:Test
```

<span data-ttu-id="d8317-173">VSTest se ejecuta cuando un archivo de cualquiera de los proyectos de prueba cambie.</span><span class="sxs-lookup"><span data-stu-id="d8317-173">VSTest executes when any file changes in either test project.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="d8317-174">`dotnet-watch` en GitHub</span><span class="sxs-lookup"><span data-stu-id="d8317-174">`dotnet-watch` in GitHub</span></span>

<span data-ttu-id="d8317-175">`dotnet-watch` forma parte del [repositorio de DotNetTools](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch) de GitHub.</span><span class="sxs-lookup"><span data-stu-id="d8317-175">`dotnet-watch` is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).</span></span>
