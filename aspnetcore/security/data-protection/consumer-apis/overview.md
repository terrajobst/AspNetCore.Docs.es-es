---
title: Información general sobre las API de consumidor para ASP.NET Core
author: rick-anderson
description: Reciba una breve descripción de las distintas API de consumidor disponibles en la ASP.NET Core biblioteca de protección de datos.
ms.author: riande
ms.date: 06/11/2019
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: ff9badb55813cae0aa72d3a95dc53792332f109b
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78654587"
---
# <a name="consumer-apis-overview-for-aspnet-core"></a>Información general sobre las API de consumidor para ASP.NET Core

Las interfaces `IDataProtectionProvider` y `IDataProtector` son las interfaces básicas a través de las cuales los consumidores usan el sistema de protección de datos. Se encuentran en el paquete [Microsoft. AspNetCore. Desproteccion. Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/) .

## <a name="idataprotectionprovider"></a>IDataProtectionProvider

La interfaz del proveedor representa la raíz del sistema de protección de datos. No se puede usar directamente para proteger o desproteger los datos. En su lugar, el consumidor debe obtener una referencia a un `IDataProtector` llamando a `IDataProtectionProvider.CreateProtector(purpose)`, donde propósito es una cadena que describe el caso de uso previsto del consumidor. Vea [cadenas de propósito](xref:security/data-protection/consumer-apis/purpose-strings) para obtener mucha más información sobre la intención de este parámetro y cómo elegir un valor adecuado.

## <a name="idataprotector"></a>IDataProtector

La interfaz del protector se devuelve mediante una llamada a `CreateProtector`, y es esta interfaz la que los consumidores pueden usar para realizar las operaciones de protección y desprotección.

Para proteger un fragmento de datos, pase los datos al método `Protect`. La interfaz básica define un método que convierte Byte []-> Byte [], pero también hay una sobrecarga (proporcionada como método de extensión) que convierte la cadena > cadena. La seguridad que ofrecen los dos métodos es idéntica; el desarrollador debe elegir cualquier sobrecarga que sea más conveniente para su caso de uso. Con independencia de la sobrecarga elegida, el valor devuelto por el método Protect ahora está protegido (cifrado y con corrección de manipulaciones) y la aplicación puede enviarlo a un cliente que no es de confianza.

Para desproteger un fragmento de datos previamente protegido, pase los datos protegidos al método `Unprotect`. (Hay sobrecargas basadas en bytes [] y basadas en cadenas para facilitar el desarrollo). Si la carga protegida fue generada por una llamada anterior a `Protect` en este mismo `IDataProtector`, el método `Unprotect` devolverá la carga sin protección original. Si la carga protegida se ha manipulado o se ha producido en otro `IDataProtector`, el método `Unprotect` producirá CryptographicException.

El concepto de la misma comparación con el de los diferentes `IDataProtector` es el concepto de propósito. Si dos instancias de `IDataProtector` se generaron a partir de la misma `IDataProtectionProvider` raíz pero a través de cadenas de propósito diferentes en la llamada a `IDataProtectionProvider.CreateProtector`, se consideran [diferentes protectores](xref:security/data-protection/consumer-apis/purpose-strings)y una no podrá desproteger las cargas generadas por el otro.

## <a name="consuming-these-interfaces"></a>Utilizar estas interfaces

En el caso de un componente con reconocimiento de lectura, el uso previsto es que el componente toma un parámetro `IDataProtectionProvider` en su constructor y que el sistema DI proporciona automáticamente este servicio cuando se crea una instancia del componente.

> [!NOTE]
> Algunas aplicaciones (como las aplicaciones de consola o las aplicaciones ASP.NET 4. x) podrían no ser compatibles con DI, por lo que no puede usar el mecanismo que se describe aquí. En estos escenarios, consulte el documento [escenarios no compatibles con di](xref:security/data-protection/configuration/non-di-scenarios) /para obtener más información sobre cómo obtener una instancia de un proveedor de `IDataProtection` sin pasar por la inserción.

En el ejemplo siguiente se muestran tres conceptos:

1. [Agregue el sistema de protección de datos](xref:security/data-protection/configuration/overview) al contenedor de servicios.

2. Usar DI para recibir una instancia de un `IDataProtectionProvider`, y

3. Crear un `IDataProtector` a partir de un `IDataProtectionProvider` y usarlo para proteger y desproteger los datos.

[!code-csharp[](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

El paquete Microsoft. AspNetCore. desproteccion. Abstractions contiene un método de extensión `IServiceProvider.GetDataProtector` como comodidad para los desarrolladores. Encapsula como una operación única y recupera una `IDataProtectionProvider` del proveedor de servicios y llama a `IDataProtectionProvider.CreateProtector`. En el ejemplo siguiente se muestra su uso.

[!code-csharp[](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> Las instancias de `IDataProtectionProvider` y `IDataProtector` son seguras para subprocesos para varios llamadores. Está pensado que una vez que un componente obtiene una referencia a un `IDataProtector` a través de una llamada a `CreateProtector`, usará esa referencia para varias llamadas a `Protect` y `Unprotect`. Una llamada a `Unprotect` producirá CryptographicException si la carga protegida no se puede comprobar o descifrar. Algunos componentes pueden querer omitir los errores durante las operaciones de desprotección; un componente que lee las cookies de autenticación podría controlar este error y tratar la solicitud como si no tuviera cookies en absoluto, en lugar de que se produzca un error en la solicitud. Los componentes que quieren este comportamiento deben detectar específicamente CryptographicException en lugar de admitir todas las excepciones.
