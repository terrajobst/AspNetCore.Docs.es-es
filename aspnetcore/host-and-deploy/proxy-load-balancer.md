---
title: Configurar ASP.NET Core para trabajar con servidores proxy y equilibradores de carga
author: guardrex
description: Obtenga información acerca de la configuración para las aplicaciones hospedadas detrás de servidores proxy y equilibradores de carga, que a menudo ocultan importante solicitan información.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/proxy-load-balancer
ms.openlocfilehash: b153a7406ae1b31a2aa453135c6bd0e5ce0b2997
ms.sourcegitcommit: d45d766504c2c5aad2453f01f089bc6b696b5576
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2018
---
# <a name="configure-aspnet-core-to-work-with-proxy-servers-and-load-balancers"></a>Configurar ASP.NET Core para trabajar con servidores proxy y equilibradores de carga

Por [Luke Latham](https://github.com/guardrex) y [Chris Ross](https://github.com/Tratcher)

En la configuración recomendada para ASP.NET Core, la aplicación se hospeda mediante módulo principal de IIS/ASP.NET, Nginx o Apache. Servidores proxy, equilibradores de carga y otros dispositivos de red a menudo interferir con información sobre la solicitud antes de llegar a la aplicación:

* Cuando las solicitudes HTTPS son procesadas por el proxy a través de HTTP, el esquema original (HTTPS) se pierden y se debe reenviar en un encabezado.
* Dado que una aplicación recibe una solicitud desde el servidor proxy y no su origen true en la red corporativa o de Internet, la dirección IP del cliente original también se debe reenviar en un encabezado.

Esta información puede ser importante en el procesamiento de solicitudes, por ejemplo en redirecciones, autenticación, generación de vínculo, evaluación de la directiva y geoloation de cliente.

## <a name="forwarded-headers"></a>Encabezados reenviados

Por convención, servidores proxy reenvían la información de encabezados HTTP.

| Header | Descripción |
| ------ | ----------- |
| X-Forwarded-For | Contiene información sobre el cliente que inició la solicitud y los servidores proxy posteriores en una cadena de servidores proxy. Este parámetro puede contener IP direcciones (y, opcionalmente, los números de puerto). En una cadena de servidores proxy, el primer parámetro indica al cliente que primero se realizó la solicitud. Los identificadores de proxy posteriores seguir. El proxy de último en la cadena no se encuentra en la lista de parámetros. Dirección IP del proxy última y, opcionalmente, un número de puerto, están disponibles como la dirección IP remota en el nivel de transporte. |
| Proto reenviados X | El valor del esquema de origen (HTTP/HTTPS). El valor también puede ser una lista de esquemas de si la solicitud ha pasado a varios servidores proxy. |
| X-Forwarded-Host | El valor original del campo de encabezado de Host. Por lo general, los servidores proxy no modifican el encabezado de Host. Vea [Microsoft Security Advisory CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) para obtener información sobre una vulnerabilidad de elevación de privilegios que afecta a los sistemas donde no valida el servidor proxy o encabezados de Host Restrict a valores correctos conocidos. |

El Middleware de encabezados reenviados desde el [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) empaquetar, lee estos encabezados y rellene los campos asociados en [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext). 

Las actualizaciones de software intermedio:

* [HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash; establecen a través de la `X-Forwarded-For` valor del encabezado. Opciones de configuración adicionales influyen en cómo se establece el middleware `RemoteIpAddress`. Para obtener más información, consulte el [opciones de Middleware de encabezados reenviados](#forwarded-headers-middleware-options).
* [HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash; establecen a través de la `X-Forwarded-Proto` valor del encabezado.
* [HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash; establecen a través de la `X-Forwarded-Host` valor del encabezado.

Tenga en cuenta que no todos los dispositivos de red agregue el `X-Forwarded-For` y `X-Forwarded-Proto` encabezados sin ninguna configuración adicional. Consulte las instrucciones del fabricante de su dispositivo si las solicitudes procesadas por el proxy no contienen estos encabezados cuando llegan a la aplicación.

Reenvía encabezados Middleware [configuración predeterminada](#forwarded-headers-middleware-options) pueden configurarse. La configuración predeterminada es:

* Sólo hay *un proxy* entre la aplicación y el origen de las solicitudes.
* Solo las direcciones de bucle invertido se configurado para servidores proxy conocidos y conocidas redes.

## <a name="iisiis-express-and-aspnet-core-module"></a>IIS o IIS Express y módulo principal de ASP.NET

Reenviado Middleware de encabezados está habilitada de forma predeterminada por el Middleware de integración de IIS cuando la aplicación se ejecuta detrás de IIS y el módulo principal de ASP.NET. Reenviado Middleware de encabezados está activado para ejecutarse primero en la canalización de middleware con una configuración restringida específica para el módulo de núcleo de ASP.NET debido a los problemas de confianza con encabezados reenviados (por ejemplo, [suplantación de direcciones IP](https://www.iplocation.net/ip-spoofing)). El middleware está configurado para reenviar el `X-Forwarded-For` y `X-Forwarded-Proto` encabezados y está restringido a un servidor proxy único localhost. Si se requiere configuración adicional, consulte el [opciones de Middleware de encabezados reenviados](#forwarded-headers-middleware-options).

## <a name="other-proxy-server-and-load-balancer-scenarios"></a>Otros escenarios de equilibrador de carga y el servidor proxy

Externa mediante Middleware de integración de IIS, Middleware de encabezados reenviados no está habilitada de forma predeterminada. Reenviado Middleware de encabezados debe estar habilitada para una aplicación para procesar reenviado encabezados con [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders). Después de habilitar el middleware si no hay ningún [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) se especifican para el software intermedio, el valor predeterminado [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) son [ForwardedHeaders.None ](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).

Configurar el middleware con [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) para reenviar el `X-Forwarded-For` y `X-Forwarded-Proto` encabezados en `Startup.ConfigureServices`. Invocar la [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) método `Startup.Configure` antes de llamar a otro middleware:

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
> Si no hay ningún [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) se especifican en `Startup.ConfigureServices` o directamente en el método de extensión con [UseForwardedHeaders (IApplicationBuilder, ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_), el valor predeterminado para reenviar los encabezados están [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). El [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) propiedad debe configurarse con los encabezados para reenviar.

## <a name="forwarded-headers-middleware-options"></a>Opciones de Middleware de encabezados de reenviados

[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) controlan el comportamiento del Middleware de encabezados reenvían de:

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardLimit = 2;
    options.KnownProxies.Add(IPAddress.Parse("127.0.10.1"));
    options.ForwardedForHeaderName = "X-Forwarded-For-Custom-Header-Name";
});
```

| Opción | Descripción |
| ------ | ----------- |
| [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | Usar el encabezado especificado por esta propiedad en lugar del especificado por [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername).<br><br>De manera predeterminada, es `X-Forwarded-For`. |
| [ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | Identifica qué reenviadores deben procesarse. Consulte la [ForwardedHeaders Enum](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) para obtener la lista de campos que se aplican. Los valores típicos que se asigna a esta propiedad son <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.<br><br>El valor predeterminado es [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). |
| [ForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | Usar el encabezado especificado por esta propiedad en lugar del especificado por [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername).<br><br>De manera predeterminada, es `X-Forwarded-Host`. |
| [ForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | Usar el encabezado especificado por esta propiedad en lugar del especificado por [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername).<br><br>De manera predeterminada, es `X-Forwarded-Proto`. |
| [ForwardLimit](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | Limita el número de entradas en los encabezados que se procesan. Establecido en `null` deshabilitar el límite, pero esto solo debe realizarse si `KnownProxies` o `KnownNetworks` están configurados.<br><br>El valor predeterminado es 1. |
| [KnownNetworks](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | Intervalos de los servidores proxy conocidos para aceptar encabezados reenviados desde la dirección. Proporcionar intervalos IP mediante notación de enrutamiento entre dominios sin clase (CIDR).<br><br>El valor predeterminado es una [IList](/dotnet/api/system.collections.generic.ilist-1)\<[red IP](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> que contiene una única entrada para `IPAddress.Loopback`. |
| [KnownProxies](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | Direcciones de los servidores proxy conocidos para aceptar encabezados reenviados desde. Use `KnownProxies` para especificar la dirección IP exacta coincidencias.<br><br>El valor predeterminado es una [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> que contiene una única entrada para `IPAddress.IPv6Loopback`. |
| [OriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | Usar el encabezado especificado por esta propiedad en lugar del especificado por [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).<br><br>De manera predeterminada, es `X-Original-For`. |
| [OriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | Usar el encabezado especificado por esta propiedad en lugar del especificado por [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).<br><br>De manera predeterminada, es `X-Original-Host`. |
| [OriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | Usar el encabezado especificado por esta propiedad en lugar del especificado por [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).<br><br>De manera predeterminada, es `X-Original-Proto`. |
| [RequireHeaderSymmetry](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | Requieren el número de valores de encabezado esté sincronizada entre el [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) está procesando.<br><br>El valor predeterminado en ASP.NET Core 1.x es `true`. El valor predeterminado de ASP.NET Core 2.0 o posterior es `false`. |

## <a name="scenarios-and-use-cases"></a>Escenarios y casos de uso

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a>Cuando no es posible agregar reenviado encabezados y todas las solicitudes son seguras

En algunos casos, no sería posible agregar encabezados reenviados a las solicitudes procesadas por el proxy a la aplicación. Si el servidor proxy es obligar a que todas las solicitudes externas públicas son HTTPS, el esquema se puede establecer manualmente en `Startup.Configure` antes de usar cualquier tipo de middleware:

```csharp
app.Use((context, next) =>
{
    context.Request.Scheme = "https";
    return next();
});
```

Este código puede deshabilitarse con una variable de entorno u otra configuración de configuración en un entorno de ensayo o de desarrollo.

### <a name="deal-with-path-base-and-proxies-that-change-the-request-path"></a>Tratar con la base de la ruta de acceso y los servidores proxy que cambie la ruta de acceso de solicitud

Algunos servidores proxy pasa intacta la ruta de acceso, pero con una aplicación de la ruta de acceso base que debe quitarse para que el enrutamiento funciona correctamente. [UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase) middleware divide la ruta de acceso en [HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) y la ruta de acceso base de aplicación en [HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase).

Si `/foo` es la ruta de acceso de base de aplicación para una ruta de acceso de proxy que se pasa como `/foo/api/1`, los conjuntos de middleware `Request.PathBase` a `/foo` y `Request.Path` a `/api/1` con el siguiente comando:

```csharp
app.UsePathBase("/foo");
```

La ruta de acceso y la ruta de acceso base original se vuelven a aplicar cuando se llama de nuevo el middleware en orden inverso. Para obtener más información sobre el procesamiento de pedidos de middleware, consulte [Middleware](xref:fundamentals/middleware/index).

Si el proxy recorta la ruta de acceso (por ejemplo, el reenvío `/foo/api/1` a `/api/1`), corrección redirige y vincula estableciendo la solicitud [PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase) propiedad:

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

Si el proxy está agregando datos de ruta de acceso, descarte parte de la ruta de acceso para corregir las redirecciones y vínculos mediante [StartsWithSegments (PathString, PathString)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__) y asignar a la [ruta de acceso](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) propiedad:

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

## <a name="troubleshoot"></a>Solucionar problemas

Cuando no se reenvían encabezados según lo esperado, habilitar [registro](xref:fundamentals/logging/index). Si los registros no proporcionan suficiente información para solucionar el problema, enumerar los encabezados de solicitud recibidos por el servidor. Los encabezados pueden escribirse en una respuesta de aplicación mediante middleware en línea:

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
    }
}
```

Asegúrese de que la X - reenviada-* encabezados son recibidos por el servidor con los valores esperados. Si hay varios valores en un encabezado determinado, tenga en cuenta los encabezados de procesos reenviados Middleware de encabezados en orden inverso de derecha a izquierda.

IP remotas original de la solicitud debe coincidir con una entrada en el `KnownProxies` o `KnownNetworks` se enumeran antes de X reenvían para procesar. Esto limita la suplantación de identidad de encabezado si no se aceptan los reenviadores de proxy no es de confianza.

## <a name="additional-resources"></a>Recursos adicionales

* [Microsoft Security Advisory CVE-2018-0787: ASP.NET Core vulnerabilidad de elevación de privilegios](https://github.com/aspnet/Announcements/issues/295)
