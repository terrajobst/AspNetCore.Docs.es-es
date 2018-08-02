---
title: Hospedar ASP.NET Core en Linux con Apache
description: Aprenda a configurar Apache como servidor proxy inverso en CentOS para redirigir el tráfico HTTP a una aplicación web ASP.NET Core que se ejecuta en Kestrel.
author: spboyer
ms.author: spboyer
ms.custom: mvc
ms.date: 03/13/2018
uid: host-and-deploy/linux-apache
ms.openlocfilehash: 2431e989d6fc2cf83bca47aaa41a2bf686c0ab54
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219360"
---
# <a name="host-aspnet-core-on-linux-with-apache"></a>Hospedar ASP.NET Core en Linux con Apache

Por [Shayne Boyer](https://github.com/spboyer)

Mediante esta guía, aprenda a configurar [Apache](https://httpd.apache.org/) como servidor proxy inverso en [CentOS 7](https://www.centos.org/) para redirigir el tráfico HTTP a una aplicación web ASP.NET Core que se ejecuta en [Kestrel](xref:fundamentals/servers/kestrel). La [extensión mod_proxy](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) y los módulos relacionados crean el proxy inverso del servidor.

## <a name="prerequisites"></a>Requisitos previos

1. Servidor que ejecute CentOS 7, con una cuenta de usuario estándar con privilegios sudo.
1. Tener .NET Core Runtime instalado en el servidor.
   1. Vaya a la [página de descargas de .NET Core](https://www.microsoft.com/net/download/all).
   1. Seleccione el tiempo de ejecución más reciente que no sea versión preliminar en la lista **Runtime**.
   1. Seleccione y siga las instrucciones relativas a CentOS/Oracle.
1. Disponer de una aplicación de ASP.NET Core existente.

## <a name="publish-and-copy-over-the-app"></a>Publicar y copiar en la aplicación

Configure la aplicación para una [implementación dependiente de Framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd).

Ejecute [dotnet publish](/dotnet/core/tools/dotnet-publish) desde el entorno de desarrollo para empaquetar una aplicación en un directorio (por ejemplo, *bin/Release/&lt;target_framework_moniker&gt;/publish*) que se pueda ejecutar en el servidor:

```console
dotnet publish --configuration Release
```

La aplicación también se puede publicar como una [implementación independiente](/dotnet/core/deploying/#self-contained-deployments-scd) si prefiere no mantener .NET Core Runtime en el servidor.

Copie la aplicación de ASP.NET Core en el servidor usando una herramienta que se integre en el flujo de trabajo de la organización (como SCP o SFTP). Lo habitual es encontrar las aplicaciones web en el directorio *var* (por ejemplo, *aspnetcore/var/hellomvc*).

> [!NOTE]
> En un escenario de implementación de producción, un flujo de trabajo de integración continua lleva a cabo la tarea de publicar la aplicación y copiar los recursos en el servidor.

## <a name="configure-a-proxy-server"></a>Configurar un servidor proxy

Un proxy inverso es una configuración común para trabajar con aplicaciones web dinámicas. El proxy inverso finaliza la solicitud HTTP y la reenvía a la aplicación ASP.NET.

Un servidor proxy es el que reenvía las solicitudes de cliente a otro servidor en lugar de realizarlas él mismo. Los proxies inversos las reenvían a un destino fijo, normalmente en nombre de clientes arbitrarios. En esta guía, Apache se configura como proxy inverso que se ejecuta en el mismo servidor en el que Kestrel atiende la aplicación ASP.NET Core.

Como el proxy inverso reenvía las solicitudes, use el [Middleware de encabezados reenviados](xref:host-and-deploy/proxy-load-balancer) del paquete [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/). El middleware actualiza `Request.Scheme`, mediante el encabezado `X-Forwarded-Proto`, para que los URI de redireccionamiento y otras directivas de seguridad funcionen correctamente.

Cualquier componente que dependa del esquema (como la autenticación, la generación de vínculos, los redireccionamientos o la geolocalización) debe colocarse después de invocar al Middleware de encabezados reenviados. Como norma general, el Middleware de encabezados reenviados se debe ejecutar antes de cualquier otro middleware, salvo el middleware de diagnóstico y control de errores. Hacerlo en ese orden garantiza que el middleware que se basa en la información de encabezados reenviados pueda usar los valores de encabezado para procesarlos.

::: moniker range=">= aspnetcore-2.0"
> [!NOTE]
> Cualquiera de las configuraciones&mdash;con o sin un servidor proxy inverso&mdash;es una configuración de hospedaje válida y admitida para ASP.NET 2.0 o aplicaciones posteriores. Para más información, vea [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy) (Cuándo se debe usar Kestrel con un proxy inverso).
::: moniker-end

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

### <a name="install-apache"></a>Instalar Apache

Actualice los paquetes de CentOS a sus versiones estables más recientes:

```bash
sudo yum update -y
```

Instale el servidor web de Apache en CentOS con un único comando `yum`:

```bash
sudo yum -y install httpd mod_ssl
```

Salida de ejemplo después de ejecutar el comando:

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
> En este ejemplo, la salida refleja httpd.86_64, puesto que la versión de CentOS 7 es de 64 bits. Para comprobar dónde está instalado Apache, ejecute `whereis httpd` desde un símbolo del sistema.

### <a name="configure-apache"></a>Configurar Apache

Los archivos de configuración de Apache se encuentran en el directorio `/etc/httpd/conf.d/`. Todos los archivos que tengan la extensión *.conf* se procesan en orden alfabético, además de los archivos de configuración del módulo de `/etc/httpd/conf.modules.d/`, que contiene todos los archivos de configuración necesarios para cargar los módulos.

Cree un archivo de configuración denominado *hellomvc.conf* para la aplicación:

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

El bloque `VirtualHost` puede aparecer varias veces en uno o varios archivos en un servidor. En el archivo de configuración anterior, Apache acepta tráfico público en el puerto 80. El dominio `www.example.com` se atiende y el alias `*.example.com` se resuelve en el mismo sitio web. Para más información, consulte [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) (Compatibilidad con el host virtual basado en nombres). Las solicitudes se redirigen mediante proxy en la raíz al puerto 5000 del servidor en 127.0.0.1. Para la comunicación bidireccional, se requieren `ProxyPass` y `ProxyPassReverse`. Para cambiar la IP o el puerto de Kestrel, vea [Kestrel: configuración de punto de conexión](xref:fundamentals/servers/kestrel#endpoint-configuration).

> [!WARNING]
> Si no se especifica una [directiva de ServerName](https://httpd.apache.org/docs/current/mod/core.html#servername) correcta en **VirtualHost**, el bloque expone la aplicación a las vulnerabilidades de seguridad. Los enlaces de carácter comodín de subdominio (por ejemplo, `*.example.com`) no presentan este riesgo de seguridad si se controla todo el dominio primario (a diferencia de `*.com`, que sí es vulnerable). Vea la [sección 5.4 de RFC 7230](https://tools.ietf.org/html/rfc7230#section-5.4) para obtener más información.

El registro se puede configurar por `VirtualHost` con las directivas `ErrorLog` y `CustomLog`. `ErrorLog` es la ubicación donde el servidor registra los errores, y `CustomLog` establece el nombre de archivo y el formato del archivo de registro. En este caso, aquí es donde se registra la información de la solicitud. Hay una línea para cada solicitud.

Guarde el archivo y pruebe la configuración. Si se pasa todo, la respuesta debe ser `Syntax [OK]`.

```bash
sudo service httpd configtest
```

Reinicie Apache:

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a>Supervisión de la aplicación

Apache está configurado ahora para reenviar las solicitudes efectuadas a `http://localhost:80` en la aplicación ASP.NET Core que se ejecuta en Kestrel en `http://127.0.0.1:5000`.  En cambio, Apache no está configurado para administrar el proceso de Kestrel. Use *systemd* y cree un archivo de servicio para iniciar y supervisar la aplicación web subyacente. *systemd* es un sistema de inicio que proporciona muchas características eficaces para iniciar, detener y administrar procesos. 

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
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
SyslogIdentifier=dotnet-example
User=apache
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```

> [!NOTE]
> **Usuario**: si el usuario *apache* no se usa en la configuración, primero se debe crear el usuario y se le debe conceder la propiedad adecuada para los archivos.

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

Inicie el servicio y compruebe que se está ejecutando:

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

Con el proxy inverso configurado y Kestrel administrado mediante *systemd*, la aplicación web está completamente configurada y se puede acceder a ella desde un explorador en la máquina local en `http://localhost`. Inspeccionar los encabezados de respuesta; el encabezado **Server** indica que Kestrel atiende la aplicación ASP.NET Core:

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>Ver los registros

Dado que la aplicación web que usa Kestrel se administra mediante *systemd*, los procesos y eventos se registran en un diario centralizado. Sin embargo, este diario incluye todas las entradas de todos los servicios y procesos administrados por *systemd*. Para ver los elementos específicos de `kestrel-hellomvc.service`, use el siguiente comando:

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

Para el filtrado de tiempo, especifique las opciones de tiempo con el comando. Por ejemplo, use `--since today` para filtrar por el día actual o `--until 1 hour ago` para ver las entradas de la hora anterior. Para más información, consulte la [página man de journalctl](https://www.unix.com/man-page/centos/1/journalctl/).

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="data-protection"></a>Protección de datos

Hay varios [softwares intermedios](xref:fundamentals/middleware/index) de ASP.NET Core que utilizan la [pila de protección de datos de ASP.NET Core](xref:security/data-protection/index), incluidos los de autenticación, como el de cookies, y las protecciones de falsificación de solicitud entre sitios (CSRF). Aunque el código de usuario no llame a las API de protección de datos, esta se debe configurar para crear un [almacén de claves](xref:security/data-protection/implementation/key-management) criptográficas persistente. Si no se configura la protección de datos, las claves se conservan en memoria y se descartan cuando se reinicia la aplicación.

Si el conjunto de claves se almacena en memoria cuando se reinicia la aplicación:

* Todos los tokens de autenticación basados en cookies se invalidan.
* Los usuarios tienen que iniciar sesión de nuevo en la siguiente solicitud.
* Ya no se pueden descifrar los datos protegidos con el conjunto de claves. Esto puede incluir [tokens CSRF](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) y [cookies de TempData de ASP.NET Core MVC](xref:fundamentals/app-state#tempdata).

Para configurar la protección de datos de modo que sea persistente y permita cifrar el anillo de claves, consulte:

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>

## <a name="securing-the-app"></a>Protección de la aplicación

### <a name="configure-firewall"></a>Configurar el firewall

*Firewalld* es un demonio dinámico para administrar el firewall con compatibilidad con zonas de red. Los puertos y el filtrado de paquetes se pueden seguir administrando mediante iptables. *Firewalld* debe instalarse de forma predeterminada. `yum` puede usarse para instalar el paquete o comprobar que está instalado.

```bash
sudo yum install firewalld -y
```

Use `firewalld` para abrir solo los puertos necesarios para la aplicación. En este caso se usan los puertos 80 y 443. Los siguientes comandos establecen de forma permanente que se abran los puertos 80 y 443:

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

Vuelva a cargar la configuración del firewall. Compruebe los servicios y puertos disponibles en la zona predeterminada. Hay opciones disponibles si se inspecciona `firewall-cmd -h`.

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

Para configurar Apache para SSL, se usa el módulo *mod_ssl*. Cuando se instaló el módulo *httpd*, también lo hizo el módulo *mod_ssl*. Si aún no se ha instalado, use `yum` para agregarlo a la configuración.

```bash
sudo yum install mod_ssl
```

Para usar SSL, instale el módulo `mod_rewrite` para habilitar la reescritura de direcciones URL:

```bash
sudo yum install mod_rewrite
```

Modifique el archivo *hellomvc.conf* para permitir la reescritura de direcciones URL y proteger la comunicación en el puerto 443:

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
> En este ejemplo se usa un certificado generado localmente. **SSLCertificateFile** debe ser el archivo de certificado principal para el nombre de dominio. **SSLCertificateKeyFile** debe ser el archivo de claves generado al crear el CSR. **SSLCertificateChainFile** debe ser el archivo de certificado intermedio (si existe) proporcionado por la entidad de certificación.

Guarde el archivo y pruebe la configuración.

```bash
sudo service httpd configtest
```

Reinicie Apache:

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a>Sugerencias adicionales de Apache

### <a name="additional-headers"></a>Encabezados adicionales

Para protegerse frente a ataques malintencionados, hay unos encabezados que se deben modificar o agregar. Asegúrese de que el módulo `mod_headers` está instalado.

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a>Protección de Apache de los ataques de secuestro de clic

El [secuestro de clic](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), también conocido como *redireccionamiento de interfaz de usuario*, es un ataque malintencionado donde al visitante de un sitio web se le engaña para que haga clic en un vínculo o botón de una página distinta de la que actualmente está visitando. Use `X-FRAME-OPTIONS` para proteger el sitio.

Edite el archivo *httpd.conf*.

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

Agregue la línea `Header append X-FRAME-OPTIONS "SAMEORIGIN"`. Guarde el archivo. Reinicie Apache.

#### <a name="mime-type-sniffing"></a>Examen de tipo MIME

El encabezado `X-Content-Type-Options` impide que Internet Explorer *examine MIME* (de forma que el elemento `Content-Type` de un archivo se determina a partir del contenido del archivo). Si el servidor establece el encabezado `Content-Type` en `text/html` con la opción `nosniff` establecida, Internet Explorer representa el contenido como `text/html` sin tener en cuenta el contenido del archivo.

Edite el archivo *httpd.conf*.

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

Agregue la línea `Header set X-Content-Type-Options "nosniff"`. Guarde el archivo. Reinicie Apache.

### <a name="load-balancing"></a>Equilibrio de carga

En este ejemplo se muestra cómo instalar y configurar Apache en CentOS 7 y Kestrel en el mismo equipo de la instancia. Para no tener un único punto de error, el uso de *mod_proxy_balancer* y la modificación de **VirtualHost** permitirían administrar varias instancias de las aplicaciones web detrás del servidor proxy de Apache.

```bash
sudo yum install mod_proxy_balancer
```

En el archivo de configuración que se muestra a continuación, se configura una instancia adicional de la aplicación `hellomvc` para que se ejecute en el puerto 5001. La sección *Proxy* se establece con una configuración de equilibrador con dos miembros para equilibrar la carga *byrequests*.

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

### <a name="rate-limits"></a>Límites de velocidad

Use *mod_ratelimit*, que se incluye en el módulo *httpd*; el ancho de banda de los clientes puede ser limitado:

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
En el archivo de ejemplo se limita el ancho de banda a 600 KB/s en la ubicación raíz:

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```

## <a name="additional-resources"></a>Recursos adicionales

* [Configurar ASP.NET Core para trabajar con servidores proxy y equilibradores de carga](xref:host-and-deploy/proxy-load-balancer)
