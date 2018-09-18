---
title: Hash de contraseñas en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo realizar un hash de contraseñas mediante las API de protección de datos de ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: 70301ffffbaaf3c5ff0642b19b80e40be83aa438
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46010967"
---
# <a name="hash-passwords-in-aspnet-core"></a><span data-ttu-id="1edef-103">Hash de contraseñas en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1edef-103">Hash passwords in ASP.NET Core</span></span>

<span data-ttu-id="1edef-104">La base de código de protección de datos incluye un paquete *Microsoft.AspNetCore.Cryptography.KeyDerivation* que contiene funciones de derivación de claves criptográficas.</span><span class="sxs-lookup"><span data-stu-id="1edef-104">The data protection code base includes a package *Microsoft.AspNetCore.Cryptography.KeyDerivation* which contains cryptographic key derivation functions.</span></span> <span data-ttu-id="1edef-105">Este paquete es un componente independiente y no tiene dependencias en el resto del sistema de protección de datos.</span><span class="sxs-lookup"><span data-stu-id="1edef-105">This package is a standalone component and has no dependencies on the rest of the data protection system.</span></span> <span data-ttu-id="1edef-106">Se pueden usar completamente por separado.</span><span class="sxs-lookup"><span data-stu-id="1edef-106">It can be used completely independently.</span></span> <span data-ttu-id="1edef-107">El origen existe junto con el código de protección de datos base para su comodidad.</span><span class="sxs-lookup"><span data-stu-id="1edef-107">The source exists alongside the data protection code base as a convenience.</span></span>

<span data-ttu-id="1edef-108">Actualmente, el paquete ofrece un método `KeyDerivation.Pbkdf2` que permite el hash de una contraseña mediante la [PBKDF2 algoritmo](https://tools.ietf.org/html/rfc2898#section-5.2).</span><span class="sxs-lookup"><span data-stu-id="1edef-108">The package currently offers a method `KeyDerivation.Pbkdf2` which allows hashing a password using the [PBKDF2 algorithm](https://tools.ietf.org/html/rfc2898#section-5.2).</span></span> <span data-ttu-id="1edef-109">Esta API es muy similar a .NET Framework existente [Rfc2898DeriveBytes tipo](/dotnet/api/system.security.cryptography.rfc2898derivebytes), pero hay tres diferencias importantes:</span><span class="sxs-lookup"><span data-stu-id="1edef-109">This API is very similar to the .NET Framework's existing [Rfc2898DeriveBytes type](/dotnet/api/system.security.cryptography.rfc2898derivebytes), but there are three important distinctions:</span></span>

1. <span data-ttu-id="1edef-110">El `KeyDerivation.Pbkdf2` método admite el consumo de varias PRFs (actualmente `HMACSHA1`, `HMACSHA256`, y `HMACSHA512`), mientras que el `Rfc2898DeriveBytes` escriba sólo admite `HMACSHA1`.</span><span class="sxs-lookup"><span data-stu-id="1edef-110">The `KeyDerivation.Pbkdf2` method supports consuming multiple PRFs (currently `HMACSHA1`, `HMACSHA256`, and `HMACSHA512`), whereas the `Rfc2898DeriveBytes` type only supports `HMACSHA1`.</span></span>

2. <span data-ttu-id="1edef-111">El `KeyDerivation.Pbkdf2` método detecta el sistema operativo actual e intenta elegir la implementación de la rutina, proporcionando un rendimiento mucho mejor en ciertos casos más optimizada.</span><span class="sxs-lookup"><span data-stu-id="1edef-111">The `KeyDerivation.Pbkdf2` method detects the current operating system and attempts to choose the most optimized implementation of the routine, providing much better performance in certain cases.</span></span> <span data-ttu-id="1edef-112">(En Windows 8, ofrece aproximadamente 10 veces el rendimiento de `Rfc2898DeriveBytes`.)</span><span class="sxs-lookup"><span data-stu-id="1edef-112">(On Windows 8, it offers around 10x the throughput of `Rfc2898DeriveBytes`.)</span></span>

3. <span data-ttu-id="1edef-113">El `KeyDerivation.Pbkdf2` método requiere que el llamador especificar todos los parámetros (sal, PRF y número de iteraciones).</span><span class="sxs-lookup"><span data-stu-id="1edef-113">The `KeyDerivation.Pbkdf2` method requires the caller to specify all parameters (salt, PRF, and iteration count).</span></span> <span data-ttu-id="1edef-114">El `Rfc2898DeriveBytes` tipo proporciona valores predeterminados para estos.</span><span class="sxs-lookup"><span data-stu-id="1edef-114">The `Rfc2898DeriveBytes` type provides default values for these.</span></span>

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

<span data-ttu-id="1edef-115">Consulte la [código fuente](https://github.com/aspnet/Identity/blob/master/src/Core/PasswordHasher.cs) para ASP.NET Core Identity `PasswordHasher` caso de uso de tipo para un mundo real.</span><span class="sxs-lookup"><span data-stu-id="1edef-115">See the [source code](https://github.com/aspnet/Identity/blob/master/src/Core/PasswordHasher.cs) for ASP.NET Core Identity's `PasswordHasher` type for a real-world use case.</span></span>
