---
title: "Mediante la autenticación de Cookie sin identidad principal de ASP.NET"
author: rick-anderson
description: "Obtener una explicación del uso de autenticación con cookies sin ASP.NET Core Identity"
ms.author: riande
manager: wpickett
ms.date: 10/11/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/cookie
ms.openlocfilehash: 781b89a222bbf26b51e56fad707c591779f2e4bd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="using-cookie-authentication-without-aspnet-core-identity"></a>Mediante la autenticación de Cookie sin identidad principal de ASP.NET

Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Luke Latham](https://github.com/guardrex)

Como ha visto en los temas de autenticación anteriores, [ASP.NET Core Identity](xref:security/authentication/identity) es un proveedor de autenticación completo y completa para crear y mantener los inicios de sesión. Sin embargo, puede que desee utilizar su propia lógica de autenticación personalizada con la autenticación basada en cookies a veces. Puede usar la autenticación basada en cookies como un proveedor de autenticación independiente sin ASP.NET Core Identity.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

Para obtener información sobre migración autenticación basada en cookies de ASP.NET Core 1.x a 2.0, consulte [migrar autenticación e identidad al tema principal de ASP.NET 2.0 (autenticación basada en cookies)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).

## <a name="configuration"></a>Configuración

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Si no está usando la [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), instale la versión 2.0 de la [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) paquete NuGet.

En el `ConfigureServices` (método), crear el servicio de Middleware de autenticación con el `AddAuthentication` y `AddCookie` métodos:

[!code-csharp[Main](cookie/sample/Startup.cs?name=snippet1)]

`AuthenticationScheme`pasa al `AddAuthentication` establece el esquema de autenticación predeterminado para la aplicación. `AuthenticationScheme`es útil cuando hay varias instancias de autenticación con cookies y desea [autorizarse con un esquema específico](xref:security/authorization/limitingidentitybyscheme). Establecer el `AuthenticationScheme` a `CookieAuthenticationDefaults.AuthenticationScheme` proporciona un valor de "Cookies" para el esquema. Puede proporcionar cualquier valor de cadena que distingue el esquema.

En el `Configure` método, use la `UseAuthentication` método que se invoca el Middleware de autenticación que establece el `HttpContext.User` propiedad. Llame a la `UseAuthentication` método antes de llamar a `UseMvcWithDefaultRoute` o `UseMvc`:

[!code-csharp[Main](cookie/sample/Startup.cs?name=snippet2)]

**Opciones de AddCookie**

El [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) clase se utiliza para configurar las opciones de proveedor de autenticación.

| Opción | Descripción |
| ------ | ----------- |
| [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | Proporciona la ruta de acceso para suministrar con un 302 encontrado (redireccionamiento de la dirección URL) cuando se desencadena por `HttpContext.ForbidAsync`. El valor predeterminado es `/Account/AccessDenied`. |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | El emisor que se usará para la [emisor](/dotnet/api/system.security.claims.claim.issuer) propiedad en las notificaciones creados por el servicio de autenticación de la cookie. |
| [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | El nombre de dominio donde se sirve la cookie. De forma predeterminada, este es el nombre de host de la solicitud. El explorador sólo envía la cookie en las solicitudes a un nombre de host coincidente. Puede que desee ajustar esta opción para que las cookies disponibles para todos los hosts en el dominio. Por ejemplo, establecer el dominio de cookies `.contoso.com` pone a disposición `contoso.com`, `www.contoso.com`, y `staging.www.contoso.com`. |
| [Cookie.Expiration](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | Obtiene o establece el tiempo de vida de una cookie. Actualmente, esta opción no ops y quedarán obsoleta en ASP.NET Core 2.1 +. Use la `ExpireTimeSpan` opción para establecer la expiración de la cookie. Para obtener más información, consulte [aclarar el comportamiento de CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293). |
| [Cookie.HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | Una marca que indica si la cookie debe ser accesible sólo a los servidores. Si cambia este valor a `false` permite que los scripts del lado cliente para tener acceso a la cookie y se puede abrir la aplicación al robo de cookies debe tener la aplicación un [scripting entre sitios (XSS)](xref:security/cross-site-scripting) una vulnerabilidad. El valor predeterminado es `true`. |
| [Cookie.Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | Establece el nombre de la cookie. |
| [Cookie.Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | Se utiliza para aislar las aplicaciones que se ejecutan en el mismo nombre de host. Si tiene aplicaciones que se ejecutan en `/app1` y desea restringir las cookies a esa aplicación, establezca el `CookiePath` propiedad `/app1`. Al hacerlo, la cookie solo está disponible en las solicitudes a `/app1` y cualquier aplicación aparecen debajo de él. |
| [Cookie.SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | Indica si el explorador debe permitir la cookie que se adjuntará a sólo las solicitudes del mismo sitio (`SameSiteMode.Strict`) o solicitudes entre sitios mediante métodos de prueba de errores HTTP y solicitudes del mismo sitio (`SameSiteMode.Lax`). Cuando se establece en `SameSiteMode.None`, no se establece el valor del encabezado de cookie. Tenga en cuenta que [Middleware de cookies directiva](#cookie-policy-middleware) podría sobrescribir el valor que se proporcione. Para admitir la autenticación de OAuth, el valor predeterminado es `SameSiteMode.Lax`. Para obtener más información, consulte [roto debido a la directiva de SameSite cookie de autenticación de OAuth](https://github.com/aspnet/Security/issues/1231). |
| [Cookie.SecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | Una marca que indica si la cookie creada debe limitarse a HTTPS (`CookieSecurePolicy.Always`), HTTP o HTTPS (`CookieSecurePolicy.None`), o el mismo protocolo que la solicitud (`CookieSecurePolicy.SameAsRequest`). El valor predeterminado es `CookieSecurePolicy.SameAsRequest`. |
| [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | Establece el `DataProtectionProvider` que se utiliza para crear el valor predeterminado `TicketDataFormat`. Si el `TicketDataFormat` propiedad está establecida, el `DataProtectionProvider` opción no se utiliza. Si no se proporciona, se utiliza el proveedor de protección de datos de la aplicación predeterminada. |
| [Eventos](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | El controlador llama a métodos en el proveedor que proporcionan el control de la aplicación en determinados puntos de procesamiento. Si `Events` no siempre, se proporciona una instancia predeterminada que no hace nada cuando se llaman a los métodos. |
| [EventsType](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | Utiliza como el tipo de servicio para obtener el `Events` instancia en lugar de la propiedad. |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | El `TimeSpan` tras el cual expira el vale de autenticación que se almacena dentro de la cookie. `ExpireTimeSpan`se agrega a la hora actual para crear la fecha de expiración para el vale. El `ExpiredTimeSpan` siempre que el valor sea en el cifrado AuthTicket verificada por el servidor. También puede ir a la [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) encabezado, pero solo si `IsPersistent` se establece. Para establecer `IsPersistent` a `true`, configurar la [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) pasado a `SignInAsync`. El valor predeterminado de `ExpireTimeSpan` es 14 días. |
| [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | Proporciona la ruta de acceso para suministrar con un 302 encontrado (redireccionamiento de la dirección URL) cuando se desencadena por `HttpContext.ChallengeAsync`. La dirección URL actual que generó el 401 se agrega a la `LoginPath` como un parámetro de cadena de consulta denominado por la `ReturnUrlParameter`. Una vez una solicitud para la `LoginPath` concede una nueva identidad de inicio de sesión, la `ReturnUrlParameter` valor se utiliza para redirigir el explorador a la dirección URL que produjo el código de estado sin autorización original. El valor predeterminado es `/Account/Login`. |
| [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | Si el `LogoutPath` se proporciona al controlador, a continuación, redirige una solicitud a dicha ruta de acceso en función del valor de la `ReturnUrlParameter`. El valor predeterminado es `/Account/Logout`. |
| [ReturnUrlParameter](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | Determina el nombre del parámetro de cadena de consulta que se anexa el controlador para una respuesta 302 de Found (redireccionamiento de la dirección URL). `ReturnUrlParameter`se utiliza cuando llega una solicitud en el `LoginPath` o `LogoutPath` para devolver el explorador a la dirección URL original después de realiza la acción de inicio de sesión o cierre de sesión. El valor predeterminado es `ReturnUrl`. |
| [SessionStore](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | Contenedor opcional que se utiliza para almacenar la identidad todas las solicitudes. Cuando se utiliza, solo un identificador de sesión se envía al cliente. `SessionStore`puede utilizarse para mitigar los posibles problemas con identidades grandes. |
| [SlidingExpiration](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | Una marca que indica si se debe emitir una nueva cookie con una fecha de caducidad actualizada dinámicamente. Esto puede suceder en cualquier solicitud que el período de expiración de cookie actual es más del 50% expira. La nueva fecha de expiración se mueve hacia delante como la fecha actual más el `ExpireTimespan`. Un [tiempo de expiración de cookie absoluta](xref:security/authentication/cookie#absolute-cookie-expiration) puede establecerse mediante el `AuthenticationProperties` al llamar a la clase `SignInAsync`. Una hora de expiración absoluta puede mejorar la seguridad de la aplicación mediante la limitación de la cantidad de tiempo que la cookie de autenticación es válida. El valor predeterminado es `true`. |
| [TicketDataFormat](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | El `TicketDataFormat` se usa para proteger y desproteger la identidad y otras propiedades que se almacenan en el valor de cookie. Si no se proporciona un `TicketDataFormat` se crea utilizando el [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0). |
| [Validate](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | Método que compruebe que las opciones son válidas. |

Establecer `CookieAuthenticationOptions` en la configuración del servicio para la autenticación en el `ConfigureServices` método:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

ASP.NET Core 1.x usa cookies [middleware](xref:fundamentals/middleware) que serializa una entidad de seguridad de usuario en una cookie cifrada. En solicitudes posteriores, la cookie se valida y se vuelve a crear la entidad de seguridad y se asigna a la `HttpContext.User` propiedad.

Instalar el [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) paquete de NuGet en el proyecto. Este paquete contiene el middleware de cookies.

Use la `UseCookieAuthentication` método en el `Configure` método en su *Startup.cs* archivo antes de `UseMvc` o `UseMvcWithDefaultRoute`:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions()
{
    AccessDeniedPath = "/Account/Forbidden/",
    AuthenticationScheme = CookieAuthenticationDefaults.AuthenticationScheme,
    AutomaticAuthenticate = true,
    AutomaticChallenge = true,
    LoginPath = "/Account/Unauthorized/"
});
```

**Opciones de CookieAuthenticationOptions**

El [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) clase se utiliza para configurar las opciones de proveedor de autenticación.

| Opción | Descripción |
| ------ | ----------- |
| [AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | Establece el esquema de autenticación. `AuthenticationScheme`es útil cuando hay varias instancias de autenticación y desea [autorizarse con un esquema específico](xref:security/authorization/limitingidentitybyscheme). Establecer el `AuthenticationScheme` a `CookieAuthenticationDefaults.AuthenticationScheme` proporciona un valor de "Cookies" para el esquema. Puede proporcionar cualquier valor de cadena que distingue el esquema. |
| [AutomaticAuthenticate](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | Establece un valor para indicar que la autenticación con cookies debe ejecutarse en cada solicitud y que intentan validar y reconstruir cualquier entidad de seguridad serializado que creó. |
| [AutomaticChallenge](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | Si es true, el middleware de autenticación controla desafíos automática. Si false, el middleware de autenticación sólo altera las respuestas cuando indique explícitamente la `AuthenticationScheme`. |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | El emisor que se usará para la [emisor](/dotnet/api/system.security.claims.claim.issuer) propiedad en cualquier notificación creado por el middleware de autenticación de la cookie. |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | El nombre de dominio donde se sirve la cookie. De forma predeterminada, este es el nombre de host de la solicitud. El explorador sólo sirve la cookie a un nombre de host coincidente. Puede que desee ajustar esta opción para que las cookies disponibles para todos los hosts en el dominio. Por ejemplo, establecer el dominio de cookies `.contoso.com` pone a disposición `contoso.com`, `www.contoso.com`, y `staging.www.contoso.com`. |
| [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | Una marca que indica si la cookie debe ser accesible sólo a los servidores. Si cambia este valor a `false` permite que los scripts del lado cliente para tener acceso a la cookie y se puede abrir la aplicación al robo de cookies debe tener la aplicación un [scripting entre sitios (XSS)](xref:security/cross-site-scripting) una vulnerabilidad. El valor predeterminado es `true`. |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | Se utiliza para aislar las aplicaciones que se ejecutan en el mismo nombre de host. Si tiene aplicaciones que se ejecutan en `/app1` y desea restringir las cookies a esa aplicación, establezca el `CookiePath` propiedad `/app1`. Al hacerlo, la cookie solo está disponible en las solicitudes a `/app1` y cualquier aplicación aparecen debajo de él. |
| [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | Una marca que indica si la cookie creada debe limitarse a HTTPS (`CookieSecurePolicy.Always`), HTTP o HTTPS (`CookieSecurePolicy.None`), o el mismo protocolo que la solicitud (`CookieSecurePolicy.SameAsRequest`). El valor predeterminado es `CookieSecurePolicy.SameAsRequest`. |
| [Descripción](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | Información adicional sobre el tipo de autenticación que debe ponerse a disposición de la aplicación. |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | El `TimeSpan` tras el cual expira el vale de autenticación. Se agrega a la hora actual para crear la fecha de expiración para el vale. Usar `ExpireTimeSpan`, debe establecer `IsPersistent` a `true` en el `AuthenticationProperties` pasado a `SignInAsync`. El valor predeterminado es 14 días. |
| [SlidingExpiration](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | Una marca que indica si la fecha de expiración de la cookie restablece cuando haya más de la mitad de la `ExpireTimeSpan` ha transcurrido el intervalo. La nueva hora de exipiration se mueve hacia delante como la fecha actual más el `ExpireTimespan`. Un [tiempo de expiración de cookie absoluta](xref:security/authentication/cookie#absolute-cookie-expiration) puede establecerse mediante el `AuthenticationProperties` al llamar a la clase `SignInAsync`. Una hora de expiración absoluta puede mejorar la seguridad de la aplicación mediante la limitación de la cantidad de tiempo que la cookie de autenticación es válida. El valor predeterminado es `true`. |

Establecer `CookieAuthenticationOptions` para el Middleware de autenticación de Cookie en el `Configure` método:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

---

## <a name="cookie-policy-middleware"></a>Middleware de cookies directiva.

[Middleware de cookies directiva](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) habilita las capacidades de directiva de cookie de una aplicación. Agregar el middleware a la canalización de procesamiento de la aplicación es el orden de minúsculas; solo afecta a los componentes registrados después de él en la canalización.

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 El [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) proporcionado para el Middleware de directiva de Cookie le permiten controlar características globales del procesamiento de la cookie y el enlace en los controladores de procesamiento de cookie cuando se agrega o se eliminan las cookies.

| Property | Descripción |
| -------- | ----------- |
| [HttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | Afecta a si las cookies deben estar HttpOnly, que es una marca que indica si la cookie debe ser accesible sólo a los servidores. El valor predeterminado es `HttpOnlyPolicy.None`. |
| [MinimumSameSitePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | Afecta al atributo del mismo sitio de la cookie (ver abajo). El valor predeterminado es `SameSiteMode.Lax`. Esta opción está disponible para el núcleo de ASP.NET 2.0 +. |
| [OnAppendCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | Se llama cuando se anexa una cookie. |
| [OnDeleteCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | Se llama cuando se elimina una cookie. |
| [Proteger](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | Determina si las cookies deben estar seguro. El valor predeterminado es `CookieSecurePolicy.None`. |

**MinimumSameSitePolicy** (ASP.NET 2.0 + sólo principal)

El valor predeterminado `MinimumSameSitePolicy` valor es `SameSiteMode.Lax` para permitir la autenticación de OAuth2. Estrictamente aplicar una directiva del mismo sitio de `SameSiteMode.Strict`, establezca el `MinimumSameSitePolicy`. Aunque esta configuración interrumpe OAuth2 y otros esquemas de autenticación entre orígenes, eleva el nivel de seguridad de la cookie para otros tipos de aplicaciones que no confían en el procesamiento de solicitudes entre orígenes.

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

La configuración de directiva Middleware de cookies para `MinimumSameSitePolicy` pueden afectar a la configuración de `Cookie.SameSite` en `CookieAuthenticationOptions` valores según la tabla siguiente.

| MinimumSameSitePolicy | Cookie.SameSite | Configuración de Cookie.SameSite resultante |
| --------------------- | --------------- | --------------------------------- |
| SameSiteMode.None     | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Lax      | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Lax<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Strict   | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Strict<br>SameSiteMode.Strict<br>SameSiteMode.Strict |

## <a name="creating-an-authentication-cookie"></a>Crear una cookie de autenticación

Para crear una cookie que contiene información de usuario, debe construir un [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal). La información de usuario se serializa y se almacena en la cookie. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Llame a [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) para iniciar sesión en el usuario:

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Llame a [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) para iniciar sesión en el usuario:

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

---

`SignInAsync`crea una cookie cifrada y lo agrega a la respuesta actual. Si no se especifica un `AuthenticationScheme`, se utiliza el esquema predeterminado.

Tras los bastidores, el cifrado usado es ASP.NET Core [protección de datos](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) sistema. Si aloja la aplicación en varios equipos, equilibrio de carga entre las aplicaciones o usar una granja de servidores web, debe [configurar la protección de datos](xref:security/data-protection/configuration/overview) para usar el mismo anillo de clave y el identificador de la aplicación.

## <a name="signing-out"></a>Cierre de sesión

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Para cerrar la sesión del usuario actual y eliminar las cookies, llame a [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):

```csharp
await HttpContext.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Para cerrar la sesión del usuario actual y eliminar las cookies, llame a [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

---

Si no está usando `CookieAuthenticationDefaults.AuthenticationScheme` (o "Cookies") como el esquema (por ejemplo, "ContosoCookie"), proporcione el esquema que usó al configurar el proveedor de autenticación. En caso contrario, se utiliza el esquema predeterminado.

## <a name="reacting-to-back-end-changes"></a>Reaccionar ante los cambios de back-end

Una vez que se crea una cookie, se convierte en el único origen de identidad. Incluso si se deshabilita a un usuario en los sistemas back-end, el sistema de autenticación de cookie no tiene ningún conocimiento de este, y un usuario permanece ha iniciado sesión como su cookie es válida.

El [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) eventos en ASP.NET Core 2.x o [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) método en ASP.NET Core 1.x puede usarse para interceptar y reemplazar la validación de la identidad de la cookie. Este enfoque reduce el riesgo de revocados a los usuarios obtener acceso a la aplicación.

Un enfoque de validación de la cookie se basa en realizar el seguimiento de cuándo se ha modificado la base de datos de usuario. Si la base de datos no se ha modificado desde que se emitió la cookie del usuario, no hay ninguna necesidad de volver a autenticar al usuario si su cookie sigue siendo válido. Para implementar este escenario, la base de datos, que se implementa en `IUserRepository` en este ejemplo, almacena una `LastChanged` valor. Cuando cualquier usuario se actualiza en la base de datos, la `LastChanged` valor se establece en la hora actual.

Con el fin de invalidar una cookie cuando los cambios de la base de datos se basan en el `LastChanged` valor, cree la cookie con un `LastChanged` notificación que contiene el actual `LastChanged` valor de la base de datos:

```csharp
var claims = new List<Claim>
{
    new Claim(ClaimTypes.Name, user.Email),
    new Claim("LastChanged", {Database Value})
};

var claimsIdentity = new ClaimsIdentity(
    claims, 
    CookieAuthenticationDefaults.AuthenticationScheme);

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Para implementar una invalidación para el `ValidatePrincipal` eventos, escribir un método con la siguiente firma en una clase que derive de [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

Un ejemplo es similar a lo siguiente:

```csharp
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Authentication.Cookies;

public class CustomCookieAuthenticationEvents : CookieAuthenticationEvents
{
    private readonly IUserRepository _userRepository;

    public CustomCookieAuthenticationEvents(IUserRepository userRepository)
    {
        // Get the database from registered DI services.
        _userRepository = userRepository;
    }

    public override async Task ValidatePrincipal(CookieValidatePrincipalContext context)
    {
        var userPrincipal = context.Principal;

        // Look for the LastChanged claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !_userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

Registrar la instancia de eventos durante el registro de servicio de cookie en el `ConfigureServices` método. Proporcionar un registro de servicio con ámbito para su `CustomCookieAuthenticationEvents` clase:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Para implementar una invalidación para el `ValidateAsync` eventos, escribir un método con la siguiente firma:

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

ASP.NET Core Identity implementa esta comprobación como parte de su [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync). Un ejemplo es similar a lo siguiente:

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = 
            context.HttpContext.RequestServices
                .GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

Registrar el evento durante la configuración de autenticación de cookie en el `Configure` método:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

---

Considere la posibilidad de una situación en la que se actualiza el nombre del usuario &mdash; una decisión que no afectan a la seguridad de ninguna manera. Si desea actualizar la entidad de seguridad de usuario de manera no destructiva, llame a `context.ReplacePrincipal` y establezca el `context.ShouldRenew` propiedad `true`.

> [!WARNING]
> El enfoque descrito aquí se desencadena en cada solicitud. Esto puede dar lugar a una reducción del rendimiento de gran tamaño de la aplicación.

## <a name="persistent-cookies"></a>Cookies persistentes

Puede que desee la cookie que se conservan entre sesiones del explorador. Esta persistencia solo debería habilitarse con el consentimiento del usuario explícita con una casilla "Recordar mi cuenta" en Inicio de sesión o un mecanismo similar. 

El fragmento de código siguiente crea una identidad y cookie correspondiente que sobrevive a través de los cierres de explorador. Se respetan los parámetros de expiración deslizante configurados previamente. Si la cookie expira mientras se cierra el explorador, el explorador borra la cookie de una vez que se reinicie.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

El [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) clase reside en el `Microsoft.AspNetCore.Authentication` espacio de nombres.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

El [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) clase reside en el `Microsoft.AspNetCore.Http.Authentication` espacio de nombres.

---

## <a name="absolute-cookie-expiration"></a>Expiración de cookie absoluta

Puede establecer un tiempo de expiración absoluta con `ExpiresUtc`. También debe establecer `IsPersistent`; en caso contrario, `ExpiresUtc` se omite y se crea una cookie de sesión único. Cuando `ExpiresUtc` está establecido en `SignInAsync`, invalida el valor de la `ExpireTimeSpan` opción de `CookieAuthenticationOptions`, si se establece.

El fragmento de código siguiente crea una identidad y cookie correspondiente que tiene una validez de 20 minutos. Esto omite cualquier configuración de expiración deslizante configurado previamente.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

---

## <a name="see-also"></a>Vea también

* [Cambios de autenticación 2.0 / anuncio de migración](https://github.com/aspnet/Announcements/issues/262)
* [Limitación de identidad por esquema](xref:security/authorization/limitingidentitybyscheme)
