---
title: Conceptos básicos de ASP.NET Core
author: rick-anderson
description: Obtenga información sobre los conceptos básicos para crear aplicaciones de ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 03/02/2019
uid: fundamentals/index
---
# <a name="aspnet-core-fundamentals"></a>Conceptos básicos de ASP.NET Core

Este artículo es una introducción de los temas clave para entender cómo desarrollar aplicaciones de ASP.NET Core.

## <a name="the-startup-class"></a>Clase Startup

La clase `Startup` es donde:

* Se configuran los servicios requeridos por la aplicación.
* Se define la solicitud de canalización.

* Se agrega al método `Startup.ConfigureServices` el código para configurar (o *registrar*) servicios. Los *servicios* son componentes que usan la aplicación. Por ejemplo, un objeto de contexto de Entity Framework Core es un servicio.
* Se agrega al método `Startup.Configure` el código para configurar la canalización de control de solicitudes. La canalización se compone de una serie de componentes de *software intermedio*. Por ejemplo, un software intermedio podría controlar las solicitudes de archivos estáticos o redirigir las solicitudes HTTP a HTTPS. Cada software intermedio lleva a cabo las operaciones asincrónicas en un contexto `HttpContext` y, después, invoca el siguiente software intermedio de la canalización o finaliza la solicitud.

::: moniker range=">= aspnetcore-2.0"

Aquí tiene una clase `Startup` de ejemplo:

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

::: moniker-end

Para obtener más información, vea [Inicio de la aplicación](xref:fundamentals/startup).

## <a name="dependency-injection-services"></a>Inserción de dependencias (servicios)

ASP.NET Core tiene un marco de inserción de dependencias (DI) integrado que pone a disposición los servicios configurados para las clases de una aplicación. Una manera de obtener una instancia de un servicio en una clase es crear un constructor con un parámetro del tipo necesario. El parámetro puede ser el tipo de servicio o una interfaz. El sistema de DI proporciona el servicio en tiempo de ejecución.

::: moniker range=">= aspnetcore-2.0"

Aquí tiene una clase que usa DI para obtener un objeto de contexto de Entity Framework Core. La línea resaltada es un ejemplo de inserción de constructor:

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

::: moniker-end

Aunque DI está integrada, está diseñada para permitirle conectar un contenedor de inversión de control (IoC) de terceros si lo prefiere.

Para obtener más información, consulte [Inserción de dependencias](xref:fundamentals/dependency-injection).

## <a name="middleware"></a>Software intermedio

La canalización de control de solicitudes se compone de una serie de componentes de software intermedio. Cada componente lleva a cabo las operaciones asincrónicas en un contexto `HttpContext` y, después, invoca el siguiente software intermedio de la canalización o finaliza la solicitud.

Normalmente, se agrega un componente de software intermedio a la canalización al invocar su método de extensión `Use...` en el método `Startup.Configure`. Por ejemplo, para habilitar la representación de los archivos estáticos, llame a `UseStaticFiles`.

::: moniker range=">= aspnetcore-2.0"

El código resaltado en el ejemplo siguiente configura la canalización de control de solicitudes:

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

::: moniker-end

ASP.NET Core incluye una gran variedad de software intermedio integrado. Además, puede escribir software intermedio personalizado.

Para obtener más información vea [Software intermedio](xref:fundamentals/middleware/index).

<a id="host"/>

## <a name="the-host"></a>El host

Una aplicación de ASP.NET Core compila un *host* durante el inicio. El host es un objeto que encapsula todos los recursos de la aplicación, como:

* Una implementación de servidor de HTTP
* Componentes de software intermedio
* Registro
* DI
* Configuración

La razón principal para incluir todos los recursos interdependientes de la aplicación en un objeto es la administración de la duración: el control sobre el inicio de la aplicación y el apagado estable.

El código para crear un host se encuentra en `Program.Main` y sigue el [patrón de generador](https://wikipedia.org/wiki/Builder_pattern). Se llama a los métodos para configurar cada recurso que forma parte del host. Se llama a un método de generador para agrupar y crear una instancia del objeto host.

::: moniker range="<= aspnetcore-2.2"

ASP.NET Core 2.x usa el host web (la clase `WebHost`) para las aplicaciones web. El marco proporciona `CreateDefaultBuilder` para configurar un host con opciones de uso común, como las siguientes:

* Use [Kestrel](#servers) como servidor web y habilite la integración de IIS.
* Cargue la configuración de *appsettings.json*, las variables de entorno, los argumentos de línea de comandos y otros orígenes.
* Envíe la salida de registro a la consola y los proveedores de depuración.

::: moniker-end

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

Aquí tiene un código de ejemplo que crea un host:

[!code-csharp[](index/snapshots/2.x/Program1.cs?highlight=9)]

Para obtener más información, vea [Host web](xref:fundamentals/host/web-host).

::: moniker-end

::: moniker range="> aspnetcore-2.2"

En ASP.NET Core 3.0, en una aplicación web se puede usar el host web (clase `WebHost`) o el host genérico (clase `Host`). Se recomienda el host genérico, mientras que el host web está disponible para compatibilidad con versiones anteriores.

El marco proporciona los métodos `CreateDefaultBuilder` y `ConfigureWebHostDefaults` para configurar un host con opciones de uso común, como las siguientes:

* Use [Kestrel](#servers) como servidor web y habilite la integración de IIS.
* Cargue la configuración de *appsettings.json*, *appsettings.[EnvironmentName].json*, las variables de entorno y los argumentos de línea de comandos.
* Envíe la salida de registro a la consola y los proveedores de depuración.

Aquí tiene un código de ejemplo que crea un host. Se resaltan los métodos que configuran el host con las opciones usadas habitualmente.

[!code-csharp[](index/snapshots/3.x/Program1.cs?highlight=9-10)]

Para obtener más información, vea [Host genérico](xref:fundamentals/host/generic-host) y [Host web](xref:fundamentals/host/web-host).

::: moniker-end

### <a name="advanced-host-scenarios"></a>Escenarios de host avanzados

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

El host web está diseñado para incluir una implementación de servidor HTTP, que no es necesaria para otros tipos de aplicaciones. NET. A partir de la versión 2.1, el host genérico (clase `Host`) está disponible para que lo use cualquier aplicación de .NET Core, no solo las aplicaciones de ASP.NET Core. El host genérico permite usar funciones transversales como el registro, la inserción de dependencias, la configuración y la administración de la duración en otros tipos de aplicaciones. Para obtener más información, vea [Host genérico](xref:fundamentals/host/generic-host).

::: moniker-end

::: moniker range="> aspnetcore-2.2"

El host genérico está disponible para que lo use cualquier aplicación de .NET Core, no solo las aplicaciones de ASP.NET Core. El host genérico permite usar funciones transversales como el registro, la inserción de dependencias, la configuración y la administración de la duración en otros tipos de aplicaciones. Para obtener más información, vea [Host genérico](xref:fundamentals/host/generic-host).

::: moniker-end

También puede usar el host para ejecutar tareas en segundo plano. Para obtener más información, vea [Tareas en segundo plano](xref:fundamentals/host/hosted-services).

## <a name="servers"></a>Servidores

Una aplicación ASP.NET Core usa una implementación de servidor HTTP para escuchar las solicitudes HTTP. El servidor expone las solicitudes a la aplicación como un conjunto de [características de solicitud](xref:fundamentals/request-features) integradas en un contexto `HttpContext`.

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

ASP.NET Core proporciona las siguientes implementaciones de servidor:

* *Kestrel* es un servidor web multiplataforma. Kestrel se suele ejecutar en una configuración de proxy inverso con [IIS](https://www.iis.net/). En ASP.NET Core 2.0 y versiones posteriores, Kestrel puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet.
* Un *servidor HTTP de IIS* es un servidor para Windows que usa IIS. Con este servidor, la aplicación de ASP.NET Core e IIS se ejecutan en el mismo proceso.
* *HTTP.sys* es un servidor de Windows que no se usa con IIS.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ASP.NET Core proporciona la implementación de servidor multiplataforma *Kestrel*. En ASP.NET Core 2.0 y versiones posteriores, Kestrel puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet. Kestrel se suele ejecutar en una configuración de proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

ASP.NET Core proporciona la implementación de servidor multiplataforma *Kestrel*. En ASP.NET Core 2.0 y versiones posteriores, Kestrel puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet. Kestrel se suele ejecutar en una configuración de proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

ASP.NET Core proporciona las siguientes implementaciones de servidor:

* *Kestrel* es un servidor web multiplataforma. Kestrel se suele ejecutar en una configuración de proxy inverso con [IIS](https://www.iis.net/). En ASP.NET Core 2.0 y versiones posteriores, Kestrel puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet.
* *HTTP.sys* es un servidor de Windows que no se usa con IIS.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ASP.NET Core proporciona la implementación de servidor multiplataforma *Kestrel*. En ASP.NET Core 2.0 y versiones posteriores, Kestrel puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet. Kestrel se suele ejecutar en una configuración de proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

ASP.NET Core proporciona la implementación de servidor multiplataforma *Kestrel*. En ASP.NET Core 2.0 y versiones posteriores, Kestrel puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet. Kestrel se suele ejecutar en una configuración de proxy inverso con [Nginx](http://nginx.org) o [Apache](https://httpd.apache.org/).

---

::: moniker-end

Para obtener más información, vea [Servidores](xref:fundamentals/servers/index).

## <a name="configuration"></a>Configuración

ASP.NET Core proporciona un marco de configuración que obtiene la configuración como pares nombre/valor de un conjunto ordenado de proveedores de configuración. Hay proveedores de configuración integrados para una gran variedad de orígenes, como archivos *.json* y *.xml*, variables de entorno y argumentos de línea de comandos. También puede escribir proveedores de configuración personalizados.

Por ejemplo, puede especificar que la configuración procede de *appsettings.json* y las variables de entorno. Después, cuando se solicite el valor de *ConnectionString*, el marco buscará primero en el archivo *appsettings.json*. Si se encuentra el valor allí, pero también en una variable de entorno, el valor de la variable de entorno tendría prioridad.

Para administrar los datos de configuración confidencial como las contraseñas, ASP.NET Core proporciona una [herramienta de administrador secreto](xref:security/app-secrets). Para los secretos de producción, se recomienda [Azure Key Vault](/aspnet/core/security/key-vault-configuration).

Para obtener más información, vea [Configuración](xref:fundamentals/configuration/index).

## <a name="options"></a>Opciones

Siempre que sea posible, ASP.NET Core sigue el *patrón de opciones* para almacenar y recuperar los valores de configuración. El patrón de opciones usa clases para representar grupos de configuraciones relacionadas.

Por ejemplo, el código siguiente define las opciones de WebSockets:

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

Para obtener más información, vea [Opciones](xref:fundamentals/configuration/options).

## <a name="environments"></a>Entornos

Los entornos de ejecución, como *desarrollo*, *almacenamiento provisional* y *Producción*, son un concepto de primera clase en ASP.NET Core. Puede especificar el entorno que ejecuta una aplicación estableciendo la variable de entorno `ASPNETCORE_ENVIRONMENT`. ASP.NET Core lee dicha variable de entorno al inicio de la aplicación y almacena el valor en una implementación `IHostingEnvironment`. El objeto de entorno está disponible en cualquier parte de la aplicación a través de DI.

::: moniker range=">= aspnetcore-2.0"

El siguiente ejemplo de código desde la clase `Startup` configura la aplicación para que proporcione información detallada del error solo cuando se ejecuta en el desarrollo:

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

::: moniker-end

Para obtener más información, vea [Entornos](xref:fundamentals/environments).

## <a name="logging"></a>Registro

ASP.NET Core es compatible con una API de registro que funciona con una gran variedad de proveedores de registro integrados y de terceros. Entre los proveedores disponibles se incluyen los siguientes:

* Consola
* Depuración
* Seguimiento de eventos en Windows
* Registro de errores de Windows
* TraceSource
* Azure App Service
* Azure Application Insights

Escribir registros desde cualquier lugar en el código de una aplicación mediante la obtención de un objeto `ILogger` de DI y llamar a métodos de registro.

::: moniker range=">= aspnetcore-2.0"

Aquí tiene un código de ejemplo que usa un objeto `ILogger`, con la inserción del constructor y resaltadas las llamadas del método de registro.

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

::: moniker-end

La interfaz `ILogger` le permite pasar cualquier número de campos para el proveedor de registro. Los campos habitualmente se usan para construir una cadena de mensaje, pero el proveedor también puede enviarlos como campos independientes o almacén de datos. Esta característica permite a los proveedores de registro implementar el [registro semántico, también conocido como registro estructurado](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

Para obtener más información, vea [Registro](xref:fundamentals/logging/index).

## <a name="routing"></a>Enrutamiento

Una *ruta* es un patrón de dirección URL que se asigna a un controlador. El controlador normalmente es una página de Razor, un método de acción en un controlador MVC o un software intermedio. El enrutamiento de ASP.NET Core le permite controlar las direcciones URL usadas por la aplicación.

Para más información, vea [Enrutamiento](xref:fundamentals/routing).

## <a name="error-handling"></a>Control de errores

ASP.NET Core tiene características integradas para controlar los errores, tales como:

* Una página de excepciones para el desarrollador
* Páginas de errores personalizados
* Páginas de códigos de estado estáticos
* Control de excepciones de inicio

Para obtener más información, vea [Control de excepciones](xref:fundamentals/error-handling).

::: moniker range=">= aspnetcore-2.1"

## <a name="make-http-requests"></a>Realización de solicitudes HTTP

Una implementación de `IHttpClientFactory` está disponible para crear instancias de `HttpClient`. Este servicio:

* Proporciona una ubicación central para denominar y configurar instancias de `HttpClient` lógicas. Así, por ejemplo, se puede registrar y configurar un cliente *github* para tener acceso a GitHub. y, de igual modo, registrar otro cliente predeterminado para otros fines.
* Admite el registro y encadenamiento de varios controladores de delegación para crear una canalización de middleware de solicitud saliente. Este patrón es similar a la canalización de middleware de entrada de ASP.NET Core. Dicho patrón proporciona un mecanismo para administrar cuestiones transversales relativas a las solicitudes HTTP, como el almacenamiento en caché, el control de errores, la serialización y el registro.
* Se integra con *Polly*, una conocida biblioteca de terceros para el control de errores transitorios.
* Administra la agrupación y duración de las instancias de `HttpClientMessageHandler` subyacentes para evitar los problemas de DNS que suelen producirse al administrar las duraciones de `HttpClient` manualmente.
* Agrega una experiencia de registro configurable (a través de *ILogger*) en todas las solicitudes enviadas a través de los clientes creados por Factory.

Para obtener más información, vea [Realización de solicitudes HTTP](xref:fundamentals/http-requests).

::: moniker-end

## <a name="content-root"></a>Raíz del contenido

La raíz del contenido es la ruta de acceso base a cualquier contenido privado que usa la aplicación, como sus archivos de Razor. De forma predeterminada, la raíz del contenido es la ruta de acceso base para el archivo ejecutable que hospeda la aplicación. Se puede especificar una ubicación alternativa al [crear el host](#host).

::: moniker range="<= aspnetcore-2.2"

Para obtener más información, vea [Raíz del contenido](xref:fundamentals/host/web-host#content-root).

::: moniker-end

::: moniker range="> aspnetcore-2.2"

Para obtener más información, vea [Raíz del contenido](xref:fundamentals/host/generic-host#content-root).

::: moniker-end

## <a name="web-root"></a>Raíz web

La raíz web (también conocida como *webroot*) es la ruta de acceso base a los recursos públicos y estáticos, como archivos de imágenes, CSS y JavaScript. De forma predeterminada, el software intermedio de archivos estáticos solo ofrecerá archivos desde el directorio raíz web (y subdirectorios). El valor predeterminado de la ruta de acceso web es *\<raíz del contenido>/wwwroot*, pero se puede especificar una ubicación diferente al [crear el host](#host).

En los archivos de Razor (*.cshtml*), la virgulilla `~/` apunta a la raíz web. Las rutas de acceso que empiezan por `~/` se conocen como rutas de acceso virtuales.

Para obtener más información, consulte [Archivos estáticos](xref:fundamentals/static-files).
