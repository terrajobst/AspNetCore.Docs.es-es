---
title: "Mediante la autenticación de Cookie sin identidad principal de ASP.NET"
author: rick-anderson
description: "Obtener una explicación del uso de autenticación con cookies sin ASP.NET Core Identity"
keywords: "Núcleo de ASP.NET, las cookies"
ms.author: riande
manager: wpickett
ms.date: 08/14/2017
ms.topic: article
ms.assetid: 2bdcbf95-8d9d-4537-a4a0-a5ee439dcb62
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/cookie
ms.openlocfilehash: e5c53a7044edb56e065b2dc1536343fdaf9fb007
ms.sourcegitcommit: 7d8f4e3443a2989a64343f8fec83e6a4c4ed2f97
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2017
---
# <a name="using-cookie-authentication-without-aspnet-core-identity"></a>Mediante la autenticación de Cookie sin identidad principal de ASP.NET

<a name="security-authentication-cookie-middleware"></a>

ASP.NET Core 1.x proporciona una cookie [middleware](../../fundamentals/middleware.md#fundamentals-middleware) que serializa una entidad de seguridad de usuario en una cookie cifrada y, a continuación, en solicitudes posteriores, valida la cookie, vuelve a crear la entidad de seguridad y lo asigna a la `HttpContext.User` propiedad . Si desea proporcionar sus propias pantallas de inicio de sesión y las bases de datos de usuario, puede usar el middleware de cookies como una característica independiente.

Un cambio importante en ASP.NET Core 2.x es que el middleware de cookies está ausente. En su lugar, el `UseAuthentication` invocación del método en el `Configure` método *Startup.cs* agrega el AuthenticationMiddleware que establece el `HttpContext.User` propiedad.

<a name="security-authentication-cookie-middleware-configuring"></a>

## <a name="adding-and-configuring"></a>Agregar y configurar

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Complete los pasos siguientes:

- Si no usa el `Microsoft.AspNetCore.All` [metapackage](xref:fundamentals/metapackage), instale la versión 2.0 del `Microsoft.AspNetCore.Authentication.Cookies` paquete de NuGet en el proyecto.

- Invocar la `UseAuthentication` método en el `Configure` método de la *Startup.cs* archivo:

    ```csharp
    app.UseAuthentication();
    ```

- Invocar la `AddAuthentication` y `AddCookie` métodos en el `ConfigureServices` método de la *Startup.cs* archivo:

    ```csharp
    services.AddAuthentication("MyCookieAuthenticationScheme")
            .AddCookie("MyCookieAuthenticationScheme", options => {
                options.AccessDeniedPath = "/Account/Forbidden/";
                options.LoginPath = "/Account/Unauthorized/";
            });
    ```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Complete los pasos siguientes:

- Instalar el `Microsoft.AspNetCore.Authentication.Cookies` paquete de NuGet en el proyecto. Este paquete contiene el middleware de cookies.

- Agregue las líneas siguientes a la `Configure` método en su *Startup.cs* archivo antes de la `app.UseMvc()` instrucción:

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions()
    {
        AccessDeniedPath = "/Account/Forbidden/",
        AuthenticationScheme = "MyCookieAuthenticationScheme",
        AutomaticAuthenticate = true,
        AutomaticChallenge = true,
        LoginPath = "/Account/Unauthorized/"
    });
    ```

---

Los fragmentos de código anteriores configurar algunas o todas las opciones siguientes:

* `AccessDeniedPath`-Ésta es la ruta de acceso relativa a la que redirección las solicitudes cuando un usuario intenta obtener acceso a un recurso pero no pasa cualquier [directivas de autorización](xref:security/authorization/policies#security-authorization-policies-based) para ese recurso.

* `AuthenticationScheme`-Éste es un valor por el que se conoce un esquema de autenticación de cookie en particular. Esto es útil cuando hay varias instancias de autenticación con cookies y desea [limitar la autorización a una instancia](xref:security/authorization/limitingidentitybyscheme#security-authorization-limiting-by-scheme).

* `AutomaticAuthenticate`-Esta marca solo es relevante para ASP.NET Core 1.x. Indica que la autenticación con cookies debe ejecutarse en cada solicitud y que intentan validar y reconstruir cualquier entidad de seguridad serializado que creó.

* `AutomaticChallenge`-Esta marca solo es relevante para ASP.NET Core 1.x. Indica que la autenticación con cookies 1.x debe redirigir el explorador a la `LoginPath` o `AccessDeniedPath` cuando se produce un error en la autorización.

* `LoginPath`-Ésta es la ruta de acceso relativa a la que redirección las solicitudes cuando un usuario intenta obtener acceso a un recurso pero no se ha autenticado.

[Otras opciones](xref:security/authentication/cookie#security-authentication-cookie-options) incluyen la capacidad de establecer el emisor de notificaciones crea la autenticación con cookies, se quita el nombre de la cookie de la autenticación, el dominio de la cookie y diversas propiedades de seguridad en la cookie. De forma predeterminada, la autenticación con cookies utiliza opciones de seguridad adecuadas para las cookies que cree, como:
- Si establece la marca HttpOnly para impedir el acceso de la cookie en JavaScript del lado cliente
- Limita la cookie a HTTPS si una solicitud ha viajado a través de HTTPS

<a name="security-authentication-cookie-middleware-creating-a-cookie"></a>

## <a name="creating-an-identity-cookie"></a>Crear una cookie de identidad

Para crear una cookie que contiene la información de usuario, debe construir un [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) que contiene la información que se va a serializar en la cookie. Una vez que tenga una adecuado `ClaimsPrincipal` de objeto, llame a lo siguiente dentro del método de controlador:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync("MyCookieAuthenticationScheme", principal);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync("MyCookieAuthenticationScheme", principal);
```

---

Esto crea una cookie cifrada y lo agrega a la respuesta actual. El `AuthenticationScheme` especificado durante la [configuración](xref:security/authentication/cookie#security-authentication-cookie-middleware-configuring) debe usarse al llamar a `SignInAsync`.

Tras los bastidores, el cifrado usado es ASP.NET Core [protección de datos](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) sistema. Si hospedan en varios equipos, equilibrio de carga, o usar una granja de servidores web, a continuación, deberá [configurar la protección de datos](xref:security/data-protection/configuration/overview#data-protection-configuring) para usar el mismo anillo de clave y el identificador de la aplicación.

## <a name="signing-out"></a>Cierre de sesión

Para cerrar la sesión del usuario actual y eliminar las cookies, llame a lo siguiente en el controlador:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignOutAsync("MyCookieAuthenticationScheme");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignOutAsync("MyCookieAuthenticationScheme");
```

---

## <a name="reacting-to-back-end-changes"></a>Reaccionar ante los cambios de back-end

>[!WARNING]
> Una vez creada una cookie principal, se convierte en el único origen de identidad. Incluso si se deshabilita a un usuario en los sistemas back-end, la autenticación con cookies no tiene ningún conocimiento de este, y un usuario permanece ha iniciado sesión como su cookie es válida.

La autenticación con cookies proporciona una serie de eventos en su clase de opción. El `ValidateAsync()` evento puede utilizarse para interceptar e invalidar la validación de la identidad de la cookie.

Considere la posibilidad de una base de datos de usuario de back-end que puede tener una columna "LastChanged". Con el fin de invalidar una cookie cuando cambia la base de datos, debe en primer lugar, cuando [crear la cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie), agregar una notificación de "LastChanged" que contiene el valor actual. Cuando se cambia la base de datos, debe actualizarse el valor de "LastChanged".

Para implementar una invalidación para el `ValidateAsync()` eventos, debe escribir un método con la siguiente firma:

```csharp
Task ValidateAsync(CookieValidatePrincipalContext context);
```

ASP.NET Core Identity implementa esta comprobación como parte de su `SecurityStampValidator`. Un ejemplo es similar a lo siguiente:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = context.HttpContext.RequestServices.GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        string lastChanged;
        lastChanged = (from c in userPrincipal.Claims
                        where c.Type == "LastUpdated"
                        select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(userPrincipal, lastChanged))
        {
            context.RejectPrincipal();
            await context.HttpContext.SignOutAsync("MyCookieAuthenticationScheme");
        }
    }
}
```

Esto podría, a continuación, conectarse durante el registro de servicio de cookie en el `ConfigureServices` método *Startup.cs*:

```csharp
services.AddAuthentication("MyCookieAuthenticationScheme")
        .AddCookie("MyCookieAuthenticationScheme", options =>
        {
            options.Events = new CookieAuthenticationEvents
            {
                OnValidatePrincipal = LastChangedValidator.ValidateAsync
            };
        });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = context.HttpContext.RequestServices.GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        string lastChanged;
        lastChanged = (from c in userPrincipal.Claims
                        where c.Type == "LastUpdated"
                        select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(userPrincipal, lastChanged))
        {
            context.RejectPrincipal();
            await context.HttpContext.Authentication.SignOutAsync("MyCookieAuthenticationScheme");
        }
    }
}
```

Esto, a continuación, podría estar conectado seguridad durante la configuración de autenticación de cookie en el `Configure` método *Startup.cs*:

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

Considere el ejemplo en el que se ha actualizado su nombre &mdash; una decisión que no afectan a la seguridad de ninguna manera. Si desea actualizar la entidad de seguridad de usuario de manera no destructiva, puede llamar a `context.ReplacePrincipal()` y establezca el `context.ShouldRenew` propiedad `true`.

<a name="security-authentication-cookie-options"></a>

## <a name="controlling-cookie-options"></a>Controlar opciones de cookie

El [CookieAuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) clase viene con distintas opciones de configuración para ajustar las cookies que se está creadas.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET Core 2.x unifica las API utilizadas para la configuración de cookies. 1.x API se han marcado como obsoletas y una nueva `Cookie` propiedad de tipo `CookieBuilder` se ha introducido en el `CookieAuthenticationOptions` clase. Se recomienda encarecidamente que migre a la API de 2.x.

* `ClaimsIssuer`es el emisor que se usará para la [emisor](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) propiedad en las notificaciones que se creó mediante la autenticación con cookies.

* `CookieBuilder.Domain`es el nombre de dominio al que se sirve la cookie. De forma predeterminada, este es el nombre de host que se envió la solicitud al. El explorador sólo sirve la cookie a un nombre de host coincidente. Puede que desee ajustar esta opción para que las cookies disponibles para todos los hosts en el dominio. Por ejemplo, establecer el dominio de cookies `.contoso.com` pone a disposición `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`, etcetera.

* `CookieBuilder.HttpOnly`es una marca que indica si la cookie debe ser accesible sólo a los servidores. El valor predeterminado es `true`. Si cambia este valor puede abrir la aplicación al robo de cookies debe la aplicación tenga un error de Scripting entre sitios.

* `CookieBuilder.Path`puede utilizarse para aislar las aplicaciones que se ejecutan en el mismo nombre de host. Si tiene aplicaciones que se ejecutan `/app1` y desea limitar las cookies que se emite para sólo se enviará a esa aplicación, a continuación, debe establecer el `CookiePath` propiedad `/app1`. Al hacerlo, la cookie solo está disponible para las solicitudes a `/app1` o cualquier aparecen debajo de él.

* `CookieBuilder.SameSite`indica si el explorador debe permitir que la cookie que se adjuntará a las solicitudes del mismo sitio o entre sitios. El valor predeterminado es `SameSiteMode.Lax`.

* `CookieBuilder.SecurePolicy`es una marca que indica si la cookie creada debe limitarse a HTTPS, HTTP o HTTPS o el mismo protocolo que la solicitud. El valor predeterminado es `SameAsRequest`.

* `ExpireTimeSpan`es el `TimeSpan` tras el cual expira la cookie. Se agrega a la fecha y hora actuales para crear la fecha de expiración de la cookie.

* `SlidingExpiration`es una marca que indica si la fecha de expiración de la cookie restablece cuando haya más de la mitad de la `ExpireTimeSpan` ha transcurrido el intervalo. La nueva fecha de expiración se mueve hacia delante como la fecha actual más el `ExpireTimespan`. Un [la hora de expiración absoluta](xref:security/authentication/cookie#security-authentication-absolute-expiry) puede establecerse mediante el `AuthenticationProperties` al llamar a la clase `SignInAsync`. Una fecha de expiración absoluta puede mejorar la seguridad de la aplicación mediante la limitación de la cantidad de tiempo para el que la cookie de autenticación es válida.

Un ejemplo del uso `CookieAuthenticationOptions` en el `ConfigureServices` método *Startup.cs* sigue:

```csharp
services.AddAuthentication("MyCookieAuthenticationScheme")
        .AddCookie("MyCookieAuthenticationScheme", options =>
        {
            options.Cookie.Name = "AuthCookie";
            options.Cookie.Domain = "contoso.com";
            options.Cookie.Path = "/";
            options.Cookie.HttpOnly = true;
            options.Cookie.SameSite = SameSiteMode.Lax;
            options.Cookie.SecurePolicy = CookieSecurePolicy.Always;
        });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* `ClaimsIssuer`es el emisor que se usará para la [emisor](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) propiedad en las notificaciones que se creó el middleware.

* `CookieDomain`es el nombre de dominio al que se sirve la cookie. De forma predeterminada, este es el nombre de host que se envió la solicitud al. El explorador sólo sirve la cookie a un nombre de host coincidente. Puede que desee ajustar esta opción para que las cookies disponibles para todos los hosts en el dominio. Por ejemplo, establecer el dominio de cookies `.contoso.com` pone a disposición `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`, etcetera.

* `CookieHttpOnly`es una marca que indica si la cookie debe ser accesible sólo a los servidores. El valor predeterminado es `true`. Si cambia este valor puede abrir la aplicación al robo de cookies debe la aplicación tenga un error de Scripting entre sitios.

* `CookiePath`puede utilizarse para aislar las aplicaciones que se ejecutan en el mismo nombre de host. Si tiene aplicaciones que se ejecutan `/app1` y desea limitar las cookies que se emite para sólo se enviará a esa aplicación, a continuación, debe establecer el `CookiePath` propiedad `/app1`. Al hacerlo, la cookie solo está disponible para las solicitudes a `/app1` o cualquier aparecen debajo de él.

* `CookieSecure`es una marca que indica si la cookie creada debe limitarse a HTTPS, HTTP o HTTPS o el mismo protocolo que la solicitud. El valor predeterminado es `SameAsRequest`.

* `ExpireTimeSpan`es el `TimeSpan` tras el cual expira la cookie. Se agrega a la fecha y hora actuales para crear la fecha de expiración de la cookie.

* `SlidingExpiration`es una marca que indica si la fecha de expiración de la cookie restablece cuando haya más de la mitad de la `ExpireTimeSpan` ha transcurrido el intervalo. La nueva fecha de expiración se mueve hacia delante como la fecha actual más el `ExpireTimespan`. Un [la hora de expiración absoluta](xref:security/authentication/cookie#security-authentication-absolute-expiry) puede establecerse mediante el `AuthenticationProperties` al llamar a la clase `SignInAsync`. Una fecha de expiración absoluta puede mejorar la seguridad de la aplicación mediante la limitación de la cantidad de tiempo para el que la cookie de autenticación es válida.

Un ejemplo del uso `CookieAuthenticationOptions` en el `Configure` método *Startup.cs* sigue:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    CookieName = "AuthCookie",
    CookieDomain = "contoso.com",
    CookiePath = "/",
    CookieHttpOnly = true,
    CookieSecure = CookieSecurePolicy.Always
});
```

---

## <a name="persistent-cookies-and-absolute-expiry-times"></a>Las cookies persistentes y las horas de expiración absoluta

Puede que desee la expiración de la cookie se conservan entre sesiones del explorador y desea que una fecha de expiración absoluta para la identidad y el token de transporte. Esta persistencia solo debería habilitarse con el consentimiento del usuario explícita, a través de una casilla "Recordar mi cuenta" en Inicio de sesión o un mecanismo similar. Puede realizar estas acciones mediante el uso de la `AuthenticationProperties` parámetro en el `SignInAsync` llama al método cuando [en una identidad de firma y la creación de la cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie). Por ejemplo:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

El `AuthenticationProperties` (clase), utilizado en el fragmento de código anterior, se encuentra en la `Microsoft.AspNetCore.Authentication` espacio de nombres.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

El `AuthenticationProperties` (clase), utilizado en el fragmento de código anterior, se encuentra en la `Microsoft.AspNetCore.Http.Authentication` espacio de nombres.

---

El fragmento de código anterior crea una identidad y cookie correspondiente que sobrevive a través de los cierres de explorador. Cualquier configuración de expiración deslizante configurada anteriormente mediante [opciones de cookie](xref:security/authentication/cookie#security-authentication-cookie-options) se aplican aún. Si la cookie expira mientras se cierra el explorador, el explorador borra una vez que se reinicie.

<a name="security-authentication-absolute-expiry"></a>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

---

El fragmento de código anterior crea una identidad y cookie correspondiente que tiene una validez de 20 minutos. Esto omite cualquier configuración de expiración deslizante configurada anteriormente mediante [opciones de cookie](xref:security/authentication/cookie#security-authentication-cookie-options).

El `ExpiresUtc` y `IsPersistent` propiedades son mutuamente excluyentes.
