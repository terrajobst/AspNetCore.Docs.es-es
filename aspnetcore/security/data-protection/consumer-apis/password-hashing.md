---
title: "Hash de contraseña"
author: rick-anderson
description: "Este documento explica cómo realizar un hash de contraseñas mediante el uso de la API de protección de datos de ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: e97d17b5f6de2e0ddcde6d51618675388b43911a
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="password-hashing"></a><span data-ttu-id="50c3f-103">Hash de contraseña</span><span class="sxs-lookup"><span data-stu-id="50c3f-103">Password Hashing</span></span>

<span data-ttu-id="50c3f-104">La base de código de protección de datos incluye un paquete *Microsoft.AspNetCore.Cryptography.KeyDerivation* que contiene funciones de derivación de claves criptográficas.</span><span class="sxs-lookup"><span data-stu-id="50c3f-104">The data protection code base includes a package *Microsoft.AspNetCore.Cryptography.KeyDerivation* which contains cryptographic key derivation functions.</span></span> <span data-ttu-id="50c3f-105">Este paquete es un componente independiente y no tiene ninguna dependencia en el resto del sistema de protección de datos.</span><span class="sxs-lookup"><span data-stu-id="50c3f-105">This package is a standalone component and has no dependencies on the rest of the data protection system.</span></span> <span data-ttu-id="50c3f-106">Se puede utilizar completamente por separado.</span><span class="sxs-lookup"><span data-stu-id="50c3f-106">It can be used completely independently.</span></span> <span data-ttu-id="50c3f-107">El origen coexista con el código de protección de datos base para su comodidad.</span><span class="sxs-lookup"><span data-stu-id="50c3f-107">The source exists alongside the data protection code base as a convenience.</span></span>

<span data-ttu-id="50c3f-108">Actualmente, el paquete ofrece un método `KeyDerivation.Pbkdf2` que permite que se aplican algoritmos hash a una contraseña mediante el [PBKDF2 algoritmo](https://tools.ietf.org/html/rfc2898#section-5.2).</span><span class="sxs-lookup"><span data-stu-id="50c3f-108">The package currently offers a method `KeyDerivation.Pbkdf2` which allows hashing a password using the [PBKDF2 algorithm](https://tools.ietf.org/html/rfc2898#section-5.2).</span></span> <span data-ttu-id="50c3f-109">Esta API es muy similar a existente de .NET Framework [Rfc2898DeriveBytes tipo](https://docs.microsoft.com/dotnet/api/system.security.cryptography.rfc2898derivebytes), pero hay tres diferencias importantes:</span><span class="sxs-lookup"><span data-stu-id="50c3f-109">This API is very similar to the .NET Framework's existing [Rfc2898DeriveBytes type](https://docs.microsoft.com/dotnet/api/system.security.cryptography.rfc2898derivebytes), but there are three important distinctions:</span></span>

1. <span data-ttu-id="50c3f-110">El `KeyDerivation.Pbkdf2` método compatible con el uso de varios PRFs (actualmente `HMACSHA1`, `HMACSHA256`, y `HMACSHA512`), mientras que la `Rfc2898DeriveBytes` escriba sólo admite `HMACSHA1`.</span><span class="sxs-lookup"><span data-stu-id="50c3f-110">The `KeyDerivation.Pbkdf2` method supports consuming multiple PRFs (currently `HMACSHA1`, `HMACSHA256`, and `HMACSHA512`), whereas the `Rfc2898DeriveBytes` type only supports `HMACSHA1`.</span></span>

2. <span data-ttu-id="50c3f-111">El `KeyDerivation.Pbkdf2` método detecta el sistema operativo actual e intenta elegir la implementación de la rutina, proporcionando un rendimiento mucho mejor en ciertos casos más optimizada.</span><span class="sxs-lookup"><span data-stu-id="50c3f-111">The `KeyDerivation.Pbkdf2` method detects the current operating system and attempts to choose the most optimized implementation of the routine, providing much better performance in certain cases.</span></span> <span data-ttu-id="50c3f-112">(En Windows 8, que ofrece aproximadamente 10 veces el rendimiento de `Rfc2898DeriveBytes`.)</span><span class="sxs-lookup"><span data-stu-id="50c3f-112">(On Windows 8, it offers around 10x the throughput of `Rfc2898DeriveBytes`.)</span></span>

3. <span data-ttu-id="50c3f-113">El `KeyDerivation.Pbkdf2` método requiere que el llamador especificar todos los parámetros (sal, PRF y número de iteraciones).</span><span class="sxs-lookup"><span data-stu-id="50c3f-113">The `KeyDerivation.Pbkdf2` method requires the caller to specify all parameters (salt, PRF, and iteration count).</span></span> <span data-ttu-id="50c3f-114">El `Rfc2898DeriveBytes` tipo proporciona valores predeterminados para estos.</span><span class="sxs-lookup"><span data-stu-id="50c3f-114">The `Rfc2898DeriveBytes` type provides default values for these.</span></span>

[!code-csharp[Main](password-hashing/samples/passwordhasher.cs)]

<span data-ttu-id="50c3f-115">Vea el código fuente de ASP.NET Core Identity `PasswordHasher` caso de uso de tipo para un mundo real.</span><span class="sxs-lookup"><span data-stu-id="50c3f-115">See the source code for ASP.NET Core Identity's `PasswordHasher` type for a real-world use case.</span></span>
