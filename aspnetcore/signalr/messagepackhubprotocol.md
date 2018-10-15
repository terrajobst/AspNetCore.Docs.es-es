---
title: Usar el protocolo de MessagePack concentrador de SignalR para ASP.NET Core
author: tdykstra
description: Agregar concentrador MessagePack protocolo a ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 0874afc5493eca5d43dfde30bb28aedc1f193744
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325586"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a><span data-ttu-id="f806d-103">Usar el protocolo de MessagePack concentrador de SignalR para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f806d-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="f806d-104">Por [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="f806d-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="f806d-105">En este artículo se da por supuesto que el lector está familiarizado con los temas tratados en [comenzar](xref:tutorials/signalr).</span><span class="sxs-lookup"><span data-stu-id="f806d-105">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="f806d-106">¿Qué es MessagePack?</span><span class="sxs-lookup"><span data-stu-id="f806d-106">What is MessagePack?</span></span>

<span data-ttu-id="f806d-107">[MessagePack](https://msgpack.org/index.html) es un formato de serialización binaria es rápido y compacto.</span><span class="sxs-lookup"><span data-stu-id="f806d-107">[MessagePack](https://msgpack.org/index.html) is a binary serialization format that is fast and compact.</span></span> <span data-ttu-id="f806d-108">Es útil cuando constituyen un problema de rendimiento y ancho de banda porque crea mensajes más pequeños en comparación con [JSON](https://www.json.org/).</span><span class="sxs-lookup"><span data-stu-id="f806d-108">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="f806d-109">Dado que es un formato binario, los mensajes son ilegibles al examinar los registros y seguimientos de red a menos que los bytes se pasan a través de un analizador MessagePack.</span><span class="sxs-lookup"><span data-stu-id="f806d-109">Because it's a binary format, messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> <span data-ttu-id="f806d-110">Tiene compatibilidad integrada con el formato MessagePack SignalR y proporciona las API para el cliente y servidor usar.</span><span class="sxs-lookup"><span data-stu-id="f806d-110">SignalR has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="f806d-111">Configurar MessagePack en el servidor</span><span class="sxs-lookup"><span data-stu-id="f806d-111">Configure MessagePack on the server</span></span>

<span data-ttu-id="f806d-112">Para habilitar el protocolo de Hub MessagePack en el servidor, instale el `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` paquete en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f806d-112">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="f806d-113">En el archivo Startup.cs agregue `AddMessagePackProtocol` a la `AddSignalR` llamada a habilitar la compatibilidad con MessagePack en el servidor.</span><span class="sxs-lookup"><span data-stu-id="f806d-113">In the Startup.cs file add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="f806d-114">JSON está habilitado de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="f806d-114">JSON is enabled by default.</span></span> <span data-ttu-id="f806d-115">Agregar MessagePack habilita la compatibilidad con clientes MessagePack y JSON.</span><span class="sxs-lookup"><span data-stu-id="f806d-115">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="f806d-116">Para personalizar cómo MessagePack dará formato a los datos, `AddMessagePackProtocol` toma un delegado para configurar las opciones.</span><span class="sxs-lookup"><span data-stu-id="f806d-116">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="f806d-117">En ese delegado, el `FormatterResolvers` propiedad puede usarse para configurar las opciones de serialización MessagePack.</span><span class="sxs-lookup"><span data-stu-id="f806d-117">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="f806d-118">Para obtener más información sobre cómo funcionan las resoluciones, visite la biblioteca MessagePack en [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span><span class="sxs-lookup"><span data-stu-id="f806d-118">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="f806d-119">Atributos se pueden usar en los objetos que desea serializar para definir cómo debe controlarse.</span><span class="sxs-lookup"><span data-stu-id="f806d-119">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

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

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="f806d-120">Configurar MessagePack en el cliente</span><span class="sxs-lookup"><span data-stu-id="f806d-120">Configure MessagePack on the client</span></span>

> [!NOTE]
> <span data-ttu-id="f806d-121">JSON está habilitada de forma predeterminada para los clientes compatibles.</span><span class="sxs-lookup"><span data-stu-id="f806d-121">JSON is enabled by default for the supported clients.</span></span> <span data-ttu-id="f806d-122">Los clientes pueden admitir solo un único protocolo.</span><span class="sxs-lookup"><span data-stu-id="f806d-122">Clients can only support a single protocol.</span></span> <span data-ttu-id="f806d-123">Agregar compatibilidad con MessagePack reemplazará cualquier previamente los protocolos configurados.</span><span class="sxs-lookup"><span data-stu-id="f806d-123">Adding MessagePack support will replace any previously configured protocols.</span></span>

### <a name="net-client"></a><span data-ttu-id="f806d-124">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="f806d-124">.NET client</span></span>

<span data-ttu-id="f806d-125">Para habilitar MessagePack en el cliente. NET, instale el `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` paquetes y llame a `AddMessagePackProtocol` en `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f806d-125">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="f806d-126">Esto `AddMessagePackProtocol` llamada toma un delegado para configurar las opciones al igual que el servidor.</span><span class="sxs-lookup"><span data-stu-id="f806d-126">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="f806d-127">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="f806d-127">JavaScript client</span></span>

<span data-ttu-id="f806d-128">MessagePack compatibilidad con el cliente de Javascript proporciona el `@aspnet/signalr-protocol-msgpack` paquete NPM.</span><span class="sxs-lookup"><span data-stu-id="f806d-128">MessagePack support for the Javascript client is provided by the `@aspnet/signalr-protocol-msgpack` NPM package.</span></span>

```console
npm install @aspnet/signalr-protocol-msgpack
```

<span data-ttu-id="f806d-129">Después de instalar el paquete de npm, el módulo se puede utilizar directamente a través de un cargador de módulos de JavaScript o importado en el explorador haciendo referencia a la *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* archivo.</span><span class="sxs-lookup"><span data-stu-id="f806d-129">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* file.</span></span> <span data-ttu-id="f806d-130">En un explorador, el `msgpack5` también se debe hacer referencia a la biblioteca.</span><span class="sxs-lookup"><span data-stu-id="f806d-130">In a browser, the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="f806d-131">Use un `<script>` etiqueta para crear una referencia.</span><span class="sxs-lookup"><span data-stu-id="f806d-131">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="f806d-132">La biblioteca puede encontrarse en *node_modules\msgpack5\dist\msgpack5.js*.</span><span class="sxs-lookup"><span data-stu-id="f806d-132">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="f806d-133">Cuando se usa el `<script>` elemento, el orden es importante.</span><span class="sxs-lookup"><span data-stu-id="f806d-133">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="f806d-134">Si *signalr-protocol-msgpack.js* se hace referencia antes de *msgpack5.js*, se produce un error al intentar conectarse con MessagePack.</span><span class="sxs-lookup"><span data-stu-id="f806d-134">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="f806d-135">*signalr.js* también es necesaria antes de *signalr-protocol-msgpack.js*.</span><span class="sxs-lookup"><span data-stu-id="f806d-135">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="f806d-136">Agregar `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` a la `HubConnectionBuilder` va a configurar el cliente para utilizar el protocolo MessagePack al conectarse a un servidor.</span><span class="sxs-lookup"><span data-stu-id="f806d-136">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="f806d-137">En este momento, no hay ninguna opción de configuración para el protocolo MessagePack en el cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f806d-137">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="related-resources"></a><span data-ttu-id="f806d-138">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="f806d-138">Related resources</span></span>

* [<span data-ttu-id="f806d-139">Primeros pasos</span><span class="sxs-lookup"><span data-stu-id="f806d-139">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="f806d-140">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="f806d-140">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="f806d-141">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="f806d-141">JavaScript client</span></span>](xref:signalr/javascript-client)
