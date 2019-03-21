---
title: Configurar la identidad de ASP.NET Core
author: AdrienTorris
description: Comprender los valores predeterminados de ASP.NET Core Identity y obtenga información sobre cómo configurar las propiedades de identidad para usar valores personalizados.
ms.author: riande
ms.date: 02/11/2019
uid: security/authentication/identity-configuration
ms.openlocfilehash: 823182bed2cb953e07f9374d135868aeb2be9c60
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210124"
---
# <a name="configure-aspnet-core-identity"></a><span data-ttu-id="34f8d-103">Configurar la identidad de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="34f8d-103">Configure ASP.NET Core Identity</span></span>

<span data-ttu-id="34f8d-104">ASP.NET Core Identity utiliza valores predeterminados para la configuración de directiva de contraseñas, bloqueo y la configuración de la cookie.</span><span class="sxs-lookup"><span data-stu-id="34f8d-104">ASP.NET Core Identity uses default values for settings such as password policy, lockout, and cookie configuration.</span></span> <span data-ttu-id="34f8d-105">Esta configuración puede invalidarse en la `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="34f8d-105">These settings can be overridden in the `Startup` class.</span></span>

## <a name="identity-options"></a><span data-ttu-id="34f8d-106">Opciones de identidad</span><span class="sxs-lookup"><span data-stu-id="34f8d-106">Identity options</span></span>

<span data-ttu-id="34f8d-107">El [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) clase representa las opciones que pueden usarse para configurar el sistema de identidad.</span><span class="sxs-lookup"><span data-stu-id="34f8d-107">The [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) class represents the options that can be used to configure the Identity system.</span></span> <span data-ttu-id="34f8d-108">`IdentityOptions` se debe establecer **después** llamada `AddIdentity` o `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="34f8d-108">`IdentityOptions` must be set **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

### <a name="claims-identity"></a><span data-ttu-id="34f8d-109">Identidad de notificaciones</span><span class="sxs-lookup"><span data-stu-id="34f8d-109">Claims Identity</span></span>

<span data-ttu-id="34f8d-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) especifica la [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) con las propiedades mostradas en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="34f8d-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) specifies the [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) with the properties shown in the following table.</span></span>

| <span data-ttu-id="34f8d-111">Property</span><span class="sxs-lookup"><span data-stu-id="34f8d-111">Property</span></span> | <span data-ttu-id="34f8d-112">Descripción</span><span class="sxs-lookup"><span data-stu-id="34f8d-112">Description</span></span> | <span data-ttu-id="34f8d-113">Default</span><span class="sxs-lookup"><span data-stu-id="34f8d-113">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="34f8d-114">RoleClaimType</span><span class="sxs-lookup"><span data-stu-id="34f8d-114">RoleClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | <span data-ttu-id="34f8d-115">Obtiene o establece el tipo de notificación utilizado para una notificación de rol.</span><span class="sxs-lookup"><span data-stu-id="34f8d-115">Gets or sets the claim type used for a role claim.</span></span> | [<span data-ttu-id="34f8d-116">ClaimTypes.Role</span><span class="sxs-lookup"><span data-stu-id="34f8d-116">ClaimTypes.Role</span></span>](/dotnet/api/system.security.claims.claimtypes.role) |
| [<span data-ttu-id="34f8d-117">SecurityStampClaimType</span><span class="sxs-lookup"><span data-stu-id="34f8d-117">SecurityStampClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | <span data-ttu-id="34f8d-118">Obtiene o establece el tipo de notificación utilizado para la notificación de marca de seguridad.</span><span class="sxs-lookup"><span data-stu-id="34f8d-118">Gets or sets the claim type used for the security stamp claim.</span></span> | `AspNet.Identity.SecurityStamp` |
| [<span data-ttu-id="34f8d-119">UserIdClaimType</span><span class="sxs-lookup"><span data-stu-id="34f8d-119">UserIdClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | <span data-ttu-id="34f8d-120">Obtiene o establece el tipo de notificación utilizado para la notificación de identificador de usuario.</span><span class="sxs-lookup"><span data-stu-id="34f8d-120">Gets or sets the claim type used for the user identifier claim.</span></span> | [<span data-ttu-id="34f8d-121">ClaimTypes.NameIdentifier</span><span class="sxs-lookup"><span data-stu-id="34f8d-121">ClaimTypes.NameIdentifier</span></span>](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [<span data-ttu-id="34f8d-122">UserNameClaimType</span><span class="sxs-lookup"><span data-stu-id="34f8d-122">UserNameClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | <span data-ttu-id="34f8d-123">Obtiene o establece el tipo de notificación utilizado para la notificación de nombre de usuario.</span><span class="sxs-lookup"><span data-stu-id="34f8d-123">Gets or sets the claim type used for the user name claim.</span></span> | [<span data-ttu-id="34f8d-124">ClaimTypes.Name</span><span class="sxs-lookup"><span data-stu-id="34f8d-124">ClaimTypes.Name</span></span>](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a><span data-ttu-id="34f8d-125">Bloqueo</span><span class="sxs-lookup"><span data-stu-id="34f8d-125">Lockout</span></span>

<span data-ttu-id="34f8d-126">El bloqueo se establece el [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) método:</span><span class="sxs-lookup"><span data-stu-id="34f8d-126">Lockout is set in the [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) method:</span></span>

[!code-csharp[](identity-configuration/sample/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="34f8d-127">El código anterior se basa en el `Login` plantilla de identidad.</span><span class="sxs-lookup"><span data-stu-id="34f8d-127">The preceding code is based on the `Login` Identity template.</span></span> 

<span data-ttu-id="34f8d-128">Opciones de bloqueo están establecidas `StartUp.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="34f8d-128">Lockout options are set in `StartUp.ConfigureServices`:</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_lock)]

<span data-ttu-id="34f8d-129">El código anterior establece el [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) con los valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="34f8d-129">The preceding code sets the [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with default values.</span></span>

<span data-ttu-id="34f8d-130">Una autenticación correcta restablece el número de intentos de acceso erróneos y restablece el reloj.</span><span class="sxs-lookup"><span data-stu-id="34f8d-130">A successful authentication resets the failed access attempts count and resets the clock.</span></span>

<span data-ttu-id="34f8d-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) especifica la [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) con las propiedades mostradas en la tabla.</span><span class="sxs-lookup"><span data-stu-id="34f8d-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) specifies the [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="34f8d-132">Property</span><span class="sxs-lookup"><span data-stu-id="34f8d-132">Property</span></span> | <span data-ttu-id="34f8d-133">Descripción</span><span class="sxs-lookup"><span data-stu-id="34f8d-133">Description</span></span> | <span data-ttu-id="34f8d-134">Default</span><span class="sxs-lookup"><span data-stu-id="34f8d-134">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="34f8d-135">AllowedForNewUsers</span><span class="sxs-lookup"><span data-stu-id="34f8d-135">AllowedForNewUsers</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | <span data-ttu-id="34f8d-136">Determina si un usuario nuevo puede estar bloqueado.</span><span class="sxs-lookup"><span data-stu-id="34f8d-136">Determines if a new user can be locked out.</span></span> | `true` |
| [<span data-ttu-id="34f8d-137">DefaultLockoutTimeSpan</span><span class="sxs-lookup"><span data-stu-id="34f8d-137">DefaultLockoutTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | <span data-ttu-id="34f8d-138">La cantidad de tiempo un usuario está bloqueado cuando se produce un bloqueo.</span><span class="sxs-lookup"><span data-stu-id="34f8d-138">The amount of time a user is locked out when a lockout occurs.</span></span> | <span data-ttu-id="34f8d-139">5 minutos</span><span class="sxs-lookup"><span data-stu-id="34f8d-139">5 minutes</span></span> |
| [<span data-ttu-id="34f8d-140">MaxFailedAccessAttempts</span><span class="sxs-lookup"><span data-stu-id="34f8d-140">MaxFailedAccessAttempts</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | <span data-ttu-id="34f8d-141">El número de intentos de acceso erróneos hasta que un usuario está bloqueado, si está habilitado el bloqueo.</span><span class="sxs-lookup"><span data-stu-id="34f8d-141">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span> | <span data-ttu-id="34f8d-142">5</span><span class="sxs-lookup"><span data-stu-id="34f8d-142">5</span></span> |

### <a name="password"></a><span data-ttu-id="34f8d-143">Contraseña</span><span class="sxs-lookup"><span data-stu-id="34f8d-143">Password</span></span>

<span data-ttu-id="34f8d-144">De forma predeterminada, identidad requiere que las contraseñas contengan un carácter en mayúscula, letra minúscula, un dígito y un carácter no alfanumérico.</span><span class="sxs-lookup"><span data-stu-id="34f8d-144">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="34f8d-145">Las contraseñas deben tener al menos seis caracteres.</span><span class="sxs-lookup"><span data-stu-id="34f8d-145">Passwords must be at least six characters long.</span></span> <span data-ttu-id="34f8d-146">[Opciones de contraseña](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) se pueden establecer en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="34f8d-146">[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) can be set in `Startup.ConfigureServices`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_pw)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

::: moniker-end

<span data-ttu-id="34f8d-147">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) especifica la [opciones de contraseña](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) con las propiedades mostradas en la tabla.</span><span class="sxs-lookup"><span data-stu-id="34f8d-147">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) specifies the [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) with the properties shown in the table.</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="34f8d-148">Property</span><span class="sxs-lookup"><span data-stu-id="34f8d-148">Property</span></span> | <span data-ttu-id="34f8d-149">Descripción</span><span class="sxs-lookup"><span data-stu-id="34f8d-149">Description</span></span> | <span data-ttu-id="34f8d-150">Default</span><span class="sxs-lookup"><span data-stu-id="34f8d-150">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="34f8d-151">RequireDigit</span><span class="sxs-lookup"><span data-stu-id="34f8d-151">RequireDigit</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | <span data-ttu-id="34f8d-152">Requiere un número entre 0-9 en la contraseña.</span><span class="sxs-lookup"><span data-stu-id="34f8d-152">Requires a number between 0-9 in the password.</span></span> | `true` |
| [<span data-ttu-id="34f8d-153">RequiredLength</span><span class="sxs-lookup"><span data-stu-id="34f8d-153">RequiredLength</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | <span data-ttu-id="34f8d-154">La longitud mínima de la contraseña.</span><span class="sxs-lookup"><span data-stu-id="34f8d-154">The minimum length of the password.</span></span> | <span data-ttu-id="34f8d-155">6</span><span class="sxs-lookup"><span data-stu-id="34f8d-155">6</span></span> |
| [<span data-ttu-id="34f8d-156">RequireLowercase</span><span class="sxs-lookup"><span data-stu-id="34f8d-156">RequireLowercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | <span data-ttu-id="34f8d-157">Requiere un carácter en minúscula en la contraseña.</span><span class="sxs-lookup"><span data-stu-id="34f8d-157">Requires a lowercase character in the password.</span></span> | `true` |
| [<span data-ttu-id="34f8d-158">RequireNonAlphanumeric</span><span class="sxs-lookup"><span data-stu-id="34f8d-158">RequireNonAlphanumeric</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | <span data-ttu-id="34f8d-159">Requiere un carácter que no son alfanuméricos en la contraseña.</span><span class="sxs-lookup"><span data-stu-id="34f8d-159">Requires a non-alphanumeric character in the password.</span></span> | `true` |
| [<span data-ttu-id="34f8d-160">RequiredUniqueChars</span><span class="sxs-lookup"><span data-stu-id="34f8d-160">RequiredUniqueChars</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | <span data-ttu-id="34f8d-161">Solo se aplica a ASP.NET Core 2.0 o posterior.</span><span class="sxs-lookup"><span data-stu-id="34f8d-161">Only applies to ASP.NET Core 2.0 or later.</span></span><br><br> <span data-ttu-id="34f8d-162">Requiere el número de caracteres distintos de la contraseña.</span><span class="sxs-lookup"><span data-stu-id="34f8d-162">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="34f8d-163">1</span><span class="sxs-lookup"><span data-stu-id="34f8d-163">1</span></span> |
| [<span data-ttu-id="34f8d-164">RequireUppercase</span><span class="sxs-lookup"><span data-stu-id="34f8d-164">RequireUppercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | <span data-ttu-id="34f8d-165">Requiere un carácter en mayúsculas en la contraseña.</span><span class="sxs-lookup"><span data-stu-id="34f8d-165">Requires an uppercase character in the password.</span></span> | `true` |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="34f8d-166">Property</span><span class="sxs-lookup"><span data-stu-id="34f8d-166">Property</span></span> | <span data-ttu-id="34f8d-167">Descripción</span><span class="sxs-lookup"><span data-stu-id="34f8d-167">Description</span></span> | <span data-ttu-id="34f8d-168">Default</span><span class="sxs-lookup"><span data-stu-id="34f8d-168">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="34f8d-169">RequireDigit</span><span class="sxs-lookup"><span data-stu-id="34f8d-169">RequireDigit</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | <span data-ttu-id="34f8d-170">Requiere un número entre 0-9 en la contraseña.</span><span class="sxs-lookup"><span data-stu-id="34f8d-170">Requires a number between 0-9 in the password.</span></span> | `true` |
| [<span data-ttu-id="34f8d-171">RequiredLength</span><span class="sxs-lookup"><span data-stu-id="34f8d-171">RequiredLength</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | <span data-ttu-id="34f8d-172">La longitud mínima de la contraseña.</span><span class="sxs-lookup"><span data-stu-id="34f8d-172">The minimum length of the password.</span></span> | <span data-ttu-id="34f8d-173">6</span><span class="sxs-lookup"><span data-stu-id="34f8d-173">6</span></span> |
| [<span data-ttu-id="34f8d-174">RequireLowercase</span><span class="sxs-lookup"><span data-stu-id="34f8d-174">RequireLowercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | <span data-ttu-id="34f8d-175">Requiere un carácter en minúscula en la contraseña.</span><span class="sxs-lookup"><span data-stu-id="34f8d-175">Requires a lowercase character in the password.</span></span> | `true` |
| [<span data-ttu-id="34f8d-176">RequireNonAlphanumeric</span><span class="sxs-lookup"><span data-stu-id="34f8d-176">RequireNonAlphanumeric</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | <span data-ttu-id="34f8d-177">Requiere un carácter que no son alfanuméricos en la contraseña.</span><span class="sxs-lookup"><span data-stu-id="34f8d-177">Requires a non-alphanumeric character in the password.</span></span> | `true` |
| [<span data-ttu-id="34f8d-178">RequireUppercase</span><span class="sxs-lookup"><span data-stu-id="34f8d-178">RequireUppercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | <span data-ttu-id="34f8d-179">Requiere un carácter en mayúsculas en la contraseña.</span><span class="sxs-lookup"><span data-stu-id="34f8d-179">Requires an uppercase character in the password.</span></span> | `true` |

::: moniker-end

### <a name="sign-in"></a><span data-ttu-id="34f8d-180">Inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="34f8d-180">Sign-in</span></span>

<span data-ttu-id="34f8d-181">El código siguiente establece `SignIn` configuración (para los valores predeterminados):</span><span class="sxs-lookup"><span data-stu-id="34f8d-181">The following code sets `SignIn` settings (to default values):</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_si)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)] 

::: moniker-end

<span data-ttu-id="34f8d-182">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) especifica la [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) con las propiedades mostradas en la tabla.</span><span class="sxs-lookup"><span data-stu-id="34f8d-182">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) specifies the [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="34f8d-183">Property</span><span class="sxs-lookup"><span data-stu-id="34f8d-183">Property</span></span> | <span data-ttu-id="34f8d-184">Descripción</span><span class="sxs-lookup"><span data-stu-id="34f8d-184">Description</span></span> | <span data-ttu-id="34f8d-185">Default</span><span class="sxs-lookup"><span data-stu-id="34f8d-185">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="34f8d-186">RequireConfirmedEmail</span><span class="sxs-lookup"><span data-stu-id="34f8d-186">RequireConfirmedEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | <span data-ttu-id="34f8d-187">Requiere un correo electrónico confirmado al iniciar sesión.</span><span class="sxs-lookup"><span data-stu-id="34f8d-187">Requires a confirmed email to sign in.</span></span> | `false` |
| [<span data-ttu-id="34f8d-188">RequireConfirmedPhoneNumber</span><span class="sxs-lookup"><span data-stu-id="34f8d-188">RequireConfirmedPhoneNumber</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | <span data-ttu-id="34f8d-189">Requiere un número de teléfono confirmada iniciar sesión.</span><span class="sxs-lookup"><span data-stu-id="34f8d-189">Requires a confirmed phone number to sign in.</span></span> | `false` |

### <a name="tokens"></a><span data-ttu-id="34f8d-190">tokens</span><span class="sxs-lookup"><span data-stu-id="34f8d-190">Tokens</span></span>

<span data-ttu-id="34f8d-191">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) especifica la [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) con las propiedades mostradas en la tabla.</span><span class="sxs-lookup"><span data-stu-id="34f8d-191">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) specifies the [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) with the properties shown in the table.</span></span>

|                                                        <span data-ttu-id="34f8d-192">Property</span><span class="sxs-lookup"><span data-stu-id="34f8d-192">Property</span></span>                                                         |                                                                                      <span data-ttu-id="34f8d-193">Descripción</span><span class="sxs-lookup"><span data-stu-id="34f8d-193">Description</span></span>                                                                                      |
|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     [<span data-ttu-id="34f8d-194">AuthenticatorTokenProvider</span><span class="sxs-lookup"><span data-stu-id="34f8d-194">AuthenticatorTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider)     |                                       <span data-ttu-id="34f8d-195">Obtiene o establece el `AuthenticatorTokenProvider` utilizado para validar los inicios de sesión de dos fases con un autenticador.</span><span class="sxs-lookup"><span data-stu-id="34f8d-195">Gets or sets the `AuthenticatorTokenProvider` used to validate two-factor sign-ins with an authenticator.</span></span>                                       |
|       [<span data-ttu-id="34f8d-196">ChangeEmailTokenProvider</span><span class="sxs-lookup"><span data-stu-id="34f8d-196">ChangeEmailTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider)       |                                     <span data-ttu-id="34f8d-197">Obtiene o establece el `ChangeEmailTokenProvider` usado para generar tokens que se usan en el correo electrónico de confirmación del cambio de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="34f8d-197">Gets or sets the `ChangeEmailTokenProvider` used to generate tokens used in email change confirmation emails.</span></span>                                     |
| [<span data-ttu-id="34f8d-198">ChangePhoneNumberTokenProvider</span><span class="sxs-lookup"><span data-stu-id="34f8d-198">ChangePhoneNumberTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) |                                      <span data-ttu-id="34f8d-199">Obtiene o establece el `ChangePhoneNumberTokenProvider` usado para generar tokens que se usan al cambiar los números de teléfono.</span><span class="sxs-lookup"><span data-stu-id="34f8d-199">Gets or sets the `ChangePhoneNumberTokenProvider` used to generate tokens used when changing phone numbers.</span></span>                                      |
| [<span data-ttu-id="34f8d-200">EmailConfirmationTokenProvider</span><span class="sxs-lookup"><span data-stu-id="34f8d-200">EmailConfirmationTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) |                                             <span data-ttu-id="34f8d-201">Obtiene o establece el proveedor de tokens que se usa para generar tokens que se usan en los correos electrónicos de confirmación de cuenta.</span><span class="sxs-lookup"><span data-stu-id="34f8d-201">Gets or sets the token provider used to generate tokens used in account confirmation emails.</span></span>                                              |
|     [<span data-ttu-id="34f8d-202">PasswordResetTokenProvider</span><span class="sxs-lookup"><span data-stu-id="34f8d-202">PasswordResetTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider)     | <span data-ttu-id="34f8d-203">Obtiene o establece el [IUserTwoFactorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) usado para generar tokens que se usan en los correos electrónicos de restablecimiento de contraseña.</span><span class="sxs-lookup"><span data-stu-id="34f8d-203">Gets or sets the [IUserTwoFactorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) used to generate tokens used in password reset emails.</span></span> |
|                    [<span data-ttu-id="34f8d-204">ProviderMap</span><span class="sxs-lookup"><span data-stu-id="34f8d-204">ProviderMap</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap)                    |                <span data-ttu-id="34f8d-205">Utilizado para construir un [proveedor de tokens de usuario](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) con la clave que se usa como el nombre del proveedor.</span><span class="sxs-lookup"><span data-stu-id="34f8d-205">Used to construct a [User Token Provider](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) with the key used as the provider's name.</span></span>                 |

### <a name="user"></a><span data-ttu-id="34f8d-206">Usuario</span><span class="sxs-lookup"><span data-stu-id="34f8d-206">User</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_user)]

<span data-ttu-id="34f8d-207">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) especifica la [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) con las propiedades mostradas en la tabla.</span><span class="sxs-lookup"><span data-stu-id="34f8d-207">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) specifies the [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="34f8d-208">Property</span><span class="sxs-lookup"><span data-stu-id="34f8d-208">Property</span></span> | <span data-ttu-id="34f8d-209">Descripción</span><span class="sxs-lookup"><span data-stu-id="34f8d-209">Description</span></span> | <span data-ttu-id="34f8d-210">Default</span><span class="sxs-lookup"><span data-stu-id="34f8d-210">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="34f8d-211">AllowedUserNameCharacters</span><span class="sxs-lookup"><span data-stu-id="34f8d-211">AllowedUserNameCharacters</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | <span data-ttu-id="34f8d-212">Caracteres permitidos en el nombre de usuario.</span><span class="sxs-lookup"><span data-stu-id="34f8d-212">Allowed characters in the username.</span></span> | <span data-ttu-id="34f8d-213">abcdefghijklmnopqrstuvwxyz</span><span class="sxs-lookup"><span data-stu-id="34f8d-213">abcdefghijklmnopqrstuvwxyz</span></span><br><span data-ttu-id="34f8d-214">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span><span class="sxs-lookup"><span data-stu-id="34f8d-214">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span></span><br><span data-ttu-id="34f8d-215">0123456789</span><span class="sxs-lookup"><span data-stu-id="34f8d-215">0123456789</span></span><br><span data-ttu-id="34f8d-216">-.\_@+</span><span class="sxs-lookup"><span data-stu-id="34f8d-216">-.\_@+</span></span> |
| [<span data-ttu-id="34f8d-217">RequireUniqueEmail</span><span class="sxs-lookup"><span data-stu-id="34f8d-217">RequireUniqueEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | <span data-ttu-id="34f8d-218">Requiere que cada usuario tiene un correo electrónico única.</span><span class="sxs-lookup"><span data-stu-id="34f8d-218">Requires each user to have a unique email.</span></span> | `false` |

### <a name="cookie-settings"></a><span data-ttu-id="34f8d-219">Configuración de cookies</span><span class="sxs-lookup"><span data-stu-id="34f8d-219">Cookie settings</span></span>

<span data-ttu-id="34f8d-220">Configurar la cookie de la aplicación en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="34f8d-220">Configure the app's cookie in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="34f8d-221">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) debe llamarse **después** llamada `AddIdentity` o `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="34f8d-221">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) must be called **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_cookie)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

::: moniker-end

<span data-ttu-id="34f8d-222">Para obtener más información, consulte [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).</span><span class="sxs-lookup"><span data-stu-id="34f8d-222">For more information, see [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).</span></span>

## <a name="password-hasher-options"></a><span data-ttu-id="34f8d-223">Opciones de contraseña Hasher</span><span class="sxs-lookup"><span data-stu-id="34f8d-223">Password Hasher options</span></span>

<span data-ttu-id="34f8d-224"><xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions> Obtiene y establece las opciones para crear valores hash de contraseña.</span><span class="sxs-lookup"><span data-stu-id="34f8d-224"><xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions> gets and sets options for password hashing.</span></span>

| <span data-ttu-id="34f8d-225">Opción</span><span class="sxs-lookup"><span data-stu-id="34f8d-225">Option</span></span> | <span data-ttu-id="34f8d-226">Descripción</span><span class="sxs-lookup"><span data-stu-id="34f8d-226">Description</span></span> |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> | <span data-ttu-id="34f8d-227">El modo de compatibilidad que se usa al hash de contraseñas nuevas.</span><span class="sxs-lookup"><span data-stu-id="34f8d-227">The compatibility mode used when hashing new passwords.</span></span> <span data-ttu-id="34f8d-228">Tiene como valor predeterminado <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>.</span><span class="sxs-lookup"><span data-stu-id="34f8d-228">Defaults to <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>.</span></span> <span data-ttu-id="34f8d-229">El primer byte de una contraseña con algoritmo hash, denominado un *marcador de formato*, especifica la versión del algoritmo hash utilizado para la contraseña.</span><span class="sxs-lookup"><span data-stu-id="34f8d-229">The first byte of a hashed password, called a *format marker*, specifies the version of the hashing algorithm used to hash the password.</span></span> <span data-ttu-id="34f8d-230">Al comprobar una contraseña en un algoritmo hash, el <xref:Microsoft.AspNetCore.Identity.PasswordHasher`1.VerifyHashedPassword*> método selecciona el algoritmo correcto basándose en el primer byte.</span><span class="sxs-lookup"><span data-stu-id="34f8d-230">When verifying a password against a hash, the <xref:Microsoft.AspNetCore.Identity.PasswordHasher`1.VerifyHashedPassword*> method selects the correct algorithm based on the first byte.</span></span> <span data-ttu-id="34f8d-231">Un cliente es puede autenticarse con independencia de los cuales se usó la versión del algoritmo para la contraseña.</span><span class="sxs-lookup"><span data-stu-id="34f8d-231">A client is able to authenticate regardless of which version of the algorithm was used to hash the password.</span></span> <span data-ttu-id="34f8d-232">Establecer el modo de compatibilidad afecta el hash de *nuevas contraseñas*.</span><span class="sxs-lookup"><span data-stu-id="34f8d-232">Setting the compatibility mode affects the hashing of *new passwords*.</span></span> |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> | <span data-ttu-id="34f8d-233">El número de iteraciones usadas al hash de contraseñas mediante PBKDF2.</span><span class="sxs-lookup"><span data-stu-id="34f8d-233">The number of iterations used when hashing passwords using PBKDF2.</span></span> <span data-ttu-id="34f8d-234">Este valor es utiliza únicamente cuando el <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> está establecido en <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>.</span><span class="sxs-lookup"><span data-stu-id="34f8d-234">This value is only used when the <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> is set to <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>.</span></span> <span data-ttu-id="34f8d-235">El valor debe ser un entero positivo y el valor predeterminado es `10000`.</span><span class="sxs-lookup"><span data-stu-id="34f8d-235">The value must be a positive integer and defaults to `10000`.</span></span> |

<span data-ttu-id="34f8d-236">En el ejemplo siguiente, la <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> está establecido en `12000` en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="34f8d-236">In the following example, the <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> is set to `12000` in `Startup.ConfigureServices`:</span></span>

```csharp
// using Microsoft.AspNetCore.Identity;

services.Configure<PasswordHasherOptions>(option =>
{
    option.IterationCount = 12000;
});
```
