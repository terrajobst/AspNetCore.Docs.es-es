---
title: Hash de contraseñas en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo realizar un hash de contraseñas mediante las API de protección de datos de ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: 882ac9b256b0cdf5fd19dc4bd2757cac7e8ecad3
ms.sourcegitcommit: a742b55e4b8276a48b8b4394784554fecd883c84
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45538383"
---
# <a name="hash-passwords-in-aspnet-core"></a>Hash de contraseñas en ASP.NET Core

La base de código de protección de datos incluye un paquete *Microsoft.AspNetCore.Cryptography.KeyDerivation* que contiene funciones de derivación de claves criptográficas. Este paquete es un componente independiente y no tiene dependencias en el resto del sistema de protección de datos. Se pueden usar completamente por separado. El origen existe junto con el código de protección de datos base para su comodidad.

Actualmente, el paquete ofrece un método `KeyDerivation.Pbkdf2` que permite el hash de una contraseña mediante la [PBKDF2 algoritmo](https://tools.ietf.org/html/rfc2898#section-5.2). Esta API es muy similar a .NET Framework existente [Rfc2898DeriveBytes tipo](/dotnet/api/system.security.cryptography.rfc2898derivebytes), pero hay tres diferencias importantes:

1. El `KeyDerivation.Pbkdf2` método admite el consumo de varias PRFs (actualmente `HMACSHA1`, `HMACSHA256`, y `HMACSHA512`), mientras que el `Rfc2898DeriveBytes` escriba sólo admite `HMACSHA1`.

2. El `KeyDerivation.Pbkdf2` método detecta el sistema operativo actual e intenta elegir la implementación de la rutina, proporcionando un rendimiento mucho mejor en ciertos casos más optimizada. (En Windows 8, ofrece aproximadamente 10 veces el rendimiento de `Rfc2898DeriveBytes`.)

3. El `KeyDerivation.Pbkdf2` método requiere que el llamador especificar todos los parámetros (sal, PRF y número de iteraciones). El `Rfc2898DeriveBytes` tipo proporciona valores predeterminados para estos.

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

Vea el código (https://github.com/aspnet/Identity/blob/master/src/Core/PasswordHasher.cs) para ASP.NET Core Identity `PasswordHasher` caso de uso de tipo para un mundo real.
