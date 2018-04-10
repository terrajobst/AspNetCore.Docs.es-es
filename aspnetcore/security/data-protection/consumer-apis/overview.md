---
title: Información general de las API de consumidor para ASP.NET Core
author: rick-anderson
description: Recibir una breve introducción del consumidor diversas API disponibles dentro de la biblioteca de protección de datos de ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: 5d161ed8fbc39bcf4a970644480b4e909810b555
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2018
---
# <a name="consumer-apis-overview-for-aspnet-core"></a>Información general de las API de consumidor para ASP.NET Core

El `IDataProtectionProvider` y `IDataProtector` interfaces son las interfaces básicas a través del cual los consumidores utilizan el sistema de protección de datos. Se encuentran en el [Microsoft.AspNetCore.DataProtection.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/) paquete.

## <a name="idataprotectionprovider"></a>IDataProtectionProvider

La interfaz del proveedor representa la raíz del sistema de protección de datos. Directamente no puede usarse para proteger o desproteger los datos. En su lugar, el consumidor debe obtener una referencia a un `IDataProtector` mediante una llamada a `IDataProtectionProvider.CreateProtector(purpose)`, donde el propósito es una cadena que describe el caso de uso previsto del consumidor. Vea [propósito cadenas](xref:security/data-protection/consumer-apis/purpose-strings) para mucho más información sobre la intención de este parámetro y cómo elegir un valor adecuado.

## <a name="idataprotector"></a>IDataProtector

La interfaz de protector devuelto por una llamada a `CreateProtector`y de esta interfaz que los consumidores pueden usar para realizar proteger y desproteger las operaciones.

Para proteger un elemento de datos, pasar los datos a la `Protect` método. La interfaz básica define un método que byte convierte [] -> byte [], pero no hay también una sobrecarga (proporcionada como un método de extensión) que convierte la cadena -> cadena. La seguridad proporcionada por los dos métodos es idéntica; el desarrollador debe elegir cualquier sobrecarga es más conveniente para sus casos de uso. Con independencia de la sobrecarga elegida, el valor devuelto por el proteja método ahora está protegido (descifra y compatible con tecnologías de manipulaciones) y la aplicación puede enviar a un cliente no es de confianza.

Para desproteger un elemento de datos protegidos anteriormente, pasar los datos protegidos en el `Unprotect` método. (Hay byte []-basada en cadena y basados en sobrecargas para mayor comodidad de desarrollador.) Si la carga protegida fue generada por una llamada anterior a `Protect` en esta misma `IDataProtector`, el `Unprotect` método devolverá la carga sin protección original. Si la carga protegida se ha alterado o ha sido creada por otro `IDataProtector`, el `Unprotect` método producirá CryptographicException.

El concepto de misma frente a diferentes `IDataProtector` ties hacia el concepto de propósito. Si dos `IDataProtector` instancias se generaron desde la misma raíz `IDataProtectionProvider` , pero a través de las cadenas de propósito diferente en la llamada a `IDataProtectionProvider.CreateProtector`, considera que está [diferentes protectores](xref:security/data-protection/consumer-apis/purpose-strings), y uno no podrá desproteger cargas generados por el otro.

## <a name="consuming-these-interfaces"></a>Consumir estas interfaces

Para un componente compatible con DI, el uso previsto es que el componente que pase un `IDataProtectionProvider` parámetro en su constructor y compruebe que el sistema DI automáticamente proporciona este servicio cuando se crea una instancia del componente.

> [!NOTE]
> Algunas aplicaciones (por ejemplo, las aplicaciones de consola o aplicaciones de ASP.NET 4.x) podrían no ser DI compatible con por lo que no se puede usar el mecanismo descrito aquí. Para consultar estos escenarios el [no escenarios DI](xref:security/data-protection/configuration/non-di-scenarios) documento para obtener más información sobre cómo obtener una instancia de un `IDataProtection` proveedor sin pasar por DI.

El siguiente ejemplo muestra tres conceptos:

1. [Agregue el sistema de protección de datos](xref:security/data-protection/configuration/overview) al contenedor de servicios,

2. Usar DI para recibir una instancia de un `IDataProtectionProvider`, y

3. Crear un `IDataProtector` desde un `IDataProtectionProvider` y usarla para proteger y desproteger los datos.

[!code-csharp[](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

El paquete Microsoft.AspNetCore.DataProtection.Abstractions contiene un método de extensión `IServiceProvider.GetDataProtector` como una comodidad para desarrolladores. Encapsula como una única operación ambos recuperar un `IDataProtectionProvider` desde el proveedor de servicios y llamar al método `IDataProtectionProvider.CreateProtector`. El ejemplo siguiente muestra su uso.

[!code-csharp[](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> Instancias de `IDataProtectionProvider` y `IDataProtector` son seguras para subprocesos para varios de los llamadores. Se ha diseñado que una vez que un componente obtiene una referencia a un `IDataProtector` mediante una llamada a `CreateProtector`, utilizará dicha referencia para varias llamadas a `Protect` y `Unprotect`. Una llamada a `Unprotect` iniciará CryptographicException si no se puede comprobar o descifrar la carga protegida. Algunos componentes que desee omitir los errores durante la desproteger operaciones; un componente que lee las cookies de autenticación puede controlar este error y tratar la solicitud como si no tuviera en absoluto ninguna cookie en lugar de producirá un error en la solicitud directamente. Los componentes que desea este comportamiento deben detectar específicamente CryptographicException en lugar de admitir todas las excepciones.
