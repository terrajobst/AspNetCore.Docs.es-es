---
title: Desarrollo de aplicaciones ASP.NET Core con OpenAPI
author: ryanbrandenburg
description: Muestra cómo usar la herramienta "Microsoft.dotnet-openapi" para agregar referencias a archivos OpenAPI.
ms.author: rybrande
ms.date: 09/26/2019
monikerRange: '>= aspnetcore-3.0'
uid: web-api/Microsoft.dotnet-openapi
ms.openlocfilehash: f5eae9e871bc8efc30d500769adb845ff244a90c
ms.sourcegitcommit: e644258c95dd50a82284f107b9bf3becbc43b2b2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317771"
---
# <a name="develop-aspnet-core-apps-using-openapi-tools"></a><span data-ttu-id="b4de2-103">Desarrollo de aplicaciones ASP.NET Core con herramientas de OpenAPI</span><span class="sxs-lookup"><span data-stu-id="b4de2-103">Develop ASP.NET Core apps using OpenAPI tools</span></span>

<span data-ttu-id="b4de2-104">Por Ryan Brandenburg</span><span class="sxs-lookup"><span data-stu-id="b4de2-104">By Ryan Brandenburg</span></span>

<span data-ttu-id="b4de2-105">[Microsoft.dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi) es una [herramienta global de .NET Core](/dotnet/core/tools/global-tools) para administrar las referencias de [OpenAPI](https://github.com/OAI/OpenAPI-Specification) dentro de un proyecto.</span><span class="sxs-lookup"><span data-stu-id="b4de2-105">[Microsoft.dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi) is a [.NET Core Global Tool](/dotnet/core/tools/global-tools) for managing [OpenAPI](https://github.com/OAI/OpenAPI-Specification) references within a project.</span></span>

## <a name="installation"></a><span data-ttu-id="b4de2-106">Instalación</span><span class="sxs-lookup"><span data-stu-id="b4de2-106">Installation</span></span>

<span data-ttu-id="b4de2-107">Para instalar `Microsoft.dotnet-openapi`, ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="b4de2-107">To install `Microsoft.dotnet-openapi`, run the following command:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.dotnet-openapi
```

## <a name="add"></a><span data-ttu-id="b4de2-108">Agregar</span><span class="sxs-lookup"><span data-stu-id="b4de2-108">Add</span></span>

<span data-ttu-id="b4de2-109">Al agregar una referencia de OpenAPI mediante cualquiera de los comandos de esta página, se agrega un elemento `<OpenApiReference />` similar al siguiente al archivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="b4de2-109">Adding an OpenAPI reference using any of the commands on this page adds an `<OpenApiReference />` element similar to the following to the *.csproj* file:</span></span>

```xml
<OpenApiReference Include="openapi.json" />
```

<span data-ttu-id="b4de2-110">La referencia anterior es necesaria para que la aplicación llame al código de cliente generado.</span><span class="sxs-lookup"><span data-stu-id="b4de2-110">The preceding reference is required for the app to call the generated client code.</span></span>

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

### <a name="add-file"></a><span data-ttu-id="b4de2-111">Agregar archivo</span><span class="sxs-lookup"><span data-stu-id="b4de2-111">Add File</span></span>

#### <a name="options"></a><span data-ttu-id="b4de2-112">Opciones</span><span class="sxs-lookup"><span data-stu-id="b4de2-112">Options</span></span>

| <span data-ttu-id="b4de2-113">Opción corta</span><span class="sxs-lookup"><span data-stu-id="b4de2-113">Short option</span></span>| <span data-ttu-id="b4de2-114">Opción larga</span><span class="sxs-lookup"><span data-stu-id="b4de2-114">Long option</span></span>| <span data-ttu-id="b4de2-115">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="b4de2-115">Description</span></span> | <span data-ttu-id="b4de2-116">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="b4de2-116">Example</span></span> |
|-------|------|-------|---------|
| <span data-ttu-id="b4de2-117">-v</span><span class="sxs-lookup"><span data-stu-id="b4de2-117">-v</span></span>|<span data-ttu-id="b4de2-118">--verbose</span><span class="sxs-lookup"><span data-stu-id="b4de2-118">--verbose</span></span> | <span data-ttu-id="b4de2-119">Mostrar resultado detallado.</span><span class="sxs-lookup"><span data-stu-id="b4de2-119">Show verbose output.</span></span> |<span data-ttu-id="b4de2-120">dotnet openapi add file *-v* .\OpenAPI.json</span><span class="sxs-lookup"><span data-stu-id="b4de2-120">dotnet openapi add file *-v* .\OpenAPI.json</span></span> |
| <span data-ttu-id="b4de2-121">-p</span><span class="sxs-lookup"><span data-stu-id="b4de2-121">-p</span></span>|<span data-ttu-id="b4de2-122">--updateProject</span><span class="sxs-lookup"><span data-stu-id="b4de2-122">--updateProject</span></span> | <span data-ttu-id="b4de2-123">El proyecto sobre el que actuar.</span><span class="sxs-lookup"><span data-stu-id="b4de2-123">The project to operate on.</span></span> |<span data-ttu-id="b4de2-124">dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json</span><span class="sxs-lookup"><span data-stu-id="b4de2-124">dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="b4de2-125">-c</span><span class="sxs-lookup"><span data-stu-id="b4de2-125">-c</span></span>|<span data-ttu-id="b4de2-126">--code-generator</span><span class="sxs-lookup"><span data-stu-id="b4de2-126">--code-generator</span></span>| <span data-ttu-id="b4de2-127">El generador de código que se va a aplicar a la referencia.</span><span class="sxs-lookup"><span data-stu-id="b4de2-127">The code generator to apply to the reference.</span></span> <span data-ttu-id="b4de2-128">Las opciones son `NSwagCSharp` y `NSwagTypeScript`.</span><span class="sxs-lookup"><span data-stu-id="b4de2-128">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> <span data-ttu-id="b4de2-129">Si no se especifica `--code-generator`, la herramienta usa `NSwagCSharp` de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="b4de2-129">If `--code-generator` is not specified the tooling defaults to `NSwagCSharp`.</span></span>|<span data-ttu-id="b4de2-130">dotnet openapi add file .\OpenApi.json --code-generator</span><span class="sxs-lookup"><span data-stu-id="b4de2-130">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="b4de2-131">-h</span><span class="sxs-lookup"><span data-stu-id="b4de2-131">-h</span></span>|<span data-ttu-id="b4de2-132">--help</span><span class="sxs-lookup"><span data-stu-id="b4de2-132">--help</span></span>|<span data-ttu-id="b4de2-133">Mostrar información de ayuda.</span><span class="sxs-lookup"><span data-stu-id="b4de2-133">Show help information</span></span>|<span data-ttu-id="b4de2-134">dotnet openapi add file --help</span><span class="sxs-lookup"><span data-stu-id="b4de2-134">dotnet openapi add file --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="b4de2-135">Argumentos</span><span class="sxs-lookup"><span data-stu-id="b4de2-135">Arguments</span></span>

|  <span data-ttu-id="b4de2-136">Argumento</span><span class="sxs-lookup"><span data-stu-id="b4de2-136">Argument</span></span>  | <span data-ttu-id="b4de2-137">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="b4de2-137">Description</span></span> | <span data-ttu-id="b4de2-138">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="b4de2-138">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="b4de2-139">source-file</span><span class="sxs-lookup"><span data-stu-id="b4de2-139">source-file</span></span> | <span data-ttu-id="b4de2-140">El origen a partir del cual se va a crear una referencia.</span><span class="sxs-lookup"><span data-stu-id="b4de2-140">The source to create a reference from.</span></span> <span data-ttu-id="b4de2-141">Debe ser un archivo de OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="b4de2-141">Must be an OpenAPI file.</span></span> |<span data-ttu-id="b4de2-142">dotnet openapi add file *.\OpenAPI.json*</span><span class="sxs-lookup"><span data-stu-id="b4de2-142">dotnet openapi add file *.\OpenAPI.json*</span></span> |

### <a name="add-url"></a><span data-ttu-id="b4de2-143">Agregar dirección URL</span><span class="sxs-lookup"><span data-stu-id="b4de2-143">Add URL</span></span>

#### <a name="options"></a><span data-ttu-id="b4de2-144">Opciones</span><span class="sxs-lookup"><span data-stu-id="b4de2-144">Options</span></span>

| <span data-ttu-id="b4de2-145">Opción corta</span><span class="sxs-lookup"><span data-stu-id="b4de2-145">Short option</span></span>| <span data-ttu-id="b4de2-146">Opción larga</span><span class="sxs-lookup"><span data-stu-id="b4de2-146">Long option</span></span>| <span data-ttu-id="b4de2-147">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="b4de2-147">Description</span></span> | <span data-ttu-id="b4de2-148">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="b4de2-148">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="b4de2-149">-v</span><span class="sxs-lookup"><span data-stu-id="b4de2-149">-v</span></span>|<span data-ttu-id="b4de2-150">--verbose</span><span class="sxs-lookup"><span data-stu-id="b4de2-150">--verbose</span></span> | <span data-ttu-id="b4de2-151">Mostrar resultado detallado.</span><span class="sxs-lookup"><span data-stu-id="b4de2-151">Show verbose output.</span></span> |<span data-ttu-id="b4de2-152">dotnet openapi add url *-v* `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="b4de2-152">dotnet openapi add url *-v* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="b4de2-153">-p</span><span class="sxs-lookup"><span data-stu-id="b4de2-153">-p</span></span>|<span data-ttu-id="b4de2-154">--updateProject</span><span class="sxs-lookup"><span data-stu-id="b4de2-154">--updateProject</span></span> | <span data-ttu-id="b4de2-155">El proyecto sobre el que actuar.</span><span class="sxs-lookup"><span data-stu-id="b4de2-155">The project to operate on.</span></span> |<span data-ttu-id="b4de2-156">dotnet openapi add url *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="b4de2-156">dotnet openapi add url *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="b4de2-157">-o</span><span class="sxs-lookup"><span data-stu-id="b4de2-157">-o</span></span>|<span data-ttu-id="b4de2-158">--output-file</span><span class="sxs-lookup"><span data-stu-id="b4de2-158">--output-file</span></span> | <span data-ttu-id="b4de2-159">Dónde colocar la copia local del archivo OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="b4de2-159">Where to place the local copy of the OpenAPI file.</span></span> |<span data-ttu-id="b4de2-160">dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient.json*</span><span class="sxs-lookup"><span data-stu-id="b4de2-160">dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient.json*</span></span> |
| <span data-ttu-id="b4de2-161">-c</span><span class="sxs-lookup"><span data-stu-id="b4de2-161">-c</span></span>|<span data-ttu-id="b4de2-162">--code-generator</span><span class="sxs-lookup"><span data-stu-id="b4de2-162">--code-generator</span></span>| <span data-ttu-id="b4de2-163">El generador de código que se va a aplicar a la referencia.</span><span class="sxs-lookup"><span data-stu-id="b4de2-163">The code generator to apply to the reference.</span></span> <span data-ttu-id="b4de2-164">Las opciones son `NSwagCSharp` y `NSwagTypeScript`.</span><span class="sxs-lookup"><span data-stu-id="b4de2-164">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> |<span data-ttu-id="b4de2-165">dotnet openapi add file .\OpenApi.json --code-generator</span><span class="sxs-lookup"><span data-stu-id="b4de2-165">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="b4de2-166">-h</span><span class="sxs-lookup"><span data-stu-id="b4de2-166">-h</span></span>|<span data-ttu-id="b4de2-167">--help</span><span class="sxs-lookup"><span data-stu-id="b4de2-167">--help</span></span>|<span data-ttu-id="b4de2-168">Mostrar información de ayuda.</span><span class="sxs-lookup"><span data-stu-id="b4de2-168">Show help information</span></span>|<span data-ttu-id="b4de2-169">dotnet openapi add url --help</span><span class="sxs-lookup"><span data-stu-id="b4de2-169">dotnet openapi add url --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="b4de2-170">Argumentos</span><span class="sxs-lookup"><span data-stu-id="b4de2-170">Arguments</span></span>

|  <span data-ttu-id="b4de2-171">Argumento</span><span class="sxs-lookup"><span data-stu-id="b4de2-171">Argument</span></span>  | <span data-ttu-id="b4de2-172">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="b4de2-172">Description</span></span> | <span data-ttu-id="b4de2-173">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="b4de2-173">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="b4de2-174">source-URL</span><span class="sxs-lookup"><span data-stu-id="b4de2-174">source-URL</span></span> | <span data-ttu-id="b4de2-175">El origen a partir del cual se va a crear una referencia.</span><span class="sxs-lookup"><span data-stu-id="b4de2-175">The source to create a reference from.</span></span> <span data-ttu-id="b4de2-176">Debe ser una dirección URL.</span><span class="sxs-lookup"><span data-stu-id="b4de2-176">Must be a URL.</span></span> |<span data-ttu-id="b4de2-177">dotnet openapi add url `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="b4de2-177">dotnet openapi add url `https://contoso.com/openapi.json`</span></span> |

## <a name="remove"></a><span data-ttu-id="b4de2-178">Quitar</span><span class="sxs-lookup"><span data-stu-id="b4de2-178">Remove</span></span>

<span data-ttu-id="b4de2-179">Quita la referencia de OpenAPI que coincide con el nombre de archivo dado del archivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="b4de2-179">Removes the OpenAPI reference matching the given filename from the *.csproj* file.</span></span> <span data-ttu-id="b4de2-180">Cuando la referencia de OpenAPI se quita, no se generarán los clientes.</span><span class="sxs-lookup"><span data-stu-id="b4de2-180">When the OpenAPI reference is removed, clients won't be generated.</span></span> <span data-ttu-id="b4de2-181">Los archivos *.json* y *.yaml* locales se eliminan.</span><span class="sxs-lookup"><span data-stu-id="b4de2-181">Local *.json* and *.yaml* files are deleted.</span></span>

### <a name="options"></a><span data-ttu-id="b4de2-182">Opciones</span><span class="sxs-lookup"><span data-stu-id="b4de2-182">Options</span></span>

| <span data-ttu-id="b4de2-183">Opción corta</span><span class="sxs-lookup"><span data-stu-id="b4de2-183">Short option</span></span>| <span data-ttu-id="b4de2-184">Opción larga</span><span class="sxs-lookup"><span data-stu-id="b4de2-184">Long option</span></span>| <span data-ttu-id="b4de2-185">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="b4de2-185">Description</span></span>| <span data-ttu-id="b4de2-186">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="b4de2-186">Example</span></span> |
|-------|------|------------|---------|
| <span data-ttu-id="b4de2-187">-v</span><span class="sxs-lookup"><span data-stu-id="b4de2-187">-v</span></span>|<span data-ttu-id="b4de2-188">--verbose</span><span class="sxs-lookup"><span data-stu-id="b4de2-188">--verbose</span></span> | <span data-ttu-id="b4de2-189">Mostrar resultado detallado.</span><span class="sxs-lookup"><span data-stu-id="b4de2-189">Show verbose output.</span></span> |<span data-ttu-id="b4de2-190">dotnet openapi remove *-v*</span><span class="sxs-lookup"><span data-stu-id="b4de2-190">dotnet openapi remove *-v*</span></span>|
| <span data-ttu-id="b4de2-191">-p</span><span class="sxs-lookup"><span data-stu-id="b4de2-191">-p</span></span>|<span data-ttu-id="b4de2-192">--updateProject</span><span class="sxs-lookup"><span data-stu-id="b4de2-192">--updateProject</span></span> | <span data-ttu-id="b4de2-193">El proyecto sobre el que actuar.</span><span class="sxs-lookup"><span data-stu-id="b4de2-193">The project to operate on.</span></span> |<span data-ttu-id="b4de2-194">dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json</span><span class="sxs-lookup"><span data-stu-id="b4de2-194">dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="b4de2-195">-h</span><span class="sxs-lookup"><span data-stu-id="b4de2-195">-h</span></span>|<span data-ttu-id="b4de2-196">--help</span><span class="sxs-lookup"><span data-stu-id="b4de2-196">--help</span></span>|<span data-ttu-id="b4de2-197">Mostrar información de ayuda.</span><span class="sxs-lookup"><span data-stu-id="b4de2-197">Show help information</span></span>|<span data-ttu-id="b4de2-198">dotnet openapi remove --help</span><span class="sxs-lookup"><span data-stu-id="b4de2-198">dotnet openapi remove --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="b4de2-199">Argumentos</span><span class="sxs-lookup"><span data-stu-id="b4de2-199">Arguments</span></span>

|  <span data-ttu-id="b4de2-200">Argumento</span><span class="sxs-lookup"><span data-stu-id="b4de2-200">Argument</span></span>  | <span data-ttu-id="b4de2-201">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="b4de2-201">Description</span></span>| <span data-ttu-id="b4de2-202">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="b4de2-202">Example</span></span> |
| ------------|------------|---------|
| <span data-ttu-id="b4de2-203">source-file</span><span class="sxs-lookup"><span data-stu-id="b4de2-203">source-file</span></span> | <span data-ttu-id="b4de2-204">Origen al que se va a quitar la referencia.</span><span class="sxs-lookup"><span data-stu-id="b4de2-204">The source to remove the reference to.</span></span> |<span data-ttu-id="b4de2-205">dotnet openapi remove *.\OpenAPI.json*</span><span class="sxs-lookup"><span data-stu-id="b4de2-205">dotnet openapi remove *.\OpenAPI.json*</span></span> |

## <a name="refresh"></a><span data-ttu-id="b4de2-206">Refresh</span><span class="sxs-lookup"><span data-stu-id="b4de2-206">Refresh</span></span>

<span data-ttu-id="b4de2-207">Actualiza la versión local de un archivo que se descargó usando el contenido más reciente de la dirección URL de descarga.</span><span class="sxs-lookup"><span data-stu-id="b4de2-207">Refreshes the local version of a file that was downloaded using the latest content from the download URL.</span></span>

### <a name="options"></a><span data-ttu-id="b4de2-208">Opciones</span><span class="sxs-lookup"><span data-stu-id="b4de2-208">Options</span></span>

| <span data-ttu-id="b4de2-209">Opción corta</span><span class="sxs-lookup"><span data-stu-id="b4de2-209">Short option</span></span>| <span data-ttu-id="b4de2-210">Opción larga</span><span class="sxs-lookup"><span data-stu-id="b4de2-210">Long option</span></span>| <span data-ttu-id="b4de2-211">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="b4de2-211">Description</span></span> | <span data-ttu-id="b4de2-212">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="b4de2-212">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="b4de2-213">-v</span><span class="sxs-lookup"><span data-stu-id="b4de2-213">-v</span></span>|<span data-ttu-id="b4de2-214">--verbose</span><span class="sxs-lookup"><span data-stu-id="b4de2-214">--verbose</span></span> | <span data-ttu-id="b4de2-215">Mostrar resultado detallado.</span><span class="sxs-lookup"><span data-stu-id="b4de2-215">Show verbose output.</span></span> | <span data-ttu-id="b4de2-216">dotnet openapi refresh *-v* `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="b4de2-216">dotnet openapi refresh *-v* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="b4de2-217">-p</span><span class="sxs-lookup"><span data-stu-id="b4de2-217">-p</span></span>|<span data-ttu-id="b4de2-218">--updateProject</span><span class="sxs-lookup"><span data-stu-id="b4de2-218">--updateProject</span></span> | <span data-ttu-id="b4de2-219">El proyecto sobre el que actuar.</span><span class="sxs-lookup"><span data-stu-id="b4de2-219">The project to operate on.</span></span> | <span data-ttu-id="b4de2-220">dotnet openapi refresh *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="b4de2-220">dotnet openapi refresh *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="b4de2-221">-h</span><span class="sxs-lookup"><span data-stu-id="b4de2-221">-h</span></span>|<span data-ttu-id="b4de2-222">--help</span><span class="sxs-lookup"><span data-stu-id="b4de2-222">--help</span></span>|<span data-ttu-id="b4de2-223">Mostrar información de ayuda.</span><span class="sxs-lookup"><span data-stu-id="b4de2-223">Show help information</span></span>|<span data-ttu-id="b4de2-224">dotnet openapi refresh --help</span><span class="sxs-lookup"><span data-stu-id="b4de2-224">dotnet openapi refresh --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="b4de2-225">Argumentos</span><span class="sxs-lookup"><span data-stu-id="b4de2-225">Arguments</span></span>

|  <span data-ttu-id="b4de2-226">Argumento</span><span class="sxs-lookup"><span data-stu-id="b4de2-226">Argument</span></span>  | <span data-ttu-id="b4de2-227">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="b4de2-227">Description</span></span> | <span data-ttu-id="b4de2-228">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="b4de2-228">Example</span></span> |
| ------------|-------------|---------|
| <span data-ttu-id="b4de2-229">source-URL</span><span class="sxs-lookup"><span data-stu-id="b4de2-229">source-URL</span></span> | <span data-ttu-id="b4de2-230">Dirección URL desde la que se va a actualizar la referencia.</span><span class="sxs-lookup"><span data-stu-id="b4de2-230">The URL to refresh the reference from.</span></span> | <span data-ttu-id="b4de2-231">dotnet openapi refresh `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="b4de2-231">dotnet openapi refresh `https://contoso.com/openapi.json`</span></span> |
