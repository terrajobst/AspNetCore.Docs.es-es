---
title: Consideraciones de diseño de API de SignalR
author: anurse
description: Obtenga información sobre cómo diseñar SignalR APIs para la compatibilidad entre versiones de la aplicación.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 11/06/2018
uid: signalr/api-design
ms.openlocfilehash: 3f17bf055b793e8fc91fbcc15f668928ca261f77
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897812"
---
# <a name="signalr-api-design-considerations"></a><span data-ttu-id="20b57-103">Consideraciones de diseño de API de SignalR</span><span class="sxs-lookup"><span data-stu-id="20b57-103">SignalR API design considerations</span></span>

<span data-ttu-id="20b57-104">Por [Andrew Stanton-Nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="20b57-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="20b57-105">Este artículo proporcionan instrucciones para la creación de API basadas en SignalR.</span><span class="sxs-lookup"><span data-stu-id="20b57-105">This article provides guidance for building SignalR-based APIs.</span></span>

## <a name="use-custom-object-parameters-to-ensure-backwards-compatibility"></a><span data-ttu-id="20b57-106">Usar parámetros de objeto personalizado para garantizar la compatibilidad con versiones anteriores</span><span class="sxs-lookup"><span data-stu-id="20b57-106">Use custom object parameters to ensure backwards-compatibility</span></span>

<span data-ttu-id="20b57-107">Agregar parámetros a un método de concentrador SignalR (en el cliente o servidor) es un *cambio brusco*.</span><span class="sxs-lookup"><span data-stu-id="20b57-107">Adding parameters to a SignalR hub method (on either the client or the server) is a *breaking change*.</span></span> <span data-ttu-id="20b57-108">Esto significa que los clientes y servidores más antiguos se producirán errores al intentar invocar el método sin el número apropiado de parámetros.</span><span class="sxs-lookup"><span data-stu-id="20b57-108">This means older clients/servers will get errors when they try to invoke the method without the appropriate number of parameters.</span></span> <span data-ttu-id="20b57-109">Sin embargo, agregar propiedades a un parámetro de objeto personalizado es **no** un cambio importante.</span><span class="sxs-lookup"><span data-stu-id="20b57-109">However, adding properties to a custom object parameter is **not** a breaking change.</span></span> <span data-ttu-id="20b57-110">Esto puede usarse para diseñar las API compatibles que sean resistentes a los cambios en el cliente o el servidor.</span><span class="sxs-lookup"><span data-stu-id="20b57-110">This can be used to design compatible APIs that are resilient to changes on the client or the server.</span></span>

<span data-ttu-id="20b57-111">Por ejemplo, considere la posibilidad de una API de servidor similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="20b57-111">For example, consider a server-side API like the following:</span></span>

[!code-csharp[ParameterBasedOldVersion](api-design/sample/Samples.cs?name=ParameterBasedOldVersion)]

<span data-ttu-id="20b57-112">El cliente de JavaScript llama a este método con `invoke` como sigue:</span><span class="sxs-lookup"><span data-stu-id="20b57-112">The JavaScript client calls this method using `invoke` as follows:</span></span>

[!code-typescript[CallWithOneParameter](api-design/sample/Samples.ts?name=CallWithOneParameter)]

<span data-ttu-id="20b57-113">Si posteriormente agrega un segundo parámetro para el método de servidor, los clientes más antiguos no proporcionan este valor de parámetro.</span><span class="sxs-lookup"><span data-stu-id="20b57-113">If you later add a second parameter to the server method, older clients won't provide this parameter value.</span></span> <span data-ttu-id="20b57-114">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="20b57-114">For example:</span></span>

[!code-csharp[ParameterBasedNewVersion](api-design/sample/Samples.cs?name=ParameterBasedNewVersion)]

<span data-ttu-id="20b57-115">Cuando el cliente anterior intenta invocar este método, producirá un error similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="20b57-115">When the old client tries to invoke this method, it will get an error like this:</span></span>

```
Microsoft.AspNetCore.SignalR.HubException: Failed to invoke 'GetTotalLength' due to an error on the server.
```

<span data-ttu-id="20b57-116">En el servidor, verá un mensaje de registro similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="20b57-116">On the server, you'll see a log message like this:</span></span>

```
System.IO.InvalidDataException: Invocation provides 1 argument(s) but target expects 2.
```

<span data-ttu-id="20b57-117">El cliente anterior sólo envía un parámetro, pero la API más reciente del servidor requiere dos parámetros.</span><span class="sxs-lookup"><span data-stu-id="20b57-117">The old client only sent one parameter, but the newer server API required two parameters.</span></span> <span data-ttu-id="20b57-118">Usar objetos personalizados como parámetros, proporciona más flexibilidad.</span><span class="sxs-lookup"><span data-stu-id="20b57-118">Using custom objects as parameters gives you more flexibility.</span></span> <span data-ttu-id="20b57-119">Vamos a volver a diseñar la API original para usar un objeto personalizado:</span><span class="sxs-lookup"><span data-stu-id="20b57-119">Let's redesign the original API to use a custom object:</span></span>

[!code-csharp[ObjectBasedOldVersion](api-design/sample/Samples.cs?name=ObjectBasedOldVersion)]

<span data-ttu-id="20b57-120">Ahora, el cliente utiliza un objeto para llamar al método:</span><span class="sxs-lookup"><span data-stu-id="20b57-120">Now, the client uses an object to call the method:</span></span>

[!code-typescript[CallWithObject](api-design/sample/Samples.ts?name=CallWithObject)]

<span data-ttu-id="20b57-121">En lugar de agregar un parámetro, agregue una propiedad a la `TotalLengthRequest` objeto:</span><span class="sxs-lookup"><span data-stu-id="20b57-121">Instead of adding a parameter, add a property to the `TotalLengthRequest` object:</span></span>

[!code-csharp[ObjectBasedNewVersion](api-design/sample/Samples.cs?name=ObjectBasedNewVersion&highlight=4,9-13)]

<span data-ttu-id="20b57-122">Cuando el cliente anterior envía un solo parámetro, el archivo extra `Param2` propiedad quedará `null`.</span><span class="sxs-lookup"><span data-stu-id="20b57-122">When the old client sends a single parameter, the extra `Param2` property will be left `null`.</span></span> <span data-ttu-id="20b57-123">Puede detectar un mensaje enviado por un cliente anterior mediante la comprobación de la `Param2` para `null` y aplicar un valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="20b57-123">You can detect a message sent by an older client by checking the `Param2` for `null` and apply a default value.</span></span> <span data-ttu-id="20b57-124">Un nuevo cliente puede enviar los dos parámetros.</span><span class="sxs-lookup"><span data-stu-id="20b57-124">A new client can send both parameters.</span></span>

[!code-typescript[CallWithObjectNew](api-design/sample/Samples.ts?name=CallWithObjectNew)]

<span data-ttu-id="20b57-125">Esta misma técnica funciona para los métodos definidos en el cliente.</span><span class="sxs-lookup"><span data-stu-id="20b57-125">The same technique works for methods defined on the client.</span></span> <span data-ttu-id="20b57-126">Puede enviar un objeto personalizado desde el lado del servidor:</span><span class="sxs-lookup"><span data-stu-id="20b57-126">You can send a custom object from the server side:</span></span>

[!code-csharp[ClientSideObjectBasedOld](api-design/sample/Samples.cs?name=ClientSideObjectBasedOld)]

<span data-ttu-id="20b57-127">En el lado cliente, tener acceso a la `Message` propiedad en lugar de utilizar un parámetro:</span><span class="sxs-lookup"><span data-stu-id="20b57-127">On the client side, you access the `Message` property rather than using a parameter:</span></span>

[!code-typescript[OnWithObjectOld](api-design/sample/Samples.ts?name=OnWithObjectOld)]

<span data-ttu-id="20b57-128">Si más adelante decide agregar el remitente del mensaje a la carga, agregar una propiedad al objeto:</span><span class="sxs-lookup"><span data-stu-id="20b57-128">If you later decide to add the sender of the message to the payload, add a property to the object:</span></span>

[!code-csharp[ClientSideObjectBasedNew](api-design/sample/Samples.cs?name=ClientSideObjectBasedNew&highlight=5)]

<span data-ttu-id="20b57-129">Los clientes más antiguos no se espera el `Sender` valor, por lo que podrá omitirlo.</span><span class="sxs-lookup"><span data-stu-id="20b57-129">The older clients won't be expecting the `Sender` value, so they'll ignore it.</span></span> <span data-ttu-id="20b57-130">Un nuevo cliente puede aceptarla mediante la actualización para leer la nueva propiedad:</span><span class="sxs-lookup"><span data-stu-id="20b57-130">A new client can accept it by updating to read the new property:</span></span>

[!code-typescript[OnWithObjectNew](api-design/sample/Samples.ts?name=OnWithObjectNew&highlight=2-5)]

<span data-ttu-id="20b57-131">En este caso, el nuevo cliente también es tolerante a errores de un servidor antiguo que no proporciona la `Sender` valor.</span><span class="sxs-lookup"><span data-stu-id="20b57-131">In this case, the new client is also tolerant of an old server that doesn't provide the `Sender` value.</span></span> <span data-ttu-id="20b57-132">Dado que el servidor anterior no proporciona la `Sender` valor, el cliente comprueba si existe antes de acceder a él.</span><span class="sxs-lookup"><span data-stu-id="20b57-132">Since the old server won't provide the `Sender` value, the client checks to see if it exists before accessing it.</span></span>
