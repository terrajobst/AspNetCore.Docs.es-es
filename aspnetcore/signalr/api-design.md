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
# <a name="signalr-api-design-considerations"></a>Consideraciones de diseño de API de SignalR

Por [Andrew Stanton-Nurse](https://twitter.com/anurse)

Este artículo proporcionan instrucciones para la creación de API basadas en SignalR.

## <a name="use-custom-object-parameters-to-ensure-backwards-compatibility"></a>Usar parámetros de objeto personalizado para garantizar la compatibilidad con versiones anteriores

Agregar parámetros a un método de concentrador SignalR (en el cliente o servidor) es un *cambio brusco*. Esto significa que los clientes y servidores más antiguos se producirán errores al intentar invocar el método sin el número apropiado de parámetros. Sin embargo, agregar propiedades a un parámetro de objeto personalizado es **no** un cambio importante. Esto puede usarse para diseñar las API compatibles que sean resistentes a los cambios en el cliente o el servidor.

Por ejemplo, considere la posibilidad de una API de servidor similar al siguiente:

[!code-csharp[ParameterBasedOldVersion](api-design/sample/Samples.cs?name=ParameterBasedOldVersion)]

El cliente de JavaScript llama a este método con `invoke` como sigue:

[!code-typescript[CallWithOneParameter](api-design/sample/Samples.ts?name=CallWithOneParameter)]

Si posteriormente agrega un segundo parámetro para el método de servidor, los clientes más antiguos no proporcionan este valor de parámetro. Por ejemplo:

[!code-csharp[ParameterBasedNewVersion](api-design/sample/Samples.cs?name=ParameterBasedNewVersion)]

Cuando el cliente anterior intenta invocar este método, producirá un error similar al siguiente:

```
Microsoft.AspNetCore.SignalR.HubException: Failed to invoke 'GetTotalLength' due to an error on the server.
```

En el servidor, verá un mensaje de registro similar al siguiente:

```
System.IO.InvalidDataException: Invocation provides 1 argument(s) but target expects 2.
```

El cliente anterior sólo envía un parámetro, pero la API más reciente del servidor requiere dos parámetros. Usar objetos personalizados como parámetros, proporciona más flexibilidad. Vamos a volver a diseñar la API original para usar un objeto personalizado:

[!code-csharp[ObjectBasedOldVersion](api-design/sample/Samples.cs?name=ObjectBasedOldVersion)]

Ahora, el cliente utiliza un objeto para llamar al método:

[!code-typescript[CallWithObject](api-design/sample/Samples.ts?name=CallWithObject)]

En lugar de agregar un parámetro, agregue una propiedad a la `TotalLengthRequest` objeto:

[!code-csharp[ObjectBasedNewVersion](api-design/sample/Samples.cs?name=ObjectBasedNewVersion&highlight=4,9-13)]

Cuando el cliente anterior envía un solo parámetro, el archivo extra `Param2` propiedad quedará `null`. Puede detectar un mensaje enviado por un cliente anterior mediante la comprobación de la `Param2` para `null` y aplicar un valor predeterminado. Un nuevo cliente puede enviar los dos parámetros.

[!code-typescript[CallWithObjectNew](api-design/sample/Samples.ts?name=CallWithObjectNew)]

Esta misma técnica funciona para los métodos definidos en el cliente. Puede enviar un objeto personalizado desde el lado del servidor:

[!code-csharp[ClientSideObjectBasedOld](api-design/sample/Samples.cs?name=ClientSideObjectBasedOld)]

En el lado cliente, tener acceso a la `Message` propiedad en lugar de utilizar un parámetro:

[!code-typescript[OnWithObjectOld](api-design/sample/Samples.ts?name=OnWithObjectOld)]

Si más adelante decide agregar el remitente del mensaje a la carga, agregar una propiedad al objeto:

[!code-csharp[ClientSideObjectBasedNew](api-design/sample/Samples.cs?name=ClientSideObjectBasedNew&highlight=5)]

Los clientes más antiguos no se espera el `Sender` valor, por lo que podrá omitirlo. Un nuevo cliente puede aceptarla mediante la actualización para leer la nueva propiedad:

[!code-typescript[OnWithObjectNew](api-design/sample/Samples.ts?name=OnWithObjectNew&highlight=2-5)]

En este caso, el nuevo cliente también es tolerante a errores de un servidor antiguo que no proporciona la `Sender` valor. Dado que el servidor anterior no proporciona la `Sender` valor, el cliente comprueba si existe antes de acceder a él.
