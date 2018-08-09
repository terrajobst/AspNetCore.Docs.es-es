---
title: Usar el protocolo de MessagePack concentrador de SignalR para ASP.NET Core
author: tdykstra
description: Agregar concentrador MessagePack protocolo a ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 78b708c50ce7a8101c9eaa558171540e61c0d7f0
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095000"
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

MessagePack compatibilidad con el cliente de Javascript proporciona el `@aspnet/signalr-protocol-msgpack` paquete NPM.

```console
npm install @aspnet/signalr-protocol-msgpack
```

Después de instalar el paquete de npm, el módulo se puede utilizar directamente a través de un cargador de módulos de JavaScript o importado en el explorador haciendo referencia a la *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* archivo. En un explorador el `msgpack5` también se debe hacer referencia a la biblioteca. Use un `<script>` etiqueta para crear una referencia. La biblioteca puede encontrarse en *node_modules\msgpack5\dist\msgpack5.js*.

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

## <a name="related-resources"></a>Recursos relacionados

* [Primeros pasos](xref:tutorials/signalr)
* [Cliente .NET](xref:signalr/dotnet-client)
* [Cliente de JavaScript](xref:signalr/javascript-client)
