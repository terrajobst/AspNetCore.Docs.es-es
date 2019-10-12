---
title: Administración de referencias de protobuf con dotnet-GRPC
author: juntaoluo
description: Obtenga información sobre cómo agregar, actualizar, eliminar y enumerar referencias de protobuf con la herramienta global dotnet-GRPC.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/24/2019
uid: grpc/dotnet-grpc
ms.openlocfilehash: ebd57419be24f7f4ed9765e36cf14189be8438b1
ms.sourcegitcommit: 020c3760492efed71b19e476f25392dda5dd7388
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/12/2019
ms.locfileid: "72290050"
---
# <a name="manage-protobuf-references-with-dotnet-grpc"></a><span data-ttu-id="9da37-103">Administración de referencias de protobuf con dotnet-GRPC</span><span class="sxs-lookup"><span data-stu-id="9da37-103">Manage Protobuf references with dotnet-grpc</span></span>

<span data-ttu-id="9da37-104">Por [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="9da37-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="9da37-105">`dotnet-grpc` es una herramienta global de .NET Core para administrar las referencias de protobuf dentro de un proyecto de gRPC de .NET.</span><span class="sxs-lookup"><span data-stu-id="9da37-105">`dotnet-grpc` is a .NET Core Global Tool for managing Protobuf references within a .NET gRPC project.</span></span> <span data-ttu-id="9da37-106">La herramienta se puede usar para agregar, actualizar, quitar y enumerar las referencias de protobuf.</span><span class="sxs-lookup"><span data-stu-id="9da37-106">The tool can be used to add, refresh, remove, and list Protobuf references.</span></span>

## <a name="installation"></a><span data-ttu-id="9da37-107">Instalación</span><span class="sxs-lookup"><span data-stu-id="9da37-107">Installation</span></span>

<span data-ttu-id="9da37-108">Para instalar la [herramienta global de .net Core](/dotnet/core/tools/global-tools)`dotnet-grpc`, ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="9da37-108">To install the `dotnet-grpc` [.NET Core Global Tool](/dotnet/core/tools/global-tools), run the following command:</span></span>

```dotnetcli
dotnet tool install -g dotnet-grpc
```

## <a name="add-references"></a><span data-ttu-id="9da37-109">Agregar referencias</span><span class="sxs-lookup"><span data-stu-id="9da37-109">Add references</span></span>

<span data-ttu-id="9da37-110">`dotnet-grpc` se puede usar para agregar referencias de protobuf como elementos `<Protobuf />` al archivo *. csproj* :</span><span class="sxs-lookup"><span data-stu-id="9da37-110">`dotnet-grpc` can be used to add Protobuf references as `<Protobuf />` items to the *.csproj* file:</span></span>

```xml
<Protobuf Include="..\Proto\count.proto" GrpcServices="Server" Link="Protos\count.proto" />
```

<span data-ttu-id="9da37-111">Las referencias de protobuf se usan para generar C# los recursos de cliente o servidor.</span><span class="sxs-lookup"><span data-stu-id="9da37-111">The Protobuf references are used to generate the C# client and/or server assets.</span></span> <span data-ttu-id="9da37-112">El `dotnet-grpc`tool puede:</span><span class="sxs-lookup"><span data-stu-id="9da37-112">The `dotnet-grpc`tool can:</span></span>

* <span data-ttu-id="9da37-113">Cree una referencia de protobuf a partir de archivos locales en el disco.</span><span class="sxs-lookup"><span data-stu-id="9da37-113">Create a Protobuf reference from local files on disk.</span></span>
* <span data-ttu-id="9da37-114">Cree una referencia de protobuf desde un archivo remoto especificado por una dirección URL.</span><span class="sxs-lookup"><span data-stu-id="9da37-114">Create a Protobuf reference from a remote file specified by a URL.</span></span>
* <span data-ttu-id="9da37-115">Asegúrese de que se han agregado al proyecto las dependencias de paquete gRPC correctas.</span><span class="sxs-lookup"><span data-stu-id="9da37-115">Ensure the correct gRPC package dependencies are added to the project.</span></span>

<span data-ttu-id="9da37-116">Por ejemplo, el paquete `Grpc.AspNetCore` se agrega a una aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="9da37-116">For example, the `Grpc.AspNetCore` package is added to a web app.</span></span> <span data-ttu-id="9da37-117">`Grpc.AspNetCore` contiene bibliotecas de cliente y de servidor gRPC y compatibilidad con herramientas.</span><span class="sxs-lookup"><span data-stu-id="9da37-117">`Grpc.AspNetCore` contains gRPC server and client libraries and tooling support.</span></span> <span data-ttu-id="9da37-118">Como alternativa, los paquetes `Grpc.Net.Client`, `Grpc.Tools` y `Google.Protobuf`, que contienen solo las bibliotecas de cliente de gRPC y la compatibilidad con herramientas, se agregan a una aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="9da37-118">Alternatively, the `Grpc.Net.Client`, `Grpc.Tools` and `Google.Protobuf` packages, which contain only the gRPC client libraries and tooling support, are added to a Console app.</span></span>

### <a name="add-file"></a><span data-ttu-id="9da37-119">Agregar archivo</span><span class="sxs-lookup"><span data-stu-id="9da37-119">Add file</span></span>

<span data-ttu-id="9da37-120">El comando `add-file` se usa para agregar archivos locales en el disco como referencias de protobuf.</span><span class="sxs-lookup"><span data-stu-id="9da37-120">The `add-file` command is used to add local files on disk as Protobuf references.</span></span> <span data-ttu-id="9da37-121">Las rutas de acceso de archivo proporcionadas:</span><span class="sxs-lookup"><span data-stu-id="9da37-121">The file paths provided:</span></span>

* <span data-ttu-id="9da37-122">Puede ser relativa al directorio actual o a las rutas de acceso absolutas.</span><span class="sxs-lookup"><span data-stu-id="9da37-122">Can be relative to the current directory or absolute paths.</span></span>
* <span data-ttu-id="9da37-123">Puede contener caracteres comodín para [comodines](https://wikipedia.org/wiki/Glob_(programming))de archivos basados en patrones.</span><span class="sxs-lookup"><span data-stu-id="9da37-123">May contain wild cards for pattern-based file [globbing](https://wikipedia.org/wiki/Glob_(programming)).</span></span>

<span data-ttu-id="9da37-124">Si algún archivo está fuera del directorio del proyecto, se agrega un elemento `Link` para mostrar el archivo en la carpeta `Protos` en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9da37-124">If any files are outside the project directory, a `Link` element is added to display the file under the folder `Protos` in Visual Studio.</span></span>

### <a name="usage"></a><span data-ttu-id="9da37-125">Uso</span><span class="sxs-lookup"><span data-stu-id="9da37-125">Usage</span></span>

```dotnetcli
dotnet grpc add-file [options] <files>...
```

#### <a name="arguments"></a><span data-ttu-id="9da37-126">Argumentos</span><span class="sxs-lookup"><span data-stu-id="9da37-126">Arguments</span></span>

| <span data-ttu-id="9da37-127">Argumento</span><span class="sxs-lookup"><span data-stu-id="9da37-127">Argument</span></span> | <span data-ttu-id="9da37-128">Descripción</span><span class="sxs-lookup"><span data-stu-id="9da37-128">Description</span></span> |
|-|-|
| <span data-ttu-id="9da37-129">archivos</span><span class="sxs-lookup"><span data-stu-id="9da37-129">files</span></span> | <span data-ttu-id="9da37-130">El archivo protobuf hace referencia a.</span><span class="sxs-lookup"><span data-stu-id="9da37-130">The protobuf file references.</span></span> <span data-ttu-id="9da37-131">Puede tratarse de una ruta de acceso a Glob para los archivos protobuf locales.</span><span class="sxs-lookup"><span data-stu-id="9da37-131">These can be a path to glob for local protobuf files.</span></span> |

#### <a name="options"></a><span data-ttu-id="9da37-132">Opciones</span><span class="sxs-lookup"><span data-stu-id="9da37-132">Options</span></span>

| <span data-ttu-id="9da37-133">Opción corta</span><span class="sxs-lookup"><span data-stu-id="9da37-133">Short option</span></span> | <span data-ttu-id="9da37-134">Opción larga</span><span class="sxs-lookup"><span data-stu-id="9da37-134">Long option</span></span> | <span data-ttu-id="9da37-135">Descripción</span><span class="sxs-lookup"><span data-stu-id="9da37-135">Description</span></span> |
|-|-|-|
| <span data-ttu-id="9da37-136">-p</span><span class="sxs-lookup"><span data-stu-id="9da37-136">-p</span></span> | <span data-ttu-id="9da37-137">--proyecto</span><span class="sxs-lookup"><span data-stu-id="9da37-137">--project</span></span> | <span data-ttu-id="9da37-138">Ruta de acceso al archivo de proyecto en el que se va a operar.</span><span class="sxs-lookup"><span data-stu-id="9da37-138">The path to the project file to operate on.</span></span> <span data-ttu-id="9da37-139">Si no se especifica un archivo, el comando busca uno en el directorio actual.</span><span class="sxs-lookup"><span data-stu-id="9da37-139">If a file is not specified, the command searches the current directory for one.</span></span>
| <span data-ttu-id="9da37-140">-s</span><span class="sxs-lookup"><span data-stu-id="9da37-140">-s</span></span> | <span data-ttu-id="9da37-141">--servicios</span><span class="sxs-lookup"><span data-stu-id="9da37-141">--services</span></span> | <span data-ttu-id="9da37-142">El tipo de servicios de gRPC que deben generarse.</span><span class="sxs-lookup"><span data-stu-id="9da37-142">The type of gRPC services that should be generated.</span></span> <span data-ttu-id="9da37-143">Si se especifica `Default`, se usa `Both` para los proyectos web y `Client` para los proyectos que no son de Web.</span><span class="sxs-lookup"><span data-stu-id="9da37-143">If `Default` is specified, `Both` is used for Web projects and `Client` is used for non-Web projects.</span></span> <span data-ttu-id="9da37-144">Los valores aceptados son `Both`, `Client`, `Default`, `None`, `Server`.</span><span class="sxs-lookup"><span data-stu-id="9da37-144">Accepted values are `Both`, `Client`, `Default`, `None`, `Server`.</span></span>
| <span data-ttu-id="9da37-145">-i</span><span class="sxs-lookup"><span data-stu-id="9da37-145">-i</span></span> | <span data-ttu-id="9da37-146">--Additional-Import-dirs</span><span class="sxs-lookup"><span data-stu-id="9da37-146">--additional-import-dirs</span></span> | <span data-ttu-id="9da37-147">Directorios adicionales que se usarán al resolver importaciones para los archivos protobuf.</span><span class="sxs-lookup"><span data-stu-id="9da37-147">Additional directories to be used when resolving imports for the protobuf files.</span></span> <span data-ttu-id="9da37-148">Se trata de una lista de rutas de acceso separadas por punto y coma.</span><span class="sxs-lookup"><span data-stu-id="9da37-148">This is a semicolon separated list of paths.</span></span>
| | <span data-ttu-id="9da37-149">--acceso</span><span class="sxs-lookup"><span data-stu-id="9da37-149">--access</span></span> | <span data-ttu-id="9da37-150">Modificador de acceso que se va a usar C# para las clases generadas.</span><span class="sxs-lookup"><span data-stu-id="9da37-150">The access modifier to use for the generated C# classes.</span></span> <span data-ttu-id="9da37-151">El valor predeterminado es `Public`.</span><span class="sxs-lookup"><span data-stu-id="9da37-151">The default value is `Public`.</span></span> <span data-ttu-id="9da37-152">Los valores aceptados son `Internal` y `Public`.</span><span class="sxs-lookup"><span data-stu-id="9da37-152">Accepted values are `Internal` and `Public`.</span></span>

### <a name="add-url"></a><span data-ttu-id="9da37-153">Agregar dirección URL</span><span class="sxs-lookup"><span data-stu-id="9da37-153">Add URL</span></span>

<span data-ttu-id="9da37-154">El comando `add-url` se usa para agregar un archivo remoto especificado por una dirección URL de origen como referencia de protobuf.</span><span class="sxs-lookup"><span data-stu-id="9da37-154">The `add-url` command is used to add a remote file specified by an source URL as Protobuf reference.</span></span> <span data-ttu-id="9da37-155">Se debe proporcionar una ruta de acceso de archivo para especificar dónde descargar el archivo remoto.</span><span class="sxs-lookup"><span data-stu-id="9da37-155">A file path must be provided to specify where to download the remote file.</span></span> <span data-ttu-id="9da37-156">La ruta de acceso del archivo puede ser relativa al directorio actual o a una ruta de acceso absoluta.</span><span class="sxs-lookup"><span data-stu-id="9da37-156">The file path can be relative to the current directory or an absolute path.</span></span> <span data-ttu-id="9da37-157">Si la ruta de acceso del archivo está fuera del directorio del proyecto, se agrega un elemento `Link` para mostrar el archivo en la carpeta virtual `Protos` en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9da37-157">If the file path is outside the project directory, a `Link` element is added to display the file under the virtual folder `Protos` in Visual Studio.</span></span>

### <a name="usage"></a><span data-ttu-id="9da37-158">Uso</span><span class="sxs-lookup"><span data-stu-id="9da37-158">Usage</span></span>

```dotnetcli
dotnet-grpc add-url [options] <url>
```

#### <a name="arguments"></a><span data-ttu-id="9da37-159">Argumentos</span><span class="sxs-lookup"><span data-stu-id="9da37-159">Arguments</span></span>

| <span data-ttu-id="9da37-160">Argumento</span><span class="sxs-lookup"><span data-stu-id="9da37-160">Argument</span></span> | <span data-ttu-id="9da37-161">Descripción</span><span class="sxs-lookup"><span data-stu-id="9da37-161">Description</span></span> |
|-|-|
| <span data-ttu-id="9da37-162">url</span><span class="sxs-lookup"><span data-stu-id="9da37-162">url</span></span> | <span data-ttu-id="9da37-163">La dirección URL de un archivo protobuf remoto.</span><span class="sxs-lookup"><span data-stu-id="9da37-163">The URL to a remote protobuf file.</span></span> |

#### <a name="options"></a><span data-ttu-id="9da37-164">Opciones</span><span class="sxs-lookup"><span data-stu-id="9da37-164">Options</span></span>

| <span data-ttu-id="9da37-165">Opción corta</span><span class="sxs-lookup"><span data-stu-id="9da37-165">Short option</span></span> | <span data-ttu-id="9da37-166">Opción larga</span><span class="sxs-lookup"><span data-stu-id="9da37-166">Long option</span></span> | <span data-ttu-id="9da37-167">Descripción</span><span class="sxs-lookup"><span data-stu-id="9da37-167">Description</span></span> |
|-|-|-|
| <span data-ttu-id="9da37-168">-o</span><span class="sxs-lookup"><span data-stu-id="9da37-168">-o</span></span> | <span data-ttu-id="9da37-169">--salida</span><span class="sxs-lookup"><span data-stu-id="9da37-169">--output</span></span> | <span data-ttu-id="9da37-170">Especifica la ruta de acceso de descarga para el archivo protobuf remoto.</span><span class="sxs-lookup"><span data-stu-id="9da37-170">Specifies the download path for the remote protobuf file.</span></span> <span data-ttu-id="9da37-171">Esta es una opción necesaria.</span><span class="sxs-lookup"><span data-stu-id="9da37-171">This is a required option.</span></span>
| <span data-ttu-id="9da37-172">-p</span><span class="sxs-lookup"><span data-stu-id="9da37-172">-p</span></span> | <span data-ttu-id="9da37-173">--proyecto</span><span class="sxs-lookup"><span data-stu-id="9da37-173">--project</span></span> | <span data-ttu-id="9da37-174">Ruta de acceso al archivo de proyecto en el que se va a operar.</span><span class="sxs-lookup"><span data-stu-id="9da37-174">The path to the project file to operate on.</span></span> <span data-ttu-id="9da37-175">Si no se especifica un archivo, el comando busca uno en el directorio actual.</span><span class="sxs-lookup"><span data-stu-id="9da37-175">If a file is not specified, the command searches the current directory for one.</span></span>
| <span data-ttu-id="9da37-176">-s</span><span class="sxs-lookup"><span data-stu-id="9da37-176">-s</span></span> | <span data-ttu-id="9da37-177">--servicios</span><span class="sxs-lookup"><span data-stu-id="9da37-177">--services</span></span> | <span data-ttu-id="9da37-178">El tipo de servicios de gRPC que deben generarse.</span><span class="sxs-lookup"><span data-stu-id="9da37-178">The type of gRPC services that should be generated.</span></span> <span data-ttu-id="9da37-179">Si se especifica `Default`, se usa `Both` para los proyectos web y `Client` para los proyectos que no son de Web.</span><span class="sxs-lookup"><span data-stu-id="9da37-179">If `Default` is specified, `Both` is used for Web projects and `Client` is used for non-Web projects.</span></span> <span data-ttu-id="9da37-180">Los valores aceptados son `Both`, `Client`, `Default`, `None`, `Server`.</span><span class="sxs-lookup"><span data-stu-id="9da37-180">Accepted values are `Both`, `Client`, `Default`, `None`, `Server`.</span></span>
| <span data-ttu-id="9da37-181">-i</span><span class="sxs-lookup"><span data-stu-id="9da37-181">-i</span></span> | <span data-ttu-id="9da37-182">--Additional-Import-dirs</span><span class="sxs-lookup"><span data-stu-id="9da37-182">--additional-import-dirs</span></span> | <span data-ttu-id="9da37-183">Directorios adicionales que se usarán al resolver importaciones para los archivos protobuf.</span><span class="sxs-lookup"><span data-stu-id="9da37-183">Additional directories to be used when resolving imports for the protobuf files.</span></span> <span data-ttu-id="9da37-184">Se trata de una lista de rutas de acceso separadas por punto y coma.</span><span class="sxs-lookup"><span data-stu-id="9da37-184">This is a semicolon separated list of paths.</span></span>
| | <span data-ttu-id="9da37-185">--acceso</span><span class="sxs-lookup"><span data-stu-id="9da37-185">--access</span></span> | <span data-ttu-id="9da37-186">Modificador de acceso que se va a usar C# para las clases generadas.</span><span class="sxs-lookup"><span data-stu-id="9da37-186">The access modifier to use for the generated C# classes.</span></span> <span data-ttu-id="9da37-187">El valor predeterminado es `Public`.</span><span class="sxs-lookup"><span data-stu-id="9da37-187">Default value is `Public`.</span></span> <span data-ttu-id="9da37-188">Los valores aceptados son `Internal` y `Public`.</span><span class="sxs-lookup"><span data-stu-id="9da37-188">Accepted values are `Internal` and `Public`.</span></span>

## <a name="remove"></a><span data-ttu-id="9da37-189">Quitar</span><span class="sxs-lookup"><span data-stu-id="9da37-189">Remove</span></span>

<span data-ttu-id="9da37-190">El comando `remove` se usa para quitar las referencias de protobuf del archivo *. csproj* .</span><span class="sxs-lookup"><span data-stu-id="9da37-190">The `remove` command is used to remove Protobuf references from the *.csproj* file.</span></span> <span data-ttu-id="9da37-191">El comando acepta argumentos de ruta de acceso y direcciones URL de origen como argumentos.</span><span class="sxs-lookup"><span data-stu-id="9da37-191">The command accepts path arguments and source URLs as arguments.</span></span> <span data-ttu-id="9da37-192">La herramienta:</span><span class="sxs-lookup"><span data-stu-id="9da37-192">The tool:</span></span>

* <span data-ttu-id="9da37-193">Solo quita la referencia de protobuf.</span><span class="sxs-lookup"><span data-stu-id="9da37-193">Only removes the Protobuf reference.</span></span>
* <span data-ttu-id="9da37-194">No elimina el archivo *. proto* , incluso si se descargó originalmente de una dirección URL remota.</span><span class="sxs-lookup"><span data-stu-id="9da37-194">Does not delete the *.proto* file, even if it was originally downloaded from a remote URL.</span></span>

### <a name="usage"></a><span data-ttu-id="9da37-195">Uso</span><span class="sxs-lookup"><span data-stu-id="9da37-195">Usage</span></span>

```dotnetcli
dotnet-grpc remove [options] <references>...
```

### <a name="arguments"></a><span data-ttu-id="9da37-196">Argumentos</span><span class="sxs-lookup"><span data-stu-id="9da37-196">Arguments</span></span>

| <span data-ttu-id="9da37-197">Argumento</span><span class="sxs-lookup"><span data-stu-id="9da37-197">Argument</span></span> | <span data-ttu-id="9da37-198">Descripción</span><span class="sxs-lookup"><span data-stu-id="9da37-198">Description</span></span> |
|-|-|
| <span data-ttu-id="9da37-199">referencias</span><span class="sxs-lookup"><span data-stu-id="9da37-199">references</span></span> | <span data-ttu-id="9da37-200">Direcciones URL o rutas de acceso de archivo de las referencias de protobuf que se van a quitar.</span><span class="sxs-lookup"><span data-stu-id="9da37-200">The URLs or file paths of the protobuf references to remove.</span></span> |

### <a name="options"></a><span data-ttu-id="9da37-201">Opciones</span><span class="sxs-lookup"><span data-stu-id="9da37-201">Options</span></span>

| <span data-ttu-id="9da37-202">Opción corta</span><span class="sxs-lookup"><span data-stu-id="9da37-202">Short option</span></span> | <span data-ttu-id="9da37-203">Opción larga</span><span class="sxs-lookup"><span data-stu-id="9da37-203">Long option</span></span> | <span data-ttu-id="9da37-204">Descripción</span><span class="sxs-lookup"><span data-stu-id="9da37-204">Description</span></span> |
|-|-|-|
| <span data-ttu-id="9da37-205">-p</span><span class="sxs-lookup"><span data-stu-id="9da37-205">-p</span></span> | <span data-ttu-id="9da37-206">--proyecto</span><span class="sxs-lookup"><span data-stu-id="9da37-206">--project</span></span> | <span data-ttu-id="9da37-207">Ruta de acceso al archivo de proyecto en el que se va a operar.</span><span class="sxs-lookup"><span data-stu-id="9da37-207">The path to the project file to operate on.</span></span> <span data-ttu-id="9da37-208">Si no se especifica un archivo, el comando busca uno en el directorio actual.</span><span class="sxs-lookup"><span data-stu-id="9da37-208">If a file is not specified, the command searches the current directory for one.</span></span>

## <a name="refresh"></a><span data-ttu-id="9da37-209">Refresh</span><span class="sxs-lookup"><span data-stu-id="9da37-209">Refresh</span></span>

<span data-ttu-id="9da37-210">El comando `refresh` se usa para actualizar una referencia remota con el contenido más reciente de la dirección URL de origen.</span><span class="sxs-lookup"><span data-stu-id="9da37-210">The `refresh` command is used to update a remote reference with the latest content from the source URL.</span></span> <span data-ttu-id="9da37-211">Tanto la ruta de acceso del archivo de descarga como la dirección URL de origen se pueden usar para especificar la referencia que se va a actualizar.</span><span class="sxs-lookup"><span data-stu-id="9da37-211">Both the download file path and the source URL can be used to specify the reference to be updated.</span></span> <span data-ttu-id="9da37-212">Nota:</span><span class="sxs-lookup"><span data-stu-id="9da37-212">Note:</span></span>

* <span data-ttu-id="9da37-213">Los valores hash del contenido del archivo se comparan para determinar si se debe actualizar el archivo local.</span><span class="sxs-lookup"><span data-stu-id="9da37-213">The hashes of the file contents are compared to determine whether the local file should be updated.</span></span>
* <span data-ttu-id="9da37-214">No se compara ninguna información de marca de tiempo.</span><span class="sxs-lookup"><span data-stu-id="9da37-214">No timestamp information is compared.</span></span>

<span data-ttu-id="9da37-215">La herramienta siempre reemplaza el archivo local con el archivo remoto si se necesita una actualización.</span><span class="sxs-lookup"><span data-stu-id="9da37-215">The tool always replaces the local file with the remote file if an update is needed.</span></span>

### <a name="usage"></a><span data-ttu-id="9da37-216">Uso</span><span class="sxs-lookup"><span data-stu-id="9da37-216">Usage</span></span>

```dotnetcli
dotnet-grpc refresh [options] [<references>...]
```

### <a name="arguments"></a><span data-ttu-id="9da37-217">Argumentos</span><span class="sxs-lookup"><span data-stu-id="9da37-217">Arguments</span></span>

| <span data-ttu-id="9da37-218">Argumento</span><span class="sxs-lookup"><span data-stu-id="9da37-218">Argument</span></span> | <span data-ttu-id="9da37-219">Descripción</span><span class="sxs-lookup"><span data-stu-id="9da37-219">Description</span></span> |
|-|-|
| <span data-ttu-id="9da37-220">referencias</span><span class="sxs-lookup"><span data-stu-id="9da37-220">references</span></span> | <span data-ttu-id="9da37-221">Las direcciones URL o las rutas de acceso de archivo a referencias de protobuf remotas que deben actualizarse.</span><span class="sxs-lookup"><span data-stu-id="9da37-221">The URLs or file paths to remote protobuf references that should be updated.</span></span> <span data-ttu-id="9da37-222">Deje este argumento vacío para actualizar todas las referencias remotas.</span><span class="sxs-lookup"><span data-stu-id="9da37-222">Leave this argument empty to refresh all remote references.</span></span> |

### <a name="options"></a><span data-ttu-id="9da37-223">Opciones</span><span class="sxs-lookup"><span data-stu-id="9da37-223">Options</span></span>

| <span data-ttu-id="9da37-224">Opción corta</span><span class="sxs-lookup"><span data-stu-id="9da37-224">Short option</span></span> | <span data-ttu-id="9da37-225">Opción larga</span><span class="sxs-lookup"><span data-stu-id="9da37-225">Long option</span></span> | <span data-ttu-id="9da37-226">Descripción</span><span class="sxs-lookup"><span data-stu-id="9da37-226">Description</span></span> |
|-|-|-|
| <span data-ttu-id="9da37-227">-p</span><span class="sxs-lookup"><span data-stu-id="9da37-227">-p</span></span> | <span data-ttu-id="9da37-228">--proyecto</span><span class="sxs-lookup"><span data-stu-id="9da37-228">--project</span></span> | <span data-ttu-id="9da37-229">Ruta de acceso al archivo de proyecto en el que se va a operar.</span><span class="sxs-lookup"><span data-stu-id="9da37-229">The path to the project file to operate on.</span></span> <span data-ttu-id="9da37-230">Si no se especifica un archivo, el comando busca uno en el directorio actual.</span><span class="sxs-lookup"><span data-stu-id="9da37-230">If a file is not specified, the command searches the current directory for one.</span></span>
| | <span data-ttu-id="9da37-231">--ejecución seca</span><span class="sxs-lookup"><span data-stu-id="9da37-231">--dry-run</span></span> | <span data-ttu-id="9da37-232">Genera una lista de archivos que se actualizarían sin descargar ningún contenido nuevo.</span><span class="sxs-lookup"><span data-stu-id="9da37-232">Outputs a list of files that would be updated without downloading any new content.</span></span>

## <a name="list"></a><span data-ttu-id="9da37-233">Lista</span><span class="sxs-lookup"><span data-stu-id="9da37-233">List</span></span>

<span data-ttu-id="9da37-234">El comando `list` se usa para mostrar todas las referencias de protobuf en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="9da37-234">The `list` command is used to display all the Protobuf references in the project file.</span></span> <span data-ttu-id="9da37-235">Si todos los valores de una columna son valores predeterminados, se puede omitir la columna.</span><span class="sxs-lookup"><span data-stu-id="9da37-235">If all values of a column are default values, the column may be omitted.</span></span>

### <a name="usage"></a><span data-ttu-id="9da37-236">Uso</span><span class="sxs-lookup"><span data-stu-id="9da37-236">Usage</span></span>

```dotnetcli
dotnet-grpc list [options]
```

### <a name="options"></a><span data-ttu-id="9da37-237">Opciones</span><span class="sxs-lookup"><span data-stu-id="9da37-237">Options</span></span>

| <span data-ttu-id="9da37-238">Opción corta</span><span class="sxs-lookup"><span data-stu-id="9da37-238">Short option</span></span> | <span data-ttu-id="9da37-239">Opción larga</span><span class="sxs-lookup"><span data-stu-id="9da37-239">Long option</span></span> | <span data-ttu-id="9da37-240">Descripción</span><span class="sxs-lookup"><span data-stu-id="9da37-240">Description</span></span> |
|-|-|-|
| <span data-ttu-id="9da37-241">-p</span><span class="sxs-lookup"><span data-stu-id="9da37-241">-p</span></span> | <span data-ttu-id="9da37-242">--proyecto</span><span class="sxs-lookup"><span data-stu-id="9da37-242">--project</span></span> | <span data-ttu-id="9da37-243">Ruta de acceso al archivo de proyecto en el que se va a operar.</span><span class="sxs-lookup"><span data-stu-id="9da37-243">The path to the project file to operate on.</span></span> <span data-ttu-id="9da37-244">Si no se especifica un archivo, el comando busca uno en el directorio actual.</span><span class="sxs-lookup"><span data-stu-id="9da37-244">If a file is not specified, the command searches the current directory for one.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9da37-245">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="9da37-245">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
