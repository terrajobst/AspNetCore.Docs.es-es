---
title: Solucionar problemas de proyectos de ASP.NET Core
author: Rick-Anderson
description: Conozca y solucione advertencias y errores en proyectos de ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/10/2019
uid: test/troubleshoot
ms.openlocfilehash: b434af2dd046045836d2f6f7f7b7b2d57699bedc
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308281"
---
# <a name="troubleshoot-aspnet-core-projects"></a>Solucionar problemas de proyectos de ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Los vínculos siguientes proporcionan instrucciones para la solución de problemas:

* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [NDC Conference (Londres, 2018): Diagnóstico de problemas en aplicaciones ASP.NET Core](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [Blog de ASP.NET: Solución de problemas de rendimiento de ASP.NET Core](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a>SDK de .NET Core advertencias

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>Se instalan las versiones 32 y 64 bits de los SDK de .NET Core

En el cuadro de diálogo **nuevo proyecto** de ASP.net Core, puede ver la siguiente ADVERTENCIA:

> Se instalan las versiones de 32 y 64 bits de los SDK de .NET Core. Solo se muestran las plantillas de las versiones de 64 bits instaladas\\en '\\C:\\archivos de programa dotnet\\SDK '.

Esta advertencia aparece cuando se instalan las versiones de 32 bits (x86) y 64 bits (x64) del [SDK de .net Core](https://www.microsoft.com/net/download/all) . Los motivos comunes por los que se pueden instalar ambas versiones son:

* Originalmente descargó el instalador de SDK de .NET Core mediante un equipo de 32 bits, pero lo copió y lo instaló en un equipo de 64 bits.
* Otra aplicación instaló el SDK de .NET Core de 32 bits.
* Se descargó e instaló la versión incorrecta.

Desinstale el SDK de .NET Core de 32 bits para evitar esta advertencia. Desinstalar en **el panel** > de control**programas y características** > **desinstalar o cambiar un programa**. Si comprende por qué se produce la advertencia y sus implicaciones, puede omitir la advertencia.

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>El SDK de .NET Core se instala en varias ubicaciones

En el cuadro de diálogo **nuevo proyecto** de ASP.net Core, puede ver la siguiente ADVERTENCIA:

> El SDK de .NET Core se instala en varias ubicaciones. Solo se muestran las plantillas de los SDK instalados en\\' C\\:\\archivos\\de programa dotnet SDK '.

Verá este mensaje si tiene al menos una instalación del SDK de .net Core en un directorio fuera de *C:\\archivos\\de\\programa dotnet SDK\\* . Normalmente esto sucede cuando el SDK de .NET Core se ha implementado en una máquina mediante copiar y pegar en lugar de con el instalador de MSI.

Desinstale todos los SDK de .NET Core de 32 bits y los tiempos de ejecución para evitar esta advertencia. Desinstalar en **el panel** > de control**programas y características** > **desinstalar o cambiar un programa**. Si comprende por qué se produce la advertencia y sus implicaciones, puede omitir la advertencia.

### <a name="no-net-core-sdks-were-detected"></a>No se detectó ningún SDK de .NET Core.

* En el cuadro de diálogo **nuevo proyecto** de Visual Studio para ASP.net Core, es posible que vea la siguiente ADVERTENCIA:

  > No se detectó ningún SDK de .NET Core, asegúrese de que se incluyen `PATH`en la variable de entorno.

* Al ejecutar un `dotnet` comando, la advertencia aparece como:

  > No es posible encontrar ningún SDK de dotnet instalado.

Estas advertencias aparecen cuando la variable `PATH` de entorno no apunta a ningún SDK de .net Core en la máquina. Para resolver este problema:

* Instale el SDK de .NET Core. Obtenga el instalador más reciente de las [descargas de .net](https://dotnet.microsoft.com/download).
* Compruebe que la `PATH` variable de entorno apunta a la ubicación donde está instalado el SDK`C:\Program Files\dotnet\` (para 64 bits/x64 o `C:\Program Files (x86)\dotnet\` para 32 bits/x86). Normalmente, el instalador de SDK `PATH`establece el. Instale siempre los mismos SDK de bits y tiempos de ejecución en el mismo equipo.

### <a name="missing-sdk-after-installing-the-net-core-hosting-bundle"></a>Falta el SDK después de instalar el lote de hospedaje de .NET Core

Al instalar el [lote de hospedaje de .net Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) , se modifica el `PATH` cuando se instala el tiempo de ejecución de .net Core para que apunte a la versión de 32 bits (x86) de .net Core (`C:\Program Files (x86)\dotnet\`). Esto puede dar lugar a que falten SDK cuando se usa el comando .net Core `dotnet` de 32 bits (x86) ([no se detectó ningún SDK de .net Core](#no-net-core-sdks-were-detected)). Para resolver este problema, desplácese `C:\Program Files\dotnet\` a una posición anterior `C:\Program Files (x86)\dotnet\` a `PATH`la.

## <a name="obtain-data-from-an-app"></a>Obtención de datos de una aplicación

Si una aplicación es capaz de responder a las solicitudes, puede obtener los siguientes datos de la aplicación mediante middleware:

* Método &ndash; de solicitud, esquema, host, pathbase, ruta de acceso, cadena de consulta, encabezados
* Dirección &ndash; IP remota de conexión, Puerto remoto, dirección IP local, puerto local, certificado de cliente
* Nombre &ndash; de identidad, nombre para mostrar
* Valores de configuración
* Variables de entorno

Coloque el siguiente código de [middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) al principio de la `Startup.Configure` canalización de procesamiento de solicitudes del método. El entorno se comprueba antes de que se ejecute el middleware para asegurarse de que el código solo se ejecuta en el entorno de desarrollo.

Para obtener el entorno, use cualquiera de los métodos siguientes:

* Inserte en el `Startup.Configure` método y compruebe el entorno con la variable local. `IHostingEnvironment` En el código de ejemplo siguiente se muestra este enfoque.

* Asigne el entorno a una propiedad en la `Startup` clase. Compruebe el entorno con la propiedad (por ejemplo, `if (Environment.IsDevelopment())`).

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
