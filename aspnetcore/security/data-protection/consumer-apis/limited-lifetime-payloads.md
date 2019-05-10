---
title: Limite la duración de cargas protegidas en ASP.NET Core
author: rick-anderson
description: Aprenda a limitar la duración de una carga protegida mediante las API de protección de datos de ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 8dc3b856ec67477ec8ae777749c9bf3107eb4eda
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897122"
---
# <a name="limit-the-lifetime-of-protected-payloads-in-aspnet-core"></a>Limite la duración de cargas protegidas en ASP.NET Core

Hay escenarios donde el desarrollador de aplicaciones desea crear una carga protegida que expira después de un período de tiempo establecido. Por ejemplo, la carga protegida podría representar un token de restablecimiento de contraseña que solo debe ser válido durante una hora. Es ciertamente posible para el desarrollador crear su propio formato de carga que contiene una fecha de expiración incrustados, y los desarrolladores avanzados que desee hacer esto de todos modos, pero para la mayoría de los desarrolladores administrar la caducidad de estas puede crecer tedioso.

Para facilitar esta tarea para nuestra audiencia de desarrolladores, el paquete [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) contiene API de la utilidad para crear cargas que expiran automáticamente tras un período de tiempo establecido. Estas API de bloqueo de la `ITimeLimitedDataProtector` tipo.

## <a name="api-usage"></a>Uso de la API

El `ITimeLimitedDataProtector` es la interfaz principal para proteger y desproteger las cargas de tiempo limitado / expiración automática. Para crear una instancia de un `ITimeLimitedDataProtector`, primero tendrá que una instancia de normal [IDataProtector](xref:security/data-protection/consumer-apis/overview) construido con un propósito específico. Una vez el `IDataProtector` instancia está disponible, llame a la `IDataProtector.ToTimeLimitedDataProtector` método de extensión para obtener un protector de vuelta con capacidades integradas de expiración.

`ITimeLimitedDataProtector` expone los métodos de extensión y de superficie de API siguientes:

* CreateProtector (fin de cadena): ITimeLimitedDataProtector - esta API es similar a la existente `IDataProtectionProvider.CreateProtector` en que se puede usar para crear [finalidad cadenas](xref:security/data-protection/consumer-apis/purpose-strings) desde un protector de tiempo limitado de raíz.

* Proteger (byte [] texto simple, expiración DateTimeOffset): byte]

* Proteger (texto simple de byte [], duración del intervalo de tiempo): byte]

* Proteger (como texto simple de byte []): byte]

* Proteger (texto simple de cadena, expiración DateTimeOffset): cadena

* Proteger (texto simple de cadena, duración del intervalo de tiempo): cadena

* Proteger (texto simple de cadena): cadena

Además de los principales `Protect` métodos que admiten solo el texto sin formato, hay nuevas sobrecargas que permiten especificar la fecha de expiración de la carga. La fecha de expiración se puede especificar como una fecha absoluta (a través de un `DateTimeOffset`) o como una hora relativa (desde el sistema actual tiempo a través de un `TimeSpan`). Si se llama a una sobrecarga que no tiene una fecha de expiración, la carga se supone que nunca expiran.

* Desproteger (byte [] protectedData, out DateTimeOffset expiración): byte]

* Desproteger (byte [] protectedData): byte]

* Desproteger (cadena protectedData, out DateTimeOffset expiración): cadena

* Desproteger (cadena protectedData): cadena

El `Unprotect` métodos devuelven los datos originales sin protección. Si aún no ha expirado la carga, la expiración absoluta se devuelve como un parámetro junto con los datos originales sin protección de salida opcionales. Si la carga ha caducado, todas las sobrecargas del método Unprotect producirá CryptographicException.

>[!WARNING]
> No se recomienda utilizar estas API para proteger las cargas que requieren la persistencia a largo plazo o indefinida. "¿Puedo permitirme para que las cargas protegidas que recuperarse después de un mes?" puede actuar como una buena regla general; Si la respuesta es no a los desarrolladores a continuación, considere la API alternativas.

El ejemplo siguiente se usa el [las rutas de código que no sean de DI](xref:security/data-protection/configuration/non-di-scenarios) para instanciar el sistema de protección de datos. Para ejecutar este ejemplo, asegúrese de que primero ha agregado una referencia al paquete Microsoft.AspNetCore.DataProtection.Extensions.

[!code-csharp[](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
