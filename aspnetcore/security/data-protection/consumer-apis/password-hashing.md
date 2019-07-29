---
title: Contraseñas de hash en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo aplicar un algoritmo hash a las contraseñas mediante las API de protección de datos de ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: bd4b8fcf6a5a16a86ada97bbd3519f872d1417b7
ms.sourcegitcommit: 0efb9e219fef481dee35f7b763165e488aa6cf9c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2019
ms.locfileid: "68602455"
---
# <a name="hash-passwords-in-aspnet-core"></a>Contraseñas de hash en ASP.NET Core

La base de código de protección de datos incluye un paquete *Microsoft. AspNetCore. Cryptography. derivación* que contiene las funciones de derivación de claves de cifrado. Este paquete es un componente independiente y no tiene dependencias en el resto del sistema de protección de datos. Se puede usar de forma totalmente independiente. El origen existe junto con el código base de protección de datos como una comodidad.

El paquete ofrece actualmente un método `KeyDerivation.Pbkdf2` que permite aplicar un algoritmo hash a una contraseña mediante el [algoritmo PBKDF2](https://tools.ietf.org/html/rfc2898#section-5.2). Esta API es muy similar al [tipo Rfc2898DeriveBytes](/dotnet/api/system.security.cryptography.rfc2898derivebytes)existente del .NET Framework, pero hay tres diferencias importantes:

1. El `KeyDerivation.Pbkdf2` método admite el consumo de varias PRFS ( `HMACSHA1`actualmente `HMACSHA256`, y `HMACSHA512`), mientras que `Rfc2898DeriveBytes` el tipo solo `HMACSHA1`admite.

2. El `KeyDerivation.Pbkdf2` método detecta el sistema operativo actual e intenta elegir la implementación más optimizada de la rutina, lo que proporciona un rendimiento mucho mejor en ciertos casos. (En Windows 8, ofrece aproximadamente 10 veces el rendimiento de `Rfc2898DeriveBytes`).

3. El `KeyDerivation.Pbkdf2` método requiere que el llamador especifique todos los parámetros (sal, PRF y recuento de iteraciones). El `Rfc2898DeriveBytes` tipo proporciona los valores predeterminados para estos.

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

Vea el [código fuente](https://github.com/aspnet/AspNetCore/blob/master/src/Identity/Extensions.Core/src/PasswordHasher.cs) de ASP.net Core `PasswordHasher` tipo de identidad para un caso de uso real.
