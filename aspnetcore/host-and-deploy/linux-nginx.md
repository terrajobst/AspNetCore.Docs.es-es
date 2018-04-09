---
title: Hospedar ASP.NET Core en Linux con Nginx
author: rick-anderson
description: Describe cómo configurar Nginx como un proxy inverso en Ubuntu 16.04 para reenviar el tráfico HTTP a una aplicación web de ASP.NET Core con Kestrel.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: 64093b9fcfa9047145de8f8b142f72fa1515f248
ms.sourcegitcommit: d45d766504c2c5aad2453f01f089bc6b696b5576
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2018
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a>Hospedar ASP.NET Core en Linux con Nginx

Por [Sourabh Shirhatti](https://twitter.com/sshirhatti)

En esta guía se explica cómo configurar un entorno de ASP.NET Core listo para producción en un servidor de Ubuntu 16.04.

> [!NOTE]
> Para Ubuntu 14.04, *supervisord* se recomienda como una solución para supervisar el proceso de Kestrel. *systemd* no está disponible en Ubuntu 14.04. [Vea la versión anterior de este documento](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).

En esta guía:

* Coloca una aplicación existente de ASP.NET Core detrás de un servidor proxy inverso.
* Configura el servidor proxy inverso para reenviar las solicitudes al servidor web Kestrel.
* Garantiza que la aplicación web se ejecuta al iniciar como un demonio.
* Configura una herramienta de administración de procesos para ayudar a reiniciar la aplicación web.

## <a name="prerequisites"></a>Requisitos previos

1. Acceso a un servidor de Ubuntu 16.04 con una cuenta de usuario estándar con privilegios sudo
1. Una aplicación existente de ASP.NET Core

## <a name="copy-over-the-app"></a>Copiar a través de la aplicación

Ejecutar [publicar dotnet](/dotnet/core/tools/dotnet-publish) desde el entorno de desarrollo para empaquetar una aplicación en un directorio independiente que se puede ejecutar en el servidor.

Copie la aplicación de ASP.NET Core en el servidor mediante cualquier herramienta que se integra en el flujo de trabajo de la organización (por ejemplo, SCP, FTP). Pruebe la aplicación, por ejemplo:

* Desde la línea de comandos, ejecute `dotnet <app_assembly>.dll`.
* En un explorador, vaya a `http://<serveraddress>:<port>` para comprobar que la aplicación funciona en Linux. 
 
## <a name="configure-a-reverse-proxy-server"></a>Configurar un servidor proxy inverso

Un proxy inverso es una configuración común para servir las aplicaciones web dinámicas. Un proxy inverso finaliza la solicitud HTTP y lo reenvía a la aplicación de ASP.NET Core.

### <a name="why-use-a-reverse-proxy-server"></a>¿Por qué usar un servidor proxy inverso?

Kestrel es excelente para servir contenido dinámico de ASP.NET Core. Sin embargo, las capacidades de servicio web tienen tantas características como servidores, como IIS, Apache o Nginx. Un servidor proxy inverso puede descargar de trabajo, como servir contenido estático, almacenamiento en caché las solicitudes de la compresión de las solicitudes y la terminación SSL desde el servidor HTTP. Un servidor proxy inverso puede residir en un equipo dedicado o se puede implementar junto con un servidor HTTP.

Para los fines de esta guía, se usa una única instancia de Nginx. Se ejecuta en el mismo servidor, junto con el servidor HTTP. En función de requisitos, se puede elegir una configuración diferente.

Dado que se reenvíen solicitudes por proxy inverso, usar el Middleware de encabezados reenviados desde el [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) paquete. Las actualizaciones de software intermedio el `Request.Scheme`, usando la `X-Forwarded-Proto` encabezado, por lo que ese URI de redirección y las otras directivas de seguridad funcionen correctamente.

Al utilizar cualquier tipo de middleware de autenticación, el Middleware de encabezados reenviados primero debe ejecutar. Este orden garantiza que el middleware de autenticación puede consumir los valores de encabezado y generar el URI de redireccionamiento correcto.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Invocar la [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) método `Startup.Configure` antes de llamar a [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) o similar middleware de esquema de autenticación. Configurar el middleware para reenviar el `X-Forwarded-For` y `X-Forwarded-Proto` encabezados:

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Invocar la [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) método `Startup.Configure` antes de llamar a [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) y [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) o un esquema de autenticación similares middleware. Configurar el middleware para reenviar el `X-Forwarded-For` y `X-Forwarded-Proto` encabezados:

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

Si no hay ningún [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) se especifican en el middleware, los encabezados de forma predeterminada para reenviar son `None`.

Configuración adicional podría ser necesaria para las aplicaciones hospedadas detrás de servidores proxy y equilibradores de carga. Para obtener más información, consulte [configurar ASP.NET Core para trabajar con servidores proxy y equilibradores de carga](xref:host-and-deploy/proxy-load-balancer).

### <a name="install-nginx"></a>Instalar Nginx

```bash
sudo apt-get install nginx
```

> [!NOTE]
> Si se instala los módulos de Nginx opcionales, podría ser necesario generar Nginx de origen.

Use `apt-get` para instalar Nginx. El instalador crea un script de inicio de System V que ejecuta Nginx como demonio al iniciarse el sistema. Puesto que Nginx se ha instalado por primera vez, ejecute lo siguiente para iniciarlo de forma explícita:

```bash
sudo service nginx start
```

Compruebe que un explorador muestra la página de aterrizaje predeterminada de Nginx.

### <a name="configure-nginx"></a>Configurar Nginx

Para configurar Nginx como un proxy inverso para reenviar solicitudes a la aplicación de ASP.NET Core, modifique */etc/nginx/sites-available/default*. Ábralo en un editor de texto y reemplace el contenido por el siguiente:

```nginx
server {
    listen        80;
    server_name   example.com *.example.com;
    location / {
        proxy_pass         http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $http_host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

Si no `server_name` coincidencias, Nginx utiliza el servidor predeterminado. Si no se define ningún servidor de forma predeterminada, el primer servidor en el archivo de configuración es el servidor predeterminado. Como práctica recomendada, agregue un servidor de datos específica que devuelve un código de estado de 444 en el archivo de configuración. Un ejemplo de configuración de servidor predeterminada es:

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

Con la anterior configuración predeterminada y el archivo de servidor, Nginx acepta tráfico público en el puerto 80 con encabezado de host `example.com` o `*.example.com`. Las solicitudes que no coincida con estos hosts no se reenviarán al Kestrel. Nginx reenvía las solicitudes de búsqueda de coincidencias al Kestrel en `http://localhost:5000`. Vea [cómo nginx procesa una solicitud](https://nginx.org/docs/http/request_processing.html) para obtener más información.

> [!WARNING]
> Si no se especifica un propios [directiva de nombre_de_servidor](https://nginx.org/docs/http/server_names.html) expone la aplicación a las vulnerabilidades de seguridad. Enlace de comodín de subdominio (por ejemplo, `*.example.com`) no supone este riesgo de seguridad si controla el dominio primario todo (en contraposición a `*.com`, que es vulnerable). Vea la [sección 5.4 de RFC 7230](https://tools.ietf.org/html/rfc7230#section-5.4) para obtener más información.

Una vez establecida la configuración de Nginx, ejecute `sudo nginx -t` para comprobar la sintaxis de los archivos de configuración. Si la prueba del archivo de configuración es correcta, forzar Nginx para recoger los cambios mediante la ejecución de `sudo nginx -s reload`.

## <a name="monitoring-the-app"></a>Supervisión de la aplicación

El servidor está configurado para reenviar las solicitudes realizadas a `http://<serveraddress>:80` en la aplicación de ASP.NET Core que se ejecuta en Kestrel en `http://127.0.0.1:5000`. Sin embargo, Nginx no está configurado para administrar el proceso de Kestrel. *systemd* puede utilizarse para crear un archivo de servicio para iniciar y supervisar la aplicación web subyacente. *systemd* es un sistema de inicio que proporciona muchas características eficaces para iniciar, detener y administrar procesos. 

### <a name="create-the-service-file"></a>Crear el archivo de servicio

Cree el archivo de definición de servicio:

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

El siguiente es un archivo de servicio de ejemplo para la aplicación:

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

**Nota:** si el usuario *www datos* no se usa por la configuración, el usuario definido aquí se debe crear primero y determinado propiedad adecuada para los archivos.
**Nota:** Linux tiene un sistema de archivos distingue mayúsculas de minúsculas. Si se establece ASPNETCORE_ENVIRONMENT en los resultados de "Producción" en la búsqueda del archivo de configuración *appsettings. Production.JSON*, no *appsettings.production.json*.

Guarde el archivo y habilite el servicio.

```bash
systemctl enable kestrel-hellomvc.service
```

Iniciar el servicio y compruebe que se está ejecutando.

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

Con el proxy inverso configurado y Kestrel administrada a través de systemd, la aplicación web esté completamente configurada y puede tener acceso desde un explorador en el equipo local en `http://localhost`. También es accesible desde un equipo remoto, bloqueo de cualquier firewall que podría estar bloqueando. Inspeccionar los encabezados de respuesta, el `Server` encabezado muestra la aplicación de ASP.NET Core sea atendida por Kestrel.

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>Ver los registros

Desde la aplicación web utilizando Kestrel se administra con `systemd`, todos los procesos y los eventos se registran en un diario de centralizada. En cambio, este diario incluye todas las entradas de todos los servicios y procesos administrados por `systemd`. Para ver los elementos específicos de `kestrel-hellomvc.service`, use el siguiente comando:

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

Para obtener más opciones de filtrado, las opciones de tiempo como `--since today`, `--until 1 hour ago` o una combinación de estas pueden reducir la cantidad de entradas que se devuelven.

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a>Protección de la aplicación

### <a name="enable-apparmor"></a>Habilitar AppArmor

Linux Security Modules (LSM) es un marco que forma parte del núcleo de Linux desde Linux 2.6. LSM admite diferentes implementaciones de los módulos de seguridad. [AppArmor](https://wiki.ubuntu.com/AppArmor) es un LSM que implementa un sistema de control de acceso obligatorio que permite restringir el programa a un conjunto limitado de recursos. Asegúrese de que AppArmor está habilitado y configurado correctamente.

### <a name="configuring-the-firewall"></a>Configuración del firewall

Cierre todos los puertos externos que no estén en uso. Uncomplicated Firewall (ufw) proporciona un front-end para `iptables` al proporcionar una interfaz de línea de comandos para configurar el firewall. Compruebe que `ufw` está configurado para permitir el tráfico en los puertos que sean necesarios.

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a>Proteger Nginx

La distribución predeterminada de Nginx no habilita SSL. Para habilitar características de seguridad adicionales, compile desde el origen.

#### <a name="download-the-source-and-install-the-build-dependencies"></a>Descargar el origen e instalar las dependencias de compilación

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download Nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a>Cambiar el nombre de la respuesta de Nginx

Edite *src/http/ngx_http_header_filter_module.c*:

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a>Configurar las opciones y la compilación

Se necesita la biblioteca PCRE para expresiones regulares. Las expresiones regulares se usan en la directiva de ubicación de ngx_http_rewrite_module. http_ssl_module agrega compatibilidad con el protocolo HTTPS.

Considere el uso de un firewall de aplicación web como *ModSecurity* para proteger la aplicación.

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a>Configurar SSL

* Configurar el servidor para que escuche el tráfico HTTPS en el puerto `443` mediante la especificación de un certificado válido emitido por una entidad de confianza de certificados (CA).

* Reforzar la seguridad mediante el uso de algunos de los procedimientos descritos en la siguiente */etc/nginx/nginx.conf* archivo. Entre los ejemplos se incluye la elección de un cifrado más seguro y el redireccionamiento de todo el tráfico a través de HTTP a HTTPS.

* Agregar un encabezado `HTTP Strict-Transport-Security` (HSTS) garantiza que todas las solicitudes siguientes realizadas por el cliente sean solo a través de HTTPS.

* No agregue el encabezado de seguridad de transporte Strict o elige una adecuada `max-age` si SSL se deshabilitará en el futuro.

Agregue el archivo de configuración */etc/nginx/proxy.conf*:

[!code-nginx[](linux-nginx/proxy.conf)]

Edite el archivo de configuración */etc/nginx/nginx.conf*. El ejemplo contiene las dos secciones `http` y `server` en un archivo de configuración.

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a>Proteger Nginx frente al secuestro de clic
El secuestro de clic es una técnica malintencionada para recopilar los clics de un usuario infectado. El secuestro de clic engaña a la víctima (visitante) para que haga clic en un sitio infectado. X-FRAME-OPTIONS de la utilice para proteger el sitio.

Edite el archivo *nginx.conf*:

```bash
sudo nano /etc/nginx/nginx.conf
```

Agregue la línea `add_header X-Frame-Options "SAMEORIGIN";` y guarde el archivo, después, reinicie Nginx.

#### <a name="mime-type-sniffing"></a>Examen de tipo MIME

Este encabezado evita que la mayoría de los exploradores examinen el MIME de una respuesta fuera del tipo de contenido declarado, ya que el encabezado indica al explorador que no reemplace el tipo de contenido de respuesta. Con la opción `nosniff`, si el servidor indica que el contenido es "text/html", el explorador lo representa como "text/html".

Edite el archivo *nginx.conf*:

```bash
sudo nano /etc/nginx/nginx.conf
```

Agregue la línea `add_header X-Content-Type-Options "nosniff";`, guarde el archivo y después reinicie Nginx.
