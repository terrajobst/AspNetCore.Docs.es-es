---
title: La autenticación de Facebook, Google y proveedores externos sin ASP.NET Core identidad
author: rick-anderson
description: Explicación del uso de Facebook, Google, Twitter, etc. autenticación de usuarios de cuentas sin ASP.NET Core identidad.
ms.author: riande
ms.date: 09/25/2019
uid: security/authentication/social/social-without-identity
ms.openlocfilehash: 54dd93a13b2f7ed09c2c305f529d5f4610567184
ms.sourcegitcommit: 6d26ab647ede4f8e57465e29b03be5cb130fc872
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2019
ms.locfileid: "71999899"
---
# <a name="use-social-sign-in-provider-authentication-without-aspnet-core-identity"></a>Usar la autenticación de proveedor de inicio de sesión social sin ASP.NET Core identidad

::: moniker range=">= aspnetcore-3.0"

<xref:security/authentication/social/index> describe cómo permitir a los usuarios iniciar sesión con OAuth 2,0 con las credenciales de proveedores de autenticación externos. El enfoque descrito en ese tema incluye ASP.NET Core identidad como proveedor de autenticación.

Este ejemplo muestra cómo utilizar un proveedor de autenticación externo **sin** ASP.net Core identidad. Esto resulta útil para las aplicaciones que no requieren todas las características de ASP.NET Core identidad, pero que aún requieren la integración con un proveedor de autenticación externo de confianza.

Este ejemplo utiliza la [autenticación de Google](xref:security/authentication/google-logins) para autenticar a los usuarios. El uso de la autenticación de Google desplaza muchas de las complejidades de la administración del proceso de inicio de sesión en Google. Para integrar con otro proveedor de autenticación externo, vea los temas siguientes:

* [Autenticación con Facebook](xref:security/authentication/facebook-logins)
* [Autenticación con Microsoft](xref:security/authentication/microsoft-logins)
* [Autenticación con Twitter](xref:security/authentication/twitter-logins)
* [Otros proveedores](xref:security/authentication/otherlogins)

## <a name="configuration"></a>Configuración

En el método `ConfigureServices`, configure los esquemas de autenticación de la aplicación con los métodos <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>, <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> y <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*>:

[!code-csharp[](social-without-identity/3.0sample/Startup.cs?name=snippet1)]

La llamada a <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> establece la @no__t de la aplicación-1. El `DefaultScheme` es el esquema predeterminado utilizado por los siguientes métodos de extensión de autenticación `HttpContext`:

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

Al establecer el @no__t de la aplicación en [CookieAuthenticationDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("cookies"), se configura la aplicación para que use cookies como esquema predeterminado para estos métodos de extensión. Al establecer el @no__t de la aplicación en [GoogleDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google"), se configura la aplicación para que use Google como esquema predeterminado para las llamadas a `ChallengeAsync`. `DefaultChallengeScheme` reemplaza a `DefaultScheme`. Vea <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> para obtener más propiedades que invalide `DefaultScheme` cuando se establezca.

En `Startup.Configure`, llame a `UseAuthentication` y @no__t 2 para establecer la propiedad `HttpContext.User` y ejecutar el middleware de autorización para las solicitudes. Llame a los métodos `UseAuthentication` y `UseAuthorization` antes de llamar a `UseEndpoints`:

[!code-csharp[](social-without-identity/3.0sample/Startup.cs?name=snippet2)]

Para obtener más información sobre los esquemas de autenticación y la autenticación de cookies, vea <xref:security/authentication/cookie>.

## <a name="apply-authorization"></a>Aplicar autorización

Pruebe la configuración de autenticación de la aplicación aplicando el atributo `AuthorizeAttribute` a un controlador, acción o página. El código siguiente limita el acceso a la página de *privacidad* a los usuarios que se han autenticado:

[!code-csharp[](social-without-identity/3.0sample/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a>Cerrar sesión

Para cerrar la sesión del usuario actual y eliminar su cookie, llame a [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*). El siguiente código agrega un controlador de página `Logout` a la página de *Índice* :

[!code-csharp[](social-without-identity/3.0sample/Pages/Index.cshtml.cs?name=snippet&highlight=14-18)]

Observe que la llamada a `SignOutAsync` no especifica un esquema de autenticación. El @no__t de la aplicación-0 de `CookieAuthenticationDefaults.AuthenticationScheme` se usa como retroceso.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
::: moniker range="< aspnetcore-3.0"

<xref:security/authentication/social/index> describe cómo permitir a los usuarios iniciar sesión con OAuth 2,0 con las credenciales de proveedores de autenticación externos. El enfoque descrito en ese tema incluye ASP.NET Core identidad como proveedor de autenticación.

Este ejemplo muestra cómo utilizar un proveedor de autenticación externo **sin** ASP.net Core identidad. Esto resulta útil para las aplicaciones que no requieren todas las características de ASP.NET Core identidad, pero que aún requieren la integración con un proveedor de autenticación externo de confianza.

Este ejemplo utiliza la [autenticación de Google](xref:security/authentication/google-logins) para autenticar a los usuarios. El uso de la autenticación de Google desplaza muchas de las complejidades de la administración del proceso de inicio de sesión en Google. Para integrar con otro proveedor de autenticación externo, vea los temas siguientes:

* [Autenticación con Facebook](xref:security/authentication/facebook-logins)
* [Autenticación con Microsoft](xref:security/authentication/microsoft-logins)
* [Autenticación con Twitter](xref:security/authentication/twitter-logins)
* [Otros proveedores](xref:security/authentication/otherlogins)

## <a name="configuration"></a>Configuración

En el método `ConfigureServices`, configure los esquemas de autenticación de la aplicación con los métodos `AddAuthentication`, `AddCookie` y `AddGoogle`:

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet1)]

La llamada a [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) establece el [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme)de la aplicación. El `DefaultScheme` es el esquema predeterminado utilizado por los siguientes métodos de extensión de autenticación `HttpContext`:

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

Al establecer el @no__t de la aplicación en [CookieAuthenticationDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("cookies"), se configura la aplicación para que use cookies como esquema predeterminado para estos métodos de extensión. Al establecer el @no__t de la aplicación en [GoogleDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google"), se configura la aplicación para que use Google como esquema predeterminado para las llamadas a `ChallengeAsync`. `DefaultChallengeScheme` reemplaza a `DefaultScheme`. Vea <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> para obtener más propiedades que invalide `DefaultScheme` cuando se establezca.

En el método `Configure`, llame al método `UseAuthentication` para invocar el middleware de autenticación que establece la propiedad `HttpContext.User`. Llame al método `UseAuthentication` antes de llamar a `UseMvcWithDefaultRoute` o `UseMvc`:

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet2)]

Para obtener más información sobre los esquemas de autenticación y la autenticación de cookies, vea <xref:security/authentication/cookie>.

## <a name="apply-authorization"></a>Aplicar autorización

Pruebe la configuración de autenticación de la aplicación aplicando el atributo `AuthorizeAttribute` a un controlador, acción o página. El código siguiente limita el acceso a la página de *privacidad* a los usuarios que se han autenticado:

[!code-csharp[](social-without-identity/sample/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a>Cerrar sesión

Para cerrar la sesión del usuario actual y eliminar su cookie, llame a [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*). El siguiente código agrega un controlador de página `Logout` a la página de *Índice* :

[!code-csharp[](social-without-identity/sample/Pages/Index.cshtml.cs?name=snippet&highlight=7-11)]

Observe que la llamada a `SignOutAsync` no especifica un esquema de autenticación. El @no__t de la aplicación-0 de `CookieAuthenticationDefaults.AuthenticationScheme` se usa como retroceso.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
