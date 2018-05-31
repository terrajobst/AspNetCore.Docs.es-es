---
title: Hospedar ASP.NET Core en Linux con Nginx
author: rick-anderson
description: Se describe cómo configurar Nginx como un proxy inverso en Ubuntu 16.04 para reenviar el tráfico HTTP a una aplicación web de ASP.NET Core que se ejecuta en Kestrel.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: d37aa25c712d715aa4134587a84e5923f9cb5b79
ms.sourcegitcommit: 50d40c83fa641d283c097f986dde5341ebe1b44c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2018
ms.locfileid: "34452560"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a>Hospedar ASP.NET Core en Linux con Nginx

Por [Sourabh Shirhatti](https://twitter.com/sshirhatti)

En esta guía se explica cómo configurar un entorno de ASP.NET Core listo para producción en un servidor de Ubuntu 16.04.

> [!NOTE]
> En Ubuntu 14.04, *supervisord* is es la solución recomendada para supervisar el proceso de Kestrel. *systemd* no está disponible en Ubuntu 14.04. [Consulte la versión anterior de este documento](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).

En esta guía:

* Se coloca una aplicación ASP.NET Core existente detrás de un servidor proxy inverso.
* Se configura el servidor proxy inverso para reenviar las solicitudes al servidor web de Kestrel.
* Se garantiza que la aplicación web se ejecute al inicio como un demonio.
* Se configura una herramienta de administración de procesos para ayudar a reiniciar la aplicación web.

## <a name="prerequisites"></a>Requisitos previos

1. Acceso a un servidor de Ubuntu 16.04 con una cuenta de usuario estándar con privilegios sudo
1. Una aplicación ASP.NET Core existente

## <a name="copy-over-the-app"></a>Copia a través de la aplicación

Ejecute [dotnet publish](/dotnet/core/tools/dotnet-publish) desde el entorno de desarrollo para empaquetar una aplicación en un directorio independiente que pueda ejecutarse en el servidor.

Copie la aplicación ASP.NET Core en el servidor mediante cualquiera de las herramientas que se integran en el flujo de trabajo de la organización (por ejemplo, SCP, FTP). Pruebe la aplicación, por ejemplo:

* Ejecute `dotnet <app_assembly>.dll` desde la línea de comandos.
* En un explorador, vaya a `http://<serveraddress>:<port>` para comprobar que la aplicación funciona en Linux. 
 
## <a name="configure-a-reverse-proxy-server"></a>Configurar un servidor proxy inverso

Un proxy inverso es una configuración común para trabajar con aplicaciones web dinámicas. Un proxy inverso finaliza la solicitud HTTP y la reenvía a la aplicación ASP.NET Core.

### <a name="why-use-a-reverse-proxy-server"></a>¿Por qué usar un servidor proxy inverso?

Kestrel resulta muy adecuado para suministrar contenido dinámico de ASP.NET Core. Sin embargo, las funcionalidades de servicio web no son tan completas como las de los servidores, como IIS, Apache o Nginx. Un servidor proxy inverso puede descargar trabajo, por ejemplo, suministrar contenido estático, almacenar solicitudes en caché, comprimir solicitudes y finalizar SSL desde el servidor HTTP. Un servidor proxy inverso puede residir en un equipo dedicado o se puede implementar junto con un servidor HTTP.

Para los fines de esta guía, se usa una única instancia de Nginx. Se ejecuta en el mismo servidor, junto con el servidor HTTP. En función de requisitos, se puede elegir una configuración diferente.

Como el proxy inverso reenvía las solicitudes, use el Middleware de encabezados reenviados del paquete [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/). El middleware actualiza `Request.Scheme`, mediante el encabezado `X-Forwarded-Proto`, para que los URI de redireccionamiento y otras directivas de seguridad funcionen correctamente.

Al usar cualquier tipo de middleware de autenticación, el Middleware de encabezados reenviados debe ejecutarse primero. Este orden garantiza que el middleware de autenticación puede consumir los valores de encabezado y generar los URI de redireccionamiento correctos.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Invoque el método [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) en `Startup.Configure` antes de llamar a [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) o a un middleware de esquema de autenticación similar. Configure el middleware para reenviar los encabezados `X-Forwarded-For` y `X-Forwarded-Proto`:

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Invoque el método [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) en `Startup.Configure` antes de llamar a [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) y [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) o un middleware de esquema de autenticación similar. Configure el middleware para reenviar los encabezados `X-Forwarded-For` y `X-Forwarded-Proto`:

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

Si no se especifica ningún valor [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) para el software intermedio, los encabezados predeterminados para reenviar son `None`.

Podría ser necesario realizar una configuración adicional para las aplicaciones hospedadas detrás de servidores proxy y equilibradores de carga. Para más información, vea [Configurar ASP.NET Core para trabajar con servidores proxy y equilibradores de carga](xref:host-and-deploy/proxy-load-balancer).

### <a name="install-nginx"></a>Instalar Nginx

```bash
sudo apt-get install nginx
```

> [!NOTE]
> Si se van a instalar módulos de Nginx opcionales, podría ser necesario compilar Nginx desde el origen.

Use `apt-get` para instalar Nginx. El instalador crea un script de inicio de System V que ejecuta Nginx como demonio al iniciarse el sistema. Puesto que Nginx se ha instalado por primera vez, ejecute lo siguiente para iniciarlo de forma explícita:

```bash
sudo service nginx start
```

Compruebe que un explorador muestra la página de aterrizaje predeterminada de Nginx.

### <a name="configure-nginx"></a>Configurar Nginx

Para configurar Nginx como un proxy inverso para reenviar solicitudes a la aplicación ASP.NET Core, modifique */etc/nginx/sites-available/default*. Ábralo en un editor de texto y reemplace el contenido por el siguiente:

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

Cuando no hay ninguna coincidencia de `server_name`, Nginx usa el servidor predeterminado. Si no se define ningún servidor predeterminado, el primer servidor del archivo de configuración es el servidor predeterminado. Como procedimiento recomendado, agregue un servidor predeterminado específico que devuelva un código de estado 444 en el archivo de configuración. Un ejemplo de configuración del servidor predeterminado es:

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

Con el archivo de configuración anterior y el servidor predeterminado, Nginx acepta tráfico público en el puerto 80 con el encabezado de host `example.com` o `*.example.com`. Las solicitudes que no coincidan con estos hosts no se reenviarán al Kestrel. Nginx reenvía las solicitudes coincidentes con Kestrel a `http://localhost:5000`. Para más información, consulte [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) (Cómo Nginx procesa una solicitud).

> [!WARNING]
> Si no se especifica una [directiva de server_name](https://nginx.org/docs/http/server_names.html) adecuada, su aplicación se expone a vulnerabilidades de seguridad. Los enlaces de carácter comodín de subdominio (por ejemplo, `*.example.com`) no presentan este riesgo de seguridad si se controla todo el dominio primario (a diferencia de `*.com`, que sí es vulnerable). Vea la [sección 5.4 de RFC 7230](https://tools.ietf.org/html/rfc7230#section-5.4) para obtener más información.

Una vez establecida la configuración de Nginx, ejecute `sudo nginx -t` para comprobar la sintaxis de los archivos de configuración. Si la prueba del archivo de configuración es correcta, fuerce a Nginx a recopilar los cambios mediante la ejecución de `sudo nginx -s reload`.

## <a name="monitoring-the-app"></a>Supervisión de la aplicación

Nginx ahora está configurado para reenviar las solicitudes realizadas a `http://<serveraddress>:80` en la aplicación de ASP.NET Core que se ejecuta en Kestrel en `http://127.0.0.1:5000`. Sin embargo, Nginx no está configurado para administrar el proceso de Kestrel. Se puede usar *systemd* para crear un archivo de servicio para iniciar y supervisar la aplicación web subyacente. *systemd* es un sistema de inicio que proporciona muchas características eficaces para iniciar, detener y administrar procesos. 

### <a name="create-the-service-file"></a>Crear el archivo de servicio

Cree el archivo de definición de servicio:

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

Este es un archivo de servicio de ejemplo de la aplicación:

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

Si el usuario *www-data* no se usó en la configuración, primero se debe crear el usuario aquí definido y otorgársele la propiedad adecuada de los archivos.

Linux tiene un sistema de archivos que distingue mayúsculas de minúsculas. Al establecer ASPNETCORE_ENVIRONMENT en "Production", se busca el archivo de configuración *appsettings.Production.json*, en lugar de *appsettings.production.json*.

> [!NOTE]
> Algunos valores (por ejemplo, cadenas de conexión de SQL) deben ser de escape para que los proveedores de configuración lean las variables de entorno. Use el siguiente comando para generar un valor de escape correctamente para su uso en el archivo de configuración:
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

Guarde el archivo y habilite el servicio.

```bash
systemctl enable kestrel-hellomvc.service
```

Inicie el servicio y compruebe que se está ejecutando.

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

Con el proxy inverso configurado y Kestrel administrado a través de systemd, la aplicación web está completamente configurada y se puede acceder a ella desde un explorador en la máquina local en `http://localhost`. También es accesible desde una máquina remota, salvo que haya algún firewall que la pueda estar bloqueando. Al inspeccionar los encabezados de respuesta, el encabezado `Server` muestra que Kestrel suministra la aplicación ASP.NET Core.

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>Ver los registros

Dado que la aplicación web que usa Kestrel se administra mediante `systemd`, todos los procesos y eventos se registran en un diario centralizado. En cambio, este diario incluye todas las entradas de todos los servicios y procesos administrados por `systemd`. Para ver los elementos específicos de `kestrel-hellomvc.service`, use el siguiente comando:

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

Para obtener más opciones de filtrado, las opciones de tiempo como `--since today`, `--until 1 hour ago` o una combinación de estas pueden reducir la cantidad de entradas que se devuelven.

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a>Protección de la aplicación

### <a name="enable-apparmor"></a>Habilitar AppArmor

Linux Security Modules (LSM) es una plataforma que forma parte del kernel de Linux desde Linux 2.6. LSM admite diferentes implementaciones de los módulos de seguridad. [AppArmor](https://wiki.ubuntu.com/AppArmor) es un LSM que implementa un sistema de control de acceso obligatorio que permite restringir el programa a un conjunto limitado de recursos. Asegúrese de que AppArmor está habilitado y configurado correctamente.

### <a name="configuring-the-firewall"></a>Configuración del firewall

Cierre todos los puertos externos que no estén en uso. Uncomplicated Firewall (ufw) proporciona un front-end para `iptables` al proporcionar una interfaz de línea de comandos para configurar el firewall. Compruebe que `ufw` está configurado para permitir el tráfico en los puertos necesarios.

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

* Configure el servidor para que escuche el tráfico HTTPS en el puerto `443`. Para ello, especifique un certificado válido emitido por una entidad de certificados (CA) de confianza.

* Refuerce la seguridad con algunos de los procedimientos descritos en el siguiente archivo */etc/nginx/nginx.conf*. Entre los ejemplos se incluye la elección de un cifrado más seguro y el redireccionamiento de todo el tráfico a través de HTTP a HTTPS.

* Agregar un encabezado `HTTP Strict-Transport-Security` (HSTS) garantiza que todas las solicitudes siguientes realizadas por el cliente sean solo a través de HTTPS.

* No agregue el encabezado Strict-Transport-Security o elija un valor de `max-age` adecuado si tiene previsto deshabilitar SSL en el futuro.

Agregue el archivo de configuración */etc/nginx/proxy.conf*:

[!code-nginx[](linux-nginx/proxy.conf)]

Edite el archivo de configuración */etc/nginx/nginx.conf*. El ejemplo contiene las dos secciones `http` y `server` en un archivo de configuración.

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

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
