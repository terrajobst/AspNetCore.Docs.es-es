---
title: Usar el protocolo MessagePack Hub en SignalR para ASP.NET Core
author: bradygaster
description: Agregue el protocolo MessagePack Hub al SignalRde ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 1b01357233a9b95a5da052d92e30232c94e78a78
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2020
ms.locfileid: "76727221"
---
# <a name="use-messagepack-hub-protocol-in-opno-locsignalr-for-aspnet-core"></a>Usar el protocolo MessagePack Hub en SignalR para ASP.NET Core

Por [Brennan Conroy](https://github.com/BrennanConroy)

En este artículo se [da por supuesto](xref:tutorials/signalr)que el lector está familiarizado con los temas descritos en introducción.

## <a name="what-is-messagepack"></a>¿Qué es MessagePack?

[MessagePack](https://msgpack.org/index.html) es un formato de serialización binaria rápido y compacto. Resulta útil cuando el rendimiento y el ancho de banda son un problema porque crea mensajes más pequeños en comparación con [JSON](https://www.json.org/). Dado que se trata de un formato binario, los mensajes no se pueden leer al mirar los seguimientos de red y los registros a menos que los bytes se pasen a través de un analizador de MessagePack. SignalR tiene compatibilidad integrada con el formato MessagePack y proporciona las API para que las use el cliente y el servidor.

## <a name="configure-messagepack-on-the-server"></a>Configuración de MessagePack en el servidor

Para habilitar el protocolo MessagePack Hub en el servidor, instale el paquete de `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` en la aplicación. En el archivo Startup.cs, agregue `AddMessagePackProtocol` a la llamada `AddSignalR` para habilitar la compatibilidad con MessagePack en el servidor.

> [!NOTE]
> JSON está habilitado de forma predeterminada. Agregar MessagePack habilita la compatibilidad con los clientes JSON y MessagePack.

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

Para personalizar la forma en que MessagePack aplicará formato a los datos, `AddMessagePackProtocol` toma un delegado para configurar las opciones. En ese delegado, la propiedad `FormatterResolvers` se puede usar para configurar las opciones de serialización de MessagePack. Para obtener más información sobre cómo funcionan los solucionadores, visite la biblioteca MessagePack en [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp). Los atributos se pueden usar en los objetos que se van a serializar para definir cómo deben administrarse.

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        options.FormatterResolvers = new List<MessagePack.IFormatterResolver>()
        {
            MessagePack.Resolvers.StandardResolver.Instance
        };
    });
```

## <a name="configure-messagepack-on-the-client"></a>Configuración de MessagePack en el cliente

> [!NOTE]
> JSON está habilitado de forma predeterminada para los clientes compatibles. Los clientes solo pueden admitir un único protocolo. Al agregar compatibilidad con MessagePack se reemplazarán todos los protocolos configurados previamente.

### <a name="net-client"></a>Cliente .NET

Para habilitar MessagePack en el cliente de .NET, instale el paquete de `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` y llame a `AddMessagePackProtocol` en `HubConnectionBuilder`.

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> Esta llamada de `AddMessagePackProtocol` toma un delegado para configurar opciones como el servidor.

### <a name="javascript-client"></a>Cliente de JavaScript

::: moniker range=">= aspnetcore-3.0"

El paquete de `@microsoft/signalr-protocol-msgpack` NPM proporciona compatibilidad con MessagePack para el cliente JavaScript. Instale el paquete ejecutando el siguiente comando en un shell de comandos:

```console
npm install @microsoft/signalr-protocol-msgpack
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

El paquete de `@aspnet/signalr-protocol-msgpack` NPM proporciona compatibilidad con MessagePack para el cliente JavaScript. Instale el paquete ejecutando el siguiente comando en un shell de comandos:

```console
npm install @aspnet/signalr-protocol-msgpack
```

::: moniker-end

Después de instalar el paquete NPM, el módulo se puede usar directamente a través de un cargador de módulos de JavaScript o importarse en el explorador haciendo referencia al archivo siguiente:

::: moniker range=">= aspnetcore-3.0"

*node_modules\\@microsoft\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* 

::: moniker-end

::: moniker range="< aspnetcore-3.0"

*node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* 

::: moniker-end

En un explorador, también se debe hacer referencia a la biblioteca `msgpack5`. Use una etiqueta de `<script>` para crear una referencia. La biblioteca se puede encontrar en *node_modules \msgpack5\dist\msgpack5.js*.

> [!NOTE]
> Al utilizar el elemento `<script>`, el orden es importante. Si se hace referencia a *signalr-Protocol-msgpack. js* antes de *msgpack5. js*, se produce un error al intentar conectarse con MessagePack. *signalr. js* también es necesario antes de *signalr-Protocol-msgpack. js*.

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

Al agregar `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` al `HubConnectionBuilder`, se configurará el cliente para que use el protocolo MessagePack al conectarse a un servidor.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> En este momento, no hay ninguna opción de configuración para el protocolo MessagePack en el cliente de JavaScript.

## <a name="messagepack-quirks"></a>Peculiaridades de MessagePack

Hay algunos problemas que se deben tener en cuenta al usar el protocolo MessagePack Hub.

### <a name="messagepack-is-case-sensitive"></a>MessagePack distingue entre mayúsculas y minúsculas

El protocolo MessagePack distingue mayúsculas de minúsculas. Por ejemplo, considere la siguiente C# clase:

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

Al enviar desde el cliente JavaScript, debe usar `PascalCased` nombres de propiedad, ya que las mayúsculas y C# minúsculas deben coincidir exactamente con la clase. Por ejemplo:

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

El uso de nombres de `camelCased` no se C# enlaza correctamente a la clase. Puede solucionar este paso mediante el atributo `Key` para especificar un nombre diferente para la propiedad MessagePack. Para obtener más información, consulte [la documentación de MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp#object-serialization).

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a>DateTime. Kind no se conserva al serializar o deserializar

El protocolo MessagePack no proporciona una manera de codificar el valor `Kind` de un `DateTime`. Como resultado, al deserializar una fecha, el protocolo MessagePack Hub supone que la fecha de entrada está en formato UTC. Si está trabajando con `DateTime` valores en la hora local, se recomienda convertir a UTC antes de enviarlos. Conviértalos de la hora UTC a la hora local cuando los reciba.

Para obtener más información sobre esta limitación, vea el tema sobre el problema de GitHub [ASPNET/SignalR#2632](https://github.com/aspnet/SignalR/issues/2632).

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a>No se admite DateTime. MinValue en MessagePack en JavaScript

La biblioteca [msgpack5](https://github.com/mcollina/msgpack5) que usa el cliente SignalR JavaScript no admite el tipo de `timestamp96` en MessagePack. Este tipo se usa para codificar valores de fecha muy grandes (ya sea muy temprano en el pasado o muy lejos en el futuro). El valor de `DateTime.MinValue` es `January 1, 0001` que se debe codificar en un valor de `timestamp96`. Por este motivo, no se admite el envío de `DateTime.MinValue` a un cliente de JavaScript. Cuando el cliente JavaScript recibe `DateTime.MinValue`, se produce el siguiente error:

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

Normalmente, `DateTime.MinValue` se usa para codificar un valor "Missing" o `null`. Si necesita codificar ese valor en MessagePack, use un valor de `DateTime` que acepte valores NULL (`DateTime?`) o codifique un valor de `bool` independiente que indique si la fecha está presente.

Para obtener más información sobre esta limitación, vea el tema sobre el problema de GitHub [ASPNET/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228).

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a>Compatibilidad con MessagePack en el entorno de compilación "anterior a la hora"

La biblioteca [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8) utilizada por el cliente y el servidor .net utiliza la generación de código para optimizar la serialización. Como resultado, no se admite de forma predeterminada en los entornos que usan la compilación "anterior a la hora" (como Xamarin iOS o Unity). Es posible usar MessagePack en estos entornos mediante la "generación previa" del código del serializador/deserializador. Para obtener más información, consulte [la documentación de MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8#pre-code-generationunityxamarin-supports). Una vez que haya generado los serializadores previamente, puede registrarlos mediante el delegado de configuración que se pasa a `AddMessagePackProtocol`:

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        options.FormatterResolvers = new List<MessagePack.IFormatterResolver>()
        {
            MessagePack.Resolvers.GeneratedResolver.Instance,
            MessagePack.Resolvers.StandardResolver.Instance
        };
    });
```

### <a name="type-checks-are-more-strict-in-messagepack"></a>Las comprobaciones de tipo son más estrictas en MessagePack

El protocolo del concentrador de JSON realizará las conversiones de tipos durante la deserialización. Por ejemplo, si el objeto entrante tiene un valor de propiedad que es un número (`{ foo: 42 }`) pero la propiedad de la clase .NET es de tipo `string`, se convertirá el valor. Sin embargo, MessagePack no realiza esta conversión y producirá una excepción que se puede observar en los registros del lado servidor (y en la consola):

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

Para obtener más información sobre esta limitación, vea el tema sobre el problema de GitHub [ASPNET/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937).

## <a name="related-resources"></a>Recursos relacionados

* [Primeros pasos](xref:tutorials/signalr)
* [Cliente .NET](xref:signalr/dotnet-client)
* [Cliente de JavaScript](xref:signalr/javascript-client)
