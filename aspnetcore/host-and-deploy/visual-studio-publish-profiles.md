---
title: "Perfiles para la implementación de aplicaciones de ASP.NET Core de publicación de Visual Studio"
author: rick-anderson
description: "Descubra cómo crear perfiles para las aplicaciones de ASP.NET Core de publicación en Visual Studio."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 09/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 1f403447c39db4ebfe3dafda591602f0dc9db8c3
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="b7269-103">Perfiles para la implementación de aplicaciones de ASP.NET Core de publicación de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b7269-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="b7269-104">Por [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b7269-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b7269-105">Este artículo se centra en el uso de Visual Studio 2017 para crear perfiles de publicación.</span><span class="sxs-lookup"><span data-stu-id="b7269-105">This article focuses on using Visual Studio 2017 to create publish profiles.</span></span> <span data-ttu-id="b7269-106">Los perfiles de publicación creados con Visual Studio se pueden ejecutar en MSBuild y en Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="b7269-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="b7269-107">El artículo proporciona detalles sobre el proceso de publicación.</span><span class="sxs-lookup"><span data-stu-id="b7269-107">The article provides details of the publishing process.</span></span> <span data-ttu-id="b7269-108">Vea [Publicar una aplicación web de ASP.NET Core en Azure App Service con Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) para obtener instrucciones sobre la publicación en Azure.</span><span class="sxs-lookup"><span data-stu-id="b7269-108">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="b7269-109">El siguiente archivo *.csproj* se creó con el comando `dotnet new mvc`:</span><span class="sxs-lookup"><span data-stu-id="b7269-109">The following *.csproj* file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b7269-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b7269-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b7269-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b7269-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="b7269-112">El atributo `Sdk` del elemento `<Project>` (en la primera línea) del marcado anterior hace lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="b7269-112">The `Sdk` attribute in the `<Project>` element (in the first line) of the markup above does the following:</span></span>

* <span data-ttu-id="b7269-113">Importa el archivo de propiedades de *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* al principio.</span><span class="sxs-lookup"><span data-stu-id="b7269-113">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="b7269-114">Importa el archivo targets de *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* al final.</span><span class="sxs-lookup"><span data-stu-id="b7269-114">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="b7269-115">La ubicación predeterminada de `MSBuildSDKsPath` (con Visual Studio 2017 Enterprise) es la carpeta *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.</span><span class="sxs-lookup"><span data-stu-id="b7269-115">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="b7269-116">`Microsoft.NET.Sdk.Web` depende de:</span><span class="sxs-lookup"><span data-stu-id="b7269-116">`Microsoft.NET.Sdk.Web` depends on:</span></span>

* <span data-ttu-id="b7269-117">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="b7269-117">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="b7269-118">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="b7269-118">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="b7269-119">Lo que hace que las propiedades y destinos que se importarán los siguientes:</span><span class="sxs-lookup"><span data-stu-id="b7269-119">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="b7269-120">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="b7269-120">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="b7269-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="b7269-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span></span>
* <span data-ttu-id="b7269-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="b7269-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="b7269-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="b7269-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span></span>

<span data-ttu-id="b7269-124">Publicar la importación de destinos el derecho de conjunto de destinos en función del método de publicación que se utiliza.</span><span class="sxs-lookup"><span data-stu-id="b7269-124">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="b7269-125">Cuando Visual Studio o MSBuild carga un proyecto, se llevan a cabo las siguientes acciones de alto nivel:</span><span class="sxs-lookup"><span data-stu-id="b7269-125">When MSBuild or Visual Studio loads a project, the following high level actions are performed:</span></span>

* <span data-ttu-id="b7269-126">Compilación del proyecto</span><span class="sxs-lookup"><span data-stu-id="b7269-126">Build project</span></span>
* <span data-ttu-id="b7269-127">Cálculo de los archivos para la publicación</span><span class="sxs-lookup"><span data-stu-id="b7269-127">Compute files to publish</span></span>
* <span data-ttu-id="b7269-128">Publicación de los archivos en el destino</span><span class="sxs-lookup"><span data-stu-id="b7269-128">Publish files to destination</span></span>

### <a name="compute-project-items"></a><span data-ttu-id="b7269-129">Cálculo de los elementos del proyecto</span><span class="sxs-lookup"><span data-stu-id="b7269-129">Compute project items</span></span>

<span data-ttu-id="b7269-130">Cuando se carga el proyecto, se calculan los elementos del proyecto (archivos).</span><span class="sxs-lookup"><span data-stu-id="b7269-130">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="b7269-131">El atributo `item type` determina cómo se procesa el archivo.</span><span class="sxs-lookup"><span data-stu-id="b7269-131">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="b7269-132">De forma predeterminada, los archivos *.cs* se incluyen en la lista de elementos `Compile`.</span><span class="sxs-lookup"><span data-stu-id="b7269-132">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="b7269-133">Después se compilan los archivos de la lista de elementos `Compile`.</span><span class="sxs-lookup"><span data-stu-id="b7269-133">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="b7269-134">El `Content` lista de elementos contiene archivos que se publican además de los resultados de compilación.</span><span class="sxs-lookup"><span data-stu-id="b7269-134">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="b7269-135">De forma predeterminada, los archivos que coincidan con el patrón `wwwroot/**` se incluyen en el `Content` elemento.</span><span class="sxs-lookup"><span data-stu-id="b7269-135">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="b7269-136">[wwwroot /\* \* es un patrón de uso de comodines en](https://gruntjs.com/configuring-tasks#globbing-patterns) que especifica todos los archivos de la *wwwroot* carpeta **y** las subcarpetas.</span><span class="sxs-lookup"><span data-stu-id="b7269-136">[wwwroot/\*\* is a globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) that specifies all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="b7269-137">Para agregar explícitamente un archivo a la lista de publicación, agregue el archivo directamente en el archivo *.csproj*, como se muestra en [Incluir archivos](#including-files).</span><span class="sxs-lookup"><span data-stu-id="b7269-137">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Including Files](#including-files).</span></span>

<span data-ttu-id="b7269-138">Al seleccionar la **publicar** botón en Visual Studio o al publicar desde la línea de comandos:</span><span class="sxs-lookup"><span data-stu-id="b7269-138">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="b7269-139">Se calculan los elementos/propiedades (los archivos que se deben compilar).</span><span class="sxs-lookup"><span data-stu-id="b7269-139">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="b7269-140">Solo para Visual Studio: se restauran los paquetes NuGet</span><span class="sxs-lookup"><span data-stu-id="b7269-140">Visual Studio only: NuGet packages are restored.</span></span> <span data-ttu-id="b7269-141">(la restauración debe ser explícita por parte del usuario en la CLI).</span><span class="sxs-lookup"><span data-stu-id="b7269-141">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="b7269-142">Se compila el proyecto.</span><span class="sxs-lookup"><span data-stu-id="b7269-142">The project builds.</span></span>
* <span data-ttu-id="b7269-143">Se calculan los elementos de publicación (los archivos que se deben publicar).</span><span class="sxs-lookup"><span data-stu-id="b7269-143">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="b7269-144">Se publica el proyecto</span><span class="sxs-lookup"><span data-stu-id="b7269-144">The project is published.</span></span> <span data-ttu-id="b7269-145">(los archivos calculados se copian en el destino de publicación).</span><span class="sxs-lookup"><span data-stu-id="b7269-145">(The computed files are copied to the publish destination.)</span></span>

<span data-ttu-id="b7269-146">Cuando hace referencia a un proyecto de ASP.NET Core `Microsoft.NET.Sdk.Web` en el archivo de proyecto, un *app_offline.htm* archivo se coloca en la raíz del directorio de aplicación web.</span><span class="sxs-lookup"><span data-stu-id="b7269-146">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="b7269-147">Cuando el archivo está presente, el módulo de ASP.NET Core cierra correctamente la aplicación y proporciona el archivo *app_offline.htm* durante la implementación.</span><span class="sxs-lookup"><span data-stu-id="b7269-147">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="b7269-148">Para más información, vea [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#appofflinehtm) (Referencia de configuración del módulo de ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="b7269-148">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="b7269-149">Publicación de línea de comandos básica</span><span class="sxs-lookup"><span data-stu-id="b7269-149">Basic command-line publishing</span></span>

<span data-ttu-id="b7269-150">Publicación de línea de comandos funciona en todas las plataformas de .NET Core compatibles y no requiere Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b7269-150">Command-line publishing works on all .NET Core supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="b7269-151">En los ejemplos siguientes, el comando `dotnet publish` se ejecuta desde el directorio del proyecto (que contiene el archivo *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="b7269-151">In the samples below, the `dotnet publish` command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="b7269-152">Si no es así en la carpeta del proyecto, pasar de manera explícita en la ruta de acceso del archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="b7269-152">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="b7269-153">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="b7269-153">For example:</span></span>

```console
dotnet publish c:/webs/web1
```

<span data-ttu-id="b7269-154">Ejecute los comandos siguientes para crear y publicar una aplicación web:</span><span class="sxs-lookup"><span data-stu-id="b7269-154">Run the following commands to create and publish a web app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b7269-155">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b7269-155">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b7269-156">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b7269-156">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

<span data-ttu-id="b7269-157">El comando `dotnet publish` genera un resultado similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="b7269-157">The `dotnet publish` produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="b7269-158">La carpeta de publicación predeterminada es `bin\$(Configuration)\netcoreapp<version>\publish`,</span><span class="sxs-lookup"><span data-stu-id="b7269-158">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="b7269-159">mientras que la de `$(Configuration)` es Debug.</span><span class="sxs-lookup"><span data-stu-id="b7269-159">The default for `$(Configuration)` is Debug.</span></span> <span data-ttu-id="b7269-160">En el ejemplo anterior, el `<TargetFramework>` es `netcoreapp2.0`.</span><span class="sxs-lookup"><span data-stu-id="b7269-160">In the sample above, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="b7269-161">`dotnet publish -h` muestra información de ayuda de la publicación.</span><span class="sxs-lookup"><span data-stu-id="b7269-161">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="b7269-162">El siguiente comando especifica una compilación `Release` y el directorio de publicación:</span><span class="sxs-lookup"><span data-stu-id="b7269-162">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:/MyWebs/test
```

<span data-ttu-id="b7269-163">El `dotnet publish` comando llame a MSBuild que se invoca el `Publish` destino.</span><span class="sxs-lookup"><span data-stu-id="b7269-163">The `dotnet publish` command calls MSBuild which invokes the `Publish` target.</span></span> <span data-ttu-id="b7269-164">Los parámetros pasados a `dotnet publish` se pasan a MSBuild.</span><span class="sxs-lookup"><span data-stu-id="b7269-164">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="b7269-165">El parámetro `-c` se asigna a la propiedad de MSBuild `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="b7269-165">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="b7269-166">El parámetro `-o` se asigna a `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="b7269-166">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="b7269-167">Propiedades de MSBuild se pueden pasar mediante cualquiera de los siguientes formatos:</span><span class="sxs-lookup"><span data-stu-id="b7269-167">MSBuild properties can be passed using either of the following formats:</span></span>

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="b7269-168">El comando siguiente publica una compilación `Release` en un recurso compartido de red:</span><span class="sxs-lookup"><span data-stu-id="b7269-168">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="b7269-169">El recurso compartido de red se especifica con barras diagonales (*//r8/*) y funciona en todas las plataformas compatibles de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b7269-169">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="b7269-170">Confirme que la aplicación publicada para la implementación no se está ejecutando.</span><span class="sxs-lookup"><span data-stu-id="b7269-170">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="b7269-171">Los archivos de la carpeta *publish* quedan bloqueados mientras se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b7269-171">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="b7269-172">No se puede llevar a cabo la implementación porque no se pueden copiar los archivos bloqueados.</span><span class="sxs-lookup"><span data-stu-id="b7269-172">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="b7269-173">Publicar los perfiles</span><span class="sxs-lookup"><span data-stu-id="b7269-173">Publish profiles</span></span>

<span data-ttu-id="b7269-174">En esta sección se usa Visual Studio 2017 y versiones posteriores para crear perfiles de publicación.</span><span class="sxs-lookup"><span data-stu-id="b7269-174">This section uses Visual Studio 2017 and higher to create publishing profiles.</span></span> <span data-ttu-id="b7269-175">Una vez creado, publicación de Visual Studio o la línea de comandos está disponible.</span><span class="sxs-lookup"><span data-stu-id="b7269-175">Once created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="b7269-176">Los perfiles de publicación pueden simplificar el proceso de publicación.</span><span class="sxs-lookup"><span data-stu-id="b7269-176">Publish profiles can simplify the publishing process.</span></span> <span data-ttu-id="b7269-177">Varios perfiles de publicación puede existir.</span><span class="sxs-lookup"><span data-stu-id="b7269-177">Multiple publish profiles can exist.</span></span> <span data-ttu-id="b7269-178">Para crear un perfil de publicación en Visual Studio, haga doble clic en el proyecto en el Explorador de soluciones y seleccione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="b7269-178">To create a publish profile in Visual Studio, right-click on the project in Solution Explorer and select **Publish**.</span></span> <span data-ttu-id="b7269-179">Como alternativa, seleccione **publicar \<nombre del proyecto >** en el menú Generar.</span><span class="sxs-lookup"><span data-stu-id="b7269-179">Alternatively, select **Publish \<project name>** from the build menu.</span></span> <span data-ttu-id="b7269-180">Aparecerá la pestaña **Publicar** de la página de las capacidades de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b7269-180">The **Publish** tab of the application capacities page is displayed.</span></span> <span data-ttu-id="b7269-181">Si el proyecto no contiene ningún perfil de publicación, se mostrará la siguiente página:</span><span class="sxs-lookup"><span data-stu-id="b7269-181">If the project doesn't contain a publish profile, the following page is displayed:</span></span>

![La ficha de publicación de la página de las capacidades de la aplicación con Azure, IIS, FTB, carpeta con Azure seleccionada.](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="b7269-184">Al seleccionar **Carpeta**, el botón **Publicar** crea un perfil de publicación para la carpeta y lo publica.</span><span class="sxs-lookup"><span data-stu-id="b7269-184">When **Folder** is selected, the **Publish** button creates a folder publish profile and publishes.</span></span>

![Pestaña **Publicar** de la página de las capacidades de la aplicación, donde se muestran las opciones Azure, IIS, FTP y Carpeta](visual-studio-publish-profiles/_static/pub1.png)

<span data-ttu-id="b7269-186">Una vez que se crea un perfil de publicación, la **publicar** pestaña cambios y seleccione **crear nuevo perfil** para crear un nuevo perfil.</span><span class="sxs-lookup"><span data-stu-id="b7269-186">Once a publish profile is created, the **Publish** tab changes, and select **Create new profile** to create a new profile.</span></span>

![Pestaña **Publicar** de la página de las capacidades de la aplicación, donde se muestra FolderProfile (creado en el último paso) y el vínculo Crear nuevo perfil](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="b7269-188">El Asistente para publicación admite los siguientes destinos de publicación:</span><span class="sxs-lookup"><span data-stu-id="b7269-188">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="b7269-189">Microsoft Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b7269-189">Microsoft Azure App Service</span></span>
* <span data-ttu-id="b7269-190">IIS, FTP, etc. (para cualquier servidor web)</span><span class="sxs-lookup"><span data-stu-id="b7269-190">IIS, FTP, etc (for any web server)</span></span>
* <span data-ttu-id="b7269-191">Carpeta</span><span class="sxs-lookup"><span data-stu-id="b7269-191">Folder</span></span>
* <span data-ttu-id="b7269-192">Importar perfil (permite la importación de perfiles).</span><span class="sxs-lookup"><span data-stu-id="b7269-192">Import profile (allows profile import).</span></span>
* <span data-ttu-id="b7269-193">Microsoft Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="b7269-193">Microsoft Azure Virtual Machines</span></span>

<span data-ttu-id="b7269-194">Vea [¿Qué opciones de publicación son las adecuadas para mí?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) para más información.</span><span class="sxs-lookup"><span data-stu-id="b7269-194">See [What publishing options are right for me?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) for more information.</span></span>

<span data-ttu-id="b7269-195">Al crear un perfil de publicación con Visual Studio, un *propiedades/PublishProfiles/\<nombre de publicación > .pubxml* se crea el archivo de MSBuild.</span><span class="sxs-lookup"><span data-stu-id="b7269-195">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/\<publish name>.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="b7269-196">Este archivo *.pubxml* es un archivo de MSBuild que contiene opciones de configuración de publicación.</span><span class="sxs-lookup"><span data-stu-id="b7269-196">This *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="b7269-197">Este archivo se puede cambiar para personalizar la compilación y el proceso de publicación.</span><span class="sxs-lookup"><span data-stu-id="b7269-197">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="b7269-198">Este archivo se lee durante el proceso de publicación.</span><span class="sxs-lookup"><span data-stu-id="b7269-198">This file is read by the publishing process.</span></span> <span data-ttu-id="b7269-199">`<LastUsedBuildConfiguration>`es especial porque es una propiedad global y no debe estar en cualquier archivo que se importa en la compilación.</span><span class="sxs-lookup"><span data-stu-id="b7269-199">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="b7269-200">Vea [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) (MSBuild: Cómo establecer la propiedad de configuración) para más información.</span><span class="sxs-lookup"><span data-stu-id="b7269-200">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more info.</span></span>
<span data-ttu-id="b7269-201">El *.pubxml* archivo no debe protegerse en control de código fuente porque depende de la *.user* archivo.</span><span class="sxs-lookup"><span data-stu-id="b7269-201">The *.pubxml* file shouldn't be checked into source control because it depends on the *.user* file.</span></span> <span data-ttu-id="b7269-202">El archivo *.user* nunca debe estar protegido bajo control de código fuente porque puede contener información confidencial y solo es válido para un usuario y un equipo.</span><span class="sxs-lookup"><span data-stu-id="b7269-202">The *.user* file should never be checked into source control because it can contain sensitive information and it's only valid for one user and machine.</span></span>

<span data-ttu-id="b7269-203">La información confidencial (como la contraseña de publicación) se cifra a nivel de usuario/equipo y se almacenan en el archivo *Properties/PublishProfiles/\<nombre_publicación>.pubxml.user*.</span><span class="sxs-lookup"><span data-stu-id="b7269-203">Sensitive information (like the publish password) is encrypted on a per user/machine level and stored in the *Properties/PublishProfiles/\<publish name>.pubxml.user* file.</span></span> <span data-ttu-id="b7269-204">Dado que este archivo puede contener información confidencial, **no** debe estar protegido bajo control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="b7269-204">Because this file can contain sensitive information, it should **not** be checked into source control.</span></span>

<span data-ttu-id="b7269-205">Para obtener información general sobre cómo publicar una aplicación web de ASP.NET Core vea [Host e implementar](index.md).</span><span class="sxs-lookup"><span data-stu-id="b7269-205">For an overview of how to publish a web app on ASP.NET Core see [Host and deploy](index.md).</span></span> <span data-ttu-id="b7269-206">[Hospedar e implementar](index.md) es un proyecto de código abierto en https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="b7269-206">[Host and deploy](index.md) is an open source project at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="b7269-207">`dotnet publish`Puede usar la carpeta, MSDeploy, y [KUDU](https://github.com/projectkudu/kudu/wiki) perfiles de publicación:</span><span class="sxs-lookup"><span data-stu-id="b7269-207">`dotnet publish` can use folder, MSDeploy, and [KUDU](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>
 
<span data-ttu-id="b7269-208">Carpeta (funcione entre plataformas):`dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span><span class="sxs-lookup"><span data-stu-id="b7269-208">Folder (works cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span></span>

<span data-ttu-id="b7269-209">MSDeploy (actualmente este sólo funciona en windows desde MSDeploy no multiplataforma):`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span><span class="sxs-lookup"><span data-stu-id="b7269-209">MSDeploy (currently this only works in windows since MSDeploy isn't cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span></span>

<span data-ttu-id="b7269-210">Paquete de MSDeploy (actualmente este sólo funciona en windows desde MSDeploy no multiplataforma):`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span><span class="sxs-lookup"><span data-stu-id="b7269-210">MSDeploy package(currently this only works in windows since MSDeploy isn't cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span></span>

<span data-ttu-id="b7269-211">En los ejemplos anteriores, **no** pasar `deployonbuild` a `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="b7269-211">In the preceeding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="b7269-212">Para obtener más información, consulte [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="b7269-212">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="b7269-213">`dotnet publish` admite las API de KUDU para publicar en Azure desde cualquier plataforma.</span><span class="sxs-lookup"><span data-stu-id="b7269-213">`dotnet publish` supports KUDU apis to publish to Azure from any platform.</span></span> <span data-ttu-id="b7269-214">Admite las API KUDU pero es compatible con websdk para plat entre publicar en Azure de publicación de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b7269-214">Visual Studio publish does support the KUDU APIs but it's supported by websdk for cross plat publish to Azure.</span></span>

<span data-ttu-id="b7269-215">Agregue un perfil de publicación a la carpeta *Properties/PublishProfiles* con el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="b7269-215">Add a publish profile to *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="b7269-216">Ejecute el siguiente comando se completa el contenido de la publicación y publicarlo en Azure mediante las API de KUDU:</span><span class="sxs-lookup"><span data-stu-id="b7269-216">Running the following command zips up the publish contents and publish it to Azure using the KUDU APIs:</span></span>

`dotnet publish /p:PublishProfile=Azure /p:Configuration=Release`

<span data-ttu-id="b7269-217">Establezca las siguientes propiedades de MSBuild cuando use un perfil de publicación:</span><span class="sxs-lookup"><span data-stu-id="b7269-217">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

<span data-ttu-id="b7269-218">Cuando se publica con un perfil denominado *FolderProfile*, se puede ejecutar cualquiera de los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="b7269-218">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="b7269-219">Al invocar `dotnet build`, llama a `msbuild` para ejecutar la compilación y proceso de publicación.</span><span class="sxs-lookup"><span data-stu-id="b7269-219">When invoking `dotnet build`, it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="b7269-220">Al llamar a `dotnet build` o `msbuild` es básicamente equivalente al pasar de un perfil de la carpeta.</span><span class="sxs-lookup"><span data-stu-id="b7269-220">Calling `dotnet build` or `msbuild` is essentially equivalent when passing in a folder profile.</span></span> <span data-ttu-id="b7269-221">Al llamar a MSBuild directamente en Windows, se utiliza la versión de .NET Framework de MSBuild.</span><span class="sxs-lookup"><span data-stu-id="b7269-221">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="b7269-222">Actualmente, MSDeploy está limitado a los equipos con Windows para efectuar publicaciones.</span><span class="sxs-lookup"><span data-stu-id="b7269-222">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="b7269-223">Al llamar a `dotnet build` en un perfil que no es de carpeta se invoca a MSBuild, que usa MSDeploy en los perfiles que no son de carpeta.</span><span class="sxs-lookup"><span data-stu-id="b7269-223">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="b7269-224">Al llamar a `dotnet build` en un perfil que no es de carpeta, se invoca a MSBuild (mediante MSDeploy), lo cual genera un error (incluso si se ejecuta en una plataforma de Windows).</span><span class="sxs-lookup"><span data-stu-id="b7269-224">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="b7269-225">Para publicar con un perfil que no es de carpeta, llame directamente a MSBuild.</span><span class="sxs-lookup"><span data-stu-id="b7269-225">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="b7269-226">El siguiente perfil de publicación de carpeta se creó con Visual Studio y se publica en un recurso compartido de red:</span><span class="sxs-lookup"><span data-stu-id="b7269-226">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

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

<span data-ttu-id="b7269-227">Tenga en cuenta que `<LastUsedBuildConfiguration>` está establecido en `Release`.</span><span class="sxs-lookup"><span data-stu-id="b7269-227">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="b7269-228">Al efectuar una publicación en Visual Studio, el valor de la propiedad de configuración `<LastUsedBuildConfiguration>` se establece con el valor cuando se inicia el proceso de publicación.</span><span class="sxs-lookup"><span data-stu-id="b7269-228">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="b7269-229">El `<LastUsedBuildConfiguration>` propiedad de configuración es especial y no debe reemplazarse en un archivo de MSBuild importado.</span><span class="sxs-lookup"><span data-stu-id="b7269-229">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="b7269-230">Esta propiedad se puede invalidar desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="b7269-230">This property can be overridden from the command line.</span></span> <span data-ttu-id="b7269-231">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="b7269-231">For example:</span></span>

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="b7269-232">Con MSBuild:</span><span class="sxs-lookup"><span data-stu-id="b7269-232">Using MSBuild:</span></span>

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="b7269-233">Publicar en un punto de conexión de MSDeploy desde la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="b7269-233">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="b7269-234">Como se mencionó anteriormente, la publicación puede realizarse mediante `dotnet publish` o `msbuild` comando.</span><span class="sxs-lookup"><span data-stu-id="b7269-234">As previously mentioned, publishing can be accomplished using `dotnet publish` or the `msbuild` command.</span></span> <span data-ttu-id="b7269-235">`dotnet publish` se ejecuta en el contexto de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b7269-235">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="b7269-236">El `msbuild` comando requiere .NET framework y, por tanto, se limita a los entornos de Windows.</span><span class="sxs-lookup"><span data-stu-id="b7269-236">The `msbuild` command requires .NET framework, and is therefore limited to Windows environments.</span></span>

<span data-ttu-id="b7269-237">La forma más fácil de publicar con MSDeploy consiste en crear primero un perfil de publicación en Visual Studio 2017 y, luego, usar el perfil en la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="b7269-237">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="b7269-238">En el ejemplo siguiente, se crea una aplicación web de ASP.NET Core (mediante `dotnet new mvc`), y se agrega un perfil de publicación de Azure con Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b7269-238">In the following sample, an ASP.NET Core web app is created (using `dotnet new mvc`), and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="b7269-239">Ejecutar `msbuild` desde una **símbolo del sistema para desarrolladores de VS 2017**.</span><span class="sxs-lookup"><span data-stu-id="b7269-239">Run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="b7269-240">El símbolo del sistema para desarrolladores tiene el valor correcto *msbuild.exe* en su ruta de acceso con un conjunto de variables de MSBuild.</span><span class="sxs-lookup"><span data-stu-id="b7269-240">The Developer Command Prompt has the correct *msbuild.exe* in its path with some MSBuild variables set.</span></span>

<span data-ttu-id="b7269-241">MSBuild usa la sintaxis siguiente:</span><span class="sxs-lookup"><span data-stu-id="b7269-241">MSBuild uses the following syntax:</span></span>

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>`

<span data-ttu-id="b7269-242">Obtener la `Password` desde el  *\<nombre de publicación >. Configuración de publicación* archivo.</span><span class="sxs-lookup"><span data-stu-id="b7269-242">Get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="b7269-243">Descargue el *. Configuración de publicación* archivo desde:</span><span class="sxs-lookup"><span data-stu-id="b7269-243">Download the *.PublishSettings* file from either:</span></span>

* <span data-ttu-id="b7269-244">El Explorador de soluciones: Haga doble clic en la aplicación Web y seleccione **descargar un perfil de publicación**.</span><span class="sxs-lookup"><span data-stu-id="b7269-244">Solution Explorer: Right-click on the Web App and select **Download Publish Profile**.</span></span>
* <span data-ttu-id="b7269-245">El Portal de administración de Azure: Seleccione **Get perfil de publicación** desde la hoja de la aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="b7269-245">The Azure Management Portal: Select **Get publish profile** from the Web App blade.</span></span>

<span data-ttu-id="b7269-246">`Username` está en el perfil de publicación.</span><span class="sxs-lookup"><span data-stu-id="b7269-246">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="b7269-247">En el siguiente ejemplo se usa el perfil de publicación "Web11112 - Web Deploy":</span><span class="sxs-lookup"><span data-stu-id="b7269-247">The following sample uses the "Web11112 - Web Deploy" publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a><span data-ttu-id="b7269-248">Excluir archivos</span><span class="sxs-lookup"><span data-stu-id="b7269-248">Excluding files</span></span>

<span data-ttu-id="b7269-249">Al publicar aplicaciones web de ASP.NET Core, se incluyen los artefactos de compilación y el contenido de la carpeta *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="b7269-249">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="b7269-250">`msbuild` admite los [patrones globales](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="b7269-250">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="b7269-251">Por ejemplo, la siguiente `<Content>` marcado de elemento excluye todo el texto (*.txt*) archivos desde el *wwwroot/content* carpeta y todas sus subcarpetas.</span><span class="sxs-lookup"><span data-stu-id="b7269-251">For example, the following `<Content>` element markup excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="b7269-252">El marcado anterior se puede agregar a un perfil de publicación o al archivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="b7269-252">The markup above can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="b7269-253">Si se agrega al archivo *.csproj*, la regla se agrega a todos los perfiles de publicación del proyecto.</span><span class="sxs-lookup"><span data-stu-id="b7269-253">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="b7269-254">El siguiente marcado del elemento `<MsDeploySkipRules>` excluye todos los archivos de la carpeta *wwwroot/content*:</span><span class="sxs-lookup"><span data-stu-id="b7269-254">The following `<MsDeploySkipRules>` element markup exludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="b7269-255">`<MsDeploySkipRules>`No eliminar el *omitir* destinos desde el sitio de implementación.</span><span class="sxs-lookup"><span data-stu-id="b7269-255">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="b7269-256">`<Content>`las carpetas y archivos de destino se eliminan desde el sitio de implementación.</span><span class="sxs-lookup"><span data-stu-id="b7269-256">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="b7269-257">Por ejemplo, suponga que una aplicación web implementadas tenía los siguientes archivos:</span><span class="sxs-lookup"><span data-stu-id="b7269-257">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="b7269-258">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b7269-258">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="b7269-259">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b7269-259">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="b7269-260">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b7269-260">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="b7269-261">Si el siguiente `<MsDeploySkipRules>` se agrega el marcado, no puede eliminar esos archivos en el sitio de implementación.</span><span class="sxs-lookup"><span data-stu-id="b7269-261">If the following `<MsDeploySkipRules>` markup is added, those files wouldn't be deleted on the deployment site.</span></span>

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

<span data-ttu-id="b7269-262">El `<MsDeploySkipRules>` marcado mostrado anteriormente impide que la *omitido* archivos desde que se va a depoyed, pero no elimina esos archivos una vez que se implementen.</span><span class="sxs-lookup"><span data-stu-id="b7269-262">The `<MsDeploySkipRules>` markup shown above prevents the *skipped* files from being depoyed but won't delete those files once they're deployed.</span></span>

<span data-ttu-id="b7269-263">El siguiente `<Content>` marcado elimina los archivos de destino en el sitio de implementación:</span><span class="sxs-lookup"><span data-stu-id="b7269-263">The following `<Content>` markup deletes the targeted files at the deployment site:</span></span>

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="b7269-264">Mediante la implementación de línea de comandos con el `<Content>` marcado anterior, el resultado en un resultado similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="b7269-264">Using command-line deployment with the `<Content>` markup above results in output similar to the following:</span></span>

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

## <a name="including-files"></a><span data-ttu-id="b7269-265">Incluir archivos</span><span class="sxs-lookup"><span data-stu-id="b7269-265">Including files</span></span>

<span data-ttu-id="b7269-266">El siguiente marcado incluye un *imágenes* carpeta fuera del directorio de proyecto para el *wwwroot/imágenes* carpeta del sitio de publicación:</span><span class="sxs-lookup"><span data-stu-id="b7269-266">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="b7269-267">El marcado se puede agregar al archivo *.csproj* o al perfil de publicación.</span><span class="sxs-lookup"><span data-stu-id="b7269-267">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="b7269-268">Si se agrega a la *.csproj* archivo, se incluye en cada perfil de publicación en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="b7269-268">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="b7269-269">En el siguiente marcado resaltado se muestra cómo:</span><span class="sxs-lookup"><span data-stu-id="b7269-269">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="b7269-270">Copiar un archivo de fuera del proyecto a la carpeta *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="b7269-270">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="b7269-271">Excluir la carpeta *wwwroot\Content*.</span><span class="sxs-lookup"><span data-stu-id="b7269-271">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="b7269-272">Excluir *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b7269-272">Exclude *Views\Home\About2.cshtml*.</span></span>

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

<span data-ttu-id="b7269-273">Vea [WebSDK Readme](https://github.com/aspnet/websdk) (Archivo Léame de WebSDK) para ver más ejemplos de implementación.</span><span class="sxs-lookup"><span data-stu-id="b7269-273">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

### <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="b7269-274">Ejecutar un destino antes o después de la publicación</span><span class="sxs-lookup"><span data-stu-id="b7269-274">Run a target before or after publishing</span></span>

<span data-ttu-id="b7269-275">Integrado `BeforePublish` y `AfterPublish` destinos pueden utilizarse para ejecutar un destino antes o después del destino de publicación.</span><span class="sxs-lookup"><span data-stu-id="b7269-275">The built-in `BeforePublish` and `AfterPublish` targets can be used to execute a target before or after the publish target.</span></span> <span data-ttu-id="b7269-276">Se puede agregar el siguiente marcado al perfil de publicación para registrar mensajes en la salida de la consola antes y después de la publicación:</span><span class="sxs-lookup"><span data-stu-id="b7269-276">The following markup can be added to the publish profile to log messages to the console output before and after publishing:</span></span>

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a><span data-ttu-id="b7269-277">Servicio Kudu</span><span class="sxs-lookup"><span data-stu-id="b7269-277">The Kudu service</span></span>

<span data-ttu-id="b7269-278">Para ver los archivos en el una implementación de aplicaciones web de servicio de aplicaciones de Azure, use la [servicio Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="b7269-278">To view the files in the an Azure Apps Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="b7269-279">Anexar la `scm` token utilizado para el nombre de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="b7269-279">Append the `scm` token to the name of the web app.</span></span> <span data-ttu-id="b7269-280">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="b7269-280">For example:</span></span>

| <span data-ttu-id="b7269-281">Dirección URL</span><span class="sxs-lookup"><span data-stu-id="b7269-281">URL</span></span>                                    | <span data-ttu-id="b7269-282">Resultado</span><span class="sxs-lookup"><span data-stu-id="b7269-282">Result</span></span>      |
| -------------------------------------- | ----------- |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="b7269-283">Aplicación web</span><span class="sxs-lookup"><span data-stu-id="b7269-283">Web App</span></span>     |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="b7269-284">Servicio Kudu</span><span class="sxs-lookup"><span data-stu-id="b7269-284">Kudu sevice</span></span> |

<span data-ttu-id="b7269-285">Seleccione el elemento de menú [Consola de depuración](https://github.com/projectkudu/kudu/wiki/Kudu-console) para ver/editar/eliminar/agregar archivos.</span><span class="sxs-lookup"><span data-stu-id="b7269-285">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view/edit/delete/add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b7269-286">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="b7269-286">Additional resources</span></span>

* <span data-ttu-id="b7269-287">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifica la implementación de aplicaciones web y sitios Web a los servidores IIS.</span><span class="sxs-lookup"><span data-stu-id="b7269-287">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="b7269-288">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): problemas de archivos y solicitud de características para la implementación.</span><span class="sxs-lookup"><span data-stu-id="b7269-288">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
