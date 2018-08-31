---
title: Perfiles de publicación de Visual Studio para la implementación de aplicaciones ASP.NET Core
author: rick-anderson
description: Aprenda a crear perfiles de publicación en Visual Studio y usarlos para administrar implementaciones de aplicaciones ASP.NET Core en diversos destinos.
ms.author: riande
ms.custom: mvc
ms.date: 04/10/2018
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 2958b83de13207b93790004a4fa60b0509af3cd2
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2018
ms.locfileid: "41902591"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="5d52b-103">Perfiles de publicación de Visual Studio para la implementación de aplicaciones ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5d52b-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="5d52b-104">Por [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5d52b-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5d52b-105">Este documento se centra en el uso de Visual Studio 2017 para crear y usar perfiles de publicación.</span><span class="sxs-lookup"><span data-stu-id="5d52b-105">This document focuses on using Visual Studio 2017 to create and use publish profiles.</span></span> <span data-ttu-id="5d52b-106">Los perfiles de publicación creados con Visual Studio se pueden ejecutar en MSBuild y en Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="5d52b-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="5d52b-107">Vea [Publicar una aplicación web de ASP.NET Core en Azure App Service con Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) para obtener instrucciones sobre la publicación en Azure.</span><span class="sxs-lookup"><span data-stu-id="5d52b-107">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="5d52b-108">El siguiente archivo de proyecto se creó con el comando `dotnet new mvc`:</span><span class="sxs-lookup"><span data-stu-id="5d52b-108">The following project file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5d52b-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5d52b-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
  </ItemGroup>

  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.0" />
  </ItemGroup>

</Project>
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5d52b-110">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5d52b-110">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.5" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.6" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.3" />
  </ItemGroup>

</Project>
```

---

<span data-ttu-id="5d52b-111">El atributo `<Project>` del elemento `Sdk` lleva a cabo las siguientes tareas:</span><span class="sxs-lookup"><span data-stu-id="5d52b-111">The `<Project>` element's `Sdk` attribute accomplishes the following tasks:</span></span>

* <span data-ttu-id="5d52b-112">Importa el archivo de propiedades de *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* al comienzo.</span><span class="sxs-lookup"><span data-stu-id="5d52b-112">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="5d52b-113">Importa el archivo targets de *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* al final.</span><span class="sxs-lookup"><span data-stu-id="5d52b-113">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="5d52b-114">La ubicación predeterminada de `MSBuildSDKsPath` (con Visual Studio 2017 Enterprise) es la carpeta *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.</span><span class="sxs-lookup"><span data-stu-id="5d52b-114">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="5d52b-115">El SDK de `Microsoft.NET.Sdk.Web` depende de:</span><span class="sxs-lookup"><span data-stu-id="5d52b-115">The `Microsoft.NET.Sdk.Web` SDK depends on:</span></span>

* <span data-ttu-id="5d52b-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="5d52b-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="5d52b-117">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="5d52b-117">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="5d52b-118">Lo que hace que se importen las propiedades y los destinos siguientes:</span><span class="sxs-lookup"><span data-stu-id="5d52b-118">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="5d52b-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="5d52b-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="5d52b-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="5d52b-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span></span>
* <span data-ttu-id="5d52b-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="5d52b-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="5d52b-122">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="5d52b-122">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span></span>

<span data-ttu-id="5d52b-123">Los destinos de publicación importan el conjunto adecuado de destinos en función del método de publicación usado.</span><span class="sxs-lookup"><span data-stu-id="5d52b-123">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="5d52b-124">Cuando se carga un proyecto en Visual Studio o MSBuild, se llevan a cabo las siguientes acciones generales:</span><span class="sxs-lookup"><span data-stu-id="5d52b-124">When MSBuild or Visual Studio loads a project, the following high-level actions occur:</span></span>

* <span data-ttu-id="5d52b-125">Compilación del proyecto</span><span class="sxs-lookup"><span data-stu-id="5d52b-125">Build project</span></span>
* <span data-ttu-id="5d52b-126">Cálculo de los archivos para la publicación</span><span class="sxs-lookup"><span data-stu-id="5d52b-126">Compute files to publish</span></span>
* <span data-ttu-id="5d52b-127">Publicación de los archivos en el destino</span><span class="sxs-lookup"><span data-stu-id="5d52b-127">Publish files to destination</span></span>

## <a name="compute-project-items"></a><span data-ttu-id="5d52b-128">Cálculo de los elementos del proyecto</span><span class="sxs-lookup"><span data-stu-id="5d52b-128">Compute project items</span></span>

<span data-ttu-id="5d52b-129">Cuando se carga el proyecto, se calculan los elementos del proyecto (archivos).</span><span class="sxs-lookup"><span data-stu-id="5d52b-129">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="5d52b-130">El atributo `item type` determina cómo se procesa el archivo.</span><span class="sxs-lookup"><span data-stu-id="5d52b-130">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="5d52b-131">De forma predeterminada, los archivos *.cs* se incluyen en la lista de elementos `Compile`.</span><span class="sxs-lookup"><span data-stu-id="5d52b-131">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="5d52b-132">Después se compilan los archivos de la lista de elementos `Compile`.</span><span class="sxs-lookup"><span data-stu-id="5d52b-132">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="5d52b-133">La lista de elementos `Content` contiene archivos que se van a publicar, además de los resultados de compilación.</span><span class="sxs-lookup"><span data-stu-id="5d52b-133">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="5d52b-134">De forma predeterminada, los archivos que coinciden con el patrón `wwwroot/**` se incluyen en el elemento `Content`.</span><span class="sxs-lookup"><span data-stu-id="5d52b-134">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="5d52b-135">El `wwwroot/\*\*` [patrón global](https://gruntjs.com/configuring-tasks#globbing-patterns) coincide con los archivos de la carpeta *wwwroot* y de las subcarpetas **y**.</span><span class="sxs-lookup"><span data-stu-id="5d52b-135">The `wwwroot/\*\*` [globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) matches all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="5d52b-136">Para agregar explícitamente un archivo a la lista de publicación, agregue el archivo directamente en el archivo *.csproj*, como se muestra en [Archivos de inclusión](#include-files).</span><span class="sxs-lookup"><span data-stu-id="5d52b-136">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Include Files](#include-files).</span></span>

<span data-ttu-id="5d52b-137">Al seleccionar el botón **Publicar** en Visual Studio o al publicar desde la línea de comandos:</span><span class="sxs-lookup"><span data-stu-id="5d52b-137">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="5d52b-138">Se calculan los elementos/propiedades (los archivos que se deben compilar).</span><span class="sxs-lookup"><span data-stu-id="5d52b-138">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="5d52b-139">**Solo para Visual Studio**: se restauran los paquetes NuGet.</span><span class="sxs-lookup"><span data-stu-id="5d52b-139">**Visual Studio only**: NuGet packages are restored.</span></span> <span data-ttu-id="5d52b-140">(la restauración debe ser explícita por parte del usuario en la CLI).</span><span class="sxs-lookup"><span data-stu-id="5d52b-140">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="5d52b-141">Se compila el proyecto.</span><span class="sxs-lookup"><span data-stu-id="5d52b-141">The project builds.</span></span>
* <span data-ttu-id="5d52b-142">Se calculan los elementos de publicación (los archivos que se deben publicar).</span><span class="sxs-lookup"><span data-stu-id="5d52b-142">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="5d52b-143">Se publica el proyecto (los archivos calculados se copian en el destino de publicación).</span><span class="sxs-lookup"><span data-stu-id="5d52b-143">The project is published (the computed files are copied to the publish destination).</span></span>

<span data-ttu-id="5d52b-144">Cuando hace referencia a un proyecto de ASP.NET Core `Microsoft.NET.Sdk.Web` en el archivo de proyecto, se coloca un archivo *app_offline.htm* en la raíz del directorio de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="5d52b-144">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="5d52b-145">Cuando el archivo está presente, el módulo de ASP.NET Core cierra correctamente la aplicación y proporciona el archivo *app_offline.htm* durante la implementación.</span><span class="sxs-lookup"><span data-stu-id="5d52b-145">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="5d52b-146">Para más información, vea [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm) (Referencia de configuración del módulo de ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="5d52b-146">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="5d52b-147">Publicación básica de línea de comandos</span><span class="sxs-lookup"><span data-stu-id="5d52b-147">Basic command-line publishing</span></span>

<span data-ttu-id="5d52b-148">La publicación de línea de comandos funciona en todas las plataformas admitidas de .NET Core y no se requiere Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5d52b-148">Command-line publishing works on all .NET Core-supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="5d52b-149">En los ejemplos siguientes, se ejecuta el comando [dotnet publish](/dotnet/core/tools/dotnet-publish) desde el directorio del proyecto (que contiene el archivo *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="5d52b-149">In the samples below, the [dotnet publish](/dotnet/core/tools/dotnet-publish) command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="5d52b-150">Si no se encuentra en la carpeta del proyecto, pase de forma explícita la ruta de acceso del archivo del proyecto.</span><span class="sxs-lookup"><span data-stu-id="5d52b-150">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="5d52b-151">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="5d52b-151">For example:</span></span>

```console
dotnet publish C:\Webs\Web1
```

<span data-ttu-id="5d52b-152">Ejecute los comandos siguientes para crear y publicar una aplicación web:</span><span class="sxs-lookup"><span data-stu-id="5d52b-152">Run the following commands to create and publish a web app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5d52b-153">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5d52b-153">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5d52b-154">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5d52b-154">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

<span data-ttu-id="5d52b-155">El comando [dotnet publish](/dotnet/core/tools/dotnet-publish) produce una salida similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="5d52b-155">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="5d52b-156">La carpeta de publicación predeterminada es `bin\$(Configuration)\netcoreapp<version>\publish`,</span><span class="sxs-lookup"><span data-stu-id="5d52b-156">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="5d52b-157">El valor predeterminado de `$(Configuration)` es *Debug*.</span><span class="sxs-lookup"><span data-stu-id="5d52b-157">The default for `$(Configuration)` is *Debug*.</span></span> <span data-ttu-id="5d52b-158">En el ejemplo anterior, el elemento `<TargetFramework>` es `netcoreapp2.0`.</span><span class="sxs-lookup"><span data-stu-id="5d52b-158">In the preceding sample, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="5d52b-159">`dotnet publish -h` muestra información de ayuda de la publicación.</span><span class="sxs-lookup"><span data-stu-id="5d52b-159">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="5d52b-160">El siguiente comando especifica una compilación `Release` y el directorio de publicación:</span><span class="sxs-lookup"><span data-stu-id="5d52b-160">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:\MyWebs\test
```

<span data-ttu-id="5d52b-161">El comando [dotnet publish](/dotnet/core/tools/dotnet-publish) llama a MSBuild, que invoca el destino `Publish`.</span><span class="sxs-lookup"><span data-stu-id="5d52b-161">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command calls MSBuild, which invokes the `Publish` target.</span></span> <span data-ttu-id="5d52b-162">Todos los parámetros pasados a `dotnet publish` se pasan a MSBuild.</span><span class="sxs-lookup"><span data-stu-id="5d52b-162">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="5d52b-163">El parámetro `-c` se asigna a la propiedad de MSBuild `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="5d52b-163">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="5d52b-164">El parámetro `-o` se asigna a `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="5d52b-164">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="5d52b-165">Se pueden pasar propiedades de MSBuild mediante cualquiera de los siguientes formatos:</span><span class="sxs-lookup"><span data-stu-id="5d52b-165">MSBuild properties can be passed using either of the following formats:</span></span>

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="5d52b-166">El comando siguiente publica una compilación `Release` en un recurso compartido de red:</span><span class="sxs-lookup"><span data-stu-id="5d52b-166">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="5d52b-167">El recurso compartido de red se especifica con barras diagonales (*//r8/*) y funciona en todas las plataformas compatibles de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="5d52b-167">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="5d52b-168">Confirme que la aplicación publicada para la implementación no se está ejecutando.</span><span class="sxs-lookup"><span data-stu-id="5d52b-168">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="5d52b-169">Los archivos de la carpeta *publish* quedan bloqueados mientras se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5d52b-169">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="5d52b-170">No se puede llevar a cabo la implementación porque no se pueden copiar los archivos bloqueados.</span><span class="sxs-lookup"><span data-stu-id="5d52b-170">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="5d52b-171">Publicar los perfiles</span><span class="sxs-lookup"><span data-stu-id="5d52b-171">Publish profiles</span></span>

<span data-ttu-id="5d52b-172">En esta sección se usa Visual Studio 2017 para crear un perfil de publicación.</span><span class="sxs-lookup"><span data-stu-id="5d52b-172">This section uses Visual Studio 2017 to create a publishing profile.</span></span> <span data-ttu-id="5d52b-173">Una vez creado, es posible publicar desde Visual Studio o la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="5d52b-173">Once created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="5d52b-174">Los perfiles de publicación pueden simplificar el proceso de publicación, y puede existir cualquier número de perfiles.</span><span class="sxs-lookup"><span data-stu-id="5d52b-174">Publish profiles can simplify the publishing process, and any number of profiles can exist.</span></span> <span data-ttu-id="5d52b-175">Cree un perfil de publicación en Visual Studio mediante la selección de una de las rutas de acceso siguientes:</span><span class="sxs-lookup"><span data-stu-id="5d52b-175">Create a publish profile in Visual Studio by choosing one of the following paths:</span></span>

* <span data-ttu-id="5d52b-176">Haga clic con el botón derecho en el Explorador de soluciones y seleccione **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="5d52b-176">Right-click the project in Solution Explorer and select **Publish**.</span></span>
* <span data-ttu-id="5d52b-177">Seleccione **Publicar &lt;nombre_del_proyecto&gt;** en el menú **Compilar**.</span><span class="sxs-lookup"><span data-stu-id="5d52b-177">Select **Publish &lt;project_name&gt;** from the **Build** menu.</span></span>

<span data-ttu-id="5d52b-178">Aparece la pestaña **Publicar** de la página de funcionalidades de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5d52b-178">The **Publish** tab of the app capacities page is displayed.</span></span> <span data-ttu-id="5d52b-179">Si el proyecto no contiene ningún perfil de publicación, se muestra la página siguiente:</span><span class="sxs-lookup"><span data-stu-id="5d52b-179">If the project lacks a publish profile, the following page is displayed:</span></span>

![La pestaña Publicar de la página de funcionalidades de la aplicación](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="5d52b-181">Cuando se seleccione **Carpeta**, especifique una ruta de acceso a la carpeta para almacenar los recursos publicados.</span><span class="sxs-lookup"><span data-stu-id="5d52b-181">When **Folder** is selected, specify a folder path to store the published assets.</span></span> <span data-ttu-id="5d52b-182">La carpeta predeterminada es *bin\Release\PublishOutput*.</span><span class="sxs-lookup"><span data-stu-id="5d52b-182">The default folder is *bin\Release\PublishOutput*.</span></span> <span data-ttu-id="5d52b-183">Haga clic en el botón **Crear perfil** para terminar.</span><span class="sxs-lookup"><span data-stu-id="5d52b-183">Click the **Create Profile** button to finish.</span></span>

<span data-ttu-id="5d52b-184">Una vez que se crea un perfil de publicación, la pestaña **Publicar** cambia.</span><span class="sxs-lookup"><span data-stu-id="5d52b-184">Once a publish profile is created, the **Publish** tab changes.</span></span> <span data-ttu-id="5d52b-185">El perfil recién creado aparece en una lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="5d52b-185">The newly created profile appears in a drop-down list.</span></span> <span data-ttu-id="5d52b-186">Haga clic en **Crear nuevo perfil** para crear otro nuevo perfil.</span><span class="sxs-lookup"><span data-stu-id="5d52b-186">Click **Create new profile** to create another new profile.</span></span>

![La pestaña Publicar de la página de funcionalidades de la aplicación que muestra FolderProfile](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="5d52b-188">El Asistente para publicación admite los siguientes destinos de publicación:</span><span class="sxs-lookup"><span data-stu-id="5d52b-188">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="5d52b-189">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="5d52b-189">Azure App Service</span></span>
* <span data-ttu-id="5d52b-190">Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="5d52b-190">Azure Virtual Machines</span></span>
* <span data-ttu-id="5d52b-191">IIS, FTP, etc. (para cualquier servidor web)</span><span class="sxs-lookup"><span data-stu-id="5d52b-191">IIS, FTP, etc. (for any web server)</span></span>
* <span data-ttu-id="5d52b-192">Carpeta</span><span class="sxs-lookup"><span data-stu-id="5d52b-192">Folder</span></span>
* <span data-ttu-id="5d52b-193">Perfil de importación</span><span class="sxs-lookup"><span data-stu-id="5d52b-193">Import Profile</span></span>

<span data-ttu-id="5d52b-194">Para más información, consulte [¿Qué opciones de publicación son las adecuadas para mí?](/visualstudio/ide/not-in-toc/web-publish-options)</span><span class="sxs-lookup"><span data-stu-id="5d52b-194">For more information, see [What publishing options are right for me](/visualstudio/ide/not-in-toc/web-publish-options).</span></span>

<span data-ttu-id="5d52b-195">Al crear un perfil de publicación con Visual Studio, se crea un archivo de MSBuild *Properties/PublishProfiles/&lt;nombre_del_perfil&gt;.pubxml*.</span><span class="sxs-lookup"><span data-stu-id="5d52b-195">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="5d52b-196">Este archivo *.pubxml* es un archivo de MSBuild que contiene la configuración de publicación.</span><span class="sxs-lookup"><span data-stu-id="5d52b-196">The *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="5d52b-197">Este archivo se puede modificar para personalizar el proceso de compilación y publicación.</span><span class="sxs-lookup"><span data-stu-id="5d52b-197">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="5d52b-198">Este archivo se lee durante el proceso de publicación.</span><span class="sxs-lookup"><span data-stu-id="5d52b-198">This file is read by the publishing process.</span></span> <span data-ttu-id="5d52b-199">`<LastUsedBuildConfiguration>` es especial porque es una propiedad global y no debe estar en ningún archivo importado en la compilación.</span><span class="sxs-lookup"><span data-stu-id="5d52b-199">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="5d52b-200">Consulte [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) (MSBuild: Cómo establecer la propiedad de configuración) para más información.</span><span class="sxs-lookup"><span data-stu-id="5d52b-200">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more information.</span></span>

<span data-ttu-id="5d52b-201">Al publicar en un destino de Azure, el archivo *.pubxml* contiene el identificador de suscripción de Azure.</span><span class="sxs-lookup"><span data-stu-id="5d52b-201">When publishing to an Azure target, the *.pubxml* file contains your Azure subscription identifier.</span></span> <span data-ttu-id="5d52b-202">Con ese tipo de destino, no se recomienda agregar este archivo al control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="5d52b-202">With that target type, adding this file to source control is discouraged.</span></span> <span data-ttu-id="5d52b-203">Al publicar en un destino que no sea Azure, es seguro insertar el archivo *.pubxml* en el repositorio.</span><span class="sxs-lookup"><span data-stu-id="5d52b-203">When publishing to a non-Azure target, it's safe to check in the *.pubxml* file.</span></span>

<span data-ttu-id="5d52b-204">La información confidencial (por ejemplo, la contraseña de publicación) se cifra en cada usuario y máquina.</span><span class="sxs-lookup"><span data-stu-id="5d52b-204">Sensitive information (like the publish password) is encrypted on a per user/machine level.</span></span> <span data-ttu-id="5d52b-205">Se almacena en el archivo *Properties/PublishProfiles/&lt;nombre_del_perfil&gt;.pubxml.user*.</span><span class="sxs-lookup"><span data-stu-id="5d52b-205">It's stored in the *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml.user* file.</span></span> <span data-ttu-id="5d52b-206">Como este archivo puede contener información confidencial, no se debe insertar en el repositorio de control del código fuente.</span><span class="sxs-lookup"><span data-stu-id="5d52b-206">Because this file can store sensitive information, it shouldn't be checked into source control.</span></span>

<span data-ttu-id="5d52b-207">Para información general sobre cómo publicar una aplicación web en ASP.NET Core, consulte [Hospedaje e implementación](xref:host-and-deploy/index),</span><span class="sxs-lookup"><span data-stu-id="5d52b-207">For an overview of how to publish a web app on ASP.NET Core, see [Host and deploy](xref:host-and-deploy/index).</span></span> <span data-ttu-id="5d52b-208">Las tareas de MSBuild y los destinos necesarios para publicar una aplicación ASP.NET Core son de código abierto en https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="5d52b-208">The MSBuild tasks and targets necessary to publish an ASP.NET Core app are open-source at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="5d52b-209">`dotnet publish` puede usar perfiles de publicación, de carpeta, Msdeploy y [Kudu](https://github.com/projectkudu/kudu/wiki):</span><span class="sxs-lookup"><span data-stu-id="5d52b-209">`dotnet publish` can use folder, MSDeploy, and [Kudu](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>

<span data-ttu-id="5d52b-210">Carpeta (funciona entre plataformas)</span><span class="sxs-lookup"><span data-stu-id="5d52b-210">Folder (works cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

<span data-ttu-id="5d52b-211">MSDeploy (actualmente solo funciona en Windows dado que MSDeploy no es multiplataforma):</span><span class="sxs-lookup"><span data-stu-id="5d52b-211">MSDeploy (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

<span data-ttu-id="5d52b-212">El paquete de MSDeploy (actualmente solo funciona en Windows dado que MSDeploy no es multiplataforma):</span><span class="sxs-lookup"><span data-stu-id="5d52b-212">MSDeploy package (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

<span data-ttu-id="5d52b-213">En los ejemplos anteriores, **no** pase `deployonbuild` a `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="5d52b-213">In the preceding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="5d52b-214">Para más información, consulte [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="5d52b-214">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="5d52b-215">`dotnet publish` admite las API de Kudu para publicar en Azure desde cualquier plataforma.</span><span class="sxs-lookup"><span data-stu-id="5d52b-215">`dotnet publish` supports Kudu APIs to publish to Azure from any platform.</span></span> <span data-ttu-id="5d52b-216">La publicación de Visual Studio admite API de Kudu, pero es compatible con WebSDK para la publicación multiplataforma en Azure.</span><span class="sxs-lookup"><span data-stu-id="5d52b-216">Visual Studio publish supports the Kudu APIs, but it's supported by WebSDK for cross-platform publish to Azure.</span></span>

<span data-ttu-id="5d52b-217">Agregue un perfil de publicación a la carpeta *Properties/PublishProfiles* con el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="5d52b-217">Add a publish profile to the *Properties/PublishProfiles* folder with the following content:</span></span>

```xml
<Project>
  <PropertyGroup>
    <PublishProtocol>Kudu</PublishProtocol>
    <PublishSiteName>nodewebapp</PublishSiteName>
    <UserName>username</UserName>
    <Password>password</Password>
  </PropertyGroup>
</Project>
```

<span data-ttu-id="5d52b-218">Ejecute el comando siguiente para descomprimir el contenido de publicación y publicarlo en Azure mediante las API de Kudu:</span><span class="sxs-lookup"><span data-stu-id="5d52b-218">Run the following command to zip up the publish contents and publish it to Azure using the Kudu APIs:</span></span>

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

<span data-ttu-id="5d52b-219">Establezca las siguientes propiedades de MSBuild cuando use un perfil de publicación:</span><span class="sxs-lookup"><span data-stu-id="5d52b-219">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

<span data-ttu-id="5d52b-220">Por ejemplo, al efectuar una publicación con un perfil denominado *FolderProfile*, se puede ejecutar cualquiera de los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="5d52b-220">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="5d52b-221">Al invocar [dotnet build](/dotnet/core/tools/dotnet-build), se llama a `msbuild` para ejecutar el proceso de compilación y publicación.</span><span class="sxs-lookup"><span data-stu-id="5d52b-221">When invoking [dotnet build](/dotnet/core/tools/dotnet-build), it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="5d52b-222">Llamar a `dotnet build` o `msbuild` es equivalente cuando se pasa un perfil de carpeta.</span><span class="sxs-lookup"><span data-stu-id="5d52b-222">Calling either `dotnet build` or `msbuild` is equivalent when passing in a folder profile.</span></span> <span data-ttu-id="5d52b-223">Al llamar a MSBuild directamente en Windows, se usa la versión .NET Framework de MSBuild.</span><span class="sxs-lookup"><span data-stu-id="5d52b-223">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="5d52b-224">Actualmente, MSDeploy está limitado a los equipos con Windows para efectuar publicaciones.</span><span class="sxs-lookup"><span data-stu-id="5d52b-224">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="5d52b-225">Al llamar a `dotnet build` en un perfil que no es de carpeta se invoca a MSBuild, que usa MSDeploy en los perfiles que no son de carpeta.</span><span class="sxs-lookup"><span data-stu-id="5d52b-225">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="5d52b-226">Al llamar a `dotnet build` en un perfil que no es de carpeta, se invoca a MSBuild (mediante MSDeploy), lo cual genera un error (incluso si se ejecuta en una plataforma de Windows).</span><span class="sxs-lookup"><span data-stu-id="5d52b-226">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="5d52b-227">Para publicar con un perfil que no es de carpeta, llame directamente a MSBuild.</span><span class="sxs-lookup"><span data-stu-id="5d52b-227">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="5d52b-228">El siguiente perfil de publicación de carpeta se creó con Visual Studio y se publica en un recurso compartido de red:</span><span class="sxs-lookup"><span data-stu-id="5d52b-228">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework>netcoreapp1.1</PublishFramework>
    <ProjectGuid>c30c453c-312e-40c4-aec9-394a145dee0b</ProjectGuid>
    <publishUrl>\\r8\Release\AdminWeb</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
</Project>
```

<span data-ttu-id="5d52b-229">Tenga en cuenta que `<LastUsedBuildConfiguration>` está establecido en `Release`.</span><span class="sxs-lookup"><span data-stu-id="5d52b-229">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="5d52b-230">Al efectuar una publicación en Visual Studio, el valor de la propiedad de configuración `<LastUsedBuildConfiguration>` se establece con el valor cuando se inicia el proceso de publicación.</span><span class="sxs-lookup"><span data-stu-id="5d52b-230">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="5d52b-231">La propiedad de configuración `<LastUsedBuildConfiguration>` es especial y no se debe reemplazar en un archivo importado de MSBuild.</span><span class="sxs-lookup"><span data-stu-id="5d52b-231">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="5d52b-232">Esta propiedad se puede invalidar desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="5d52b-232">This property can be overridden from the command line.</span></span>

<span data-ttu-id="5d52b-233">Con la CLI de .NET Core:</span><span class="sxs-lookup"><span data-stu-id="5d52b-233">Using the .NET Core CLI:</span></span>

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

<span data-ttu-id="5d52b-234">Con MSBuild:</span><span class="sxs-lookup"><span data-stu-id="5d52b-234">Using MSBuild:</span></span>

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="5d52b-235">Publicar en un punto de conexión de MSDeploy desde la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="5d52b-235">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="5d52b-236">La publicación se puede realizar mediante la CLI de .NET Core o MSBuild.</span><span class="sxs-lookup"><span data-stu-id="5d52b-236">Publishing can be accomplished using the .NET Core CLI or MSBuild.</span></span> <span data-ttu-id="5d52b-237">`dotnet publish` se ejecuta en el contexto de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="5d52b-237">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="5d52b-238">El comando `msbuild` requiere .NET Framework, de forma que se limita a los entornos de Windows.</span><span class="sxs-lookup"><span data-stu-id="5d52b-238">The `msbuild` command requires .NET Framework, which limits it to Windows environments.</span></span>

<span data-ttu-id="5d52b-239">La forma más fácil de publicar con MSDeploy consiste en crear primero un perfil de publicación en Visual Studio 2017 y, luego, usar el perfil en la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="5d52b-239">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="5d52b-240">En el ejemplo siguiente, se crea una aplicación web de ASP.NET Core (con `dotnet new mvc`) y se agrega un perfil de publicación de Azure con Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5d52b-240">In the following sample, an ASP.NET Core web app is created (using `dotnet new mvc`), and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="5d52b-241">Ejecute `msbuild` desde un **Símbolo del sistema para desarrolladores de VS 2017**.</span><span class="sxs-lookup"><span data-stu-id="5d52b-241">Run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="5d52b-242">El Símbolo del sistema para desarrolladores tiene el archivo *msbuild.exe* correcto en su ruta de acceso con algunas variables de MSBuild establecidas.</span><span class="sxs-lookup"><span data-stu-id="5d52b-242">The Developer Command Prompt has the correct *msbuild.exe* in its path with some MSBuild variables set.</span></span>

<span data-ttu-id="5d52b-243">MSBuild usa la sintaxis siguiente:</span><span class="sxs-lookup"><span data-stu-id="5d52b-243">MSBuild uses the following syntax:</span></span>

```console
msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>
```

<span data-ttu-id="5d52b-244">Obtenga el valor `Password` del archivo *\<nombre de la publicación>.PublishSettings*.</span><span class="sxs-lookup"><span data-stu-id="5d52b-244">Get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="5d52b-245">Descargue el archivo *.PublishSettings* desde:</span><span class="sxs-lookup"><span data-stu-id="5d52b-245">Download the *.PublishSettings* file from either:</span></span>

* <span data-ttu-id="5d52b-246">Explorador de soluciones: haga clic con el botón derecho en la aplicación web y seleccione **Descargar perfil de publicación**.</span><span class="sxs-lookup"><span data-stu-id="5d52b-246">Solution Explorer: Right-click on the Web App and select **Download Publish Profile**.</span></span>
* <span data-ttu-id="5d52b-247">Azure Portal: haga clic en **Get publish profile** (Obtener perfil de publicación) en el panel **Overview** (Información general) de Web Apps.</span><span class="sxs-lookup"><span data-stu-id="5d52b-247">Azure portal: Click **Get publish profile** on the Web App's **Overview** panel.</span></span>

<span data-ttu-id="5d52b-248">`Username` está en el perfil de publicación.</span><span class="sxs-lookup"><span data-stu-id="5d52b-248">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="5d52b-249">En el siguiente ejemplo se usa el perfil de publicación *Web11112 - Web Deploy*:</span><span class="sxs-lookup"><span data-stu-id="5d52b-249">The following sample uses the *Web11112 - Web Deploy* publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="exclude-files"></a><span data-ttu-id="5d52b-250">Archivos de exclusión</span><span class="sxs-lookup"><span data-stu-id="5d52b-250">Exclude files</span></span>

<span data-ttu-id="5d52b-251">Al publicar aplicaciones web de ASP.NET Core, se incluyen los artefactos de compilación y el contenido de la carpeta *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="5d52b-251">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="5d52b-252">`msbuild` admite los [patrones globales](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="5d52b-252">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="5d52b-253">Por ejemplo, el siguiente elemento `<Content>` excluye todo los archivos de texto (*.txt*) de la carpeta *wwwroot/content* y de todas sus subcarpetas.</span><span class="sxs-lookup"><span data-stu-id="5d52b-253">For example, the following `<Content>` element excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="5d52b-254">El marcado anterior se puede agregar a un perfil de publicación o al archivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="5d52b-254">The preceding markup can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="5d52b-255">Si se agrega al archivo *.csproj*, la regla se agrega a todos los perfiles de publicación del proyecto.</span><span class="sxs-lookup"><span data-stu-id="5d52b-255">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="5d52b-256">El siguiente elemento `<MsDeploySkipRules>` excluye todos los archivos de la carpeta *wwwroot/content*:</span><span class="sxs-lookup"><span data-stu-id="5d52b-256">The following `<MsDeploySkipRules>` element excludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="5d52b-257">`<MsDeploySkipRules>` no eliminará del sitio de implementación los destinos *skip*.</span><span class="sxs-lookup"><span data-stu-id="5d52b-257">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="5d52b-258">Los archivos y carpetas destinados a `<Content>` se eliminan del sitio de implementación.</span><span class="sxs-lookup"><span data-stu-id="5d52b-258">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="5d52b-259">Por ejemplo, suponga que una aplicación web implementada tenía los siguientes archivos:</span><span class="sxs-lookup"><span data-stu-id="5d52b-259">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="5d52b-260">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="5d52b-260">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="5d52b-261">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="5d52b-261">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="5d52b-262">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="5d52b-262">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="5d52b-263">Si se agregan los siguientes elementos `<MsDeploySkipRules>`, esos archivos no se eliminarán en el sitio de implementación.</span><span class="sxs-lookup"><span data-stu-id="5d52b-263">If the following `<MsDeploySkipRules>` elements are added, those files wouldn't be deleted on the deployment site.</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About1.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About2.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About3.cshtml</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="5d52b-264">Los elementos `<MsDeploySkipRules>` anteriores impiden que se implementen los archivos *omitidos*.</span><span class="sxs-lookup"><span data-stu-id="5d52b-264">The preceding `<MsDeploySkipRules>` elements prevent the *skipped* files from being deployed.</span></span> <span data-ttu-id="5d52b-265">Una vez que se implementan esos archivos, no se eliminarán.</span><span class="sxs-lookup"><span data-stu-id="5d52b-265">It won't delete those files once they're deployed.</span></span>

<span data-ttu-id="5d52b-266">El siguiente elemento `<Content>` elimina los archivos de destino en el sitio de implementación:</span><span class="sxs-lookup"><span data-stu-id="5d52b-266">The following `<Content>` element deletes the targeted files at the deployment site:</span></span>

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="5d52b-267">El uso de la implementación de línea de comandos con el elemento `<Content>` anterior, produce la siguiente salida:</span><span class="sxs-lookup"><span data-stu-id="5d52b-267">Using command-line deployment with the preceding `<Content>` element yields the following output:</span></span>

```console
MSDeployPublish:
  Starting Web deployment task from source: manifest(C:\Webs\Web1\obj\Release\netcoreapp1.1\PubTmp\Web1.SourceManifest.
  xml) to Destination: auto().
  Deleting file (Web11112\Views\Home\About1.cshtml).
  Deleting file (Web11112\Views\Home\About2.cshtml).
  Deleting file (Web11112\Views\Home\About3.cshtml).
  Updating file (Web11112\web.config).
  Updating file (Web11112\Web1.deps.json).
  Updating file (Web11112\Web1.dll).
  Updating file (Web11112\Web1.pdb).
  Updating file (Web11112\Web1.runtimeconfig.json).
  Successfully executed Web deployment task.
  Publish Succeeded.
Done Building Project "C:\Webs\Web1\Web1.csproj" (default targets).
```

## <a name="include-files"></a><span data-ttu-id="5d52b-268">Archivos de inclusión</span><span class="sxs-lookup"><span data-stu-id="5d52b-268">Include files</span></span>

<span data-ttu-id="5d52b-269">El siguiente marcado incluye una carpeta *images* fuera del directorio del proyecto a la carpeta *wwwroot/images* del sitio de publicación:</span><span class="sxs-lookup"><span data-stu-id="5d52b-269">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="5d52b-270">El marcado se puede agregar al archivo *.csproj* o al perfil de publicación.</span><span class="sxs-lookup"><span data-stu-id="5d52b-270">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="5d52b-271">Si se agrega al archivo *.csproj*, se incluye en todos los perfiles de publicación del proyecto.</span><span class="sxs-lookup"><span data-stu-id="5d52b-271">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="5d52b-272">En el siguiente marcado resaltado se muestra cómo:</span><span class="sxs-lookup"><span data-stu-id="5d52b-272">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="5d52b-273">Copiar un archivo de fuera del proyecto a la carpeta *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="5d52b-273">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="5d52b-274">Excluir la carpeta *wwwroot\Content*.</span><span class="sxs-lookup"><span data-stu-id="5d52b-274">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="5d52b-275">Excluir *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5d52b-275">Exclude *Views\Home\About2.cshtml*.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework />
    <ProjectGuid>afa9f185-7ce0-4935-9da1-ab676229d68a</ProjectGuid>
    <publishUrl>bin\Release\PublishOutput</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
  <ItemGroup>
    <ResolvedFileToPublish Include="..\ReadMe2.MD">
      <RelativePath>wwwroot\ReadMe2.MD</RelativePath>
    </ResolvedFileToPublish>

    <Content Update="wwwroot\Content\**\*" CopyToPublishDirectory="Never" />
    <Content Update="Views\Home\About2.cshtml" CopyToPublishDirectory="Never" />

  </ItemGroup>
</Project>
```

<span data-ttu-id="5d52b-276">Vea [WebSDK Readme](https://github.com/aspnet/websdk) (Archivo Léame de WebSDK) para ver más ejemplos de implementación.</span><span class="sxs-lookup"><span data-stu-id="5d52b-276">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

## <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="5d52b-277">Ejecutar un destino antes o después de la publicación</span><span class="sxs-lookup"><span data-stu-id="5d52b-277">Run a target before or after publishing</span></span>

<span data-ttu-id="5d52b-278">Los destinos `BeforePublish` y `AfterPublish` ejecutan un destino antes o después del destino de publicación.</span><span class="sxs-lookup"><span data-stu-id="5d52b-278">The built-in `BeforePublish` and `AfterPublish` targets execute a target before or after the publish target.</span></span> <span data-ttu-id="5d52b-279">Agregue los siguientes elementos al perfil de publicación para registrar mensajes de la consola antes y después de la publicación:</span><span class="sxs-lookup"><span data-stu-id="5d52b-279">Add the following elements to the publish profile to log console messages both before and after publishing:</span></span>

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a><span data-ttu-id="5d52b-280">Publicación en un servidor mediante un certificado que no es de confianza</span><span class="sxs-lookup"><span data-stu-id="5d52b-280">Publish to a server using an untrusted certificate</span></span>

<span data-ttu-id="5d52b-281">Agregue la propiedad `<AllowUntrustedCertificate>` con un valor de `True` al perfil de publicación:</span><span class="sxs-lookup"><span data-stu-id="5d52b-281">Add the `<AllowUntrustedCertificate>` property with a value of `True` to the publish profile:</span></span>

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a><span data-ttu-id="5d52b-282">Servicio Kudu</span><span class="sxs-lookup"><span data-stu-id="5d52b-282">The Kudu service</span></span>

<span data-ttu-id="5d52b-283">Para ver los archivos de la implementación de una aplicación web de Azure App Service, use el [servicio Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="5d52b-283">To view the files in an Azure App Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="5d52b-284">Anexe el token `scm` al nombre de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="5d52b-284">Append the `scm` token to the web app name.</span></span> <span data-ttu-id="5d52b-285">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="5d52b-285">For example:</span></span>

| <span data-ttu-id="5d52b-286">Resolución</span><span class="sxs-lookup"><span data-stu-id="5d52b-286">URL</span></span>                                    | <span data-ttu-id="5d52b-287">Resultado</span><span class="sxs-lookup"><span data-stu-id="5d52b-287">Result</span></span>       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="5d52b-288">Aplicación web</span><span class="sxs-lookup"><span data-stu-id="5d52b-288">Web App</span></span>      |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="5d52b-289">Servicio Kudu</span><span class="sxs-lookup"><span data-stu-id="5d52b-289">Kudu service</span></span> |

<span data-ttu-id="5d52b-290">Seleccione el elemento de menú [Consola de depuración](https://github.com/projectkudu/kudu/wiki/Kudu-console) para ver, editar, eliminar o agregar archivos.</span><span class="sxs-lookup"><span data-stu-id="5d52b-290">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view, edit, delete, or add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5d52b-291">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="5d52b-291">Additional resources</span></span>

* <span data-ttu-id="5d52b-292">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifica la implementación de aplicaciones web y sitios web en servidores de IIS.</span><span class="sxs-lookup"><span data-stu-id="5d52b-292">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="5d52b-293">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): problemas de archivos y características de solicitud para la implementación.</span><span class="sxs-lookup"><span data-stu-id="5d52b-293">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
* [<span data-ttu-id="5d52b-294">Publicación de una aplicación web ASP.NET en una máquina virtual de Azure desde Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5d52b-294">Publish an ASP.NET Web App to an Azure VM from Visual Studio</span></span>](/azure/virtual-machines/windows/publish-web-app-from-visual-studio)
