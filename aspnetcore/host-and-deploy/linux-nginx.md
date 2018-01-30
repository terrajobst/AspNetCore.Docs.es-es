---
title: Hospedar ASP.NET Core en Linux con Nginx
author: rick-anderson
description: "Describe cómo configurar Nginx como un proxy inverso en Ubuntu 16.04 para reenviar el tráfico HTTP a una aplicación web de ASP.NET Core con Kestrel."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 08/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: 9939e420fee41b11e709da911d4051a048e789b3
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="3f32c-103">Hospedar ASP.NET Core en Linux con Nginx</span><span class="sxs-lookup"><span data-stu-id="3f32c-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="3f32c-104">Por [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="3f32c-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="3f32c-105">En esta guía se explica cómo configurar un entorno de ASP.NET Core listo para producción en un servidor de Ubuntu 16.04.</span><span class="sxs-lookup"><span data-stu-id="3f32c-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 Server.</span></span>

<span data-ttu-id="3f32c-106">**Nota:** para Ubuntu 14.04, *supervisord* se recomienda como una solución para supervisar el proceso de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f32c-106">**Note:** For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="3f32c-107">*systemd* no está disponible en Ubuntu 14.04.</span><span class="sxs-lookup"><span data-stu-id="3f32c-107">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="3f32c-108">[Vea la versión anterior de este documento](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span><span class="sxs-lookup"><span data-stu-id="3f32c-108">[See previous version of this document](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)</span></span>

<span data-ttu-id="3f32c-109">En esta guía:</span><span class="sxs-lookup"><span data-stu-id="3f32c-109">This guide:</span></span>

* <span data-ttu-id="3f32c-110">Coloca una aplicación existente de ASP.NET Core detrás de un servidor proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="3f32c-110">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="3f32c-111">Configura el servidor proxy inverso para reenviar las solicitudes al servidor web Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f32c-111">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="3f32c-112">Garantiza que la aplicación web se ejecuta al iniciar como un demonio.</span><span class="sxs-lookup"><span data-stu-id="3f32c-112">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="3f32c-113">Configura una herramienta de administración de procesos para ayudar a reiniciar la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="3f32c-113">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3f32c-114">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="3f32c-114">Prerequisites</span></span>

1. <span data-ttu-id="3f32c-115">Acceso a un servidor de Ubuntu 16.04 con una cuenta de usuario estándar con privilegios sudo</span><span class="sxs-lookup"><span data-stu-id="3f32c-115">Access to an Ubuntu 16.04 Server with a standard user account with sudo privilege</span></span>
1. <span data-ttu-id="3f32c-116">Una aplicación existente de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f32c-116">An existing ASP.NET Core app</span></span>

## <a name="copy-over-the-app"></a><span data-ttu-id="3f32c-117">Copiar a través de la aplicación</span><span class="sxs-lookup"><span data-stu-id="3f32c-117">Copy over the app</span></span>

<span data-ttu-id="3f32c-118">Ejecute `dotnet publish` desde el entorno de desarrollo para empaquetar una aplicación en un directorio independiente que se puede ejecutar en el servidor.</span><span class="sxs-lookup"><span data-stu-id="3f32c-118">Run `dotnet publish` from the dev environment to package an app into a self-contained directory that can run on the server.</span></span>

<span data-ttu-id="3f32c-119">Copie la aplicación de ASP.NET Core en el servidor mediante cualquier herramienta que se integra en el flujo de trabajo de la organización (por ejemplo, SCP, FTP).</span><span class="sxs-lookup"><span data-stu-id="3f32c-119">Copy the ASP.NET Core app to the server using whatever tool integrates into the organization's workflow (for example, SCP, FTP).</span></span> <span data-ttu-id="3f32c-120">Pruebe la aplicación, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="3f32c-120">Test the app, for example:</span></span>

* <span data-ttu-id="3f32c-121">Desde la línea de comandos, ejecute `dotnet <app_assembly>.dll`.</span><span class="sxs-lookup"><span data-stu-id="3f32c-121">From the command line, run `dotnet <app_assembly>.dll`.</span></span>
* <span data-ttu-id="3f32c-122">En un explorador, vaya a `http://<serveraddress>:<port>` para comprobar que la aplicación funciona en Linux.</span><span class="sxs-lookup"><span data-stu-id="3f32c-122">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux.</span></span> 
 
## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="3f32c-123">Configurar un servidor proxy inverso</span><span class="sxs-lookup"><span data-stu-id="3f32c-123">Configure a reverse proxy server</span></span>

<span data-ttu-id="3f32c-124">Un proxy inverso es una configuración común para servir las aplicaciones web dinámicas.</span><span class="sxs-lookup"><span data-stu-id="3f32c-124">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="3f32c-125">Un proxy inverso finaliza la solicitud HTTP y lo reenvía a la aplicación de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3f32c-125">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

### <a name="why-use-a-reverse-proxy-server"></a><span data-ttu-id="3f32c-126">¿Por qué usar un servidor proxy inverso?</span><span class="sxs-lookup"><span data-stu-id="3f32c-126">Why use a reverse proxy server?</span></span>

<span data-ttu-id="3f32c-127">Kestrel es excelente para servir contenido dinámico de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3f32c-127">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="3f32c-128">Sin embargo, las capacidades de servicio web tienen tantas características como servidores, como IIS, Apache o Nginx.</span><span class="sxs-lookup"><span data-stu-id="3f32c-128">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="3f32c-129">Un servidor proxy inverso puede descargar de trabajo, como servir contenido estático, almacenamiento en caché las solicitudes de la compresión de las solicitudes y la terminación SSL desde el servidor HTTP.</span><span class="sxs-lookup"><span data-stu-id="3f32c-129">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="3f32c-130">Un servidor proxy inverso puede residir en un equipo dedicado o se puede implementar junto con un servidor HTTP.</span><span class="sxs-lookup"><span data-stu-id="3f32c-130">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="3f32c-131">Para los fines de esta guía, se usa una única instancia de Nginx.</span><span class="sxs-lookup"><span data-stu-id="3f32c-131">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="3f32c-132">Se ejecuta en el mismo servidor, junto con el servidor HTTP.</span><span class="sxs-lookup"><span data-stu-id="3f32c-132">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="3f32c-133">En función de requisitos, una instalación diferentes pueden elegirá.</span><span class="sxs-lookup"><span data-stu-id="3f32c-133">Based on requirements, a different setup may be choosen.</span></span>

<span data-ttu-id="3f32c-134">Dado que el proxy inverso reenvía las solicitudes, use el software intermedio `ForwardedHeaders` del paquete `Microsoft.AspNetCore.HttpOverrides`.</span><span class="sxs-lookup"><span data-stu-id="3f32c-134">Because requests are forwarded by reverse proxy, use the `ForwardedHeaders` middleware from the `Microsoft.AspNetCore.HttpOverrides` package.</span></span> <span data-ttu-id="3f32c-135">El software intermedio actualiza `Request.Scheme`, mediante el encabezado `X-Forwarded-Proto`, para que esos URI de redirección y otras directivas de seguridad funcionen correctamente.</span><span class="sxs-lookup"><span data-stu-id="3f32c-135">This middleware updates `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="3f32c-136">Al configurar un servidor proxy inverso, el software intermedio de autenticación necesita que se ejecute `UseForwardedHeaders` primero.</span><span class="sxs-lookup"><span data-stu-id="3f32c-136">When setting up a reverse proxy server, the authentication middleware needs `UseForwardedHeaders` to run first.</span></span> <span data-ttu-id="3f32c-137">Este orden garantiza que el software intermedio de autenticación puede consumir los valores afectados y generar URI de redirección correctos.</span><span class="sxs-lookup"><span data-stu-id="3f32c-137">This ordering ensures that the authentication middleware can consume the affected values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3f32c-138">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3f32c-138">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3f32c-139">Invoque el método `UseForwardedHeaders` (en el método `Configure` de *Startup.cs*) antes de llamar a `UseAuthentication` o un software intermedio de esquema de autenticación similar:</span><span class="sxs-lookup"><span data-stu-id="3f32c-139">Invoke the `UseForwardedHeaders` method (in the `Configure` method of *Startup.cs*) before calling `UseAuthentication` or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3f32c-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3f32c-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3f32c-141">Invoque el método `UseForwardedHeaders` (en el método `Configure` de *Startup.cs*) antes de llamar a `UseIdentity` y `UseFacebookAuthentication` o un software intermedio de esquema de autenticación similar:</span><span class="sxs-lookup"><span data-stu-id="3f32c-141">Invoke the `UseForwardedHeaders` method (in the `Configure` method of *Startup.cs*) before calling `UseIdentity` and `UseFacebookAuthentication` or similar authentication scheme middleware:</span></span>

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

### <a name="install-nginx"></a><span data-ttu-id="3f32c-142">Instalar Nginx</span><span class="sxs-lookup"><span data-stu-id="3f32c-142">Install Nginx</span></span>

```bash
sudo apt-get install nginx
```

> [!NOTE]
> <span data-ttu-id="3f32c-143">Si se instala los módulos de Nginx opcionales, podría ser necesario generar Nginx de origen.</span><span class="sxs-lookup"><span data-stu-id="3f32c-143">If optional Nginx modules will be installed, building Nginx from source might be required.</span></span>

<span data-ttu-id="3f32c-144">Use `apt-get` para instalar Nginx.</span><span class="sxs-lookup"><span data-stu-id="3f32c-144">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="3f32c-145">El instalador crea un script de inicio de System V que ejecuta Nginx como demonio al iniciarse el sistema.</span><span class="sxs-lookup"><span data-stu-id="3f32c-145">The installer creates a System V init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="3f32c-146">Puesto que Nginx se ha instalado por primera vez, ejecute lo siguiente para iniciarlo de forma explícita:</span><span class="sxs-lookup"><span data-stu-id="3f32c-146">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="3f32c-147">Compruebe que un explorador muestra la página de aterrizaje predeterminada de Nginx.</span><span class="sxs-lookup"><span data-stu-id="3f32c-147">Verify a browser displays the default landing page for Nginx.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="3f32c-148">Configurar Nginx</span><span class="sxs-lookup"><span data-stu-id="3f32c-148">Configure Nginx</span></span>

<span data-ttu-id="3f32c-149">Para configurar Nginx como un proxy inverso para reenviar solicitudes a nuestra aplicación de ASP.NET Core, modifique `/etc/nginx/sites-available/default`.</span><span class="sxs-lookup"><span data-stu-id="3f32c-149">To configure Nginx as a reverse proxy to forward requests to our ASP.NET Core app, modify `/etc/nginx/sites-available/default`.</span></span> <span data-ttu-id="3f32c-150">Ábralo en un editor de texto y reemplace el contenido por el siguiente:</span><span class="sxs-lookup"><span data-stu-id="3f32c-150">Open it in a text editor, and replace the contents with the following:</span></span>

```
server {
    listen 80;
    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $http_host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

<span data-ttu-id="3f32c-151">Este archivo de configuración de Nginx reenvía tráfico público entrante del puerto `80` al puerto `5000`.</span><span class="sxs-lookup"><span data-stu-id="3f32c-151">This Nginx configuration file forwards incoming public traffic from port `80` to port `5000`.</span></span>

<span data-ttu-id="3f32c-152">Una vez establecida la configuración de Nginx, ejecute `sudo nginx -t` para comprobar la sintaxis de los archivos de configuración.</span><span class="sxs-lookup"><span data-stu-id="3f32c-152">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="3f32c-153">Si la prueba del archivo de configuración es correcta, forzar Nginx para recoger los cambios mediante la ejecución de `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="3f32c-153">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="3f32c-154">Supervisión de la aplicación</span><span class="sxs-lookup"><span data-stu-id="3f32c-154">Monitoring the app</span></span>

<span data-ttu-id="3f32c-155">El servidor está configurado para reenviar las solicitudes realizadas a `http://<serveraddress>:80` en la aplicación de ASP.NET Core que se ejecuta en Kestrel en `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="3f32c-155">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="3f32c-156">Sin embargo, Nginx no está configurado para administrar el proceso de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f32c-156">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="3f32c-157">*systemd* puede utilizarse para crear un archivo de servicio para iniciar y supervisar la aplicación web subyacente.</span><span class="sxs-lookup"><span data-stu-id="3f32c-157">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="3f32c-158">*systemd* es un sistema de inicio que proporciona muchas características eficaces para iniciar, detener y administrar procesos.</span><span class="sxs-lookup"><span data-stu-id="3f32c-158">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="3f32c-159">Crear el archivo de servicio</span><span class="sxs-lookup"><span data-stu-id="3f32c-159">Create the service file</span></span>

<span data-ttu-id="3f32c-160">Cree el archivo de definición de servicio:</span><span class="sxs-lookup"><span data-stu-id="3f32c-160">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="3f32c-161">El siguiente es un archivo de servicio de ejemplo para la aplicación:</span><span class="sxs-lookup"><span data-stu-id="3f32c-161">The following is an example service file for the app:</span></span>

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
RestartSec=10  # Restart service after 10 seconds if dotnet service crashes
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="3f32c-162">**Nota:** si el usuario *www datos* no se usa por la configuración, el usuario definido aquí se debe crear primero y determinado propiedad adecuada para los archivos.</span><span class="sxs-lookup"><span data-stu-id="3f32c-162">**Note:** If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>
<span data-ttu-id="3f32c-163">**Nota:** Linux tiene un sistema de archivos distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="3f32c-163">**Note:** Linux has a case-sensitive file system.</span></span> <span data-ttu-id="3f32c-164">Si se establece ASPNETCORE_ENVIRONMENT en los resultados de "Producción" en la búsqueda del archivo de configuración *appsettings. Production.JSON*, no *appsettings.production.json*.</span><span class="sxs-lookup"><span data-stu-id="3f32c-164">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

<span data-ttu-id="3f32c-165">Guarde el archivo y habilite el servicio.</span><span class="sxs-lookup"><span data-stu-id="3f32c-165">Save the file and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="3f32c-166">Iniciar el servicio y compruebe que se está ejecutando.</span><span class="sxs-lookup"><span data-stu-id="3f32c-166">Start the service and verify that it's running.</span></span>

```
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="3f32c-167">Con el proxy inverso configurado y Kestrel administrada a través de systemd, la aplicación web esté completamente configurada y puede tener acceso desde un explorador en el equipo local en `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="3f32c-167">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="3f32c-168">También es accesible desde un equipo remoto, bloqueo de cualquier firewall que podría estar bloqueando.</span><span class="sxs-lookup"><span data-stu-id="3f32c-168">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="3f32c-169">Inspeccionar los encabezados de respuesta, el `Server` encabezado muestra la aplicación de ASP.NET Core sea atendida por Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f32c-169">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="3f32c-170">Ver los registros</span><span class="sxs-lookup"><span data-stu-id="3f32c-170">Viewing logs</span></span>

<span data-ttu-id="3f32c-171">Desde la aplicación web utilizando Kestrel se administra con `systemd`, todos los procesos y los eventos se registran en un diario de centralizada.</span><span class="sxs-lookup"><span data-stu-id="3f32c-171">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="3f32c-172">En cambio, este diario incluye todas las entradas de todos los servicios y procesos administrados por `systemd`.</span><span class="sxs-lookup"><span data-stu-id="3f32c-172">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="3f32c-173">Para ver los elementos específicos de `kestrel-hellomvc.service`, use el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="3f32c-173">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="3f32c-174">Para obtener más opciones de filtrado, las opciones de tiempo como `--since today`, `--until 1 hour ago` o una combinación de estas pueden reducir la cantidad de entradas que se devuelven.</span><span class="sxs-lookup"><span data-stu-id="3f32c-174">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="3f32c-175">Protección de la aplicación</span><span class="sxs-lookup"><span data-stu-id="3f32c-175">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="3f32c-176">Habilitar AppArmor</span><span class="sxs-lookup"><span data-stu-id="3f32c-176">Enable AppArmor</span></span>

<span data-ttu-id="3f32c-177">Linux Security Modules (LSM) es un marco que forma parte del núcleo de Linux desde Linux 2.6.</span><span class="sxs-lookup"><span data-stu-id="3f32c-177">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="3f32c-178">LSM admite diferentes implementaciones de los módulos de seguridad.</span><span class="sxs-lookup"><span data-stu-id="3f32c-178">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="3f32c-179">[AppArmor](https://wiki.ubuntu.com/AppArmor) es un LSM que implementa un sistema de control de acceso obligatorio que permite restringir el programa a un conjunto limitado de recursos.</span><span class="sxs-lookup"><span data-stu-id="3f32c-179">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="3f32c-180">Asegúrese de que AppArmor está habilitado y configurado correctamente.</span><span class="sxs-lookup"><span data-stu-id="3f32c-180">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="3f32c-181">Configuración del firewall</span><span class="sxs-lookup"><span data-stu-id="3f32c-181">Configuring the firewall</span></span>

<span data-ttu-id="3f32c-182">Cierre todos los puertos externos que no estén en uso.</span><span class="sxs-lookup"><span data-stu-id="3f32c-182">Close off all external ports that are not in use.</span></span> <span data-ttu-id="3f32c-183">Uncomplicated Firewall (ufw) proporciona un front-end para `iptables` al proporcionar una interfaz de línea de comandos para configurar el firewall.</span><span class="sxs-lookup"><span data-stu-id="3f32c-183">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="3f32c-184">Compruebe que `ufw` está configurado para permitir el tráfico en los puertos que sean necesarios.</span><span class="sxs-lookup"><span data-stu-id="3f32c-184">Verify that `ufw` is configured to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="3f32c-185">Proteger Nginx</span><span class="sxs-lookup"><span data-stu-id="3f32c-185">Securing Nginx</span></span>

<span data-ttu-id="3f32c-186">La distribución predeterminada de Nginx no habilita SSL.</span><span class="sxs-lookup"><span data-stu-id="3f32c-186">The default distribution of Nginx doesn't enable SSL.</span></span> <span data-ttu-id="3f32c-187">Para habilitar características de seguridad adicionales, compile desde el origen.</span><span class="sxs-lookup"><span data-stu-id="3f32c-187">To enable additional security features, build from source.</span></span>

#### <a name="download-the-source-and-install-the-build-dependencies"></a><span data-ttu-id="3f32c-188">Descargar el origen e instalar las dependencias de compilación</span><span class="sxs-lookup"><span data-stu-id="3f32c-188">Download the source and install the build dependencies</span></span>

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download Nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="3f32c-189">Cambiar el nombre de la respuesta de Nginx</span><span class="sxs-lookup"><span data-stu-id="3f32c-189">Change the Nginx response name</span></span>

<span data-ttu-id="3f32c-190">Edite *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="3f32c-190">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a><span data-ttu-id="3f32c-191">Configurar las opciones y la compilación</span><span class="sxs-lookup"><span data-stu-id="3f32c-191">Configure the options and build</span></span>

<span data-ttu-id="3f32c-192">Se necesita la biblioteca PCRE para expresiones regulares.</span><span class="sxs-lookup"><span data-stu-id="3f32c-192">The PCRE library is required for regular expressions.</span></span> <span data-ttu-id="3f32c-193">Las expresiones regulares se usan en la directiva de ubicación de ngx_http_rewrite_module.</span><span class="sxs-lookup"><span data-stu-id="3f32c-193">Regular expressions are used in the location directive for the ngx_http_rewrite_module.</span></span> <span data-ttu-id="3f32c-194">http_ssl_module agrega compatibilidad con el protocolo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3f32c-194">The http_ssl_module adds HTTPS protocol support.</span></span>

<span data-ttu-id="3f32c-195">Considere el uso de un firewall de aplicación web como *ModSecurity* para proteger la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3f32c-195">Consider using a web app firewall like *ModSecurity* to harden the app.</span></span>

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a><span data-ttu-id="3f32c-196">Configurar SSL</span><span class="sxs-lookup"><span data-stu-id="3f32c-196">Configure SSL</span></span>

* <span data-ttu-id="3f32c-197">Configurar el servidor para que escuche el tráfico HTTPS en el puerto `443` mediante la especificación de un certificado válido emitido por una entidad de confianza de certificados (CA).</span><span class="sxs-lookup"><span data-stu-id="3f32c-197">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="3f32c-198">Reforzar la seguridad mediante el uso de algunos de los procedimientos descritos en la siguiente */etc/nginx/nginx.conf* archivo.</span><span class="sxs-lookup"><span data-stu-id="3f32c-198">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="3f32c-199">Entre los ejemplos se incluye la elección de un cifrado más seguro y el redireccionamiento de todo el tráfico a través de HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3f32c-199">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="3f32c-200">Agregar un encabezado `HTTP Strict-Transport-Security` (HSTS) garantiza que todas las solicitudes siguientes realizadas por el cliente sean solo a través de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3f32c-200">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="3f32c-201">No agregue el encabezado de seguridad de transporte Strict o elige una adecuada `max-age` si SSL se deshabilitará en el futuro.</span><span class="sxs-lookup"><span data-stu-id="3f32c-201">Don't add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="3f32c-202">Agregue el archivo de configuración */etc/nginx/proxy.conf*:</span><span class="sxs-lookup"><span data-stu-id="3f32c-202">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[Main](linux-nginx/proxy.conf)]

<span data-ttu-id="3f32c-203">Edite el archivo de configuración */etc/nginx/nginx.conf*.</span><span class="sxs-lookup"><span data-stu-id="3f32c-203">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="3f32c-204">El ejemplo contiene las dos secciones `http` y `server` en un archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="3f32c-204">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[Main](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="3f32c-205">Proteger Nginx frente al secuestro de clic</span><span class="sxs-lookup"><span data-stu-id="3f32c-205">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="3f32c-206">El secuestro de clic es una técnica malintencionada para recopilar los clics de un usuario infectado.</span><span class="sxs-lookup"><span data-stu-id="3f32c-206">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="3f32c-207">El secuestro de clic engaña a la víctima (visitante) para que haga clic en un sitio infectado.</span><span class="sxs-lookup"><span data-stu-id="3f32c-207">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="3f32c-208">X-FRAME-OPTIONS de la utilice para proteger el sitio.</span><span class="sxs-lookup"><span data-stu-id="3f32c-208">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="3f32c-209">Edite el archivo *nginx.conf*:</span><span class="sxs-lookup"><span data-stu-id="3f32c-209">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="3f32c-210">Agregue la línea `add_header X-Frame-Options "SAMEORIGIN";` y guarde el archivo, después, reinicie Nginx.</span><span class="sxs-lookup"><span data-stu-id="3f32c-210">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="3f32c-211">Examen de tipo MIME</span><span class="sxs-lookup"><span data-stu-id="3f32c-211">MIME-type sniffing</span></span>

<span data-ttu-id="3f32c-212">Este encabezado evita que la mayoría de los exploradores examinen el MIME de una respuesta fuera del tipo de contenido declarado, ya que el encabezado indica al explorador que no reemplace el tipo de contenido de respuesta.</span><span class="sxs-lookup"><span data-stu-id="3f32c-212">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="3f32c-213">Con la opción `nosniff`, si el servidor indica que el contenido es "text/html", el explorador lo representa como "text/html".</span><span class="sxs-lookup"><span data-stu-id="3f32c-213">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="3f32c-214">Edite el archivo *nginx.conf*:</span><span class="sxs-lookup"><span data-stu-id="3f32c-214">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="3f32c-215">Agregue la línea `add_header X-Content-Type-Options "nosniff";`, guarde el archivo y después reinicie Nginx.</span><span class="sxs-lookup"><span data-stu-id="3f32c-215">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>
