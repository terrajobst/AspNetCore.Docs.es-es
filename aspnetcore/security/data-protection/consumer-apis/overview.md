---
title: "Información general de las API de consumidor"
author: rick-anderson
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: f69beb9d-a519-43a8-857c-f6b01886a903
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: d23a6ce50eef71f393124b9420f4ba473904d8b4
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="consumer-apis-overview"></a>Información general de las API de consumidor

Las interfaces IDataProtectionProvider y IDataProtector son las interfaces básicas a través del cual los consumidores utilizan el sistema de protección de datos. Se encuentran en el paquete Microsoft.AspNetCore.DataProtection.Abstractions.

## <a name="idataprotectionprovider"></a>IDataProtectionProvider

La interfaz del proveedor representa la raíz del sistema de protección de datos. Directamente no puede usarse para proteger o desproteger los datos. En su lugar, el consumidor debe obtener una referencia a un IDataProtector mediante una llamada a IDataProtectionProvider.CreateProtector(purpose), donde el propósito es una cadena que describe el caso de uso previsto del consumidor. Vea [propósito cadenas](purpose-strings.md) para mucho más información sobre la intención de este parámetro y cómo elegir un valor adecuado.

## <a name="idataprotector"></a>IDataProtector

La interfaz de protector devuelto por una llamada a CreateProtector, y es esta interfaz que los consumidores pueden usar para realizar proteger y desproteger las operaciones.

Para proteger un elemento de datos, pase los datos para el método de protección. La interfaz básica define un método que byte convierte [] -> byte [], pero también hay una sobrecarga (proporcionada como un método de extensión) que convierte la cadena -> cadena. La seguridad proporcionada por los dos métodos es idéntica; el desarrollador debe elegir cualquier sobrecarga es más conveniente para sus casos de uso. Con independencia de la sobrecarga elegida, el valor devuelto por el proteja método ahora está protegido (descifra y compatible con tecnologías de manipulaciones) y la aplicación puede enviar a un cliente no es de confianza.

Para desproteger un elemento de datos protegidos anteriormente, pasar los datos protegidos para el método Unprotect. (Hay byte []-basada en cadena y basados en sobrecargas para mayor comodidad de desarrollador.) Si la carga protegida fue generada por una llamada anterior a proteger en este mismo IDataProtector, el método Unprotect devolverá la carga sin protección original. Si la carga protegida se ha alterado o ha sido creada por un IDataProtector diferentes, el método Unprotect producirá CryptographicException.

El concepto de misma frente a diferentes IDataProtector une hacia el concepto de propósito. Si dos instancias de IDataProtector generados desde la misma raíz IDataProtectionProvider pero a través de las cadenas de propósito diferente en la llamada a IDataProtectionProvider.CreateProtector, se consideran [diferentes protectores](purpose-strings.md), y uno no podrá desproteger cargas generados por el otro.

## <a name="consuming-these-interfaces"></a>Consumir estas interfaces

Para un componente compatible con DI, el uso previsto es que el componente toma un parámetro de IDataProtectionProvider en su constructor y compruebe que el sistema DI automáticamente proporciona este servicio cuando se crea una instancia del componente.

> [!NOTE]
> Algunas aplicaciones (por ejemplo, las aplicaciones de consola o aplicaciones de ASP.NET 4.x) podrían no ser DI compatible con por lo que no se puede usar el mecanismo descrito aquí. Para consultar estos escenarios el [no escenarios DI](../configuration/non-di-scenarios.md) documento para obtener más información sobre cómo obtener una instancia de un proveedor de IDataProtection sin pasar por DI.

El siguiente ejemplo muestra tres conceptos:

1. [Agregar el sistema de protección de datos](../configuration/overview.md) al contenedor de servicios,

2. Usar DI para recibir una instancia de un IDataProtectionProvider, y

3. Crear un IDataProtector desde un IDataProtectionProvider y usarla para proteger y desproteger los datos.

[!code-csharp[Main](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

El paquete Microsoft.AspNetCore.DataProtection.Abstractions contiene un método de extensión IServiceProvider.GetDataProtector como una comodidad para desarrolladores. Encapsula como una única operación tanto recuperar un IDataProtectionProvider desde el proveedor de servicios y una llamada a IDataProtectionProvider.CreateProtector. El ejemplo siguiente muestra su uso.

[!code-csharp[Main](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> Las instancias de IDataProtectionProvider y IDataProtector son seguras para subprocesos para distintos llamadores. Se pretende que una vez que un componente obtiene una referencia a un IDataProtector mediante una llamada a CreateProtector, utilizará dicha referencia para varias llamadas a proteger y llamada de Unprotect.A a Unprotect producirá CryptographicException si la carga protegida no se puede comprobar o descifrarse. Algunos componentes que desee omitir los errores durante la desproteger operaciones; un componente que lee las cookies de autenticación puede controlar este error y tratar la solicitud como si no tuviera en absoluto ninguna cookie en lugar de producirá un error en la solicitud directamente. Los componentes que desea este comportamiento deben detectar específicamente CryptographicException en lugar de admitir todas las excepciones.
