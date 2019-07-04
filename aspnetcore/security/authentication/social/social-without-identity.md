---
title: Facebook, Google y la autenticación de proveedor externo sin ASP.NET Core Identity
author: rick-anderson
description: Explicación del uso de Facebook, Google, Twitter, autenticación de usuario de cuenta etc. sin ASP.NET Core Identity.
ms.author: riande
ms.date: 07/04/2019
uid: security/authentication/social/social-without-identity
ms.openlocfilehash: 1e7124e8b07c0faf2d005ec3ef55c0414a697d64
ms.sourcegitcommit: f6e6730872a7d6f039f97d1df762f0d0bd5e34cf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561568"
---
# <a name="use-social-sign-in-provider-authentication-without-aspnet-core-identity"></a>Usar la autenticación del proveedor de inicio de sesión social sin ASP.NET Core Identity

<xref:security/authentication/social/index> Describe cómo habilitar usuarios iniciar sesión mediante OAuth 2.0 con las credenciales de proveedores de autenticación externos. El enfoque descrito en este tema incluye ASP.NET Core Identity como proveedor de autenticación.

Este ejemplo muestra cómo usar el proveedor de autenticación externo **sin** ASP.NET Core Identity. Esto es útil para las aplicaciones que no requieren todas las características de ASP.NET Core Identity, pero seguirá necesitan la integración con un proveedor de autenticación externo de confianza.

Este ejemplo utiliza [autenticación de Google](xref:security/authentication/google-logins) para autenticar a los usuarios. Autenticación con Google desplaza muchas de las complejidades de administrar el proceso de inicio de sesión de Google. Para integrar con un proveedor de autenticación externo diferente, vea los temas siguientes:

* [Autenticación con Facebook](xref:security/authentication/facebook-logins)
* [Autenticación con Microsoft](xref:security/authentication/microsoft-logins)
* [Autenticación con Twitter](xref:security/authentication/twitter-logins)
* [Otros proveedores](xref:security/authentication/otherlogins)

## <a name="configuration"></a>Configuración

En el `ConfigureServices` método, configure los esquemas de autenticación de la aplicación con el `AddAuthentication`, `AddCookie` y `AddGoogle` métodos:

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet1)]

La llamada a [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) establece la aplicación [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme). El `DefaultScheme` es el esquema predeterminado para las siguientes `HttpContext` métodos de extensión de autenticación:

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

Configuración de la aplicación `DefaultScheme` a [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies") configura la aplicación para usar las Cookies que el esquema predeterminado para estos métodos de extensión. Configuración de la aplicación <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> a [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") configura la aplicación para usar Google como el esquema predeterminado para las llamadas a `ChallengeAsync`. `DefaultChallengeScheme` invalida `DefaultScheme`. Consulte <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> de propiedades adicionales que reemplazan `DefaultScheme` cuando se establece.

En el `Configure` método, llame a la `UseAuthentication` método para invocar el Middleware de autenticación que establece el `HttpContext.User` propiedad. Llame a la `UseAuthentication` método antes de llamar a `UseMvcWithDefaultRoute` o `UseMvc`:

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet2)]

Para obtener más información acerca de los esquemas de autenticación y autenticación con cookies, consulte <xref:security/authentication/cookie>.

## <a name="applying-authorization"></a>Aplicar la autorización

Probar la configuración de autenticación de la aplicación aplicando la `AuthorizeAttribute` atributo a un controlador, acción o página. El siguiente código limita el acceso a la *privacidad* página a los usuarios que han sido autenticados:

[!code-csharp[](social-without-identity/sample/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a>Cerrar sesión

Para cerrar la sesión del usuario actual y eliminar sus cookies, llame a [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0). El código siguiente agrega un `Logout` controlador de páginas la *índice* página:

[!code-csharp[](social-without-identity/sample/Pages/Index.cshtml.cs?name=snippet&highlight=7-11)]

Tenga en cuenta que la llamada a `SignOutAsync` no especifica un esquema de autenticación. La aplicación `DefaultScheme` de `CookieAuthenticationDefaults.AuthenticationScheme` se utiliza como un retroceso.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>
