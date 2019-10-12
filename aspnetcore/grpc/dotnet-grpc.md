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
# <a name="manage-protobuf-references-with-dotnet-grpc"></a>Administración de referencias de protobuf con dotnet-GRPC

Por [John Luo](https://github.com/juntaoluo)

`dotnet-grpc` es una herramienta global de .NET Core para administrar las referencias de protobuf dentro de un proyecto de gRPC de .NET. La herramienta se puede usar para agregar, actualizar, quitar y enumerar las referencias de protobuf.

## <a name="installation"></a>Instalación

Para instalar la [herramienta global de .net Core](/dotnet/core/tools/global-tools)`dotnet-grpc`, ejecute el siguiente comando:

```dotnetcli
dotnet tool install -g dotnet-grpc
```

## <a name="add-references"></a>Agregar referencias

`dotnet-grpc` se puede usar para agregar referencias de protobuf como elementos `<Protobuf />` al archivo *. csproj* :

```xml
<Protobuf Include="..\Proto\count.proto" GrpcServices="Server" Link="Protos\count.proto" />
```

Las referencias de protobuf se usan para generar C# los recursos de cliente o servidor. El `dotnet-grpc`tool puede:

* Cree una referencia de protobuf a partir de archivos locales en el disco.
* Cree una referencia de protobuf desde un archivo remoto especificado por una dirección URL.
* Asegúrese de que se han agregado al proyecto las dependencias de paquete gRPC correctas.

Por ejemplo, el paquete `Grpc.AspNetCore` se agrega a una aplicación Web. `Grpc.AspNetCore` contiene bibliotecas de cliente y de servidor gRPC y compatibilidad con herramientas. Como alternativa, los paquetes `Grpc.Net.Client`, `Grpc.Tools` y `Google.Protobuf`, que contienen solo las bibliotecas de cliente de gRPC y la compatibilidad con herramientas, se agregan a una aplicación de consola.

### <a name="add-file"></a>Agregar archivo

El comando `add-file` se usa para agregar archivos locales en el disco como referencias de protobuf. Las rutas de acceso de archivo proporcionadas:

* Puede ser relativa al directorio actual o a las rutas de acceso absolutas.
* Puede contener caracteres comodín para [comodines](https://wikipedia.org/wiki/Glob_(programming))de archivos basados en patrones.

Si algún archivo está fuera del directorio del proyecto, se agrega un elemento `Link` para mostrar el archivo en la carpeta `Protos` en Visual Studio.

### <a name="usage"></a>Uso

```dotnetcli
dotnet grpc add-file [options] <files>...
```

#### <a name="arguments"></a>Argumentos

| Argumento | Descripción |
|-|-|
| archivos | El archivo protobuf hace referencia a. Puede tratarse de una ruta de acceso a Glob para los archivos protobuf locales. |

#### <a name="options"></a>Opciones

| Opción corta | Opción larga | Descripción |
|-|-|-|
| -p | --proyecto | Ruta de acceso al archivo de proyecto en el que se va a operar. Si no se especifica un archivo, el comando busca uno en el directorio actual.
| -s | --servicios | El tipo de servicios de gRPC que deben generarse. Si se especifica `Default`, se usa `Both` para los proyectos web y `Client` para los proyectos que no son de Web. Los valores aceptados son `Both`, `Client`, `Default`, `None`, `Server`.
| -i | --Additional-Import-dirs | Directorios adicionales que se usarán al resolver importaciones para los archivos protobuf. Se trata de una lista de rutas de acceso separadas por punto y coma.
| | --acceso | Modificador de acceso que se va a usar C# para las clases generadas. El valor predeterminado es `Public`. Los valores aceptados son `Internal` y `Public`.

### <a name="add-url"></a>Agregar dirección URL

El comando `add-url` se usa para agregar un archivo remoto especificado por una dirección URL de origen como referencia de protobuf. Se debe proporcionar una ruta de acceso de archivo para especificar dónde descargar el archivo remoto. La ruta de acceso del archivo puede ser relativa al directorio actual o a una ruta de acceso absoluta. Si la ruta de acceso del archivo está fuera del directorio del proyecto, se agrega un elemento `Link` para mostrar el archivo en la carpeta virtual `Protos` en Visual Studio.

### <a name="usage"></a>Uso

```dotnetcli
dotnet-grpc add-url [options] <url>
```

#### <a name="arguments"></a>Argumentos

| Argumento | Descripción |
|-|-|
| url | La dirección URL de un archivo protobuf remoto. |

#### <a name="options"></a>Opciones

| Opción corta | Opción larga | Descripción |
|-|-|-|
| -o | --salida | Especifica la ruta de acceso de descarga para el archivo protobuf remoto. Esta es una opción necesaria.
| -p | --proyecto | Ruta de acceso al archivo de proyecto en el que se va a operar. Si no se especifica un archivo, el comando busca uno en el directorio actual.
| -s | --servicios | El tipo de servicios de gRPC que deben generarse. Si se especifica `Default`, se usa `Both` para los proyectos web y `Client` para los proyectos que no son de Web. Los valores aceptados son `Both`, `Client`, `Default`, `None`, `Server`.
| -i | --Additional-Import-dirs | Directorios adicionales que se usarán al resolver importaciones para los archivos protobuf. Se trata de una lista de rutas de acceso separadas por punto y coma.
| | --acceso | Modificador de acceso que se va a usar C# para las clases generadas. El valor predeterminado es `Public`. Los valores aceptados son `Internal` y `Public`.

## <a name="remove"></a>Quitar

El comando `remove` se usa para quitar las referencias de protobuf del archivo *. csproj* . El comando acepta argumentos de ruta de acceso y direcciones URL de origen como argumentos. La herramienta:

* Solo quita la referencia de protobuf.
* No elimina el archivo *. proto* , incluso si se descargó originalmente de una dirección URL remota.

### <a name="usage"></a>Uso

```dotnetcli
dotnet-grpc remove [options] <references>...
```

### <a name="arguments"></a>Argumentos

| Argumento | Descripción |
|-|-|
| referencias | Direcciones URL o rutas de acceso de archivo de las referencias de protobuf que se van a quitar. |

### <a name="options"></a>Opciones

| Opción corta | Opción larga | Descripción |
|-|-|-|
| -p | --proyecto | Ruta de acceso al archivo de proyecto en el que se va a operar. Si no se especifica un archivo, el comando busca uno en el directorio actual.

## <a name="refresh"></a>Refresh

El comando `refresh` se usa para actualizar una referencia remota con el contenido más reciente de la dirección URL de origen. Tanto la ruta de acceso del archivo de descarga como la dirección URL de origen se pueden usar para especificar la referencia que se va a actualizar. Nota:

* Los valores hash del contenido del archivo se comparan para determinar si se debe actualizar el archivo local.
* No se compara ninguna información de marca de tiempo.

La herramienta siempre reemplaza el archivo local con el archivo remoto si se necesita una actualización.

### <a name="usage"></a>Uso

```dotnetcli
dotnet-grpc refresh [options] [<references>...]
```

### <a name="arguments"></a>Argumentos

| Argumento | Descripción |
|-|-|
| referencias | Las direcciones URL o las rutas de acceso de archivo a referencias de protobuf remotas que deben actualizarse. Deje este argumento vacío para actualizar todas las referencias remotas. |

### <a name="options"></a>Opciones

| Opción corta | Opción larga | Descripción |
|-|-|-|
| -p | --proyecto | Ruta de acceso al archivo de proyecto en el que se va a operar. Si no se especifica un archivo, el comando busca uno en el directorio actual.
| | --ejecución seca | Genera una lista de archivos que se actualizarían sin descargar ningún contenido nuevo.

## <a name="list"></a>Lista

El comando `list` se usa para mostrar todas las referencias de protobuf en el archivo de proyecto. Si todos los valores de una columna son valores predeterminados, se puede omitir la columna.

### <a name="usage"></a>Uso

```dotnetcli
dotnet-grpc list [options]
```

### <a name="options"></a>Opciones

| Opción corta | Opción larga | Descripción |
|-|-|-|
| -p | --proyecto | Ruta de acceso al archivo de proyecto en el que se va a operar. Si no se especifica un archivo, el comando busca uno en el directorio actual.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
