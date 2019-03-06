---
title: Usar el protocolo de MessagePack concentrador de SignalR para ASP.NET Core
author: bradygaster
description: Agregar concentrador MessagePack protocolo a ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/27/2019
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 7742f6f8bb53fb3c299ff98ae52a0da519ff396c
ms.sourcegitcommit: 6ddd8a7675c1c1d997c8ab2d4498538e44954cac
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2019
ms.locfileid: "57400676"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a>Usar el protocolo de MessagePack concentrador de SignalR para ASP.NET Core

Por [Brennan Conroy](https://github.com/BrennanConroy)

En este artículo se da por supuesto que el lector está familiarizado con los temas tratados en [comenzar](xref:tutorials/signalr).

## <a name="what-is-messagepack"></a>¿Qué es MessagePack?

[MessagePack](https://msgpack.org/index.html) es un formato de serialización binaria es rápido y compacto. Es útil cuando constituyen un problema de rendimiento y ancho de banda porque crea mensajes más pequeños en comparación con [JSON](https://www.json.org/). Dado que es un formato binario, los mensajes son ilegibles al examinar los registros y seguimientos de red a menos que los bytes se pasan a través de un analizador MessagePack. Tiene compatibilidad integrada con el formato MessagePack SignalR y proporciona las API para el cliente y servidor usar.

## <a name="configure-messagepack-on-the-server"></a>Configurar MessagePack en el servidor

Para habilitar el protocolo de Hub MessagePack en el servidor, instale el `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` paquete en la aplicación. En el archivo Startup.cs agregue `AddMessagePackProtocol` a la `AddSignalR` llamada a habilitar la compatibilidad con MessagePack en el servidor.

> [!NOTE]
> JSON está habilitado de forma predeterminada. Agregar MessagePack habilita la compatibilidad con clientes MessagePack y JSON.

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

Para personalizar cómo MessagePack dará formato a los datos, `AddMessagePackProtocol` toma un delegado para configurar las opciones. En ese delegado, el `FormatterResolvers` propiedad puede usarse para configurar las opciones de serialización MessagePack. Para obtener más información sobre cómo funcionan las resoluciones, visite la biblioteca MessagePack en [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp). Atributos se pueden usar en los objetos que desea serializar para definir cómo debe controlarse.

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

## <a name="configure-messagepack-on-the-client"></a>Configurar MessagePack en el cliente

> [!NOTE]
> JSON está habilitada de forma predeterminada para los clientes compatibles. Los clientes pueden admitir solo un único protocolo. Agregar compatibilidad con MessagePack reemplazará cualquier previamente los protocolos configurados.

### <a name="net-client"></a>Cliente .NET

Para habilitar MessagePack en el cliente. NET, instale el `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` paquetes y llame a `AddMessagePackProtocol` en `HubConnectionBuilder`.

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> Esto `AddMessagePackProtocol` llamada toma un delegado para configurar las opciones al igual que el servidor.

### <a name="javascript-client"></a>Cliente de JavaScript

MessagePack compatibilidad con el cliente de JavaScript proporciona el `@aspnet/signalr-protocol-msgpack` paquete npm.

```console
npm install @aspnet/signalr-protocol-msgpack
```

Después de instalar el paquete de npm, el módulo se puede utilizar directamente a través de un cargador de módulos de JavaScript o importado en el explorador haciendo referencia a la *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* archivo. En un explorador, el `msgpack5` también se debe hacer referencia a la biblioteca. Use un `<script>` etiqueta para crear una referencia. La biblioteca puede encontrarse en *node_modules\msgpack5\dist\msgpack5.js*.

> [!NOTE]
> Cuando se usa el `<script>` elemento, el orden es importante. Si *signalr-protocol-msgpack.js* se hace referencia antes de *msgpack5.js*, se produce un error al intentar conectarse con MessagePack. *signalr.js* también es necesaria antes de *signalr-protocol-msgpack.js*.

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

Agregar `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` a la `HubConnectionBuilder` va a configurar el cliente para utilizar el protocolo MessagePack al conectarse a un servidor.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> En este momento, no hay ninguna opción de configuración para el protocolo MessagePack en el cliente de JavaScript.

## <a name="messagepack-quirks"></a>MessagePack peculiaridades

Hay algunos problemas que tenga en cuenta cuando se usa el protocolo de concentrador MessagePack.

### <a name="messagepack-is-case-sensitive"></a>MessagePack distingue mayúsculas de minúsculas

El protocolo MessagePack distingue mayúsculas de minúsculas. Por ejemplo, considere la siguiente C# clase:

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

Cuando se envían desde el cliente de JavaScript, debe usar `PascalCased` nombres de propiedad, ya que deben coincidir con las mayúsculas y minúsculas del C# clase exactamente. Por ejemplo:

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

Uso de `camelCased` nombres correctamente no se enlazará a la C# clase. Puede solucionar esto mediante el uso de la `Key` atributo para especificar un nombre diferente para la propiedad MessagePack. Para obtener más información, consulte [la documentación de MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp#object-serialization).

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a>DateTime.Kind no se conserva al serializar o deserializar

El protocolo MessagePack no proporciona una manera de codificar el `Kind` valor de un `DateTime`. Como resultado, al deserializar una fecha, el protocolo de Hub MessagePack supone que la fecha de entrada está en formato UTC. Si está trabajando con `DateTime` valores de hora local, se recomienda convertir a UTC antes de enviarlos. Convertirlos a la hora UTC a la hora local cuando los recibe.

Para obtener más información sobre esta limitación, vea GitHub problema [aspnet/SignalR #2632](https://github.com/aspnet/SignalR/issues/2632).

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a>DateTime.MinValue no es compatible con MessagePack en JavaScript

El [msgpack5](https://github.com/mcollina/msgpack5) biblioteca usada por el cliente de JavaScript de SignalR no es compatible con la `timestamp96` tipo en MessagePack. Este tipo se usa para codificar los valores de fecha muy grande (ya sea muy pronto en el pasado o en el futuro muy lejano). El valor de `DateTime.MinValue` es `January 1, 0001` que debe estar codificado en un `timestamp96` valor. Debido a esto, enviar `DateTime.MinValue` un JavaScript no se admite el cliente. Cuando `DateTime.MinValue` es recibido por el cliente de JavaScript, se produce el error siguiente:

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

Por lo general, `DateTime.MinValue` se usa para codificar un "Falta" o `null` valor. Si tiene que codificar ese valor en MessagePack, usar una que acepta valores NULL `DateTime` valor (`DateTime?`) o codificar un independiente `bool` valor que indica si la fecha está presente.

Para obtener más información sobre esta limitación, vea GitHub problema [aspnet/SignalR #2228](https://github.com/aspnet/SignalR/issues/2228).

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a>Compatibilidad con MessagePack en el entorno de compilación "ahead-of-time"

El [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp) biblioteca usada por el cliente de .NET y el servidor usa la generación de código para optimizar la serialización. Como resultado, no se admite de forma predeterminada en entornos que usan las compilación "ahead-of-time" (por ejemplo, Xamarin iOS o Unity). Es posible usar MessagePack en estos entornos "generando previamente" el código del serializador/deserializador. Para obtener más información, consulte [la documentación de MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp#pre-code-generationunityxamarin-supports). Una vez que los serializadores generados previamente, puede registrarlos mediante el delegado de configuración pasando a `AddMessagePackProtocol`:

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

### <a name="type-checks-are-more-strict-in-messagepack"></a>Comprobaciones de tipo sean más estrictas en MessagePack

El protocolo de Hub JSON llevará a cabo las conversiones de tipos durante la deserialización. Por ejemplo, si el objeto entrante tiene un valor de propiedad que es un número (`{ foo: 42 }`), pero la propiedad en la clase de .NET es de tipo `string`, se convertirá el valor. Sin embargo, MessagePack no realiza esta conversión y producirá una excepción que se puede ver en los registros del lado servidor (y en la consola):

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

Para obtener más información sobre esta limitación, vea GitHub problema [aspnet/SignalR #2937](https://github.com/aspnet/SignalR/issues/2937).

## <a name="related-resources"></a>Recursos relacionados

* [Primeros pasos](xref:tutorials/signalr)
* [Cliente .NET](xref:signalr/dotnet-client)
* [Cliente de JavaScript](xref:signalr/javascript-client)
