---
title: Comando dotnet aspnet-codegenerator
author: rick-anderson
ms.author: riande
description: El comando dotnet aspnet-codegenerator aplica scaffolding a los proyectos de ASP.NET Core
ms.date: 07/04/2019
uid: fundamentals/tools/dotnet-aspnet-codegenerator
ms.openlocfilehash: 55b592d9d203970777c84438e210519957abb35d
ms.sourcegitcommit: f6e6730872a7d6f039f97d1df762f0d0bd5e34cf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561688"
---
# <a name="dotnet-aspnet-codegenerator"></a><span data-ttu-id="49313-103">dotnet aspnet-codegenerator</span><span class="sxs-lookup"><span data-stu-id="49313-103">dotnet aspnet-codegenerator</span></span>

<span data-ttu-id="49313-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="49313-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="49313-105">`dotnet aspnet-codegenerator`: ejecuta el motor de scaffolding de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="49313-105">`dotnet aspnet-codegenerator` - Runs the ASP.NET Core scaffolding engine.</span></span> <span data-ttu-id="49313-106">`dotnet aspnet-codegenerator` solo es necesario para aplicar scaffolding desde la línea de comandos, no es necesario para usar la técnica de scaffolding con Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="49313-106">`dotnet aspnet-codegenerator` is only required to scaffold from the command line, it's not need to use scaffolding with Visual Studio.</span></span>

<span data-ttu-id="49313-107">Este artículo se aplica al [SDK de .NET Core 2.2](https://dotnet.microsoft.com/download/dotnet-core/2.2) y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="49313-107">This article applies to [.NET Core 2.2 SDK](https://dotnet.microsoft.com/download/dotnet-core/2.2) and later.</span></span>

## <a name="installing-aspnet-codegenerator"></a><span data-ttu-id="49313-108">Instalación de aspnet-codegenerator</span><span class="sxs-lookup"><span data-stu-id="49313-108">Installing aspnet-codegenerator</span></span>

<span data-ttu-id="49313-109">`aspnet-codegenerator` es una [herramienta global](/dotnet/core/tools/global-tools) que debe estar instalada.</span><span class="sxs-lookup"><span data-stu-id="49313-109">`aspnet-codegenerator` is a [global tool](/dotnet/core/tools/global-tools) that must be installed.</span></span> <span data-ttu-id="49313-110">El comando siguiente instala la versión estable más reciente de la herramienta `aspnet-codegenerator`:</span><span class="sxs-lookup"><span data-stu-id="49313-110">The following command installs the latest stable version of the `aspnet-codegenerator` tool:</span></span>

```console
dotnet tool install -g aspnet-codegenerator
```

<span data-ttu-id="49313-111">El comando siguiente actualiza `aspnet-codegenerator` a la versión estable más reciente disponible desde los SDK instalados de .NET Core:</span><span class="sxs-lookup"><span data-stu-id="49313-111">The following command updates `aspnet-codegenerator` to the latest stable version available from the installed .NET Core SDKs:</span></span>

```console
dotnet tool update -g aspnet-codegenerator
```

## <a name="synopsis"></a><span data-ttu-id="49313-112">Sinopsis</span><span class="sxs-lookup"><span data-stu-id="49313-112">Synopsis</span></span>

```
dotnet aspnet-codegenerator [arguments] [-p|--project] [-n|--nuget-package-dir] [-c|--configuration] [-tfm|--target-framework] [-b|--build-base-path] [--no-build] 
dotnet aspnet-codegenerator [-h|--help]
```

## <a name="description"></a><span data-ttu-id="49313-113">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="49313-113">Description</span></span>

<span data-ttu-id="49313-114">El comando `dotnet aspnet-codegenerator ` global ejecuta el generador de código de ASP.NET Core y el motor de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="49313-114">The `dotnet aspnet-codegenerator ` global command runs the ASP.NET Core code generator and scaffolding engine.</span></span>

## <a name="arguments"></a><span data-ttu-id="49313-115">Argumentos</span><span class="sxs-lookup"><span data-stu-id="49313-115">Arguments</span></span>

`generator`

<span data-ttu-id="49313-116">El generador de código que se va a ejecutar.</span><span class="sxs-lookup"><span data-stu-id="49313-116">The code generator to run.</span></span> <span data-ttu-id="49313-117">Estos generadores están disponibles:</span><span class="sxs-lookup"><span data-stu-id="49313-117">The following generators are available:</span></span>

| <span data-ttu-id="49313-118">Generator</span><span class="sxs-lookup"><span data-stu-id="49313-118">Generator</span></span> | <span data-ttu-id="49313-119">Operación</span><span class="sxs-lookup"><span data-stu-id="49313-119">Operation</span></span> |
| ----------------- | ------------ | 
| <span data-ttu-id="49313-120">área</span><span class="sxs-lookup"><span data-stu-id="49313-120">area</span></span>      | [<span data-ttu-id="49313-121">Aplica scaffolding a un área</span><span class="sxs-lookup"><span data-stu-id="49313-121">Scaffolds an Area</span></span>](/aspnet/core/mvc/controllers/areas) |
  <span data-ttu-id="49313-122">controlador</span><span class="sxs-lookup"><span data-stu-id="49313-122">controller</span></span>| [<span data-ttu-id="49313-123">Aplica scaffolding a un controlador</span><span class="sxs-lookup"><span data-stu-id="49313-123">Scaffolds a controller</span></span>](/aspnet/core/tutorials/first-mvc-app/adding-model) |
  <span data-ttu-id="49313-124">identidad</span><span class="sxs-lookup"><span data-stu-id="49313-124">identity</span></span>  | [<span data-ttu-id="49313-125">Aplica scaffolding a una identidad</span><span class="sxs-lookup"><span data-stu-id="49313-125">Scaffolds Identity</span></span>](/aspnet/core/security/authentication/scaffold-identity) |
  <span data-ttu-id="49313-126">razorpage</span><span class="sxs-lookup"><span data-stu-id="49313-126">razorpage</span></span> | [<span data-ttu-id="49313-127">Aplica scaffolding a Razor Pages</span><span class="sxs-lookup"><span data-stu-id="49313-127">Scaffolds Razor Pages</span></span>](/aspnet/core/tutorials/razor-pages/model) |
  <span data-ttu-id="49313-128">view</span><span class="sxs-lookup"><span data-stu-id="49313-128">view</span></span>      | [<span data-ttu-id="49313-129">Aplica scaffolding a una vista</span><span class="sxs-lookup"><span data-stu-id="49313-129">Scaffolds a view</span></span>](/aspnet/core/mvc/views/overview) |

## <a name="options"></a><span data-ttu-id="49313-130">Opciones</span><span class="sxs-lookup"><span data-stu-id="49313-130">Options</span></span>

`-n|--nuget-package-dir`

<span data-ttu-id="49313-131">Especifica el directorio del paquete de NuGet.</span><span class="sxs-lookup"><span data-stu-id="49313-131">Specifies the NuGet package directory.</span></span>

`-c|--configuration {Debug|Release}`

<span data-ttu-id="49313-132">Define la configuración de compilación.</span><span class="sxs-lookup"><span data-stu-id="49313-132">Defines the build configuration.</span></span> <span data-ttu-id="49313-133">El valor predeterminado es `Debug`.</span><span class="sxs-lookup"><span data-stu-id="49313-133">The default value is `Debug`.</span></span>

`-tfm|--target-framework`

<span data-ttu-id="49313-134">Establece como destino el [marco](/dotnet/standard/frameworks) que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="49313-134">Target [Framework](/dotnet/standard/frameworks) to use.</span></span> <span data-ttu-id="49313-135">Por ejemplo: `net46`.</span><span class="sxs-lookup"><span data-stu-id="49313-135">For example, `net46`.</span></span>

`-b|--build-base-path`

<span data-ttu-id="49313-136">La ruta de acceso de la base de compilación.</span><span class="sxs-lookup"><span data-stu-id="49313-136">The build base path.</span></span>

`-h|--help`

<span data-ttu-id="49313-137">Imprime una corta ayuda para el comando.</span><span class="sxs-lookup"><span data-stu-id="49313-137">Prints out a short help for the command.</span></span>

`--no-build`

<span data-ttu-id="49313-138">No compila el proyecto antes de ejecutarlo.</span><span class="sxs-lookup"><span data-stu-id="49313-138">Doesn't build the project before running.</span></span> <span data-ttu-id="49313-139">También establece la marca `--no-restore` de forma implícita.</span><span class="sxs-lookup"><span data-stu-id="49313-139">It also implicitly sets the `--no-restore` flag.</span></span>

`-p|--project <PATH>`

<span data-ttu-id="49313-140">Especifica la ruta de acceso del archivo del proyecto que se va a ejecutar (nombre de la carpeta o ruta de acceso completa).</span><span class="sxs-lookup"><span data-stu-id="49313-140">Specifies the path of the project file to run (folder name or full path).</span></span> <span data-ttu-id="49313-141">Si no se especifica, se toma como predeterminado el directorio actual.</span><span class="sxs-lookup"><span data-stu-id="49313-141">If not specified, it defaults to the current directory.</span></span>

## <a name="generator-options"></a><span data-ttu-id="49313-142">Opciones del generador</span><span class="sxs-lookup"><span data-stu-id="49313-142">Generator options</span></span>

<span data-ttu-id="49313-143">En las secciones siguientes se detallan las opciones disponibles para los generadores compatibles:</span><span class="sxs-lookup"><span data-stu-id="49313-143">The following sections detail the options available for the supported generators:</span></span>

* <span data-ttu-id="49313-144">Área</span><span class="sxs-lookup"><span data-stu-id="49313-144">Area</span></span>
* <span data-ttu-id="49313-145">Controlador</span><span class="sxs-lookup"><span data-stu-id="49313-145">Controller</span></span>
* <span data-ttu-id="49313-146">identidad</span><span class="sxs-lookup"><span data-stu-id="49313-146">Identity</span></span>  
* <span data-ttu-id="49313-147">Razorpage</span><span class="sxs-lookup"><span data-stu-id="49313-147">Razorpage</span></span>
* <span data-ttu-id="49313-148">Ver</span><span class="sxs-lookup"><span data-stu-id="49313-148">View</span></span>

<a name="area"></a>

### <a name="area-options"></a><span data-ttu-id="49313-149">Opciones del área</span><span class="sxs-lookup"><span data-stu-id="49313-149">Area options</span></span>

<span data-ttu-id="49313-150">Esta herramienta está diseñada para los proyectos web de ASP.NET Core con controladores y vistas.</span><span class="sxs-lookup"><span data-stu-id="49313-150">This tool is intended for ASP.NET Core web projects with controllers and views.</span></span> <span data-ttu-id="49313-151">No está pensada para las aplicaciones de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="49313-151">It's not intended for Razor Pages apps.</span></span>

<span data-ttu-id="49313-152">Uso: `dotnet aspnet-codegenerator area AreaNameToGenerate`</span><span class="sxs-lookup"><span data-stu-id="49313-152">Usage: `dotnet aspnet-codegenerator area AreaNameToGenerate`</span></span>

<span data-ttu-id="49313-153">El comando anterior genera estas carpetas:</span><span class="sxs-lookup"><span data-stu-id="49313-153">The preceding command generates the following folders:</span></span>

* <span data-ttu-id="49313-154">*Áreas*</span><span class="sxs-lookup"><span data-stu-id="49313-154">*Areas*</span></span>
  * <span data-ttu-id="49313-155">*AreaNameToGenerate*</span><span class="sxs-lookup"><span data-stu-id="49313-155">*AreaNameToGenerate*</span></span>
    * <span data-ttu-id="49313-156">*Controladores*</span><span class="sxs-lookup"><span data-stu-id="49313-156">*Controllers*</span></span>
    * <span data-ttu-id="49313-157">*Data*</span><span class="sxs-lookup"><span data-stu-id="49313-157">*Data*</span></span>
    * <span data-ttu-id="49313-158">*Models*</span><span class="sxs-lookup"><span data-stu-id="49313-158">*Models*</span></span>
    * <span data-ttu-id="49313-159">*Vistas*</span><span class="sxs-lookup"><span data-stu-id="49313-159">*Views*</span></span>

<a name="ctl"></a>

### <a name="controller-options"></a><span data-ttu-id="49313-160">Opciones del controlador</span><span class="sxs-lookup"><span data-stu-id="49313-160">Controller options</span></span>

<span data-ttu-id="49313-161">En la tabla siguiente se muestran las opciones para `controller` y `razorpage` de `aspnet-codegenerator`:</span><span class="sxs-lookup"><span data-stu-id="49313-161">The following table lists options for  `aspnet-codegenerator` `controller` and `razorpage`:</span></span>

[!INCLUDE [aspnet-codegenerator-args-md.md](~/includes/aspnet-codegenerator-args-md.md)]

<span data-ttu-id="49313-162">En la tabla siguiente se muestran las opciones únicas para `aspnet-codegenerator controller`:</span><span class="sxs-lookup"><span data-stu-id="49313-162">The following table lists options unique to  `aspnet-codegenerator controller`:</span></span>

| <span data-ttu-id="49313-163">Opción</span><span class="sxs-lookup"><span data-stu-id="49313-163">Option</span></span>               | <span data-ttu-id="49313-164">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="49313-164">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="49313-165">--controllerName o -name</span><span class="sxs-lookup"><span data-stu-id="49313-165">--controllerName or -name</span></span> | <span data-ttu-id="49313-166">El nombre del controlador.</span><span class="sxs-lookup"><span data-stu-id="49313-166">Name of the controller.</span></span> |
| <span data-ttu-id="49313-167">--useAsyncActions o -async</span><span class="sxs-lookup"><span data-stu-id="49313-167">--useAsyncActions or -async</span></span> | <span data-ttu-id="49313-168">Genera acciones asincrónicas del controlador.</span><span class="sxs-lookup"><span data-stu-id="49313-168">Generate async controller actions.</span></span> |
| <span data-ttu-id="49313-169">--noViews o -nv</span><span class="sxs-lookup"><span data-stu-id="49313-169">--noViews or -nv</span></span> | <span data-ttu-id="49313-170">**No** genera vistas.</span><span class="sxs-lookup"><span data-stu-id="49313-170">Generate **no** views.</span></span> |
| <span data-ttu-id="49313-171">--restWithNoViews o -api</span><span class="sxs-lookup"><span data-stu-id="49313-171">--restWithNoViews or -api</span></span>  | <span data-ttu-id="49313-172">Genera un controlador con una API de estilo REST.</span><span class="sxs-lookup"><span data-stu-id="49313-172">Generate a Controller with REST style API.</span></span> <span data-ttu-id="49313-173">Se supone `noViews` y todas las opciones relacionadas con la vista se omiten.</span><span class="sxs-lookup"><span data-stu-id="49313-173">`noViews` is assumed and any view related options are ignored.</span></span> |
| <span data-ttu-id="49313-174">--readWriteActions o -actions</span><span class="sxs-lookup"><span data-stu-id="49313-174">--readWriteActions or -actions</span></span> | <span data-ttu-id="49313-175">Genera un controlador con acciones de lectura/escritura sin un modelo.</span><span class="sxs-lookup"><span data-stu-id="49313-175">Generate controller with read/write actions without a model.</span></span> |

<span data-ttu-id="49313-176">Use el modificador `-h` para obtener ayuda sobre el comando `aspnet-codegenerator controller`:</span><span class="sxs-lookup"><span data-stu-id="49313-176">Use the `-h` switch for help on the `aspnet-codegenerator controller` command:</span></span>

```console
dotnet aspnet-codegenerator controller -h
```

<span data-ttu-id="49313-177">Consulte [Aplicar scaffolding al modelo de película](/aspnet/core/tutorials/razor-pages/model) para ver un ejemplo de `dotnet aspnet-codegenerator controller`.</span><span class="sxs-lookup"><span data-stu-id="49313-177">See [Scaffold the movie model](/aspnet/core/tutorials/razor-pages/model) for an example of `dotnet aspnet-codegenerator controller`.</span></span>

### <a name="razorpage"></a><span data-ttu-id="49313-178">Razorpage</span><span class="sxs-lookup"><span data-stu-id="49313-178">Razorpage</span></span>

<a name="rp"></a>

<span data-ttu-id="49313-179">Es posible aplicar scaffolding a Razor Pages de manera individual si se especifica el nombre de la página nueva y la plantilla a usar.</span><span class="sxs-lookup"><span data-stu-id="49313-179">Razor Pages can be individually scaffolded by specifying the name of the new page and the template to use.</span></span> <span data-ttu-id="49313-180">Las plantillas admitidas son:</span><span class="sxs-lookup"><span data-stu-id="49313-180">The supported templates are:</span></span>

* `Empty`
* `Create`
* `Edit`
* `Delete`
* `Details`
* `List`

<span data-ttu-id="49313-181">Por ejemplo, el comando siguiente usa la plantilla de edición para generar *MyEdit.cshtml* y *MyEdit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="49313-181">For example, the following command uses the Edit template to generate *MyEdit.cshtml* and *MyEdit.cshtml.cs*:</span></span>

```console
dotnet aspnet-codegenerator razorpage MyEdit Edit -m Movie -dc RazorPagesMovieContext -outDir Pages/Movies
```

<span data-ttu-id="49313-182">Por lo general, no se especifica la plantilla ni el nombre del archivo generado y se crean estas plantillas:</span><span class="sxs-lookup"><span data-stu-id="49313-182">Typically, the template and generated file name is not specified, and the following templates are created:</span></span>

* `Create`
* `Edit`
* `Delete`
* `Details`
* `List`

<span data-ttu-id="49313-183">En la tabla siguiente se muestran las opciones para `razorpage` y `controller` de `aspnet-codegenerator`:</span><span class="sxs-lookup"><span data-stu-id="49313-183">The following table lists options for  `aspnet-codegenerator` `razorpage` and `controller`:</span></span>

[!INCLUDE [aspnet-codegenerator-args-md.md](~/includes/aspnet-codegenerator-args-md.md)]

<span data-ttu-id="49313-184">En la tabla siguiente se muestran las opciones únicas para `aspnet-codegenerator razorpage`:</span><span class="sxs-lookup"><span data-stu-id="49313-184">The following table lists options unique to  `aspnet-codegenerator razorpage`:</span></span>

| <span data-ttu-id="49313-185">Opción</span><span class="sxs-lookup"><span data-stu-id="49313-185">Option</span></span>               | <span data-ttu-id="49313-186">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="49313-186">Description</span></span>|
| ----------------- | ------------ |
|   <span data-ttu-id="49313-187">--namespaceName o -namespace</span><span class="sxs-lookup"><span data-stu-id="49313-187">--namespaceName or -namespace</span></span> | <span data-ttu-id="49313-188">El nombre del espacio de nombres que se usará para la clase PageModel generada.</span><span class="sxs-lookup"><span data-stu-id="49313-188">The name of the namespace to use for the generated PageModel</span></span> |
| <span data-ttu-id="49313-189">--partialView o -partial</span><span class="sxs-lookup"><span data-stu-id="49313-189">--partialView or -partial</span></span> | <span data-ttu-id="49313-190">Genera una vista parcial.</span><span class="sxs-lookup"><span data-stu-id="49313-190">Generate a partial view.</span></span> <span data-ttu-id="49313-191">Si se especifica, las opciones de diseño -l y -udl se ignoran.</span><span class="sxs-lookup"><span data-stu-id="49313-191">Layout options -l and -udl are ignored if this is specified.</span></span> |
| <span data-ttu-id="49313-192">--noPageModel o -npm</span><span class="sxs-lookup"><span data-stu-id="49313-192">--noPageModel or -npm</span></span> | <span data-ttu-id="49313-193">Cambia a no generar una clase PageModel para una plantilla vacía.</span><span class="sxs-lookup"><span data-stu-id="49313-193">Switch to not generate a PageModel class for Empty template</span></span> |

<span data-ttu-id="49313-194">Use el modificador `-h` para obtener ayuda sobre el comando `aspnet-codegenerator razorpage`:</span><span class="sxs-lookup"><span data-stu-id="49313-194">Use the `-h` switch for help on the `aspnet-codegenerator razorpage` command:</span></span>

```console
dotnet aspnet-codegenerator razorpage -h
```

<span data-ttu-id="49313-195">Consulte [Aplicar scaffolding al modelo de película](/aspnet/core/tutorials/razor-pages/model) para ver un ejemplo de `dotnet aspnet-codegenerator razorpage`.</span><span class="sxs-lookup"><span data-stu-id="49313-195">See [Scaffold the movie model](/aspnet/core/tutorials/razor-pages/model) for an example of `dotnet aspnet-codegenerator razorpage`.</span></span>

### <a name="identity"></a><span data-ttu-id="49313-196">identidad</span><span class="sxs-lookup"><span data-stu-id="49313-196">Identity</span></span>

<span data-ttu-id="49313-197">Consulte [Identidad de scaffolding](/aspnet/core/security/authentication/scaffold-identity)</span><span class="sxs-lookup"><span data-stu-id="49313-197">See [Scaffold Identity](/aspnet/core/security/authentication/scaffold-identity)</span></span>