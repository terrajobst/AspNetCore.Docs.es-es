---
title: ASP.NET Core 2,1 MVC SameSite ejemplo de cookie
author: rick-anderson
description: ASP.NET Core 2,1 MVC SameSite ejemplo de cookie
monikerRange: '>= aspnetcore-2.1 < aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/03/2019
uid: security/samesite/mvc21
ms.openlocfilehash: 14f3d3d27d5740519e1ba57529d5694b4cdeb9ec
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78654995"
---
# <a name="aspnet-core-21-mvc-samesite-cookie-sample"></a>ASP.NET Core 2,1 MVC SameSite ejemplo de cookie

ASP.NET Core 2,1 tiene compatibilidad integrada con el atributo [SameSite](https://www.owasp.org/index.php/SameSite) , pero se escribió en el estándar original. El [comportamiento revisado](https://github.com/dotnet/aspnetcore/issues/8212) cambió el significado de `SameSite.None` para emitir el atributo sameSite con un valor de `None`, en lugar de no emitir el valor. Si no desea emitir el valor, puede establecer la propiedad `SameSite` de una cookie en-1.

## <a name="sampleCode"></a>Escribir el atributo SameSite

A continuación se describe un ejemplo de cómo escribir un atributo SameSite en una cookie:

```c#
var cookieOptions = new CookieOptions
{
    // Set the secure flag, which Chrome's changes will require for SameSite none.
    // Note this will also require you to be running on HTTPS
    Secure = true,

    // Set the cookie to HTTP only which is good practice unless you really do need
    // to access it client side in scripts.
    HttpOnly = true,

    // Add the SameSite attribute, this will emit the attribute with a value of none.
    // To not emit the attribute at all set the SameSite property to (SameSiteMode)(-1).
    SameSite = SameSiteMode.None
};

// Add the cookie to the response cookie collection
Response.Cookies.Append(CookieName, "cookieValue", cookieOptions);
```

## <a name="setting-cookie-authentication-and-session-state-cookies"></a>Establecer cookies de autenticación de cookies y de estado de sesión

Autenticación de cookies, estado de sesión y [otros componentes](https://docs.microsoft.com/aspnet/core/security/samesite?view=aspnetcore-2.1) establezca sus opciones de sameSite a través de las opciones de cookie, por ejemplo

```c#
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.Cookie.SameSite = SameSiteMode.None;
        options.Cookie.SecurePolicy = CookieSecurePolicy.Always;
        options.Cookie.IsEssential = true;
    });

services.AddSession(options =>
{
    options.Cookie.SameSite = SameSiteMode.None;
    options.Cookie.SecurePolicy = CookieSecurePolicy.Always;
    options.Cookie.IsEssential = true;
});
```

En el código anterior, la autenticación de cookies y el estado de sesión establecen el atributo sameSite en `None`, emitiendo el atributo con un valor `None` y también establecen el atributo Secure en true.

### <a name="run-the-sample"></a>Ejecución del ejemplo

Si ejecuta el [proyecto de ejemplo](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore21MVC), cargue el depurador del explorador en la página inicial y úselo para ver la colección de cookies para el sitio. Para ello, en Edge y Chrome, presione `F12`, después, seleccione la pestaña `Application` y haga clic en la dirección URL del sitio en la opción `Cookies` de la sección `Storage`.

![Lista de cookies del depurador del explorador](BrowserDebugger.png)

Puede ver en la imagen anterior que la cookie creada por el ejemplo al hacer clic en el botón "crear cookie de SameSite" tiene un valor de atributo SameSite de `Lax`, que coincide con el valor establecido en el [código de ejemplo](#sampleCode).

## <a name="interception"></a>Interceptar cookies

Para interceptar las cookies, para ajustar el valor NONE según su compatibilidad en el agente del explorador del usuario, debe usar el middleware `CookiePolicy`. Debe colocarse en la canalización de solicitudes HTTP **antes** que los componentes que escriben cookies y se configuran en `ConfigureServices()`.

Para insertarlo en la canalización, use `app.UseCookiePolicy()` en el método `Configure(IApplicationBuilder, IHostingEnvironment)` de [Startup.CS](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNetCore21MVC/Startup.cs). Por ejemplo:

```c#
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
       app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseCookiePolicy();
    app.UseAuthentication();
    app.UseSession();

    app.UseMvc(routes =>
    {
        routes.MapRoute(
            name: "default",
            template: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

A continuación, en el `ConfigureServices(IServiceCollection services)` configurar la Directiva de cookies para llamar a una clase auxiliar cuando se anexan o se eliminan cookies. Por ejemplo:

```c#
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<CookiePolicyOptions>(options =>
    {
        options.CheckConsentNeeded = context => true;
        options.MinimumSameSitePolicy = SameSiteMode.None;
        options.OnAppendCookie = cookieContext =>
            CheckSameSite(cookieContext.Context, cookieContext.CookieOptions);
        options.OnDeleteCookie = cookieContext =>
            CheckSameSite(cookieContext.Context, cookieContext.CookieOptions);
    });
}

private void CheckSameSite(HttpContext httpContext, CookieOptions options)
{
    if (options.SameSite == SameSiteMode.None)
    {
        var userAgent = httpContext.Request.Headers["User-Agent"].ToString();
        if (SameSite.BrowserDetection.DisallowsSameSiteNone(userAgent))
        {
            options.SameSite = (SameSiteMode)(-1);
        }
    }
}
```

La función auxiliar `CheckSameSite(HttpContext, CookieOptions)`:

* Se llama a cuando se anexan cookies a la solicitud o se eliminan de la solicitud.
* Comprueba si la propiedad `SameSite` está establecida en `None`.
* Si `SameSite` se establece en `None` y se sabe que el agente de usuario actual no admite el valor del atributo none. La comprobación se realiza mediante la clase [SameSiteSupport](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/samesite/sample/snippets/SameSiteSupport.cs) :
  * Establece `SameSite` para no emitir el valor estableciendo la propiedad en `(SameSiteMode)(-1)`

## <a name="targeting-net-framework"></a>Destinar .NET Framework

ASP.NET Core y System. Web (ASP.NET clásico) tienen implementaciones independientes de SameSite. Las revisiones de SameSite KB para .NET Framework no son necesarias si se usa ASP.NET Core y el requisito de versión mínima del marco de trabajo de System. Web SameSite (.NET 4.7.2) se aplican a ASP.NET Core.

ASP.NET Core en .NET requiere la actualización de las dependencias de paquetes Nuget para obtener las correcciones correspondientes.

Para obtener los cambios de ASP.NET Core para .NET Framework Asegúrese de que tiene una referencia directa a las versiones y los paquetes revisados (2.1.14 o versiones posteriores 2,1).

```xml
<PackageReference Include="Microsoft.Net.Http.Headers" Version="2.1.14" />
<PackageReference Include="Microsoft.AspNetCore.CookiePolicy" Version="2.1.14" />
```

### <a name="more-information"></a>Más información
 

de [actualizaciones de Chrome](https://www.chromium.org/updates/same-site) [ASP.net Core documentación de SameSite](https://docs.microsoft.com/aspnet/core/security/samesite?view=aspnetcore-2.1)
[ASP.net Core 2,1 SameSite de cambios](https://github.com/dotnet/aspnetcore/issues/8212) en el anuncio