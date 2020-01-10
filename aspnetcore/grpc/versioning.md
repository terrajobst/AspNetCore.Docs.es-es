---
title: Control de versiones de gRPC Services
author: jamesnk
description: Obtenga información sobre la versión de gRPC Services.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 01/09/2020
uid: grpc/versioning
ms.openlocfilehash: 9bd76009ba28a1abef25a98686afea6753d4a8f4
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828521"
---
# <a name="versioning-grpc-services"></a>Control de versiones de gRPC Services

Por [James Newton-King](https://twitter.com/jamesnk)

Las nuevas características agregadas a una aplicación pueden requerir que los servicios de gRPC que se proporcionen a los clientes cambien, en ocasiones, de maneras inesperadas y poco importantes. Cuando gRPC Services cambie:

* Se debe tener en cuenta la forma en que los cambios afectan a los clientes.
* Se debe implementar una estrategia de control de versiones para admitir cambios.

## <a name="backwards-compatibility"></a>Compatibilidad con versiones anteriores

El protocolo gRPC se ha diseñado para admitir servicios que cambian con el tiempo. Por lo general, las adiciones a los servicios y métodos de gRPC son ininterrumpidas. Los cambios no importantes permiten a los clientes existentes seguir trabajando sin realizar cambios. El cambio o la eliminación de los servicios de gRPC son cambios importantes. Cuando los servicios de gRPC tienen cambios importantes, los clientes que usan ese servicio deben actualizarse y volver a implementarse.

Realizar cambios no importantes en un servicio tiene una serie de ventajas:

* Los clientes existentes continúan ejecutándose.
* Evita el trabajo necesario para notificar a los clientes los cambios importantes y actualizarlos.
* Solo se debe documentar y mantener una versión del servicio.

### <a name="non-breaking-changes"></a>Cambios secundarios

Estos cambios no interrumpen el nivel de protocolo gRPC y el nivel binario de .NET.

* **Agregar un nuevo servicio**
* **Agregar un nuevo método a un servicio**
* Al **Agregar un campo a un mensaje de solicitud** , los campos agregados a un mensaje de solicitud se deserializan con el [valor predeterminado](https://developers.google.com/protocol-buffers/docs/proto3#default) en el servidor cuando no se establecen. Para ser un cambio no problemático, el servicio debe realizarse correctamente cuando los clientes más antiguos no establezcan el nuevo campo.
* **Agregar un campo a un mensaje de respuesta** : los campos que se agregan a un mensaje de respuesta se deserializan en la colección de [campos desconocidos](https://developers.google.com/protocol-buffers/docs/proto3#unknowns) del mensaje en el cliente.
* **Agregar un valor a una enumeración** : las enumeraciones se serializan como un valor numérico. Los nuevos valores de enumeración se deserializan en el cliente en el valor de enumeración sin un nombre de enumeración. Para ser un cambio no problemático, los clientes más antiguos deben ejecutarse correctamente al recibir el nuevo valor de enumeración.

### <a name="binary-breaking-changes"></a>Cambios de interrupción binarios

Los siguientes cambios no interrumpen el nivel del protocolo gRPC, pero es necesario actualizar el cliente si se actualiza al contrato *. proto* o al ensamblado .net de cliente más reciente. La compatibilidad binaria es importante si tiene previsto publicar una biblioteca de gRPC en NuGet.

* **Quitar un** campo: los valores de un campo quitado se deserializan en [los campos desconocidos](https://developers.google.com/protocol-buffers/docs/proto3#unknowns)de un mensaje. No se trata de un cambio importante del protocolo gRPC, pero el cliente debe actualizarse si se actualiza al contrato más reciente. Es importante que un número de campo quitado no se reutilice accidentalmente en el futuro. Para asegurarse de que esto no suceda, especifique los nombres y números de campo eliminados en el mensaje con la palabra clave [reservada](https://developers.google.com/protocol-buffers/docs/proto3#reserved) de protobuf.
* Cambiar el **nombre de un mensaje** : los nombres de mensajes no se envían normalmente en la red, por lo que no se trata de un cambio importante del protocolo gRPC. El cliente deberá actualizarse si se actualiza al contrato más reciente. Una situación en la que los nombres de los mensajes **se** envían en la red es con [cualquier](https://developers.google.com/protocol-buffers/docs/proto3#any) campo, cuando se utiliza el nombre del mensaje para identificar el tipo de mensaje.
* Cambiar el `csharp_namespace` de cambio de **csharp_namespace** cambiará el espacio de nombres de los tipos .net generados. No se trata de un cambio importante del protocolo gRPC, pero el cliente debe actualizarse si se actualiza al contrato más reciente.

### <a name="protocol-breaking-changes"></a>Cambios importantes del Protocolo

Los elementos siguientes son los cambios de protocolo y de interrupción binaria:

* **Cambiar el nombre de un campo** : con el contenido de protobuf, los nombres de campo solo se usan en el código generado. El número de campo se usa para identificar los campos de la red. Cambiar el nombre de un campo no es un cambio importante en el protocolo para protobuf. Sin embargo, si un servidor usa contenido JSON, al cambiar el nombre de un campo es un cambio importante.
* **Cambiar un** tipo de datos de campo: cambiar el tipo de datos de un campo a un [tipo incompatible](https://developers.google.com/protocol-buffers/docs/proto3#updating) producirá errores al deserializar el mensaje. Aunque el nuevo tipo de datos sea compatible, es probable que el cliente deba actualizarse para admitir el nuevo tipo si se actualiza al contrato más reciente.
* **Cambio de un número de campo** : con las cargas de protobuf, el número de campo se usa para identificar los campos de la red.
* Al cambiar el nombre de **un paquete, servicio o método** -gRPC, se usa el nombre del paquete, el nombre del servicio y el nombre del método para generar la dirección URL. El cliente obtiene un estado no *implementado* del servidor.
* **Quitar un servicio o método** : el cliente obtiene un estado no *implementado* del servidor al llamar al método quitado.

### <a name="behavior-breaking-changes"></a>Cambios importantes en el comportamiento

Al realizar cambios no importantes, también debe tener en cuenta si los clientes más antiguos pueden seguir trabajando con el nuevo comportamiento del servicio. Por ejemplo, al agregar un nuevo campo a un mensaje de solicitud:

* No es un cambio importante del protocolo.
* Al devolver un estado de error en el servidor, si el nuevo campo no está establecido, se convierte en un cambio importante para los clientes antiguos.

La compatibilidad con el comportamiento viene determinada por el código específico de la aplicación.

## <a name="version-number-services"></a>Servicios de número de versión

Los servicios deben procurarse que sigan siendo compatibles con los clientes antiguos. Es posible que los cambios realizados en la aplicación requieran cambios importantes. Interrumpir los clientes antiguos y forzar su actualización junto con el servicio no es una buena experiencia del usuario. Una manera de mantener la compatibilidad con versiones anteriores al realizar cambios importantes es publicar varias versiones de un servicio.

gRPC admite un especificador de [paquete](https://developers.google.com/protocol-buffers/docs/proto3#packages) opcional, que funciona de manera muy similar a un espacio de nombres de .net. De hecho, el `package` se usará como espacio de nombres .NET para los tipos .NET generados si `option csharp_namespace` no se establece en el archivo *. proto* . El paquete se puede usar para especificar un número de versión para el servicio y sus mensajes:

[!code-protobuf[](versioning/sample/greet.v1.proto?highlight=3)]

El nombre del paquete se combina con el nombre del servicio para identificar una dirección del servicio. Una dirección de servicio permite hospedar varias versiones de un servicio en paralelo:

* `greet.v1.Greeter`
* `greet.v2.Greeter`

Las implementaciones del servicio con control de versiones se registran en *Startup.CS*:

```csharp
app.UseEndpoints(endpoints =>
{
    // Implements greet.v1.Greeter
    endpoints.MapGrpcService<GreeterServiceV1>();

    // Implements greet.v2.Greeter
    endpoints.MapGrpcService<GreeterServiceV2>();
});
```

Incluir un número de versión en el nombre del paquete le ofrece la oportunidad de publicar una versión *V2* del servicio con cambios importantes, a la vez que continúa admitiendo clientes más antiguos que llaman a la versión *v1* . Una vez que los clientes se han actualizado para usar el servicio *V2* , puede optar por quitar la versión anterior. Al planear la publicación de varias versiones de un servicio:

* Evite los cambios importantes si es razonable.
* No actualice el número de versión a menos que realice cambios importantes.
* Actualice el número de versión cuando realice cambios importantes.

La publicación de varias versiones de un servicio la duplica. Para reducir la duplicación, considere la posibilidad de mover la lógica de negocios de las implementaciones de servicio a una ubicación centralizada que se pueda reutilizar en las implementaciones antiguas y nuevas:

[!code-csharp[](versioning/sample/GreeterServiceV1.cs?highlight=10,19)]

Los servicios y mensajes generados con distintos nombres de paquete son **tipos de .net diferentes**. Mover la lógica de negocios a una ubicación centralizada requiere la asignación de mensajes a tipos comunes.
