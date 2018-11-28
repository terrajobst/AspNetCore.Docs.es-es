---
title: Solución de problemas de proyectos de ASP.NET Core
author: Rick-Anderson
description: Conozca y solucione advertencias y errores en proyectos de ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/26/2018
uid: test/troubleshoot
ms.openlocfilehash: 7a3361970bde2b8761c76884fc1905957d075c5c
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450780"
---
# <a name="troubleshoot-aspnet-core-projects"></a>Solución de problemas de proyectos de ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Los vínculos siguientes proporcionan orientación para la solución:

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [Conferencia NDC (London, 2018): Diagnóstico de problemas en aplicaciones ASP.NET Core](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [Blog de ASP.NET: Solucionar problemas de rendimiento de ASP.NET Core](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a>Advertencias de SDK de .NET core

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>Se instalan las versiones de 64 bits del SDK de .NET Core y 32 bits

En el **nuevo proyecto** cuadro de diálogo para ASP.NET Core, es posible que vea la advertencia siguiente:

> Se instalan las versiones de 32 y 64 bits del SDK de .NET Core. Sólo las plantillas de las versiones de 64 bits instaladas en ' C:\\archivos de programa\\dotnet\\sdk\\' se mostrará.

![Captura de pantalla del cuadro de diálogo OneASP.NET que muestra el mensaje de advertencia](troubleshoot/_static/both32and64bit.png)

Esta advertencia aparece cuando (x86) 32 bits y las versiones de 64 bits (x 64) de la [SDK de .NET Core](https://www.microsoft.com/net/download/all) están instalados. Causas comunes que se pueden instalar ambas versiones se incluyen:

* Descargar al instalador del SDK de .NET Core con un equipo de 32 bits pero, a continuación, copiarla en y originalmente había instalado en un equipo de 64 bits.
* Se instaló el SDK de .NET Core de 32 bits por otra aplicación.
* Descargado e instalada la versión incorrecta.

Desinstalar el SDK de .NET Core de 32 bits para evitar esta advertencia. Desinstalar desde **Panel de Control** > **programas y características** > **desinstalar o cambiar un programa**. Si entiende por qué se produce la advertencia y sus implicaciones, puede omitir la advertencia.

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>El SDK de .NET Core está instalado en varias ubicaciones

En el **nuevo proyecto** cuadro de diálogo para ASP.NET Core, es posible que vea la advertencia siguiente:

> El SDK de .NET Core está instalado en varias ubicaciones. Sólo las plantillas de los SDK instalados en ' C:\\archivos de programa\\dotnet\\sdk\\' se mostrará.

![Captura de pantalla del cuadro de diálogo OneASP.NET que muestra el mensaje de advertencia](troubleshoot/_static/multiplelocations.png)

Vea este mensaje cuando haya al menos una instalación de SDK de .NET Core en un directorio fuera de *C:\\archivos de programa\\dotnet\\sdk\\*. Normalmente esto sucede cuando el SDK de .NET Core se ha implementado en un equipo con copiar y pegar en lugar del instalador MSI.

Desinstalar el SDK de .NET Core de 32 bits para evitar esta advertencia. Desinstalar desde **Panel de Control** > **programas y características** > **desinstalar o cambiar un programa**. Si entiende por qué se produce la advertencia y sus implicaciones, puede omitir la advertencia.

### <a name="no-net-core-sdks-were-detected"></a>Se han detectado ningún SDK de .NET Core

En el **nuevo proyecto** cuadro de diálogo para ASP.NET Core, es posible que vea la advertencia siguiente:

> Se han detectado ningún SDK de .NET Core, asegúrese de que se incluyen en la variable de entorno 'PATH'.

![Captura de pantalla del cuadro de diálogo OneASP.NET que muestra el mensaje de advertencia](troubleshoot/_static/NoNetCore.png)

Esta advertencia aparece cuando la variable de entorno `PATH` no apunta a ningún SDK de .NET Core en el equipo. Para resolver este problema:

* Instalar o comprobar que está instalado el SDK de .NET Core.
* Compruebe que el `PATH` variable de entorno se apunta a la ubicación donde está instalado el SDK. El instalador se establece normalmente el `PATH`.

## <a name="obtain-data-from-an-app"></a>Obtener datos desde una aplicación

Si una aplicación es capaz de responder a las solicitudes, puede obtener los siguientes datos de la aplicación con middleware:

* Solicitar &ndash; método, esquema, host, pathbase, ruta de acceso, cadena de consulta, encabezados
* Conexión &ndash; dirección IP remota, puerto remoto, dirección IP local, puerto local, el certificado de cliente
* Identidad &ndash; nombre, nombre para mostrar
* Valores de configuración
* Variables de entorno

Incluya las líneas siguientes [middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) código al principio de la `Startup.Configure` canalización de procesamiento de solicitudes del método. El entorno se comprueba antes de ejecuta el software intermedio para asegurarse de que el código solo se ejecuta en el entorno de desarrollo.

Para obtener el entorno, use cualquiera de los métodos siguientes:

* Insertar el `IHostingEnvironment` en el `Startup.Configure` método y comprobar el entorno con la variable local. El código de ejemplo siguiente muestra este enfoque.

* Asignar el entorno a una propiedad en el `Startup` clase. Comprobar el entorno mediante la propiedad (por ejemplo, `if (Environment.IsDevelopment())`).

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, 
    IConfiguration config)
{
    if (env.IsDevelopment())
    {
        app.Run(async (context) =>
        {
            var sb = new StringBuilder();
            var nl = System.Environment.NewLine;
            var rule = string.Concat(nl, new string('-', 40), nl);
            var authSchemeProvider = app.ApplicationServices
                .GetRequiredService<IAuthenticationSchemeProvider>();

            sb.Append($"Request{rule}");
            sb.Append($"{DateTimeOffset.Now}{nl}");
            sb.Append($"{context.Request.Method} {context.Request.Path}{nl}");
            sb.Append($"Scheme: {context.Request.Scheme}{nl}");
            sb.Append($"Host: {context.Request.Headers["Host"]}{nl}");
            sb.Append($"PathBase: {context.Request.PathBase.Value}{nl}");
            sb.Append($"Path: {context.Request.Path.Value}{nl}");
            sb.Append($"Query: {context.Request.QueryString.Value}{nl}{nl}");

            sb.Append($"Connection{rule}");
            sb.Append($"RemoteIp: {context.Connection.RemoteIpAddress}{nl}");
            sb.Append($"RemotePort: {context.Connection.RemotePort}{nl}");
            sb.Append($"LocalIp: {context.Connection.LocalIpAddress}{nl}");
            sb.Append($"LocalPort: {context.Connection.LocalPort}{nl}");
            sb.Append($"ClientCert: {context.Connection.ClientCertificate}{nl}{nl}");

            sb.Append($"Identity{rule}");
            sb.Append($"User: {context.User.Identity.Name}{nl}");
            var scheme = await authSchemeProvider
                .GetSchemeAsync(IISDefaults.AuthenticationScheme);
            sb.Append($"DisplayName: {scheme?.DisplayName}{nl}{nl}");

            sb.Append($"Headers{rule}");
            foreach (var header in context.Request.Headers)
            {
                sb.Append($"{header.Key}: {header.Value}{nl}");
            }
            sb.Append(nl);

            sb.Append($"Websockets{rule}");
            if (context.Features.Get<IHttpUpgradeFeature>() != null)
            {
                sb.Append($"Status: Enabled{nl}{nl}");
            }
            else
            {
                sb.Append($"Status: Disabled{nl}{nl}");
            }

            sb.Append($"Configuration{rule}");
            foreach (var pair in config.AsEnumerable())
            {
                sb.Append($"{pair.Path}: {pair.Value}{nl}");
            }
            sb.Append(nl);

            sb.Append($"Environment Variables{rule}");
            var vars = System.Environment.GetEnvironmentVariables();
            foreach (var key in vars.Keys.Cast<string>().OrderBy(key => key, 
                StringComparer.OrdinalIgnoreCase))
            {
                var value = vars[key];
                sb.Append($"{key}: {value}{nl}");
            }

            context.Response.ContentType = "text/plain";
            await context.Response.WriteAsync(sb.ToString());
        });
    }
}
```
