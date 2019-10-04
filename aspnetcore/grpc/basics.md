---
title: Servicios gRPC con C#
author: juntaoluo
description: Conozca los conceptos básicos al escribir servicios de gRPC C#con.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 07/03/2019
uid: grpc/basics
ms.openlocfilehash: 8d99d036fd4b00fc4568e67ea5225dc006dea4b1
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2019
ms.locfileid: "71925189"
---
# <a name="grpc-services-with-c"></a>gRPC Services con C\#

En este documento se describen los conceptos necesarios para escribir aplicaciones de [gRPC](https://grpc.io/docs/guides/) en C#. Los temas que se tratan aquí se aplican a las aplicaciones gRPC basadas en [C-Core](https://grpc.io/blog/grpc-stacks)y en ASP.net Core.

## <a name="proto-file"></a>archivo proto

gRPC usa un enfoque de contrato primero para el desarrollo de API. Los búferes de protocolo (protobuf) se usan de forma predeterminada como el lenguaje de diseño de interfaz (IDL). El archivo *@no__t -1. proto* contiene:

* La definición del servicio gRPC.
* Los mensajes enviados entre clientes y servidores.

Para obtener más información sobre la sintaxis de los archivos protobuf, consulte la [documentación oficial (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).

Por ejemplo, considere el archivo *Greeter. proto* que se usa en la introducción [al servicio gRPC](xref:tutorials/grpc/grpc-start):

* Define un `Greeter` servicio.
* El `Greeter` servicio define una `SayHello` llamada a.
* `SayHello`envía un `HelloRequest` mensaje y recibe un `HelloReply` mensaje:

[!code-protobuf[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Protos/greet.proto)]

## <a name="add-a-proto-file-to-a-c-app"></a>Agregar un archivo. proto a una aplicación\# de C

El archivo *@no__t -1. proto* se incluye en un proyecto agregándolo al grupo de elementos `<Protobuf>`:

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

## <a name="c-tooling-support-for-proto-files"></a>Compatibilidad de herramientas de C# con archivos .proto

El paquete de herramientas [GRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/) es necesario para generar los C# recursos desde *@no__t archivos -3. proto* . Los recursos generados (archivos):

* Se generan según sea necesario cada vez que se compila el proyecto.
* No se agregan al proyecto ni se protegen en el control de código fuente.
* Son un artefacto de compilación incluido en el directorio *obj* .

Este paquete es necesario para los proyectos de servidor y de cliente. El `Grpc.AspNetCore` metapaquete incluye una referencia a `Grpc.Tools`. Los proyectos de servidor `Grpc.AspNetCore` pueden agregarse mediante el administrador de paquetes en Visual Studio `<PackageReference>` o agregando un al archivo de proyecto:

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=1&range=12)]

Los proyectos de cliente deben hacer referencia directamente a `Grpc.Tools` junto con los demás paquetes necesarios para usar el cliente de gRPC. El paquete de herramientas no es necesario en tiempo de ejecución, por lo que `PrivateAssets="All"`la dependencia se marca con:

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/GrpcGreeterClient.csproj?highlight=3&range=9-11)]

## <a name="generated-c-assets"></a>Activos C# generados

El paquete de herramientas genera los C# tipos que representan los mensajes definidos en los archivos *@no__t -2. proto* incluidos.

En el caso de los recursos del lado servidor, se genera un tipo base de servicio abstracto. El tipo base contiene las definiciones de todas las llamadas a gRPC contenidas en el archivo *. proto* . Cree una implementación de servicio concreta que derive de este tipo base e implemente la lógica para las llamadas a gRPC. En el ejemplo descrito anteriormente, se genera un tipo `GreeterBase` abstracto que contiene un método virtual `SayHello`. `greet.proto` Una implementación `GreeterService` concreta invalida el método e implementa la lógica que controla la llamada a gRPC.

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

En el caso de los recursos del lado cliente, se genera un tipo de cliente concreto. Las llamadas a gRPC en el archivo *. proto* se traducen en métodos en el tipo concreto, al que se puede llamar. En el ejemplo descrito anteriormente, se genera un tipo `GreeterClient`concreto. `greet.proto` Llame `GreeterClient.SayHelloAsync` a para iniciar una llamada de gRPC al servidor.

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet)]

De forma predeterminada, se generan recursos de servidor y cliente para cada @no__t el archivo *-1. proto* incluido en el grupo de elementos `<Protobuf>`. Para asegurarse de que solo se generan los recursos del servidor en un proyecto `GrpcServices` de servidor, el `Server`atributo se establece en.

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

Del mismo modo, el atributo se `Client` establece en en los proyectos de cliente.

[!INCLUDE[](~/includes/gRPCazure.md)]

## <a name="additional-resources"></a>Recursos adicionales

* <xref:grpc/index>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/client>
