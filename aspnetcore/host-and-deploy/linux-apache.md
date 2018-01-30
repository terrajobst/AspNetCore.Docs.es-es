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
# <a name="host-aspnet-core-on-linux-with-apache"></a>Hospedar ASP.NET Core en Linux con Apache

Por [Shayne Boyer](https://github.com/spboyer)

Con esta guía, obtenga información acerca de cómo configurar [Apache](https://httpd.apache.org/) como un servidor proxy inverso en [CentOS 7](https://www.centos.org/) para redirigir el tráfico HTTP a una aplicación web de ASP.NET Core con [Kestrel](xref:fundamentals/servers/kestrel). El [mod_proxy extensión](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) y módulos relacionados Crear proxy inverso del servidor.

## <a name="prerequisites"></a>Requisitos previos

1. Servidor que ejecuta CentOS 7 con una cuenta de usuario estándar con privilegios sudo
2. Aplicación de ASP.NET Core

## <a name="publish-the-app"></a>Publicar la aplicación

Publicar la aplicación como un [implementación independiente](/dotnet/core/deploying/#self-contained-deployments-scd) en configuración de lanzamiento para el tiempo de ejecución de CentOS 7 (`centos.7-x64`). Copie el contenido de la *bin/Release/netcoreapp2.0/centos.7-x64/publish* carpeta al servidor mediante el SCP, FTP u otro método de transferencia de archivos.

> [!NOTE]
> En un escenario de implementación de producción, un flujo de trabajo de integración continua realiza el trabajo de publicación de la aplicación y copiar los activos en el servidor. 

## <a name="configure-a-proxy-server"></a>Configurar un servidor proxy

Un proxy inverso es una configuración común para servir las aplicaciones web dinámicas. El proxy inverso finaliza la solicitud HTTP y lo reenvía a la aplicación ASP.NET.

Los servidores proxy son los que reenvían las solicitudes de cliente a otro servidor en lugar de realizarlas ellos mismos. Los proxies inversos las reenvían a un destino fijo, normalmente en nombre de clientes arbitrarios. En esta guía, Apache está configurado como el proxy inverso que se ejecuta en el mismo servidor que Kestrel sigue dando servicio a la aplicación de ASP.NET Core.

### <a name="install-apache"></a>Instalar Apache

Actualizar paquetes de CentOS a sus versiones estables más recientes:

```bash
sudo yum update -y
```

Instalar el servidor web Apache en CentOS con una sola `yum` comando:

```bash
sudo yum -y install httpd mod_ssl
```

Resultado después de ejecutar el comando de ejemplo:

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
> En este ejemplo, el resultado refleja httpd.86_64 puesto que la versión de CentOS 7 es de 64 bits. Para comprobar dónde está instalado Apache, ejecute `whereis httpd` desde un símbolo del sistema. 

### <a name="configure-apache-for-reverse-proxy"></a>Configurar Apache para un proxy inverso

Los archivos de configuración de Apache se encuentran en el directorio `/etc/httpd/conf.d/`. Cualquier archivo con el *.conf* extensión se procesa en orden alfabético además de los archivos de configuración de módulo en `/etc/httpd/conf.modules.d/`, que contiene cualquier configuración de los archivos necesario para cargar los módulos.

Crear un archivo de configuración de la aplicación denominada `hellomvc.conf`:

```
<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
</VirtualHost>
```

El **VirtualHost** nodo puede aparecer varias veces en uno o varios archivos en un servidor. **VirtualHost** se establece para que escuche en cualquier dirección IP que usa el puerto 80. Las dos líneas siguientes se establecen en las solicitudes de proxy en el directorio raíz en el servidor en 127.0.0.1 en el puerto 5000. Para la comunicación bidireccional, *ProxyPass* y *ProxyPassReverse* son necesarios.

Se puede configurar el registro por **VirtualHost** con **ErrorLog** y **CustomLog** directivas. **Registro de errores** es la ubicación donde el servidor registra errores, y **CustomLog** establece el nombre de archivo y el formato de archivo de registro. En este caso, esto es donde se registra la información de la solicitud. Hay una línea para cada solicitud.

Guarde el archivo y la configuración de pruebas. Si se pasa todo, la respuesta debe ser `Syntax [OK]`.

```bash
sudo service httpd configtest
```

Reinicie Apache:

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a>Supervisión de la aplicación

Apache ahora está configurado para reenviar las solicitudes realizadas a `http://localhost:80` a la aplicación de ASP.NET Core con Kestrel en `http://127.0.0.1:5000`.  Sin embargo, Apache no está configurado para administrar el proceso de Kestrel. Use *systemd* y cree un archivo de servicio para iniciar y supervisar la aplicación web subyacente. *systemd* es un sistema de inicio que proporciona muchas características eficaces para iniciar, detener y administrar procesos. 


### <a name="create-the-service-file"></a>Crear el archivo de servicio

Cree el archivo de definición de servicio:

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

Un archivo de servicio de ejemplo para la aplicación:

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
> **Usuario** &mdash; si el usuario *apache* no se usa la configuración, el usuario debe crear primero y determinado propiedad adecuada para los archivos.

Guarde el archivo y habilitar el servicio:

```bash
systemctl enable kestrel-hellomvc.service
```

Iniciar el servicio y compruebe que se está ejecutando:

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

Con el proxy inverso configurado y Kestrel administrada a través de *systemd*, la aplicación web está configurada completamente y son accesibles desde un explorador en el equipo local en `http://localhost`. Inspeccionar los encabezados de respuesta, el **Server** encabezado indica que la aplicación de ASP.NET Core atendida por Kestrel:

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>Ver los registros

Desde la aplicación web utilizando Kestrel se administra con *systemd*, procesos y eventos se registran en un diario de centralizada. Sin embargo, este diario incluye entradas para todos los servicios y procesos administrados por *systemd*. Para ver los elementos específicos de `kestrel-hellomvc.service`, use el siguiente comando:

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

Para filtrar el tiempo, especifique las opciones de tiempo con el comando. Por ejemplo, utilice `--since today` para filtrar en la actualidad o `--until 1 hour ago` para ver las entradas de la hora anterior. Para obtener más información, consulte el [página del comando man journalctl](https://www.unix.com/man-page/centos/1/journalctl/).

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a>Protección de la aplicación

### <a name="configure-firewall"></a>Configurar el firewall

*Firewalld* es un demonio dinámico para administrar el firewall con compatibilidad para las zonas de red. Puertos y el filtrado de paquetes pueden seguir administrándose mediante iptables. *Firewalld* debe instalarse de forma predeterminada. `yum`puede utilizarse para instalar el paquete o compruebe que está instalado.

```bash
sudo yum install firewalld -y
```

Use `firewalld` para abrir sólo los puertos necesarios para la aplicación. En este caso se usan los puertos 80 y 443. Los siguientes comandos de forma permanente establecen los puertos 80 y 443 para abrir:

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

Vuelva a cargar la configuración del firewall. Compruebe los servicios disponibles y los puertos en la zona predeterminada. Opciones están disponibles mediante la inspección `firewall-cmd -h`.

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

### <a name="ssl-configuration"></a>Configuración de SSL

Para configurar Apache para SSL, el *mod_ssl* módulo se usa. Cuando el *httpd* se instaló el módulo, el *mod_ssl* también se instala el módulo. Si aún no se ha instalado, use `yum` para agregarlo a la configuración.

```bash
sudo yum install mod_ssl
```
Para utilizar SSL, instalar el `mod_rewrite` módulo para habilitar la reescritura de direcciones URL:

```bash
sudo yum install mod_rewrite
```

Modificar el *hellomvc.conf* archivo para habilitar la reescritura de direcciones URL y proteger la comunicación en el puerto 443:

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
> En este ejemplo se utiliza un certificado generado localmente. **SSLCertificateFile** debe ser el archivo de certificado principal para el nombre de dominio. **SSLCertificateKeyFile** debe ser el archivo de clave generar cuando se crea el CSR. **SSLCertificateChainFile** debe ser el archivo de certificado intermedio (si existe) que se ha proporcionado por la entidad de certificación.

Guarde el archivo y la configuración de prueba:

```bash
sudo service httpd configtest
```

Reinicie Apache:

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a>Sugerencias adicionales de Apache

### <a name="additional-headers"></a>Encabezados adicionales

Para proteger frente a ataques malintencionados, hay algunos encabezados que deben ser modificados o agregados. Asegúrese de que el `mod_headers` módulo está instalado:

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a>Apache seguro frente a ataques de clickjacking

[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), también conocida como un *UI reclamación ataque*, es un ataque malintencionado donde el visitante de un sitio Web engañar al hacer clic en un vínculo o botón en una página distinta que actualmente está visitando. Use `X-FRAME-OPTIONS` para proteger el sitio.

Editar la *httpd.conf* archivo:

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

Agregue la línea `Header append X-FRAME-OPTIONS "SAMEORIGIN"`. Guarde el archivo. Reinicie Apache.

#### <a name="mime-type-sniffing"></a>Examen de tipo MIME

El `X-Content-Type-Options` encabezado impide que Internet Explorer *examen de MIME* (determinar un archivo `Content-Type` desde el contenido del archivo). Si el servidor establece la `Content-Type` encabezado a `text/html` con el `nosniff` representa el conjunto de opciones, Internet Explorer el contenido como `text/html` sin tener en cuenta el contenido del archivo.

Editar la *httpd.conf* archivo:

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

Agregue la línea `Header set X-Content-Type-Options "nosniff"`. Guarde el archivo. Reinicie Apache.

### <a name="load-balancing"></a>Equilibrio de carga 

En este ejemplo se muestra cómo instalar y configurar Apache en CentOS 7 y Kestrel en el mismo equipo de la instancia. Para no tener un único punto de error; usar *mod_proxy_balancer* y modificar el **VirtualHost** permitiría para administrar varias instancias de las aplicaciones web detrás del servidor de proxy de Apache.

```bash
sudo yum install mod_proxy_balancer
```

En el archivo de configuración se muestra a continuación, una instancia adicional de la `hellomvc` aplicación está configurada para que se ejecute en el puerto 5001. El *Proxy* sección se establece con una configuración de equilibrador con dos miembros para equilibrar la carga *byrequests*.

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

### <a name="rate-limits"></a>Límites de velocidad
Usar *mod_ratelimit*, que se incluye en el *httpd* módulo, el ancho de banda de clientes puede ser limitada:

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
El archivo de ejemplo limita el ancho de banda como 600 KB/s en la ubicación raíz:

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```
