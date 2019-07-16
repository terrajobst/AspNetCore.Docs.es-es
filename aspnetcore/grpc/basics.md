---
title: Servicios gRPC con C#
author: juntaoluo
description: Aprenda los conceptos básicos al escribir servicios gRPC con C#.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/31/2019
uid: grpc/basics
ms.openlocfilehash: 78d744d641396c449a142375c69730333f8183cd
ms.sourcegitcommit: 1bf80f4acd62151ff8cce517f03f6fa891136409
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/15/2019
ms.locfileid: "68223884"
---
# <a name="grpc-services-with-c"></a>gRPC servicios con c#\#

Este documento describen los conceptos necesarios para escribir [gRPC](https://grpc.io/docs/guides/) aplicaciones en C#. Los temas tratados aquí se aplican a ambos [C-core](https://grpc.io/blog/grpc-stacks)-aplicaciones basadas en ASP.NET Core y basada en gRPC.

## <a name="proto-file"></a>archivo proto

gRPC usa un enfoque de contrato primero al desarrollo de API. Los búferes de protocolo (protobuf) se usan como el lenguaje de diseño de interfaz (IDL) de forma predeterminada. El *.proto* contiene el archivo:

* La definición del servicio gRPC.
* Los mensajes enviados entre clientes y servidores.

Para obtener más información sobre la sintaxis de los archivos protobuf, consulte el [documentación oficial (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).

Por ejemplo, considere la *greet.proto* archivo usado en [Introducción gRPC service](xref:tutorials/grpc/grpc-start):

* Define un `Greeter` service.
* El `Greeter` servicio define un `SayHello` llamar.
* `SayHello` envía un `HelloRequest` del mensaje y recibe un `HelloReply` mensaje:

[!code-protobuf[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/Protos/greet.proto)]

## <a name="add-a-proto-file-to-a-c-app"></a>Agregar un archivo .proto a una C\# app

El *.proto* archivo está incluido en un proyecto agregándolo a la `<Protobuf>` grupo de elementos:

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

## <a name="c-tooling-support-for-proto-files"></a>C#Compatibilidad con herramientas de .proto archivos

El paquete de herramientas [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) es necesaria para generar el C# activos desde *.proto* archivos. Los recursos generados (archivos):

* Se generan según sea necesario cada vez que se compila el proyecto.
* No se agrega al proyecto o proteger en el control de código fuente.
* Son un artefacto de compilación contenido en el *obj* directory.

Este paquete es necesario para los proyectos de servidor y cliente. `Grpc.Tools` se pueden agregar mediante el Administrador de paquetes en Visual Studio o agregando un `<PackageReference>` al archivo de proyecto:

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=1&range=15)]

El paquete de herramientas no es necesario en el runtime, de modo que la dependencia se marca con `PrivateAssets="All"`.

## <a name="generated-c-assets"></a>Genera C# activos

El paquete de herramientas genera la C# tipos que representan los mensajes definidos en el que se incluyen *.proto* archivos.

Para los recursos de servidor, se genera un tipo base abstracto de servicio. El tipo base contiene las definiciones de todas las llamadas gRPC contenidas en el *.proto* archivo. Crear una implementación de servicio concreta que se deriva este tipo base e implementa la lógica para las llamadas gRPC. Para el `greet.proto`, en el ejemplo se ha descrito anteriormente, abstracta `GreeterBase` tipo que contiene un virtual `SayHello` se genera el método. Una implementación concreta `GreeterService` invalida el método e implementa la lógica que administra la llamada gRPC.

[!code-csharp[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

Para los recursos del lado cliente, se genera un tipo concreto de cliente. Llama la gRPC el *.proto* archivo se traducen a métodos en el tipo concreto, que se puede llamar. Para el `greet.proto`, en el ejemplo se ha descrito anteriormente, un hormigón `GreeterClient` tipo se genera. Llame a `GreeterClient.SayHello` para iniciar una llamada gRPC al servidor.

[!code-csharp[](~/tutorials//grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?highlight=5-8&name=snippet)]

De forma predeterminada, los recursos de servidor y cliente se generan para cada *.proto* archivo incluido en el `<Protobuf>` grupo de elementos. Para asegurarse de que solo los recursos de servidor se generan en un proyecto de servidor, el `GrpcServices` atributo está establecido en `Server`.

[!code-xml[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

De forma similar, el atributo está establecido en `Client` en proyectos de cliente.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:grpc/index>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>
