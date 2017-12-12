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
ms.openlocfilehash: 2861ca474e7e82da81943966394a92040ce96ab8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="configure-identity"></a><span data-ttu-id="6f4ba-104">Configurar la identidad</span><span class="sxs-lookup"><span data-stu-id="6f4ba-104">Configure Identity</span></span>

<span data-ttu-id="6f4ba-105">ASP.NET Core Identity tiene algunos comportamientos predeterminados que se pueden reemplazar fácilmente en la aplicación `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-105">ASP.NET Core Identity has some default behaviors that you can override easily in your application's `Startup` class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="6f4ba-106">Directiva de contraseñas</span><span class="sxs-lookup"><span data-stu-id="6f4ba-106">Passwords policy</span></span>

<span data-ttu-id="6f4ba-107">De forma predeterminada, identidad requiere que las contraseñas contengan un carácter en mayúsculas, caracteres en minúsculas, un dígito y un carácter no alfanumérico.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-107">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="6f4ba-108">También hay algunas otras restricciones.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-108">There are also some other restrictions.</span></span> <span data-ttu-id="6f4ba-109">Si desea simplificar las restricciones de contraseña, puede hacerlo el `Startup` clase de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-109">If you want to simplify password restrictions, you can do that in the `Startup` class of your application.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6f4ba-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6f4ba-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="6f4ba-111">Núcleo de ASP.NET 2.0 agregado la `RequiredUniqueChars` propiedad.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-111">ASP.NET Core 2.0 added the `RequiredUniqueChars` property.</span></span> <span data-ttu-id="6f4ba-112">De lo contrario, las opciones son las mismas de ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-112">Otherwise, the options are the same from ASP.NET Core 1.x.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6f4ba-113">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6f4ba-113">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

<span data-ttu-id="6f4ba-114">`IdentityOptions.Password`tiene las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="6f4ba-114">`IdentityOptions.Password` has the following properties:</span></span>
* <span data-ttu-id="6f4ba-115">`RequireDigit`: No necesita un número comprendido entre 0-9 en la contraseña.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-115">`RequireDigit`: Requires a number between 0-9 in the password.</span></span> <span data-ttu-id="6f4ba-116">El valor predeterminado es true.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-116">Defaults to true.</span></span>
* <span data-ttu-id="6f4ba-117">`RequiredLength`: La longitud mínima de la contraseña.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-117">`RequiredLength`: The minimum length of the password.</span></span> <span data-ttu-id="6f4ba-118">El valor predeterminado es 6.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-118">Defaults to 6.</span></span>
* <span data-ttu-id="6f4ba-119">`RequireNonAlphanumeric`: Requiere un carácter que no sean alfanuméricos en la contraseña.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-119">`RequireNonAlphanumeric`: Requires a non-alphanumeric character in the password.</span></span> <span data-ttu-id="6f4ba-120">El valor predeterminado es true.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-120">Defaults to true.</span></span>
* <span data-ttu-id="6f4ba-121">`RequireUppercase`: Requiere un carácter en mayúscula en la contraseña.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-121">`RequireUppercase`: Requires an upper case character in the password.</span></span> <span data-ttu-id="6f4ba-122">El valor predeterminado es true.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-122">Defaults to true.</span></span>
* <span data-ttu-id="6f4ba-123">`RequireLowercase`: Requiere un carácter en minúsculas de la contraseña.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-123">`RequireLowercase`: Requires a lower case character in the password.</span></span> <span data-ttu-id="6f4ba-124">El valor predeterminado es true.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-124">Defaults to true.</span></span>
* <span data-ttu-id="6f4ba-125">`RequiredUniqueChars`: El número de caracteres distintos de la contraseña no necesita.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-125">`RequiredUniqueChars`: Requires the number of distinct characters in the password.</span></span> <span data-ttu-id="6f4ba-126">El valor predeterminado es 1.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-126">Defaults to 1.</span></span>


## <a name="users-lockout"></a><span data-ttu-id="6f4ba-127">Bloqueo del usuario</span><span class="sxs-lookup"><span data-stu-id="6f4ba-127">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

<span data-ttu-id="6f4ba-128">`IdentityOptions.Lockout`tiene las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="6f4ba-128">`IdentityOptions.Lockout` has the following properties:</span></span>
* <span data-ttu-id="6f4ba-129">`DefaultLockoutTimeSpan`: La cantidad de tiempo que un usuario está bloqueado cuando se produce un bloqueo.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-129">`DefaultLockoutTimeSpan`: The amount of time a user is locked out when a lockout occurs.</span></span> <span data-ttu-id="6f4ba-130">El valor predeterminado es 5 minutos.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-130">Defaults to 5 minutes.</span></span>
* <span data-ttu-id="6f4ba-131">`MaxFailedAccessAttempts`: El número de intentos de acceso erróneos hasta que se bloquea un usuario, si está habilitado el bloqueo.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-131">`MaxFailedAccessAttempts`: The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span> <span data-ttu-id="6f4ba-132">El valor predeterminado es 5.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-132">Defaults to 5.</span></span>
* <span data-ttu-id="6f4ba-133">`AllowedForNewUsers`: Determina si un usuario nuevo puede bloquearse. El valor predeterminado es true.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-133">`AllowedForNewUsers`: Determines if a new user can be locked out. Defaults to true.</span></span>


## <a name="sign-in-settings"></a><span data-ttu-id="6f4ba-134">Inicie sesión en la configuración</span><span class="sxs-lookup"><span data-stu-id="6f4ba-134">Sign in settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

<span data-ttu-id="6f4ba-135">`IdentityOptions.SignIn`tiene las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="6f4ba-135">`IdentityOptions.SignIn` has the following properties:</span></span>
* <span data-ttu-id="6f4ba-136">`RequireConfirmedEmail`: No necesita un correo electrónico confirmado para iniciar sesión en.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-136">`RequireConfirmedEmail`: Requires a confirmed email to sign in.</span></span> <span data-ttu-id="6f4ba-137">El valor predeterminado es false.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-137">Defaults to false.</span></span>
* <span data-ttu-id="6f4ba-138">`RequireConfirmedPhoneNumber`: No necesita un número de teléfono confirmada iniciar sesión en.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-138">`RequireConfirmedPhoneNumber`: Requires a confirmed phone number to sign in.</span></span> <span data-ttu-id="6f4ba-139">El valor predeterminado es false.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-139">Defaults to false.</span></span>


## <a name="user-validation-settings"></a><span data-ttu-id="6f4ba-140">Configuración de la validación de usuario</span><span class="sxs-lookup"><span data-stu-id="6f4ba-140">User validation settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

<span data-ttu-id="6f4ba-141">`IdentityOptions.User`tiene las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="6f4ba-141">`IdentityOptions.User` has the following properties:</span></span>
* <span data-ttu-id="6f4ba-142">`RequireUniqueEmail`: Requiere que cada usuario tiene un correo electrónico único.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-142">`RequireUniqueEmail`: Requires each User to have a unique email.</span></span> <span data-ttu-id="6f4ba-143">El valor predeterminado es false.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-143">Defaults to false.</span></span>
* <span data-ttu-id="6f4ba-144">`AllowedUserNameCharacters`: Permite caracteres en el nombre de usuario.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-144">`AllowedUserNameCharacters`: Allowed characters in the username.</span></span> <span data-ttu-id="6f4ba-145">El valor predeterminado es abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-145">Defaults to abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+.</span></span>

## <a name="applications-cookie-settings"></a><span data-ttu-id="6f4ba-146">Configuración de cookies de la aplicación</span><span class="sxs-lookup"><span data-stu-id="6f4ba-146">Application's cookie settings</span></span>

<span data-ttu-id="6f4ba-147">Al igual que la directiva de contraseñas, todos los valores de cookie de la aplicación pueden cambiarse en el `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-147">Like the passwords policy, all the settings of the application's cookie can be changed in the `Startup` class.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6f4ba-148">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6f4ba-148">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="6f4ba-149">En `ConfigureServices` en la `Startup` (clase), puede configurar la cookie de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-149">Under `ConfigureServices` in the `Startup` class, you can configure the application's cookie.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6f4ba-150">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6f4ba-150">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

--- 

<span data-ttu-id="6f4ba-151">`CookieAuthenticationOptions`tiene las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="6f4ba-151">`CookieAuthenticationOptions` has the following properties:</span></span>
* <span data-ttu-id="6f4ba-152">`Cookie.Name`: El nombre de la cookie.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-152">`Cookie.Name`: The name of the cookie.</span></span> <span data-ttu-id="6f4ba-153">El valor predeterminado. AspNetCore.Cookies.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-153">Defaults to .AspNetCore.Cookies.</span></span>
* <span data-ttu-id="6f4ba-154">`Cookie.HttpOnly`: Cuando sea true, la cookie no es accesible desde scripts del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-154">`Cookie.HttpOnly`: When true, the cookie is not accessible from client-side scripts.</span></span> <span data-ttu-id="6f4ba-155">El valor predeterminado es true.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-155">Defaults to true.</span></span>
* <span data-ttu-id="6f4ba-156">`ExpireTimeSpan`: Controla cuánto tiempo el vale de autenticación almacenado en la cookie será válida desde el punto en que se crea.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-156">`ExpireTimeSpan`: Controls how much time the authentication ticket stored in the cookie will remain valid from the point it is created.</span></span> <span data-ttu-id="6f4ba-157">El valor predeterminado es 14 días.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-157">Defaults to 14 days.</span></span>
* <span data-ttu-id="6f4ba-158">`LoginPath`: Cuando un usuario está autorizado, se redirigirán a esta ruta de acceso al inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-158">`LoginPath`: When a user is unauthorized, they will be redirected to this path to login.</span></span> <span data-ttu-id="6f4ba-159">El valor predeterminado es/Account/inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-159">Defaults to /Account/Login.</span></span>
* <span data-ttu-id="6f4ba-160">`LogoutPath`: Cuando un usuario se cerró la sesión, se redirigirán a esta ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-160">`LogoutPath`: When a user is logged out, they will be redirected to this path.</span></span> <span data-ttu-id="6f4ba-161">El valor predeterminado es/Account/cierre de sesión.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-161">Defaults to /Account/Logout.</span></span>
* <span data-ttu-id="6f4ba-162">`AccessDeniedPath`: Cuando un usuario se produce un error de comprobación de la autorización, se redirigirán a esta ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-162">`AccessDeniedPath`: When a user fails an authorization check, they will be redirected to this path.</span></span> <span data-ttu-id="6f4ba-163">El valor predeterminado es/Account/AccessDenied.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-163">Defaults to /Account/AccessDenied.</span></span>
* <span data-ttu-id="6f4ba-164">`SlidingExpiration`: Cuando sea true, se emitirá una cookie nueva con una nueva hora de expiración cuando la cookie actual está en más de medio a través de la ventana de expiración.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-164">`SlidingExpiration`: When true, a new cookie will be issued with a new expiration time when the current cookie is more than halfway through the expiration window.</span></span> <span data-ttu-id="6f4ba-165">El valor predeterminado es true.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-165">Defaults to true.</span></span>
* <span data-ttu-id="6f4ba-166">`ReturnUrlParameter`: ReturnUrlParameter determina el nombre del parámetro de cadena de consulta que se anexa el middleware cuando se cambia un código de estado sin autorización 401 a una redirección 302 en la ruta de acceso de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-166">`ReturnUrlParameter`: The ReturnUrlParameter determines the name of the query string parameter which is appended by the middleware when a 401 Unauthorized status code is changed to a 302 redirect onto the login path.</span></span>
* <span data-ttu-id="6f4ba-167">`AuthenticationScheme`: Esto solo es relevante para ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-167">`AuthenticationScheme`: This is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="6f4ba-168">El nombre lógico para un esquema de autenticación concreto.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-168">The logical name for a particular authentication scheme.</span></span>
* <span data-ttu-id="6f4ba-169">`AutomaticAuthenticate`: Esta marca solo es pertinente para ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-169">`AutomaticAuthenticate`: This flag is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="6f4ba-170">Cuando sea true, autenticación con cookies debe ejecutar en cada solicitud y se intentan validar y reconstruir cualquier entidad de seguridad serializado que creó.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-170">When true, cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>

