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
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896522"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a><span data-ttu-id="8b5e3-103">Usar el protocolo de MessagePack concentrador de SignalR para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8b5e3-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="8b5e3-104">Por [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="8b5e3-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="8b5e3-105">En este artículo se da por supuesto que el lector está familiarizado con los temas tratados en [comenzar](xref:tutorials/signalr).</span><span class="sxs-lookup"><span data-stu-id="8b5e3-105">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="8b5e3-106">¿Qué es MessagePack?</span><span class="sxs-lookup"><span data-stu-id="8b5e3-106">What is MessagePack?</span></span>

<span data-ttu-id="8b5e3-107">[MessagePack](https://msgpack.org/index.html) es un formato de serialización binaria es rápido y compacto.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-107">[MessagePack](https://msgpack.org/index.html) is a binary serialization format that is fast and compact.</span></span> <span data-ttu-id="8b5e3-108">Es útil cuando constituyen un problema de rendimiento y ancho de banda porque crea mensajes más pequeños en comparación con [JSON](https://www.json.org/).</span><span class="sxs-lookup"><span data-stu-id="8b5e3-108">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="8b5e3-109">Dado que es un formato binario, los mensajes son ilegibles al examinar los registros y seguimientos de red a menos que los bytes se pasan a través de un analizador MessagePack.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-109">Because it's a binary format, messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> <span data-ttu-id="8b5e3-110">Tiene compatibilidad integrada con el formato MessagePack SignalR y proporciona las API para el cliente y servidor usar.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-110">SignalR has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="8b5e3-111">Configurar MessagePack en el servidor</span><span class="sxs-lookup"><span data-stu-id="8b5e3-111">Configure MessagePack on the server</span></span>

<span data-ttu-id="8b5e3-112">Para habilitar el protocolo de Hub MessagePack en el servidor, instale el `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` paquete en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-112">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="8b5e3-113">En el archivo Startup.cs agregue `AddMessagePackProtocol` a la `AddSignalR` llamada a habilitar la compatibilidad con MessagePack en el servidor.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-113">In the Startup.cs file add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="8b5e3-114">JSON está habilitado de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-114">JSON is enabled by default.</span></span> <span data-ttu-id="8b5e3-115">Agregar MessagePack habilita la compatibilidad con clientes MessagePack y JSON.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-115">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="8b5e3-116">Para personalizar cómo MessagePack dará formato a los datos, `AddMessagePackProtocol` toma un delegado para configurar las opciones.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-116">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="8b5e3-117">En ese delegado, el `FormatterResolvers` propiedad puede usarse para configurar las opciones de serialización MessagePack.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-117">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="8b5e3-118">Para obtener más información sobre cómo funcionan las resoluciones, visite la biblioteca MessagePack en [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span><span class="sxs-lookup"><span data-stu-id="8b5e3-118">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="8b5e3-119">Atributos se pueden usar en los objetos que desea serializar para definir cómo debe controlarse.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-119">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

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

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="8b5e3-120">Configurar MessagePack en el cliente</span><span class="sxs-lookup"><span data-stu-id="8b5e3-120">Configure MessagePack on the client</span></span>

> [!NOTE]
> <span data-ttu-id="8b5e3-121">JSON está habilitada de forma predeterminada para los clientes compatibles.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-121">JSON is enabled by default for the supported clients.</span></span> <span data-ttu-id="8b5e3-122">Los clientes pueden admitir solo un único protocolo.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-122">Clients can only support a single protocol.</span></span> <span data-ttu-id="8b5e3-123">Agregar compatibilidad con MessagePack reemplazará cualquier previamente los protocolos configurados.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-123">Adding MessagePack support will replace any previously configured protocols.</span></span>

### <a name="net-client"></a><span data-ttu-id="8b5e3-124">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="8b5e3-124">.NET client</span></span>

<span data-ttu-id="8b5e3-125">Para habilitar MessagePack en el cliente. NET, instale el `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` paquetes y llame a `AddMessagePackProtocol` en `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-125">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="8b5e3-126">Esto `AddMessagePackProtocol` llamada toma un delegado para configurar las opciones al igual que el servidor.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-126">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="8b5e3-127">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="8b5e3-127">JavaScript client</span></span>

<span data-ttu-id="8b5e3-128">MessagePack compatibilidad con el cliente de JavaScript proporciona el `@aspnet/signalr-protocol-msgpack` paquete npm.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-128">MessagePack support for the JavaScript client is provided by the `@aspnet/signalr-protocol-msgpack` npm package.</span></span>

```console
npm install @aspnet/signalr-protocol-msgpack
```

<span data-ttu-id="8b5e3-129">Después de instalar el paquete de npm, el módulo se puede utilizar directamente a través de un cargador de módulos de JavaScript o importado en el explorador haciendo referencia a la *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* archivo.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-129">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* file.</span></span> <span data-ttu-id="8b5e3-130">En un explorador, el `msgpack5` también se debe hacer referencia a la biblioteca.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-130">In a browser, the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="8b5e3-131">Use un `<script>` etiqueta para crear una referencia.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-131">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="8b5e3-132">La biblioteca puede encontrarse en *node_modules\msgpack5\dist\msgpack5.js*.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-132">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="8b5e3-133">Cuando se usa el `<script>` elemento, el orden es importante.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-133">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="8b5e3-134">Si *signalr-protocol-msgpack.js* se hace referencia antes de *msgpack5.js*, se produce un error al intentar conectarse con MessagePack.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-134">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="8b5e3-135">*signalr.js* también es necesaria antes de *signalr-protocol-msgpack.js*.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-135">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="8b5e3-136">Agregar `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` a la `HubConnectionBuilder` va a configurar el cliente para utilizar el protocolo MessagePack al conectarse a un servidor.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-136">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="8b5e3-137">En este momento, no hay ninguna opción de configuración para el protocolo MessagePack en el cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-137">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="messagepack-quirks"></a><span data-ttu-id="8b5e3-138">MessagePack peculiaridades</span><span class="sxs-lookup"><span data-stu-id="8b5e3-138">MessagePack quirks</span></span>

<span data-ttu-id="8b5e3-139">Hay algunos problemas que tenga en cuenta cuando se usa el protocolo de concentrador MessagePack.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-139">There are a few issues to be aware of when using the MessagePack Hub Protocol.</span></span>

### <a name="messagepack-is-case-sensitive"></a><span data-ttu-id="8b5e3-140">MessagePack distingue mayúsculas de minúsculas</span><span class="sxs-lookup"><span data-stu-id="8b5e3-140">MessagePack is case-sensitive</span></span>

<span data-ttu-id="8b5e3-141">El protocolo MessagePack distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-141">The MessagePack protocol is case-sensitive.</span></span> <span data-ttu-id="8b5e3-142">Por ejemplo, considere la siguiente C# clase:</span><span class="sxs-lookup"><span data-stu-id="8b5e3-142">For example, consider the following C# class:</span></span>

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

<span data-ttu-id="8b5e3-143">Cuando se envían desde el cliente de JavaScript, debe usar `PascalCased` nombres de propiedad, ya que deben coincidir con las mayúsculas y minúsculas del C# clase exactamente.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-143">When sending from the JavaScript client, you must use `PascalCased` property names, since the casing must match the C# class exactly.</span></span> <span data-ttu-id="8b5e3-144">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8b5e3-144">For example:</span></span>

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

<span data-ttu-id="8b5e3-145">Uso de `camelCased` nombres correctamente no se enlazará a la C# clase.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-145">Using `camelCased` names won't properly bind to the C# class.</span></span> <span data-ttu-id="8b5e3-146">Puede solucionar esto mediante el uso de la `Key` atributo para especificar un nombre diferente para la propiedad MessagePack.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-146">You can work around this by using the `Key` attribute to specify a different name for the MessagePack property.</span></span> <span data-ttu-id="8b5e3-147">Para obtener más información, consulte [la documentación de MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp#object-serialization).</span><span class="sxs-lookup"><span data-stu-id="8b5e3-147">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp#object-serialization).</span></span>

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a><span data-ttu-id="8b5e3-148">DateTime.Kind no se conserva al serializar o deserializar</span><span class="sxs-lookup"><span data-stu-id="8b5e3-148">DateTime.Kind is not preserved when serializing/deserializing</span></span>

<span data-ttu-id="8b5e3-149">El protocolo MessagePack no proporciona una manera de codificar el `Kind` valor de un `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-149">The MessagePack protocol doesn't provide a way to encode the `Kind` value of a `DateTime`.</span></span> <span data-ttu-id="8b5e3-150">Como resultado, al deserializar una fecha, el protocolo de Hub MessagePack supone que la fecha de entrada está en formato UTC.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-150">As a result, when deserializing a date, the MessagePack Hub Protocol assumes the incoming date is in UTC format.</span></span> <span data-ttu-id="8b5e3-151">Si está trabajando con `DateTime` valores de hora local, se recomienda convertir a UTC antes de enviarlos.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-151">If you're working with `DateTime` values in local time, we recommend converting to UTC before sending them.</span></span> <span data-ttu-id="8b5e3-152">Convertirlos a la hora UTC a la hora local cuando los recibe.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-152">Convert them from UTC to local time when you receive them.</span></span>

<span data-ttu-id="8b5e3-153">Para obtener más información sobre esta limitación, vea GitHub problema [aspnet/SignalR #2632](https://github.com/aspnet/SignalR/issues/2632).</span><span class="sxs-lookup"><span data-stu-id="8b5e3-153">For more information on this limitation, see GitHub issue [aspnet/SignalR#2632](https://github.com/aspnet/SignalR/issues/2632).</span></span>

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a><span data-ttu-id="8b5e3-154">DateTime.MinValue no es compatible con MessagePack en JavaScript</span><span class="sxs-lookup"><span data-stu-id="8b5e3-154">DateTime.MinValue is not supported by MessagePack in JavaScript</span></span>

<span data-ttu-id="8b5e3-155">El [msgpack5](https://github.com/mcollina/msgpack5) biblioteca usada por el cliente de JavaScript de SignalR no es compatible con la `timestamp96` tipo en MessagePack.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-155">The [msgpack5](https://github.com/mcollina/msgpack5) library used by the SignalR JavaScript client doesn't support the `timestamp96` type in MessagePack.</span></span> <span data-ttu-id="8b5e3-156">Este tipo se usa para codificar los valores de fecha muy grande (ya sea muy pronto en el pasado o en el futuro muy lejano).</span><span class="sxs-lookup"><span data-stu-id="8b5e3-156">This type is used to encode very large date values (either very early in the past or very far in the future).</span></span> <span data-ttu-id="8b5e3-157">El valor de `DateTime.MinValue` es `January 1, 0001` que debe estar codificado en un `timestamp96` valor.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-157">The value of `DateTime.MinValue` is `January 1, 0001` which must be encoded in a `timestamp96` value.</span></span> <span data-ttu-id="8b5e3-158">Debido a esto, enviar `DateTime.MinValue` un JavaScript no se admite el cliente.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-158">Because of this, sending `DateTime.MinValue` to a JavaScript client isn't supported.</span></span> <span data-ttu-id="8b5e3-159">Cuando `DateTime.MinValue` es recibido por el cliente de JavaScript, se produce el error siguiente:</span><span class="sxs-lookup"><span data-stu-id="8b5e3-159">When `DateTime.MinValue` is received by the JavaScript client, the following error is thrown:</span></span>

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

<span data-ttu-id="8b5e3-160">Por lo general, `DateTime.MinValue` se usa para codificar un "Falta" o `null` valor.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-160">Usually, `DateTime.MinValue` is used to encode a "missing" or `null` value.</span></span> <span data-ttu-id="8b5e3-161">Si tiene que codificar ese valor en MessagePack, usar una que acepta valores NULL `DateTime` valor (`DateTime?`) o codificar un independiente `bool` valor que indica si la fecha está presente.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-161">If you need to encode that value in MessagePack, use a nullable `DateTime` value (`DateTime?`) or encode a separate `bool` value indicating if the date is present.</span></span>

<span data-ttu-id="8b5e3-162">Para obtener más información sobre esta limitación, vea GitHub problema [aspnet/SignalR #2228](https://github.com/aspnet/SignalR/issues/2228).</span><span class="sxs-lookup"><span data-stu-id="8b5e3-162">For more information on this limitation, see GitHub issue [aspnet/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228).</span></span>

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a><span data-ttu-id="8b5e3-163">Compatibilidad con MessagePack en el entorno de compilación "ahead-of-time"</span><span class="sxs-lookup"><span data-stu-id="8b5e3-163">MessagePack support in "ahead-of-time" compilation environment</span></span>

<span data-ttu-id="8b5e3-164">El [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp) biblioteca usada por el cliente de .NET y el servidor usa la generación de código para optimizar la serialización.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-164">The [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp) library used by the .NET client and server uses code generation to optimize serialization.</span></span> <span data-ttu-id="8b5e3-165">Como resultado, no se admite de forma predeterminada en entornos que usan las compilación "ahead-of-time" (por ejemplo, Xamarin iOS o Unity).</span><span class="sxs-lookup"><span data-stu-id="8b5e3-165">As a result, it isn't supported by default on environments that use "ahead-of-time" compilation (such as Xamarin iOS or Unity).</span></span> <span data-ttu-id="8b5e3-166">Es posible usar MessagePack en estos entornos "generando previamente" el código del serializador/deserializador.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-166">It's possible to use MessagePack in these environments by "pre-generating" the serializer/deserializer code.</span></span> <span data-ttu-id="8b5e3-167">Para obtener más información, consulte [la documentación de MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp#pre-code-generationunityxamarin-supports).</span><span class="sxs-lookup"><span data-stu-id="8b5e3-167">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp#pre-code-generationunityxamarin-supports).</span></span> <span data-ttu-id="8b5e3-168">Una vez que los serializadores generados previamente, puede registrarlos mediante el delegado de configuración pasando a `AddMessagePackProtocol`:</span><span class="sxs-lookup"><span data-stu-id="8b5e3-168">Once you have pre-generated the serializers, you can register them using the configuration delegate passed to `AddMessagePackProtocol`:</span></span>

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

### <a name="type-checks-are-more-strict-in-messagepack"></a><span data-ttu-id="8b5e3-169">Comprobaciones de tipo sean más estrictas en MessagePack</span><span class="sxs-lookup"><span data-stu-id="8b5e3-169">Type checks are more strict in MessagePack</span></span>

<span data-ttu-id="8b5e3-170">El protocolo de Hub JSON llevará a cabo las conversiones de tipos durante la deserialización.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-170">The JSON Hub Protocol will perform type conversions during deserialization.</span></span> <span data-ttu-id="8b5e3-171">Por ejemplo, si el objeto entrante tiene un valor de propiedad que es un número (`{ foo: 42 }`), pero la propiedad en la clase de .NET es de tipo `string`, se convertirá el valor.</span><span class="sxs-lookup"><span data-stu-id="8b5e3-171">For example, if the incoming object has a property value that is a number (`{ foo: 42 }`) but the property on the .NET class is of type `string`, the value will be converted.</span></span> <span data-ttu-id="8b5e3-172">Sin embargo, MessagePack no realiza esta conversión y producirá una excepción que se puede ver en los registros del lado servidor (y en la consola):</span><span class="sxs-lookup"><span data-stu-id="8b5e3-172">However, MessagePack doesn't perform this conversion and will throw an exception that can be seen in server-side logs (and in the console):</span></span>

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

<span data-ttu-id="8b5e3-173">Para obtener más información sobre esta limitación, vea GitHub problema [aspnet/SignalR #2937](https://github.com/aspnet/SignalR/issues/2937).</span><span class="sxs-lookup"><span data-stu-id="8b5e3-173">For more information on this limitation, see GitHub issue [aspnet/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937).</span></span>

## <a name="related-resources"></a><span data-ttu-id="8b5e3-174">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="8b5e3-174">Related resources</span></span>

* [<span data-ttu-id="8b5e3-175">Primeros pasos</span><span class="sxs-lookup"><span data-stu-id="8b5e3-175">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="8b5e3-176">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="8b5e3-176">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="8b5e3-177">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="8b5e3-177">JavaScript client</span></span>](xref:signalr/javascript-client)
