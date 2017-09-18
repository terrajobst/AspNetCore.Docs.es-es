---
title: Configurar la identidad de ASP.NET Core
author: AdrienTorris
description: Comprender los valores predeterminados de ASP.NET Core Identity y configurar las distintas propiedades de identidad para usar valores personalizados.
keywords: "Autenticación de ASP.NET Core, identidad, seguridad"
ms.author: scaddie
manager: wpickett
ms.date: 09/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: 629fcc2941b2d2fda9604a3eac04be3d0f5294b2
ms.sourcegitcommit: ddefc78270bd9b5ae0b1bd8de6c45f6977e7dceb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2017
---
# <a name="configure-identity"></a>Configurar la identidad

Identidad de ASP.NET Core tiene algunos comportamientos predeterminados que se pueden reemplazar fácilmente en la clase de inicio de la aplicación.

## <a name="passwords-policy"></a>Directiva de contraseñas

De forma predeterminada, identidad requiere que las contraseñas contengan un carácter en mayúsculas, caracteres en minúsculas y dígitos. También hay algunas otras restricciones. Si desea simplificar las restricciones de contraseña, puede hacerlo en la clase de inicio de la aplicación.

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=60-65)]

## <a name="applications-cookie-settings"></a>Configuración de cookies de la aplicación

Al igual que la directiva de contraseñas, todos los valores de cookie de la aplicación pueden cambiarse en la clase de inicio.

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=72-80)]

## <a name="users-lockout"></a>Bloqueo del usuario

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=67-70)]
