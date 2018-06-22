---
title: Utilice el protocolo de concentrador de MessagePack en SignalR para ASP.NET Core
author: rachelappel
description: Agregar MessagePack concentrador protocolo principal de ASP.NET signalr.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 702c77502868d6666cb2634b6959f029e036d14e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274994"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a>Utilice el protocolo de concentrador de MessagePack en SignalR para ASP.NET Core

Por [Brennan Conroy](https://github.com/BrennanConroy)

En este artículo se da por supuesto que el lector está familiarizado con los temas tratados en [Introducción](xref:tutorials/signalr).

## <a name="what-is-messagepack"></a>¿Qué es MessagePack?

[MessagePack](https://msgpack.org/index.html) es un formato de serialización binaria es rápido y compacto. Es útil cuando el rendimiento y el ancho de banda son un problema porque crea mensajes más pequeños en comparación con [JSON](https://www.json.org/). Dado que es un formato binario, mensajes no son legibles cuando se examinan los registros y seguimientos de red a menos que los bytes se pasan a través de un analizador de MessagePack. SignalR tiene compatibilidad integrada para el formato MessagePack y proporciona las API para el cliente y el servidor usar.

## <a name="configure-messagepack-on-the-server"></a>Configurar MessagePack en el servidor

Para habilitar el protocolo de concentrador MessagePack en el servidor, instale el `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` paquete de la aplicación. En el archivo Startup.cs agregue `AddMessagePackProtocol` a la `AddSignalR` llamada para habilitar la compatibilidad de MessagePack en el servidor.

> [!NOTE]
> JSON está habilitada de forma predeterminada. Agregar MessagePack habilita la compatibilidad para los clientes de JSON y MessagePack.

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

Para personalizar cómo MessagePack dará formato a los datos, `AddMessagePackProtocol` toma un delegado para configurar las opciones. En ese delegado, el `FormatterResolvers` propiedad puede utilizarse para configurar las opciones de serialización MessagePack. Para obtener más información sobre cómo funcionan los solucionadores, visite la biblioteca de MessagePack en [MessagePack CSharp](https://github.com/neuecc/MessagePack-CSharp). Los atributos se pueden utilizar en los objetos que desea serializar para definir cómo debe controlarse.

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

Para habilitar MessagePack en el cliente. NET, instale el `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` paquete y llame al método `AddMessagePackProtocol` en `HubConnectionBuilder`.

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> Esto `AddMessagePackProtocol` llamada toma un delegado para configurar opciones como el servidor.

### <a name="javascript-client"></a>Cliente de JavaScript

MessagePack para el cliente de Javascript se admiten por los `@aspnet/signalr-protocol-msgpack` paquete NPM.

```console
npm install @aspnet/signalr-protocol-msgpack
```

Después de instalar el paquete npm, el módulo se puede usar directamente mediante un cargador de módulos de JavaScript o importarse en el explorador haciendo referencia a la *node_modules\\ @aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*  archivo. En un explorador la `msgpack5` también se debe hacer referencia a la biblioteca. Use un `<script>` etiqueta que se va a crear una referencia. La biblioteca se puede encontrar en *node_modules\msgpack5\dist\msgpack5.js*.

> [!NOTE]
> Cuando se usa el `<script>` elemento, el orden es importante. Si *msgpack.js de protocolo de signalr* se hace referencia antes de *msgpack5.js*, se produce un error al intentar conectarse con MessagePack. *signalr.js* también es necesaria antes de *msgpack.js de protocolo de signalr*.

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
