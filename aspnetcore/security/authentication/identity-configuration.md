---
title: Configurar la identidad de ASP.NET Core
author: AdrienTorris
description: Comprender los valores predeterminados de ASP.NET Core Identity y configurar las distintas propiedades de identidad para usar valores personalizados.
keywords: "Autenticación de ASP.NET Core, identidad, seguridad"
ms.author: scaddie
manager: wpickett
ms.date: 01/11/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: ac204cb89aac1f90adc64c4f0bec4e946cb8c4d9
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2018
---
# <a name="configure-identity"></a><span data-ttu-id="1d29d-104">Configurar la identidad</span><span class="sxs-lookup"><span data-stu-id="1d29d-104">Configure Identity</span></span>

<span data-ttu-id="1d29d-105">ASP.NET Core Identity tiene comportamientos comunes en las aplicaciones, como directiva de contraseñas y tiempo de bloqueo, configuración de cookies que se puede reemplazar fácilmente en la aplicación `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="1d29d-105">ASP.NET Core Identity has common behaviors in applications such as password policy, lockout time, and cookie settings that you can override easily in your application's `Startup` class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="1d29d-106">Directiva de contraseñas</span><span class="sxs-lookup"><span data-stu-id="1d29d-106">Passwords policy</span></span>

<span data-ttu-id="1d29d-107">De forma predeterminada, identidad requiere que las contraseñas contengan un carácter en mayúsculas, caracteres en minúsculas, un dígito y un carácter no alfanumérico.</span><span class="sxs-lookup"><span data-stu-id="1d29d-107">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="1d29d-108">También hay algunas otras restricciones.</span><span class="sxs-lookup"><span data-stu-id="1d29d-108">There are also some other restrictions.</span></span> <span data-ttu-id="1d29d-109">Para simplificar las restricciones de contraseña, modifique la `ConfigureServices` método de la `Startup` clase de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1d29d-109">To simplify password restrictions, modify the `ConfigureServices` method of the `Startup` class of your application.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1d29d-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1d29d-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="1d29d-111">Núcleo de ASP.NET 2.0 agregado la `RequiredUniqueChars` propiedad.</span><span class="sxs-lookup"><span data-stu-id="1d29d-111">ASP.NET Core 2.0 added the `RequiredUniqueChars` property.</span></span> <span data-ttu-id="1d29d-112">De lo contrario, las opciones son las mismas de ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="1d29d-112">Otherwise, the options are the same from ASP.NET Core 1.x.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1d29d-113">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1d29d-113">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

<span data-ttu-id="1d29d-114">`IdentityOptions.Password`tiene las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="1d29d-114">`IdentityOptions.Password` has the following properties:</span></span>

| <span data-ttu-id="1d29d-115">Property</span><span class="sxs-lookup"><span data-stu-id="1d29d-115">Property</span></span>                | <span data-ttu-id="1d29d-116">Descripción</span><span class="sxs-lookup"><span data-stu-id="1d29d-116">Description</span></span>                       | <span data-ttu-id="1d29d-117">Default</span><span class="sxs-lookup"><span data-stu-id="1d29d-117">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireDigit`          | <span data-ttu-id="1d29d-118">Requiere un número entre 0-9 en la contraseña.</span><span class="sxs-lookup"><span data-stu-id="1d29d-118">Requires a number between 0-9 in the password.</span></span> | <span data-ttu-id="1d29d-119">true</span><span class="sxs-lookup"><span data-stu-id="1d29d-119">true</span></span> |
| `RequiredLength`        | <span data-ttu-id="1d29d-120">La longitud mínima de la contraseña.</span><span class="sxs-lookup"><span data-stu-id="1d29d-120">The minimum length of the password.</span></span> | <span data-ttu-id="1d29d-121">6</span><span class="sxs-lookup"><span data-stu-id="1d29d-121">6</span></span> |
| `RequireNonAlphanumeric`| <span data-ttu-id="1d29d-122">Requiere un carácter que no sean alfanuméricos en la contraseña.</span><span class="sxs-lookup"><span data-stu-id="1d29d-122">Requires a non-alphanumeric character in the password.</span></span> | <span data-ttu-id="1d29d-123">true</span><span class="sxs-lookup"><span data-stu-id="1d29d-123">true</span></span> |
| `RequireUppercase`      | <span data-ttu-id="1d29d-124">Requiere un carácter en mayúscula en la contraseña.</span><span class="sxs-lookup"><span data-stu-id="1d29d-124">Requires an upper case character in the password.</span></span> | <span data-ttu-id="1d29d-125">true</span><span class="sxs-lookup"><span data-stu-id="1d29d-125">true</span></span> |
| `RequireLowercase`      | <span data-ttu-id="1d29d-126">Requiere un carácter en minúsculas de la contraseña.</span><span class="sxs-lookup"><span data-stu-id="1d29d-126">Requires a lower case character in the password.</span></span> | <span data-ttu-id="1d29d-127">true</span><span class="sxs-lookup"><span data-stu-id="1d29d-127">true</span></span> |
| `RequiredUniqueChars`   | <span data-ttu-id="1d29d-128">Requiere el número de caracteres distintos de la contraseña.</span><span class="sxs-lookup"><span data-stu-id="1d29d-128">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="1d29d-129">1</span><span class="sxs-lookup"><span data-stu-id="1d29d-129">1</span></span> |


## <a name="users-lockout"></a><span data-ttu-id="1d29d-130">Bloqueo del usuario</span><span class="sxs-lookup"><span data-stu-id="1d29d-130">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

<span data-ttu-id="1d29d-131">`IdentityOptions.Lockout`tiene las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="1d29d-131">`IdentityOptions.Lockout` has the following properties:</span></span>

| <span data-ttu-id="1d29d-132">Property</span><span class="sxs-lookup"><span data-stu-id="1d29d-132">Property</span></span>                | <span data-ttu-id="1d29d-133">Descripción</span><span class="sxs-lookup"><span data-stu-id="1d29d-133">Description</span></span>                       | <span data-ttu-id="1d29d-134">Default</span><span class="sxs-lookup"><span data-stu-id="1d29d-134">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `DefaultLockoutTimeSpan` | <span data-ttu-id="1d29d-135">La cantidad de tiempo que un usuario está bloqueado cuando se produce un bloqueo.</span><span class="sxs-lookup"><span data-stu-id="1d29d-135">The amount of time a user is locked out when a lockout occurs.</span></span>  | <span data-ttu-id="1d29d-136">5 minutos</span><span class="sxs-lookup"><span data-stu-id="1d29d-136">5 minutes</span></span>  |
| `MaxFailedAccessAttempts` | <span data-ttu-id="1d29d-137">El número de intentos de acceso erróneos hasta que se bloquea un usuario, si está habilitado el bloqueo.</span><span class="sxs-lookup"><span data-stu-id="1d29d-137">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span>  | <span data-ttu-id="1d29d-138">5</span><span class="sxs-lookup"><span data-stu-id="1d29d-138">5</span></span> |
| `AllowedForNewUsers` | <span data-ttu-id="1d29d-139">Determina si un usuario nuevo puede bloquearse.</span><span class="sxs-lookup"><span data-stu-id="1d29d-139">Determines if a new user can be locked out.</span></span>  | <span data-ttu-id="1d29d-140">true</span><span class="sxs-lookup"><span data-stu-id="1d29d-140">true</span></span> |

## <a name="sign-in-settings"></a><span data-ttu-id="1d29d-141">Inicie sesión en la configuración</span><span class="sxs-lookup"><span data-stu-id="1d29d-141">Sign in settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

<span data-ttu-id="1d29d-142">`IdentityOptions.SignIn`tiene las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="1d29d-142">`IdentityOptions.SignIn` has the following properties:</span></span>

| <span data-ttu-id="1d29d-143">Property</span><span class="sxs-lookup"><span data-stu-id="1d29d-143">Property</span></span>                | <span data-ttu-id="1d29d-144">Descripción</span><span class="sxs-lookup"><span data-stu-id="1d29d-144">Description</span></span>                       | <span data-ttu-id="1d29d-145">Default</span><span class="sxs-lookup"><span data-stu-id="1d29d-145">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireConfirmedEmail` | <span data-ttu-id="1d29d-146">Requiere un correo electrónico confirmado para iniciar sesión en.</span><span class="sxs-lookup"><span data-stu-id="1d29d-146">Requires a confirmed email to sign in.</span></span> | <span data-ttu-id="1d29d-147">False</span><span class="sxs-lookup"><span data-stu-id="1d29d-147">false</span></span>  |
| `RequireConfirmedPhoneNumber` |  <span data-ttu-id="1d29d-148">Requiere un número de teléfono confirmada iniciar sesión en.</span><span class="sxs-lookup"><span data-stu-id="1d29d-148">Requires a confirmed phone number to sign in.</span></span> | <span data-ttu-id="1d29d-149">False</span><span class="sxs-lookup"><span data-stu-id="1d29d-149">false</span></span>  |

## <a name="user-validation-settings"></a><span data-ttu-id="1d29d-150">Configuración de la validación de usuario</span><span class="sxs-lookup"><span data-stu-id="1d29d-150">User validation settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

<span data-ttu-id="1d29d-151">`IdentityOptions.User`tiene las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="1d29d-151">`IdentityOptions.User` has the following properties:</span></span>

| <span data-ttu-id="1d29d-152">Property</span><span class="sxs-lookup"><span data-stu-id="1d29d-152">Property</span></span>                | <span data-ttu-id="1d29d-153">Descripción</span><span class="sxs-lookup"><span data-stu-id="1d29d-153">Description</span></span>                       | <span data-ttu-id="1d29d-154">Default</span><span class="sxs-lookup"><span data-stu-id="1d29d-154">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireUniqueEmail`  | <span data-ttu-id="1d29d-155">Requiere que cada usuario tiene un correo electrónico único.</span><span class="sxs-lookup"><span data-stu-id="1d29d-155">Requires each User to have a unique email.</span></span> | <span data-ttu-id="1d29d-156">False</span><span class="sxs-lookup"><span data-stu-id="1d29d-156">false</span></span>  |
| `AllowedUserNameCharacters`  | <span data-ttu-id="1d29d-157">Caracteres permitidos en el nombre de usuario.</span><span class="sxs-lookup"><span data-stu-id="1d29d-157">Allowed characters in the username.</span></span> | <span data-ttu-id="1d29d-158">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span><span class="sxs-lookup"><span data-stu-id="1d29d-158">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span></span> |


## <a name="applications-cookie-settings"></a><span data-ttu-id="1d29d-159">Configuración de cookies de la aplicación</span><span class="sxs-lookup"><span data-stu-id="1d29d-159">Application's cookie settings</span></span>

<span data-ttu-id="1d29d-160">Al igual que la directiva de contraseñas, todos los valores de cookie de la aplicación pueden cambiarse en el `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="1d29d-160">Like the passwords policy, all the settings of the application's cookie can be changed in the `Startup` class.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1d29d-161">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1d29d-161">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="1d29d-162">En `ConfigureServices` en la `Startup` (clase), puede configurar la cookie de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1d29d-162">Under `ConfigureServices` in the `Startup` class, you can configure the application's cookie.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1d29d-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1d29d-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

<span data-ttu-id="1d29d-164">`CookieAuthenticationOptions`tiene las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="1d29d-164">`CookieAuthenticationOptions` has the following properties:</span></span>

| <span data-ttu-id="1d29d-165">Property</span><span class="sxs-lookup"><span data-stu-id="1d29d-165">Property</span></span>                | <span data-ttu-id="1d29d-166">Descripción</span><span class="sxs-lookup"><span data-stu-id="1d29d-166">Description</span></span>                       | <span data-ttu-id="1d29d-167">Default</span><span class="sxs-lookup"><span data-stu-id="1d29d-167">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `Cookie.Name`  | <span data-ttu-id="1d29d-168">El nombre de la cookie.</span><span class="sxs-lookup"><span data-stu-id="1d29d-168">The name of the cookie.</span></span>  | <span data-ttu-id="1d29d-169">. AspNetCore.Cookies.</span><span class="sxs-lookup"><span data-stu-id="1d29d-169">.AspNetCore.Cookies.</span></span>  |
| `Cookie.HttpOnly`  | <span data-ttu-id="1d29d-170">Cuando sea true, la cookie no es accesible desde scripts del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="1d29d-170">When true, the cookie is not accessible from client-side scripts.</span></span>  |  <span data-ttu-id="1d29d-171">true</span><span class="sxs-lookup"><span data-stu-id="1d29d-171">true</span></span> |
| `ExpireTimeSpan`  | <span data-ttu-id="1d29d-172">Controla cuánto tiempo el vale de autenticación almacenado en la cookie será válida desde el punto en que se crea.</span><span class="sxs-lookup"><span data-stu-id="1d29d-172">Controls how much time the authentication ticket stored in the cookie will remain valid from the point it is created.</span></span>  | <span data-ttu-id="1d29d-173">14 días</span><span class="sxs-lookup"><span data-stu-id="1d29d-173">14 days</span></span>  |
| `LoginPath`  | <span data-ttu-id="1d29d-174">Cuando un usuario está autorizado, se redirigirán a esta ruta de acceso al inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="1d29d-174">When a user is unauthorized, they will be redirected to this path to login.</span></span> | <span data-ttu-id="1d29d-175">/ Cuenta/inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="1d29d-175">/Account/Login</span></span>  |
| `LogoutPath`  | <span data-ttu-id="1d29d-176">Cuando un usuario se cerró la sesión, se redirigirán a esta ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="1d29d-176">When a user is logged out, they will be redirected to this path.</span></span>  | <span data-ttu-id="1d29d-177">/ Cuenta/cierre de sesión</span><span class="sxs-lookup"><span data-stu-id="1d29d-177">/Account/Logout</span></span>  |
| `AccessDeniedPath`  | <span data-ttu-id="1d29d-178">Cuando un usuario se produce un error de comprobación de la autorización, se redirigirán a esta ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="1d29d-178">When a user fails an authorization check, they will be redirected to this path.</span></span>  |   |
| `SlidingExpiration`  | <span data-ttu-id="1d29d-179">Cuando sea true, se emitirá una cookie nueva con una nueva hora de expiración cuando la cookie actual está en más de medio a través de la ventana de expiración.</span><span class="sxs-lookup"><span data-stu-id="1d29d-179">When true, a new cookie will be issued with a new expiration time when the current cookie is more than halfway through the expiration window.</span></span>  | <span data-ttu-id="1d29d-180">/ Cuenta/AccessDenied</span><span class="sxs-lookup"><span data-stu-id="1d29d-180">/Account/AccessDenied</span></span> |
| `ReturnUrlParameter`  | <span data-ttu-id="1d29d-181">Determina el nombre del parámetro de cadena de consulta que se anexa el middleware cuando se cambia un código de estado sin autorización 401 a una redirección 302 en la ruta de acceso de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="1d29d-181">Determines the name of the query string parameter which is appended by the middleware when a 401 Unauthorized status code is changed to a 302 redirect onto the login path.</span></span>  |  <span data-ttu-id="1d29d-182">true</span><span class="sxs-lookup"><span data-stu-id="1d29d-182">true</span></span> |
| `AuthenticationScheme`  | <span data-ttu-id="1d29d-183">Esto solo es relevante para ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="1d29d-183">This is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="1d29d-184">El nombre lógico para un esquema de autenticación concreto.</span><span class="sxs-lookup"><span data-stu-id="1d29d-184">The logical name for a particular authentication scheme.</span></span> |  |
| `AutomaticAuthenticate`  | <span data-ttu-id="1d29d-185">Esta marca solo es pertinente para ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="1d29d-185">This flag is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="1d29d-186">Cuando sea true, autenticación con cookies debe ejecutar en cada solicitud y se intentan validar y reconstruir cualquier entidad de seguridad serializado que creó.</span><span class="sxs-lookup"><span data-stu-id="1d29d-186">When true, cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>  |  |