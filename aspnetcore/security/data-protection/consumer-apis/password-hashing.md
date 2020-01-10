---
title: Contraseñas de hash en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo aplicar un algoritmo hash a las contraseñas mediante las API de protección de datos de ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: 52532f9f4d7f9037609c8273deb8b6964d81c714
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828573"
---
# <a name="hash-passwords-in-aspnet-core"></a>Contraseñas de hash en ASP.NET Core

La base de código de protección de datos incluye un paquete *Microsoft. AspNetCore. Cryptography. derivación* que contiene las funciones de derivación de claves de cifrado. Este paquete es un componente independiente y no tiene dependencias en el resto del sistema de protección de datos. Se puede usar de forma totalmente independiente. El origen existe junto con el código base de protección de datos como una comodidad.

El paquete ofrece actualmente un método `KeyDerivation.Pbkdf2` que permite aplicar un algoritmo hash a una contraseña mediante el [algoritmo PBKDF2](https://tools.ietf.org/html/rfc2898#section-5.2). Esta API es muy similar al [tipo Rfc2898DeriveBytes](/dotnet/api/system.security.cryptography.rfc2898derivebytes)existente del .NET Framework, pero hay tres diferencias importantes:

1. El método `KeyDerivation.Pbkdf2` admite el consumo de varias PRFs (actualmente `HMACSHA1`, `HMACSHA256`y `HMACSHA512`), mientras que el tipo `Rfc2898DeriveBytes` solo admite `HMACSHA1`.

2. El método `KeyDerivation.Pbkdf2` detecta el sistema operativo actual e intenta elegir la implementación más optimizada de la rutina, lo que proporciona un rendimiento mucho mejor en ciertos casos. (En Windows 8, ofrece aproximadamente 10 veces el rendimiento de `Rfc2898DeriveBytes`).

3. El método `KeyDerivation.Pbkdf2` requiere que el llamador especifique todos los parámetros (sal, PRF y recuento de iteraciones). El tipo de `Rfc2898DeriveBytes` proporciona valores predeterminados para ellos.

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

Vea el [código fuente](https://github.com/dotnet/AspNetCore/blob/master/src/Identity/Extensions.Core/src/PasswordHasher.cs) de ASP.net Core tipo de `PasswordHasher` de identidad para un caso de uso real.
