---
title: Solución de problemas de proyectos de ASP.NET Core
author: Rick-Anderson
description: Conozca y solucione advertencias y errores en proyectos de ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2019
uid: test/troubleshoot
ms.openlocfilehash: 3d755b2f0c509d65dea86bbe719e42935d87d546
ms.sourcegitcommit: 687ffb15ebe65379f75c84739ea851d5a0d788b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58488746"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="56534-103">Solución de problemas de proyectos de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56534-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="56534-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="56534-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="56534-105">Los vínculos siguientes proporcionan orientación para la solución:</span><span class="sxs-lookup"><span data-stu-id="56534-105">The following links provide troubleshooting guidance:</span></span>

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="56534-106">Conferencia NDC (London, 2018): Diagnosticar problemas en aplicaciones ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56534-106">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="56534-107">Blog de ASP.NET: Solucionar problemas de rendimiento de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56534-107">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="56534-108">Advertencias de SDK de .NET core</span><span class="sxs-lookup"><span data-stu-id="56534-108">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="56534-109">Se instalan las versiones de 64 bits del SDK de .NET Core y 32 bits</span><span class="sxs-lookup"><span data-stu-id="56534-109">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="56534-110">En el **nuevo proyecto** cuadro de diálogo para ASP.NET Core, es posible que vea la advertencia siguiente:</span><span class="sxs-lookup"><span data-stu-id="56534-110">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="56534-111">Se instalan las versiones de 32 y 64 bits del SDK de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="56534-111">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="56534-112">Sólo las plantillas de las versiones de 64 bits instaladas en ' C:\\archivos de programa\\dotnet\\sdk\\' se mostrará.</span><span class="sxs-lookup"><span data-stu-id="56534-112">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

<span data-ttu-id="56534-113">Esta advertencia aparece cuando (x86) 32 bits y las versiones de 64 bits (x 64) de la [SDK de .NET Core](https://www.microsoft.com/net/download/all) están instalados.</span><span class="sxs-lookup"><span data-stu-id="56534-113">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="56534-114">Causas comunes que se pueden instalar ambas versiones se incluyen:</span><span class="sxs-lookup"><span data-stu-id="56534-114">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="56534-115">Descargar al instalador del SDK de .NET Core con un equipo de 32 bits pero, a continuación, copiarla en y originalmente había instalado en un equipo de 64 bits.</span><span class="sxs-lookup"><span data-stu-id="56534-115">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="56534-116">Se instaló el SDK de .NET Core de 32 bits por otra aplicación.</span><span class="sxs-lookup"><span data-stu-id="56534-116">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="56534-117">Descargado e instalada la versión incorrecta.</span><span class="sxs-lookup"><span data-stu-id="56534-117">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="56534-118">Desinstalar el SDK de .NET Core de 32 bits para evitar esta advertencia.</span><span class="sxs-lookup"><span data-stu-id="56534-118">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="56534-119">Desinstalar desde **Panel de Control** > **programas y características** > **desinstalar o cambiar un programa**.</span><span class="sxs-lookup"><span data-stu-id="56534-119">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="56534-120">Si entiende por qué se produce la advertencia y sus implicaciones, puede omitir la advertencia.</span><span class="sxs-lookup"><span data-stu-id="56534-120">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="56534-121">El SDK de .NET Core está instalado en varias ubicaciones</span><span class="sxs-lookup"><span data-stu-id="56534-121">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="56534-122">En el **nuevo proyecto** cuadro de diálogo para ASP.NET Core, es posible que vea la advertencia siguiente:</span><span class="sxs-lookup"><span data-stu-id="56534-122">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="56534-123">El SDK de .NET Core está instalado en varias ubicaciones.</span><span class="sxs-lookup"><span data-stu-id="56534-123">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="56534-124">Sólo las plantillas de los SDK instalados en ' C:\\archivos de programa\\dotnet\\sdk\\' se mostrará.</span><span class="sxs-lookup"><span data-stu-id="56534-124">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

<span data-ttu-id="56534-125">Vea este mensaje cuando haya al menos una instalación de SDK de .NET Core en un directorio fuera de *C:\\archivos de programa\\dotnet\\sdk\\*.</span><span class="sxs-lookup"><span data-stu-id="56534-125">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="56534-126">Normalmente esto sucede cuando el SDK de .NET Core se ha implementado en un equipo con copiar y pegar en lugar del instalador MSI.</span><span class="sxs-lookup"><span data-stu-id="56534-126">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="56534-127">Desinstale todos los 32-bit SDK de .NET Core y los tiempos de ejecución para evitar esta advertencia.</span><span class="sxs-lookup"><span data-stu-id="56534-127">Uninstall all 32-bit .NET Core SDKs and runtimes to prevent this warning.</span></span> <span data-ttu-id="56534-128">Desinstalar desde **Panel de Control** > **programas y características** > **desinstalar o cambiar un programa**.</span><span class="sxs-lookup"><span data-stu-id="56534-128">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="56534-129">Si entiende por qué se produce la advertencia y sus implicaciones, puede omitir la advertencia.</span><span class="sxs-lookup"><span data-stu-id="56534-129">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="56534-130">Se han detectado ningún SDK de .NET Core</span><span class="sxs-lookup"><span data-stu-id="56534-130">No .NET Core SDKs were detected</span></span>

* <span data-ttu-id="56534-131">En Visual Studio **nuevo proyecto** cuadro de diálogo para ASP.NET Core, es posible que vea la advertencia siguiente:</span><span class="sxs-lookup"><span data-stu-id="56534-131">In the Visual Studio **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

  > <span data-ttu-id="56534-132">Se han detectado ningún SDK de .NET Core, asegúrese de que se incluyen en la variable de entorno `PATH`.</span><span class="sxs-lookup"><span data-stu-id="56534-132">No .NET Core SDKs were detected, ensure they are included in the environment variable `PATH`.</span></span>

* <span data-ttu-id="56534-133">Cuando se ejecuta un `dotnet` comando, aparece la advertencia como:</span><span class="sxs-lookup"><span data-stu-id="56534-133">When executing a `dotnet` command, the warning appears as:</span></span>

  > <span data-ttu-id="56534-134">No era posible encontrar cualquier SDK de dotnet instalado.</span><span class="sxs-lookup"><span data-stu-id="56534-134">It was not possible to find any installed dotnet SDKs.</span></span>

<span data-ttu-id="56534-135">Estas advertencias aparecen cuando la variable de entorno `PATH` no apunta a ningún SDK de .NET Core en el equipo.</span><span class="sxs-lookup"><span data-stu-id="56534-135">These warnings appear when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="56534-136">Para resolver este problema:</span><span class="sxs-lookup"><span data-stu-id="56534-136">To resolve this problem:</span></span>

* <span data-ttu-id="56534-137">Instale el SDK de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="56534-137">Install the .NET Core SDK.</span></span> <span data-ttu-id="56534-138">Obtener el instalador más reciente de [descargas de .NET](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="56534-138">Obtain the latest installer from [.NET Downloads](https://dotnet.microsoft.com/download).</span></span>
* <span data-ttu-id="56534-139">Compruebe que la `PATH` variable de entorno se apunta a la ubicación donde está instalado el SDK (`C:\Program Files\dotnet\` para 64-bit/x64 o `C:\Program Files (x86)\dotnet\` para 32-bit/x86).</span><span class="sxs-lookup"><span data-stu-id="56534-139">Verify that the `PATH` environment variable points to the location where the SDK is installed (`C:\Program Files\dotnet\` for 64-bit/x64 or `C:\Program Files (x86)\dotnet\` for 32-bit/x86).</span></span> <span data-ttu-id="56534-140">El instalador del SDK se establece normalmente el `PATH`.</span><span class="sxs-lookup"><span data-stu-id="56534-140">The SDK installer normally sets the `PATH`.</span></span> <span data-ttu-id="56534-141">Instale siempre el mismo valor de bits SDK y los tiempos de ejecución en el mismo equipo.</span><span class="sxs-lookup"><span data-stu-id="56534-141">Always install the same bitness SDKs and runtimes on the same machine.</span></span>

### <a name="missing-sdk-after-installing-the-net-core-hosting-bundle"></a><span data-ttu-id="56534-142">Faltan SDK después de instalar el lote de hospedaje de .NET Core</span><span class="sxs-lookup"><span data-stu-id="56534-142">Missing SDK after installing the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="56534-143">Instalar el [conjunto de hospedaje de .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) modifica el `PATH` cuando instala el tiempo de ejecución de .NET Core para que apunte a la versión de 32 bits (x 86) de .NET Core (`C:\Program Files (x86)\dotnet\`).</span><span class="sxs-lookup"><span data-stu-id="56534-143">Installing the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) modifies the `PATH` when it installs the .NET Core runtime to point to the 32-bit (x86) version of .NET Core (`C:\Program Files (x86)\dotnet\`).</span></span> <span data-ttu-id="56534-144">Esto puede provocar que faltan SDK cuando el núcleo de .NET de 32 bits (x 86) `dotnet` se usa el comando ([ningún SDK de .NET Core se detectaron](#no-net-core-sdks-were-detected)).</span><span class="sxs-lookup"><span data-stu-id="56534-144">This can result in missing SDKs when the 32-bit (x86) .NET Core `dotnet` command is used ([No .NET Core SDKs were detected](#no-net-core-sdks-were-detected)).</span></span> <span data-ttu-id="56534-145">Para resolver este problema, mueva `C:\Program Files\dotnet\` a una posición delante `C:\Program Files (x86)\dotnet\` en el `PATH`.</span><span class="sxs-lookup"><span data-stu-id="56534-145">To resolve this problem, move `C:\Program Files\dotnet\` to a position before `C:\Program Files (x86)\dotnet\` on the `PATH`.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="56534-146">Obtención de datos de una aplicación</span><span class="sxs-lookup"><span data-stu-id="56534-146">Obtain data from an app</span></span>

<span data-ttu-id="56534-147">Si una aplicación es capaz de responder a las solicitudes, puede obtener los siguientes datos de la aplicación con middleware:</span><span class="sxs-lookup"><span data-stu-id="56534-147">If an app is capable of responding to requests, you can obtain the following data from the app using middleware:</span></span>

* <span data-ttu-id="56534-148">Solicitar &ndash; método, esquema, host, pathbase, ruta de acceso, cadena de consulta, encabezados</span><span class="sxs-lookup"><span data-stu-id="56534-148">Request &ndash; Method, scheme, host, pathbase, path, query string, headers</span></span>
* <span data-ttu-id="56534-149">Conexión &ndash; dirección IP remota, puerto remoto, dirección IP local, puerto local, el certificado de cliente</span><span class="sxs-lookup"><span data-stu-id="56534-149">Connection &ndash; Remote IP address, remote port, local IP address, local port, client certificate</span></span>
* <span data-ttu-id="56534-150">Identidad &ndash; nombre, nombre para mostrar</span><span class="sxs-lookup"><span data-stu-id="56534-150">Identity &ndash; Name, display name</span></span>
* <span data-ttu-id="56534-151">Valores de configuración</span><span class="sxs-lookup"><span data-stu-id="56534-151">Configuration settings</span></span>
* <span data-ttu-id="56534-152">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="56534-152">Environment variables</span></span>

<span data-ttu-id="56534-153">Incluya las líneas siguientes [middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) código al principio de la `Startup.Configure` canalización de procesamiento de solicitudes del método.</span><span class="sxs-lookup"><span data-stu-id="56534-153">Place the following [middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) code at the beginning of the `Startup.Configure` method's request processing pipeline.</span></span> <span data-ttu-id="56534-154">El entorno se comprueba antes de ejecuta el software intermedio para asegurarse de que el código solo se ejecuta en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="56534-154">The environment is checked before the middleware is run to ensure that the code is only executed in the Development environment.</span></span>

<span data-ttu-id="56534-155">Para obtener el entorno, use cualquiera de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="56534-155">To obtain the environment, use either of the following approaches:</span></span>

* <span data-ttu-id="56534-156">Insertar el `IHostingEnvironment` en el `Startup.Configure` método y comprobar el entorno con la variable local.</span><span class="sxs-lookup"><span data-stu-id="56534-156">Inject the `IHostingEnvironment` into the `Startup.Configure` method and check the environment with the local variable.</span></span> <span data-ttu-id="56534-157">El código de ejemplo siguiente muestra este enfoque.</span><span class="sxs-lookup"><span data-stu-id="56534-157">The following sample code demonstrates this approach.</span></span>

* <span data-ttu-id="56534-158">Asignar el entorno a una propiedad en el `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="56534-158">Assign the environment to a property in the `Startup` class.</span></span> <span data-ttu-id="56534-159">Comprobar el entorno mediante la propiedad (por ejemplo, `if (Environment.IsDevelopment())`).</span><span class="sxs-lookup"><span data-stu-id="56534-159">Check the environment using the property (for example, `if (Environment.IsDevelopment())`).</span></span>

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
