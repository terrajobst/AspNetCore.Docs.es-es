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
ms.openlocfilehash: adf577ae1e1c752c3b1a332ec94a7a7627a7a4b4
ms.sourcegitcommit: 76d42f09f3e0dd2f2105493eca6b29994aa47706
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2017
---
# <a name="configure-identity"></a>Configurar la identidad

ASP.NET Core Identity tiene algunos comportamientos predeterminados que se pueden reemplazar fácilmente en la aplicación `Startup` clase.

## <a name="passwords-policy"></a>Directiva de contraseñas

De forma predeterminada, identidad requiere que las contraseñas contengan un carácter en mayúsculas, caracteres en minúsculas, un dígito y un carácter alfanumérico. También hay algunas otras restricciones. Si desea simplificar las restricciones de contraseña, puede hacerlo el `Startup` clase de la aplicación.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Núcleo de ASP.NET 2.0 agregado la `RequiredUniqueChars` propiedad. De lo contrario, las opciones son las mismas de ASP.NET Core 1.x.

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

`IdentityOptions.Password`tiene las siguientes propiedades:
* `RequireDigit`: No necesita un número comprendido entre 0-9 en la contraseña. El valor predeterminado es true.
* `RequiredLength`: La longitud mínima de la contraseña. El valor predeterminado es 6.
* `RequireNonAlphanumeric`: Requiere un carácter que no sean alfanuméricos en la contraseña. El valor predeterminado es true.
* `RequireUppercase`: Requiere un carácter en mayúscula en la contraseña. El valor predeterminado es true.
* `RequireLowercase`: Requiere un carácter en minúsculas de la contraseña. El valor predeterminado es true.
* `RequiredUniqueChars`: El número de caracteres distintos de la contraseña no necesita. El valor predeterminado es 1.


## <a name="users-lockout"></a>Bloqueo del usuario

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

`IdentityOptions.Lockout`tiene las siguientes propiedades:
* `DefaultLockoutTimeSpan`: La cantidad de tiempo que un usuario está bloqueado cuando se produce un bloqueo. El valor predeterminado es 5 minutos.
* `MaxFailedAccessAttempts`: El número de intentos de acceso erróneos hasta que se bloquea un usuario, si está habilitado el bloqueo. El valor predeterminado es 5.
* `AllowedForNewUsers`: Determina si un usuario nuevo puede bloquearse. El valor predeterminado es true.


## <a name="sign-in-settings"></a>Inicie sesión en la configuración

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

`IdentityOptions.SignIn`tiene las siguientes propiedades:
* `RequireConfirmedEmail`: No necesita un correo electrónico confirmado para iniciar sesión en. El valor predeterminado es false.
* `RequireConfirmedPhoneNumber`: No necesita un número de teléfono confirmada iniciar sesión en. El valor predeterminado es false.


## <a name="user-validation-settings"></a>Configuración de la validación de usuario

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

`IdentityOptions.User`tiene las siguientes propiedades:
* `RequireUniqueEmail`: Requiere que cada usuario tiene un correo electrónico único. El valor predeterminado es false.
* `AllowedUserNameCharacters`: Permite caracteres en el nombre de usuario. El valor predeterminado es abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+.

## <a name="applications-cookie-settings"></a>Configuración de cookies de la aplicación

Al igual que la directiva de contraseñas, todos los valores de cookie de la aplicación pueden cambiarse en el `Startup` clase.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

En `ConfigureServices` en la `Startup` (clase), puede configurar la cookie de la aplicación.

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

--- 

`CookieAuthenticationOptions`tiene las siguientes propiedades:
* `Cookie.Name`: El nombre de la cookie. El valor predeterminado. AspNetCore.Cookies.
* `Cookie.HttpOnly`: Cuando sea true, la cookie no es accesible desde scripts del lado cliente. El valor predeterminado es true.
* `ExpireTimeSpan`: Controla cuánto tiempo el vale de autenticación almacenado en la cookie será válida desde el punto en que se crea. El valor predeterminado es 14 días.
* `LoginPath`: Cuando un usuario está autorizado, se redirigirán a esta ruta de acceso al inicio de sesión. El valor predeterminado es/Account/inicio de sesión.
* `LogoutPath`: Cuando un usuario se cerró la sesión, se redirigirán a esta ruta de acceso. El valor predeterminado es/Account/cierre de sesión.
* `AccessDeniedPath`: Cuando un usuario se produce un error de comprobación de la autorización, se redirigirán a esta ruta de acceso. El valor predeterminado es/Account/AccessDenied.
* `SlidingExpiration`: Cuando sea true, se emitirá una cookie nueva con una nueva hora de expiración cuando la cookie actual está en más de medio a través de la ventana de expiración. El valor predeterminado es true.
* `ReturnUrlParameter`: ReturnUrlParameter determina el nombre del parámetro de cadena de consulta que se anexa el middleware cuando se cambia un código de estado sin autorización 401 a una redirección 302 en la ruta de acceso de inicio de sesión.
* `AuthenticationScheme`: Esto solo es relevante para ASP.NET Core 1.x. El nombre lógico para un esquema de autenticación concreto.
* `AutomaticAuthenticate`: Esta marca solo es pertinente para ASP.NET Core 1.x. Cuando sea true, autenticación con cookies debe ejecutar en cada solicitud y se intentan validar y reconstruir cualquier entidad de seguridad serializado que creó.

