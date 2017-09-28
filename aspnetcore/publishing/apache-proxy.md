---
title: Hospedar ASP.NET Core en Linux con Apache
description: "Aprenda a configurar Apache como servidor proxy inverso en CentOS para redirigir el tráfico HTTP a una aplicación web de ASP.NET Core ejecutándose en Kestrel."
keywords: ASP.NET Core,Apache,CentOS,proxy inverso,Linux,mod_proxy,httpd,hospedaje
author: spboyer
ms.author: spboyer
manager: wpickett
ms.date: 10/19/2016
ms.topic: article
ms.assetid: fa9b0cb7-afb3-4361-9e7e-33afffeaca0c
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/apache-proxy
ms.openlocfilehash: 34ede2fdebe0e9516f62e39618e7adba3c8c89db
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2017
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-apache-and-deploy-to-it"></a><span data-ttu-id="67f55-104">Configurar un entorno de hospedaje para ASP.NET Core en Linux con Apache e implementar en él</span><span class="sxs-lookup"><span data-stu-id="67f55-104">Set up a hosting environment for ASP.NET Core on Linux with Apache, and deploy to it</span></span>

<span data-ttu-id="67f55-105">Por [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="67f55-105">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="67f55-106">Apache es un servidor HTTP muy conocido que se puede configurar como proxy para redirigir el tráfico HTTP similar a nginx.</span><span class="sxs-lookup"><span data-stu-id="67f55-106">Apache is a very popular HTTP server and can be configured as a proxy to redirect HTTP traffic similar to nginx.</span></span> <span data-ttu-id="67f55-107">En esta guía aprenderá a configurar Apache en CentOS 7 y a usarlo como proxy inverso para aceptar conexiones entrantes y redirigirlas a la aplicación de ASP.NET Core que se ejecuta en Kestrel.</span><span class="sxs-lookup"><span data-stu-id="67f55-107">In this guide, we will learn how to set up Apache on CentOS 7 and use it as a reverse proxy to welcome incoming connections and redirect them to the ASP.NET Core application running on Kestrel.</span></span> <span data-ttu-id="67f55-108">Para ello usaremos la extensión *mod_proxy* y otros módulos de Apache relacionados.</span><span class="sxs-lookup"><span data-stu-id="67f55-108">For this purpose, we will use the *mod_proxy* extension and other related Apache modules.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="67f55-109">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="67f55-109">Prerequisites</span></span>

1. <span data-ttu-id="67f55-110">Un servidor que ejecute CentOS 7, con una cuenta de usuario estándar con privilegios sudo.</span><span class="sxs-lookup"><span data-stu-id="67f55-110">A server running CentOS 7, with a standard user account with sudo privilege.</span></span>
2. <span data-ttu-id="67f55-111">Una aplicación de ASP.NET Core existente.</span><span class="sxs-lookup"><span data-stu-id="67f55-111">An existing ASP.NET Core application.</span></span> 

## <a name="publish-your-application"></a><span data-ttu-id="67f55-112">Publicar su aplicación</span><span class="sxs-lookup"><span data-stu-id="67f55-112">Publish your application</span></span>

<span data-ttu-id="67f55-113">Ejecute `dotnet publish -c Release` desde el entorno de desarrollo para empaquetar la aplicación en un directorio independiente que se pueda ejecutar en el servidor.</span><span class="sxs-lookup"><span data-stu-id="67f55-113">Run `dotnet publish -c Release` from your development environment to package your application into a self-contained directory that can run on your server.</span></span> <span data-ttu-id="67f55-114">Luego se debe copiar la aplicación publicada en el servidor mediante SCP, FTP u otro método de transferencia de archivos.</span><span class="sxs-lookup"><span data-stu-id="67f55-114">The published application must then be copied to the server using SCP, FTP or other file transfer method.</span></span> 

> [!NOTE]
> <span data-ttu-id="67f55-115">En un escenario de implementación de producción, un flujo de trabajo de integración continuo lleva a cabo la tarea de publicar la aplicación y copiar los recursos en el servidor.</span><span class="sxs-lookup"><span data-stu-id="67f55-115">Under a production deployment scenario, a continuous integration workflow does the work of publishing the application and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="67f55-116">Configurar un servidor proxy</span><span class="sxs-lookup"><span data-stu-id="67f55-116">Configure a proxy server</span></span>

<span data-ttu-id="67f55-117">Un proxy inverso es una configuración común para usar aplicaciones web dinámicas.</span><span class="sxs-lookup"><span data-stu-id="67f55-117">A reverse proxy is a common setup for serving dynamic web applications.</span></span> <span data-ttu-id="67f55-118">El proxy inverso finaliza la solicitud HTTP y la reenvía a la aplicación de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="67f55-118">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET application.</span></span>

<span data-ttu-id="67f55-119">Los servidores proxy son los que reenvían las solicitudes de cliente a otro servidor en lugar de realizarlas ellos mismos.</span><span class="sxs-lookup"><span data-stu-id="67f55-119">A proxy server is one which forwards client requests to another server instead of fulfilling them itself.</span></span> <span data-ttu-id="67f55-120">Los proxies inversos las reenvían a un destino fijo, normalmente en nombre de clientes arbitrarios.</span><span class="sxs-lookup"><span data-stu-id="67f55-120">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="67f55-121">En esta guía, Apache se configura como proxy inverso que se ejecuta en el mismo servidor en el que Kestrel da servicio a la aplicación de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="67f55-121">In this guide, Apache is being configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core application.</span></span> 

<span data-ttu-id="67f55-122">Cada elemento de la aplicación puede residir en equipos físicos diferentes, en contenedores de Docker o en una combinación de configuraciones según sus necesidades o restricciones de arquitectura.</span><span class="sxs-lookup"><span data-stu-id="67f55-122">Each piece of the application can exist on separate physical machines, Docker containers, or a combination of configurations depending on your architectural needs or restrictions.</span></span>

### <a name="install-apache"></a><span data-ttu-id="67f55-123">Instalar Apache</span><span class="sxs-lookup"><span data-stu-id="67f55-123">Install Apache</span></span>

<span data-ttu-id="67f55-124">Para instalar el servidor web de Apache en CentOS se usa un solo comando, pero primero vamos a actualizar los paquetes.</span><span class="sxs-lookup"><span data-stu-id="67f55-124">Installing the Apache web server on CentOS is a single command, but first let's update our packages.</span></span>

```bash
    sudo yum update -y
```

<span data-ttu-id="67f55-125">Esto garantiza que todos los paquetes instalados se actualicen a la versión más reciente.</span><span class="sxs-lookup"><span data-stu-id="67f55-125">This ensures that all of the installed packages are updated to their latest version.</span></span> <span data-ttu-id="67f55-126">Instalar Apache mediante `yum`</span><span class="sxs-lookup"><span data-stu-id="67f55-126">Install Apache using `yum`</span></span>

```bash
    sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="67f55-127">La salida debe reflejar algo parecido a lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="67f55-127">The output should reflect something similar to the following.</span></span>

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
> <span data-ttu-id="67f55-128">En este ejemplo, la salida refleja httpd.86_64, puesto que la versión de CentOS 7 es de 64 bits.</span><span class="sxs-lookup"><span data-stu-id="67f55-128">In this example the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="67f55-129">La salida podría ser diferente para su servidor.</span><span class="sxs-lookup"><span data-stu-id="67f55-129">The output may be different for your server.</span></span> <span data-ttu-id="67f55-130">Para comprobar dónde está instalado Apache, ejecute `whereis httpd` desde un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="67f55-130">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span> 

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="67f55-131">Configurar Apache para un proxy inverso</span><span class="sxs-lookup"><span data-stu-id="67f55-131">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="67f55-132">Los archivos de configuración de Apache se encuentran en el directorio `/etc/httpd/conf.d/`.</span><span class="sxs-lookup"><span data-stu-id="67f55-132">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="67f55-133">Todos los archivos que tengan la extensión **.conf** se procesarán en orden alfabético, además de los archivos de configuración del módulo de `/etc/httpd/conf.modules.d/`, que contiene todos los archivos de configuración necesarios para cargar los módulos.</span><span class="sxs-lookup"><span data-stu-id="67f55-133">Any file with the **.conf** extension will be processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="67f55-134">Cree un archivo de configuración para la aplicación. En este ejemplo lo llamaremos `hellomvc.conf`.</span><span class="sxs-lookup"><span data-stu-id="67f55-134">Create a configuration file for your app, for this example we'll call it `hellomvc.conf`</span></span>

```text
    <VirtualHost *:80>
        ProxyPreserveHost On
        ProxyPass / http://127.0.0.1:5000/
        ProxyPassReverse / http://127.0.0.1:5000/
        ErrorLog /var/log/httpd/hellomvc-error.log
        CustomLog /var/log/httpd/hellomvc-access.log common
    </VirtualHost>
```

<span data-ttu-id="67f55-135">El nodo *VirtualHost*, del que puede haber varios en un archivo o en muchos archivos de un servidor, está configurado para escuchar en cualquier dirección IP con el puerto 80.</span><span class="sxs-lookup"><span data-stu-id="67f55-135">The *VirtualHost* node, of which there can be multiple in a file or on a server in many files, is set to listen on any IP address using port 80.</span></span> <span data-ttu-id="67f55-136">Las dos líneas siguientes se establecen para pasar todas las solicitudes recibidas en la raíz al equipo 127.0.0.1, puerto 5000, y en orden inverso.</span><span class="sxs-lookup"><span data-stu-id="67f55-136">The next two lines are set to pass all requests received at the root to the machine 127.0.0.1 port 5000 and in reverse.</span></span> <span data-ttu-id="67f55-137">Para que haya una comunicación bidireccional, las opciones *ProxyPass* y *ProxyPassReverse* son necesarias.</span><span class="sxs-lookup"><span data-stu-id="67f55-137">For there to be bi-directional communication, both settings *ProxyPass* and *ProxyPassReverse* are required.</span></span>

<span data-ttu-id="67f55-138">El registro se puede configurar por VirtualHost mediante las directivas *ErrorLog* y *CustomLog*.</span><span class="sxs-lookup"><span data-stu-id="67f55-138">Logging can be configured per VirtualHost using *ErrorLog* and *CustomLog* directives.</span></span> <span data-ttu-id="67f55-139">*ErrorLog* es la ubicación en la que el servidor registrará los errores y *CustomLog* establece el nombre de archivo y el formato del archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="67f55-139">*ErrorLog* is the location where the server will log errors and *CustomLog* sets the filename and format of log file.</span></span> <span data-ttu-id="67f55-140">En nuestro caso, aquí se registrará la información de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="67f55-140">In our case this is where request information will be logged.</span></span> <span data-ttu-id="67f55-141">Habrá una línea para cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="67f55-141">There will be one line for each request.</span></span>

<span data-ttu-id="67f55-142">Guarde el archivo y pruebe la configuración.</span><span class="sxs-lookup"><span data-stu-id="67f55-142">Save the file, and test the configuration.</span></span> <span data-ttu-id="67f55-143">Si se pasa todo, la respuesta debe ser `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="67f55-143">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
    sudo service httpd configtest
```

<span data-ttu-id="67f55-144">Reinicie Apache.</span><span class="sxs-lookup"><span data-stu-id="67f55-144">Restart Apache.</span></span>

```text
    sudo systemctl restart httpd
    sudo systemctl enable httpd
```

## <a name="monitoring-our-application"></a><span data-ttu-id="67f55-145">Supervisar la aplicación</span><span class="sxs-lookup"><span data-stu-id="67f55-145">Monitoring our application</span></span>

<span data-ttu-id="67f55-146">Apache ahora está configurado para reenviar las solicitudes efectuadas a `http://localhost:80` en la aplicación de ASP.NET Core que se ejecuta en Kestrel en `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="67f55-146">Apache is now setup to forward requests made to `http://localhost:80` on to the ASP.NET Core application running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="67f55-147">En cambio, Apache no está configurado para administrar el proceso de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="67f55-147">However, Apache is not set up to manage the Kestrel process.</span></span> <span data-ttu-id="67f55-148">Usaremos *systemd* y crearemos un archivo de servicio para iniciar y supervisar la aplicación web subyacente.</span><span class="sxs-lookup"><span data-stu-id="67f55-148">We will use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="67f55-149">*systemd* es un sistema de inicio que proporciona muchas características eficaces para iniciar, detener y administrar procesos.</span><span class="sxs-lookup"><span data-stu-id="67f55-149">*systemd* is an init system that provides many powerful features for starting, stopping and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="67f55-150">Crear el archivo de servicio</span><span class="sxs-lookup"><span data-stu-id="67f55-150">Create the service file</span></span>

<span data-ttu-id="67f55-151">Cree el archivo de definición de servicio.</span><span class="sxs-lookup"><span data-stu-id="67f55-151">Create the service definition file</span></span> 

```bash
    sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="67f55-152">Archivo de servicio de ejemplo de nuestra aplicación.</span><span class="sxs-lookup"><span data-stu-id="67f55-152">An example service file for our application.</span></span>

```text
[Unit]
    Description=Example .NET Web API Application running on CentOS 7

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
> <span data-ttu-id="67f55-153">**Usuario**: Si la configuración no emplea el usuario *apache*, primero se deberá crear el usuario definido aquí y luego se le deberá proporcionar la propiedad adecuada de los archivos.</span><span class="sxs-lookup"><span data-stu-id="67f55-153">**User** -- If the user *apache* is not used by your configuration, the user defined here must be created first and given proper ownership for files</span></span>

<span data-ttu-id="67f55-154">Guarde el archivo y habilite el servicio.</span><span class="sxs-lookup"><span data-stu-id="67f55-154">Save the file and enable the service.</span></span>

```bash
    systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="67f55-155">Inicie el servicio y compruebe que se está ejecutando.</span><span class="sxs-lookup"><span data-stu-id="67f55-155">Start the service and verify that it is running.</span></span>

```
    systemctl start kestrel-hellomvc.service
    systemctl status kestrel-hellomvc.service

    ● kestrel-hellomvc.service - Example .NET Web API Application running on CentOS 7
        Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
        Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
    Main PID: 9021 (dotnet)
        CGroup: /system.slice/kestrel-hellomvc.service
                └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="67f55-156">Con el proxy inverso configurado y Kestrel administrado a través de systemd, la aplicación web está completamente configurada y se puede obtener acceso a ella desde un explorador en el equipo local en `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="67f55-156">With the reverse proxy configured and Kestrel managed through systemd, the web application is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="67f55-157">Al inspeccionar los encabezados de respuesta, el **servidor** sigue mostrando la aplicación de ASP.NET Core que proporciona Kestrel.</span><span class="sxs-lookup"><span data-stu-id="67f55-157">Inspecting the response headers, the **Server** still shows the ASP.NET Core application being served by Kestrel.</span></span>

```text
    HTTP/1.1 200 OK
    Date: Tue, 11 Oct 2016 16:22:23 GMT
    Server: Kestrel
    Keep-Alive: timeout=5, max=98
    Connection: Keep-Alive
    Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="67f55-158">Ver los registros</span><span class="sxs-lookup"><span data-stu-id="67f55-158">Viewing logs</span></span>

<span data-ttu-id="67f55-159">Dado que la aplicación web que usa Kestrel se administra mediante systemd, todos los procesos y eventos se registran en un diario centralizado.</span><span class="sxs-lookup"><span data-stu-id="67f55-159">Since the web application using Kestrel is managed using systemd, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="67f55-160">En cambio, este diario incluye todas las entradas de todos los servicios y procesos administrados por systemd.</span><span class="sxs-lookup"><span data-stu-id="67f55-160">However, this journal includes all entries for all services and processes managed by systemd.</span></span> <span data-ttu-id="67f55-161">Para ver los elementos específicos de `kestrel-hellomvc.service`, use el siguiente comando.</span><span class="sxs-lookup"><span data-stu-id="67f55-161">To view the `kestrel-hellomvc.service` specific items, use the following command.</span></span>

```bash
    sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="67f55-162">Para filtrar más, las opciones de tiempo como `--since today`, `--until 1 hour ago` o una combinación de estas pueden reducir la cantidad de entradas que se devuelven.</span><span class="sxs-lookup"><span data-stu-id="67f55-162">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
    sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a><span data-ttu-id="67f55-163">Proteger la aplicación</span><span class="sxs-lookup"><span data-stu-id="67f55-163">Securing our application</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="67f55-164">Configurar el firewall</span><span class="sxs-lookup"><span data-stu-id="67f55-164">Configure firewall</span></span>

<span data-ttu-id="67f55-165">*Firewalld* es un demonio dinámico que sirve para administrar firewalls con compatibilidad con las zonas de red, aunque puede seguir usando iptables para administrar los puertos y el filtrado de paquetes.</span><span class="sxs-lookup"><span data-stu-id="67f55-165">*Firewalld* is a dynamic daemon to manage firewall with support for network zones, although you can still use iptables to manage ports and packet filtering.</span></span> <span data-ttu-id="67f55-166">Firewalld debe estar instalado de forma predeterminada, mientras que `yum` se puede usar para instalar el paquete o para efectuar comprobaciones.</span><span class="sxs-lookup"><span data-stu-id="67f55-166">Firewalld should be installed by default, `yum` can be used to install the package or verify.</span></span>

```bash
    sudo yum install firewalld -y
```

<span data-ttu-id="67f55-167">Con `firewalld` puede abrir únicamente los puertos necesarios para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="67f55-167">Using `firewalld` you can open only the ports needed for the application.</span></span> <span data-ttu-id="67f55-168">En este caso se usan los puertos 80 y 443.</span><span class="sxs-lookup"><span data-stu-id="67f55-168">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="67f55-169">Los siguientes comandos los configuran como abiertos de forma permanente.</span><span class="sxs-lookup"><span data-stu-id="67f55-169">The following commands permanently sets these to open.</span></span>

```bash
    sudo firewall-cmd --add-port=80/tcp --permanent
    sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="67f55-170">Vuelva a cargar la configuración del firewall y compruebe los servicios y puertos disponibles en la zona predeterminada.</span><span class="sxs-lookup"><span data-stu-id="67f55-170">Reload the firewall settings, and check the available services and ports in the default zone.</span></span> <span data-ttu-id="67f55-171">Las opciones están disponibles si se inspecciona `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="67f55-171">Options are available by inspecting `firewall-cmd -h`</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="67f55-172">Configuración de SSL</span><span class="sxs-lookup"><span data-stu-id="67f55-172">SSL configuration</span></span>

<span data-ttu-id="67f55-173">Para configurar Apache para SSL se usa el módulo mod_ssl,</span><span class="sxs-lookup"><span data-stu-id="67f55-173">To configure Apache for SSL, the mod_ssl module is used.</span></span>  <span data-ttu-id="67f55-174">que se instaló al instalar el módulo `httpd`.</span><span class="sxs-lookup"><span data-stu-id="67f55-174">This was installed initially when we installed the `httpd` module.</span></span> <span data-ttu-id="67f55-175">Si no aparece o no está instalado, use yum para agregarlo a la configuración.</span><span class="sxs-lookup"><span data-stu-id="67f55-175">If it was missed or not installed, use yum to add it to your configuration.</span></span>

```bash
    sudo yum install mod_ssl
```
<span data-ttu-id="67f55-176">Para usar SSL, instale `mod_rewrite`.</span><span class="sxs-lookup"><span data-stu-id="67f55-176">To enforce SSL, install `mod_rewrite`</span></span>

```bash
    sudo yum install mod_rewrite
```

<span data-ttu-id="67f55-177">El archivo `hellomvc.conf` creado para este ejemplo se debe modificar para habilitar la reescritura, así como para agregar la nueva sección **VirtualHost** para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="67f55-177">The `hellomvc.conf` file that was created for this example needs to be modified to enable the rewrite as well as adding the new **VirtualHost** section for HTTPS.</span></span>

```text
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
> <span data-ttu-id="67f55-178">En este ejemplo se usa un certificado generado localmente.</span><span class="sxs-lookup"><span data-stu-id="67f55-178">This example is using a locally generated certificate.</span></span> <span data-ttu-id="67f55-179">**SSLCertificateFile** debe ser el archivo de certificado principal para el nombre de dominio.</span><span class="sxs-lookup"><span data-stu-id="67f55-179">**SSLCertificateFile** should be your primary certificate file for your domain name.</span></span> <span data-ttu-id="67f55-180">**SSLCertificateKeyFile** debe ser el archivo de claves generado al crear el CSR.</span><span class="sxs-lookup"><span data-stu-id="67f55-180">**SSLCertificateKeyFile** should be the key file generated when you created the CSR.</span></span> <span data-ttu-id="67f55-181">**SSLCertificateChainFile** debe ser el archivo de certificado intermedio (si existe) proporcionado por la entidad de certificación.</span><span class="sxs-lookup"><span data-stu-id="67f55-181">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by your certificate authority</span></span>

<span data-ttu-id="67f55-182">Guarde el archivo y pruebe la configuración.</span><span class="sxs-lookup"><span data-stu-id="67f55-182">Save the file, and test the configuration.</span></span>

```bash
    sudo service httpd configtest
```

<span data-ttu-id="67f55-183">Reinicie Apache.</span><span class="sxs-lookup"><span data-stu-id="67f55-183">Restart Apache.</span></span>

```bash
    sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="67f55-184">Sugerencias adicionales de Apache</span><span class="sxs-lookup"><span data-stu-id="67f55-184">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="67f55-185">Encabezados adicionales</span><span class="sxs-lookup"><span data-stu-id="67f55-185">Additional Headers</span></span> 
<span data-ttu-id="67f55-186">Para protegerse frente a ataques malintencionados, hay unos encabezados que se deben modificar o agregar.</span><span class="sxs-lookup"><span data-stu-id="67f55-186">In order to secure against malicious attacks there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="67f55-187">Asegúrese de que el módulo `mod_headers` está instalado.</span><span class="sxs-lookup"><span data-stu-id="67f55-187">Ensure that the `mod_headers` module is installed.</span></span>

```bash
    sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking"></a><span data-ttu-id="67f55-188">Proteger Apache frente al secuestro de clic</span><span class="sxs-lookup"><span data-stu-id="67f55-188">Secure Apache from clickjacking</span></span>
<span data-ttu-id="67f55-189">El secuestro de clic es una técnica malintencionada para recopilar los clics de un usuario infectado.</span><span class="sxs-lookup"><span data-stu-id="67f55-189">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="67f55-190">El secuestro de clic engaña a la víctima (visitante) para que haga clic en un sitio infectado.</span><span class="sxs-lookup"><span data-stu-id="67f55-190">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="67f55-191">Use X-FRAME-OPTIONS para proteger su sitio.</span><span class="sxs-lookup"><span data-stu-id="67f55-191">Use X-FRAME-OPTIONS to secure your site.</span></span>

<span data-ttu-id="67f55-192">Edite el archivo httpd.conf.</span><span class="sxs-lookup"><span data-stu-id="67f55-192">Edit the httpd.conf file.</span></span>

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="67f55-193">Agregue la línea `Header append X-FRAME-OPTIONS "SAMEORIGIN"` y guarde el archivo. Luego, reinicie Apache.</span><span class="sxs-lookup"><span data-stu-id="67f55-193">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"` and save the file, then restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="67f55-194">Examen de tipo MIME</span><span class="sxs-lookup"><span data-stu-id="67f55-194">MIME-type sniffing</span></span>

<span data-ttu-id="67f55-195">Este encabezado evita que Internet Explorer examine el MIME de una respuesta fuera del tipo de contenido declarado, ya que el encabezado indica al explorador que no reemplace el tipo de contenido de respuesta.</span><span class="sxs-lookup"><span data-stu-id="67f55-195">This header prevents Internet Explorer from MIME-sniffing a response away from the declared content-type as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="67f55-196">Con la opción nosniff, si el servidor indica que el contenido es text/html, el explorador lo representará como text/html.</span><span class="sxs-lookup"><span data-stu-id="67f55-196">With the nosniff option, if the server says the content is text/html, the browser will render it as text/html.</span></span>

<span data-ttu-id="67f55-197">Edite el archivo httpd.conf.</span><span class="sxs-lookup"><span data-stu-id="67f55-197">Edit the httpd.conf file.</span></span>

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="67f55-198">Agregue la línea `Header set X-Content-Type-Options "nosniff"` y guarde el archivo. Luego, reinicie Apache.</span><span class="sxs-lookup"><span data-stu-id="67f55-198">Add the line `Header set X-Content-Type-Options "nosniff"` and save the file, then restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="67f55-199">Equilibrio de carga</span><span class="sxs-lookup"><span data-stu-id="67f55-199">Load Balancing</span></span> 

<span data-ttu-id="67f55-200">En este ejemplo se muestra cómo instalar y configurar Apache en CentOS 7 y Kestrel en el mismo equipo de la instancia.</span><span class="sxs-lookup"><span data-stu-id="67f55-200">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span>  <span data-ttu-id="67f55-201">Pero para no tener un único punto de error, el uso de *mod_proxy_balancer* y la modificación de VirtualHost permitirían administrar varias instancias de las aplicaciones web detrás del servidor proxy de Apache.</span><span class="sxs-lookup"><span data-stu-id="67f55-201">However, in order to not have a single point of failure; using *mod_proxy_balancer* and modifying the VirtualHost would allow for managing mutliple instances of the web applications behind the Apache proxy server.</span></span>

```bash
    sudo yum install mod_proxy_balancer
```

<span data-ttu-id="67f55-202">En el archivo de configuración se ha configurado una instancia adicional de la aplicación `hellomvc` para ejecutarse en el puerto 5001. También se ha establecido la sección *Proxy* con una configuración de equilibrador con dos miembros para equilibrar la carga de *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="67f55-202">In the configuration file, an additional instance of the `hellomvc` app has been setup to run on port 5001 and the *Proxy* section has been set with a balancer configuration with two members to load balance *byrequests*.</span></span>

```text
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

### <a name="rate-limits"></a><span data-ttu-id="67f55-203">Límites de velocidad</span><span class="sxs-lookup"><span data-stu-id="67f55-203">Rate Limits</span></span>
<span data-ttu-id="67f55-204">Si usa `mod_ratelimit`, que se incluye en el módulo `htttpd`, puede limitar el ancho de banda de los clientes.</span><span class="sxs-lookup"><span data-stu-id="67f55-204">Using `mod_ratelimit`, which is included in the `htttpd` module you can limit the amount of bandwidth of clients.</span></span> 

```bash
    sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="67f55-205">En el archivo de ejemplo se limita el ancho de banda a 600 KB/s en la ubicación raíz.</span><span class="sxs-lookup"><span data-stu-id="67f55-205">The example file limits bandwidth as 600 KB/sec under the root location.</span></span>

```text
    <IfModule mod_ratelimit.c>
        <Location />
            SetOutputFilter RATE_LIMIT
            SetEnv rate-limit 600
        </Location>
    </IfModule>
```
