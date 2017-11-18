---
title: "Crear perfiles de publicación para Visual Studio y MSBuild"
author: rick-anderson
description: "Explica la publicación web en Visual Studio."
keywords: "ASP.NET Core, publicación web, publicación, msbuild, implementación web, dotnet publish, Visual Studio 2017"
ms.author: riande
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.assetid: 0377a02d-8fda-47a5-929a-24a16e1d2c93
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/web-publishing-vs
ms.openlocfilehash: f010f9d90165ce4d6718fe1440e600985f21a01d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="create-publish-profiles-for-visual-studio-and-msbuild-to-deploy-aspnet-core-apps"></a><span data-ttu-id="39760-104">Crear perfiles de publicación para Visual Studio y MSBuild para implementar aplicaciones ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="39760-104">Create publish profiles for Visual Studio and MSBuild, to deploy ASP.NET Core apps</span></span>

<span data-ttu-id="39760-105">Por [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="39760-105">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="39760-106">Este artículo se centra en el uso de Visual Studio 2017 para crear perfiles de publicación.</span><span class="sxs-lookup"><span data-stu-id="39760-106">This article focuses on using Visual Studio 2017 to create publish profiles.</span></span> <span data-ttu-id="39760-107">Los perfiles de publicación creados con Visual Studio se pueden ejecutar en MSBuild y en Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="39760-107">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="39760-108">El artículo proporciona detalles sobre el proceso de publicación.</span><span class="sxs-lookup"><span data-stu-id="39760-108">The article provides details of the publishing process.</span></span> <span data-ttu-id="39760-109">Vea [Publicar una aplicación web de ASP.NET Core en Azure App Service con Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) para obtener instrucciones sobre la publicación en Azure.</span><span class="sxs-lookup"><span data-stu-id="39760-109">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="39760-110">El siguiente archivo *.csproj* se creó con el comando `dotnet new mvc`:</span><span class="sxs-lookup"><span data-stu-id="39760-110">The following *.csproj* file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="39760-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="39760-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="39760-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="39760-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.1" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.2" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.1" />
  </ItemGroup>

</Project>
```

---

<span data-ttu-id="39760-113">El atributo `Sdk` del elemento `<Project>` (en la primera línea) del marcado anterior hace lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="39760-113">The `Sdk` attribute in the `<Project>` element (in the first line) of the markup above does the following:</span></span>

* <span data-ttu-id="39760-114">Importa el archivo `props` de *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* al principio.</span><span class="sxs-lookup"><span data-stu-id="39760-114">Imports the `props` file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="39760-115">Importa el archivo targets de *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* al final.</span><span class="sxs-lookup"><span data-stu-id="39760-115">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="39760-116">La ubicación predeterminada de `MSBuildSDKsPath` (con Visual Studio 2017 Enterprise) es la carpeta *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.</span><span class="sxs-lookup"><span data-stu-id="39760-116">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="39760-117">`Microsoft.NET.Sdk.Web` depende de:</span><span class="sxs-lookup"><span data-stu-id="39760-117">`Microsoft.NET.Sdk.Web` depends on:</span></span>

* <span data-ttu-id="39760-118">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="39760-118">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="39760-119">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="39760-119">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="39760-120">Esto hace que se importen los siguientes archivos `props` y `targets`:</span><span class="sxs-lookup"><span data-stu-id="39760-120">Which causes the following `props` and `targets` to be imported:</span></span>

* <span data-ttu-id="39760-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="39760-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="39760-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="39760-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span></span>
* <span data-ttu-id="39760-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="39760-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="39760-124">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="39760-124">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span></span>

<span data-ttu-id="39760-125">La publicación de los archivos targets volverá a importar el conjunto correcto de archivos targets en función del método de publicación empleado.</span><span class="sxs-lookup"><span data-stu-id="39760-125">Publish targets will again import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="39760-126">Cuando Visual Studio o MSBuild carga un proyecto, se llevan a cabo las siguientes acciones de alto nivel:</span><span class="sxs-lookup"><span data-stu-id="39760-126">When MSBuild or Visual Studio loads a project, the following high level actions are performed:</span></span>

* <span data-ttu-id="39760-127">Compilación del proyecto</span><span class="sxs-lookup"><span data-stu-id="39760-127">Build project</span></span>
* <span data-ttu-id="39760-128">Cálculo de los archivos para la publicación</span><span class="sxs-lookup"><span data-stu-id="39760-128">Compute files to publish</span></span>
* <span data-ttu-id="39760-129">Publicación de los archivos en el destino</span><span class="sxs-lookup"><span data-stu-id="39760-129">Publish files to destination</span></span>

### <a name="compute-project-items"></a><span data-ttu-id="39760-130">Cálculo de los elementos del proyecto</span><span class="sxs-lookup"><span data-stu-id="39760-130">Compute project items</span></span>

<span data-ttu-id="39760-131">Cuando se carga el proyecto, se calculan los elementos del proyecto (archivos).</span><span class="sxs-lookup"><span data-stu-id="39760-131">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="39760-132">El atributo `item type` determina cómo se procesa el archivo.</span><span class="sxs-lookup"><span data-stu-id="39760-132">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="39760-133">De forma predeterminada, los archivos *.cs* se incluyen en la lista de elementos `Compile`.</span><span class="sxs-lookup"><span data-stu-id="39760-133">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="39760-134">Después se compilan los archivos de la lista de elementos `Compile`.</span><span class="sxs-lookup"><span data-stu-id="39760-134">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="39760-135">La lista de elementos `Content` contiene archivos que se van a publicar, además de los resultados de compilación.</span><span class="sxs-lookup"><span data-stu-id="39760-135">The `Content` item list contains files that will be published in addition to the build outputs.</span></span> <span data-ttu-id="39760-136">De forma predeterminada, los archivos que coincidan con el patrón wwwroot/** se incluirán en el elemento `Content`.</span><span class="sxs-lookup"><span data-stu-id="39760-136">By default, files matching the pattern wwwroot/** will be included in the `Content` item.</span></span> <span data-ttu-id="39760-137">[wwwroot/** es un patrón global](https://gruntjs.com/configuring-tasks#globbing-patterns) que especifica todos los archivos de la carpeta *wwwroot* **y** de las subcarpetas.</span><span class="sxs-lookup"><span data-stu-id="39760-137">[wwwroot/** is a globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) that specifies all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="39760-138">Para agregar explícitamente un archivo a la lista de publicación, agregue el archivo directamente en el archivo *.csproj*, como se muestra en [Incluir archivos](#including-files).</span><span class="sxs-lookup"><span data-stu-id="39760-138">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Including Files](#including-files).</span></span>

<span data-ttu-id="39760-139">Al seleccionar el botón **Publicar** en Visual Studio o al efectuar una publicación desde la línea de comandos:</span><span class="sxs-lookup"><span data-stu-id="39760-139">When you select the **Publish** button in Visual Studio or when you publish from command line:</span></span>

- <span data-ttu-id="39760-140">Se calculan los elementos/propiedades (los archivos que se deben compilar).</span><span class="sxs-lookup"><span data-stu-id="39760-140">The properties/items are computed (the files that are needed to build).</span></span>
- <span data-ttu-id="39760-141">Solo para Visual Studio: se restauran los paquetes NuGet</span><span class="sxs-lookup"><span data-stu-id="39760-141">Visual Studio only: NuGet packages are restored.</span></span>  <span data-ttu-id="39760-142">(la restauración debe ser explícita por parte del usuario en la CLI).</span><span class="sxs-lookup"><span data-stu-id="39760-142">(Restore needs to be explicit by the user on the CLI.)</span></span>
- <span data-ttu-id="39760-143">Se compila el proyecto.</span><span class="sxs-lookup"><span data-stu-id="39760-143">The project builds.</span></span>
- <span data-ttu-id="39760-144">Se calculan los elementos de publicación (los archivos que se deben publicar).</span><span class="sxs-lookup"><span data-stu-id="39760-144">The publish items are computed (the files that are needed to publish).</span></span>
- <span data-ttu-id="39760-145">Se publica el proyecto</span><span class="sxs-lookup"><span data-stu-id="39760-145">The project is published.</span></span> <span data-ttu-id="39760-146">(los archivos calculados se copian en el destino de publicación).</span><span class="sxs-lookup"><span data-stu-id="39760-146">(The computed files are copied to the publish destination.)</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="39760-147">Publicación básica de línea de comandos</span><span class="sxs-lookup"><span data-stu-id="39760-147">Basic command line publishing</span></span>

<span data-ttu-id="39760-148">Esta sección es válida para todas las plataformas compatibles de .NET Core y no es necesario disponer de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="39760-148">This section works on all .NET Core supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="39760-149">En los ejemplos siguientes, el comando `dotnet publish` se ejecuta desde el directorio del proyecto (que contiene el archivo *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="39760-149">In the samples below, the `dotnet publish` command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="39760-150">Si no se encuentra en la carpeta del proyecto, puede pasar de forma explícita la ruta de acceso del archivo del proyecto.</span><span class="sxs-lookup"><span data-stu-id="39760-150">If you're not in the project folder, you can explicitly pass in the project file path.</span></span> <span data-ttu-id="39760-151">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="39760-151">For example:</span></span>

```console
dotnet publish  c:/webs/web1
```

<span data-ttu-id="39760-152">Ejecute los comandos siguientes para crear y publicar una aplicación web:</span><span class="sxs-lookup"><span data-stu-id="39760-152">Run the following commands to create and publish a web app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="39760-153">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="39760-153">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="39760-154">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="39760-154">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

--------------

<span data-ttu-id="39760-155">El comando `dotnet publish` genera un resultado similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="39760-155">The `dotnet publish` produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="39760-156">La carpeta de publicación predeterminada es `bin\$(Configuration)\netcoreapp<version>\publish`,</span><span class="sxs-lookup"><span data-stu-id="39760-156">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="39760-157">mientras que la de `$(Configuration)` es Debug.</span><span class="sxs-lookup"><span data-stu-id="39760-157">The default for `$(Configuration)` is Debug.</span></span> <span data-ttu-id="39760-158">En el ejemplo anterior, el `<TargetFramework>` es `netcoreapp2.0`.</span><span class="sxs-lookup"><span data-stu-id="39760-158">In the sample above, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="39760-159">`dotnet publish -h` muestra información de ayuda de la publicación.</span><span class="sxs-lookup"><span data-stu-id="39760-159">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="39760-160">El siguiente comando especifica una compilación `Release` y el directorio de publicación:</span><span class="sxs-lookup"><span data-stu-id="39760-160">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:/MyWebs/test
```

<span data-ttu-id="39760-161">El comando `dotnet publish` llama a `MSBuild`, que invoca el destino `Publish`.</span><span class="sxs-lookup"><span data-stu-id="39760-161">The `dotnet publish` command calls `MSBuild` which invokes the `Publish` target.</span></span> <span data-ttu-id="39760-162">Todos los parámetros pasados a `dotnet publish` se pasan a `MSBuild`.</span><span class="sxs-lookup"><span data-stu-id="39760-162">Any parameters passed to `dotnet publish` are passed to `MSBuild`.</span></span> <span data-ttu-id="39760-163">El parámetro `-c` se asigna a la propiedad de MSBuild `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="39760-163">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="39760-164">El parámetro `-o` se asigna a `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="39760-164">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="39760-165">Puede pasar propiedades de MSBuild mediante cualquiera de los siguientes formatos:</span><span class="sxs-lookup"><span data-stu-id="39760-165">You can pass MSBuild properties using either of the following formats:</span></span>

- ` p:<NAME>=<VALUE>`
- `/p:<NAME>=<VALUE>`

<span data-ttu-id="39760-166">El comando siguiente publica una compilación `Release` en un recurso compartido de red:</span><span class="sxs-lookup"><span data-stu-id="39760-166">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="39760-167">El recurso compartido de red se especifica con barras diagonales (*//r8/*) y funciona en todas las plataformas compatibles de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="39760-167">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="39760-168">Confirme que la aplicación publicada para la implementación no se está ejecutando.</span><span class="sxs-lookup"><span data-stu-id="39760-168">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="39760-169">Los archivos de la carpeta *publish* quedan bloqueados mientras se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="39760-169">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="39760-170">No se puede llevar a cabo la implementación porque no se pueden copiar los archivos bloqueados.</span><span class="sxs-lookup"><span data-stu-id="39760-170">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="39760-171">Publicar los perfiles</span><span class="sxs-lookup"><span data-stu-id="39760-171">Publish profiles</span></span>

<span data-ttu-id="39760-172">En esta sección se usa Visual Studio 2017 y versiones posteriores para crear perfiles de publicación.</span><span class="sxs-lookup"><span data-stu-id="39760-172">This section uses Visual Studio 2017 and higher to create publishing profiles.</span></span> <span data-ttu-id="39760-173">Una vez creados, puede publicarlos en Visual Studio o en la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="39760-173">Once created, you can publish from Visual Studio or the command line.</span></span>

<span data-ttu-id="39760-174">Los perfiles de publicación pueden simplificar el proceso de publicación.</span><span class="sxs-lookup"><span data-stu-id="39760-174">Publish profiles can simplify the publishing process.</span></span> <span data-ttu-id="39760-175">Puede tener varios perfiles de publicación.</span><span class="sxs-lookup"><span data-stu-id="39760-175">You can have multiple publish profiles.</span></span> <span data-ttu-id="39760-176">Para crear un perfil de publicación en Visual Studio, haga clic con el botón derecho en el proyecto en el Explorador de soluciones y seleccione **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="39760-176">To create a publish profile in Visual Studio, right click on the project in Solution Explore and select **Publish**.</span></span> <span data-ttu-id="39760-177">Como alternativa, puede seleccionar **Publicar \<Nombre del proyecto >** en el menú Compilar.</span><span class="sxs-lookup"><span data-stu-id="39760-177">Alternatively, you can select **Publish     \<project name>** from the build menu.</span></span> <span data-ttu-id="39760-178">Aparecerá la pestaña **Publicar** de la página de las capacidades de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="39760-178">The **Publish** tab of the application capacities page is displayed.</span></span> <span data-ttu-id="39760-179">Si el proyecto no contiene ningún perfil de publicación, se mostrará la siguiente página:</span><span class="sxs-lookup"><span data-stu-id="39760-179">If the project doesn't contain a publish profile, the following page is displayed:</span></span>

![Pestaña **Publicar** de la página de las capacidades de la aplicación, donde se muestran las opciones Azure, IIS, FTP y Carpeta, con la opción Azure seleccionada.](web-publishing-vs/_static/az.png)

<span data-ttu-id="39760-182">Al seleccionar **Carpeta**, el botón **Publicar** crea un perfil de publicación para la carpeta y lo publica.</span><span class="sxs-lookup"><span data-stu-id="39760-182">When **Folder** is selected, the **Publish** button creates a folder publish profile and publishes.</span></span>

![Pestaña **Publicar** de la página de las capacidades de la aplicación, donde se muestran las opciones Azure, IIS, FTP y Carpeta](web-publishing-vs/_static/pub1.png)

<span data-ttu-id="39760-184">Una vez creado el perfil de publicación, la pestaña **Publicar** cambia y puede seleccionar **Crear nuevo perfil** para crear un perfil.</span><span class="sxs-lookup"><span data-stu-id="39760-184">Once a publish profile is created, the **Publish** tab changes, and you select **Create new profile** to create a new profile.</span></span>

![Pestaña **Publicar** de la página de las capacidades de la aplicación, donde se muestra FolderProfile (creado en el último paso) y el vínculo Crear nuevo perfil](web-publishing-vs/_static/create_new.png)

<span data-ttu-id="39760-186">El Asistente para publicación admite los siguientes destinos de publicación:</span><span class="sxs-lookup"><span data-stu-id="39760-186">The Publish wizard supports the following publish targets:</span></span>

- <span data-ttu-id="39760-187">Microsoft Azure App Service</span><span class="sxs-lookup"><span data-stu-id="39760-187">Microsoft Azure App Service</span></span>
- <span data-ttu-id="39760-188">IIS, FTP, etc. (para cualquier servidor web)</span><span class="sxs-lookup"><span data-stu-id="39760-188">IIS, FTP, etc (for any web server)</span></span>
- <span data-ttu-id="39760-189">Carpeta</span><span class="sxs-lookup"><span data-stu-id="39760-189">Folder</span></span>
- <span data-ttu-id="39760-190">Perfil de importación (le permite importar un perfil).</span><span class="sxs-lookup"><span data-stu-id="39760-190">Import profile (allows you to import a profile).</span></span>
- <span data-ttu-id="39760-191">Microsoft Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="39760-191">Microsoft Azure Virtual Machines</span></span>

<span data-ttu-id="39760-192">Vea [¿Qué opciones de publicación son las adecuadas para mí?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) para más información.</span><span class="sxs-lookup"><span data-stu-id="39760-192">See [What publishing options are right for me?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) for more information.</span></span>

<span data-ttu-id="39760-193">Al crear un perfil de publicación con Visual Studio, se crea un archivo de MSBuild *Properties/PublishProfiles/\<nombre_publicación>.pubxml*.</span><span class="sxs-lookup"><span data-stu-id="39760-193">When you create a publish profile with Visual Studio, a *Properties/PublishProfiles/\<publish name>.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="39760-194">Este archivo *.pubxml* es un archivo de MSBuild que contiene opciones de configuración de publicación.</span><span class="sxs-lookup"><span data-stu-id="39760-194">This *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="39760-195">Puede modificar este archivo para personalizar la compilación y el proceso de publicación.</span><span class="sxs-lookup"><span data-stu-id="39760-195">You can change this file to customize the build and publish process.</span></span> <span data-ttu-id="39760-196">Este archivo se lee durante el proceso de publicación.</span><span class="sxs-lookup"><span data-stu-id="39760-196">This file is read by the publishing process.</span></span> <span data-ttu-id="39760-197">`<LastUsedBuildConfiguration>` es especial porque es una propiedad global y no debe estar en ningún archivo importado en la compilación.</span><span class="sxs-lookup"><span data-stu-id="39760-197">`<LastUsedBuildConfiguration>` is special because it’s a global property and shouldn’t be in any file that’s imported in the build.</span></span> <span data-ttu-id="39760-198">Vea [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) (MSBuild: Cómo establecer la propiedad de configuración) para más información.</span><span class="sxs-lookup"><span data-stu-id="39760-198">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more info.</span></span>
<span data-ttu-id="39760-199">El archivo *.pubxml* no debe estar protegido bajo control de código fuente porque depende del archivo *.user*.</span><span class="sxs-lookup"><span data-stu-id="39760-199">The *.pubxml* file should not be checked into source control because it depends on the *.user* file.</span></span> <span data-ttu-id="39760-200">El archivo *.user* nunca debe estar protegido bajo control de código fuente porque puede contener información confidencial y solo es válido para un usuario y un equipo.</span><span class="sxs-lookup"><span data-stu-id="39760-200">The *.user* file should never be checked into source control because it can contain sensitive information and it's only valid for one user and machine.</span></span>

<span data-ttu-id="39760-201">La información confidencial (como la contraseña de publicación) se cifra a nivel de usuario/equipo y se almacenan en el archivo *Properties/PublishProfiles/\<nombre_publicación>.pubxml.user*.</span><span class="sxs-lookup"><span data-stu-id="39760-201">Sensitive information (like the publish password) is encrypted on a per user/machine level and stored in the *Properties/PublishProfiles/\<publish name>.pubxml.user* file.</span></span> <span data-ttu-id="39760-202">Dado que este archivo puede contener información confidencial, **no** debe estar protegido bajo control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="39760-202">Because this file can contain sensitive information, it should **not** be checked into source control.</span></span>

<span data-ttu-id="39760-203">Para obtener información general sobre cómo publicar una aplicación web en ASP.NET Core, vea [Publishing and Deployment](index.md) (Publicación e implementación),</span><span class="sxs-lookup"><span data-stu-id="39760-203">For an overview of how to publish a web app on ASP.NET Core see [Publishing and Deployment](index.md).</span></span> <span data-ttu-id="39760-204">[Publishing and Deployment](index.md) (Publicación e implementación) que es un proyecto de código abierto que encontrará en https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="39760-204">[Publishing and Deployment](index.md) is an open source project at https://github.com/aspnet/websdk.</span></span>

 <span data-ttu-id="39760-205">`dotnet publish` puede usar perfiles de publicación de carpeta, Msdeploy y [KUDU](https://github.com/projectkudu/kudu/wiki):</span><span class="sxs-lookup"><span data-stu-id="39760-205">`dotnet publish` can use folder, Msdeploy, and [KUDU](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>
 
<span data-ttu-id="39760-206">Carpeta (funciona entre plataformas) `dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span><span class="sxs-lookup"><span data-stu-id="39760-206">Folder (works cross-platform) `dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span></span>

<span data-ttu-id="39760-207">Msdeploy (actualmente solo funciona en Windows, ya que msdeploy no es multiplataforma): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span><span class="sxs-lookup"><span data-stu-id="39760-207">Msdeploy (currently this only works in windows since msdeploy is not cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span></span>

<span data-ttu-id="39760-208">Paquete Msdeploy (actualmente solo funciona en Windows, ya que msdeploy no es multiplataforma): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span><span class="sxs-lookup"><span data-stu-id="39760-208">Msdeploy package(currently this only works in windows since msdeploy is not cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span></span>

<span data-ttu-id="39760-209">En los ejemplos anteriores, **no** pase `deployonbuild` a `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="39760-209">In the preceeding samples, **don’t** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="39760-210">Para obtener más información, vea [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="39760-210">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish)</span></span>

<span data-ttu-id="39760-211">`dotnet publish` admite las API de KUDU para publicar en Azure desde cualquier plataforma.</span><span class="sxs-lookup"><span data-stu-id="39760-211">`dotnet publish` supports KUDU apis to publish to Azure from any platform.</span></span> <span data-ttu-id="39760-212">La publicación de Visual Studio no admite las API de KUDU, pero es compatible con websdk para la publicación multiplataforma en Azure.</span><span class="sxs-lookup"><span data-stu-id="39760-212">Visual Studio publish does support the KUDU APIs but it is supported by websdk for cross plat publish to Azure.</span></span>

<span data-ttu-id="39760-213">Agregue un perfil de publicación a la carpeta *Properties/PublishProfiles* con el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="39760-213">Add a publish profile to *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="39760-214">La ejecución del comando siguiente comprimirá el contenido de la publicación y lo publicará en Azure mediante las API de KUDU.</span><span class="sxs-lookup"><span data-stu-id="39760-214">Running the following command will zip up the publish contents and publish it to Azure using the KUDU APIs.</span></span>

`dotnet publish /p:PublishProfile=Azure /p:Configuration=Release`

<span data-ttu-id="39760-215">Establezca las siguientes propiedades de MSBuild cuando use un perfil de publicación:</span><span class="sxs-lookup"><span data-stu-id="39760-215">Set the following MSBuild properties when using a publish profile:</span></span>

- `DeployOnBuild=true`
- `PublishProfile=<Publish profile name>`

<span data-ttu-id="39760-216">Por ejemplo, al efectuar una publicación con un perfil denominado *FolderProfile*, puede ejecutar cualquiera de los comandos siguientes.</span><span class="sxs-lookup"><span data-stu-id="39760-216">For example, when publishing with a profile named *FolderProfile* you can execute either of the commands below.</span></span>

- `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
- `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="39760-217">Al invocar a `dotnet build`, llamará a `msbuild` para ejecutar la compilación y publicar el proceso.</span><span class="sxs-lookup"><span data-stu-id="39760-217">When you invoke `dotnet build` it will call `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="39760-218">Llamar a `dotnet build` o a `msbuild` es prácticamente lo mismo que pasar un perfil de la carpeta.</span><span class="sxs-lookup"><span data-stu-id="39760-218">Calling `dotnet build` or `msbuild` is essentially equivalent when you pass in a folder profile.</span></span> <span data-ttu-id="39760-219">Al llamar a MSBuild directamente en Windows, obtiene la versión .NET Framework de MSBuild.</span><span class="sxs-lookup"><span data-stu-id="39760-219">When calling MSBuild directly on Windows you get the .NET Framework version of MSBuild.</span></span>  <span data-ttu-id="39760-220">Actualmente, MSDeploy está limitado a los equipos con Windows para efectuar publicaciones.</span><span class="sxs-lookup"><span data-stu-id="39760-220">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="39760-221">Al llamar a `dotnet build` en un perfil que no es de carpeta se invoca a MSBuild, que usa MSDeploy en los perfiles que no son de carpeta.</span><span class="sxs-lookup"><span data-stu-id="39760-221">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="39760-222">Al llamar a `dotnet build` en un perfil que no es de carpeta, se invoca a MSBuild (mediante MSDeploy), lo cual genera un error (incluso si se ejecuta en una plataforma de Windows).</span><span class="sxs-lookup"><span data-stu-id="39760-222">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="39760-223">Para publicar con un perfil que no es de carpeta, llame directamente a MSBuild.</span><span class="sxs-lookup"><span data-stu-id="39760-223">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="39760-224">El siguiente perfil de publicación de carpeta se creó con Visual Studio y se publica en un recurso compartido de red:</span><span class="sxs-lookup"><span data-stu-id="39760-224">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

[!code-xml[Main](web-publishing-vs/sample/FolderProfile.pubxml?highlight=5,9,11,18)]

<span data-ttu-id="39760-225">Tenga en cuenta que `<LastUsedBuildConfiguration>` está establecido en `Release`.</span><span class="sxs-lookup"><span data-stu-id="39760-225">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span>  <span data-ttu-id="39760-226">Al efectuar una publicación en Visual Studio, el valor de la propiedad de configuración `<LastUsedBuildConfiguration>` se establece con el valor cuando se inicia el proceso de publicación.</span><span class="sxs-lookup"><span data-stu-id="39760-226">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="39760-227">La propiedad de configuración `<LastUsedBuildConfiguration>` es especial y no se debe reemplazar en un archivo importado de MSBuild.</span><span class="sxs-lookup"><span data-stu-id="39760-227">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn’t be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="39760-228">Puede reemplazar esta propiedad desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="39760-228">You can override this property from the command line.</span></span> <span data-ttu-id="39760-229">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="39760-229">For example:</span></span>

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="39760-230">Con MSBuild:</span><span class="sxs-lookup"><span data-stu-id="39760-230">Using MSBuild:</span></span>

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="39760-231">Publicar en un punto de conexión de MSDeploy desde la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="39760-231">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="39760-232">Como se ha mencionado anteriormente, puede efectuar una publicación mediante `dotnet publish` o con el comando `msbuild`.</span><span class="sxs-lookup"><span data-stu-id="39760-232">As previously mentioned, you can publish using `dotnet publish` or the `msbuild` command.</span></span> <span data-ttu-id="39760-233">`dotnet publish` se ejecuta en el contexto de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="39760-233">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="39760-234">`msbuild` requiere .NET Framework; por lo tanto, está limitado a los entornos de Windows.</span><span class="sxs-lookup"><span data-stu-id="39760-234">`msbuild` requires .NET framework, and is therefore limited to Windows environments.</span></span>

<span data-ttu-id="39760-235">La forma más fácil de publicar con MSDeploy consiste en crear primero un perfil de publicación en Visual Studio 2017 y, luego, usar el perfil en la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="39760-235">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="39760-236">En el ejemplo siguiente se ha creado una aplicación web de ASP.NET Core (con `dotnet new mvc`) y se ha agregado un perfil de publicación de Azure con Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="39760-236">In the following sample, an ASP.NET Core web app is created ( using `dotnet new mvc`) and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="39760-237">Debe ejecutar `msbuild` desde un **Símbolo del sistema para desarrolladores de VS 2017**.</span><span class="sxs-lookup"><span data-stu-id="39760-237">You run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="39760-238">El Símbolo del sistema para desarrolladores tendrá el archivo *msbuild.exe* correcto en su ruta de acceso y establecerá algunas variables de MSBuild.</span><span class="sxs-lookup"><span data-stu-id="39760-238">The Developer Command Prompt will have the correct *msbuild.exe* in its path and set some MSBuild variables.</span></span>

<span data-ttu-id="39760-239">MSBuild usa la sintaxis siguiente:</span><span class="sxs-lookup"><span data-stu-id="39760-239">MSBuild uses the following syntax:</span></span>

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile>  /p:Username=<USERNAME> /p:Password=<PASSWORD>`

<span data-ttu-id="39760-240">Puede obtener la `Password` del archivo *\<nombre_publicación>.PublishSettings*.</span><span class="sxs-lookup"><span data-stu-id="39760-240">You can get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="39760-241">Puede descargar el archivo *.PublishSettings* en:</span><span class="sxs-lookup"><span data-stu-id="39760-241">You can download the *.PublishSettings* file from:</span></span>

- <span data-ttu-id="39760-242">Explorador de soluciones: Haga clic con el botón derecho en la aplicación web y seleccione **Descargar perfil de publicación**.</span><span class="sxs-lookup"><span data-stu-id="39760-242">Solution Explorer: Right click on the Web App and select **Download Publish Profile**.</span></span>
- <span data-ttu-id="39760-243">Portal de administración de Azure: Seleccione **Obtener perfil de publicación** en la hoja Aplicación web.</span><span class="sxs-lookup"><span data-stu-id="39760-243">The Azure Management Portal: Select **Get publish profile** from the  Web App blade.</span></span>

<span data-ttu-id="39760-244">`Username` está en el perfil de publicación.</span><span class="sxs-lookup"><span data-stu-id="39760-244">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="39760-245">En el siguiente ejemplo se usa el perfil de publicación "Web11112 - Web Deploy":</span><span class="sxs-lookup"><span data-stu-id="39760-245">The following sample uses the "Web11112 - Web Deploy" publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a><span data-ttu-id="39760-246">Excluir archivos</span><span class="sxs-lookup"><span data-stu-id="39760-246">Excluding files</span></span>

<span data-ttu-id="39760-247">Al publicar aplicaciones web de ASP.NET Core, se incluyen los artefactos de compilación y el contenido de la carpeta *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="39760-247">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="39760-248">`msbuild` admite los [patrones globales](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="39760-248">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="39760-249">Por ejemplo, el siguiente marcado del elemento `<Content>` excluirá todo los archivos de texto (*.txt*) de la carpeta *wwwroot/content* y de todas sus subcarpetas.</span><span class="sxs-lookup"><span data-stu-id="39760-249">For example, the following `<Content>` element markup will exclude all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="39760-250">El marcado anterior se puede agregar a un perfil de publicación o al archivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="39760-250">The markup above can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="39760-251">Si se agrega al archivo *.csproj*, la regla se agrega a todos los perfiles de publicación del proyecto.</span><span class="sxs-lookup"><span data-stu-id="39760-251">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="39760-252">El siguiente marcado del elemento `<MsDeploySkipRules>` excluye todos los archivos de la carpeta *wwwroot/content*:</span><span class="sxs-lookup"><span data-stu-id="39760-252">The following `<MsDeploySkipRules>` element markup exludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="39760-253">`<MsDeploySkipRules>` no eliminará del sitio de implementación los destinos *skip*.</span><span class="sxs-lookup"><span data-stu-id="39760-253">`<MsDeploySkipRules>`  will not delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="39760-254">Los archivos y carpetas destinados a `<Content>` se eliminarán del sitio de implementación.</span><span class="sxs-lookup"><span data-stu-id="39760-254">`<Content>` targeted files and folders will be deleted from the deployment site.</span></span> <span data-ttu-id="39760-255">Por ejemplo, imagínese que ha implementado una aplicación web con los siguientes archivos:</span><span class="sxs-lookup"><span data-stu-id="39760-255">For example, suppose you had deployed a web app with the following files:</span></span>

- <span data-ttu-id="39760-256">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="39760-256">*Views/Home/About1.cshtml*</span></span>
- <span data-ttu-id="39760-257">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="39760-257">*Views/Home/About2.cshtml*</span></span>
- <span data-ttu-id="39760-258">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="39760-258">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="39760-259">Si hubiera agregado el siguiente marcado `<MsDeploySkipRules>`, no se habrían eliminado los archivos del sitio de implementación.</span><span class="sxs-lookup"><span data-stu-id="39760-259">If you added the following `<MsDeploySkipRules>` markup, those files would not be deleted on the deployment site.</span></span>

``` xml
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

<span data-ttu-id="39760-260">El marcado `<MsDeploySkipRules>` anterior impide que se implementen los archivos *omitidos*, pero no eliminará los archivos una vez que se hayan implementado.</span><span class="sxs-lookup"><span data-stu-id="39760-260">The `<MsDeploySkipRules>` markup shown above prevents the *skipped* files from being depoyed, but will not delete those files once they are deployed.</span></span>

<span data-ttu-id="39760-261">El siguiente marcado `<Content>` eliminaría del sitio de implementación los archivos objetivo:</span><span class="sxs-lookup"><span data-stu-id="39760-261">The following `<Content>` markup would delete the targeted files at the deployment site:</span></span>

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="39760-262">El uso de la implementación de la línea de comandos con el marcado `<Content>` anterior habría generado un resultado similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="39760-262">Using command line deployment with the `<Content>` markup above would result in output similar to the following:</span></span>

``` console
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

## <a name="including-files"></a><span data-ttu-id="39760-263">Incluir archivos</span><span class="sxs-lookup"><span data-stu-id="39760-263">Including files</span></span>

<span data-ttu-id="39760-264">Se puede usar el siguiente marcado para incluir una carpeta *images* fuera del directorio del proyecto en la carpeta *wwwroot/images* del sitio de publicación.</span><span class="sxs-lookup"><span data-stu-id="39760-264">The following markup can be used to include an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site.</span></span>

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="39760-265">El marcado se puede agregar al archivo *.csproj* o al perfil de publicación.</span><span class="sxs-lookup"><span data-stu-id="39760-265">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="39760-266">Si se agrega al archivo *.csproj*, se incluirá en todos los perfiles de publicación del proyecto.</span><span class="sxs-lookup"><span data-stu-id="39760-266">If it's added to the *.csproj* file, it will be included in each publish profile in the project.</span></span>

<span data-ttu-id="39760-267">En el siguiente marcado resaltado se muestra cómo:</span><span class="sxs-lookup"><span data-stu-id="39760-267">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="39760-268">Copiar un archivo de fuera del proyecto a la carpeta *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="39760-268">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="39760-269">Excluir la carpeta *wwwroot\Content*.</span><span class="sxs-lookup"><span data-stu-id="39760-269">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="39760-270">Excluir *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="39760-270">Exclude *Views\Home\About2.cshtml*.</span></span>

[!code-xml[Main](web-publishing-vs/sample/FolderProfile2.pubxml?highlight=21-29)]

<span data-ttu-id="39760-271">Vea [WebSDK Readme](https://github.com/aspnet/websdk) (Archivo Léame de WebSDK) para ver más ejemplos de implementación.</span><span class="sxs-lookup"><span data-stu-id="39760-271">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

### <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="39760-272">Ejecutar un destino antes o después de la publicación</span><span class="sxs-lookup"><span data-stu-id="39760-272">Run a target before or after publishing</span></span>

<span data-ttu-id="39760-273">Los destinos `BeforePublish` y `AfterPublish` integrados se pueden usar para ejecutar un destino antes o después del destino de publicación.</span><span class="sxs-lookup"><span data-stu-id="39760-273">The builtin `BeforePublish` and `AfterPublish` targets can be used to execute a target before or after the publish target.</span></span> <span data-ttu-id="39760-274">Se puede agregar el siguiente marcado al perfil de publicación para registrar mensajes en la salida de la consola antes y después de la publicación:</span><span class="sxs-lookup"><span data-stu-id="39760-274">The following markup can be added to the publish profile to log messages to the console output before and after publishing:</span></span>

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a><span data-ttu-id="39760-275">Servicio Kudu</span><span class="sxs-lookup"><span data-stu-id="39760-275">The Kudu service</span></span>

<span data-ttu-id="39760-276">Para ver los archivos en la aplicación web de Azure, use el [servicio Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="39760-276">To view the files on your Azure Web App, use the [kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="39760-277">Anexe el token `scm` al nombre o a la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="39760-277">Append the `scm` token to the name or your Web App.</span></span> <span data-ttu-id="39760-278">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="39760-278">For example:</span></span>

| <span data-ttu-id="39760-279">Dirección URL</span><span class="sxs-lookup"><span data-stu-id="39760-279">URL</span></span>               | <span data-ttu-id="39760-280">Resultado</span><span class="sxs-lookup"><span data-stu-id="39760-280">Result</span></span>|
| ----------------- | ------------ |
| `http://mysite.azurewebsites.net/` | <span data-ttu-id="39760-281">Aplicación web</span><span class="sxs-lookup"><span data-stu-id="39760-281">Web App</span></span> |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="39760-282">Servicio Kudu</span><span class="sxs-lookup"><span data-stu-id="39760-282">Kudu sevice</span></span> |

<span data-ttu-id="39760-283">Seleccione el elemento de menú [Consola de depuración](https://github.com/projectkudu/kudu/wiki/Kudu-console) para ver/editar/eliminar/agregar archivos.</span><span class="sxs-lookup"><span data-stu-id="39760-283">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view/edit/delete/add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="39760-284">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="39760-284">Additional resources</span></span>

- <span data-ttu-id="39760-285">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (msdeploy) simplifica la implementación de aplicaciones web y de sitios web en los servidores de IIS.</span><span class="sxs-lookup"><span data-stu-id="39760-285">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy)  (msdeploy) simplifies deployment of Web applications and Web sites to IIS servers.</span></span>

- <span data-ttu-id="39760-286">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): problemas de archivos y solicitud de características para la implementación.</span><span class="sxs-lookup"><span data-stu-id="39760-286">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
