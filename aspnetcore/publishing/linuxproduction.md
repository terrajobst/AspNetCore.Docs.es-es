---
title: Hospedar ASP.NET Core en Linux con Nginx
description: "Describe cómo configurar Nginx como un proxy inverso en Ubuntu 16.04 para reenviar el tráfico HTTP a una aplicación web de ASP.NET Core que se ejecuta en Kestrel."
keywords: ASP.NET Core, Linux, nginx, Ubuntu, proxy inverso
author: rick-anderson
ms.author: riande
manager: wpickett
ms.date: 08/21/2017
ms.topic: article
ms.assetid: 1c33e576-33de-481a-8ad3-896b94fde0e3
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/linuxproduction
ms.openlocfilehash: 1f2b5fc6d769c63110f832a31cd0d0aa8c3298e9
ms.sourcegitcommit: bd05f7ea8f87ad076ef6e8b704698ebcba5ca80c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/25/2017
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-nginx-and-deploy-to-it"></a>Configurar un entorno de hospedaje para ASP.NET Core en Linux con Nginx e implementar en él

Por [Sourabh Shirhatti](https://twitter.com/sshirhatti)

En esta guía se explica cómo configurar un entorno de ASP.NET Core listo para producción en un servidor de Ubuntu 16.04.

**Nota:** Para Ubuntu 14.04, se recomienda supervisord como una solución para supervisar el proceso de Kestrel. systemd no está disponible en Ubuntu 14.04. [Vea la versión anterior de este documento](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).

En esta guía:

* Se coloca una aplicación existente de ASP.NET Core detrás de un servidor proxy inverso
* Se configura el servidor proxy inverso para reenviar las solicitudes al servidor web de Kestrel
* Se asegura que la aplicación web se ejecuta al iniciar como un demonio
* Se configura una herramienta de administración de procesos para ayudar a reiniciar la aplicación web

## <a name="prerequisites"></a>Requisitos previos

1. Acceso a un servidor de Ubuntu 16.04 con una cuenta de usuario estándar con privilegios sudo
2. Una aplicación de ASP.NET Core existente

## <a name="copy-over-your-app"></a>Copiar a través de la aplicación

Ejecute `dotnet publish` desde el entorno de desarrollo para empaquetar una aplicación en un directorio independiente que se puede ejecutar en el servidor.

Copie la aplicación de ASP.NET Core en el servidor mediante cualquier herramienta (SCP, FTP, etc.) que se integre en el flujo de trabajo. Pruebe la aplicación, por ejemplo:
 - Ejecute `dotnet yourapp.dll` desde la línea de comandos.
 - En un explorador, vaya a `http://<serveraddress>:<port>` para comprobar que la aplicación funciona en Linux. 
 
**Nota:** Use [Yeoman](xref:client-side/yeoman) para crear una aplicación de ASP.NET Core para un nuevo proyecto.

## <a name="configure-a-reverse-proxy-server"></a>Configurar un servidor proxy inverso

Un proxy inverso es una configuración común para usar aplicaciones web dinámicas. Un proxy inverso finaliza la solicitud HTTP y la reenvía a la aplicación de ASP.NET Core.

### <a name="why-use-a-reverse-proxy-server"></a>¿Por qué usar un servidor proxy inverso?

Kestrel es una herramienta excelente para usar contenido dinámico de ASP.NET Core; con todo, los elementos de servicio web no tienen tantas características como los servidores como IIS, Apache o Nginx. Un servidor proxy inverso puede descargar trabajo (por ejemplo, usar contenido estático, almacenar solicitudes en caché, comprimir solicitudes y finalizar SSL desde el servidor HTTP). Un servidor proxy inverso puede residir en un equipo dedicado o se puede implementar junto con un servidor HTTP.

Para los fines de esta guía, se usa una única instancia de Nginx. Se ejecuta en el mismo servidor, junto con el servidor HTTP. Según sus requisitos, puede elegir otra configuración.

Dado que el proxy inverso reenvía las solicitudes, use el software intermedio `ForwardedHeaders` del paquete `Microsoft.AspNetCore.HttpOverrides`. El software intermedio actualiza `Request.Scheme`, mediante el encabezado `X-Forwarded-Proto`, para que esos URI de redirección y otras directivas de seguridad funcionen correctamente.

Al configurar un servidor proxy inverso, el software intermedio de autenticación necesita que se ejecute `UseForwardedHeaders` primero. Este orden garantiza que el software intermedio de autenticación puede consumir los valores afectados y generar URI de redirección correctos.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Invoque el método `UseForwardedHeaders` (en el método `Configure` de *Startup.cs*) antes de llamar a `UseAuthentication` o un software intermedio de esquema de autenticación similar:

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Invoque el método `UseForwardedHeaders` (en el método `Configure` de *Startup.cs*) antes de llamar a `UseIdentity` y `UseFacebookAuthentication` o un software intermedio de esquema de autenticación similar:

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

### <a name="install-nginx"></a>Instalar Nginx

```bash
sudo apt-get install nginx
```

> [!NOTE]
> Si planea instalar los módulos opcionales de Nginx, es posible que deba compilar Nginx desde el origen.

Use `apt-get` para instalar Nginx. El instalador crea un script de inicio de System V que ejecuta Nginx como demonio al iniciarse el sistema. Puesto que Nginx se ha instalado por primera vez, ejecute lo siguiente para iniciarlo de forma explícita:

```bash
sudo service nginx start
```

Compruebe que un explorador muestra la página de aterrizaje predeterminada de Nginx.

### <a name="configure-nginx"></a>Configurar Nginx

Para configurar Nginx como un proxy inverso a fin de que reenvíe solicitudes a nuestra aplicación de ASP.NET Core, modifique `/etc/nginx/sites-available/default`. Ábralo en un editor de texto y reemplace el contenido por el siguiente:

```nginx
server {
    listen 80;
    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

Este archivo de configuración de Nginx reenvía tráfico público entrante del puerto `80` al puerto `5000`.

Una vez que haya acabado de realizar cambios en la configuración de Nginx, puede ejecutar `sudo nginx -t` para comprobar la sintaxis de los archivos de configuración. Si la prueba de los archivos de configuración es correcta, puede pedir a Nginx que recopile los cambios al ejecutar `sudo nginx -s reload`.

## <a name="monitoring-our-application"></a>Supervisar la aplicación

Nginx ahora está configurado para reenviar las solicitudes realizadas a `http://yourhost:80` en la aplicación de ASP.NET Core que se ejecuta en Kestrel en `http://127.0.0.1:5000`. En cambio, Nginx no está configurado para administrar el proceso de Kestrel. Puede usar *systemd* y crear un archivo de servicio para iniciar y supervisar la aplicación web subyacente. *systemd* es un sistema de inicio que proporciona muchas características eficaces para iniciar, detener y administrar procesos. 

### <a name="create-the-service-file"></a>Crear el archivo de servicio

Cree el archivo de definición de servicio:

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

A continuación tiene un archivo de servicio de ejemplo de nuestra aplicación:

```ini
[Unit]
Description=Example .NET Web API Application running on Ubuntu

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
RestartSec=10  # Restart service after 10 seconds if dotnet service crashes
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```

**Nota:** Si la configuración no emplea el usuario *www-data*, el usuario definido aquí se debe crear primero y debe proporcionársele la propiedad adecuada para los archivos.

Guarde el archivo y habilite el servicio.

```bash
systemctl enable kestrel-hellomvc.service
```

Inicie el servicio y compruebe que se está ejecutando.

```
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API Application running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

Con el proxy inverso configurado y Kestrel administrado a través de systemd, la aplicación web está completamente configurada y se puede tener acceso a ella desde un explorador en el equipo local en `http://localhost`. También es accesible desde un equipo remoto, salvo que haya algún firewall que la pueda estar bloqueando. Al inspeccionar los encabezados de respuesta, el encabezado `Server` muestra la aplicación de ASP.NET Core que proporciona Kestrel.

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>Ver registros

Dado que la aplicación web que usa Kestrel se administra mediante `systemd`, todos los procesos y eventos se registran en un diario centralizado. En cambio, este diario incluye todas las entradas de todos los servicios y procesos administrados por `systemd`. Para ver los elementos específicos de `kestrel-hellomvc.service`, use el siguiente comando:

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

Para obtener más opciones de filtrado, las opciones de tiempo como `--since today`, `--until 1 hour ago` o una combinación de estas pueden reducir la cantidad de entradas que se devuelven.

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a>Proteger la aplicación

### <a name="enable-apparmor"></a>Habilitar AppArmor

Linux Security Modules (LSM) es un marco que forma parte del kernel de Linux desde Linux 2.6. LSM admite diferentes implementaciones de los módulos de seguridad. [AppArmor](https://wiki.ubuntu.com/AppArmor) es un LSM que implementa un sistema de control de acceso obligatorio que permite restringir el programa a un conjunto limitado de recursos. Asegúrese de que AppArmor está habilitado y configurado correctamente.

### <a name="configuring-our-firewall"></a>Configurar el firewall

Cierre todos los puertos externos que no estén en uso. Uncomplicated Firewall (ufw) proporciona un front-end para `iptables` al proporcionar una interfaz de línea de comandos para configurar el firewall. Compruebe que `ufw` está configurado para permitir el tráfico en los puertos que necesita.

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

# Download nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a>Cambiar el nombre de la respuesta de Nginx

Edite *src/http/ngx_http_header_filter_module.c*:

```c
static char ngx_http_server_string[] = "Server: Your Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Your Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a>Configurar las opciones y la compilación

Se necesita la biblioteca PCRE para expresiones regulares. Las expresiones regulares se usan en la directiva de ubicación de ngx_http_rewrite_module. http_ssl_module agrega compatibilidad con el protocolo HTTPS.

Considere la opción de usar un firewall de aplicaciones web como *ModSecurity* para proteger la aplicación.

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a>Configurar SSL

* Configure el servidor para que escuche el tráfico HTTPS en el puerto `443`. Para ello, especifique un certificado válido emitido por una entidad de certificados (CA) de confianza.

* Refuerce la seguridad con algunos de los procedimientos descritos en el siguiente archivo */etc/nginx/nginx.conf*. Entre los ejemplos se incluye la elección de un cifrado más seguro y el redireccionamiento de todo el tráfico a través de HTTP a HTTPS.

* Agregar un encabezado `HTTP Strict-Transport-Security` (HSTS) garantiza que todas las solicitudes siguientes realizadas por el cliente sean solo a través de HTTPS.

* No agregue el encabezado Strict-Transport-Security o elija un valor de `max-age` adecuado si tiene previsto deshabilitar SSL en el futuro.

Agregue el archivo de configuración */etc/nginx/proxy.conf*:

[!code-nginx[Main](linuxproduction/proxy.conf)]

Edite el archivo de configuración */etc/nginx/nginx.conf*. El ejemplo contiene las dos secciones `http` y `server` en un archivo de configuración.

[!code-nginx[Main](../publishing/linuxproduction/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a>Proteger Nginx frente al secuestro de clic
El secuestro de clic es una técnica malintencionada para recopilar los clics de un usuario infectado. El secuestro de clic engaña a la víctima (visitante) para que haga clic en un sitio infectado. Use X-FRAME-OPTIONS para proteger su sitio.

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
