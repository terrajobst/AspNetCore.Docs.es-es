---
title: Configurar la identidad
uid: security/authentication/identity-configuration
ms.openlocfilehash: 7ccd89360a8c7f5c8c6dfac76df42898e18a116a
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
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
