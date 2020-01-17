---
title: Desarrollo de aplicaciones ASP.NET Core con OpenAPI
author: ryanbrandenburg
description: Muestra cómo usar la herramienta "Microsoft.dotnet-openapi" para agregar referencias a archivos OpenAPI.
ms.author: rybrande
ms.date: 09/26/2019
monikerRange: '>= aspnetcore-3.0'
uid: web-api/Microsoft.dotnet-openapi
ms.openlocfilehash: 4be2846f0348788102672978a0487e646da434a0
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75354754"
---
# <a name="develop-aspnet-core-apps-using-openapi-tools"></a><span data-ttu-id="e2855-103">Desarrollo de aplicaciones ASP.NET Core con herramientas de OpenAPI</span><span class="sxs-lookup"><span data-stu-id="e2855-103">Develop ASP.NET Core apps using OpenAPI tools</span></span>

<span data-ttu-id="e2855-104">Por Ryan Brandenburg</span><span class="sxs-lookup"><span data-stu-id="e2855-104">By Ryan Brandenburg</span></span>

<span data-ttu-id="e2855-105">[Microsoft.dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi) es una [herramienta global de .NET Core](/dotnet/core/tools/global-tools) para administrar las referencias de [OpenAPI](https://github.com/OAI/OpenAPI-Specification) dentro de un proyecto.</span><span class="sxs-lookup"><span data-stu-id="e2855-105">[Microsoft.dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi) is a [.NET Core Global Tool](/dotnet/core/tools/global-tools) for managing [OpenAPI](https://github.com/OAI/OpenAPI-Specification) references within a project.</span></span>

## <a name="installation"></a><span data-ttu-id="e2855-106">Instalación</span><span class="sxs-lookup"><span data-stu-id="e2855-106">Installation</span></span>

<span data-ttu-id="e2855-107">Para instalar `Microsoft.dotnet-openapi`, ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="e2855-107">To install `Microsoft.dotnet-openapi`, run the following command:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.dotnet-openapi
```

## <a name="add"></a><span data-ttu-id="e2855-108">Agregar</span><span class="sxs-lookup"><span data-stu-id="e2855-108">Add</span></span>

<span data-ttu-id="e2855-109">Al agregar una referencia de OpenAPI mediante cualquiera de los comandos de esta página, se agrega un elemento `<OpenApiReference />` similar al siguiente al archivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="e2855-109">Adding an OpenAPI reference using any of the commands on this page adds an `<OpenApiReference />` element similar to the following to the *.csproj* file:</span></span>

```xml
<OpenApiReference Include="openapi.json" />
```

<span data-ttu-id="e2855-110">La referencia anterior es necesaria para que la aplicación llame al código de cliente generado.</span><span class="sxs-lookup"><span data-stu-id="e2855-110">The preceding reference is required for the app to call the generated client code.</span></span>

<!-- TODO: Restore after https://github.com/aspnet/AspNetCore/issues/12738
### Add Project

#### Options

| Short option | Long option | Description | Example |
|-------|------|-------|---------|
| -p|--project | The project to operate on. |dotnet openapi add project *--project .\Ref.csproj* ../Ref/ProjRef.csproj |

#### Arguments

|  Argument  | Description | Example |
|-------------|-------------|---------|
| source-file | The source to create a reference from. Must be a project file. |dotnet openapi add project *../Ref/ProjRef.csproj* | -->

### <a name="add-file"></a><span data-ttu-id="e2855-111">Agregar archivo</span><span class="sxs-lookup"><span data-stu-id="e2855-111">Add File</span></span>

#### <a name="options"></a><span data-ttu-id="e2855-112">Opciones</span><span class="sxs-lookup"><span data-stu-id="e2855-112">Options</span></span>

| <span data-ttu-id="e2855-113">Opción corta</span><span class="sxs-lookup"><span data-stu-id="e2855-113">Short option</span></span>| <span data-ttu-id="e2855-114">Opción larga</span><span class="sxs-lookup"><span data-stu-id="e2855-114">Long option</span></span>| <span data-ttu-id="e2855-115">Descripción</span><span class="sxs-lookup"><span data-stu-id="e2855-115">Description</span></span> | <span data-ttu-id="e2855-116">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="e2855-116">Example</span></span> |
|-------|------|-------|---------|
| <span data-ttu-id="e2855-117">-p</span><span class="sxs-lookup"><span data-stu-id="e2855-117">-p</span></span>|<span data-ttu-id="e2855-118">--updateProject</span><span class="sxs-lookup"><span data-stu-id="e2855-118">--updateProject</span></span> | <span data-ttu-id="e2855-119">El proyecto sobre el que actuar.</span><span class="sxs-lookup"><span data-stu-id="e2855-119">The project to operate on.</span></span> |<span data-ttu-id="e2855-120">dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json</span><span class="sxs-lookup"><span data-stu-id="e2855-120">dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="e2855-121">-c</span><span class="sxs-lookup"><span data-stu-id="e2855-121">-c</span></span>|<span data-ttu-id="e2855-122">--code-generator</span><span class="sxs-lookup"><span data-stu-id="e2855-122">--code-generator</span></span>| <span data-ttu-id="e2855-123">El generador de código que se va a aplicar a la referencia.</span><span class="sxs-lookup"><span data-stu-id="e2855-123">The code generator to apply to the reference.</span></span> <span data-ttu-id="e2855-124">Las opciones son `NSwagCSharp` y `NSwagTypeScript`.</span><span class="sxs-lookup"><span data-stu-id="e2855-124">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> <span data-ttu-id="e2855-125">Si no se especifica `--code-generator`, la herramienta usa `NSwagCSharp` de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="e2855-125">If `--code-generator` is not specified the tooling defaults to `NSwagCSharp`.</span></span>|<span data-ttu-id="e2855-126">dotnet openapi add file .\OpenApi.json --code-generator</span><span class="sxs-lookup"><span data-stu-id="e2855-126">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="e2855-127">-h</span><span class="sxs-lookup"><span data-stu-id="e2855-127">-h</span></span>|<span data-ttu-id="e2855-128">--help</span><span class="sxs-lookup"><span data-stu-id="e2855-128">--help</span></span>|<span data-ttu-id="e2855-129">Mostrar información de ayuda.</span><span class="sxs-lookup"><span data-stu-id="e2855-129">Show help information</span></span>|<span data-ttu-id="e2855-130">dotnet openapi add file --help</span><span class="sxs-lookup"><span data-stu-id="e2855-130">dotnet openapi add file --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="e2855-131">Argumentos</span><span class="sxs-lookup"><span data-stu-id="e2855-131">Arguments</span></span>

|  <span data-ttu-id="e2855-132">Argumento</span><span class="sxs-lookup"><span data-stu-id="e2855-132">Argument</span></span>  | <span data-ttu-id="e2855-133">Descripción</span><span class="sxs-lookup"><span data-stu-id="e2855-133">Description</span></span> | <span data-ttu-id="e2855-134">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="e2855-134">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="e2855-135">source-file</span><span class="sxs-lookup"><span data-stu-id="e2855-135">source-file</span></span> | <span data-ttu-id="e2855-136">El origen a partir del cual se va a crear una referencia.</span><span class="sxs-lookup"><span data-stu-id="e2855-136">The source to create a reference from.</span></span> <span data-ttu-id="e2855-137">Debe ser un archivo de OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="e2855-137">Must be an OpenAPI file.</span></span> |<span data-ttu-id="e2855-138">dotnet openapi add file *.\OpenAPI.json*</span><span class="sxs-lookup"><span data-stu-id="e2855-138">dotnet openapi add file *.\OpenAPI.json*</span></span> |

### <a name="add-url"></a><span data-ttu-id="e2855-139">Agregar dirección URL</span><span class="sxs-lookup"><span data-stu-id="e2855-139">Add URL</span></span>

#### <a name="options"></a><span data-ttu-id="e2855-140">Opciones</span><span class="sxs-lookup"><span data-stu-id="e2855-140">Options</span></span>

| <span data-ttu-id="e2855-141">Opción corta</span><span class="sxs-lookup"><span data-stu-id="e2855-141">Short option</span></span>| <span data-ttu-id="e2855-142">Opción larga</span><span class="sxs-lookup"><span data-stu-id="e2855-142">Long option</span></span>| <span data-ttu-id="e2855-143">Descripción</span><span class="sxs-lookup"><span data-stu-id="e2855-143">Description</span></span> | <span data-ttu-id="e2855-144">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="e2855-144">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="e2855-145">-p</span><span class="sxs-lookup"><span data-stu-id="e2855-145">-p</span></span>|<span data-ttu-id="e2855-146">--updateProject</span><span class="sxs-lookup"><span data-stu-id="e2855-146">--updateProject</span></span> | <span data-ttu-id="e2855-147">El proyecto sobre el que actuar.</span><span class="sxs-lookup"><span data-stu-id="e2855-147">The project to operate on.</span></span> |<span data-ttu-id="e2855-148">dotnet openapi add url *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="e2855-148">dotnet openapi add url *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="e2855-149">-o</span><span class="sxs-lookup"><span data-stu-id="e2855-149">-o</span></span>|<span data-ttu-id="e2855-150">--output-file</span><span class="sxs-lookup"><span data-stu-id="e2855-150">--output-file</span></span> | <span data-ttu-id="e2855-151">Dónde colocar la copia local del archivo OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="e2855-151">Where to place the local copy of the OpenAPI file.</span></span> |<span data-ttu-id="e2855-152">dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient.json*</span><span class="sxs-lookup"><span data-stu-id="e2855-152">dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient.json*</span></span> |
| <span data-ttu-id="e2855-153">-c</span><span class="sxs-lookup"><span data-stu-id="e2855-153">-c</span></span>|<span data-ttu-id="e2855-154">--code-generator</span><span class="sxs-lookup"><span data-stu-id="e2855-154">--code-generator</span></span>| <span data-ttu-id="e2855-155">El generador de código que se va a aplicar a la referencia.</span><span class="sxs-lookup"><span data-stu-id="e2855-155">The code generator to apply to the reference.</span></span> <span data-ttu-id="e2855-156">Las opciones son `NSwagCSharp` y `NSwagTypeScript`.</span><span class="sxs-lookup"><span data-stu-id="e2855-156">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> |<span data-ttu-id="e2855-157">dotnet openapi add file .\OpenApi.json --code-generator</span><span class="sxs-lookup"><span data-stu-id="e2855-157">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="e2855-158">-h</span><span class="sxs-lookup"><span data-stu-id="e2855-158">-h</span></span>|<span data-ttu-id="e2855-159">--help</span><span class="sxs-lookup"><span data-stu-id="e2855-159">--help</span></span>|<span data-ttu-id="e2855-160">Mostrar información de ayuda.</span><span class="sxs-lookup"><span data-stu-id="e2855-160">Show help information</span></span>|<span data-ttu-id="e2855-161">dotnet openapi add url --help</span><span class="sxs-lookup"><span data-stu-id="e2855-161">dotnet openapi add url --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="e2855-162">Argumentos</span><span class="sxs-lookup"><span data-stu-id="e2855-162">Arguments</span></span>

|  <span data-ttu-id="e2855-163">Argumento</span><span class="sxs-lookup"><span data-stu-id="e2855-163">Argument</span></span>  | <span data-ttu-id="e2855-164">Descripción</span><span class="sxs-lookup"><span data-stu-id="e2855-164">Description</span></span> | <span data-ttu-id="e2855-165">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="e2855-165">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="e2855-166">source-URL</span><span class="sxs-lookup"><span data-stu-id="e2855-166">source-URL</span></span> | <span data-ttu-id="e2855-167">El origen a partir del cual se va a crear una referencia.</span><span class="sxs-lookup"><span data-stu-id="e2855-167">The source to create a reference from.</span></span> <span data-ttu-id="e2855-168">Debe ser una dirección URL.</span><span class="sxs-lookup"><span data-stu-id="e2855-168">Must be a URL.</span></span> |<span data-ttu-id="e2855-169">dotnet openapi add url `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="e2855-169">dotnet openapi add url `https://contoso.com/openapi.json`</span></span> |

## <a name="remove"></a><span data-ttu-id="e2855-170">Quitar</span><span class="sxs-lookup"><span data-stu-id="e2855-170">Remove</span></span>

<span data-ttu-id="e2855-171">Quita la referencia de OpenAPI que coincide con el nombre de archivo dado del archivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="e2855-171">Removes the OpenAPI reference matching the given filename from the *.csproj* file.</span></span> <span data-ttu-id="e2855-172">Cuando la referencia de OpenAPI se quita, no se generarán los clientes.</span><span class="sxs-lookup"><span data-stu-id="e2855-172">When the OpenAPI reference is removed, clients won't be generated.</span></span> <span data-ttu-id="e2855-173">Los archivos *.json* y *.yaml* locales se eliminan.</span><span class="sxs-lookup"><span data-stu-id="e2855-173">Local *.json* and *.yaml* files are deleted.</span></span>

### <a name="options"></a><span data-ttu-id="e2855-174">Opciones</span><span class="sxs-lookup"><span data-stu-id="e2855-174">Options</span></span>

| <span data-ttu-id="e2855-175">Opción corta</span><span class="sxs-lookup"><span data-stu-id="e2855-175">Short option</span></span>| <span data-ttu-id="e2855-176">Opción larga</span><span class="sxs-lookup"><span data-stu-id="e2855-176">Long option</span></span>| <span data-ttu-id="e2855-177">Descripción</span><span class="sxs-lookup"><span data-stu-id="e2855-177">Description</span></span>| <span data-ttu-id="e2855-178">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="e2855-178">Example</span></span> |
|-------|------|------------|---------|
| <span data-ttu-id="e2855-179">-p</span><span class="sxs-lookup"><span data-stu-id="e2855-179">-p</span></span>|<span data-ttu-id="e2855-180">--updateProject</span><span class="sxs-lookup"><span data-stu-id="e2855-180">--updateProject</span></span> | <span data-ttu-id="e2855-181">El proyecto sobre el que actuar.</span><span class="sxs-lookup"><span data-stu-id="e2855-181">The project to operate on.</span></span> |<span data-ttu-id="e2855-182">dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json</span><span class="sxs-lookup"><span data-stu-id="e2855-182">dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="e2855-183">-h</span><span class="sxs-lookup"><span data-stu-id="e2855-183">-h</span></span>|<span data-ttu-id="e2855-184">--help</span><span class="sxs-lookup"><span data-stu-id="e2855-184">--help</span></span>|<span data-ttu-id="e2855-185">Mostrar información de ayuda.</span><span class="sxs-lookup"><span data-stu-id="e2855-185">Show help information</span></span>|<span data-ttu-id="e2855-186">dotnet openapi remove --help</span><span class="sxs-lookup"><span data-stu-id="e2855-186">dotnet openapi remove --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="e2855-187">Argumentos</span><span class="sxs-lookup"><span data-stu-id="e2855-187">Arguments</span></span>

|  <span data-ttu-id="e2855-188">Argumento</span><span class="sxs-lookup"><span data-stu-id="e2855-188">Argument</span></span>  | <span data-ttu-id="e2855-189">Descripción</span><span class="sxs-lookup"><span data-stu-id="e2855-189">Description</span></span>| <span data-ttu-id="e2855-190">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="e2855-190">Example</span></span> |
| ------------|------------|---------|
| <span data-ttu-id="e2855-191">source-file</span><span class="sxs-lookup"><span data-stu-id="e2855-191">source-file</span></span> | <span data-ttu-id="e2855-192">Origen al que se va a quitar la referencia.</span><span class="sxs-lookup"><span data-stu-id="e2855-192">The source to remove the reference to.</span></span> |<span data-ttu-id="e2855-193">dotnet openapi remove *.\OpenAPI.json*</span><span class="sxs-lookup"><span data-stu-id="e2855-193">dotnet openapi remove *.\OpenAPI.json*</span></span> |

## <a name="refresh"></a><span data-ttu-id="e2855-194">Refresh</span><span class="sxs-lookup"><span data-stu-id="e2855-194">Refresh</span></span>

<span data-ttu-id="e2855-195">Actualiza la versión local de un archivo que se descargó usando el contenido más reciente de la dirección URL de descarga.</span><span class="sxs-lookup"><span data-stu-id="e2855-195">Refreshes the local version of a file that was downloaded using the latest content from the download URL.</span></span>

### <a name="options"></a><span data-ttu-id="e2855-196">Opciones</span><span class="sxs-lookup"><span data-stu-id="e2855-196">Options</span></span>

| <span data-ttu-id="e2855-197">Opción corta</span><span class="sxs-lookup"><span data-stu-id="e2855-197">Short option</span></span>| <span data-ttu-id="e2855-198">Opción larga</span><span class="sxs-lookup"><span data-stu-id="e2855-198">Long option</span></span>| <span data-ttu-id="e2855-199">Descripción</span><span class="sxs-lookup"><span data-stu-id="e2855-199">Description</span></span> | <span data-ttu-id="e2855-200">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="e2855-200">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="e2855-201">-p</span><span class="sxs-lookup"><span data-stu-id="e2855-201">-p</span></span>|<span data-ttu-id="e2855-202">--updateProject</span><span class="sxs-lookup"><span data-stu-id="e2855-202">--updateProject</span></span> | <span data-ttu-id="e2855-203">El proyecto sobre el que actuar.</span><span class="sxs-lookup"><span data-stu-id="e2855-203">The project to operate on.</span></span> | <span data-ttu-id="e2855-204">dotnet openapi refresh *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="e2855-204">dotnet openapi refresh *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="e2855-205">-h</span><span class="sxs-lookup"><span data-stu-id="e2855-205">-h</span></span>|<span data-ttu-id="e2855-206">--help</span><span class="sxs-lookup"><span data-stu-id="e2855-206">--help</span></span>|<span data-ttu-id="e2855-207">Mostrar información de ayuda.</span><span class="sxs-lookup"><span data-stu-id="e2855-207">Show help information</span></span>|<span data-ttu-id="e2855-208">dotnet openapi refresh --help</span><span class="sxs-lookup"><span data-stu-id="e2855-208">dotnet openapi refresh --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="e2855-209">Argumentos</span><span class="sxs-lookup"><span data-stu-id="e2855-209">Arguments</span></span>

|  <span data-ttu-id="e2855-210">Argumento</span><span class="sxs-lookup"><span data-stu-id="e2855-210">Argument</span></span>  | <span data-ttu-id="e2855-211">Descripción</span><span class="sxs-lookup"><span data-stu-id="e2855-211">Description</span></span> | <span data-ttu-id="e2855-212">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="e2855-212">Example</span></span> |
| ------------|-------------|---------|
| <span data-ttu-id="e2855-213">source-URL</span><span class="sxs-lookup"><span data-stu-id="e2855-213">source-URL</span></span> | <span data-ttu-id="e2855-214">Dirección URL desde la que se va a actualizar la referencia.</span><span class="sxs-lookup"><span data-stu-id="e2855-214">The URL to refresh the reference from.</span></span> | <span data-ttu-id="e2855-215">dotnet openapi refresh `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="e2855-215">dotnet openapi refresh `https://contoso.com/openapi.json`</span></span> |
