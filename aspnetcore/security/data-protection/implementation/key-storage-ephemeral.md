---
title: Proveedores de protección de datos efímero en ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de los detalles de implementación de los proveedores de protección de datos efímero de ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: 9cdd3949826091807f3139f51aa0c05ddaac696a
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2018
---
# <a name="ephemeral-data-protection-providers-in-aspnet-core"></a><span data-ttu-id="cd4bd-103">Proveedores de protección de datos efímero en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cd4bd-103">Ephemeral data protection providers in ASP.NET Core</span></span>

<a name="data-protection-implementation-key-storage-ephemeral"></a>

<span data-ttu-id="cd4bd-104">Existen escenarios donde una aplicación necesita un throwaway `IDataProtectionProvider`.</span><span class="sxs-lookup"><span data-stu-id="cd4bd-104">There are scenarios where an application needs a throwaway `IDataProtectionProvider`.</span></span> <span data-ttu-id="cd4bd-105">Por ejemplo, solo se puede experimentar el desarrollador en una aplicación de consola de uso único o la propia aplicación es transitoria (se incluye en el script o una prueba unitaria de proyecto).</span><span class="sxs-lookup"><span data-stu-id="cd4bd-105">For example, the developer might just be experimenting in a one-off console application, or the application itself is transient (it's scripted or a unit test project).</span></span> <span data-ttu-id="cd4bd-106">Para admitir estos escenarios el [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) paquete incluye un tipo `EphemeralDataProtectionProvider`.</span><span class="sxs-lookup"><span data-stu-id="cd4bd-106">To support these scenarios the [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) package includes a type `EphemeralDataProtectionProvider`.</span></span> <span data-ttu-id="cd4bd-107">Este tipo proporciona una implementación básica de `IDataProtectionProvider` cuya clave repositorio se mantiene solamente en memoria y no escribe en ningún almacén de respaldo.</span><span class="sxs-lookup"><span data-stu-id="cd4bd-107">This type provides a basic implementation of `IDataProtectionProvider` whose key repository is held solely in-memory and isn't written out to any backing store.</span></span>

<span data-ttu-id="cd4bd-108">Cada instancia de `EphemeralDataProtectionProvider` usa su propia clave principal único.</span><span class="sxs-lookup"><span data-stu-id="cd4bd-108">Each instance of `EphemeralDataProtectionProvider` uses its own unique master key.</span></span> <span data-ttu-id="cd4bd-109">Por lo tanto, si un `IDataProtector` con raíz en un `EphemeralDataProtectionProvider` genera una carga protegida, ese carga solo puede desproteger un equivalente `IDataProtector` (les proporciona el mismo [propósito](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-consumer-apis-purposes) cadena) con raíz en el mismo `EphemeralDataProtectionProvider` instancia.</span><span class="sxs-lookup"><span data-stu-id="cd4bd-109">Therefore, if an `IDataProtector` rooted at an `EphemeralDataProtectionProvider` generates a protected payload, that payload can only be unprotected by an equivalent `IDataProtector` (given the same [purpose](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-consumer-apis-purposes) chain) rooted at the same `EphemeralDataProtectionProvider` instance.</span></span>

<span data-ttu-id="cd4bd-110">El siguiente ejemplo muestra cómo crear instancias de un `EphemeralDataProtectionProvider` y usarla para proteger y desproteger los datos.</span><span class="sxs-lookup"><span data-stu-id="cd4bd-110">The following sample demonstrates instantiating an `EphemeralDataProtectionProvider` and using it to protect and unprotect data.</span></span>

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
