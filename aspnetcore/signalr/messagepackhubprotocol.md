---
title: Usar el protocolo MessagePack Hub en Signalr para ASP.NET Core
author: bradygaster
description: Agregue el protocolo MessagePack Hub a ASP.NET Core Signalr.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 10/08/2019
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: fe09b646eba5ae15cbd9e568b276aaf7763e4b1b
ms.sourcegitcommit: fcdf9aaa6c45c1a926bd870ed8f893bdb4935152
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72165402"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a><span data-ttu-id="89d02-103">Usar el protocolo MessagePack Hub en Signalr para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="89d02-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="89d02-104">Por [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="89d02-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="89d02-105">En este artículo se [da por supuesto](xref:tutorials/signalr)que el lector está familiarizado con los temas descritos en introducción.</span><span class="sxs-lookup"><span data-stu-id="89d02-105">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="89d02-106">¿Qué es MessagePack?</span><span class="sxs-lookup"><span data-stu-id="89d02-106">What is MessagePack?</span></span>

<span data-ttu-id="89d02-107">[MessagePack](https://msgpack.org/index.html) es un formato de serialización binaria rápido y compacto.</span><span class="sxs-lookup"><span data-stu-id="89d02-107">[MessagePack](https://msgpack.org/index.html) is a binary serialization format that is fast and compact.</span></span> <span data-ttu-id="89d02-108">Resulta útil cuando el rendimiento y el ancho de banda son un problema porque crea mensajes más pequeños en comparación con [JSON](https://www.json.org/).</span><span class="sxs-lookup"><span data-stu-id="89d02-108">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="89d02-109">Dado que se trata de un formato binario, los mensajes no se pueden leer al mirar los seguimientos de red y los registros a menos que los bytes se pasen a través de un analizador de MessagePack.</span><span class="sxs-lookup"><span data-stu-id="89d02-109">Because it's a binary format, messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> <span data-ttu-id="89d02-110">Signalr tiene compatibilidad integrada con el formato MessagePack y proporciona las API para que las use el cliente y el servidor.</span><span class="sxs-lookup"><span data-stu-id="89d02-110">SignalR has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="89d02-111">Configuración de MessagePack en el servidor</span><span class="sxs-lookup"><span data-stu-id="89d02-111">Configure MessagePack on the server</span></span>

<span data-ttu-id="89d02-112">Para habilitar el protocolo MessagePack Hub en el servidor, instale el paquete `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="89d02-112">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="89d02-113">En el archivo Startup.cs, agregue `AddMessagePackProtocol` a la llamada `AddSignalR` para habilitar la compatibilidad con MessagePack en el servidor.</span><span class="sxs-lookup"><span data-stu-id="89d02-113">In the Startup.cs file add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="89d02-114">JSON está habilitado de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="89d02-114">JSON is enabled by default.</span></span> <span data-ttu-id="89d02-115">Agregar MessagePack habilita la compatibilidad con los clientes JSON y MessagePack.</span><span class="sxs-lookup"><span data-stu-id="89d02-115">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="89d02-116">Para personalizar la forma en que MessagePack dará formato a los datos, `AddMessagePackProtocol` toma un delegado para configurar las opciones.</span><span class="sxs-lookup"><span data-stu-id="89d02-116">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="89d02-117">En ese delegado, se puede usar la propiedad `FormatterResolvers` para configurar las opciones de serialización de MessagePack.</span><span class="sxs-lookup"><span data-stu-id="89d02-117">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="89d02-118">Para obtener más información sobre cómo funcionan los solucionadores, visite la biblioteca MessagePack en [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span><span class="sxs-lookup"><span data-stu-id="89d02-118">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="89d02-119">Los atributos se pueden usar en los objetos que se van a serializar para definir cómo deben administrarse.</span><span class="sxs-lookup"><span data-stu-id="89d02-119">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

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

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="89d02-120">Configuración de MessagePack en el cliente</span><span class="sxs-lookup"><span data-stu-id="89d02-120">Configure MessagePack on the client</span></span>

> [!NOTE]
> <span data-ttu-id="89d02-121">JSON está habilitado de forma predeterminada para los clientes compatibles.</span><span class="sxs-lookup"><span data-stu-id="89d02-121">JSON is enabled by default for the supported clients.</span></span> <span data-ttu-id="89d02-122">Los clientes solo pueden admitir un único protocolo.</span><span class="sxs-lookup"><span data-stu-id="89d02-122">Clients can only support a single protocol.</span></span> <span data-ttu-id="89d02-123">Al agregar compatibilidad con MessagePack se reemplazarán todos los protocolos configurados previamente.</span><span class="sxs-lookup"><span data-stu-id="89d02-123">Adding MessagePack support will replace any previously configured protocols.</span></span>

### <a name="net-client"></a><span data-ttu-id="89d02-124">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="89d02-124">.NET client</span></span>

<span data-ttu-id="89d02-125">Para habilitar MessagePack en el cliente de .NET, instale el paquete `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` y llame a `AddMessagePackProtocol` en `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="89d02-125">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="89d02-126">Esta llamada `AddMessagePackProtocol` toma un delegado para configurar opciones como el servidor.</span><span class="sxs-lookup"><span data-stu-id="89d02-126">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="89d02-127">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="89d02-127">JavaScript client</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="89d02-128">El paquete NPM `@microsoft/signalr-protocol-msgpack` proporciona compatibilidad con MessagePack para el cliente JavaScript.</span><span class="sxs-lookup"><span data-stu-id="89d02-128">MessagePack support for the JavaScript client is provided by the `@microsoft/signalr-protocol-msgpack` npm package.</span></span> <span data-ttu-id="89d02-129">Instale el paquete ejecutando el siguiente comando en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="89d02-129">Install the package by executing the following command in a command shell:</span></span>

```console
npm install @microsoft/signalr-protocol-msgpack
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="89d02-130">El paquete NPM `@aspnet/signalr-protocol-msgpack` proporciona compatibilidad con MessagePack para el cliente JavaScript.</span><span class="sxs-lookup"><span data-stu-id="89d02-130">MessagePack support for the JavaScript client is provided by the `@aspnet/signalr-protocol-msgpack` npm package.</span></span> <span data-ttu-id="89d02-131">Instale el paquete ejecutando el siguiente comando en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="89d02-131">Install the package by executing the following command in a command shell:</span></span>

```console
npm install @aspnet/signalr-protocol-msgpack
```

::: moniker-end

<span data-ttu-id="89d02-132">Después de instalar el paquete NPM, el módulo se puede usar directamente a través de un cargador de módulos de JavaScript o importarse en el explorador haciendo referencia al archivo siguiente:</span><span class="sxs-lookup"><span data-stu-id="89d02-132">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the following file:</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="89d02-133">*node_modules @ no__t-1 @ no__t-2*</span><span class="sxs-lookup"><span data-stu-id="89d02-133">*node_modules\\@microsoft\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span></span> 

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="89d02-134">*node_modules @ no__t-1 @ no__t-2*</span><span class="sxs-lookup"><span data-stu-id="89d02-134">*node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span></span> 

::: moniker-end

<span data-ttu-id="89d02-135">En un explorador, también se debe hacer referencia a la biblioteca `msgpack5`.</span><span class="sxs-lookup"><span data-stu-id="89d02-135">In a browser, the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="89d02-136">Use una etiqueta `<script>` para crear una referencia.</span><span class="sxs-lookup"><span data-stu-id="89d02-136">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="89d02-137">La biblioteca se puede encontrar en *node_modules\msgpack5\dist\msgpack5.js*.</span><span class="sxs-lookup"><span data-stu-id="89d02-137">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="89d02-138">Al utilizar el elemento `<script>`, el orden es importante.</span><span class="sxs-lookup"><span data-stu-id="89d02-138">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="89d02-139">Si se hace referencia a *signalr-Protocol-msgpack. js* antes de *msgpack5. js*, se produce un error al intentar conectarse con MessagePack.</span><span class="sxs-lookup"><span data-stu-id="89d02-139">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="89d02-140">*signalr. js* también es necesario antes de *signalr-Protocol-msgpack. js*.</span><span class="sxs-lookup"><span data-stu-id="89d02-140">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="89d02-141">Al agregar `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` al `HubConnectionBuilder`, se configurará el cliente para que use el protocolo MessagePack al conectarse a un servidor.</span><span class="sxs-lookup"><span data-stu-id="89d02-141">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="89d02-142">En este momento, no hay ninguna opción de configuración para el protocolo MessagePack en el cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="89d02-142">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="messagepack-quirks"></a><span data-ttu-id="89d02-143">Peculiaridades de MessagePack</span><span class="sxs-lookup"><span data-stu-id="89d02-143">MessagePack quirks</span></span>

<span data-ttu-id="89d02-144">Hay algunos problemas que se deben tener en cuenta al usar el protocolo MessagePack Hub.</span><span class="sxs-lookup"><span data-stu-id="89d02-144">There are a few issues to be aware of when using the MessagePack Hub Protocol.</span></span>

### <a name="messagepack-is-case-sensitive"></a><span data-ttu-id="89d02-145">MessagePack distingue entre mayúsculas y minúsculas</span><span class="sxs-lookup"><span data-stu-id="89d02-145">MessagePack is case-sensitive</span></span>

<span data-ttu-id="89d02-146">El protocolo MessagePack distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="89d02-146">The MessagePack protocol is case-sensitive.</span></span> <span data-ttu-id="89d02-147">Por ejemplo, considere la siguiente C# clase:</span><span class="sxs-lookup"><span data-stu-id="89d02-147">For example, consider the following C# class:</span></span>

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

<span data-ttu-id="89d02-148">Al enviar desde el cliente JavaScript, debe utilizar nombres de propiedad `PascalCased`, ya que las mayúsculas y C# minúsculas deben coincidir exactamente con la clase.</span><span class="sxs-lookup"><span data-stu-id="89d02-148">When sending from the JavaScript client, you must use `PascalCased` property names, since the casing must match the C# class exactly.</span></span> <span data-ttu-id="89d02-149">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="89d02-149">For example:</span></span>

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

<span data-ttu-id="89d02-150">El uso de los nombres `camelCased` no se C# enlaza correctamente a la clase.</span><span class="sxs-lookup"><span data-stu-id="89d02-150">Using `camelCased` names won't properly bind to the C# class.</span></span> <span data-ttu-id="89d02-151">Puede solucionar este fin mediante el uso del atributo `Key` para especificar un nombre diferente para la propiedad MessagePack.</span><span class="sxs-lookup"><span data-stu-id="89d02-151">You can work around this by using the `Key` attribute to specify a different name for the MessagePack property.</span></span> <span data-ttu-id="89d02-152">Para obtener más información, consulte [la documentación de MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp#object-serialization).</span><span class="sxs-lookup"><span data-stu-id="89d02-152">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp#object-serialization).</span></span>

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a><span data-ttu-id="89d02-153">DateTime. Kind no se conserva al serializar o deserializar</span><span class="sxs-lookup"><span data-stu-id="89d02-153">DateTime.Kind is not preserved when serializing/deserializing</span></span>

<span data-ttu-id="89d02-154">El protocolo MessagePack no proporciona una manera de codificar el valor `Kind` de un `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="89d02-154">The MessagePack protocol doesn't provide a way to encode the `Kind` value of a `DateTime`.</span></span> <span data-ttu-id="89d02-155">Como resultado, al deserializar una fecha, el protocolo MessagePack Hub supone que la fecha de entrada está en formato UTC.</span><span class="sxs-lookup"><span data-stu-id="89d02-155">As a result, when deserializing a date, the MessagePack Hub Protocol assumes the incoming date is in UTC format.</span></span> <span data-ttu-id="89d02-156">Si está trabajando con valores `DateTime` en la hora local, se recomienda convertir a UTC antes de enviarlos.</span><span class="sxs-lookup"><span data-stu-id="89d02-156">If you're working with `DateTime` values in local time, we recommend converting to UTC before sending them.</span></span> <span data-ttu-id="89d02-157">Conviértalos de la hora UTC a la hora local cuando los reciba.</span><span class="sxs-lookup"><span data-stu-id="89d02-157">Convert them from UTC to local time when you receive them.</span></span>

<span data-ttu-id="89d02-158">Para obtener más información sobre esta limitación, vea el problema de GitHub [ASPNET/signalr # 2632](https://github.com/aspnet/SignalR/issues/2632).</span><span class="sxs-lookup"><span data-stu-id="89d02-158">For more information on this limitation, see GitHub issue [aspnet/SignalR#2632](https://github.com/aspnet/SignalR/issues/2632).</span></span>

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a><span data-ttu-id="89d02-159">No se admite DateTime. MinValue en MessagePack en JavaScript</span><span class="sxs-lookup"><span data-stu-id="89d02-159">DateTime.MinValue is not supported by MessagePack in JavaScript</span></span>

<span data-ttu-id="89d02-160">La biblioteca [msgpack5](https://github.com/mcollina/msgpack5) que usa el cliente de signalr JavaScript no admite el tipo `timestamp96` en MessagePack.</span><span class="sxs-lookup"><span data-stu-id="89d02-160">The [msgpack5](https://github.com/mcollina/msgpack5) library used by the SignalR JavaScript client doesn't support the `timestamp96` type in MessagePack.</span></span> <span data-ttu-id="89d02-161">Este tipo se usa para codificar valores de fecha muy grandes (ya sea muy temprano en el pasado o muy lejos en el futuro).</span><span class="sxs-lookup"><span data-stu-id="89d02-161">This type is used to encode very large date values (either very early in the past or very far in the future).</span></span> <span data-ttu-id="89d02-162">El valor de `DateTime.MinValue` es `January 1, 0001`, que se debe codificar en un valor de @no__t 2.</span><span class="sxs-lookup"><span data-stu-id="89d02-162">The value of `DateTime.MinValue` is `January 1, 0001` which must be encoded in a `timestamp96` value.</span></span> <span data-ttu-id="89d02-163">Por este motivo, no se admite el envío de `DateTime.MinValue` a un cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="89d02-163">Because of this, sending `DateTime.MinValue` to a JavaScript client isn't supported.</span></span> <span data-ttu-id="89d02-164">Cuando el cliente JavaScript recibe `DateTime.MinValue`, se produce el siguiente error:</span><span class="sxs-lookup"><span data-stu-id="89d02-164">When `DateTime.MinValue` is received by the JavaScript client, the following error is thrown:</span></span>

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

<span data-ttu-id="89d02-165">Normalmente, `DateTime.MinValue` se usa para codificar un valor "Missing" o `null`.</span><span class="sxs-lookup"><span data-stu-id="89d02-165">Usually, `DateTime.MinValue` is used to encode a "missing" or `null` value.</span></span> <span data-ttu-id="89d02-166">Si necesita codificar ese valor en MessagePack, use un valor `DateTime` que acepte valores NULL (`DateTime?`) o codifique un valor `bool` independiente que indique si la fecha está presente.</span><span class="sxs-lookup"><span data-stu-id="89d02-166">If you need to encode that value in MessagePack, use a nullable `DateTime` value (`DateTime?`) or encode a separate `bool` value indicating if the date is present.</span></span>

<span data-ttu-id="89d02-167">Para obtener más información sobre esta limitación, vea el problema de GitHub [ASPNET/signalr # 2228](https://github.com/aspnet/SignalR/issues/2228).</span><span class="sxs-lookup"><span data-stu-id="89d02-167">For more information on this limitation, see GitHub issue [aspnet/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228).</span></span>

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a><span data-ttu-id="89d02-168">Compatibilidad con MessagePack en el entorno de compilación "anterior a la hora"</span><span class="sxs-lookup"><span data-stu-id="89d02-168">MessagePack support in "ahead-of-time" compilation environment</span></span>

<span data-ttu-id="89d02-169">La biblioteca [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp) utilizada por el cliente y el servidor .net utiliza la generación de código para optimizar la serialización.</span><span class="sxs-lookup"><span data-stu-id="89d02-169">The [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp) library used by the .NET client and server uses code generation to optimize serialization.</span></span> <span data-ttu-id="89d02-170">Como resultado, no se admite de forma predeterminada en los entornos que usan la compilación "anterior a la hora" (como Xamarin iOS o Unity).</span><span class="sxs-lookup"><span data-stu-id="89d02-170">As a result, it isn't supported by default on environments that use "ahead-of-time" compilation (such as Xamarin iOS or Unity).</span></span> <span data-ttu-id="89d02-171">Es posible usar MessagePack en estos entornos mediante la "generación previa" del código del serializador/deserializador.</span><span class="sxs-lookup"><span data-stu-id="89d02-171">It's possible to use MessagePack in these environments by "pre-generating" the serializer/deserializer code.</span></span> <span data-ttu-id="89d02-172">Para obtener más información, consulte [la documentación de MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp#pre-code-generationunityxamarin-supports).</span><span class="sxs-lookup"><span data-stu-id="89d02-172">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp#pre-code-generationunityxamarin-supports).</span></span> <span data-ttu-id="89d02-173">Una vez que haya generado los serializadores previamente, puede registrarlos mediante el delegado de configuración que se pasa a `AddMessagePackProtocol`:</span><span class="sxs-lookup"><span data-stu-id="89d02-173">Once you have pre-generated the serializers, you can register them using the configuration delegate passed to `AddMessagePackProtocol`:</span></span>

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

### <a name="type-checks-are-more-strict-in-messagepack"></a><span data-ttu-id="89d02-174">Las comprobaciones de tipo son más estrictas en MessagePack</span><span class="sxs-lookup"><span data-stu-id="89d02-174">Type checks are more strict in MessagePack</span></span>

<span data-ttu-id="89d02-175">El protocolo del concentrador de JSON realizará las conversiones de tipos durante la deserialización.</span><span class="sxs-lookup"><span data-stu-id="89d02-175">The JSON Hub Protocol will perform type conversions during deserialization.</span></span> <span data-ttu-id="89d02-176">Por ejemplo, si el objeto entrante tiene un valor de propiedad que es un número (`{ foo: 42 }`) pero la propiedad de la clase .NET es de tipo `string`, el valor se convertirá.</span><span class="sxs-lookup"><span data-stu-id="89d02-176">For example, if the incoming object has a property value that is a number (`{ foo: 42 }`) but the property on the .NET class is of type `string`, the value will be converted.</span></span> <span data-ttu-id="89d02-177">Sin embargo, MessagePack no realiza esta conversión y producirá una excepción que se puede observar en los registros del lado servidor (y en la consola):</span><span class="sxs-lookup"><span data-stu-id="89d02-177">However, MessagePack doesn't perform this conversion and will throw an exception that can be seen in server-side logs (and in the console):</span></span>

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

<span data-ttu-id="89d02-178">Para obtener más información sobre esta limitación, vea el problema de GitHub [ASPNET/signalr # 2937](https://github.com/aspnet/SignalR/issues/2937).</span><span class="sxs-lookup"><span data-stu-id="89d02-178">For more information on this limitation, see GitHub issue [aspnet/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937).</span></span>

## <a name="related-resources"></a><span data-ttu-id="89d02-179">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="89d02-179">Related resources</span></span>

* [<span data-ttu-id="89d02-180">Primeros pasos</span><span class="sxs-lookup"><span data-stu-id="89d02-180">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="89d02-181">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="89d02-181">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="89d02-182">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="89d02-182">JavaScript client</span></span>](xref:signalr/javascript-client)
