---
title: Desarrollo de aplicaciones ASP.NET Core con OpenAPI
author: ryanbrandenburg
description: Muestra cómo usar la herramienta "Microsoft.dotnet-openapi" para agregar referencias a archivos OpenAPI.
ms.author: rybrande
ms.date: 08/26/2019
monikerRange: '>= aspnetcore-3.0'
uid: web-api/Microsoft.dotnet-openapi
ms.openlocfilehash: a9b38bb7e69744d72867bf69cecf1fa92d7c15b3
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187369"
---
# <a name="develop-aspnet-core-apps-using-openapi-tools"></a><span data-ttu-id="69771-103">Desarrollo de aplicaciones ASP.NET Core con herramientas de OpenAPI</span><span class="sxs-lookup"><span data-stu-id="69771-103">Develop ASP.NET Core apps using OpenAPI tools</span></span>

<span data-ttu-id="69771-104">Por Ryan Brandenburg</span><span class="sxs-lookup"><span data-stu-id="69771-104">By Ryan Brandenburg</span></span>

<span data-ttu-id="69771-105">`Microsoft.dotnet-openapi` es una herramienta global de .NET Core para administrar las referencias de [OpenAPI](https://github.com/OAI/OpenAPI-Specification) dentro de un proyecto.</span><span class="sxs-lookup"><span data-stu-id="69771-105">`Microsoft.dotnet-openapi` is a .NET Core global tool for managing [OpenAPI](https://github.com/OAI/OpenAPI-Specification) references within a project.</span></span>

## <a name="installation"></a><span data-ttu-id="69771-106">Instalación</span><span class="sxs-lookup"><span data-stu-id="69771-106">Installation</span></span>

<span data-ttu-id="69771-107">Para instalar `Microsoft.dotnet-openapi`, ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="69771-107">To install `Microsoft.dotnet-openapi`, run the following command:</span></span>

```console
dotnet tool install -g Microsoft.dotnet-openapi
```

<span data-ttu-id="69771-108">`Microsoft.dotnet-openapi` es una [herramienta global de .NET Core](/dotnet/core/tools/global-tools).</span><span class="sxs-lookup"><span data-stu-id="69771-108">`Microsoft.dotnet-openapi` is a [.NET Core Global Tool](/dotnet/core/tools/global-tools).</span></span>

## <a name="add"></a><span data-ttu-id="69771-109">Agregar</span><span class="sxs-lookup"><span data-stu-id="69771-109">Add</span></span>

<span data-ttu-id="69771-110">Al agregar una referencia de OpenAPI mediante cualquiera de los comandos de esta página, se agrega un elemento `<OpenApiReference />` similar al siguiente al archivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="69771-110">Adding an OpenAPI reference using any of the commands on this page adds an `<OpenApiReference />`  element similar to the following to the *.csproj* file:</span></span>

```xml
<OpenApiReference Include="openapi.json" />
```

<span data-ttu-id="69771-111">La referencia anterior es necesaria para que la aplicación llame al código de cliente generado.</span><span class="sxs-lookup"><span data-stu-id="69771-111">The preceding reference is required for the app to call the generated client code.</span></span>

<!-- TODO: Restore after https://github.com/aspnet/AspNetCore/issues/12738
### Add Project

#### Options

| Short option | Long option | Description | Example |
|-------|------|-------|---------|
| -v|--verbose | Show verbose output. |dotnet openapi add project *-v* ../Ref/ProjRef.csproj |
| -p|--project | The project to operate on. |dotnet openapi add project *--project .\Ref.csproj* ../Ref/ProjRef.csproj |

#### Arguments

|  Argument  | Description | Example |
|-------------|-------------|---------|
| source-file | The source to create a reference from. Must be a project file. |dotnet openapi add project *../Ref/ProjRef.csproj* | -->

### <a name="add-file"></a><span data-ttu-id="69771-112">Agregar archivo</span><span class="sxs-lookup"><span data-stu-id="69771-112">Add File</span></span>

#### <a name="options"></a><span data-ttu-id="69771-113">Opciones</span><span class="sxs-lookup"><span data-stu-id="69771-113">Options</span></span>

| <span data-ttu-id="69771-114">Opción corta</span><span class="sxs-lookup"><span data-stu-id="69771-114">Short option</span></span>| <span data-ttu-id="69771-115">Opción larga</span><span class="sxs-lookup"><span data-stu-id="69771-115">Long option</span></span>| <span data-ttu-id="69771-116">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="69771-116">Description</span></span> | <span data-ttu-id="69771-117">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="69771-117">Example</span></span> |
|-------|------|-------|---------|
| <span data-ttu-id="69771-118">-v</span><span class="sxs-lookup"><span data-stu-id="69771-118">-v</span></span>|<span data-ttu-id="69771-119">--verbose</span><span class="sxs-lookup"><span data-stu-id="69771-119">--verbose</span></span> | <span data-ttu-id="69771-120">Mostrar resultado detallado.</span><span class="sxs-lookup"><span data-stu-id="69771-120">Show verbose output.</span></span> |<span data-ttu-id="69771-121">dotnet openapi add file *-v* .\OpenAPI.json</span><span class="sxs-lookup"><span data-stu-id="69771-121">dotnet openapi add file *-v* .\OpenAPI.json</span></span> |
| <span data-ttu-id="69771-122">-p</span><span class="sxs-lookup"><span data-stu-id="69771-122">-p</span></span>|<span data-ttu-id="69771-123">--updateProject</span><span class="sxs-lookup"><span data-stu-id="69771-123">--updateProject</span></span> | <span data-ttu-id="69771-124">El proyecto sobre el que actuar.</span><span class="sxs-lookup"><span data-stu-id="69771-124">The project to operate on.</span></span> |<span data-ttu-id="69771-125">dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json</span><span class="sxs-lookup"><span data-stu-id="69771-125">dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="69771-126">-c</span><span class="sxs-lookup"><span data-stu-id="69771-126">-c</span></span>|<span data-ttu-id="69771-127">--code-generator</span><span class="sxs-lookup"><span data-stu-id="69771-127">--code-generator</span></span>| <span data-ttu-id="69771-128">El generador de código que se va a aplicar a la referencia.</span><span class="sxs-lookup"><span data-stu-id="69771-128">The code generator to apply to the reference.</span></span> <span data-ttu-id="69771-129">Las opciones son `NSwagCSharp` y `NSwagTypeScript`.</span><span class="sxs-lookup"><span data-stu-id="69771-129">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> <span data-ttu-id="69771-130">Si no se especifica `--code-generator`, la herramienta usa `NSwagCSharp` de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="69771-130">If `--code-generator` is not specified the tooling defaults to `NSwagCSharp`.</span></span>|<span data-ttu-id="69771-131">dotnet openapi add file .\OpenApi.json --code-generator</span><span class="sxs-lookup"><span data-stu-id="69771-131">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="69771-132">-h</span><span class="sxs-lookup"><span data-stu-id="69771-132">-h</span></span>|<span data-ttu-id="69771-133">--help</span><span class="sxs-lookup"><span data-stu-id="69771-133">--help</span></span>|<span data-ttu-id="69771-134">Mostrar información de ayuda.</span><span class="sxs-lookup"><span data-stu-id="69771-134">Show help information</span></span>|<span data-ttu-id="69771-135">dotnet openapi add file --help</span><span class="sxs-lookup"><span data-stu-id="69771-135">dotnet openapi add file --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="69771-136">Argumentos</span><span class="sxs-lookup"><span data-stu-id="69771-136">Arguments</span></span>

|  <span data-ttu-id="69771-137">Argumento</span><span class="sxs-lookup"><span data-stu-id="69771-137">Argument</span></span>  | <span data-ttu-id="69771-138">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="69771-138">Description</span></span> | <span data-ttu-id="69771-139">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="69771-139">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="69771-140">source-file</span><span class="sxs-lookup"><span data-stu-id="69771-140">source-file</span></span> | <span data-ttu-id="69771-141">El origen a partir del cual se va a crear una referencia.</span><span class="sxs-lookup"><span data-stu-id="69771-141">The source to create a reference from.</span></span> <span data-ttu-id="69771-142">Debe ser un archivo de OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="69771-142">Must be an OpenAPI file.</span></span> |<span data-ttu-id="69771-143">dotnet openapi add file *.\OpenAPI.json*</span><span class="sxs-lookup"><span data-stu-id="69771-143">dotnet openapi add file *.\OpenAPI.json*</span></span> |

### <a name="add-url"></a><span data-ttu-id="69771-144">Agregar dirección URL</span><span class="sxs-lookup"><span data-stu-id="69771-144">Add URL</span></span>

#### <a name="options"></a><span data-ttu-id="69771-145">Opciones</span><span class="sxs-lookup"><span data-stu-id="69771-145">Options</span></span>

| <span data-ttu-id="69771-146">Opción corta</span><span class="sxs-lookup"><span data-stu-id="69771-146">Short option</span></span>| <span data-ttu-id="69771-147">Opción larga</span><span class="sxs-lookup"><span data-stu-id="69771-147">Long option</span></span>| <span data-ttu-id="69771-148">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="69771-148">Description</span></span> | <span data-ttu-id="69771-149">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="69771-149">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="69771-150">-v</span><span class="sxs-lookup"><span data-stu-id="69771-150">-v</span></span>|<span data-ttu-id="69771-151">--verbose</span><span class="sxs-lookup"><span data-stu-id="69771-151">--verbose</span></span> | <span data-ttu-id="69771-152">Mostrar resultado detallado.</span><span class="sxs-lookup"><span data-stu-id="69771-152">Show verbose output.</span></span> |<span data-ttu-id="69771-153">dotnet openapi add url *-v* `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="69771-153">dotnet openapi add url *-v* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="69771-154">-p</span><span class="sxs-lookup"><span data-stu-id="69771-154">-p</span></span>|<span data-ttu-id="69771-155">--updateProject</span><span class="sxs-lookup"><span data-stu-id="69771-155">--updateProject</span></span> | <span data-ttu-id="69771-156">El proyecto sobre el que actuar.</span><span class="sxs-lookup"><span data-stu-id="69771-156">The project to operate on.</span></span> |<span data-ttu-id="69771-157">dotnet openapi add url *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="69771-157">dotnet openapi add url *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="69771-158">-o</span><span class="sxs-lookup"><span data-stu-id="69771-158">-o</span></span>|<span data-ttu-id="69771-159">--output-file</span><span class="sxs-lookup"><span data-stu-id="69771-159">--output-file</span></span> | <span data-ttu-id="69771-160">Dónde colocar la copia local del archivo OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="69771-160">Where to place the local copy of the OpenAPI file.</span></span> |<span data-ttu-id="69771-161">dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient.json*</span><span class="sxs-lookup"><span data-stu-id="69771-161">dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient.json*</span></span> |
| <span data-ttu-id="69771-162">-c</span><span class="sxs-lookup"><span data-stu-id="69771-162">-c</span></span>|<span data-ttu-id="69771-163">--code-generator</span><span class="sxs-lookup"><span data-stu-id="69771-163">--code-generator</span></span>| <span data-ttu-id="69771-164">El generador de código que se va a aplicar a la referencia.</span><span class="sxs-lookup"><span data-stu-id="69771-164">The code generator to apply to the reference.</span></span> <span data-ttu-id="69771-165">Las opciones son `NSwagCSharp` y `NSwagTypeScript`.</span><span class="sxs-lookup"><span data-stu-id="69771-165">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> |<span data-ttu-id="69771-166">dotnet openapi add file .\OpenApi.json --code-generator</span><span class="sxs-lookup"><span data-stu-id="69771-166">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="69771-167">-h</span><span class="sxs-lookup"><span data-stu-id="69771-167">-h</span></span>|<span data-ttu-id="69771-168">--help</span><span class="sxs-lookup"><span data-stu-id="69771-168">--help</span></span>|<span data-ttu-id="69771-169">Mostrar información de ayuda.</span><span class="sxs-lookup"><span data-stu-id="69771-169">Show help information</span></span>|<span data-ttu-id="69771-170">dotnet openapi add url --help</span><span class="sxs-lookup"><span data-stu-id="69771-170">dotnet openapi add url --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="69771-171">Argumentos</span><span class="sxs-lookup"><span data-stu-id="69771-171">Arguments</span></span>

|  <span data-ttu-id="69771-172">Argumento</span><span class="sxs-lookup"><span data-stu-id="69771-172">Argument</span></span>  | <span data-ttu-id="69771-173">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="69771-173">Description</span></span> | <span data-ttu-id="69771-174">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="69771-174">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="69771-175">source-URL</span><span class="sxs-lookup"><span data-stu-id="69771-175">source-URL</span></span> | <span data-ttu-id="69771-176">El origen a partir del cual se va a crear una referencia.</span><span class="sxs-lookup"><span data-stu-id="69771-176">The source to create a reference from.</span></span> <span data-ttu-id="69771-177">Debe ser una dirección URL.</span><span class="sxs-lookup"><span data-stu-id="69771-177">Must be a URL.</span></span> |<span data-ttu-id="69771-178">dotnet openapi add url `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="69771-178">dotnet openapi add url `https://contoso.com/openapi.json`</span></span> |

## <a name="remove"></a><span data-ttu-id="69771-179">Quitar</span><span class="sxs-lookup"><span data-stu-id="69771-179">Remove</span></span>

<span data-ttu-id="69771-180">Quita la referencia de OpenAPI que coincide con el nombre de archivo dado del archivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="69771-180">Removes the OpenAPI reference matching the given filename from the *.csproj* file.</span></span> <span data-ttu-id="69771-181">Cuando la referencia de OpenAPI se quita, no se generarán los clientes.</span><span class="sxs-lookup"><span data-stu-id="69771-181">When the OpenAPI reference is removed, clients won't be generated.</span></span> <span data-ttu-id="69771-182">Los archivos *.json* y *.yaml* locales se eliminan.</span><span class="sxs-lookup"><span data-stu-id="69771-182">Local *.json* and *.yaml* files are deleted.</span></span>

### <a name="options"></a><span data-ttu-id="69771-183">Opciones</span><span class="sxs-lookup"><span data-stu-id="69771-183">Options</span></span>

| <span data-ttu-id="69771-184">Opción corta</span><span class="sxs-lookup"><span data-stu-id="69771-184">Short option</span></span>| <span data-ttu-id="69771-185">Opción larga</span><span class="sxs-lookup"><span data-stu-id="69771-185">Long option</span></span>| <span data-ttu-id="69771-186">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="69771-186">Description</span></span>| <span data-ttu-id="69771-187">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="69771-187">Example</span></span> |
|-------|------|------------|---------|
| <span data-ttu-id="69771-188">-v</span><span class="sxs-lookup"><span data-stu-id="69771-188">-v</span></span>|<span data-ttu-id="69771-189">--verbose</span><span class="sxs-lookup"><span data-stu-id="69771-189">--verbose</span></span> | <span data-ttu-id="69771-190">Mostrar resultado detallado.</span><span class="sxs-lookup"><span data-stu-id="69771-190">Show verbose output.</span></span> |<span data-ttu-id="69771-191">dotnet openapi remove *-v*</span><span class="sxs-lookup"><span data-stu-id="69771-191">dotnet openapi remove *-v*</span></span>|
| <span data-ttu-id="69771-192">-p</span><span class="sxs-lookup"><span data-stu-id="69771-192">-p</span></span>|<span data-ttu-id="69771-193">--updateProject</span><span class="sxs-lookup"><span data-stu-id="69771-193">--updateProject</span></span> | <span data-ttu-id="69771-194">El proyecto sobre el que actuar.</span><span class="sxs-lookup"><span data-stu-id="69771-194">The project to operate on.</span></span> |<span data-ttu-id="69771-195">dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json</span><span class="sxs-lookup"><span data-stu-id="69771-195">dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="69771-196">-h</span><span class="sxs-lookup"><span data-stu-id="69771-196">-h</span></span>|<span data-ttu-id="69771-197">--help</span><span class="sxs-lookup"><span data-stu-id="69771-197">--help</span></span>|<span data-ttu-id="69771-198">Mostrar información de ayuda.</span><span class="sxs-lookup"><span data-stu-id="69771-198">Show help information</span></span>|<span data-ttu-id="69771-199">dotnet openapi remove --help</span><span class="sxs-lookup"><span data-stu-id="69771-199">dotnet openapi remove --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="69771-200">Argumentos</span><span class="sxs-lookup"><span data-stu-id="69771-200">Arguments</span></span>

|  <span data-ttu-id="69771-201">Argumento</span><span class="sxs-lookup"><span data-stu-id="69771-201">Argument</span></span>  | <span data-ttu-id="69771-202">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="69771-202">Description</span></span>| <span data-ttu-id="69771-203">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="69771-203">Example</span></span> |
| ------------|------------|---------|
| <span data-ttu-id="69771-204">source-file</span><span class="sxs-lookup"><span data-stu-id="69771-204">source-file</span></span> | <span data-ttu-id="69771-205">Origen al que se va a quitar la referencia.</span><span class="sxs-lookup"><span data-stu-id="69771-205">The source to remove the reference to.</span></span> |<span data-ttu-id="69771-206">dotnet openapi remove *.\OpenAPI.json*</span><span class="sxs-lookup"><span data-stu-id="69771-206">dotnet openapi remove *.\OpenAPI.json*</span></span> |

## <a name="refresh"></a><span data-ttu-id="69771-207">Refresh</span><span class="sxs-lookup"><span data-stu-id="69771-207">Refresh</span></span>

<span data-ttu-id="69771-208">Actualiza la versión local de un archivo que se descargó usando el contenido más reciente de la dirección URL de descarga.</span><span class="sxs-lookup"><span data-stu-id="69771-208">Refreshes the local version of a file that was downloaded using the latest content from the download URL.</span></span>

### <a name="options"></a><span data-ttu-id="69771-209">Opciones</span><span class="sxs-lookup"><span data-stu-id="69771-209">Options</span></span>

| <span data-ttu-id="69771-210">Opción corta</span><span class="sxs-lookup"><span data-stu-id="69771-210">Short option</span></span>| <span data-ttu-id="69771-211">Opción larga</span><span class="sxs-lookup"><span data-stu-id="69771-211">Long option</span></span>| <span data-ttu-id="69771-212">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="69771-212">Description</span></span> | <span data-ttu-id="69771-213">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="69771-213">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="69771-214">-v</span><span class="sxs-lookup"><span data-stu-id="69771-214">-v</span></span>|<span data-ttu-id="69771-215">--verbose</span><span class="sxs-lookup"><span data-stu-id="69771-215">--verbose</span></span> | <span data-ttu-id="69771-216">Mostrar resultado detallado.</span><span class="sxs-lookup"><span data-stu-id="69771-216">Show verbose output.</span></span> | <span data-ttu-id="69771-217">dotnet openapi refresh *-v* `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="69771-217">dotnet openapi refresh *-v* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="69771-218">-p</span><span class="sxs-lookup"><span data-stu-id="69771-218">-p</span></span>|<span data-ttu-id="69771-219">--updateProject</span><span class="sxs-lookup"><span data-stu-id="69771-219">--updateProject</span></span> | <span data-ttu-id="69771-220">El proyecto sobre el que actuar.</span><span class="sxs-lookup"><span data-stu-id="69771-220">The project to operate on.</span></span> | <span data-ttu-id="69771-221">dotnet openapi refresh *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="69771-221">dotnet openapi refresh *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="69771-222">-h</span><span class="sxs-lookup"><span data-stu-id="69771-222">-h</span></span>|<span data-ttu-id="69771-223">--help</span><span class="sxs-lookup"><span data-stu-id="69771-223">--help</span></span>|<span data-ttu-id="69771-224">Mostrar información de ayuda.</span><span class="sxs-lookup"><span data-stu-id="69771-224">Show help information</span></span>|<span data-ttu-id="69771-225">dotnet openapi refresh --help</span><span class="sxs-lookup"><span data-stu-id="69771-225">dotnet openapi refresh --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="69771-226">Argumentos</span><span class="sxs-lookup"><span data-stu-id="69771-226">Arguments</span></span>

|  <span data-ttu-id="69771-227">Argumento</span><span class="sxs-lookup"><span data-stu-id="69771-227">Argument</span></span>  | <span data-ttu-id="69771-228">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="69771-228">Description</span></span> | <span data-ttu-id="69771-229">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="69771-229">Example</span></span> |
| ------------|-------------|---------|
| <span data-ttu-id="69771-230">source-URL</span><span class="sxs-lookup"><span data-stu-id="69771-230">source-URL</span></span> | <span data-ttu-id="69771-231">Dirección URL desde la que se va a actualizar la referencia.</span><span class="sxs-lookup"><span data-stu-id="69771-231">The URL to refresh the reference from.</span></span> | <span data-ttu-id="69771-232">dotnet openapi refresh `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="69771-232">dotnet openapi refresh `https://contoso.com/openapi.json`</span></span> |
