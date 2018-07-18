---
title: Configuración de ASP.NET Core para trabajar con servidores proxy y equilibradores de carga
author: guardrex
description: Aprenda sobre la configuración de las aplicaciones hospedadas detrás de los servidores proxy y equilibradores de carga, que con frecuencia ocultan información importante sobre las solicitudes.
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2018
uid: host-and-deploy/proxy-load-balancer
ms.openlocfilehash: b04219803477c9dc1c25077cde117fc629f8b6fb
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938503"
---
# <a name="configure-aspnet-core-to-work-with-proxy-servers-and-load-balancers"></a>Configuración de ASP.NET Core para trabajar con servidores proxy y equilibradores de carga

Por [Luke Latham](https://github.com/guardrex) y [Chris Ross](https://github.com/Tratcher)

En la configuración recomendada de ASP.NET Core, la aplicación se hospeda mediante IIS/módulo ASP.NET Core, Nginx o Apache. Los servidores proxy, los equilibradores de carga y otros dispositivos de red con frecuencia ocultan información sobre la solicitud antes de que llegue a la aplicación:

* Cuando las solicitudes HTTPS se redirigen mediante proxy a través de HTTP, el esquema original (HTTPS) se pierde y se debe reenviar en un encabezado.
* Como una aplicación recibe una solicitud del proxy y no desde su verdadero origen en Internet o la red corporativa, la dirección IP del cliente de origen también se debe reenviar en el encabezado.

Esta información puede ser importante en el procesamiento de las solicitudes, por ejemplo, en los redireccionamientos, la autenticación, la generación de vínculos, la evaluación de directivas y la geolocalización del cliente.

## <a name="forwarded-headers"></a>Encabezados reenviados

Por costumbre, los servidores proxy reenvían la información en encabezados HTTP.

| Header | Description |
| ------ | ----------- |
| X-Forwarded-For | Contiene información sobre el cliente que inició la solicitud y los servidores proxy posteriores en una cadena de servidores proxy. Este parámetro puede contener direcciones IP (y, opcionalmente, números de puerto). En una cadena de servidores proxy, el primer parámetro indica al cliente dónde se realizó primero la solicitud. Le siguen los identificadores de proxy posteriores. El último proxy en la cadena no se encuentra en la lista de parámetros. La última dirección IP del proxy y, opcionalmente, un número de puerto, está disponible como la dirección IP remota en la capa de transporte. |
| X-Forwarded-Proto | El valor del esquema de origen (HTTP/HTTPS). El valor también puede ser una lista de esquemas si la solicitud ha pasado por varios servidores proxy. |
| X-Forwarded-Host | El valor original del campo de encabezado de host. Por lo general, los servidores proxy no modifican el encabezado de host. Consulte [Microsoft Security Advisory CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) para información sobre una vulnerabilidad de elevación de privilegios que afecta a sistemas donde el proxy no valida ni restringe los encabezados de host a valores buenos conocidos. |

El Middleware de encabezados reenviados, del paquete [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/), lee estos encabezados y rellena los campos asociados en [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).

El middleware realiza las siguientes actualizaciones:

* [HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress): establézcalo mediante el valor de encabezado `X-Forwarded-For`. Los valores de configuración adicionales afectan a cómo el middleware establece `RemoteIpAddress`. Para más información, consulte la sección [Opciones de Middleware de encabezados reenviados](#forwarded-headers-middleware-options).
* [HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme): establézcalo mediante el valor de encabezado `X-Forwarded-Proto`.
* [HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host): establézcalo mediante el valor de encabezado `X-Forwarded-Host`.

Se pueden configurar los [valores predeterminados](#forwarded-headers-middleware-options) del Middleware de encabezados reenviados. Estos valores son:

* Solo hay *un proxy* entre la aplicación y el origen de las solicitudes.
* Solo las direcciones de bucle invertido se configuran para servidores proxy conocidos y redes conocidas.
* Los encabezados reenviados se denominan `X-Forwarded-For` y `X-Forwarded-Proto`.

No todos los dispositivos de red agregan los encabezados `X-Forwarded-For` y `X-Forwarded-Proto` sin configuración adicional. Consulte las instrucciones del fabricante de su dispositivo si las solicitudes redirigidas mediante proxy no contienen estos encabezados cuando llegan a la aplicación. Si el dispositivo usa nombres de encabezado distintos a `X-Forwarded-For` y `X-Forwarded-Proto`, establezca las opciones [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) y [ForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) para que coincidan con los nombres de encabezado empleados por el dispositivo. Para obtener más información, vea [Opciones del Middleware de encabezados reenviados](#forwarded-headers-middleware-options) y [Configuración de un proxy que usa otros nombres de encabezado](#configuration-for-a-proxy-that-uses-different-header-names).

## <a name="iisiis-express-and-aspnet-core-module"></a>IIS o IIS Express y el módulo ASP.NET Core

El Middleware de encabezados reenviados se habilita de forma predeterminada mediante el Middleware de IIS Integration cuando la aplicación se ejecuta detrás de IIS y del módulo ASP.NET Core. El Middleware de encabezados reenviados está activado para ejecutarse primero en la canalización de middleware con una configuración restringida específica del módulo ASP.NET Core debido a problemas de confianza con los encabezados reenviados (por ejemplo, [suplantación de IP](https://www.iplocation.net/ip-spoofing)). El middleware está configurado para reenviar los encabezados `X-Forwarded-For` y `X-Forwarded-Proto` y está restringido a un único proxy localhost. Si se requiere configuración adicional, consulte la sección [Opciones del Middleware de encabezados reenviados](#forwarded-headers-middleware-options).

## <a name="other-proxy-server-and-load-balancer-scenarios"></a>Otros escenarios de servidor proxy y equilibrador de carga

Al margen del uso del Middleware de IIS Integration, el Middleware de encabezados reenviados no está habilitado de forma predeterminada. El Middleware de encabezados reenviados debe estar habilitado en una aplicación para procesar los encabezados reenviados con [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders). Después de habilitar el middleware, si no se especifica [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) para él, el valor predeterminado [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) es [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).

Configure el middleware con [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) para reenviar los encabezados `X-Forwarded-For` y `X-Forwarded-Proto` en `Startup.ConfigureServices`. Invoque el método [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) en `Startup.Configure` antes de llamar a otro middleware:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.Configure<ForwardedHeadersOptions>(options =>
    {
        options.ForwardedHeaders = 
            ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto;
    });
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseForwardedHeaders();

    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseStaticFiles();
    // In ASP.NET Core 1.x, replace the following line with: app.UseIdentity();
    app.UseAuthentication();
    app.UseMvc();
}
```

> [!NOTE]
> Si no se especifica ningún valor [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) en `Startup.ConfigureServices` o directamente para el método de extensión con [UseForwardedHeaders(IApplicationBuilder, ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_), los encabezados predeterminados que se reenvían son [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). La propiedad [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) se debe configurar con los encabezados que se reenvían.

## <a name="nginx-configuration"></a>Configuración de Nginx

Para reenviar los encabezados `X-Forwarded-For` y `X-Forwarded-Proto`, vea <xref:host-and-deploy/linux-nginx#configure-nginx>. Para más información, vea [NGINX: Using the Forwarded header](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/) (NGINX: Uso del encabezado Forwarded).

## <a name="apache-configuration"></a>Configuración de Apache

`X-Forwarded-For` se agrega automáticamente. Vea [Apache Module mod_proxy: Reverse Proxy Request Headers](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html#x-headers) (Módulo de Apache mod_proxy: Encabezados de solicitud de proxy inverso). Para obtener información sobre cómo reenviar el encabezado `X-Forwarded-Proto`, vea <xref:host-and-deploy/linux-apache#configure-apache>.

## <a name="forwarded-headers-middleware-options"></a>Opciones del Middleware de encabezados reenviados

[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) controla el comportamiento del middleware de encabezados reenviados. En el ejemplo siguiente se cambian los valores predeterminados:

* Limite el número de entradas de los encabezados reenviados a `2`.
* Agregue una dirección de proxy conocida de `127.0.10.1`.
* Cambie el nombre del encabezado reenviado del valor predeterminado `X-Forwarded-For` a `X-Forwarded-For-My-Custom-Header-Name`.

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardLimit = 2;
    options.KnownProxies.Add(IPAddress.Parse("127.0.10.1"));
    options.ForwardedForHeaderName = "X-Forwarded-For-My-Custom-Header-Name";
});
```

::: moniker range=">= aspnetcore-2.1"
| Opción | Description |
| ------ | ----------- |
| AllowedHosts | Restringe los hosts por el encabezado `X-Forwarded-Host` a los valores proporcionados.<ul><li>Los valores se comparan mediante ordinal-ignore-case.</li><li>Se deben excluir los números de puerto.</li><li>Si la lista está vacía, se permiten todos los hosts.</li><li>Un carácter comodín de nivel superior `*` permite que todos los hosts que no están vacíos.</li><li>Se permiten caracteres comodín de subdominio, pero no coinciden con el dominio raíz. Por ejemplo, `*.contoso.com` coincide con el subdominio `foo.contoso.com` pero no con el dominio raíz `contoso.com`.</li><li>Se permiten nombres de host Unicode, pero se convierten en [Punycode](https://tools.ietf.org/html/rfc3492) para buscar la coincidencia.</li><li>Las [direcciones IPv6](https://tools.ietf.org/html/rfc4291) deben incluir corchetes de enlace y estar en [formato convencional](https://tools.ietf.org/html/rfc4291#section-2.2) (por ejemplo, `[ABCD:EF01:2345:6789:ABCD:EF01:2345:6789]`). Las direcciones IPv6 no usan mayúsculas y minúsculas de forma especial para buscar la igualdad lógica entre diferentes formatos, y no se realiza ninguna canonización.</li><li>Si no se restringen los hosts permitidos, un atacante podría suplantar los vínculos generados por el servicio.</li></ul>El valor predeterminado es un elemento [IList\<string>](/dotnet/api/system.collections.generic.ilist-1) vacío. |
| [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | Use el encabezado especificado por esta propiedad en lugar del especificado por [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername). Esta opción se usa cuando el reenviador o proxy no emplea el encabezado `X-Forwarded-For` sino algún otro para reenviar la información.<br><br>El valor predeterminado es `X-Forwarded-For`. |
| [ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | Identifica qué reenviadores se deben procesar. Consulte [ForwardedHeaders Enum](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) para obtener la lista de campos que se aplican. Los valores típicos que se asignan a esta propiedad son <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.<br><br>El valor predeterminado es [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). |
| [ForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | Use el encabezado especificado por esta propiedad en lugar del especificado por [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername). Esta opción se usa cuando el reenviador o proxy no emplea el encabezado `X-Forwarded-Host` sino algún otro para reenviar la información.<br><br>El valor predeterminado es `X-Forwarded-Host`. |
| [ForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | Use el encabezado especificado por esta propiedad en lugar del especificado por [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername). Esta opción se usa cuando el reenviador o proxy no emplea el encabezado `X-Forwarded-Proto` sino algún otro para reenviar la información.<br><br>El valor predeterminado es `X-Forwarded-Proto`. |
| [ForwardLimit](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | Limita el número de entradas en los encabezados que se procesan. Establézcalo en `null` para deshabilitar el límite, pero esto solo se debe realizar si están configurados `KnownProxies` o `KnownNetworks`.<br><br>El valor predeterminado es 1. |
| [KnownNetworks](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | Intervalos de direcciones de servidores proxy conocidos de los que se aceptan encabezados reenviados. Proporcione intervalos de direcciones IP mediante la notación de Enrutamiento de interdominios sin clases (CIDR).<br><br>El valor predeterminado es [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPNetwork](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> que contiene una única entrada para `IPAddress.Loopback`. |
| [KnownProxies](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | Direcciones de servidores proxy conocidos de los que se aceptan encabezados reenviados. Use `KnownProxies` para especificar las coincidencias exactas de direcciones IP.<br><br>El valor predeterminado es [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> que contiene una única entrada para `IPAddress.IPv6Loopback`. |
| [OriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | Use el encabezado especificado por esta propiedad en lugar del especificado por [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).<br><br>El valor predeterminado es `X-Original-For`. |
| [OriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | Use el encabezado especificado por esta propiedad en lugar del especificado por [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).<br><br>El valor predeterminado es `X-Original-Host`. |
| [OriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | Use el encabezado especificado por esta propiedad en lugar del especificado por [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).<br><br>El valor predeterminado es `X-Original-Proto`. |
| [RequireHeaderSymmetry](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | Requiere que el número de valores de encabezado esté sincronizado entre los valores [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) que se van a procesar.<br><br>El valor predeterminado en ASP.NET Core 1.x es `true`. El valor predeterminado en ASP.NET Core 2.0 o posterior es `false`. |
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
| Opción | Description |
| ------ | ----------- |
| [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | Use el encabezado especificado por esta propiedad en lugar del especificado por [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername). Esta opción se usa cuando el reenviador o proxy no emplea el encabezado `X-Forwarded-For` sino algún otro para reenviar la información.<br><br>El valor predeterminado es `X-Forwarded-For`. |
| [ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | Identifica qué reenviadores se deben procesar. Consulte [ForwardedHeaders Enum](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) para obtener la lista de campos que se aplican. Los valores típicos que se asignan a esta propiedad son <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.<br><br>El valor predeterminado es [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). |
| [ForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | Use el encabezado especificado por esta propiedad en lugar del especificado por [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername). Esta opción se usa cuando el reenviador o proxy no emplea el encabezado `X-Forwarded-Host` sino algún otro para reenviar la información.<br><br>El valor predeterminado es `X-Forwarded-Host`. |
| [ForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | Use el encabezado especificado por esta propiedad en lugar del especificado por [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername). Esta opción se usa cuando el reenviador o proxy no emplea el encabezado `X-Forwarded-Proto` sino algún otro para reenviar la información.<br><br>El valor predeterminado es `X-Forwarded-Proto`. |
| [ForwardLimit](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | Limita el número de entradas en los encabezados que se procesan. Establézcalo en `null` para deshabilitar el límite, pero esto solo se debe realizar si están configurados `KnownProxies` o `KnownNetworks`.<br><br>El valor predeterminado es 1. |
| [KnownNetworks](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | Intervalos de direcciones de servidores proxy conocidos de los que se aceptan encabezados reenviados. Proporcione intervalos de direcciones IP mediante la notación de Enrutamiento de interdominios sin clases (CIDR).<br><br>El valor predeterminado es [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPNetwork](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> que contiene una única entrada para `IPAddress.Loopback`. |
| [KnownProxies](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | Direcciones de servidores proxy conocidos de los que se aceptan encabezados reenviados. Use `KnownProxies` para especificar las coincidencias exactas de direcciones IP.<br><br>El valor predeterminado es [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> que contiene una única entrada para `IPAddress.IPv6Loopback`. |
| [OriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | Use el encabezado especificado por esta propiedad en lugar del especificado por [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).<br><br>El valor predeterminado es `X-Original-For`. |
| [OriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | Use el encabezado especificado por esta propiedad en lugar del especificado por [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).<br><br>El valor predeterminado es `X-Original-Host`. |
| [OriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | Use el encabezado especificado por esta propiedad en lugar del especificado por [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).<br><br>El valor predeterminado es `X-Original-Proto`. |
| [RequireHeaderSymmetry](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | Requiere que el número de valores de encabezado esté sincronizado entre los valores [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) que se van a procesar.<br><br>El valor predeterminado en ASP.NET Core 1.x es `true`. El valor predeterminado en ASP.NET Core 2.0 o posterior es `false`. |
::: moniker-end

## <a name="scenarios-and-use-cases"></a>Escenarios y casos de uso

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a>Cuando no es posible agregar encabezados reenviados y todas las solicitudes son seguras

En algunos casos, puede que no sea posible agregar encabezados reenviados a las solicitudes redirigidas mediante proxy a la aplicación. Si el proxy está forzando a que todas las solicitudes externas públicas sean HTTPS, el esquema se puede establecer manualmente en `Startup.Configure` antes de usar cualquier tipo de middleware:

```csharp
app.Use((context, next) =>
{
    context.Request.Scheme = "https";
    return next();
});
```

Este código puede deshabilitarse con una variable de entorno u otro valor de configuración en un entorno de desarrollo o ensayo.

### <a name="deal-with-path-base-and-proxies-that-change-the-request-path"></a>Tratar con la ruta de acceso base y los servidores proxy que cambian la ruta de acceso de la solicitud

Algunos servidores proxy pasan la ruta de acceso sin cambios pero con una ruta de acceso base de aplicación que se debe quitar para que el enrutamiento funcione correctamente. El middleware [UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase) divide la ruta de acceso en [HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) y la ruta de acceso base de aplicación en [HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase).

Si `/foo` es la ruta de acceso base de aplicación para una ruta de acceso de proxy que se pasa como `/foo/api/1`, el middleware establece `Request.PathBase` en `/foo` y `Request.Path` en `/api/1` con el siguiente comando:

```csharp
app.UsePathBase("/foo");
```

La ruta de acceso base y la ruta de acceso original se vuelven a aplicar cuando se llama de nuevo al middleware en orden inverso. Para obtener más información sobre el procesamiento de pedidos del middleware, vea <xref:fundamentals/middleware/index>.

Si el proxy recorta la ruta de acceso (por ejemplo, el reenvío `/foo/api/1` a `/api/1`), corrija los redireccionamientos y los vínculos mediante el establecimiento de la propiedad [PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase) de la solicitud:

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

Si el proxy va a agregar datos de ruta de acceso, descarte parte de esta ruta para corregir los redireccionamientos y los vínculos; para ello, use [StartsWithSegments(PathString, PathString)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__) y asígnelo a la propiedad [Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path):

```csharp
app.Use((context, next) =>
{
    if (context.Request.Path.StartsWithSegments("/foo", out var remainder))
    {
        context.Request.Path = remainder;
    }

    return next();
});
```

### <a name="configuration-for-a-proxy-that-uses-different-header-names"></a>Configuración de un proxy que usa otros nombres de encabezado

Si el proxy no usa los encabezados denominados `X-Forwarded-For` y `X-Forwarded-Proto` para reenviar el puerto o la dirección de proxy y originar información de esquema, establezca las opciones [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) y [ForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) de modo que coincidan con los nombres de encabezado empleados por el proxy:

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardedForHeaderName = "Header_Name_Used_By_Proxy_For_X-Forwarded-For_Header";
    options.ForwardedProtoHeaderName = "Header_Name_Used_By_Proxy_For_X-Forwarded-Proto_Header";
});
```

## <a name="troubleshoot"></a>Solucionar problemas

Cuando no se reenvíen los encabezados como estaba previsto, habilite el [registro](xref:fundamentals/logging/index). Si los registros no proporcionan suficiente información para solucionar el problema, enumere los encabezados de solicitud recibidos por el servidor. Los encabezados se pueden escribir en una respuesta de aplicación mediante middleware insertado:

```csharp
public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory)
{
    app.Run(async (context) =>
    {
        context.Response.ContentType = "text/plain";

        // Request method, scheme, and path
        await context.Response.WriteAsync(
            $"Request Method: {context.Request.Method}{Environment.NewLine}");
        await context.Response.WriteAsync(
            $"Request Scheme: {context.Request.Scheme}{Environment.NewLine}");
        await context.Response.WriteAsync(
            $"Request Path: {context.Request.Path}{Environment.NewLine}");

        // Headers
        await context.Response.WriteAsync($"Request Headers:{Environment.NewLine}");

        foreach (var header in context.Request.Headers)
        {
            await context.Response.WriteAsync($"{header.Key}: " +
                $"{header.Value}{Environment.NewLine}");
        }

        await context.Response.WriteAsync(Environment.NewLine);

        // Connection: RemoteIp
        await context.Response.WriteAsync(
            $"Request RemoteIp: {context.Connection.RemoteIpAddress}");
    });
}
```

Asegúrese de que el servidor reciba los encabezados X-Forwarded-* con los valores esperados. Si hay varios valores en un encabezado determinado, observe que el Middleware de encabezados reenviados procesa los encabezados en orden inverso, de derecha a izquierda.

La dirección IP remota original de la solicitud debe coincidir con una entrada de las listas `KnownProxies` o `KnownNetworks` antes de procesar X-Forwarded-For. Esto limita la suplantación de encabezados al no aceptarse reenviadores de servidores proxy que no son de confianza.

## <a name="additional-resources"></a>Recursos adicionales

[Microsoft Security Advisory CVE-2018-0787: ASP.NET Core Elevation Of Privilege Vulnerability](https://github.com/aspnet/Announcements/issues/295) (Microsoft Security Advisory CVE-2018-0787: Vulnerabilidad de elevación de privilegios de ASP.NET Core)
