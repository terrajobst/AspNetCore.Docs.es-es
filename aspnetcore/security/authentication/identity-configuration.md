---
title: Configurar la identidad de ASP.NET Core
author: AdrienTorris
description: Comprender los valores predeterminados de ASP.NET Core Identity y configurar las distintas propiedades de identidad para usar valores personalizados.
manager: wpickett
ms.author: scaddie
ms.date: 01/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-configuration
ms.openlocfilehash: cf7dcdb80f5edf9e10960cb08957793c36829a69
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="configure-identity"></a><span data-ttu-id="92f51-103">Configurar la identidad</span><span class="sxs-lookup"><span data-stu-id="92f51-103">Configure Identity</span></span>

<span data-ttu-id="92f51-104">ASP.NET Core Identity tiene comportamientos comunes en las aplicaciones, como directiva de contraseñas y tiempo de bloqueo, configuración de cookies que se puede reemplazar fácilmente en la aplicación `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="92f51-104">ASP.NET Core Identity has common behaviors in applications such as password policy, lockout time, and cookie settings that you can override easily in your application's `Startup` class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="92f51-105">Directiva de contraseñas</span><span class="sxs-lookup"><span data-stu-id="92f51-105">Passwords policy</span></span>

<span data-ttu-id="92f51-106">De forma predeterminada, identidad requiere que las contraseñas contengan un carácter en mayúsculas, caracteres en minúsculas, un dígito y un carácter no alfanumérico.</span><span class="sxs-lookup"><span data-stu-id="92f51-106">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="92f51-107">También hay algunas otras restricciones.</span><span class="sxs-lookup"><span data-stu-id="92f51-107">There are also some other restrictions.</span></span> <span data-ttu-id="92f51-108">Para simplificar las restricciones de contraseña, modifique la `ConfigureServices` método de la `Startup` clase de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="92f51-108">To simplify password restrictions, modify the `ConfigureServices` method of the `Startup` class of your application.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="92f51-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="92f51-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="92f51-110">Núcleo de ASP.NET 2.0 agregado la `RequiredUniqueChars` propiedad.</span><span class="sxs-lookup"><span data-stu-id="92f51-110">ASP.NET Core 2.0 added the `RequiredUniqueChars` property.</span></span> <span data-ttu-id="92f51-111">De lo contrario, las opciones son las mismas de ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="92f51-111">Otherwise, the options are the same from ASP.NET Core 1.x.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="92f51-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="92f51-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

<span data-ttu-id="92f51-113">`IdentityOptions.Password`tiene las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="92f51-113">`IdentityOptions.Password` has the following properties:</span></span>

| <span data-ttu-id="92f51-114">Property</span><span class="sxs-lookup"><span data-stu-id="92f51-114">Property</span></span>                | <span data-ttu-id="92f51-115">Descripción</span><span class="sxs-lookup"><span data-stu-id="92f51-115">Description</span></span>                       | <span data-ttu-id="92f51-116">Default</span><span class="sxs-lookup"><span data-stu-id="92f51-116">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireDigit`          | <span data-ttu-id="92f51-117">Requiere un número entre 0-9 en la contraseña.</span><span class="sxs-lookup"><span data-stu-id="92f51-117">Requires a number between 0-9 in the password.</span></span> | <span data-ttu-id="92f51-118">true</span><span class="sxs-lookup"><span data-stu-id="92f51-118">true</span></span> |
| `RequiredLength`        | <span data-ttu-id="92f51-119">La longitud mínima de la contraseña.</span><span class="sxs-lookup"><span data-stu-id="92f51-119">The minimum length of the password.</span></span> | <span data-ttu-id="92f51-120">6</span><span class="sxs-lookup"><span data-stu-id="92f51-120">6</span></span> |
| `RequireNonAlphanumeric`| <span data-ttu-id="92f51-121">Requiere un carácter que no sean alfanuméricos en la contraseña.</span><span class="sxs-lookup"><span data-stu-id="92f51-121">Requires a non-alphanumeric character in the password.</span></span> | <span data-ttu-id="92f51-122">true</span><span class="sxs-lookup"><span data-stu-id="92f51-122">true</span></span> |
| `RequireUppercase`      | <span data-ttu-id="92f51-123">Requiere un carácter en mayúscula en la contraseña.</span><span class="sxs-lookup"><span data-stu-id="92f51-123">Requires an upper case character in the password.</span></span> | <span data-ttu-id="92f51-124">true</span><span class="sxs-lookup"><span data-stu-id="92f51-124">true</span></span> |
| `RequireLowercase`      | <span data-ttu-id="92f51-125">Requiere un carácter en minúsculas de la contraseña.</span><span class="sxs-lookup"><span data-stu-id="92f51-125">Requires a lower case character in the password.</span></span> | <span data-ttu-id="92f51-126">true</span><span class="sxs-lookup"><span data-stu-id="92f51-126">true</span></span> |
| `RequiredUniqueChars`   | <span data-ttu-id="92f51-127">Requiere el número de caracteres distintos de la contraseña.</span><span class="sxs-lookup"><span data-stu-id="92f51-127">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="92f51-128">1</span><span class="sxs-lookup"><span data-stu-id="92f51-128">1</span></span> |


## <a name="users-lockout"></a><span data-ttu-id="92f51-129">Bloqueo del usuario</span><span class="sxs-lookup"><span data-stu-id="92f51-129">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

<span data-ttu-id="92f51-130">`IdentityOptions.Lockout`tiene las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="92f51-130">`IdentityOptions.Lockout` has the following properties:</span></span>

| <span data-ttu-id="92f51-131">Property</span><span class="sxs-lookup"><span data-stu-id="92f51-131">Property</span></span>                | <span data-ttu-id="92f51-132">Descripción</span><span class="sxs-lookup"><span data-stu-id="92f51-132">Description</span></span>                       | <span data-ttu-id="92f51-133">Default</span><span class="sxs-lookup"><span data-stu-id="92f51-133">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `DefaultLockoutTimeSpan` | <span data-ttu-id="92f51-134">La cantidad de tiempo que un usuario está bloqueado cuando se produce un bloqueo.</span><span class="sxs-lookup"><span data-stu-id="92f51-134">The amount of time a user is locked out when a lockout occurs.</span></span>  | <span data-ttu-id="92f51-135">5 minutos</span><span class="sxs-lookup"><span data-stu-id="92f51-135">5 minutes</span></span>  |
| `MaxFailedAccessAttempts` | <span data-ttu-id="92f51-136">El número de intentos de acceso erróneos hasta que se bloquea un usuario, si está habilitado el bloqueo.</span><span class="sxs-lookup"><span data-stu-id="92f51-136">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span>  | <span data-ttu-id="92f51-137">5</span><span class="sxs-lookup"><span data-stu-id="92f51-137">5</span></span> |
| `AllowedForNewUsers` | <span data-ttu-id="92f51-138">Determina si un usuario nuevo puede bloquearse.</span><span class="sxs-lookup"><span data-stu-id="92f51-138">Determines if a new user can be locked out.</span></span>  | <span data-ttu-id="92f51-139">true</span><span class="sxs-lookup"><span data-stu-id="92f51-139">true</span></span> |

## <a name="sign-in-settings"></a><span data-ttu-id="92f51-140">Inicie sesión en la configuración</span><span class="sxs-lookup"><span data-stu-id="92f51-140">Sign in settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

<span data-ttu-id="92f51-141">`IdentityOptions.SignIn`tiene las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="92f51-141">`IdentityOptions.SignIn` has the following properties:</span></span>

| <span data-ttu-id="92f51-142">Property</span><span class="sxs-lookup"><span data-stu-id="92f51-142">Property</span></span>                | <span data-ttu-id="92f51-143">Descripción</span><span class="sxs-lookup"><span data-stu-id="92f51-143">Description</span></span>                       | <span data-ttu-id="92f51-144">Default</span><span class="sxs-lookup"><span data-stu-id="92f51-144">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireConfirmedEmail` | <span data-ttu-id="92f51-145">Requiere un correo electrónico confirmado para iniciar sesión en.</span><span class="sxs-lookup"><span data-stu-id="92f51-145">Requires a confirmed email to sign in.</span></span> | <span data-ttu-id="92f51-146">False</span><span class="sxs-lookup"><span data-stu-id="92f51-146">false</span></span>  |
| `RequireConfirmedPhoneNumber` |  <span data-ttu-id="92f51-147">Requiere un número de teléfono confirmada iniciar sesión en.</span><span class="sxs-lookup"><span data-stu-id="92f51-147">Requires a confirmed phone number to sign in.</span></span> | <span data-ttu-id="92f51-148">False</span><span class="sxs-lookup"><span data-stu-id="92f51-148">false</span></span>  |

## <a name="user-validation-settings"></a><span data-ttu-id="92f51-149">Configuración de la validación de usuario</span><span class="sxs-lookup"><span data-stu-id="92f51-149">User validation settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

<span data-ttu-id="92f51-150">`IdentityOptions.User`tiene las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="92f51-150">`IdentityOptions.User` has the following properties:</span></span>

| <span data-ttu-id="92f51-151">Property</span><span class="sxs-lookup"><span data-stu-id="92f51-151">Property</span></span>                | <span data-ttu-id="92f51-152">Descripción</span><span class="sxs-lookup"><span data-stu-id="92f51-152">Description</span></span>                       | <span data-ttu-id="92f51-153">Default</span><span class="sxs-lookup"><span data-stu-id="92f51-153">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireUniqueEmail`  | <span data-ttu-id="92f51-154">Requiere que cada usuario tiene un correo electrónico único.</span><span class="sxs-lookup"><span data-stu-id="92f51-154">Requires each User to have a unique email.</span></span> | <span data-ttu-id="92f51-155">False</span><span class="sxs-lookup"><span data-stu-id="92f51-155">false</span></span>  |
| `AllowedUserNameCharacters`  | <span data-ttu-id="92f51-156">Caracteres permitidos en el nombre de usuario.</span><span class="sxs-lookup"><span data-stu-id="92f51-156">Allowed characters in the username.</span></span> | <span data-ttu-id="92f51-157">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span><span class="sxs-lookup"><span data-stu-id="92f51-157">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span></span> |


## <a name="applications-cookie-settings"></a><span data-ttu-id="92f51-158">Configuración de cookies de la aplicación</span><span class="sxs-lookup"><span data-stu-id="92f51-158">Application's cookie settings</span></span>

<span data-ttu-id="92f51-159">Al igual que la directiva de contraseñas, todos los valores de cookie de la aplicación pueden cambiarse en el `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="92f51-159">Like the passwords policy, all the settings of the application's cookie can be changed in the `Startup` class.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="92f51-160">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="92f51-160">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="92f51-161">En `ConfigureServices` en la `Startup` (clase), puede configurar la cookie de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="92f51-161">Under `ConfigureServices` in the `Startup` class, you can configure the application's cookie.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="92f51-162">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="92f51-162">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

<span data-ttu-id="92f51-163">`CookieAuthenticationOptions`tiene las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="92f51-163">`CookieAuthenticationOptions` has the following properties:</span></span>

| <span data-ttu-id="92f51-164">Property</span><span class="sxs-lookup"><span data-stu-id="92f51-164">Property</span></span>                | <span data-ttu-id="92f51-165">Descripción</span><span class="sxs-lookup"><span data-stu-id="92f51-165">Description</span></span>                       | <span data-ttu-id="92f51-166">Default</span><span class="sxs-lookup"><span data-stu-id="92f51-166">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `Cookie.Name`  | <span data-ttu-id="92f51-167">El nombre de la cookie.</span><span class="sxs-lookup"><span data-stu-id="92f51-167">The name of the cookie.</span></span>  | <span data-ttu-id="92f51-168">.AspNetCore.Cookies.</span><span class="sxs-lookup"><span data-stu-id="92f51-168">.AspNetCore.Cookies.</span></span>  |
| `Cookie.HttpOnly`  | <span data-ttu-id="92f51-169">Cuando sea true, la cookie no es accesible desde scripts del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="92f51-169">When true, the cookie isn't accessible from client-side scripts.</span></span>  |  <span data-ttu-id="92f51-170">true</span><span class="sxs-lookup"><span data-stu-id="92f51-170">true</span></span> |
| `ExpireTimeSpan`  | <span data-ttu-id="92f51-171">Controla cuánto tiempo el vale de autenticación almacenado en la cookie será válida desde el punto en que se crea.</span><span class="sxs-lookup"><span data-stu-id="92f51-171">Controls how much time the authentication ticket stored in the cookie will remain valid from the point it's created.</span></span>  | <span data-ttu-id="92f51-172">14 días</span><span class="sxs-lookup"><span data-stu-id="92f51-172">14 days</span></span>  |
| `LoginPath`  | <span data-ttu-id="92f51-173">Cuando un usuario está autorizado, se redirigirán a esta ruta de acceso al inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="92f51-173">When a user is unauthorized, they will be redirected to this path to login.</span></span> | <span data-ttu-id="92f51-174">/ Cuenta/inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="92f51-174">/Account/Login</span></span>  |
| `LogoutPath`  | <span data-ttu-id="92f51-175">Cuando un usuario se cerró la sesión, se redirigirán a esta ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="92f51-175">When a user is logged out, they will be redirected to this path.</span></span>  | <span data-ttu-id="92f51-176">/ Cuenta/cierre de sesión</span><span class="sxs-lookup"><span data-stu-id="92f51-176">/Account/Logout</span></span>  |
| `AccessDeniedPath`  | <span data-ttu-id="92f51-177">Cuando un usuario se produce un error de comprobación de la autorización, se redirigirán a esta ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="92f51-177">When a user fails an authorization check, they will be redirected to this path.</span></span>  |   |
| `SlidingExpiration`  | <span data-ttu-id="92f51-178">Cuando sea true, se emitirá una cookie nueva con una nueva hora de expiración cuando la cookie actual está en más de medio a través de la ventana de expiración.</span><span class="sxs-lookup"><span data-stu-id="92f51-178">When true, a new cookie will be issued with a new expiration time when the current cookie is more than halfway through the expiration window.</span></span>  | <span data-ttu-id="92f51-179">/ Cuenta/AccessDenied</span><span class="sxs-lookup"><span data-stu-id="92f51-179">/Account/AccessDenied</span></span> |
| `ReturnUrlParameter`  | <span data-ttu-id="92f51-180">Determina el nombre del parámetro de cadena de consulta que se anexa el middleware cuando se cambia un código de estado sin autorización 401 a una redirección 302 en la ruta de acceso de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="92f51-180">Determines the name of the query string parameter which is appended by the middleware when a 401 Unauthorized status code is changed to a 302 redirect onto the login path.</span></span>  |  <span data-ttu-id="92f51-181">true</span><span class="sxs-lookup"><span data-stu-id="92f51-181">true</span></span> |
| `AuthenticationScheme`  | <span data-ttu-id="92f51-182">Esto solo es relevante para ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="92f51-182">This is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="92f51-183">El nombre lógico para un esquema de autenticación concreto.</span><span class="sxs-lookup"><span data-stu-id="92f51-183">The logical name for a particular authentication scheme.</span></span> |  |
| `AutomaticAuthenticate`  | <span data-ttu-id="92f51-184">Esta marca solo es pertinente para ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="92f51-184">This flag is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="92f51-185">Cuando sea true, autenticación con cookies debe ejecutar en cada solicitud y se intentan validar y reconstruir cualquier entidad de seguridad serializado que creó.</span><span class="sxs-lookup"><span data-stu-id="92f51-185">When true, cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>  |  |
