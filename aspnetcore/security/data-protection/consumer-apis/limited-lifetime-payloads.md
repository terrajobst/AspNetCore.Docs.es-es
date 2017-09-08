---
title: "Limita la duración de cargas protegidos"
author: rick-anderson
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 000175e2-10fc-43dd-bfc2-51e004b97b44
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 4ff13803b328c1e9dd2934c38c88b43f5798de03
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="limiting-the-lifetime-of-protected-payloads"></a>Limita la duración de cargas protegidos

Existen escenarios donde desea que el desarrollador de aplicaciones para crear una carga protegida que expira tras un período de tiempo establecido. Por ejemplo, la carga protegida podría representar un token de restablecimiento de contraseña que solo debe ser válido durante una hora. Es posible para el desarrollador puede crear su propio formato de carga que contiene una fecha de expiración incrustados, y los desarrolladores avanzados que desee hacerlo de todos modos, pero para la mayoría de los desarrolladores administrar estos caducidad puede crecer tedioso.

Para facilitar esta tarea para la audiencia de desarrollador, el paquete Microsoft.AspNetCore.DataProtection.Extensions contiene API de la utilidad para crear cargas de expiran automáticamente tras un período de tiempo establecido. Estas API de bloqueo en el tipo de ITimeLimitedDataProtector.

## <a name="api-usage"></a>Uso de la API

La interfaz de ITimeLimitedDataProtector es la interfaz básica para proteger y desproteger tiempo limitado / caduca automáticamente cargas. Para crear una instancia de un ITimeLimitedDataProtector, necesitará primero una instancia de una normal [IDataProtector](overview.md) construido con un propósito específico. Una vez que la instancia IDataProtector está disponible, llame al método de extensión IDataProtector.ToTimeLimitedDataProtector para devolver un protector con capacidades integradas de expiración.

ITimeLimitedDataProtector expone los siguientes métodos de superficie y extensión de API:

* CreateProtector (propósito de cadena): ITimeLimitedDataProtector esta API es similar a la existente IDataProtectionProvider.CreateProtector en que se puede utilizar para crear [finalidad cadenas](purpose-strings.md) desde un protector de tiempo limitado de raíz.

* Proteger (byte [] texto simple, expiración de DateTimeOffset): byte]

* Proteger (texto simple de byte [], duración del intervalo de tiempo): byte]

* Proteger (como texto simple de byte []): byte]

* Proteger (texto simple de cadena, expiración de DateTimeOffset): cadena

* Proteger (texto simple de cadena, duración del intervalo de tiempo): cadena

* Proteger (texto simple de la cadena): cadena

Además de los métodos de proteger principales que tienen sólo el texto no cifrado, hay nuevas sobrecargas que permiten especificar la fecha de expiración de la carga. La fecha de expiración puede especificarse como una fecha absoluta (a través de un valor DateTimeOffset) o como una hora relativa (desde la hora actual del sistema, a través de un intervalo de tiempo). Si se llama a una sobrecarga que no toma una expiración, se supone la carga nunca a punto de expirar.

* Desproteger (byte [] protectedData, out expiración DateTimeOffset): byte]

* Desproteger (byte [] protectedData): byte]

* Desproteger (cadena protectedData, out expiración DateTimeOffset): cadena

* Desproteger (cadena protectedData): cadena

Los métodos de Unprotect devuelven los datos protegidos originales. Si aún no ha expirado la carga, la expiración absoluta se devuelve como un parámetro junto con los datos protegidos originales de salida opcionales. Si la carga ha expirado, todas las sobrecargas del método Unprotect producirá CryptographicException.

>[!WARNING]
> No se recomienda usar estas API para proteger las cargas que requieren la persistencia a largo plazo o indefinida. "¿Permitirme para que las cargas protegidas ser irrecuperables permanentemente después de un mes?" puede actuar como una buena regla general; Si la respuesta es no a los desarrolladores a continuación, deben considerar las API alternativas.

El ejemplo siguiente utiliza la [las rutas de código no DI](../configuration/non-di-scenarios.md) para crear instancias del sistema de protección de datos. Para ejecutar este ejemplo, asegúrese de que ha agregado una referencia al paquete Microsoft.AspNetCore.DataProtection.Extensions en primer lugar.

[!code-none[Main](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
