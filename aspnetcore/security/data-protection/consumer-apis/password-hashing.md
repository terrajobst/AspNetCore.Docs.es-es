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
# <a name="password-hashing"></a>Hash de contraseña

La base de código de protección de datos incluye un paquete *Microsoft.AspNetCore.Cryptography.KeyDerivation* que contiene funciones de derivación de claves criptográficas. Este paquete es un componente independiente y no tiene ninguna dependencia en el resto del sistema de protección de datos. Se puede utilizar completamente por separado. El origen coexista con el código de protección de datos base para su comodidad.

Actualmente, el paquete ofrece un método `KeyDerivation.Pbkdf2` que permite que se aplican algoritmos hash a una contraseña mediante el [PBKDF2 algoritmo](https://tools.ietf.org/html/rfc2898#section-5.2). Esta API es muy similar a existente de .NET Framework [Rfc2898DeriveBytes tipo](https://docs.microsoft.com/dotnet/api/system.security.cryptography.rfc2898derivebytes), pero hay tres diferencias importantes:

1. El `KeyDerivation.Pbkdf2` método compatible con el uso de varios PRFs (actualmente `HMACSHA1`, `HMACSHA256`, y `HMACSHA512`), mientras que la `Rfc2898DeriveBytes` escriba sólo admite `HMACSHA1`.

2. El `KeyDerivation.Pbkdf2` método detecta el sistema operativo actual e intenta elegir la implementación de la rutina, proporcionando un rendimiento mucho mejor en ciertos casos más optimizada. (En Windows 8, que ofrece aproximadamente 10 veces el rendimiento de `Rfc2898DeriveBytes`.)

3. El `KeyDerivation.Pbkdf2` método requiere que el llamador especificar todos los parámetros (sal, PRF y número de iteraciones). El `Rfc2898DeriveBytes` tipo proporciona valores predeterminados para estos.

[!code-csharp[Main](password-hashing/samples/passwordhasher.cs)]

Vea el código fuente de ASP.NET Core Identity `PasswordHasher` caso de uso de tipo para un mundo real.
