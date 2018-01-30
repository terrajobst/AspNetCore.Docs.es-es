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
ms.openlocfilehash: aa55ecd6dc8169e0e77b3899389ec924b1e1ae4a
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="ee10e-103">Hospedar ASP.NET Core en Linux con Apache</span><span class="sxs-lookup"><span data-stu-id="ee10e-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="ee10e-104">Por [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="ee10e-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="ee10e-105">Con esta guía, obtenga información acerca de cómo configurar [Apache](https://httpd.apache.org/) como un servidor proxy inverso en [CentOS 7](https://www.centos.org/) para redirigir el tráfico HTTP a una aplicación web de ASP.NET Core con [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="ee10e-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="ee10e-106">El [mod_proxy extensión](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) y módulos relacionados Crear proxy inverso del servidor.</span><span class="sxs-lookup"><span data-stu-id="ee10e-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ee10e-107">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="ee10e-107">Prerequisites</span></span>

1. <span data-ttu-id="ee10e-108">Servidor que ejecuta CentOS 7 con una cuenta de usuario estándar con privilegios sudo</span><span class="sxs-lookup"><span data-stu-id="ee10e-108">Server running CentOS 7 with a standard user account with sudo privilege</span></span>
2. <span data-ttu-id="ee10e-109">Aplicación de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ee10e-109">ASP.NET Core app</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="ee10e-110">Publicar la aplicación</span><span class="sxs-lookup"><span data-stu-id="ee10e-110">Publish the app</span></span>

<span data-ttu-id="ee10e-111">Publicar la aplicación como un [implementación independiente](/dotnet/core/deploying/#self-contained-deployments-scd) en configuración de lanzamiento para el tiempo de ejecución de CentOS 7 (`centos.7-x64`).</span><span class="sxs-lookup"><span data-stu-id="ee10e-111">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) in Release configuration for the CentOS 7 runtime (`centos.7-x64`).</span></span> <span data-ttu-id="ee10e-112">Copie el contenido de la *bin/Release/netcoreapp2.0/centos.7-x64/publish* carpeta al servidor mediante el SCP, FTP u otro método de transferencia de archivos.</span><span class="sxs-lookup"><span data-stu-id="ee10e-112">Copy the contents of the *bin/Release/netcoreapp2.0/centos.7-x64/publish* folder to the server using SCP, FTP, or other file transfer method.</span></span>

> [!NOTE]
> <span data-ttu-id="ee10e-113">En un escenario de implementación de producción, un flujo de trabajo de integración continua realiza el trabajo de publicación de la aplicación y copiar los activos en el servidor.</span><span class="sxs-lookup"><span data-stu-id="ee10e-113">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="ee10e-114">Configurar un servidor proxy</span><span class="sxs-lookup"><span data-stu-id="ee10e-114">Configure a proxy server</span></span>

<span data-ttu-id="ee10e-115">Un proxy inverso es una configuración común para servir las aplicaciones web dinámicas.</span><span class="sxs-lookup"><span data-stu-id="ee10e-115">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="ee10e-116">El proxy inverso finaliza la solicitud HTTP y lo reenvía a la aplicación ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ee10e-116">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="ee10e-117">Los servidores proxy son los que reenvían las solicitudes de cliente a otro servidor en lugar de realizarlas ellos mismos.</span><span class="sxs-lookup"><span data-stu-id="ee10e-117">A proxy server is one which forwards client requests to another server instead of fulfilling them itself.</span></span> <span data-ttu-id="ee10e-118">Los proxies inversos las reenvían a un destino fijo, normalmente en nombre de clientes arbitrarios.</span><span class="sxs-lookup"><span data-stu-id="ee10e-118">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="ee10e-119">En esta guía, Apache está configurado como el proxy inverso que se ejecuta en el mismo servidor que Kestrel sigue dando servicio a la aplicación de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ee10e-119">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

### <a name="install-apache"></a><span data-ttu-id="ee10e-120">Instalar Apache</span><span class="sxs-lookup"><span data-stu-id="ee10e-120">Install Apache</span></span>

<span data-ttu-id="ee10e-121">Actualizar paquetes de CentOS a sus versiones estables más recientes:</span><span class="sxs-lookup"><span data-stu-id="ee10e-121">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="ee10e-122">Instalar el servidor web Apache en CentOS con una sola `yum` comando:</span><span class="sxs-lookup"><span data-stu-id="ee10e-122">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="ee10e-123">Resultado después de ejecutar el comando de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ee10e-123">Sample output after running the command:</span></span>

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
> <span data-ttu-id="ee10e-124">En este ejemplo, el resultado refleja httpd.86_64 puesto que la versión de CentOS 7 es de 64 bits.</span><span class="sxs-lookup"><span data-stu-id="ee10e-124">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="ee10e-125">Para comprobar dónde está instalado Apache, ejecute `whereis httpd` desde un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="ee10e-125">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span> 

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="ee10e-126">Configurar Apache para un proxy inverso</span><span class="sxs-lookup"><span data-stu-id="ee10e-126">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="ee10e-127">Los archivos de configuración de Apache se encuentran en el directorio `/etc/httpd/conf.d/`.</span><span class="sxs-lookup"><span data-stu-id="ee10e-127">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="ee10e-128">Cualquier archivo con el *.conf* extensión se procesa en orden alfabético además de los archivos de configuración de módulo en `/etc/httpd/conf.modules.d/`, que contiene cualquier configuración de los archivos necesario para cargar los módulos.</span><span class="sxs-lookup"><span data-stu-id="ee10e-128">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="ee10e-129">Crear un archivo de configuración de la aplicación denominada `hellomvc.conf`:</span><span class="sxs-lookup"><span data-stu-id="ee10e-129">Create a configuration file for the app named `hellomvc.conf`:</span></span>

```
<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
</VirtualHost>
```

<span data-ttu-id="ee10e-130">El **VirtualHost** nodo puede aparecer varias veces en uno o varios archivos en un servidor.</span><span class="sxs-lookup"><span data-stu-id="ee10e-130">The **VirtualHost** node can appear multiple times in one or more files on a server.</span></span> <span data-ttu-id="ee10e-131">**VirtualHost** se establece para que escuche en cualquier dirección IP que usa el puerto 80.</span><span class="sxs-lookup"><span data-stu-id="ee10e-131">**VirtualHost** is set to listen on any IP address using port 80.</span></span> <span data-ttu-id="ee10e-132">Las dos líneas siguientes se establecen en las solicitudes de proxy en el directorio raíz en el servidor en 127.0.0.1 en el puerto 5000.</span><span class="sxs-lookup"><span data-stu-id="ee10e-132">The next two lines are set to proxy requests at the root to the server at 127.0.0.1 on port 5000.</span></span> <span data-ttu-id="ee10e-133">Para la comunicación bidireccional, *ProxyPass* y *ProxyPassReverse* son necesarios.</span><span class="sxs-lookup"><span data-stu-id="ee10e-133">For bi-directional communication, *ProxyPass* and *ProxyPassReverse* are required.</span></span>

<span data-ttu-id="ee10e-134">Se puede configurar el registro por **VirtualHost** con **ErrorLog** y **CustomLog** directivas.</span><span class="sxs-lookup"><span data-stu-id="ee10e-134">Logging can be configured per **VirtualHost** using **ErrorLog** and **CustomLog** directives.</span></span> <span data-ttu-id="ee10e-135">**Registro de errores** es la ubicación donde el servidor registra errores, y **CustomLog** establece el nombre de archivo y el formato de archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="ee10e-135">**ErrorLog** is the location where the server logs errors, and **CustomLog** sets the filename and format of log file.</span></span> <span data-ttu-id="ee10e-136">En este caso, esto es donde se registra la información de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="ee10e-136">In this case, this is where request information is logged.</span></span> <span data-ttu-id="ee10e-137">Hay una línea para cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="ee10e-137">There's one line for each request.</span></span>

<span data-ttu-id="ee10e-138">Guarde el archivo y la configuración de pruebas.</span><span class="sxs-lookup"><span data-stu-id="ee10e-138">Save the file and test the configuration.</span></span> <span data-ttu-id="ee10e-139">Si se pasa todo, la respuesta debe ser `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="ee10e-139">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="ee10e-140">Reinicie Apache:</span><span class="sxs-lookup"><span data-stu-id="ee10e-140">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="ee10e-141">Supervisión de la aplicación</span><span class="sxs-lookup"><span data-stu-id="ee10e-141">Monitoring the app</span></span>

<span data-ttu-id="ee10e-142">Apache ahora está configurado para reenviar las solicitudes realizadas a `http://localhost:80` a la aplicación de ASP.NET Core con Kestrel en `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="ee10e-142">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="ee10e-143">Sin embargo, Apache no está configurado para administrar el proceso de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ee10e-143">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="ee10e-144">Use *systemd* y cree un archivo de servicio para iniciar y supervisar la aplicación web subyacente.</span><span class="sxs-lookup"><span data-stu-id="ee10e-144">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="ee10e-145">*systemd* es un sistema de inicio que proporciona muchas características eficaces para iniciar, detener y administrar procesos.</span><span class="sxs-lookup"><span data-stu-id="ee10e-145">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="ee10e-146">Crear el archivo de servicio</span><span class="sxs-lookup"><span data-stu-id="ee10e-146">Create the service file</span></span>

<span data-ttu-id="ee10e-147">Cree el archivo de definición de servicio:</span><span class="sxs-lookup"><span data-stu-id="ee10e-147">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="ee10e-148">Un archivo de servicio de ejemplo para la aplicación:</span><span class="sxs-lookup"><span data-stu-id="ee10e-148">An example service file for the app:</span></span>

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
> <span data-ttu-id="ee10e-149">**Usuario** &mdash; si el usuario *apache* no se usa la configuración, el usuario debe crear primero y determinado propiedad adecuada para los archivos.</span><span class="sxs-lookup"><span data-stu-id="ee10e-149">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="ee10e-150">Guarde el archivo y habilitar el servicio:</span><span class="sxs-lookup"><span data-stu-id="ee10e-150">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="ee10e-151">Iniciar el servicio y compruebe que se está ejecutando:</span><span class="sxs-lookup"><span data-stu-id="ee10e-151">Start the service and verify that it's running:</span></span>

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

<span data-ttu-id="ee10e-152">Con el proxy inverso configurado y Kestrel administrada a través de *systemd*, la aplicación web está configurada completamente y son accesibles desde un explorador en el equipo local en `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="ee10e-152">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="ee10e-153">Inspeccionar los encabezados de respuesta, el **Server** encabezado indica que la aplicación de ASP.NET Core atendida por Kestrel:</span><span class="sxs-lookup"><span data-stu-id="ee10e-153">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="ee10e-154">Ver los registros</span><span class="sxs-lookup"><span data-stu-id="ee10e-154">Viewing logs</span></span>

<span data-ttu-id="ee10e-155">Desde la aplicación web utilizando Kestrel se administra con *systemd*, procesos y eventos se registran en un diario de centralizada.</span><span class="sxs-lookup"><span data-stu-id="ee10e-155">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="ee10e-156">Sin embargo, este diario incluye entradas para todos los servicios y procesos administrados por *systemd*.</span><span class="sxs-lookup"><span data-stu-id="ee10e-156">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="ee10e-157">Para ver los elementos específicos de `kestrel-hellomvc.service`, use el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="ee10e-157">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="ee10e-158">Para filtrar el tiempo, especifique las opciones de tiempo con el comando.</span><span class="sxs-lookup"><span data-stu-id="ee10e-158">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="ee10e-159">Por ejemplo, utilice `--since today` para filtrar en la actualidad o `--until 1 hour ago` para ver las entradas de la hora anterior.</span><span class="sxs-lookup"><span data-stu-id="ee10e-159">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="ee10e-160">Para obtener más información, consulte el [página del comando man journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="ee10e-160">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="ee10e-161">Protección de la aplicación</span><span class="sxs-lookup"><span data-stu-id="ee10e-161">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="ee10e-162">Configurar el firewall</span><span class="sxs-lookup"><span data-stu-id="ee10e-162">Configure firewall</span></span>

<span data-ttu-id="ee10e-163">*Firewalld* es un demonio dinámico para administrar el firewall con compatibilidad para las zonas de red.</span><span class="sxs-lookup"><span data-stu-id="ee10e-163">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="ee10e-164">Puertos y el filtrado de paquetes pueden seguir administrándose mediante iptables.</span><span class="sxs-lookup"><span data-stu-id="ee10e-164">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="ee10e-165">*Firewalld* debe instalarse de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="ee10e-165">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="ee10e-166">`yum`puede utilizarse para instalar el paquete o compruebe que está instalado.</span><span class="sxs-lookup"><span data-stu-id="ee10e-166">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="ee10e-167">Use `firewalld` para abrir sólo los puertos necesarios para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ee10e-167">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="ee10e-168">En este caso se usan los puertos 80 y 443.</span><span class="sxs-lookup"><span data-stu-id="ee10e-168">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="ee10e-169">Los siguientes comandos de forma permanente establecen los puertos 80 y 443 para abrir:</span><span class="sxs-lookup"><span data-stu-id="ee10e-169">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="ee10e-170">Vuelva a cargar la configuración del firewall.</span><span class="sxs-lookup"><span data-stu-id="ee10e-170">Reload the firewall settings.</span></span> <span data-ttu-id="ee10e-171">Compruebe los servicios disponibles y los puertos en la zona predeterminada.</span><span class="sxs-lookup"><span data-stu-id="ee10e-171">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="ee10e-172">Opciones están disponibles mediante la inspección `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="ee10e-172">Options are available by inspecting `firewall-cmd -h`.</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="ee10e-173">Configuración de SSL</span><span class="sxs-lookup"><span data-stu-id="ee10e-173">SSL configuration</span></span>

<span data-ttu-id="ee10e-174">Para configurar Apache para SSL, el *mod_ssl* módulo se usa.</span><span class="sxs-lookup"><span data-stu-id="ee10e-174">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="ee10e-175">Cuando el *httpd* se instaló el módulo, el *mod_ssl* también se instala el módulo.</span><span class="sxs-lookup"><span data-stu-id="ee10e-175">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="ee10e-176">Si aún no se ha instalado, use `yum` para agregarlo a la configuración.</span><span class="sxs-lookup"><span data-stu-id="ee10e-176">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```
<span data-ttu-id="ee10e-177">Para utilizar SSL, instalar el `mod_rewrite` módulo para habilitar la reescritura de direcciones URL:</span><span class="sxs-lookup"><span data-stu-id="ee10e-177">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="ee10e-178">Modificar el *hellomvc.conf* archivo para habilitar la reescritura de direcciones URL y proteger la comunicación en el puerto 443:</span><span class="sxs-lookup"><span data-stu-id="ee10e-178">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

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
> <span data-ttu-id="ee10e-179">En este ejemplo se utiliza un certificado generado localmente.</span><span class="sxs-lookup"><span data-stu-id="ee10e-179">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="ee10e-180">**SSLCertificateFile** debe ser el archivo de certificado principal para el nombre de dominio.</span><span class="sxs-lookup"><span data-stu-id="ee10e-180">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="ee10e-181">**SSLCertificateKeyFile** debe ser el archivo de clave generar cuando se crea el CSR.</span><span class="sxs-lookup"><span data-stu-id="ee10e-181">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="ee10e-182">**SSLCertificateChainFile** debe ser el archivo de certificado intermedio (si existe) que se ha proporcionado por la entidad de certificación.</span><span class="sxs-lookup"><span data-stu-id="ee10e-182">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="ee10e-183">Guarde el archivo y la configuración de prueba:</span><span class="sxs-lookup"><span data-stu-id="ee10e-183">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="ee10e-184">Reinicie Apache:</span><span class="sxs-lookup"><span data-stu-id="ee10e-184">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="ee10e-185">Sugerencias adicionales de Apache</span><span class="sxs-lookup"><span data-stu-id="ee10e-185">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="ee10e-186">Encabezados adicionales</span><span class="sxs-lookup"><span data-stu-id="ee10e-186">Additional headers</span></span>

<span data-ttu-id="ee10e-187">Para proteger frente a ataques malintencionados, hay algunos encabezados que deben ser modificados o agregados.</span><span class="sxs-lookup"><span data-stu-id="ee10e-187">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="ee10e-188">Asegúrese de que el `mod_headers` módulo está instalado:</span><span class="sxs-lookup"><span data-stu-id="ee10e-188">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="ee10e-189">Apache seguro frente a ataques de clickjacking</span><span class="sxs-lookup"><span data-stu-id="ee10e-189">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="ee10e-190">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), también conocida como un *UI reclamación ataque*, es un ataque malintencionado donde el visitante de un sitio Web engañar al hacer clic en un vínculo o botón en una página distinta que actualmente está visitando.</span><span class="sxs-lookup"><span data-stu-id="ee10e-190">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="ee10e-191">Use `X-FRAME-OPTIONS` para proteger el sitio.</span><span class="sxs-lookup"><span data-stu-id="ee10e-191">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="ee10e-192">Editar la *httpd.conf* archivo:</span><span class="sxs-lookup"><span data-stu-id="ee10e-192">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="ee10e-193">Agregue la línea `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span><span class="sxs-lookup"><span data-stu-id="ee10e-193">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="ee10e-194">Guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="ee10e-194">Save the file.</span></span> <span data-ttu-id="ee10e-195">Reinicie Apache.</span><span class="sxs-lookup"><span data-stu-id="ee10e-195">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="ee10e-196">Examen de tipo MIME</span><span class="sxs-lookup"><span data-stu-id="ee10e-196">MIME-type sniffing</span></span>

<span data-ttu-id="ee10e-197">El `X-Content-Type-Options` encabezado impide que Internet Explorer *examen de MIME* (determinar un archivo `Content-Type` desde el contenido del archivo).</span><span class="sxs-lookup"><span data-stu-id="ee10e-197">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determing a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="ee10e-198">Si el servidor establece la `Content-Type` encabezado a `text/html` con el `nosniff` representa el conjunto de opciones, Internet Explorer el contenido como `text/html` sin tener en cuenta el contenido del archivo.</span><span class="sxs-lookup"><span data-stu-id="ee10e-198">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="ee10e-199">Editar la *httpd.conf* archivo:</span><span class="sxs-lookup"><span data-stu-id="ee10e-199">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="ee10e-200">Agregue la línea `Header set X-Content-Type-Options "nosniff"`.</span><span class="sxs-lookup"><span data-stu-id="ee10e-200">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="ee10e-201">Guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="ee10e-201">Save the file.</span></span> <span data-ttu-id="ee10e-202">Reinicie Apache.</span><span class="sxs-lookup"><span data-stu-id="ee10e-202">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="ee10e-203">Equilibrio de carga</span><span class="sxs-lookup"><span data-stu-id="ee10e-203">Load Balancing</span></span> 

<span data-ttu-id="ee10e-204">En este ejemplo se muestra cómo instalar y configurar Apache en CentOS 7 y Kestrel en el mismo equipo de la instancia.</span><span class="sxs-lookup"><span data-stu-id="ee10e-204">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="ee10e-205">Para no tener un único punto de error; usar *mod_proxy_balancer* y modificar el **VirtualHost** permitiría para administrar varias instancias de las aplicaciones web detrás del servidor de proxy de Apache.</span><span class="sxs-lookup"><span data-stu-id="ee10e-205">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing mutliple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="ee10e-206">En el archivo de configuración se muestra a continuación, una instancia adicional de la `hellomvc` aplicación está configurada para que se ejecute en el puerto 5001.</span><span class="sxs-lookup"><span data-stu-id="ee10e-206">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="ee10e-207">El *Proxy* sección se establece con una configuración de equilibrador con dos miembros para equilibrar la carga *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="ee10e-207">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

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

### <a name="rate-limits"></a><span data-ttu-id="ee10e-208">Límites de velocidad</span><span class="sxs-lookup"><span data-stu-id="ee10e-208">Rate Limits</span></span>
<span data-ttu-id="ee10e-209">Usar *mod_ratelimit*, que se incluye en el *httpd* módulo, el ancho de banda de clientes puede ser limitada:</span><span class="sxs-lookup"><span data-stu-id="ee10e-209">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="ee10e-210">El archivo de ejemplo limita el ancho de banda como 600 KB/s en la ubicación raíz:</span><span class="sxs-lookup"><span data-stu-id="ee10e-210">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```
