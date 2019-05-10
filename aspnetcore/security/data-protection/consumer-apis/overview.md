---
title: Información general sobre las API de consumidor para ASP.NET Core
author: rick-anderson
description: Recibir una breve descripción del consumidor diversas API disponibles dentro de la biblioteca de protección de datos de ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: b0d11d097ee2d448b6781f6fa84445f6400fbc76
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087553"
---
# <a name="consumer-apis-overview-for-aspnet-core"></a>Información general sobre las API de consumidor para ASP.NET Core

El `IDataProtectionProvider` y `IDataProtector` interfaces son las interfaces básicas a través del cual los consumidores usan el sistema de protección de datos. Se encuentran en el [Microsoft.AspNetCore.DataProtection.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/) paquete.

## <a name="idataprotectionprovider"></a>IDataProtectionProvider

La interfaz del proveedor representa la raíz del sistema de protección de datos. No puede utilizarse directamente para proteger o desproteger los datos. En su lugar, el consumidor debe obtener una referencia a un `IDataProtector` mediante una llamada a `IDataProtectionProvider.CreateProtector(purpose)`, donde el propósito es una cadena que describe el caso de uso previsto del consumidor. Consulte [cadenas de propósito](xref:security/data-protection/consumer-apis/purpose-strings) para mucha más información sobre la intención de este parámetro y cómo elegir un valor adecuado.

## <a name="idataprotector"></a>IDataProtector

La interfaz de protector devuelto por una llamada a `CreateProtector`y es esta interfaz que los consumidores pueden usar para realizar proteger y desproteger las operaciones.

Para proteger un elemento de datos, pase los datos a la `Protect` método. La interfaz básica define un método que convierte byte -> byte [], pero hay también una sobrecarga (proporcionada como un método de extensión) que convierte la cadena -> cadena. La seguridad de los dos métodos es idéntica; el desarrollador debe elegir cualquier sobrecarga es más conveniente para su caso de uso. Con independencia de la sobrecarga elegida, el valor devuelto por el proteja método ahora está protegido (cifrados y compatible con tecnologías de alteración) y la aplicación puede enviar a un cliente de confianza.

Para desproteger un dato protegido anteriormente, pasar los datos protegidos en el `Unprotect` método. (Hay byte []-basados en cadena y basado en sobrecargas para mayor comodidad del desarrollador.) Si se genera la carga protegida por una llamada anterior a `Protect` en este mismo `IDataProtector`, el `Unprotect` método devolverá la carga sin protección original. Si la carga protegida ha sido alterada o fue creada por otro `IDataProtector`, el `Unprotect` método producirá CryptographicException.

El concepto de la misma frente a diferentes `IDataProtector` ties realizar una copia en el concepto de propósito. Si dos `IDataProtector` instancias se generaron a partir de la misma raíz `IDataProtectionProvider` pero a través de las cadenas de propósito diferente en la llamada a `IDataProtectionProvider.CreateProtector`, a continuación, se consideran [diferentes protectores](xref:security/data-protection/consumer-apis/purpose-strings), y uno no podrá desproteger cargas de generan otros.

## <a name="consuming-these-interfaces"></a>Consumo de estas interfaces

Para un componente compatible con DI, el uso previsto es que el componente de tomar un `IDataProtectionProvider` parámetro en su constructor y que el sistema de DI ofrece este servicio automáticamente cuando se crea una instancia del componente.

> [!NOTE]
> Algunas aplicaciones (por ejemplo, las aplicaciones de consola o aplicaciones de ASP.NET 4.x) podrían no ser compatibles con DI por lo que no se puede usar el mecanismo descrito aquí. Para estos escenarios consulte la [escenarios que no son compatibles con DI](xref:security/data-protection/configuration/non-di-scenarios) documento para obtener más información sobre cómo obtener una instancia de un `IDataProtection` proveedor sin tener que pasar a través de DI.

El ejemplo siguiente muestra tres conceptos:

1. [Agregue el sistema de protección de datos](xref:security/data-protection/configuration/overview) al contenedor de servicios,

2. Uso de DI para recibir una instancia de un `IDataProtectionProvider`, y

3. Creación de un `IDataProtector` desde un `IDataProtectionProvider` y usarlo para proteger y desproteger los datos.

[!code-csharp[](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

El paquete Microsoft.AspNetCore.DataProtection.Abstractions contiene un método de extensión `IServiceProvider.GetDataProtector` como una comodidad del desarrollador. Encapsula una única operación tanto al recuperar un `IDataProtectionProvider` desde el proveedor de servicios y llamar al método `IDataProtectionProvider.CreateProtector`. El ejemplo siguiente muestra su uso.

[!code-csharp[](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> Las instancias de `IDataProtectionProvider` y `IDataProtector` son seguras para subprocesos para varios de los llamadores. Se ha diseñado que una vez que un componente obtiene una referencia a un `IDataProtector` mediante una llamada a `CreateProtector`, usará esa referencia para varias llamadas a `Protect` y `Unprotect`. Una llamada a `Unprotect` generará CryptographicException si la carga protegida no se puede comprobar o descifrar. Algunos componentes que desee omitir los errores durante desproteger operaciones; un componente que lee las cookies de autenticación podría controlar este error y tratar la solicitud como si no tuviera ninguna cookie en todo lugar producirá un error en la solicitud completa. Los componentes que desean este comportamiento deben detectar específicamente CryptographicException en lugar de admitir todas las excepciones.
