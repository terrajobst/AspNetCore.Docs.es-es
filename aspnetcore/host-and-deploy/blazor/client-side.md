---
title: Hospedaje e implementación de Blazor del lado cliente
author: guardrex
description: Obtenga información sobre cómo hospedar e implementar una aplicación de Blazor con ASP.NET Core, redes de entrega de contenido (CDN), servidores de archivos y GitHub Pages.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/21/2019
uid: host-and-deploy/blazor/client-side
ms.openlocfilehash: b50516b4dce28a6b105b2ab8b9386060d5392983
ms.sourcegitcommit: 4d05e30567279072f1b070618afe58ae1bcefd5a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376402"
---
# <a name="host-and-deploy-blazor-client-side"></a>Hospedaje e implementación de Blazor del lado cliente

Por [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) y [Daniel Roth](https://github.com/danroth27)

## <a name="host-configuration-values"></a>Valores de configuración de host

Las aplicaciones de Blazor que usan el [modelo de hospedaje del lado cliente](xref:blazor/hosting-models#client-side) pueden aceptar los siguientes valores de configuración de host como argumentos de línea de comandos en tiempo de ejecución en el entorno de desarrollo.

### <a name="content-root"></a>Raíz del contenido

El argumento `--contentroot` establece la ruta de acceso absoluta al directorio que incluye los archivos de contenido de la aplicación. En los ejemplos siguientes, `/content-root-path` es la ruta de acceso raíz del contenido de la aplicación.

* Pase el argumento al ejecutar la aplicación de forma local en un símbolo del sistema. En el directorio de la aplicación, ejecute lo siguiente:

  ```console
  dotnet run --contentroot=/content-root-path
  ```

* Agregue una entrada al archivo *launchSettings.json* de la aplicación en el perfil **IIS Express**. Esta configuración se utiliza cuando se ejecuta la aplicación mediante el depurador de Visual Studio y desde un símbolo del sistema con `dotnet run`.

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* En Visual Studio, especifique el argumento en **Propiedades** > **Depuración** > **Argumentos de la aplicación**. Al establecer el argumento en la página de propiedades de Visual Studio, se agrega el argumento al archivo *launchSettings.json*.

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a>Ruta de acceso base

El argumento `--pathbase` establece la ruta de acceso base de la aplicación para una aplicación que se ejecuta localmente con una ruta de acceso virtual que no es raíz (el valor `href` de la etiqueta `<base>` se establece en una ruta de acceso que no sea `/` para ensayo y producción). En los ejemplos siguientes, `/virtual-path` es la ruta de acceso base de la aplicación. Para obtener más información, vea la sección [Ruta de acceso base de la aplicación](#app-base-path).

> [!IMPORTANT]
> A diferencia de la ruta de acceso proporcionada al valor `href` de la etiqueta `<base>`, no incluya una barra diagonal final (`/`) al pasar el valor del argumento `--pathbase`. Si se proporciona la ruta de acceso base de la aplicación en la etiqueta `<base>` como `<base href="/CoolApp/">` (se incluye una barra diagonal final), se pasa el valor del argumento de línea de comandos como `--pathbase=/CoolApp` (sin barra diagonal final).

* Pase el argumento al ejecutar la aplicación de forma local en un símbolo del sistema. En el directorio de la aplicación, ejecute lo siguiente:

  ```console
  dotnet run --pathbase=/virtual-path
  ```

* Agregue una entrada al archivo *launchSettings.json* de la aplicación en el perfil **IIS Express**. Esta configuración se utiliza cuando se ejecuta la aplicación mediante el depurador de Visual Studio y desde un símbolo del sistema con `dotnet run`.

  ```json
  "commandLineArgs": "--pathbase=/virtual-path"
  ```

* En Visual Studio, especifique el argumento en **Propiedades** > **Depuración** > **Argumentos de la aplicación**. Al establecer el argumento en la página de propiedades de Visual Studio, se agrega el argumento al archivo *launchSettings.json*.

  ```console
  --pathbase=/virtual-path
  ```

### <a name="urls"></a>Direcciones URL

El argumento `--urls` establece las direcciones IP o las direcciones de host con los puertos y protocolos en los que escuchar las solicitudes.

* Pase el argumento al ejecutar la aplicación de forma local en un símbolo del sistema. En el directorio de la aplicación, ejecute lo siguiente:

  ```console
  dotnet run --urls=http://127.0.0.1:0
  ```

* Agregue una entrada al archivo *launchSettings.json* de la aplicación en el perfil **IIS Express**. Esta configuración se utiliza cuando se ejecuta la aplicación mediante el depurador de Visual Studio y desde un símbolo del sistema con `dotnet run`.

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* En Visual Studio, especifique el argumento en **Propiedades** > **Depuración** > **Argumentos de la aplicación**. Al establecer el argumento en la página de propiedades de Visual Studio, se agrega el argumento al archivo *launchSettings.json*.

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="deployment"></a>Implementación

Con el [modelo de hospedaje del lado cliente](xref:blazor/hosting-models#client-side):

* La aplicación Blazor, sus dependencias y el entorno de ejecución de .NET se descargan en el explorador.
* La aplicación se ejecuta directamente en el subproceso de interfaz de usuario del explorador. Se puede seguir cualquiera de las estrategias siguientes:
  * Una aplicación ASP.NET Core proporciona la aplicación Blazor. Esta estrategia se trata en la sección [Implementación hospedada con ASP.NET Core](#hosted-deployment-with-aspnet-core).
  * La aplicación Blazor se coloca en un servicio o servidor web de hospedaje estático, donde no se usa .NET para proporcionar la aplicación Blazor. Esta estrategia se trata en la sección [Implementación independiente](#standalone-deployment).

## <a name="configure-the-linker"></a>Configurar el enlazador

Blazor realiza la vinculación de lenguaje intermedio (IL) en cada compilación para quitar el IL innecesario de los ensamblados de salida. La vinculación de ensamblados puede controlarse en la compilación. Para obtener más información, vea <xref:host-and-deploy/blazor/configure-linker>.

## <a name="rewrite-urls-for-correct-routing"></a>Reescritura de las URL para conseguir un enrutamiento correcto

Enrutar las solicitudes para los componentes de la página en una aplicación del lado cliente no es tan sencillo como enrutar las solicitudes a una aplicación hospedada del lado servidor. Imagine que tiene una aplicación del lado cliente con dos páginas:

* **_Main.razor**: se carga en la raíz de la aplicación y contiene un vínculo a la página de información (`href="About"`).
* **_About.Razor**: página Acerca de.

Cuando se solicita el documento predeterminado de la aplicación mediante la barra de direcciones del explorador (por ejemplo, `https://www.contoso.com/`):

1. El explorador realiza una solicitud.
1. Se devuelve la página predeterminada, que suele ser *index.html*.
1. *index.html* arranca la aplicación.
1. Se carga el enrutador de Blazor y se muestra la página principal de Razor (*Main.razor*).

En la página principal, al seleccionar el vínculo a la página de información, se carga la página de información. Seleccionar el vínculo a la página de información funciona en el cliente porque el enrutador de Blazor impide que el explorador realice una solicitud en Internet a `www.contoso.com` sobre `About` y presenta la propia página de información. Todas las solicitudes de páginas internas *dentro de la aplicación del lado cliente* funcionan del mismo modo: Las solicitudes no desencadenan solicitudes basadas en el explorador a recursos hospedados en el servidor en Internet. El enrutador controla las solicitudes de forma interna.

Si se realiza una solicitud mediante la barra de direcciones del explorador para `www.contoso.com/About`, se produce un error. Este recurso no existe en el host de Internet de la aplicación, por lo que se devuelve una respuesta *404 No encontrado*.

Dado que los exploradores realizan solicitudes a hosts basados en Internet para las páginas del lado cliente, los servidores web y los servicios de hospedaje deben reescribir todas las solicitudes de recursos que no estén físicamente en el servidor a la página *index.html*. Cuando se devuelve *index.html*, el enrutador de la aplicación del lado cliente se hace cargo y responde con el recurso correcto.

## <a name="app-base-path"></a>Ruta de acceso base de la aplicación

La *ruta de acceso base de la aplicación* es la ruta de acceso raíz de la aplicación virtual en el servidor. Por ejemplo, a una aplicación que reside en el servidor de Contoso, en una carpeta virtual de `/CoolApp/`, se accede desde `https://www.contoso.com/CoolApp`; su ruta de acceso base virtual es `/CoolApp/`. Al establecer la ruta de acceso base de la aplicación en la ruta de acceso virtual (`<base href="/CoolApp/">`), la aplicación sabe dónde reside virtualmente en el servidor. La aplicación puede usar la ruta de acceso base de la aplicación para construir direcciones URL relativas a la raíz de la aplicación desde un componente que no se encuentre en el directorio raíz. Esto permite a los componentes que existen en diferentes niveles de la estructura de directorios compilar vínculos a otros recursos en ubicaciones de toda la aplicación. La ruta de acceso base de la aplicación también se usa para interceptar clics en hipervínculos en los que el destino `href` del vínculo está dentro del espacio de URI de la ruta de acceso base de la aplicación y es el enrutador de Blazor quien controla la navegación interna.

En muchos escenarios de hospedaje, la ruta de acceso virtual del servidor a la aplicación es la raíz de la aplicación. En estos casos, la ruta de acceso base de la aplicación es una barra diagonal (`<base href="/" />`), que es la configuración predeterminada para una aplicación. En otros escenarios de hospedaje, como las subaplicaciones o los directorios virtuales de IIS y GitHub Pages, la ruta de acceso base de la aplicación debe establecerse en la ruta de acceso virtual del servidor a la aplicación. Para establecer la ruta de acceso base de la aplicación, actualice la etiqueta `<base>` dentro de los elementos de etiqueta `<head>` del archivo *wwwroot/index.HTML*. Establezca el valor del atributo `href` en `/virtual-path/` (la barra diagonal final es necesaria), donde `/virtual-path/` es la ruta de acceso raíz de la aplicación virtual completa en el servidor para la aplicación. En el ejemplo anterior, se establece la ruta de acceso virtual en `/CoolApp/`: `<base href="/CoolApp/">`.

En el caso de una aplicación con una ruta de acceso virtual que no es raíz configurada (por ejemplo, `<base href="/CoolApp/">`), la aplicación no puede encontrar sus recursos *cuando se ejecuta de forma local*. Para solucionar este problema durante la fase de desarrollo y pruebas local, puede proporcionar un argumento de *ruta de acceso base* que coincida con el valor `href` de la etiqueta `<base>` en tiempo de ejecución.

Para pasar el argumento de ruta de acceso base con la ruta de acceso raíz (`/`) al ejecutar la aplicación de forma local, ejecute el comando `dotnet run` desde el directorio de la aplicación con la opción `--pathbase`:

```console
dotnet run --pathbase=/{Virtual Path (no trailing slash)}
```

Para una aplicación con una ruta de acceso virtual base de `/CoolApp/` (`<base href="/CoolApp/">`), el comando es el siguiente:

```console
dotnet run --pathbase=/CoolApp
```

La aplicación responde de forma local en `http://localhost:port/CoolApp`.

Para más información, vea la sección sobre el [valor de configuración de host de la ruta de acceso base](#path-base).

Si una aplicación usa el [modelo de hospedaje del lado cliente](xref:blazor/hosting-models#client-side) (basado en la plantilla de proyecto de **Blazor**; la plantilla `blazor` al usar el comando [dotnet new](/dotnet/core/tools/dotnet-new)) y se hospeda como una subaplicación de IIS en una aplicación ASP.NET Core, es importante deshabilitar el controlador del módulo de ASP.NET Core heredado o asegurarse de que la subaplicación no hereda la sección `<handlers>` de la aplicación raíz (principal) en el archivo *web.config*.

Para quitar el controlador del archivo *web.config* publicado de la aplicación, agregue una sección `<handlers>` al archivo:

```xml
<handlers>
  <remove name="aspNetCore" />
</handlers>
```

Como alternativa, deshabilite la herencia de la sección `<system.webServer>` de la aplicación raíz (principal) mediante un elemento `<location>` con `inheritInChildApplications` establecido en `false`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" ... />
      </handlers>
      <aspNetCore ... />
    </system.webServer>
  </location>
</configuration>
```

Además de configurarse la ruta de acceso base de la aplicación, se quita el controlador o se deshabilita la herencia, como se describe en esta sección. Establezca la ruta de acceso base de la aplicación en el archivo *index.html* de la aplicación en el alias de IIS que ha usado al configurar la subaplicación en IIS.

## <a name="hosted-deployment-with-aspnet-core"></a>Implementación hospedada con ASP.NET Core

Una *implementación hospedada* proporciona la aplicación Blazor del lado cliente a los exploradores desde una [aplicación ASP.NET Core](xref:index) que se ejecuta en un servidor.

La aplicación Blazor se incluye con la aplicación ASP.NET Core en la salida publicada para que ambas se implementen juntas. Se requiere un servidor web que pueda hospedar una aplicación ASP.NET Core. En el caso de una implementación hospedada, Visual Studio incluye la plantilla de proyecto de **Blazor (hospedada en ASP.NET Core)** (la plantilla `blazorhosted` al usar el comando [dotnet new](/dotnet/core/tools/dotnet-new)).

Para obtener más información sobre la implementación y el hospedaje de aplicaciones de ASP.NET Core, consulte <xref:host-and-deploy/index>.

Para obtener información sobre cómo implementar en Azure App Service, vea <xref:tutorials/publish-to-azure-webapp-using-vs>.

## <a name="standalone-deployment"></a>Implementación independiente

Una *implementación independiente* proporciona la aplicación Blazor del lado cliente como un conjunto de archivos estáticos que los clientes solicitan directamente. Cualquier servidor de archivos estático es capaz de servir a la aplicación Blazor.

Los activos de implementación independientes se publican en la carpeta */bin/Release/{RED DE DESTINO}/publish/{NOMBRE DE ENSAMBLADO}/dist*.

### <a name="iis"></a>IIS

IIS es un servidor de archivos estáticos compatible con las aplicaciones de Blazor. Para configurar IIS para hospedar Blazor, vea [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis) (Compilación de un sitio web estático en IIS).

Los recursos publicados se crean en la carpeta */bin/Release/{TARGET FRAMEWORK}/publish*. Hospede el contenido de la carpeta *publish* en el servidor web o el servicio de hospedaje.

#### <a name="webconfig"></a>web.config

Cuando se publica un proyecto de Blazor, se crea un archivo *web.config* con la siguiente configuración de IIS:

* Se establecen los tipos MIME de las siguientes extensiones de archivo:
  * *.dll* &ndash; `application/octet-stream`
  * *.json* &ndash; `application/json`
  * *.wasm* &ndash; `application/wasm`
  * *.woff* &ndash; `application/font-woff`
  * *.woff2* &ndash; `application/font-woff`
* Se habilita la compresión HTTP de los siguientes tipos MIME:
  * `application/octet-stream`
  * `application/wasm`
* Se establecen reglas del módulo URL Rewrite:
  * Proporcionar el subdirectorio donde residen los recursos estáticos de la aplicación ( *{ASSEMBLY NAME}/dist/{PATH REQUESTED}* ).
  * Crear el enrutamiento de reserva de SPA para que las solicitudes de recursos que no sean archivos se redirijan al documento predeterminado de la aplicación en su carpeta de recursos estáticos ( *{ASSEMBLY NAME}/dist/index.html*).

#### <a name="install-the-url-rewrite-module"></a>Instalación del módulo URL Rewrite

El [módulo URL Rewrite](https://www.iis.net/downloads/microsoft/url-rewrite) es necesario para reescribir las URL. El módulo no se instala de forma predeterminada y no está disponible para instalar como una característica de servicio de rol del servidor web (IIS). El módulo se debe descargar desde el sitio web de IIS. Use el instalador de plataforma web para instalar el módulo:

1. De forma local, vaya a la [página de descargas del módulo URL Rewrite](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads). En el caso de la versión en inglés, seleccione **WebPI** para descargar el instalador de WebPI. En el caso de otros idiomas, seleccione la arquitectura adecuada del servidor (x86/x64) para descargar el instalador.
1. Copie el instalador en el servidor. Ejecute el instalador. Haga clic en el botón **Instalar** y acepte los términos de licencia. No es necesario reiniciar el servidor al finalizar la instalación.

#### <a name="configure-the-website"></a>Configuración del sitio web

Configure la **ruta de acceso física** del sitio web a la carpeta de la aplicación. La carpeta contiene:

* El archivo *web.config* que usa IIS para configurar el sitio web, incluidas las reglas de redireccionamiento y los tipos de contenido de archivos necesarios.
* La carpeta de recursos estáticos de la aplicación.

#### <a name="troubleshooting"></a>Solución de problemas

Si se recibe un error *500 Error interno del servidor* y el administrador de IIS produce errores al intentar acceder a la configuración del sitio web, confirme que el módulo URL Rewrite está instalado. Si no lo está, IIS no puede analizar el archivo *web.config*. Esto impide que el Administrador de IIS cargue la configuración del sitio web y que el sitio web proporcione los archivos estáticos de Blazor.

Para obtener más información sobre cómo solucionar problemas de las implementaciones en IIS, vea <xref:host-and-deploy/iis/troubleshoot>.

### <a name="azure-storage"></a>Almacenamiento de Azure

El hospedaje de archivos estáticos de Azure Storage permite el hospedaje de aplicaciones Blazor sin servidor. Se admiten nombres de dominio personalizados, Azure Content Delivery Network (CDN) y HTTPS.

Cuando el servicio de blob está habilitado para el hospedaje de sitios web estáticos en una cuenta de almacenamiento:

* Establece el **nombre de documento de índice** en `index.html`.
* Establece la **ruta de acceso del documento de error** en `index.html`. Los componentes Razor y otros puntos de conexión que no son de archivo no residen en las rutas de acceso físicas del contenido estático almacenado por el servicio de blob. Cuando se recibe una solicitud de uno de estos recursos que debe controlar el enrutador de Blazor, el error *404: no encontrado* generado por el servicio de blob enruta la solicitud a la **ruta de acceso del documento de error**. Se devuelve el blob *index.html* y el enrutador de Blazor carga y procesa la ruta de acceso.

Para más información, consulte [Hospedaje de sitios web estáticos en Azure Storage](/azure/storage/blobs/storage-blob-static-website).

### <a name="nginx"></a>Nginx

El siguiente archivo *nginx.conf* se ha simplificado para mostrar cómo configurar Nginx para enviar el archivo *index.html* siempre que no pueda encontrar un archivo correspondiente en el disco.

```
events { }
http {
    server {
        listen 80;

        location / {
            root /usr/share/nginx/html;
            try_files $uri $uri/ /index.html =404;
        }
    }
}
```

Para obtener más información sobre la configuración del servidor web de producción de Nginx, consulte [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/) (Creación de archivos de configuración de NGINX y NGINX Plus).

### <a name="nginx-in-docker"></a>Nginx en Docker

Para hospedar Blazor en Docker mediante Nginx, configure el Dockerfile para usar la imagen de Nginx basada en Alpine. Actualice el Dockerfile para copiar el archivo *nginx.config* en el contenedor.

Agregue una línea al Dockerfile, como se muestra en el ejemplo siguiente:

```Dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="github-pages"></a>GitHub Pages

Para controlar las reescrituras de URL, agregue un archivo *404.html* con un script que controle el redireccionamiento de la solicitud a la página *index.html*. Para consultar una implementación de ejemplo que ha proporcionado la comunidad, vea [Single Page Apps for GitHub Pages](http://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)) (Aplicaciones de página única para GitHub Pages [rafrex/spa-github-pages en GitHub]). Se puede ver un ejemplo del enfoque de la comunidad en [blazor-demo/blazor-demo.github.io en GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([sitio activo](https://blazor-demo.github.io/)).

Al usar un sitio de proyecto en lugar de un sitio de la organización, agregue o actualice la etiqueta `<base>` en *index.html*. Defina el valor del atributo `href` con el nombre del repositorio de GitHub con una barra diagonal final (por ejemplo, `my-repository/`).
