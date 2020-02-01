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
ms.openlocfilehash: 3c2a4285945d3fdc6bba195e3160da8b9dcbba44
ms.sourcegitcommit: 0b0e485a8a6dfcc65a7a58b365622b3839f4d624
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2020
ms.locfileid: "76928173"
---
# <a name="use-messagepack-hub-protocol-in-opno-locsignalr-for-aspnet-core"></a><span data-ttu-id="e6ecd-103">Usar el protocolo MessagePack Hub en SignalR para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e6ecd-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="e6ecd-104">Por [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="e6ecd-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="e6ecd-105">En este artículo se [da por supuesto](xref:tutorials/signalr)que el lector está familiarizado con los temas descritos en introducción.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-105">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="e6ecd-106">¿Qué es MessagePack?</span><span class="sxs-lookup"><span data-stu-id="e6ecd-106">What is MessagePack?</span></span>

<span data-ttu-id="e6ecd-107">[MessagePack](https://msgpack.org/index.html) es un formato de serialización binaria rápido y compacto.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-107">[MessagePack](https://msgpack.org/index.html) is a binary serialization format that is fast and compact.</span></span> <span data-ttu-id="e6ecd-108">Resulta útil cuando el rendimiento y el ancho de banda son un problema porque crea mensajes más pequeños en comparación con [JSON](https://www.json.org/).</span><span class="sxs-lookup"><span data-stu-id="e6ecd-108">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="e6ecd-109">Dado que se trata de un formato binario, los mensajes no se pueden leer al mirar los seguimientos de red y los registros a menos que los bytes se pasen a través de un analizador de MessagePack.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-109">Because it's a binary format, messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> SignalR<span data-ttu-id="e6ecd-110"> tiene compatibilidad integrada con el formato MessagePack y proporciona las API para que las use el cliente y el servidor.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-110"> has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="e6ecd-111">Configuración de MessagePack en el servidor</span><span class="sxs-lookup"><span data-stu-id="e6ecd-111">Configure MessagePack on the server</span></span>

<span data-ttu-id="e6ecd-112">Para habilitar el protocolo MessagePack Hub en el servidor, instale el paquete de `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-112">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="e6ecd-113">En el archivo Startup.cs, agregue `AddMessagePackProtocol` a la llamada `AddSignalR` para habilitar la compatibilidad con MessagePack en el servidor.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-113">In the Startup.cs file add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="e6ecd-114">JSON está habilitado de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-114">JSON is enabled by default.</span></span> <span data-ttu-id="e6ecd-115">Agregar MessagePack habilita la compatibilidad con los clientes JSON y MessagePack.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-115">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="e6ecd-116">Para personalizar la forma en que MessagePack aplicará formato a los datos, `AddMessagePackProtocol` toma un delegado para configurar las opciones.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-116">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="e6ecd-117">En ese delegado, la propiedad `FormatterResolvers` se puede usar para configurar las opciones de serialización de MessagePack.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-117">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="e6ecd-118">Para obtener más información sobre cómo funcionan los solucionadores, visite la biblioteca MessagePack en [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span><span class="sxs-lookup"><span data-stu-id="e6ecd-118">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="e6ecd-119">Los atributos se pueden usar en los objetos que se van a serializar para definir cómo deben administrarse.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-119">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

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

> [!WARNING]
> <span data-ttu-id="e6ecd-120">Se recomienda encarecidamente revisar [CVE-2020-5234](https://github.com/neuecc/MessagePack-CSharp/security/advisories/GHSA-7q36-4xx7-xcxf) y aplicar las revisiones recomendadas.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-120">We strongly recommend reviewing [CVE-2020-5234](https://github.com/neuecc/MessagePack-CSharp/security/advisories/GHSA-7q36-4xx7-xcxf) and applying the recommended patches.</span></span> <span data-ttu-id="e6ecd-121">Por ejemplo, al establecer la propiedad estática `MessagePackSecurity.Active` en `MessagePackSecurity.UntrustedData`.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-121">For example, setting the `MessagePackSecurity.Active` static property to `MessagePackSecurity.UntrustedData`.</span></span> <span data-ttu-id="e6ecd-122">La configuración del `MessagePackSecurity.Active` requiere la instalación manual [de una versión 1.9. x de MessagePack](https://www.nuget.org/packages/MessagePack/1.9.3).</span><span class="sxs-lookup"><span data-stu-id="e6ecd-122">Setting the `MessagePackSecurity.Active` requires manually installing a [1.9.x version of MessagePack](https://www.nuget.org/packages/MessagePack/1.9.3).</span></span> <span data-ttu-id="e6ecd-123">La instalación de `MessagePack` 1.9. x actualiza la versión que usa SignalR.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-123">Installing `MessagePack` 1.9.x upgrades the version SignalR uses.</span></span> <span data-ttu-id="e6ecd-124">Cuando `MessagePackSecurity.Active` no está establecido en `MessagePackSecurity.UntrustedData`, un cliente malintencionado podría producir una denegación de servicio.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-124">When `MessagePackSecurity.Active` is not set to `MessagePackSecurity.UntrustedData`, a malicious client could cause a denial of service.</span></span> <span data-ttu-id="e6ecd-125">Establezca `MessagePackSecurity.Active` en `Program.Main`, como se muestra en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="e6ecd-125">Set `MessagePackSecurity.Active` in `Program.Main`, as shown in the following code:</span></span>

```csharp
public static void Main(string[] args)
{
  MessagePackSecurity.Active = MessagePackSecurity.UntrustedData;

  CreateHostBuilder(args).Build().Run();
}
```

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="e6ecd-126">Configuración de MessagePack en el cliente</span><span class="sxs-lookup"><span data-stu-id="e6ecd-126">Configure MessagePack on the client</span></span>

> [!NOTE]
> <span data-ttu-id="e6ecd-127">JSON está habilitado de forma predeterminada para los clientes compatibles.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-127">JSON is enabled by default for the supported clients.</span></span> <span data-ttu-id="e6ecd-128">Los clientes solo pueden admitir un único protocolo.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-128">Clients can only support a single protocol.</span></span> <span data-ttu-id="e6ecd-129">Al agregar compatibilidad con MessagePack se reemplazarán todos los protocolos configurados previamente.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-129">Adding MessagePack support will replace any previously configured protocols.</span></span>

### <a name="net-client"></a><span data-ttu-id="e6ecd-130">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="e6ecd-130">.NET client</span></span>

<span data-ttu-id="e6ecd-131">Para habilitar MessagePack en el cliente de .NET, instale el paquete de `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` y llame a `AddMessagePackProtocol` en `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-131">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="e6ecd-132">Esta llamada de `AddMessagePackProtocol` toma un delegado para configurar opciones como el servidor.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-132">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="e6ecd-133">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="e6ecd-133">JavaScript client</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e6ecd-134">El paquete de `@microsoft/signalr-protocol-msgpack` NPM proporciona compatibilidad con MessagePack para el cliente JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-134">MessagePack support for the JavaScript client is provided by the `@microsoft/signalr-protocol-msgpack` npm package.</span></span> <span data-ttu-id="e6ecd-135">Instale el paquete ejecutando el siguiente comando en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="e6ecd-135">Install the package by executing the following command in a command shell:</span></span>

```console
npm install @microsoft/signalr-protocol-msgpack
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e6ecd-136">El paquete de `@aspnet/signalr-protocol-msgpack` NPM proporciona compatibilidad con MessagePack para el cliente JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-136">MessagePack support for the JavaScript client is provided by the `@aspnet/signalr-protocol-msgpack` npm package.</span></span> <span data-ttu-id="e6ecd-137">Instale el paquete ejecutando el siguiente comando en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="e6ecd-137">Install the package by executing the following command in a command shell:</span></span>

```console
npm install @aspnet/signalr-protocol-msgpack
```

::: moniker-end

<span data-ttu-id="e6ecd-138">Después de instalar el paquete NPM, el módulo se puede usar directamente a través de un cargador de módulos de JavaScript o importarse en el explorador haciendo referencia al archivo siguiente:</span><span class="sxs-lookup"><span data-stu-id="e6ecd-138">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the following file:</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e6ecd-139">*node_modules\\@microsoft\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span><span class="sxs-lookup"><span data-stu-id="e6ecd-139">*node_modules\\@microsoft\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span></span> 

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e6ecd-140">*node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span><span class="sxs-lookup"><span data-stu-id="e6ecd-140">*node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*</span></span> 

::: moniker-end

<span data-ttu-id="e6ecd-141">En un explorador, también se debe hacer referencia a la biblioteca `msgpack5`.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-141">In a browser, the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="e6ecd-142">Use una etiqueta de `<script>` para crear una referencia.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-142">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="e6ecd-143">La biblioteca se puede encontrar en *node_modules \msgpack5\dist\msgpack5.js*.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-143">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="e6ecd-144">Al utilizar el elemento `<script>`, el orden es importante.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-144">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="e6ecd-145">Si se hace referencia a *signalr-Protocol-msgpack. js* antes de *msgpack5. js*, se produce un error al intentar conectarse con MessagePack.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-145">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="e6ecd-146">*signalr. js* también es necesario antes de *signalr-Protocol-msgpack. js*.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-146">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="e6ecd-147">Al agregar `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` al `HubConnectionBuilder`, se configurará el cliente para que use el protocolo MessagePack al conectarse a un servidor.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-147">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="e6ecd-148">En este momento, no hay ninguna opción de configuración para el protocolo MessagePack en el cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-148">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="messagepack-quirks"></a><span data-ttu-id="e6ecd-149">Peculiaridades de MessagePack</span><span class="sxs-lookup"><span data-stu-id="e6ecd-149">MessagePack quirks</span></span>

<span data-ttu-id="e6ecd-150">Hay algunos problemas que se deben tener en cuenta al usar el protocolo MessagePack Hub.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-150">There are a few issues to be aware of when using the MessagePack Hub Protocol.</span></span>

### <a name="messagepack-is-case-sensitive"></a><span data-ttu-id="e6ecd-151">MessagePack distingue entre mayúsculas y minúsculas</span><span class="sxs-lookup"><span data-stu-id="e6ecd-151">MessagePack is case-sensitive</span></span>

<span data-ttu-id="e6ecd-152">El protocolo MessagePack distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-152">The MessagePack protocol is case-sensitive.</span></span> <span data-ttu-id="e6ecd-153">Por ejemplo, considere la siguiente C# clase:</span><span class="sxs-lookup"><span data-stu-id="e6ecd-153">For example, consider the following C# class:</span></span>

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

<span data-ttu-id="e6ecd-154">Al enviar desde el cliente JavaScript, debe usar `PascalCased` nombres de propiedad, ya que las mayúsculas y C# minúsculas deben coincidir exactamente con la clase.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-154">When sending from the JavaScript client, you must use `PascalCased` property names, since the casing must match the C# class exactly.</span></span> <span data-ttu-id="e6ecd-155">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="e6ecd-155">For example:</span></span>

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

<span data-ttu-id="e6ecd-156">El uso de nombres de `camelCased` no se C# enlaza correctamente a la clase.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-156">Using `camelCased` names won't properly bind to the C# class.</span></span> <span data-ttu-id="e6ecd-157">Puede solucionar este paso mediante el atributo `Key` para especificar un nombre diferente para la propiedad MessagePack.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-157">You can work around this by using the `Key` attribute to specify a different name for the MessagePack property.</span></span> <span data-ttu-id="e6ecd-158">Para obtener más información, consulte [la documentación de MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp#object-serialization).</span><span class="sxs-lookup"><span data-stu-id="e6ecd-158">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp#object-serialization).</span></span>

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a><span data-ttu-id="e6ecd-159">DateTime. Kind no se conserva al serializar o deserializar</span><span class="sxs-lookup"><span data-stu-id="e6ecd-159">DateTime.Kind is not preserved when serializing/deserializing</span></span>

<span data-ttu-id="e6ecd-160">El protocolo MessagePack no proporciona una manera de codificar el valor `Kind` de un `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-160">The MessagePack protocol doesn't provide a way to encode the `Kind` value of a `DateTime`.</span></span> <span data-ttu-id="e6ecd-161">Como resultado, al deserializar una fecha, el protocolo MessagePack Hub supone que la fecha de entrada está en formato UTC.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-161">As a result, when deserializing a date, the MessagePack Hub Protocol assumes the incoming date is in UTC format.</span></span> <span data-ttu-id="e6ecd-162">Si está trabajando con `DateTime` valores en la hora local, se recomienda convertir a UTC antes de enviarlos.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-162">If you're working with `DateTime` values in local time, we recommend converting to UTC before sending them.</span></span> <span data-ttu-id="e6ecd-163">Conviértalos de la hora UTC a la hora local cuando los reciba.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-163">Convert them from UTC to local time when you receive them.</span></span>

<span data-ttu-id="e6ecd-164">Para obtener más información sobre esta limitación, vea el tema sobre el problema de GitHub [ASPNET/SignalR#2632](https://github.com/aspnet/SignalR/issues/2632).</span><span class="sxs-lookup"><span data-stu-id="e6ecd-164">For more information on this limitation, see GitHub issue [aspnet/SignalR#2632](https://github.com/aspnet/SignalR/issues/2632).</span></span>

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a><span data-ttu-id="e6ecd-165">No se admite DateTime. MinValue en MessagePack en JavaScript</span><span class="sxs-lookup"><span data-stu-id="e6ecd-165">DateTime.MinValue is not supported by MessagePack in JavaScript</span></span>

<span data-ttu-id="e6ecd-166">La biblioteca [msgpack5](https://github.com/mcollina/msgpack5) que usa el cliente SignalR JavaScript no admite el tipo de `timestamp96` en MessagePack.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-166">The [msgpack5](https://github.com/mcollina/msgpack5) library used by the SignalR JavaScript client doesn't support the `timestamp96` type in MessagePack.</span></span> <span data-ttu-id="e6ecd-167">Este tipo se usa para codificar valores de fecha muy grandes (ya sea muy temprano en el pasado o muy lejos en el futuro).</span><span class="sxs-lookup"><span data-stu-id="e6ecd-167">This type is used to encode very large date values (either very early in the past or very far in the future).</span></span> <span data-ttu-id="e6ecd-168">El valor de `DateTime.MinValue` es `January 1, 0001` que se debe codificar en un valor de `timestamp96`.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-168">The value of `DateTime.MinValue` is `January 1, 0001` which must be encoded in a `timestamp96` value.</span></span> <span data-ttu-id="e6ecd-169">Por este motivo, no se admite el envío de `DateTime.MinValue` a un cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-169">Because of this, sending `DateTime.MinValue` to a JavaScript client isn't supported.</span></span> <span data-ttu-id="e6ecd-170">Cuando el cliente JavaScript recibe `DateTime.MinValue`, se produce el siguiente error:</span><span class="sxs-lookup"><span data-stu-id="e6ecd-170">When `DateTime.MinValue` is received by the JavaScript client, the following error is thrown:</span></span>

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

<span data-ttu-id="e6ecd-171">Normalmente, `DateTime.MinValue` se usa para codificar un valor "Missing" o `null`.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-171">Usually, `DateTime.MinValue` is used to encode a "missing" or `null` value.</span></span> <span data-ttu-id="e6ecd-172">Si necesita codificar ese valor en MessagePack, use un valor de `DateTime` que acepte valores NULL (`DateTime?`) o codifique un valor de `bool` independiente que indique si la fecha está presente.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-172">If you need to encode that value in MessagePack, use a nullable `DateTime` value (`DateTime?`) or encode a separate `bool` value indicating if the date is present.</span></span>

<span data-ttu-id="e6ecd-173">Para obtener más información sobre esta limitación, vea el tema sobre el problema de GitHub [ASPNET/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228).</span><span class="sxs-lookup"><span data-stu-id="e6ecd-173">For more information on this limitation, see GitHub issue [aspnet/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228).</span></span>

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a><span data-ttu-id="e6ecd-174">Compatibilidad con MessagePack en el entorno de compilación "anterior a la hora"</span><span class="sxs-lookup"><span data-stu-id="e6ecd-174">MessagePack support in "ahead-of-time" compilation environment</span></span>

<span data-ttu-id="e6ecd-175">La biblioteca [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8) utilizada por el cliente y el servidor .net utiliza la generación de código para optimizar la serialización.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-175">The [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8) library used by the .NET client and server uses code generation to optimize serialization.</span></span> <span data-ttu-id="e6ecd-176">Como resultado, no se admite de forma predeterminada en los entornos que usan la compilación "anterior a la hora" (como Xamarin iOS o Unity).</span><span class="sxs-lookup"><span data-stu-id="e6ecd-176">As a result, it isn't supported by default on environments that use "ahead-of-time" compilation (such as Xamarin iOS or Unity).</span></span> <span data-ttu-id="e6ecd-177">Es posible usar MessagePack en estos entornos mediante la "generación previa" del código del serializador/deserializador.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-177">It's possible to use MessagePack in these environments by "pre-generating" the serializer/deserializer code.</span></span> <span data-ttu-id="e6ecd-178">Para obtener más información, consulte [la documentación de MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8#pre-code-generationunityxamarin-supports).</span><span class="sxs-lookup"><span data-stu-id="e6ecd-178">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8#pre-code-generationunityxamarin-supports).</span></span> <span data-ttu-id="e6ecd-179">Una vez que haya generado los serializadores previamente, puede registrarlos mediante el delegado de configuración que se pasa a `AddMessagePackProtocol`:</span><span class="sxs-lookup"><span data-stu-id="e6ecd-179">Once you have pre-generated the serializers, you can register them using the configuration delegate passed to `AddMessagePackProtocol`:</span></span>

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

### <a name="type-checks-are-more-strict-in-messagepack"></a><span data-ttu-id="e6ecd-180">Las comprobaciones de tipo son más estrictas en MessagePack</span><span class="sxs-lookup"><span data-stu-id="e6ecd-180">Type checks are more strict in MessagePack</span></span>

<span data-ttu-id="e6ecd-181">El protocolo del concentrador de JSON realizará las conversiones de tipos durante la deserialización.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-181">The JSON Hub Protocol will perform type conversions during deserialization.</span></span> <span data-ttu-id="e6ecd-182">Por ejemplo, si el objeto entrante tiene un valor de propiedad que es un número (`{ foo: 42 }`) pero la propiedad de la clase .NET es de tipo `string`, se convertirá el valor.</span><span class="sxs-lookup"><span data-stu-id="e6ecd-182">For example, if the incoming object has a property value that is a number (`{ foo: 42 }`) but the property on the .NET class is of type `string`, the value will be converted.</span></span> <span data-ttu-id="e6ecd-183">Sin embargo, MessagePack no realiza esta conversión y producirá una excepción que se puede observar en los registros del lado servidor (y en la consola):</span><span class="sxs-lookup"><span data-stu-id="e6ecd-183">However, MessagePack doesn't perform this conversion and will throw an exception that can be seen in server-side logs (and in the console):</span></span>

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

<span data-ttu-id="e6ecd-184">Para obtener más información sobre esta limitación, vea el tema sobre el problema de GitHub [ASPNET/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937).</span><span class="sxs-lookup"><span data-stu-id="e6ecd-184">For more information on this limitation, see GitHub issue [aspnet/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937).</span></span>

## <a name="related-resources"></a><span data-ttu-id="e6ecd-185">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="e6ecd-185">Related resources</span></span>

* [<span data-ttu-id="e6ecd-186">Primeros pasos</span><span class="sxs-lookup"><span data-stu-id="e6ecd-186">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="e6ecd-187">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="e6ecd-187">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="e6ecd-188">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="e6ecd-188">JavaScript client</span></span>](xref:signalr/javascript-client)
