---
title: Proveedores de protección de datos efímeros en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre los detalles de implementación de los ASP.NET Core proveedores de protección de datos efímeros.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: e4b0014ab3bdbf90b91383e8a33102f94faa8153
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78654017"
---
# <a name="ephemeral-data-protection-providers-in-aspnet-core"></a>Proveedores de protección de datos efímeros en ASP.NET Core

<a name="data-protection-implementation-key-storage-ephemeral"></a>

Hay escenarios en los que una aplicación necesita un `IDataProtectionProvider`throwaway. Por ejemplo, el desarrollador podría estar experimentando en una aplicación de consola única o la propia aplicación es transitoria (se trata de un proyecto de prueba unitaria o de script). Para admitir estos escenarios, el paquete [Microsoft. AspNetCore. PROTECCION](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) incluye un tipo `EphemeralDataProtectionProvider`. Este tipo proporciona una implementación básica de `IDataProtectionProvider` cuyo repositorio de claves se mantiene únicamente en memoria y no se escribe en ningún almacén de respaldo.

Cada instancia de `EphemeralDataProtectionProvider` utiliza su propia clave maestra única. Por lo tanto, si una `IDataProtector` raíz en un `EphemeralDataProtectionProvider` genera una carga protegida, esa carga útil solo puede desprotegerse mediante una `IDataProtector` equivalente (dada la misma cadena de [propósito](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-consumer-apis-purposes) ) cuya raíz se encuentra en la misma instancia de `EphemeralDataProtectionProvider`.

En el ejemplo siguiente se muestra cómo crear una instancia de un `EphemeralDataProtectionProvider` y usarlo para proteger y desproteger los datos.

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
