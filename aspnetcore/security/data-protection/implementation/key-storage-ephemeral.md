---
title: Proveedores de protección de datos efímeros en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre los detalles de implementación de los proveedores de protección de datos efímeros ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: e4b0014ab3bdbf90b91383e8a33102f94faa8153
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64895182"
---
# <a name="ephemeral-data-protection-providers-in-aspnet-core"></a>Proveedores de protección de datos efímeros en ASP.NET Core

<a name="data-protection-implementation-key-storage-ephemeral"></a>

Hay escenarios donde una aplicación necesita un throwaway `IDataProtectionProvider`. Por ejemplo, el desarrollador simplemente se podría experimentar una aplicación de consola única o la propia aplicación es transitoria (se incluye en el script o una prueba unitaria de proyecto). Para admitir estos escenarios el [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) paquete incluye un tipo `EphemeralDataProtectionProvider`. Este tipo proporciona una implementación básica de `IDataProtectionProvider` cuya clave repositorio se mantiene únicamente en memoria y no se escriben en ningún almacén de respaldo.

Cada instancia de `EphemeralDataProtectionProvider` usa su propia clave principal único. Por lo tanto, si un `IDataProtector` cuya raíz comienza en un `EphemeralDataProtectionProvider` genera una carga protegida, esa carga sólo puede desproteger un equivalente `IDataProtector` (recibe la misma [propósito](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-consumer-apis-purposes) cadena) cuya raíz comienza en el mismo `EphemeralDataProtectionProvider` instancia de.

El siguiente ejemplo muestra cómo crear una instancia de un `EphemeralDataProtectionProvider` y usarlo para proteger y desproteger los datos.

```csharp
using System;
using Microsoft.AspNetCore.DataProtection;

public class Program
{
    public static void Main(string[] args)
    {
        const string purpose = "Ephemeral.App.v1";

        // create an ephemeral provider and demonstrate that it can round-trip a payload
        var provider = new EphemeralDataProtectionProvider();
        var protector = provider.CreateProtector(purpose);
        Console.Write("Enter input: ");
        string input = Console.ReadLine();

        // protect the payload
        string protectedPayload = protector.Protect(input);
        Console.WriteLine($"Protect returned: {protectedPayload}");

        // unprotect the payload
        string unprotectedPayload = protector.Unprotect(protectedPayload);
        Console.WriteLine($"Unprotect returned: {unprotectedPayload}");

        // if I create a new ephemeral provider, it won't be able to unprotect existing
        // payloads, even if I specify the same purpose
        provider = new EphemeralDataProtectionProvider();
        protector = provider.CreateProtector(purpose);
        unprotectedPayload = protector.Unprotect(protectedPayload); // THROWS
    }
}

/*
* SAMPLE OUTPUT
*
* Enter input: Hello!
* Protect returned: CfDJ8AAAAAAAAAAAAAAAAAAAAA...uGoxWLjGKtm1SkNACQ
* Unprotect returned: Hello!
* << throws CryptographicException >>
*/
```
