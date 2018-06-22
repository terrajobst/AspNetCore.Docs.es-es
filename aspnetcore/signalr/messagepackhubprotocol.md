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
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a><span data-ttu-id="0771d-103">Utilice el protocolo de concentrador de MessagePack en SignalR para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0771d-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="0771d-104">Por [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="0771d-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="0771d-105">En este artículo se da por supuesto que el lector está familiarizado con los temas tratados en [Introducción](xref:tutorials/signalr).</span><span class="sxs-lookup"><span data-stu-id="0771d-105">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="0771d-106">¿Qué es MessagePack?</span><span class="sxs-lookup"><span data-stu-id="0771d-106">What is MessagePack?</span></span>

<span data-ttu-id="0771d-107">[MessagePack](https://msgpack.org/index.html) es un formato de serialización binaria es rápido y compacto.</span><span class="sxs-lookup"><span data-stu-id="0771d-107">[MessagePack](https://msgpack.org/index.html) is a binary serialization format that is fast and compact.</span></span> <span data-ttu-id="0771d-108">Es útil cuando el rendimiento y el ancho de banda son un problema porque crea mensajes más pequeños en comparación con [JSON](https://www.json.org/).</span><span class="sxs-lookup"><span data-stu-id="0771d-108">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="0771d-109">Dado que es un formato binario, mensajes no son legibles cuando se examinan los registros y seguimientos de red a menos que los bytes se pasan a través de un analizador de MessagePack.</span><span class="sxs-lookup"><span data-stu-id="0771d-109">Because it's a binary format, messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> <span data-ttu-id="0771d-110">SignalR tiene compatibilidad integrada para el formato MessagePack y proporciona las API para el cliente y el servidor usar.</span><span class="sxs-lookup"><span data-stu-id="0771d-110">SignalR has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="0771d-111">Configurar MessagePack en el servidor</span><span class="sxs-lookup"><span data-stu-id="0771d-111">Configure MessagePack on the server</span></span>

<span data-ttu-id="0771d-112">Para habilitar el protocolo de concentrador MessagePack en el servidor, instale el `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` paquete de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0771d-112">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="0771d-113">En el archivo Startup.cs agregue `AddMessagePackProtocol` a la `AddSignalR` llamada para habilitar la compatibilidad de MessagePack en el servidor.</span><span class="sxs-lookup"><span data-stu-id="0771d-113">In the Startup.cs file add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="0771d-114">JSON está habilitada de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="0771d-114">JSON is enabled by default.</span></span> <span data-ttu-id="0771d-115">Agregar MessagePack habilita la compatibilidad para los clientes de JSON y MessagePack.</span><span class="sxs-lookup"><span data-stu-id="0771d-115">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="0771d-116">Para personalizar cómo MessagePack dará formato a los datos, `AddMessagePackProtocol` toma un delegado para configurar las opciones.</span><span class="sxs-lookup"><span data-stu-id="0771d-116">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="0771d-117">En ese delegado, el `FormatterResolvers` propiedad puede utilizarse para configurar las opciones de serialización MessagePack.</span><span class="sxs-lookup"><span data-stu-id="0771d-117">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="0771d-118">Para obtener más información sobre cómo funcionan los solucionadores, visite la biblioteca de MessagePack en [MessagePack CSharp](https://github.com/neuecc/MessagePack-CSharp).</span><span class="sxs-lookup"><span data-stu-id="0771d-118">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="0771d-119">Los atributos se pueden utilizar en los objetos que desea serializar para definir cómo debe controlarse.</span><span class="sxs-lookup"><span data-stu-id="0771d-119">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

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

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="0771d-120">Configurar MessagePack en el cliente</span><span class="sxs-lookup"><span data-stu-id="0771d-120">Configure MessagePack on the client</span></span>

### <a name="net-client"></a><span data-ttu-id="0771d-121">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="0771d-121">.NET client</span></span>

<span data-ttu-id="0771d-122">Para habilitar MessagePack en el cliente. NET, instale el `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` paquete y llame al método `AddMessagePackProtocol` en `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="0771d-122">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="0771d-123">Esto `AddMessagePackProtocol` llamada toma un delegado para configurar opciones como el servidor.</span><span class="sxs-lookup"><span data-stu-id="0771d-123">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="0771d-124">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="0771d-124">JavaScript client</span></span>

<span data-ttu-id="0771d-125">MessagePack para el cliente de Javascript se admiten por los `@aspnet/signalr-protocol-msgpack` paquete NPM.</span><span class="sxs-lookup"><span data-stu-id="0771d-125">MessagePack support for the Javascript client is provided by the `@aspnet/signalr-protocol-msgpack` NPM package.</span></span>

```console
npm install @aspnet/signalr-protocol-msgpack
```

<span data-ttu-id="0771d-126">Después de instalar el paquete npm, el módulo se puede usar directamente mediante un cargador de módulos de JavaScript o importarse en el explorador haciendo referencia a la *node_modules\\ @aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*  archivo.</span><span class="sxs-lookup"><span data-stu-id="0771d-126">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* file.</span></span> <span data-ttu-id="0771d-127">En un explorador la `msgpack5` también se debe hacer referencia a la biblioteca.</span><span class="sxs-lookup"><span data-stu-id="0771d-127">In a browser the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="0771d-128">Use un `<script>` etiqueta que se va a crear una referencia.</span><span class="sxs-lookup"><span data-stu-id="0771d-128">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="0771d-129">La biblioteca se puede encontrar en *node_modules\msgpack5\dist\msgpack5.js*.</span><span class="sxs-lookup"><span data-stu-id="0771d-129">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="0771d-130">Cuando se usa el `<script>` elemento, el orden es importante.</span><span class="sxs-lookup"><span data-stu-id="0771d-130">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="0771d-131">Si *msgpack.js de protocolo de signalr* se hace referencia antes de *msgpack5.js*, se produce un error al intentar conectarse con MessagePack.</span><span class="sxs-lookup"><span data-stu-id="0771d-131">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="0771d-132">*signalr.js* también es necesaria antes de *msgpack.js de protocolo de signalr*.</span><span class="sxs-lookup"><span data-stu-id="0771d-132">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="0771d-133">Agregar `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` a la `HubConnectionBuilder` va a configurar el cliente para utilizar el protocolo MessagePack al conectarse a un servidor.</span><span class="sxs-lookup"><span data-stu-id="0771d-133">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="0771d-134">En este momento, no hay ninguna opción de configuración para el protocolo MessagePack en el cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0771d-134">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="related-resources"></a><span data-ttu-id="0771d-135">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="0771d-135">Related resources</span></span>

* [<span data-ttu-id="0771d-136">Primeros pasos</span><span class="sxs-lookup"><span data-stu-id="0771d-136">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="0771d-137">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="0771d-137">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="0771d-138">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="0771d-138">JavaScript client</span></span>](xref:signalr/javascript-client)
