---
title: Hospedar ASP.NET Core en Linux con Apache
description: "Obtenga información acerca de cómo configurar Apache como un servidor proxy inverso en CentOS para redirigir el tráfico HTTP a una aplicación web de ASP.NET Core con Kestrel."
author: spboyer
manager: wpickett
ms.author: spboyer
ms.custom: mvc
ms.date: 10/19/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-apache
ms.openlocfilehash: 61827f456ba01ffa726f3446401156409b29111d
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2018
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="eca46-103">Hospedar ASP.NET Core en Linux con Apache</span><span class="sxs-lookup"><span data-stu-id="eca46-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="eca46-104">Por [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="eca46-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="eca46-105">Con esta guía, obtenga información acerca de cómo configurar [Apache](https://httpd.apache.org/) como un servidor proxy inverso en [CentOS 7](https://www.centos.org/) para redirigir el tráfico HTTP a una aplicación web de ASP.NET Core con [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="eca46-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="eca46-106">El [mod_proxy extensión](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) y módulos relacionados Crear proxy inverso del servidor.</span><span class="sxs-lookup"><span data-stu-id="eca46-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="eca46-107">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="eca46-107">Prerequisites</span></span>

1. <span data-ttu-id="eca46-108">Servidor que ejecuta CentOS 7 con una cuenta de usuario estándar con privilegios sudo</span><span class="sxs-lookup"><span data-stu-id="eca46-108">Server running CentOS 7 with a standard user account with sudo privilege</span></span>
2. <span data-ttu-id="eca46-109">Aplicación de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eca46-109">ASP.NET Core app</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="eca46-110">Publicar la aplicación</span><span class="sxs-lookup"><span data-stu-id="eca46-110">Publish the app</span></span>

<span data-ttu-id="eca46-111">Publicar la aplicación como un [implementación independiente](/dotnet/core/deploying/#self-contained-deployments-scd) en configuración de lanzamiento para el tiempo de ejecución de CentOS 7 (`centos.7-x64`).</span><span class="sxs-lookup"><span data-stu-id="eca46-111">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) in Release configuration for the CentOS 7 runtime (`centos.7-x64`).</span></span> <span data-ttu-id="eca46-112">Copie el contenido de la *bin/Release/netcoreapp2.0/centos.7-x64/publish* carpeta al servidor mediante el SCP, FTP u otro método de transferencia de archivos.</span><span class="sxs-lookup"><span data-stu-id="eca46-112">Copy the contents of the *bin/Release/netcoreapp2.0/centos.7-x64/publish* folder to the server using SCP, FTP, or other file transfer method.</span></span>

> [!NOTE]
> <span data-ttu-id="eca46-113">En un escenario de implementación de producción, un flujo de trabajo de integración continua realiza el trabajo de publicación de la aplicación y copiar los activos en el servidor.</span><span class="sxs-lookup"><span data-stu-id="eca46-113">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="eca46-114">Configurar un servidor proxy</span><span class="sxs-lookup"><span data-stu-id="eca46-114">Configure a proxy server</span></span>

<span data-ttu-id="eca46-115">Un proxy inverso es una configuración común para servir las aplicaciones web dinámicas.</span><span class="sxs-lookup"><span data-stu-id="eca46-115">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="eca46-116">El proxy inverso finaliza la solicitud HTTP y lo reenvía a la aplicación ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="eca46-116">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="eca46-117">Un servidor proxy es uno que reenvía las solicitudes de cliente a otro servidor en lugar de satisfacer las solicitudes de sí mismo.</span><span class="sxs-lookup"><span data-stu-id="eca46-117">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="eca46-118">Los proxies inversos las reenvían a un destino fijo, normalmente en nombre de clientes arbitrarios.</span><span class="sxs-lookup"><span data-stu-id="eca46-118">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="eca46-119">En esta guía, Apache está configurado como el proxy inverso que se ejecuta en el mismo servidor que Kestrel sigue dando servicio a la aplicación de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eca46-119">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="eca46-120">Dado que se reenvíen solicitudes por proxy inverso, usar el Middleware de encabezados reenviados desde el [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) paquete.</span><span class="sxs-lookup"><span data-stu-id="eca46-120">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="eca46-121">Las actualizaciones de software intermedio el `Request.Scheme`, usando la `X-Forwarded-Proto` encabezado, por lo que ese URI de redirección y las otras directivas de seguridad funcionen correctamente.</span><span class="sxs-lookup"><span data-stu-id="eca46-121">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="eca46-122">Al utilizar cualquier tipo de middleware de autenticación, el Middleware de encabezados reenviados primero debe ejecutar.</span><span class="sxs-lookup"><span data-stu-id="eca46-122">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="eca46-123">Este orden garantiza que el middleware de autenticación puede consumir los valores de encabezado y generar el URI de redireccionamiento correcto.</span><span class="sxs-lookup"><span data-stu-id="eca46-123">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="eca46-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="eca46-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="eca46-125">Invocar la [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) método `Startup.Configure` antes de llamar a [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) o similar middleware de esquema de autenticación:</span><span class="sxs-lookup"><span data-stu-id="eca46-125">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="eca46-126">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="eca46-126">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="eca46-127">Invocar la [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) método `Startup.Configure` antes de llamar a [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) y [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) o un esquema de autenticación similares software intermedio:</span><span class="sxs-lookup"><span data-stu-id="eca46-127">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware:</span></span>

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

<span data-ttu-id="eca46-128">Si no hay ningún [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) se especifican en el middleware, los encabezados de forma predeterminada para reenviar son `None`.</span><span class="sxs-lookup"><span data-stu-id="eca46-128">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

### <a name="install-apache"></a><span data-ttu-id="eca46-129">Instalar Apache</span><span class="sxs-lookup"><span data-stu-id="eca46-129">Install Apache</span></span>

<span data-ttu-id="eca46-130">Actualizar paquetes de CentOS a sus versiones estables más recientes:</span><span class="sxs-lookup"><span data-stu-id="eca46-130">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="eca46-131">Instalar el servidor web Apache en CentOS con una sola `yum` comando:</span><span class="sxs-lookup"><span data-stu-id="eca46-131">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="eca46-132">Resultado después de ejecutar el comando de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="eca46-132">Sample output after running the command:</span></span>

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
> <span data-ttu-id="eca46-133">En este ejemplo, el resultado refleja httpd.86_64 puesto que la versión de CentOS 7 es de 64 bits.</span><span class="sxs-lookup"><span data-stu-id="eca46-133">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="eca46-134">Para comprobar dónde está instalado Apache, ejecute `whereis httpd` desde un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="eca46-134">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span> 

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="eca46-135">Configurar Apache para un proxy inverso</span><span class="sxs-lookup"><span data-stu-id="eca46-135">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="eca46-136">Los archivos de configuración de Apache se encuentran en el directorio `/etc/httpd/conf.d/`.</span><span class="sxs-lookup"><span data-stu-id="eca46-136">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="eca46-137">Cualquier archivo con el *.conf* extensión se procesa en orden alfabético además de los archivos de configuración de módulo en `/etc/httpd/conf.modules.d/`, que contiene cualquier configuración de los archivos necesario para cargar los módulos.</span><span class="sxs-lookup"><span data-stu-id="eca46-137">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="eca46-138">Crear un archivo de configuración de la aplicación denominada `hellomvc.conf`:</span><span class="sxs-lookup"><span data-stu-id="eca46-138">Create a configuration file for the app named `hellomvc.conf`:</span></span>

```
<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
</VirtualHost>
```

<span data-ttu-id="eca46-139">El **VirtualHost** nodo puede aparecer varias veces en uno o varios archivos en un servidor.</span><span class="sxs-lookup"><span data-stu-id="eca46-139">The **VirtualHost** node can appear multiple times in one or more files on a server.</span></span> <span data-ttu-id="eca46-140">**VirtualHost** se establece para que escuche en cualquier dirección IP que usa el puerto 80.</span><span class="sxs-lookup"><span data-stu-id="eca46-140">**VirtualHost** is set to listen on any IP address using port 80.</span></span> <span data-ttu-id="eca46-141">Las dos líneas siguientes se establecen en las solicitudes de proxy en el directorio raíz en el servidor en 127.0.0.1 en el puerto 5000.</span><span class="sxs-lookup"><span data-stu-id="eca46-141">The next two lines are set to proxy requests at the root to the server at 127.0.0.1 on port 5000.</span></span> <span data-ttu-id="eca46-142">Para la comunicación bidireccional, *ProxyPass* y *ProxyPassReverse* son necesarios.</span><span class="sxs-lookup"><span data-stu-id="eca46-142">For bi-directional communication, *ProxyPass* and *ProxyPassReverse* are required.</span></span>

<span data-ttu-id="eca46-143">Se puede configurar el registro por **VirtualHost** con **ErrorLog** y **CustomLog** directivas.</span><span class="sxs-lookup"><span data-stu-id="eca46-143">Logging can be configured per **VirtualHost** using **ErrorLog** and **CustomLog** directives.</span></span> <span data-ttu-id="eca46-144">**Registro de errores** es la ubicación donde el servidor registra errores, y **CustomLog** establece el nombre de archivo y el formato de archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="eca46-144">**ErrorLog** is the location where the server logs errors, and **CustomLog** sets the filename and format of log file.</span></span> <span data-ttu-id="eca46-145">En este caso, esto es donde se registra la información de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="eca46-145">In this case, this is where request information is logged.</span></span> <span data-ttu-id="eca46-146">Hay una línea para cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="eca46-146">There's one line for each request.</span></span>

<span data-ttu-id="eca46-147">Guarde el archivo y la configuración de pruebas.</span><span class="sxs-lookup"><span data-stu-id="eca46-147">Save the file and test the configuration.</span></span> <span data-ttu-id="eca46-148">Si se pasa todo, la respuesta debe ser `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="eca46-148">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="eca46-149">Reinicie Apache:</span><span class="sxs-lookup"><span data-stu-id="eca46-149">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="eca46-150">Supervisión de la aplicación</span><span class="sxs-lookup"><span data-stu-id="eca46-150">Monitoring the app</span></span>

<span data-ttu-id="eca46-151">Apache ahora está configurado para reenviar las solicitudes realizadas a `http://localhost:80` a la aplicación de ASP.NET Core con Kestrel en `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="eca46-151">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="eca46-152">Sin embargo, Apache no está configurado para administrar el proceso de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="eca46-152">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="eca46-153">Use *systemd* y cree un archivo de servicio para iniciar y supervisar la aplicación web subyacente.</span><span class="sxs-lookup"><span data-stu-id="eca46-153">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="eca46-154">*systemd* es un sistema de inicio que proporciona muchas características eficaces para iniciar, detener y administrar procesos.</span><span class="sxs-lookup"><span data-stu-id="eca46-154">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="eca46-155">Crear el archivo de servicio</span><span class="sxs-lookup"><span data-stu-id="eca46-155">Create the service file</span></span>

<span data-ttu-id="eca46-156">Cree el archivo de definición de servicio:</span><span class="sxs-lookup"><span data-stu-id="eca46-156">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="eca46-157">Un archivo de servicio de ejemplo para la aplicación:</span><span class="sxs-lookup"><span data-stu-id="eca46-157">An example service file for the app:</span></span>

```
[Unit]
Description=Example .NET Web API App running on CentOS 7

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
# Restart service after 10 seconds if dotnet service crashes
RestartSec=10
SyslogIdentifier=dotnet-example
User=apache
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```

> [!NOTE]
> <span data-ttu-id="eca46-158">**Usuario** &mdash; si el usuario *apache* no se usa la configuración, el usuario debe crear primero y determinado propiedad adecuada para los archivos.</span><span class="sxs-lookup"><span data-stu-id="eca46-158">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="eca46-159">Guarde el archivo y habilitar el servicio:</span><span class="sxs-lookup"><span data-stu-id="eca46-159">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="eca46-160">Iniciar el servicio y compruebe que se está ejecutando:</span><span class="sxs-lookup"><span data-stu-id="eca46-160">Start the service and verify that it's running:</span></span>

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

<span data-ttu-id="eca46-161">Con el proxy inverso configurado y Kestrel administrada a través de *systemd*, la aplicación web está configurada completamente y son accesibles desde un explorador en el equipo local en `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="eca46-161">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="eca46-162">Inspeccionar los encabezados de respuesta, el **Server** encabezado indica que la aplicación de ASP.NET Core atendida por Kestrel:</span><span class="sxs-lookup"><span data-stu-id="eca46-162">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="eca46-163">Ver los registros</span><span class="sxs-lookup"><span data-stu-id="eca46-163">Viewing logs</span></span>

<span data-ttu-id="eca46-164">Desde la aplicación web utilizando Kestrel se administra con *systemd*, procesos y eventos se registran en un diario de centralizada.</span><span class="sxs-lookup"><span data-stu-id="eca46-164">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="eca46-165">Sin embargo, este diario incluye entradas para todos los servicios y procesos administrados por *systemd*.</span><span class="sxs-lookup"><span data-stu-id="eca46-165">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="eca46-166">Para ver los elementos específicos de `kestrel-hellomvc.service`, use el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="eca46-166">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="eca46-167">Para filtrar el tiempo, especifique las opciones de tiempo con el comando.</span><span class="sxs-lookup"><span data-stu-id="eca46-167">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="eca46-168">Por ejemplo, utilice `--since today` para filtrar en la actualidad o `--until 1 hour ago` para ver las entradas de la hora anterior.</span><span class="sxs-lookup"><span data-stu-id="eca46-168">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="eca46-169">Para obtener más información, consulte el [página del comando man journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="eca46-169">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="eca46-170">Protección de la aplicación</span><span class="sxs-lookup"><span data-stu-id="eca46-170">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="eca46-171">Configurar el firewall</span><span class="sxs-lookup"><span data-stu-id="eca46-171">Configure firewall</span></span>

<span data-ttu-id="eca46-172">*Firewalld* es un demonio dinámico para administrar el firewall con compatibilidad para las zonas de red.</span><span class="sxs-lookup"><span data-stu-id="eca46-172">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="eca46-173">Puertos y el filtrado de paquetes pueden seguir administrándose mediante iptables.</span><span class="sxs-lookup"><span data-stu-id="eca46-173">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="eca46-174">*Firewalld* debe instalarse de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="eca46-174">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="eca46-175">`yum`puede utilizarse para instalar el paquete o compruebe que está instalado.</span><span class="sxs-lookup"><span data-stu-id="eca46-175">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="eca46-176">Use `firewalld` para abrir sólo los puertos necesarios para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="eca46-176">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="eca46-177">En este caso se usan los puertos 80 y 443.</span><span class="sxs-lookup"><span data-stu-id="eca46-177">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="eca46-178">Los siguientes comandos de forma permanente establecen los puertos 80 y 443 para abrir:</span><span class="sxs-lookup"><span data-stu-id="eca46-178">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="eca46-179">Vuelva a cargar la configuración del firewall.</span><span class="sxs-lookup"><span data-stu-id="eca46-179">Reload the firewall settings.</span></span> <span data-ttu-id="eca46-180">Compruebe los servicios disponibles y los puertos en la zona predeterminada.</span><span class="sxs-lookup"><span data-stu-id="eca46-180">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="eca46-181">Opciones están disponibles mediante la inspección `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="eca46-181">Options are available by inspecting `firewall-cmd -h`.</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="eca46-182">Configuración de SSL</span><span class="sxs-lookup"><span data-stu-id="eca46-182">SSL configuration</span></span>

<span data-ttu-id="eca46-183">Para configurar Apache para SSL, el *mod_ssl* módulo se usa.</span><span class="sxs-lookup"><span data-stu-id="eca46-183">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="eca46-184">Cuando el *httpd* se instaló el módulo, el *mod_ssl* también se instala el módulo.</span><span class="sxs-lookup"><span data-stu-id="eca46-184">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="eca46-185">Si aún no se ha instalado, use `yum` para agregarlo a la configuración.</span><span class="sxs-lookup"><span data-stu-id="eca46-185">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```
<span data-ttu-id="eca46-186">Para utilizar SSL, instalar el `mod_rewrite` módulo para habilitar la reescritura de direcciones URL:</span><span class="sxs-lookup"><span data-stu-id="eca46-186">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="eca46-187">Modificar el *hellomvc.conf* archivo para habilitar la reescritura de direcciones URL y proteger la comunicación en el puerto 443:</span><span class="sxs-lookup"><span data-stu-id="eca46-187">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

```
<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/ [R,L]
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
> <span data-ttu-id="eca46-188">En este ejemplo se utiliza un certificado generado localmente.</span><span class="sxs-lookup"><span data-stu-id="eca46-188">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="eca46-189">**SSLCertificateFile** debe ser el archivo de certificado principal para el nombre de dominio.</span><span class="sxs-lookup"><span data-stu-id="eca46-189">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="eca46-190">**SSLCertificateKeyFile** debe ser el archivo de clave generar cuando se crea el CSR.</span><span class="sxs-lookup"><span data-stu-id="eca46-190">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="eca46-191">**SSLCertificateChainFile** debe ser el archivo de certificado intermedio (si existe) que se ha proporcionado por la entidad de certificación.</span><span class="sxs-lookup"><span data-stu-id="eca46-191">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="eca46-192">Guarde el archivo y la configuración de prueba:</span><span class="sxs-lookup"><span data-stu-id="eca46-192">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="eca46-193">Reinicie Apache:</span><span class="sxs-lookup"><span data-stu-id="eca46-193">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="eca46-194">Sugerencias adicionales de Apache</span><span class="sxs-lookup"><span data-stu-id="eca46-194">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="eca46-195">Encabezados adicionales</span><span class="sxs-lookup"><span data-stu-id="eca46-195">Additional headers</span></span>

<span data-ttu-id="eca46-196">Para proteger frente a ataques malintencionados, hay algunos encabezados que deben ser modificados o agregados.</span><span class="sxs-lookup"><span data-stu-id="eca46-196">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="eca46-197">Asegúrese de que el `mod_headers` módulo está instalado:</span><span class="sxs-lookup"><span data-stu-id="eca46-197">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="eca46-198">Apache seguro frente a ataques de clickjacking</span><span class="sxs-lookup"><span data-stu-id="eca46-198">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="eca46-199">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), también conocida como un *UI reclamación ataque*, es un ataque malintencionado donde el visitante de un sitio Web engañar al hacer clic en un vínculo o botón en una página distinta que actualmente está visitando.</span><span class="sxs-lookup"><span data-stu-id="eca46-199">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="eca46-200">Use `X-FRAME-OPTIONS` para proteger el sitio.</span><span class="sxs-lookup"><span data-stu-id="eca46-200">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="eca46-201">Editar la *httpd.conf* archivo:</span><span class="sxs-lookup"><span data-stu-id="eca46-201">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="eca46-202">Agregue la línea `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span><span class="sxs-lookup"><span data-stu-id="eca46-202">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="eca46-203">Guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="eca46-203">Save the file.</span></span> <span data-ttu-id="eca46-204">Reinicie Apache.</span><span class="sxs-lookup"><span data-stu-id="eca46-204">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="eca46-205">Examen de tipo MIME</span><span class="sxs-lookup"><span data-stu-id="eca46-205">MIME-type sniffing</span></span>

<span data-ttu-id="eca46-206">El `X-Content-Type-Options` encabezado impide que Internet Explorer *examen de MIME* (determinar un archivo `Content-Type` desde el contenido del archivo).</span><span class="sxs-lookup"><span data-stu-id="eca46-206">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determing a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="eca46-207">Si el servidor establece la `Content-Type` encabezado a `text/html` con el `nosniff` representa el conjunto de opciones, Internet Explorer el contenido como `text/html` sin tener en cuenta el contenido del archivo.</span><span class="sxs-lookup"><span data-stu-id="eca46-207">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="eca46-208">Editar la *httpd.conf* archivo:</span><span class="sxs-lookup"><span data-stu-id="eca46-208">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="eca46-209">Agregue la línea `Header set X-Content-Type-Options "nosniff"`.</span><span class="sxs-lookup"><span data-stu-id="eca46-209">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="eca46-210">Guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="eca46-210">Save the file.</span></span> <span data-ttu-id="eca46-211">Reinicie Apache.</span><span class="sxs-lookup"><span data-stu-id="eca46-211">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="eca46-212">Equilibrio de carga</span><span class="sxs-lookup"><span data-stu-id="eca46-212">Load Balancing</span></span> 

<span data-ttu-id="eca46-213">En este ejemplo se muestra cómo instalar y configurar Apache en CentOS 7 y Kestrel en el mismo equipo de la instancia.</span><span class="sxs-lookup"><span data-stu-id="eca46-213">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="eca46-214">Para no tener un único punto de error; usar *mod_proxy_balancer* y modificar el **VirtualHost** permitiría para administrar varias instancias de las aplicaciones web detrás del servidor de proxy de Apache.</span><span class="sxs-lookup"><span data-stu-id="eca46-214">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing mutliple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="eca46-215">En el archivo de configuración se muestra a continuación, una instancia adicional de la `hellomvc` aplicación está configurada para que se ejecute en el puerto 5001.</span><span class="sxs-lookup"><span data-stu-id="eca46-215">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="eca46-216">El *Proxy* sección se establece con una configuración de equilibrador con dos miembros para equilibrar la carga *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="eca46-216">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

```
<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/ [R,L]
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

### <a name="rate-limits"></a><span data-ttu-id="eca46-217">Límites de velocidad</span><span class="sxs-lookup"><span data-stu-id="eca46-217">Rate Limits</span></span>
<span data-ttu-id="eca46-218">Usar *mod_ratelimit*, que se incluye en el *httpd* módulo, el ancho de banda de clientes puede ser limitada:</span><span class="sxs-lookup"><span data-stu-id="eca46-218">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="eca46-219">El archivo de ejemplo limita el ancho de banda como 600 KB/s en la ubicación raíz:</span><span class="sxs-lookup"><span data-stu-id="eca46-219">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```
