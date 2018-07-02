---
title: Hospedar ASP.NET Core en Linux con Apache
description: Aprenda a configurar Apache como servidor proxy inverso en CentOS para redirigir el tráfico HTTP a una aplicación web ASP.NET Core que se ejecuta en Kestrel.
author: spboyer
ms.author: spboyer
ms.custom: mvc
ms.date: 03/13/2018
uid: host-and-deploy/linux-apache
ms.openlocfilehash: c46c0aa578867ce306adc67a2e0d8b650b5fa5bd
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2018
ms.locfileid: "36960897"
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="80cc9-103">Hospedar ASP.NET Core en Linux con Apache</span><span class="sxs-lookup"><span data-stu-id="80cc9-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="80cc9-104">Por [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="80cc9-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="80cc9-105">Mediante esta guía, aprenda a configurar [Apache](https://httpd.apache.org/) como servidor proxy inverso en [CentOS 7](https://www.centos.org/) para redirigir el tráfico HTTP a una aplicación web ASP.NET Core que se ejecuta en [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="80cc9-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="80cc9-106">La [extensión mod_proxy](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) y los módulos relacionados crean el proxy inverso del servidor.</span><span class="sxs-lookup"><span data-stu-id="80cc9-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="80cc9-107">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="80cc9-107">Prerequisites</span></span>

1. <span data-ttu-id="80cc9-108">Servidor que ejecute CentOS 7, con una cuenta de usuario estándar con privilegios sudo.</span><span class="sxs-lookup"><span data-stu-id="80cc9-108">Server running CentOS 7 with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="80cc9-109">Tener .NET Core Runtime instalado en el servidor.</span><span class="sxs-lookup"><span data-stu-id="80cc9-109">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="80cc9-110">Vaya a la [página de descargas de .NET Core](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="80cc9-110">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="80cc9-111">Seleccione el tiempo de ejecución más reciente que no sea versión preliminar en la lista **Runtime**.</span><span class="sxs-lookup"><span data-stu-id="80cc9-111">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="80cc9-112">Seleccione y siga las instrucciones relativas a CentOS/Oracle.</span><span class="sxs-lookup"><span data-stu-id="80cc9-112">Select and follow the instructions for CentOS/Oracle.</span></span>
1. <span data-ttu-id="80cc9-113">Disponer de una aplicación de ASP.NET Core existente.</span><span class="sxs-lookup"><span data-stu-id="80cc9-113">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="80cc9-114">Publicar y copiar en la aplicación</span><span class="sxs-lookup"><span data-stu-id="80cc9-114">Publish and copy over the app</span></span>

<span data-ttu-id="80cc9-115">Configure la aplicación para una [implementación dependiente de Framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="80cc9-115">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="80cc9-116">Ejecute [dotnet publish](/dotnet/core/tools/dotnet-publish) desde el entorno de desarrollo para empaquetar una aplicación en un directorio (por ejemplo, *bin/Release/&lt;target_framework_moniker&gt;/publish*) que se pueda ejecutar en el servidor:</span><span class="sxs-lookup"><span data-stu-id="80cc9-116">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="80cc9-117">La aplicación también se puede publicar como una [implementación independiente](/dotnet/core/deploying/#self-contained-deployments-scd) si prefiere no mantener .NET Core Runtime en el servidor.</span><span class="sxs-lookup"><span data-stu-id="80cc9-117">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="80cc9-118">Copie la aplicación de ASP.NET Core en el servidor usando una herramienta que se integre en el flujo de trabajo de la organización (como SCP o SFTP).</span><span class="sxs-lookup"><span data-stu-id="80cc9-118">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="80cc9-119">Lo habitual es encontrar las aplicaciones web en el directorio *var* (por ejemplo, *aspnetcore/var/hellomvc*).</span><span class="sxs-lookup"><span data-stu-id="80cc9-119">It's common to locate web apps under the *var* directory (for example, *var/aspnetcore/hellomvc*).</span></span>

> [!NOTE]
> <span data-ttu-id="80cc9-120">En un escenario de implementación de producción, un flujo de trabajo de integración continua lleva a cabo la tarea de publicar la aplicación y copiar los recursos en el servidor.</span><span class="sxs-lookup"><span data-stu-id="80cc9-120">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

## <a name="configure-a-proxy-server"></a><span data-ttu-id="80cc9-121">Configurar un servidor proxy</span><span class="sxs-lookup"><span data-stu-id="80cc9-121">Configure a proxy server</span></span>

<span data-ttu-id="80cc9-122">Un proxy inverso es una configuración común para trabajar con aplicaciones web dinámicas.</span><span class="sxs-lookup"><span data-stu-id="80cc9-122">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="80cc9-123">El proxy inverso finaliza la solicitud HTTP y la reenvía a la aplicación ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="80cc9-123">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="80cc9-124">Un servidor proxy es el que reenvía las solicitudes de cliente a otro servidor en lugar de realizarlas él mismo.</span><span class="sxs-lookup"><span data-stu-id="80cc9-124">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="80cc9-125">Los proxies inversos las reenvían a un destino fijo, normalmente en nombre de clientes arbitrarios.</span><span class="sxs-lookup"><span data-stu-id="80cc9-125">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="80cc9-126">En esta guía, Apache se configura como proxy inverso que se ejecuta en el mismo servidor en el que Kestrel atiende la aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="80cc9-126">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="80cc9-127">Como el proxy inverso reenvía las solicitudes, use el [Middleware de encabezados reenviados](xref:host-and-deploy/proxy-load-balancer) del paquete [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/).</span><span class="sxs-lookup"><span data-stu-id="80cc9-127">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="80cc9-128">El middleware actualiza `Request.Scheme`, mediante el encabezado `X-Forwarded-Proto`, para que los URI de redireccionamiento y otras directivas de seguridad funcionen correctamente.</span><span class="sxs-lookup"><span data-stu-id="80cc9-128">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="80cc9-129">Cualquier componente que dependa del esquema (como la autenticación, la generación de vínculos, los redireccionamientos o la geolocalización) debe colocarse después de invocar al Middleware de encabezados reenviados.</span><span class="sxs-lookup"><span data-stu-id="80cc9-129">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="80cc9-130">Como norma general, el Middleware de encabezados reenviados se debe ejecutar antes de cualquier otro middleware, salvo el middleware de diagnóstico y control de errores.</span><span class="sxs-lookup"><span data-stu-id="80cc9-130">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="80cc9-131">Hacerlo en ese orden garantiza que el middleware que se basa en la información de encabezados reenviados pueda usar los valores de encabezado para procesarlos.</span><span class="sxs-lookup"><span data-stu-id="80cc9-131">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

::: moniker range=">= aspnetcore-2.0"
> [!NOTE]
> <span data-ttu-id="80cc9-132">Cualquiera de las configuraciones&mdash;con o sin un servidor proxy inverso&mdash;es una configuración de hospedaje válida y admitida para ASP.NET 2.0 o aplicaciones posteriores.</span><span class="sxs-lookup"><span data-stu-id="80cc9-132">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="80cc9-133">Para más información, vea [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy) (Cuándo se debe usar Kestrel con un proxy inverso).</span><span class="sxs-lookup"><span data-stu-id="80cc9-133">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>
::: moniker-end

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="80cc9-134">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="80cc9-134">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="80cc9-135">Invoque el método [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) en `Startup.Configure` antes de llamar a [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) o a un middleware de esquema de autenticación similar.</span><span class="sxs-lookup"><span data-stu-id="80cc9-135">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="80cc9-136">Configure el middleware para reenviar los encabezados `X-Forwarded-For` y `X-Forwarded-Proto`:</span><span class="sxs-lookup"><span data-stu-id="80cc9-136">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="80cc9-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="80cc9-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="80cc9-138">Invoque el método [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) en `Startup.Configure` antes de llamar a [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) y [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) o un middleware de esquema de autenticación similar.</span><span class="sxs-lookup"><span data-stu-id="80cc9-138">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="80cc9-139">Configure el middleware para reenviar los encabezados `X-Forwarded-For` y `X-Forwarded-Proto`:</span><span class="sxs-lookup"><span data-stu-id="80cc9-139">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseIdentity();
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

<span data-ttu-id="80cc9-140">Si no se especifica ningún valor [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) para el software intermedio, los encabezados predeterminados para reenviar son `None`.</span><span class="sxs-lookup"><span data-stu-id="80cc9-140">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="80cc9-141">Podría ser necesario realizar una configuración adicional para las aplicaciones hospedadas detrás de servidores proxy y equilibradores de carga.</span><span class="sxs-lookup"><span data-stu-id="80cc9-141">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="80cc9-142">Para más información, vea [Configurar ASP.NET Core para trabajar con servidores proxy y equilibradores de carga](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="80cc9-142">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-apache"></a><span data-ttu-id="80cc9-143">Instalar Apache</span><span class="sxs-lookup"><span data-stu-id="80cc9-143">Install Apache</span></span>

<span data-ttu-id="80cc9-144">Actualice los paquetes de CentOS a sus versiones estables más recientes:</span><span class="sxs-lookup"><span data-stu-id="80cc9-144">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="80cc9-145">Instale el servidor web de Apache en CentOS con un único comando `yum`:</span><span class="sxs-lookup"><span data-stu-id="80cc9-145">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="80cc9-146">Salida de ejemplo después de ejecutar el comando:</span><span class="sxs-lookup"><span data-stu-id="80cc9-146">Sample output after running the command:</span></span>

```bash
Downloading packages:
httpd-2.4.6-40.el7.centos.4.x86_64.rpm               | 2.7 MB  00:00:01
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
Installing : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 
Verifying  : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 

Installed:
httpd.x86_64 0:2.4.6-40.el7.centos.4

Complete!
```

> [!NOTE]
> <span data-ttu-id="80cc9-147">En este ejemplo, la salida refleja httpd.86_64, puesto que la versión de CentOS 7 es de 64 bits.</span><span class="sxs-lookup"><span data-stu-id="80cc9-147">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="80cc9-148">Para comprobar dónde está instalado Apache, ejecute `whereis httpd` desde un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="80cc9-148">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span>

### <a name="configure-apache"></a><span data-ttu-id="80cc9-149">Configurar Apache</span><span class="sxs-lookup"><span data-stu-id="80cc9-149">Configure Apache</span></span>

<span data-ttu-id="80cc9-150">Los archivos de configuración de Apache se encuentran en el directorio `/etc/httpd/conf.d/`.</span><span class="sxs-lookup"><span data-stu-id="80cc9-150">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="80cc9-151">Todos los archivos que tengan la extensión *.conf* se procesan en orden alfabético, además de los archivos de configuración del módulo de `/etc/httpd/conf.modules.d/`, que contiene todos los archivos de configuración necesarios para cargar los módulos.</span><span class="sxs-lookup"><span data-stu-id="80cc9-151">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="80cc9-152">Cree un archivo de configuración denominado *hellomvc.conf* para la aplicación:</span><span class="sxs-lookup"><span data-stu-id="80cc9-152">Create a configuration file, named *hellomvc.conf*, for the app:</span></span>

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ServerName www.example.com
    ServerAlias *.example.com
    ErrorLog ${APACHE_LOG_DIR}hellomvc-error.log
    CustomLog ${APACHE_LOG_DIR}hellomvc-access.log common
</VirtualHost>
```

<span data-ttu-id="80cc9-153">El bloque `VirtualHost` puede aparecer varias veces en uno o varios archivos en un servidor.</span><span class="sxs-lookup"><span data-stu-id="80cc9-153">The `VirtualHost` block can appear multiple times, in one or more files on a server.</span></span> <span data-ttu-id="80cc9-154">En el archivo de configuración anterior, Apache acepta tráfico público en el puerto 80.</span><span class="sxs-lookup"><span data-stu-id="80cc9-154">In the preceding configuration file, Apache accepts public traffic on port 80.</span></span> <span data-ttu-id="80cc9-155">El dominio `www.example.com` se atiende y el alias `*.example.com` se resuelve en el mismo sitio web.</span><span class="sxs-lookup"><span data-stu-id="80cc9-155">The domain `www.example.com` is being served, and the `*.example.com` alias resolves to the same website.</span></span> <span data-ttu-id="80cc9-156">Para más información, consulte [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) (Compatibilidad con el host virtual basado en nombres).</span><span class="sxs-lookup"><span data-stu-id="80cc9-156">See [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) for more information.</span></span> <span data-ttu-id="80cc9-157">Las solicitudes se redirigen mediante proxy en la raíz al puerto 5000 del servidor en 127.0.0.1.</span><span class="sxs-lookup"><span data-stu-id="80cc9-157">Requests are proxied at the root to port 5000 of the server at 127.0.0.1.</span></span> <span data-ttu-id="80cc9-158">Para la comunicación bidireccional, se requieren `ProxyPass` y `ProxyPassReverse`.</span><span class="sxs-lookup"><span data-stu-id="80cc9-158">For bi-directional communication, `ProxyPass` and `ProxyPassReverse` are required.</span></span>

> [!WARNING]
> <span data-ttu-id="80cc9-159">Si no se especifica una [directiva de ServerName](https://httpd.apache.org/docs/current/mod/core.html#servername) correcta en **VirtualHost**, el bloque expone la aplicación a las vulnerabilidades de seguridad.</span><span class="sxs-lookup"><span data-stu-id="80cc9-159">Failure to specify a proper [ServerName directive](https://httpd.apache.org/docs/current/mod/core.html#servername) in the **VirtualHost** block exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="80cc9-160">Los enlaces de carácter comodín de subdominio (por ejemplo, `*.example.com`) no presentan este riesgo de seguridad si se controla todo el dominio primario (a diferencia de `*.com`, que sí es vulnerable).</span><span class="sxs-lookup"><span data-stu-id="80cc9-160">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="80cc9-161">Vea la [sección 5.4 de RFC 7230](https://tools.ietf.org/html/rfc7230#section-5.4) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="80cc9-161">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="80cc9-162">El registro se puede configurar por `VirtualHost` con las directivas `ErrorLog` y `CustomLog`.</span><span class="sxs-lookup"><span data-stu-id="80cc9-162">Logging can be configured per `VirtualHost` using `ErrorLog` and `CustomLog` directives.</span></span> <span data-ttu-id="80cc9-163">`ErrorLog` es la ubicación donde el servidor registra los errores, y `CustomLog` establece el nombre de archivo y el formato del archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="80cc9-163">`ErrorLog` is the location where the server logs errors, and `CustomLog` sets the filename and format of log file.</span></span> <span data-ttu-id="80cc9-164">En este caso, aquí es donde se registra la información de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="80cc9-164">In this case, this is where request information is logged.</span></span> <span data-ttu-id="80cc9-165">Hay una línea para cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="80cc9-165">There's one line for each request.</span></span>

<span data-ttu-id="80cc9-166">Guarde el archivo y pruebe la configuración.</span><span class="sxs-lookup"><span data-stu-id="80cc9-166">Save the file and test the configuration.</span></span> <span data-ttu-id="80cc9-167">Si se pasa todo, la respuesta debe ser `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="80cc9-167">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="80cc9-168">Reinicie Apache:</span><span class="sxs-lookup"><span data-stu-id="80cc9-168">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="80cc9-169">Supervisión de la aplicación</span><span class="sxs-lookup"><span data-stu-id="80cc9-169">Monitoring the app</span></span>

<span data-ttu-id="80cc9-170">Apache está configurado ahora para reenviar las solicitudes efectuadas a `http://localhost:80` en la aplicación ASP.NET Core que se ejecuta en Kestrel en `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="80cc9-170">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="80cc9-171">En cambio, Apache no está configurado para administrar el proceso de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="80cc9-171">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="80cc9-172">Use *systemd* y cree un archivo de servicio para iniciar y supervisar la aplicación web subyacente.</span><span class="sxs-lookup"><span data-stu-id="80cc9-172">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="80cc9-173">*systemd* es un sistema de inicio que proporciona muchas características eficaces para iniciar, detener y administrar procesos.</span><span class="sxs-lookup"><span data-stu-id="80cc9-173">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="80cc9-174">Crear el archivo de servicio</span><span class="sxs-lookup"><span data-stu-id="80cc9-174">Create the service file</span></span>

<span data-ttu-id="80cc9-175">Cree el archivo de definición de servicio:</span><span class="sxs-lookup"><span data-stu-id="80cc9-175">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="80cc9-176">Un archivo de servicio de ejemplo para la aplicación:</span><span class="sxs-lookup"><span data-stu-id="80cc9-176">An example service file for the app:</span></span>

```
[Unit]
Description=Example .NET Web API App running on CentOS 7

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
SyslogIdentifier=dotnet-example
User=apache
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```

> [!NOTE]
> <span data-ttu-id="80cc9-177">**Usuario**: si el usuario *apache* no se usa en la configuración, primero se debe crear el usuario y se le debe conceder la propiedad adecuada para los archivos.</span><span class="sxs-lookup"><span data-stu-id="80cc9-177">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

> [!NOTE]
> <span data-ttu-id="80cc9-178">Algunos valores (por ejemplo, cadenas de conexión de SQL) deben ser de escape para que los proveedores de configuración lean las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="80cc9-178">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="80cc9-179">Use el siguiente comando para generar un valor de escape correctamente para su uso en el archivo de configuración:</span><span class="sxs-lookup"><span data-stu-id="80cc9-179">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="80cc9-180">Guarde el archivo y habilite el servicio.</span><span class="sxs-lookup"><span data-stu-id="80cc9-180">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="80cc9-181">Inicie el servicio y compruebe que se está ejecutando:</span><span class="sxs-lookup"><span data-stu-id="80cc9-181">Start the service and verify that it's running:</span></span>

```bash
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on CentOS 7
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="80cc9-182">Con el proxy inverso configurado y Kestrel administrado mediante *systemd*, la aplicación web está completamente configurada y se puede acceder a ella desde un explorador en la máquina local en `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="80cc9-182">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="80cc9-183">Inspeccionar los encabezados de respuesta; el encabezado **Server** indica que Kestrel atiende la aplicación ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="80cc9-183">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="80cc9-184">Ver los registros</span><span class="sxs-lookup"><span data-stu-id="80cc9-184">Viewing logs</span></span>

<span data-ttu-id="80cc9-185">Dado que la aplicación web que usa Kestrel se administra mediante *systemd*, los procesos y eventos se registran en un diario centralizado.</span><span class="sxs-lookup"><span data-stu-id="80cc9-185">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="80cc9-186">Sin embargo, este diario incluye todas las entradas de todos los servicios y procesos administrados por *systemd*.</span><span class="sxs-lookup"><span data-stu-id="80cc9-186">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="80cc9-187">Para ver los elementos específicos de `kestrel-hellomvc.service`, use el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="80cc9-187">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="80cc9-188">Para el filtrado de tiempo, especifique las opciones de tiempo con el comando.</span><span class="sxs-lookup"><span data-stu-id="80cc9-188">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="80cc9-189">Por ejemplo, use `--since today` para filtrar por el día actual o `--until 1 hour ago` para ver las entradas de la hora anterior.</span><span class="sxs-lookup"><span data-stu-id="80cc9-189">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="80cc9-190">Para más información, consulte la [página man de journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="80cc9-190">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="80cc9-191">Protección de la aplicación</span><span class="sxs-lookup"><span data-stu-id="80cc9-191">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="80cc9-192">Configurar el firewall</span><span class="sxs-lookup"><span data-stu-id="80cc9-192">Configure firewall</span></span>

<span data-ttu-id="80cc9-193">*Firewalld* es un demonio dinámico para administrar el firewall con compatibilidad con zonas de red.</span><span class="sxs-lookup"><span data-stu-id="80cc9-193">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="80cc9-194">Los puertos y el filtrado de paquetes se pueden seguir administrando mediante iptables.</span><span class="sxs-lookup"><span data-stu-id="80cc9-194">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="80cc9-195">*Firewalld* debe instalarse de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="80cc9-195">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="80cc9-196">`yum` puede usarse para instalar el paquete o comprobar que está instalado.</span><span class="sxs-lookup"><span data-stu-id="80cc9-196">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="80cc9-197">Use `firewalld` para abrir solo los puertos necesarios para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="80cc9-197">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="80cc9-198">En este caso se usan los puertos 80 y 443.</span><span class="sxs-lookup"><span data-stu-id="80cc9-198">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="80cc9-199">Los siguientes comandos establecen de forma permanente que se abran los puertos 80 y 443:</span><span class="sxs-lookup"><span data-stu-id="80cc9-199">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="80cc9-200">Vuelva a cargar la configuración del firewall.</span><span class="sxs-lookup"><span data-stu-id="80cc9-200">Reload the firewall settings.</span></span> <span data-ttu-id="80cc9-201">Compruebe los servicios y puertos disponibles en la zona predeterminada.</span><span class="sxs-lookup"><span data-stu-id="80cc9-201">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="80cc9-202">Hay opciones disponibles si se inspecciona `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="80cc9-202">Options are available by inspecting `firewall-cmd -h`.</span></span>

```bash
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
```

```bash
public (default, active)
interfaces: eth0
sources: 
services: dhcpv6-client
ports: 443/tcp 80/tcp
masquerade: no
forward-ports: 
icmp-blocks: 
rich rules: 
```

### <a name="ssl-configuration"></a><span data-ttu-id="80cc9-203">Configuración de SSL</span><span class="sxs-lookup"><span data-stu-id="80cc9-203">SSL configuration</span></span>

<span data-ttu-id="80cc9-204">Para configurar Apache para SSL, se usa el módulo *mod_ssl*.</span><span class="sxs-lookup"><span data-stu-id="80cc9-204">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="80cc9-205">Cuando se instaló el módulo *httpd*, también lo hizo el módulo *mod_ssl*.</span><span class="sxs-lookup"><span data-stu-id="80cc9-205">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="80cc9-206">Si aún no se ha instalado, use `yum` para agregarlo a la configuración.</span><span class="sxs-lookup"><span data-stu-id="80cc9-206">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```

<span data-ttu-id="80cc9-207">Para usar SSL, instale el módulo `mod_rewrite` para habilitar la reescritura de direcciones URL:</span><span class="sxs-lookup"><span data-stu-id="80cc9-207">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="80cc9-208">Modifique el archivo *hellomvc.conf* para permitir la reescritura de direcciones URL y proteger la comunicación en el puerto 443:</span><span class="sxs-lookup"><span data-stu-id="80cc9-208">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

> [!NOTE]
> <span data-ttu-id="80cc9-209">En este ejemplo se usa un certificado generado localmente.</span><span class="sxs-lookup"><span data-stu-id="80cc9-209">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="80cc9-210">**SSLCertificateFile** debe ser el archivo de certificado principal para el nombre de dominio.</span><span class="sxs-lookup"><span data-stu-id="80cc9-210">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="80cc9-211">**SSLCertificateKeyFile** debe ser el archivo de claves generado al crear el CSR.</span><span class="sxs-lookup"><span data-stu-id="80cc9-211">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="80cc9-212">**SSLCertificateChainFile** debe ser el archivo de certificado intermedio (si existe) proporcionado por la entidad de certificación.</span><span class="sxs-lookup"><span data-stu-id="80cc9-212">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="80cc9-213">Guarde el archivo y pruebe la configuración.</span><span class="sxs-lookup"><span data-stu-id="80cc9-213">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="80cc9-214">Reinicie Apache:</span><span class="sxs-lookup"><span data-stu-id="80cc9-214">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="80cc9-215">Sugerencias adicionales de Apache</span><span class="sxs-lookup"><span data-stu-id="80cc9-215">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="80cc9-216">Encabezados adicionales</span><span class="sxs-lookup"><span data-stu-id="80cc9-216">Additional headers</span></span>

<span data-ttu-id="80cc9-217">Para protegerse frente a ataques malintencionados, hay unos encabezados que se deben modificar o agregar.</span><span class="sxs-lookup"><span data-stu-id="80cc9-217">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="80cc9-218">Asegúrese de que el módulo `mod_headers` está instalado.</span><span class="sxs-lookup"><span data-stu-id="80cc9-218">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="80cc9-219">Protección de Apache de los ataques de secuestro de clic</span><span class="sxs-lookup"><span data-stu-id="80cc9-219">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="80cc9-220">El [secuestro de clic](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), también conocido como *redireccionamiento de interfaz de usuario*, es un ataque malintencionado donde al visitante de un sitio web se le engaña para que haga clic en un vínculo o botón de una página distinta de la que actualmente está visitando.</span><span class="sxs-lookup"><span data-stu-id="80cc9-220">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="80cc9-221">Use `X-FRAME-OPTIONS` para proteger el sitio.</span><span class="sxs-lookup"><span data-stu-id="80cc9-221">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="80cc9-222">Edite el archivo *httpd.conf*.</span><span class="sxs-lookup"><span data-stu-id="80cc9-222">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="80cc9-223">Agregue la línea `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span><span class="sxs-lookup"><span data-stu-id="80cc9-223">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="80cc9-224">Guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="80cc9-224">Save the file.</span></span> <span data-ttu-id="80cc9-225">Reinicie Apache.</span><span class="sxs-lookup"><span data-stu-id="80cc9-225">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="80cc9-226">Examen de tipo MIME</span><span class="sxs-lookup"><span data-stu-id="80cc9-226">MIME-type sniffing</span></span>

<span data-ttu-id="80cc9-227">El encabezado `X-Content-Type-Options` impide que Internet Explorer *examine MIME* (de forma que el elemento `Content-Type` de un archivo se determina a partir del contenido del archivo).</span><span class="sxs-lookup"><span data-stu-id="80cc9-227">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="80cc9-228">Si el servidor establece el encabezado `Content-Type` en `text/html` con la opción `nosniff` establecida, Internet Explorer representa el contenido como `text/html` sin tener en cuenta el contenido del archivo.</span><span class="sxs-lookup"><span data-stu-id="80cc9-228">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="80cc9-229">Edite el archivo *httpd.conf*.</span><span class="sxs-lookup"><span data-stu-id="80cc9-229">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="80cc9-230">Agregue la línea `Header set X-Content-Type-Options "nosniff"`.</span><span class="sxs-lookup"><span data-stu-id="80cc9-230">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="80cc9-231">Guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="80cc9-231">Save the file.</span></span> <span data-ttu-id="80cc9-232">Reinicie Apache.</span><span class="sxs-lookup"><span data-stu-id="80cc9-232">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="80cc9-233">Equilibrio de carga</span><span class="sxs-lookup"><span data-stu-id="80cc9-233">Load Balancing</span></span>

<span data-ttu-id="80cc9-234">En este ejemplo se muestra cómo instalar y configurar Apache en CentOS 7 y Kestrel en el mismo equipo de la instancia.</span><span class="sxs-lookup"><span data-stu-id="80cc9-234">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="80cc9-235">Para no tener un único punto de error, el uso de *mod_proxy_balancer* y la modificación de **VirtualHost** permitirían administrar varias instancias de las aplicaciones web detrás del servidor proxy de Apache.</span><span class="sxs-lookup"><span data-stu-id="80cc9-235">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="80cc9-236">En el archivo de configuración que se muestra a continuación, se configura una instancia adicional de la aplicación `hellomvc` para que se ejecute en el puerto 5001.</span><span class="sxs-lookup"><span data-stu-id="80cc9-236">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="80cc9-237">La sección *Proxy* se establece con una configuración de equilibrador con dos miembros para equilibrar la carga *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="80cc9-237">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPass / balancer://mycluster/ 

    ProxyPassReverse / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5001/

    <Proxy balancer://mycluster>
        BalancerMember http://127.0.0.1:5000
        BalancerMember http://127.0.0.1:5001 
        ProxySet lbmethod=byrequests
    </Proxy>

    <Location />
        SetHandler balancer
    </Location>
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

### <a name="rate-limits"></a><span data-ttu-id="80cc9-238">Límites de velocidad</span><span class="sxs-lookup"><span data-stu-id="80cc9-238">Rate Limits</span></span>

<span data-ttu-id="80cc9-239">Use *mod_ratelimit*, que se incluye en el módulo *httpd*; el ancho de banda de los clientes puede ser limitado:</span><span class="sxs-lookup"><span data-stu-id="80cc9-239">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="80cc9-240">En el archivo de ejemplo se limita el ancho de banda a 600 KB/s en la ubicación raíz:</span><span class="sxs-lookup"><span data-stu-id="80cc9-240">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```

## <a name="additional-resources"></a><span data-ttu-id="80cc9-241">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="80cc9-241">Additional resources</span></span>

* [<span data-ttu-id="80cc9-242">Configurar ASP.NET Core para trabajar con servidores proxy y equilibradores de carga</span><span class="sxs-lookup"><span data-stu-id="80cc9-242">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
