---
title: "Conceptos básicos de ASP.NET Core"
author: rick-anderson
description: "Descubra los conceptos básicos para crear aplicaciones ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: fundamentals/index
ms.openlocfilehash: 7f0e30b3ac7f9cc3a32bd96f45d83ba13505a475
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2018
---
# <a name="aspnet-core-fundamentals"></a>Conceptos básicos de ASP.NET Core

Una aplicación de ASP.NET Core es una aplicación de consola que crea un servidor web en su método `Main`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

El método `Main` invoca a `WebHost.CreateDefaultBuilder`, que sigue el patrón de generador para crear un host de aplicación web. El generador tiene métodos que definen el servidor web (por ejemplo, `UseKestrel`) y la clase de inicio (`UseStartup`). En el ejemplo anterior, se asigna automáticamente el servidor web [Kestrel](xref:fundamentals/servers/kestrel). El host web de ASP.NET Core intenta ejecutarse en IIS, si está disponible. Otros servidores web, como [HTTP.sys](xref:fundamentals/servers/httpsys), se pueden usar al invocar el método de extensión adecuado. `UseStartup` se explica en la sección siguiente.

`IWebHostBuilder`, el tipo de valor devuelto de la invocación `WebHost.CreateDefaultBuilder`, proporciona muchos métodos opcionales. Algunos de estos métodos incluyen `UseHttpSys` para hospedar la aplicación en HTTP.sys y `UseContentRoot` para especificar el directorio de contenido raíz. Los métodos `Build` y `Run` crean el objeto `IWebHost` que hospeda la aplicación y empieza a escuchar las solicitudes HTTP.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]

El método `Main` usa `WebHostBuilder`, que sigue el patrón de generador para crear un host de aplicación web. El generador tiene métodos que definen el servidor web (por ejemplo, `UseKestrel`) y la clase de inicio (`UseStartup`). En el ejemplo anterior, se usa el servidor web [Kestrel](xref:fundamentals/servers/kestrel). Si se invoca el método de extensión adecuado, se pueden usar otros servidores web, como [WebListener](xref:fundamentals/servers/weblistener). `UseStartup` se explica en la sección siguiente.

`WebHostBuilder` proporciona muchos métodos opcionales, incluido `UseIISIntegration` para hospedar en IIS e IIS Express, y `UseContentRoot` para especificar el directorio de contenido raíz. Los métodos `Build` y `Run` crean el objeto `IWebHost` que hospeda la aplicación y empieza a escuchar las solicitudes HTTP.

---

## <a name="startup"></a>Inicio

El método `UseStartup` de `WebHostBuilder` especifica la clase `Startup` para la aplicación:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

La clase `Startup` es donde se define la canalización de control de solicitudes y donde se configuran los servicios necesarios para la aplicación. La clase `Startup` debe ser pública y contener los siguientes métodos:

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method
    // to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method
    // to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

`ConfigureServices` define los [servicios](#dependency-injection-services) que usa la aplicación (por ejemplo, ASP.NET Core MVC, Entity Framework Core, Identity). `Configure` define el [software intermedio](xref:fundamentals/middleware/index) en la canalización de solicitudes.

Para obtener más información, vea [Application startup](xref:fundamentals/startup) (Inicio de la aplicación).

## <a name="content-root"></a>Raíz del contenido

La raíz del contenido es la ruta de acceso base a cualquier contenido que usa la aplicación, como vistas, [páginas de Razor](xref:mvc/razor-pages/index) y activos estáticos. De forma predeterminada, la raíz del contenido es la misma que la ruta de acceso base de la aplicación para el archivo ejecutable que hospeda la aplicación.

## <a name="web-root"></a>Raíz web

La raíz web de una aplicación es el directorio del proyecto que contiene recursos públicos y estáticos, como archivos de imagen, CSS y JavaScript.

## <a name="dependency-injection-services"></a>Inserción de dependencias (servicios)

Un servicio es un componente que está pensado para su uso común en una aplicación. Los servicios se ponen a disposición a través de la [inserción de dependencias (DI)](xref:fundamentals/dependency-injection). ASP.NET Core incluye un contenedor **d**e **i**nversión del **c**ontrol (IoC) nativo que admite la [inserción de constructores](xref:mvc/controllers/dependency-injection#constructor-injection) de forma predeterminada. Si quiere, puede reemplazar el contenedor nativo predeterminado. Además de la ventaja de acoplamiento flexible, DI hace que los servicios estén disponibles en toda la aplicación (por ejemplo, el [registro](xref:fundamentals/logging/index)).

Para obtener más información, consulte [Inserción de dependencias](xref:fundamentals/dependency-injection).

## <a name="middleware"></a>Software intermedio

En ASP.NET Core, se crea la canalización de solicitudes mediante [software intermedio](xref:fundamentals/middleware/index). El software intermedio de ASP.NET Core lleva a cabo la lógica asincrónica en `HttpContext` y después invoca al siguiente software intermedio de la secuencia o finaliza la solicitud directamente. Se agrega un componente de software intermedio denominado "XYZ" al invocar un método de extensión `UseXYZ` en el método `Configure`.

ASP.NET Core incluye un amplio conjunto de middleware integrado:

* [Archivos estáticos](xref:fundamentals/static-files)
* [Enrutamiento](xref:fundamentals/routing)
* [Autenticación](xref:security/authentication/index)
* [Middleware de compresión de respuestas](xref:performance/response-compression)
* [Middleware de reescritura de dirección URL](xref:fundamentals/url-rewriting)

El software intermedio basado en [OWIN](http://owin.org) está disponible para aplicaciones ASP.NET Core y puede escribir su propio software intermedio personalizado.

Para obtener más información, consulte [Middleware](xref:fundamentals/middleware/index) (Software intermedio) y [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin) [Interfaz web abierta para .NET (OWIN)].

## <a name="environments"></a>Entornos

Los entornos, como "Desarrollo" y "Producción", son un concepto de primera clase en ASP.NET Core y se pueden establecer mediante variables de entorno.

Para obtener más información, consulte [Trabajar con varios entornos](xref:fundamentals/environments).

## <a name="configuration"></a>Configuración

ASP.NET Core usa un modelo de configuración basado en pares de nombre-valor. El modelo de configuración no se basa en `System.Configuration` o *web.config*. La configuración obtiene valores de un conjunto ordenado de proveedores de configuración. Los proveedores de configuración integrados admiten una variedad de formatos de archivo (XML, JSON, INI) y variables de entorno para habilitar la configuración basada en el entorno. También puede escribir sus propios proveedores de configuración personalizados.

Para obtener más información, vea [Configuración](xref:fundamentals/configuration/index).

## <a name="logging"></a>Registro

ASP.NET Core es compatible con una API de registro que funciona con una variedad de proveedores de registro. Los proveedores integrados admiten el envío de registros a uno o varios destinos. Se pueden usar plataformas de registro de terceros.

[Registro](xref:fundamentals/logging/index)

## <a name="error-handling"></a>Control de errores

ASP.NET Core tiene características integradas para controlar los errores en las aplicaciones, incluida una página de excepciones de desarrollador, páginas de errores personalizados, páginas de códigos de estado estáticos y control de excepciones de inicio.

Para más información, vea [Control de excepciones](xref:fundamentals/error-handling).

## <a name="routing"></a>Enrutamiento

ASP.NET Core ofrece características para el enrutamiento de solicitudes de aplicación a los controladores de ruta.

Para más información, vea [Enrutamiento](xref:fundamentals/routing).

## <a name="file-providers"></a>Proveedores de archivos

ASP.NET Core abstrae el acceso al sistema de archivos mediante el uso de proveedores de archivos, lo que ofrece una interfaz común para trabajar con archivos entre plataformas.

Para más información, vea [Proveedores de archivos](xref:fundamentals/file-providers).

## <a name="static-files"></a>Archivos estáticos

El software intermedio de archivos estáticos trabaja con archivos estáticos (por ejemplo, HTML, CSS, imágenes y JavaScript).

Para más información, vea [Trabajar con archivos estáticos](xref:fundamentals/static-files).

## <a name="hosting"></a>Hospedaje

Las aplicaciones ASP.NET Core configuran e inician un *host*. Dicho host es responsable de la administración de inicio y duración de la aplicación.

Para más información, vea [Hospedaje](xref:fundamentals/hosting).

## <a name="session-and-application-state"></a>Estado de sesión y aplicación

El estado de sesión es una característica de ASP.NET Core que se puede usar para guardar y almacenar datos de usuario mientras el usuario explora la aplicación web.

Para más información, vea [Estado de sesión y aplicación](xref:fundamentals/app-state).

## <a name="servers"></a>Servidores

El modelo de hospedaje de ASP.NET Core no escucha directamente las solicitudes. El modelo de hospedaje se basa en una implementación de servidor HTTP para reenviar la solicitud a la aplicación. La solicitud reenviada se empaqueta como un conjunto de objetos de característica al que se puede tener acceso a través de interfaces. ASP.NET Core incluye un servidor web administrado multiplataforma, denominado [Kestrel](xref:fundamentals/servers/kestrel). Kestrel se suele ejecutar detrás de un servidor web de producción como [IIS](https://www.iis.net/) o [Nginx](http://nginx.org). Kestrel se puede ejecutar como un servidor perimetral.

Para más información, vea [Servidores](xref:fundamentals/servers/index) y los temas siguientes:

* [Kestrel](xref:fundamentals/servers/kestrel)
* [Módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [HTTP.sys](xref:fundamentals/servers/httpsys) (anteriormente denominado [WebListener](xref:fundamentals/servers/weblistener))

## <a name="globalization-and-localization"></a>Globalización y localización

El hecho de crear un sitio web multilingüe con ASP.NET Core permite que este llegue a un público más amplio. ASP.NET Core proporciona servicios y software intermedio para la localización en diferentes idiomas y referencias culturales.

Para más información, vea [Globalización y localización](xref:fundamentals/localization).

## <a name="request-features"></a>Características de la solicitud

Los detalles de implementación del servidor web relacionados con las solicitudes HTTP y las respuestas se definen en las interfaces. En las implementaciones del servidor web y el software intermedio se usan estas interfaces para crear y modificar la canalización de hospedaje de la aplicación.

Para más información, vea [Características de la solicitud](xref:fundamentals/request-features).

## <a name="open-web-interface-for-net-owin"></a>Interfaz web abierta para .NET (OWIN)

ASP.NET Core es compatible con la interfaz web abierta para .NET (OWIN). OWIN permite que las aplicaciones web se desacoplen de los servidores web.

Para más información, vea [Interfaz web abierta para .NET (OWIN)](xref:fundamentals/owin).

## <a name="websockets"></a>WebSockets

[WebSocket](https://wikipedia.org/wiki/WebSocket) es un protocolo que habilita canales de comunicación bidireccional persistentes a través de conexiones TCP. Se usa para aplicaciones de chat, tableros de cotizaciones, juegos y donde se necesite funcionalidad en tiempo real en una aplicación web. ASP.NET Core es compatible con características de socket web.

Para más información, vea [WebSockets](xref:fundamentals/websockets).

## <a name="microsoftaspnetcoreall-metapackage"></a>Metapaquete Microsoft.AspNetCore.All

El metapaquete [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) para ASP.NET Core incluye lo siguiente:

* Todos los paquetes admitidos por el equipo de ASP.NET Core.
* Todos los paquetes admitidos por Entity Framework Core. 
* Dependencias internas y de terceros usadas por ASP.NET Core y Entity Framework Core.

Para más información, vea [Metapaquete Microsoft.AspNetCore.All](xref:fundamentals/metapackage).

## <a name="net-core-vs-net-framework-runtime"></a>Entorno de ejecución de .NET Core frente a .NET Framework

Una aplicación de ASP.NET Core puede tener como destino el entorno de ejecución de .NET Core o .NET Framework.

Para más información, vea [Selección entre .NET Core y .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).

## <a name="choose-between-aspnet-core-and-aspnet"></a>Elección entre ASP.NET Core y ASP.NET

Para más información sobre cómo elegir entre ASP.NET Core y ASP.NET, vea [Elección entre ASP.NET Core y ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).
