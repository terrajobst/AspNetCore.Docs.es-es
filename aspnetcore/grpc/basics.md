---
title: Servicios gRPC con C#
author: juntaoluo
description: Conozca los conceptos básicos al escribir servicios de gRPC C#con.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 07/03/2019
uid: grpc/basics
ms.openlocfilehash: b236fe6914cf7b780a9d02398ec9c92660dc1063
ms.sourcegitcommit: 2719c70cd15a430479ab4007ff3e197fbf5dfee0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68862859"
---
# <a name="grpc-services-with-c"></a><span data-ttu-id="1bef5-103">gRPC Services con C\#</span><span class="sxs-lookup"><span data-stu-id="1bef5-103">gRPC services with C\#</span></span>

<span data-ttu-id="1bef5-104">En este documento se describen los conceptos necesarios para escribir aplicaciones de [gRPC](https://grpc.io/docs/guides/) en C#.</span><span class="sxs-lookup"><span data-stu-id="1bef5-104">This document outlines the concepts needed to write [gRPC](https://grpc.io/docs/guides/) apps in C#.</span></span> <span data-ttu-id="1bef5-105">Los temas que se tratan aquí se aplican a las aplicaciones gRPC basadas en [C-Core](https://grpc.io/blog/grpc-stacks)y en ASP.net Core.</span><span class="sxs-lookup"><span data-stu-id="1bef5-105">The topics covered here apply to both [C-core](https://grpc.io/blog/grpc-stacks)-based and ASP.NET Core-based gRPC apps.</span></span>

## <a name="proto-file"></a><span data-ttu-id="1bef5-106">archivo proto</span><span class="sxs-lookup"><span data-stu-id="1bef5-106">proto file</span></span>

<span data-ttu-id="1bef5-107">gRPC usa un enfoque de contrato primero al desarrollo de API.</span><span class="sxs-lookup"><span data-stu-id="1bef5-107">gRPC uses a contract-first approach to API development.</span></span> <span data-ttu-id="1bef5-108">Los búferes de protocolo (protobuf) se usan de forma predeterminada como el lenguaje de diseño de interfaz (IDL).</span><span class="sxs-lookup"><span data-stu-id="1bef5-108">Protocol buffers (protobuf) are used as the Interface Design Language (IDL) by default.</span></span> <span data-ttu-id="1bef5-109">El archivo *. proto* contiene:</span><span class="sxs-lookup"><span data-stu-id="1bef5-109">The *.proto* file contains:</span></span>

* <span data-ttu-id="1bef5-110">La definición del servicio gRPC.</span><span class="sxs-lookup"><span data-stu-id="1bef5-110">The definition of the gRPC service.</span></span>
* <span data-ttu-id="1bef5-111">Los mensajes enviados entre clientes y servidores.</span><span class="sxs-lookup"><span data-stu-id="1bef5-111">The messages sent between clients and servers.</span></span>

<span data-ttu-id="1bef5-112">Para obtener más información sobre la sintaxis de los archivos protobuf, consulte la [documentación oficial (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).</span><span class="sxs-lookup"><span data-stu-id="1bef5-112">For more information on the syntax of protobuf files, see the [official documentation (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).</span></span>

<span data-ttu-id="1bef5-113">Por ejemplo, considere el archivo *Greeter. proto* que se usa en la introducción [al servicio gRPC](xref:tutorials/grpc/grpc-start):</span><span class="sxs-lookup"><span data-stu-id="1bef5-113">For example, consider the *greet.proto* file used in [Get started with gRPC service](xref:tutorials/grpc/grpc-start):</span></span>

* <span data-ttu-id="1bef5-114">Define un `Greeter` servicio.</span><span class="sxs-lookup"><span data-stu-id="1bef5-114">Defines a `Greeter` service.</span></span>
* <span data-ttu-id="1bef5-115">El `Greeter` servicio define una `SayHello` llamada a.</span><span class="sxs-lookup"><span data-stu-id="1bef5-115">The `Greeter` service defines a `SayHello` call.</span></span>
* <span data-ttu-id="1bef5-116">`SayHello`envía un `HelloRequest` mensaje y recibe un `HelloReply` mensaje:</span><span class="sxs-lookup"><span data-stu-id="1bef5-116">`SayHello` sends a `HelloRequest` message and receives a `HelloReply` message:</span></span>

[!code-protobuf[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Protos/greet.proto)]

## <a name="add-a-proto-file-to-a-c-app"></a><span data-ttu-id="1bef5-117">Agregar un archivo. proto a una aplicación\# de C</span><span class="sxs-lookup"><span data-stu-id="1bef5-117">Add a .proto file to a C\# app</span></span>

<span data-ttu-id="1bef5-118">El archivo *. proto* se incluye en un proyecto agregándolo al `<Protobuf>` grupo de elementos:</span><span class="sxs-lookup"><span data-stu-id="1bef5-118">The *.proto* file is included in a project by adding it to the `<Protobuf>` item group:</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

## <a name="c-tooling-support-for-proto-files"></a><span data-ttu-id="1bef5-119">C#Compatibilidad de las herramientas con los archivos. proto</span><span class="sxs-lookup"><span data-stu-id="1bef5-119">C# Tooling support for .proto files</span></span>

<span data-ttu-id="1bef5-120">El paquete de herramientas [GRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/) es necesario para generar los C# recursos de los archivos *. proto* .</span><span class="sxs-lookup"><span data-stu-id="1bef5-120">The tooling package [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) is required to generate the C# assets from *.proto* files.</span></span> <span data-ttu-id="1bef5-121">Los recursos generados (archivos):</span><span class="sxs-lookup"><span data-stu-id="1bef5-121">The generated assets (files):</span></span>

* <span data-ttu-id="1bef5-122">Se generan según sea necesario cada vez que se compila el proyecto.</span><span class="sxs-lookup"><span data-stu-id="1bef5-122">Are generated on an as-needed basis each time the project is built.</span></span>
* <span data-ttu-id="1bef5-123">No se agregan al proyecto ni se protegen en el control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="1bef5-123">Aren't added to the project or checked into source control.</span></span>
* <span data-ttu-id="1bef5-124">Son un artefacto de compilación incluido en el directorio *obj* .</span><span class="sxs-lookup"><span data-stu-id="1bef5-124">Are a build artifact contained in the *obj* directory.</span></span>

<span data-ttu-id="1bef5-125">Este paquete es necesario para los proyectos de servidor y de cliente.</span><span class="sxs-lookup"><span data-stu-id="1bef5-125">This package is required by both the server and client projects.</span></span> <span data-ttu-id="1bef5-126">El `Grpc.AspNetCore` metapaquete incluye una referencia a `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="1bef5-126">The `Grpc.AspNetCore` metapackage includes a reference to `Grpc.Tools`.</span></span> <span data-ttu-id="1bef5-127">Los proyectos de servidor `Grpc.AspNetCore` pueden agregarse mediante el administrador de paquetes en Visual Studio `<PackageReference>` o agregando un al archivo de proyecto:</span><span class="sxs-lookup"><span data-stu-id="1bef5-127">Server projects can add `Grpc.AspNetCore` using the Package Manager in Visual Studio or by adding a `<PackageReference>` to the project file:</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=1&range=12)]

<span data-ttu-id="1bef5-128">Los proyectos de cliente deben `Grpc.Tools` hacer referencia directamente junto con los demás paquetes necesarios para usar el cliente de gRPC.</span><span class="sxs-lookup"><span data-stu-id="1bef5-128">Client projects should directly reference `Grpc.Tools` alongside the other packages required to use the gRPC client.</span></span> <span data-ttu-id="1bef5-129">El paquete de herramientas no es necesario en tiempo de ejecución, por lo que `PrivateAssets="All"`la dependencia se marca con:</span><span class="sxs-lookup"><span data-stu-id="1bef5-129">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`:</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/GrpcGreeterClient.csproj?highlight=3&range=9-11)]

## <a name="generated-c-assets"></a><span data-ttu-id="1bef5-130">Activos C# generados</span><span class="sxs-lookup"><span data-stu-id="1bef5-130">Generated C# assets</span></span>

<span data-ttu-id="1bef5-131">El paquete de herramientas genera los C# tipos que representan los mensajes definidos en los archivos *. proto* incluidos.</span><span class="sxs-lookup"><span data-stu-id="1bef5-131">The tooling package generates the C# types representing the messages defined in the included *.proto* files.</span></span>

<span data-ttu-id="1bef5-132">En el caso de los recursos del lado servidor, se genera un tipo base de servicio abstracto.</span><span class="sxs-lookup"><span data-stu-id="1bef5-132">For server-side assets, an abstract service base type is generated.</span></span> <span data-ttu-id="1bef5-133">El tipo base contiene las definiciones de todas las llamadas a gRPC contenidas en el archivo *. proto* .</span><span class="sxs-lookup"><span data-stu-id="1bef5-133">The base type contains the definitions of all the gRPC calls contained in the *.proto* file.</span></span> <span data-ttu-id="1bef5-134">Cree una implementación de servicio concreta que derive de este tipo base e implemente la lógica para las llamadas a gRPC.</span><span class="sxs-lookup"><span data-stu-id="1bef5-134">Create a concrete service implementation that derives from this base type and implements the logic for the gRPC calls.</span></span> <span data-ttu-id="1bef5-135">En el ejemplo descrito anteriormente, se genera un tipo `GreeterBase` abstracto que contiene un método virtual `SayHello`. `greet.proto`</span><span class="sxs-lookup"><span data-stu-id="1bef5-135">For the `greet.proto`, the example described previously, an abstract `GreeterBase` type that contains a virtual `SayHello` method is generated.</span></span> <span data-ttu-id="1bef5-136">Una implementación `GreeterService` concreta invalida el método e implementa la lógica que controla la llamada a gRPC.</span><span class="sxs-lookup"><span data-stu-id="1bef5-136">A concrete implementation `GreeterService` overrides the method and implements the logic handling the gRPC call.</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

<span data-ttu-id="1bef5-137">En el caso de los recursos del lado cliente, se genera un tipo de cliente concreto.</span><span class="sxs-lookup"><span data-stu-id="1bef5-137">For client-side assets, a concrete client type is generated.</span></span> <span data-ttu-id="1bef5-138">Las llamadas a gRPC en el archivo *. proto* se traducen en métodos en el tipo concreto, al que se puede llamar.</span><span class="sxs-lookup"><span data-stu-id="1bef5-138">The gRPC calls in the *.proto* file are translated into methods on the concrete type, which can be called.</span></span> <span data-ttu-id="1bef5-139">En el ejemplo descrito anteriormente, se genera un tipo `GreeterClient`concreto. `greet.proto`</span><span class="sxs-lookup"><span data-stu-id="1bef5-139">For the `greet.proto`, the example described previously, a concrete `GreeterClient` type is generated.</span></span> <span data-ttu-id="1bef5-140">Llame `GreeterClient.SayHelloAsync` a para iniciar una llamada de gRPC al servidor.</span><span class="sxs-lookup"><span data-stu-id="1bef5-140">Call `GreeterClient.SayHelloAsync` to initiate a gRPC call to the server.</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?highlight=3-6&name=snippet)]

<span data-ttu-id="1bef5-141">De forma predeterminada, se generan recursos de servidor y cliente para cada archivo *. proto* incluido `<Protobuf>` en el grupo de elementos.</span><span class="sxs-lookup"><span data-stu-id="1bef5-141">By default, server and client assets are generated for each *.proto* file included in the `<Protobuf>` item group.</span></span> <span data-ttu-id="1bef5-142">Para asegurarse de que solo se generan los recursos del servidor en un proyecto `GrpcServices` de servidor, el `Server`atributo se establece en.</span><span class="sxs-lookup"><span data-stu-id="1bef5-142">To ensure only the server assets are generated in a server project, the `GrpcServices` attribute is set to `Server`.</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

<span data-ttu-id="1bef5-143">Del mismo modo, el atributo se `Client` establece en en los proyectos de cliente.</span><span class="sxs-lookup"><span data-stu-id="1bef5-143">Similarly, the attribute is set to `Client` in client projects.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1bef5-144">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="1bef5-144">Additional resources</span></span>

* <xref:grpc/index>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>
