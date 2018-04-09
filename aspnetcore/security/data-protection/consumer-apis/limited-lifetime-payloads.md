---
title: Limite la duración de las cargas protegidas en ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de cómo limitar la duración de una carga protegida mediante las API de protección de datos de ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 324887b3d29de989ad855c4e78fd5a235fdb560e
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2018
---
# <a name="limit-the-lifetime-of-protected-payloads-in-aspnet-core"></a>Limite la duración de las cargas protegidas en ASP.NET Core

Existen escenarios donde desea que el desarrollador de aplicaciones para crear una carga protegida que expira tras un período de tiempo establecido. Por ejemplo, la carga protegida podría representar un token de restablecimiento de contraseña que solo debe ser válido durante una hora. Es posible para el desarrollador puede crear su propio formato de carga que contiene una fecha de expiración incrustados, y los desarrolladores avanzados que desee hacerlo de todos modos, pero para la mayoría de los desarrolladores administrar estos caducidad puede crecer tedioso.

Para facilitar esta tarea para nuestros clientes de desarrollador, el paquete [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) contiene las API de la utilidad para crear cargas de expiran automáticamente tras un período de tiempo establecido. Estas API de bloqueo de la `ITimeLimitedDataProtector` tipo.

## <a name="api-usage"></a>Uso de la API

El `ITimeLimitedDataProtector` es la interfaz principal para proteger y desproteger cargas de tiempo limitado / expiración automática. Para crear una instancia de un `ITimeLimitedDataProtector`, primero necesitará una instancia de una normal [IDataProtector](xref:security/data-protection/consumer-apis/overview) construido con un propósito específico. Una vez el `IDataProtector` instancia esté disponible, llame a la `IDataProtector.ToTimeLimitedDataProtector` método de extensión para devolver un protector con capacidades integradas de expiración.

`ITimeLimitedDataProtector` expone los siguientes métodos de superficie y extensión de API:

* CreateProtector (propósito de cadena): ITimeLimitedDataProtector - esta API es similar a la existente `IDataProtectionProvider.CreateProtector` en que se puede utilizar para crear [finalidad cadenas](xref:security/data-protection/consumer-apis/purpose-strings) desde un protector de tiempo limitado de raíz.

* Proteger (byte [] texto simple, expiración de DateTimeOffset): byte]

* Proteger (texto simple de byte [], duración del intervalo de tiempo): byte]

* Proteger (como texto simple de byte []): byte]

* Proteger (texto simple de cadena, expiración de DateTimeOffset): cadena

* Proteger (texto simple de cadena, duración del intervalo de tiempo): cadena

* Proteger (texto simple de la cadena): cadena

Además de los principales `Protect` métodos que toman sólo el texto no cifrado, hay nuevas sobrecargas que permiten especificar la fecha de expiración de la carga. La fecha de expiración se puede especificar como una fecha absoluta (a través de un `DateTimeOffset`) o como una hora relativa (desde el sistema actual time y a través de un `TimeSpan`). Si se llama a una sobrecarga que no toma una expiración, se supone la carga nunca a punto de expirar.

* Desproteger (byte [] protectedData, out expiración DateTimeOffset): byte]

* Desproteger (byte [] protectedData): byte]

* Desproteger (cadena protectedData, out expiración DateTimeOffset): cadena

* Desproteger (cadena protectedData): cadena

El `Unprotect` métodos devuelven los datos protegidos originales. Si aún no ha expirado la carga, la expiración absoluta se devuelve como un parámetro junto con los datos protegidos originales de salida opcionales. Si la carga ha expirado, todas las sobrecargas del método Unprotect producirá CryptographicException.

>[!WARNING]
> No se recomienda usar estas API para proteger las cargas que requieren la persistencia a largo plazo o indefinida. "¿Permitirme para que las cargas protegidas ser irrecuperables permanentemente después de un mes?" puede actuar como una buena regla general; Si la respuesta es no a los desarrolladores a continuación, deben considerar las API alternativas.

El ejemplo siguiente utiliza la [las rutas de código no DI](xref:security/data-protection/configuration/non-di-scenarios) para crear instancias del sistema de protección de datos. Para ejecutar este ejemplo, asegúrese de que ha agregado una referencia al paquete Microsoft.AspNetCore.DataProtection.Extensions en primer lugar.

[!code-csharp[](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
