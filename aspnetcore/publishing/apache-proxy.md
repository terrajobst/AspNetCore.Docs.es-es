---
title: Hospedar ASP.NET Core en Linux con Apache
description: "Aprenda a configurar Apache como servidor proxy inverso en CentOS para redirigir el tráfico HTTP a una aplicación web de ASP.NET Core ejecutándose en Kestrel."
keywords: ASP.NET Core, Apache, CentOS, proxy inverso, Linux, mod_proxy, httpd, hospedaje
author: spboyer
ms.author: spboyer
manager: wpickett
ms.date: 10/19/2016
ms.topic: article
ms.assetid: fa9b0cb7-afb3-4361-9e7e-33afffeaca0c
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/apache-proxy
ms.openlocfilehash: 831e2fa148e52f6447e9065f5949785627d5e248
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-apache-and-deploy-to-it"></a>Configurar un entorno de hospedaje para ASP.NET Core en Linux con Apache e implementar en él

Por [Shayne Boyer](https://www.github.com/spboyer)

Apache es un servidor HTTP muy conocido que se puede configurar como proxy para redirigir el tráfico HTTP similar a nginx. En esta guía aprenderá a configurar Apache en CentOS 7 y a usarlo como proxy inverso para aceptar conexiones entrantes y redirigirlas a la aplicación de ASP.NET Core que se ejecuta en Kestrel. Para ello usaremos la extensión *mod_proxy* y otros módulos de Apache relacionados.

## <a name="prerequisites"></a>Requisitos previos

1. Un servidor que ejecute CentOS 7, con una cuenta de usuario estándar con privilegios sudo.
2. Una aplicación de ASP.NET Core existente. 

## <a name="publish-your-application"></a>Publicar su aplicación

Ejecute `dotnet publish -c Release` desde el entorno de desarrollo para empaquetar la aplicación en un directorio independiente que se pueda ejecutar en el servidor. Luego se debe copiar la aplicación publicada en el servidor mediante SCP, FTP u otro método de transferencia de archivos. 

> [!NOTE]
> En un escenario de implementación de producción, un flujo de trabajo de integración continuo lleva a cabo la tarea de publicar la aplicación y copiar los recursos en el servidor. 

## <a name="configure-a-proxy-server"></a>Configurar un servidor proxy

Un proxy inverso es una configuración común para usar aplicaciones web dinámicas. El proxy inverso finaliza la solicitud HTTP y la reenvía a la aplicación de ASP.NET.

Los servidores proxy son los que reenvían las solicitudes de cliente a otro servidor en lugar de realizarlas ellos mismos. Los proxies inversos las reenvían a un destino fijo, normalmente en nombre de clientes arbitrarios. En esta guía, Apache se configura como proxy inverso que se ejecuta en el mismo servidor en el que Kestrel da servicio a la aplicación de ASP.NET Core. 

Cada elemento de la aplicación puede residir en equipos físicos diferentes, en contenedores de Docker o en una combinación de configuraciones según sus necesidades o restricciones de arquitectura.

### <a name="install-apache"></a>Instalar Apache

Para instalar el servidor web de Apache en CentOS se usa un solo comando, pero primero vamos a actualizar los paquetes.

```bash
    sudo yum update -y
```

Esto garantiza que todos los paquetes instalados se actualicen a la versión más reciente. Instalar Apache mediante `yum`

```bash
    sudo yum -y install httpd mod_ssl
```

La salida debe reflejar algo parecido a lo siguiente.

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
> En este ejemplo, la salida refleja httpd.86_64, puesto que la versión de CentOS 7 es de 64 bits. La salida podría ser diferente para su servidor. Para comprobar dónde está instalado Apache, ejecute `whereis httpd` desde un símbolo del sistema. 

### <a name="configure-apache-for-reverse-proxy"></a>Configurar Apache para un proxy inverso

Los archivos de configuración de Apache se encuentran en el directorio `/etc/httpd/conf.d/`. Todos los archivos que tengan la extensión **.conf** se procesarán en orden alfabético, además de los archivos de configuración del módulo de `/etc/httpd/conf.modules.d/`, que contiene todos los archivos de configuración necesarios para cargar los módulos.

Cree un archivo de configuración para la aplicación. En este ejemplo lo llamaremos `hellomvc.conf`.

```text
    <VirtualHost *:80>
        ProxyPreserveHost On
        ProxyPass / http://127.0.0.1:5000/
        ProxyPassReverse / http://127.0.0.1:5000/
        ErrorLog /var/log/httpd/hellomvc-error.log
        CustomLog /var/log/httpd/hellomvc-access.log common
    </VirtualHost>
```

El nodo *VirtualHost*, del que puede haber varios en un archivo o en muchos archivos de un servidor, está configurado para escuchar en cualquier dirección IP con el puerto 80. Las dos líneas siguientes se establecen para pasar todas las solicitudes recibidas en la raíz al equipo 127.0.0.1, puerto 5000, y en orden inverso. Para que haya una comunicación bidireccional, las opciones *ProxyPass* y *ProxyPassReverse* son necesarias.

El registro se puede configurar por VirtualHost mediante las directivas *ErrorLog* y *CustomLog*. *ErrorLog* es la ubicación en la que el servidor registrará los errores y *CustomLog* establece el nombre de archivo y el formato del archivo de registro. En nuestro caso, aquí se registrará la información de la solicitud. Habrá una línea para cada solicitud.

Guarde el archivo y pruebe la configuración. Si se pasa todo, la respuesta debe ser `Syntax [OK]`.

```bash
    sudo service httpd configtest
```

Reinicie Apache.

```text
    sudo systemctl restart httpd
    sudo systemctl enable httpd
```

## <a name="monitoring-our-application"></a>Supervisar la aplicación

Apache ahora está configurado para reenviar las solicitudes efectuadas a `http://localhost:80` en la aplicación de ASP.NET Core que se ejecuta en Kestrel en `http://127.0.0.1:5000`.  En cambio, Apache no está configurado para administrar el proceso de Kestrel. Usaremos *systemd* y crearemos un archivo de servicio para iniciar y supervisar la aplicación web subyacente. *systemd* es un sistema de inicio que proporciona muchas características eficaces para iniciar, detener y administrar procesos. 


### <a name="create-the-service-file"></a>Crear el archivo de servicio

Cree el archivo de definición de servicio. 

```bash
    sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

Archivo de servicio de ejemplo de nuestra aplicación.

```text
[Unit]
    Description=Example .NET Web API Application running on CentOS 7

    [Service]
    WorkingDirectory=/var/aspnetcore/hellomvc
    ExecStart=/usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
    Restart=always
    RestartSec=10                                          # Restart service after 10 seconds if dotnet service crashes
    SyslogIdentifier=dotnet-example
    User=apache
    Environment=ASPNETCORE_ENVIRONMENT=Production 

    [Install]
    WantedBy=multi-user.target
```

> [!NOTE]
> **Usuario**: Si la configuración no emplea el usuario *apache*, primero se deberá crear el usuario definido aquí y luego se le deberá proporcionar la propiedad adecuada de los archivos.

Guarde el archivo y habilite el servicio.

```bash
    systemctl enable kestrel-hellomvc.service
```

Inicie el servicio y compruebe que se está ejecutando.

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

Con el proxy inverso configurado y Kestrel administrado a través de systemd, la aplicación web está completamente configurada y se puede obtener acceso a ella desde un explorador en el equipo local en `http://localhost`. Al inspeccionar los encabezados de respuesta, el **servidor** sigue mostrando la aplicación de ASP.NET Core que proporciona Kestrel.

```text
    HTTP/1.1 200 OK
    Date: Tue, 11 Oct 2016 16:22:23 GMT
    Server: Kestrel
    Keep-Alive: timeout=5, max=98
    Connection: Keep-Alive
    Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>Ver los registros

Dado que la aplicación web que usa Kestrel se administra mediante systemd, todos los procesos y eventos se registran en un diario centralizado. En cambio, este diario incluye todas las entradas de todos los servicios y procesos administrados por systemd. Para ver los elementos específicos de `kestrel-hellomvc.service`, use el siguiente comando.

```bash
    sudo journalctl -fu kestrel-hellomvc.service
```

Para filtrar más, las opciones de tiempo como `--since today`, `--until 1 hour ago` o una combinación de estas pueden reducir la cantidad de entradas que se devuelven.

```bash
    sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a>Proteger la aplicación

### <a name="configure-firewall"></a>Configurar el firewall

*Firewalld* es un demonio dinámico que sirve para administrar firewalls con compatibilidad con las zonas de red, aunque puede seguir usando iptables para administrar los puertos y el filtrado de paquetes. Firewalld debe estar instalado de forma predeterminada, mientras que `yum` se puede usar para instalar el paquete o para efectuar comprobaciones.

```bash
    sudo yum install firewalld -y
```

Con `firewalld` puede abrir únicamente los puertos necesarios para la aplicación. En este caso se usan los puertos 80 y 443. Los siguientes comandos los configuran como abiertos de forma permanente.

```bash
    sudo firewall-cmd --add-port=80/tcp --permanent
    sudo firewall-cmd --add-port=443/tcp --permanent
```

Vuelva a cargar la configuración del firewall y compruebe los servicios y puertos disponibles en la zona predeterminada. Las opciones están disponibles si se inspecciona `firewall-cmd -h`.

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

Para configurar Apache para SSL se usa el módulo mod_ssl,  que se instaló al instalar el módulo `httpd`. Si no aparece o no está instalado, use yum para agregarlo a la configuración.

```bash
    sudo yum install mod_ssl
```
Para usar SSL, instale `mod_rewrite`.

```bash
    sudo yum install mod_rewrite
```

El archivo `hellomvc.conf` creado para este ejemplo se debe modificar para habilitar la reescritura, así como para agregar la nueva sección **VirtualHost** para HTTPS.

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
> En este ejemplo se usa un certificado generado localmente. **SSLCertificateFile** debe ser el archivo de certificado principal para el nombre de dominio. **SSLCertificateKeyFile** debe ser el archivo de claves generado al crear el CSR. **SSLCertificateChainFile** debe ser el archivo de certificado intermedio (si existe) proporcionado por la entidad de certificación.

Guarde el archivo y pruebe la configuración.

```bash
    sudo service httpd configtest
```

Reinicie Apache.

```bash
    sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a>Sugerencias adicionales de Apache

### <a name="additional-headers"></a>Encabezados adicionales 
Para protegerse frente a ataques malintencionados, hay unos encabezados que se deben modificar o agregar. Asegúrese de que el módulo `mod_headers` está instalado.

```bash
    sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking"></a>Proteger Apache frente al secuestro de clic
El secuestro de clic es una técnica malintencionada para recopilar los clics de un usuario infectado. El secuestro de clic engaña a la víctima (visitante) para que haga clic en un sitio infectado. Use X-FRAME-OPTIONS para proteger su sitio.

Edite el archivo httpd.conf.

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

Agregue la línea `Header append X-FRAME-OPTIONS "SAMEORIGIN"` y guarde el archivo. Luego, reinicie Apache.

#### <a name="mime-type-sniffing"></a>Examen de tipo MIME

Este encabezado evita que Internet Explorer examine el MIME de una respuesta fuera del tipo de contenido declarado, ya que el encabezado indica al explorador que no reemplace el tipo de contenido de respuesta. Con la opción nosniff, si el servidor indica que el contenido es text/html, el explorador lo representará como text/html.

Edite el archivo httpd.conf.

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

Agregue la línea `Header set X-Content-Type-Options "nosniff"` y guarde el archivo. Luego, reinicie Apache.

### <a name="load-balancing"></a>Equilibrio de carga 

En este ejemplo se muestra cómo instalar y configurar Apache en CentOS 7 y Kestrel en el mismo equipo de la instancia.  Pero para no tener un único punto de error, el uso de *mod_proxy_balancer* y la modificación de VirtualHost permitirían administrar varias instancias de las aplicaciones web detrás del servidor proxy de Apache.

```bash
    sudo yum install mod_proxy_balancer
```

En el archivo de configuración se ha configurado una instancia adicional de la aplicación `hellomvc` para ejecutarse en el puerto 5001. También se ha establecido la sección *Proxy* con una configuración de equilibrador con dos miembros para equilibrar la carga de *byrequests*.

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

### <a name="rate-limits"></a>Límites de velocidad
Si usa `mod_ratelimit`, que se incluye en el módulo `htttpd`, puede limitar el ancho de banda de los clientes. 

```bash
    sudo nano /etc/httpd/conf.d/ratelimit.conf
```
En el archivo de ejemplo se limita el ancho de banda a 600 KB/s en la ubicación raíz.

```text
    <IfModule mod_ratelimit.c>
        <Location />
            SetOutputFilter RATE_LIMIT
            SetEnv rate-limit 600
        </Location>
    </IfModule>
```
