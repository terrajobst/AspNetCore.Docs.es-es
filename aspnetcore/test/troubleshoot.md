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
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="8ac40-103">Solucionar problemas de proyectos de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8ac40-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="8ac40-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8ac40-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8ac40-105">Los vínculos siguientes proporcionan instrucciones para la solución de problemas:</span><span class="sxs-lookup"><span data-stu-id="8ac40-105">The following links provide troubleshooting guidance:</span></span>

* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="8ac40-106">NDC Conference (Londres, 2018): Diagnóstico de problemas en aplicaciones ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8ac40-106">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="8ac40-107">Blog de ASP.NET: Solución de problemas de rendimiento de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8ac40-107">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="8ac40-108">SDK de .NET Core advertencias</span><span class="sxs-lookup"><span data-stu-id="8ac40-108">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="8ac40-109">Se instalan las versiones 32 y 64 bits de los SDK de .NET Core</span><span class="sxs-lookup"><span data-stu-id="8ac40-109">Both the 32-bit and 64-bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="8ac40-110">En el cuadro de diálogo **nuevo proyecto** de ASP.net Core, puede ver la siguiente ADVERTENCIA:</span><span class="sxs-lookup"><span data-stu-id="8ac40-110">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="8ac40-111">Se instalan las versiones de 32 y 64 bits de los SDK de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="8ac40-111">Both 32-bit and 64-bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="8ac40-112">Solo se muestran las plantillas de las versiones de 64 bits instaladas\\en '\\C:\\archivos de programa dotnet\\SDK '.</span><span class="sxs-lookup"><span data-stu-id="8ac40-112">Only templates from the 64-bit versions installed at 'C:\\Program Files\\dotnet\\sdk\\' are displayed.</span></span>

<span data-ttu-id="8ac40-113">Esta advertencia aparece cuando se instalan las versiones de 32 bits (x86) y 64 bits (x64) del [SDK de .net Core](https://www.microsoft.com/net/download/all) .</span><span class="sxs-lookup"><span data-stu-id="8ac40-113">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="8ac40-114">Los motivos comunes por los que se pueden instalar ambas versiones son:</span><span class="sxs-lookup"><span data-stu-id="8ac40-114">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="8ac40-115">Originalmente descargó el instalador de SDK de .NET Core mediante un equipo de 32 bits, pero lo copió y lo instaló en un equipo de 64 bits.</span><span class="sxs-lookup"><span data-stu-id="8ac40-115">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="8ac40-116">Otra aplicación instaló el SDK de .NET Core de 32 bits.</span><span class="sxs-lookup"><span data-stu-id="8ac40-116">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="8ac40-117">Se descargó e instaló la versión incorrecta.</span><span class="sxs-lookup"><span data-stu-id="8ac40-117">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="8ac40-118">Desinstale el SDK de .NET Core de 32 bits para evitar esta advertencia.</span><span class="sxs-lookup"><span data-stu-id="8ac40-118">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="8ac40-119">Desinstalar en **el panel** > de control**programas y características** > **desinstalar o cambiar un programa**.</span><span class="sxs-lookup"><span data-stu-id="8ac40-119">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="8ac40-120">Si comprende por qué se produce la advertencia y sus implicaciones, puede omitir la advertencia.</span><span class="sxs-lookup"><span data-stu-id="8ac40-120">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="8ac40-121">El SDK de .NET Core se instala en varias ubicaciones</span><span class="sxs-lookup"><span data-stu-id="8ac40-121">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="8ac40-122">En el cuadro de diálogo **nuevo proyecto** de ASP.net Core, puede ver la siguiente ADVERTENCIA:</span><span class="sxs-lookup"><span data-stu-id="8ac40-122">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="8ac40-123">El SDK de .NET Core se instala en varias ubicaciones.</span><span class="sxs-lookup"><span data-stu-id="8ac40-123">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="8ac40-124">Solo se muestran las plantillas de los SDK instalados en\\' C\\:\\archivos\\de programa dotnet SDK '.</span><span class="sxs-lookup"><span data-stu-id="8ac40-124">Only templates from the SDKs installed at 'C:\\Program Files\\dotnet\\sdk\\' are displayed.</span></span>

<span data-ttu-id="8ac40-125">Verá este mensaje si tiene al menos una instalación del SDK de .net Core en un directorio fuera de *C:\\archivos\\de\\programa dotnet SDK\\* .</span><span class="sxs-lookup"><span data-stu-id="8ac40-125">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="8ac40-126">Normalmente esto sucede cuando el SDK de .NET Core se ha implementado en una máquina mediante copiar y pegar en lugar de con el instalador de MSI.</span><span class="sxs-lookup"><span data-stu-id="8ac40-126">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="8ac40-127">Desinstale todos los SDK de .NET Core de 32 bits y los tiempos de ejecución para evitar esta advertencia.</span><span class="sxs-lookup"><span data-stu-id="8ac40-127">Uninstall all 32-bit .NET Core SDKs and runtimes to prevent this warning.</span></span> <span data-ttu-id="8ac40-128">Desinstalar en **el panel** > de control**programas y características** > **desinstalar o cambiar un programa**.</span><span class="sxs-lookup"><span data-stu-id="8ac40-128">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="8ac40-129">Si comprende por qué se produce la advertencia y sus implicaciones, puede omitir la advertencia.</span><span class="sxs-lookup"><span data-stu-id="8ac40-129">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="8ac40-130">No se detectó ningún SDK de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="8ac40-130">No .NET Core SDKs were detected</span></span>

* <span data-ttu-id="8ac40-131">En el cuadro de diálogo **nuevo proyecto** de Visual Studio para ASP.net Core, es posible que vea la siguiente ADVERTENCIA:</span><span class="sxs-lookup"><span data-stu-id="8ac40-131">In the Visual Studio **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

  > <span data-ttu-id="8ac40-132">No se detectó ningún SDK de .NET Core, asegúrese de que se incluyen `PATH`en la variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="8ac40-132">No .NET Core SDKs were detected, ensure they are included in the environment variable `PATH`.</span></span>

* <span data-ttu-id="8ac40-133">Al ejecutar un `dotnet` comando, la advertencia aparece como:</span><span class="sxs-lookup"><span data-stu-id="8ac40-133">When executing a `dotnet` command, the warning appears as:</span></span>

  > <span data-ttu-id="8ac40-134">No es posible encontrar ningún SDK de dotnet instalado.</span><span class="sxs-lookup"><span data-stu-id="8ac40-134">It was not possible to find any installed dotnet SDKs.</span></span>

<span data-ttu-id="8ac40-135">Estas advertencias aparecen cuando la variable `PATH` de entorno no apunta a ningún SDK de .net Core en la máquina.</span><span class="sxs-lookup"><span data-stu-id="8ac40-135">These warnings appear when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="8ac40-136">Para resolver este problema:</span><span class="sxs-lookup"><span data-stu-id="8ac40-136">To resolve this problem:</span></span>

* <span data-ttu-id="8ac40-137">Instale el SDK de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="8ac40-137">Install the .NET Core SDK.</span></span> <span data-ttu-id="8ac40-138">Obtenga el instalador más reciente de las [descargas de .net](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="8ac40-138">Obtain the latest installer from [.NET Downloads](https://dotnet.microsoft.com/download).</span></span>
* <span data-ttu-id="8ac40-139">Compruebe que la `PATH` variable de entorno apunta a la ubicación donde está instalado el SDK`C:\Program Files\dotnet\` (para 64 bits/x64 o `C:\Program Files (x86)\dotnet\` para 32 bits/x86).</span><span class="sxs-lookup"><span data-stu-id="8ac40-139">Verify that the `PATH` environment variable points to the location where the SDK is installed (`C:\Program Files\dotnet\` for 64-bit/x64 or `C:\Program Files (x86)\dotnet\` for 32-bit/x86).</span></span> <span data-ttu-id="8ac40-140">Normalmente, el instalador de SDK `PATH`establece el.</span><span class="sxs-lookup"><span data-stu-id="8ac40-140">The SDK installer normally sets the `PATH`.</span></span> <span data-ttu-id="8ac40-141">Instale siempre los mismos SDK de bits y tiempos de ejecución en el mismo equipo.</span><span class="sxs-lookup"><span data-stu-id="8ac40-141">Always install the same bitness SDKs and runtimes on the same machine.</span></span>

### <a name="missing-sdk-after-installing-the-net-core-hosting-bundle"></a><span data-ttu-id="8ac40-142">Falta el SDK después de instalar el lote de hospedaje de .NET Core</span><span class="sxs-lookup"><span data-stu-id="8ac40-142">Missing SDK after installing the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="8ac40-143">Al instalar el [lote de hospedaje de .net Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) , se modifica el `PATH` cuando se instala el tiempo de ejecución de .net Core para que apunte a la versión de 32 bits (x86) de .net Core (`C:\Program Files (x86)\dotnet\`).</span><span class="sxs-lookup"><span data-stu-id="8ac40-143">Installing the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) modifies the `PATH` when it installs the .NET Core runtime to point to the 32-bit (x86) version of .NET Core (`C:\Program Files (x86)\dotnet\`).</span></span> <span data-ttu-id="8ac40-144">Esto puede dar lugar a que falten SDK cuando se usa el comando .net Core `dotnet` de 32 bits (x86) ([no se detectó ningún SDK de .net Core](#no-net-core-sdks-were-detected)).</span><span class="sxs-lookup"><span data-stu-id="8ac40-144">This can result in missing SDKs when the 32-bit (x86) .NET Core `dotnet` command is used ([No .NET Core SDKs were detected](#no-net-core-sdks-were-detected)).</span></span> <span data-ttu-id="8ac40-145">Para resolver este problema, desplácese `C:\Program Files\dotnet\` a una posición anterior `C:\Program Files (x86)\dotnet\` a `PATH`la.</span><span class="sxs-lookup"><span data-stu-id="8ac40-145">To resolve this problem, move `C:\Program Files\dotnet\` to a position before `C:\Program Files (x86)\dotnet\` on the `PATH`.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="8ac40-146">Obtención de datos de una aplicación</span><span class="sxs-lookup"><span data-stu-id="8ac40-146">Obtain data from an app</span></span>

<span data-ttu-id="8ac40-147">Si una aplicación es capaz de responder a las solicitudes, puede obtener los siguientes datos de la aplicación mediante middleware:</span><span class="sxs-lookup"><span data-stu-id="8ac40-147">If an app is capable of responding to requests, you can obtain the following data from the app using middleware:</span></span>

* <span data-ttu-id="8ac40-148">Método &ndash; de solicitud, esquema, host, pathbase, ruta de acceso, cadena de consulta, encabezados</span><span class="sxs-lookup"><span data-stu-id="8ac40-148">Request &ndash; Method, scheme, host, pathbase, path, query string, headers</span></span>
* <span data-ttu-id="8ac40-149">Dirección &ndash; IP remota de conexión, Puerto remoto, dirección IP local, puerto local, certificado de cliente</span><span class="sxs-lookup"><span data-stu-id="8ac40-149">Connection &ndash; Remote IP address, remote port, local IP address, local port, client certificate</span></span>
* <span data-ttu-id="8ac40-150">Nombre &ndash; de identidad, nombre para mostrar</span><span class="sxs-lookup"><span data-stu-id="8ac40-150">Identity &ndash; Name, display name</span></span>
* <span data-ttu-id="8ac40-151">Valores de configuración</span><span class="sxs-lookup"><span data-stu-id="8ac40-151">Configuration settings</span></span>
* <span data-ttu-id="8ac40-152">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="8ac40-152">Environment variables</span></span>

<span data-ttu-id="8ac40-153">Coloque el siguiente código de [middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) al principio de la `Startup.Configure` canalización de procesamiento de solicitudes del método.</span><span class="sxs-lookup"><span data-stu-id="8ac40-153">Place the following [middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) code at the beginning of the `Startup.Configure` method's request processing pipeline.</span></span> <span data-ttu-id="8ac40-154">El entorno se comprueba antes de que se ejecute el middleware para asegurarse de que el código solo se ejecuta en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="8ac40-154">The environment is checked before the middleware is run to ensure that the code is only executed in the Development environment.</span></span>

<span data-ttu-id="8ac40-155">Para obtener el entorno, use cualquiera de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="8ac40-155">To obtain the environment, use either of the following approaches:</span></span>

* <span data-ttu-id="8ac40-156">Inserte en el `Startup.Configure` método y compruebe el entorno con la variable local. `IHostingEnvironment`</span><span class="sxs-lookup"><span data-stu-id="8ac40-156">Inject the `IHostingEnvironment` into the `Startup.Configure` method and check the environment with the local variable.</span></span> <span data-ttu-id="8ac40-157">En el código de ejemplo siguiente se muestra este enfoque.</span><span class="sxs-lookup"><span data-stu-id="8ac40-157">The following sample code demonstrates this approach.</span></span>

* <span data-ttu-id="8ac40-158">Asigne el entorno a una propiedad en la `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="8ac40-158">Assign the environment to a property in the `Startup` class.</span></span> <span data-ttu-id="8ac40-159">Compruebe el entorno con la propiedad (por ejemplo, `if (Environment.IsDevelopment())`).</span><span class="sxs-lookup"><span data-stu-id="8ac40-159">Check the environment using the property (for example, `if (Environment.IsDevelopment())`).</span></span>

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
