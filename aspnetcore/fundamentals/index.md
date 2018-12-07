---
title: Conceptos básicos de ASP.NET Core
author: rick-anderson
description: Descubra los conceptos básicos para crear aplicaciones de ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/01/2018
uid: fundamentals/index
ms.openlocfilehash: 8bd447632f915cadcc5199ec50b292ad27f6c3ba
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861594"
---
# <a name="aspnet-core-fundamentals"></a>Conceptos básicos de ASP.NET Core

Una aplicación de ASP.NET Core es una aplicación de consola que crea un servidor web en su método `Program.Main`. El método `Main` es el *punto de entrada administrado* de la aplicación:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

Host de .NET Core:

* Carga el [entorno de ejecución de .NET Core](https://github.com/dotnet/coreclr).
* Usa el primer argumento de la línea de comandos como la ruta de acceso al binario administrado que contiene el punto de entrada (`Main`) e inicia la ejecución del código.

El método `Main` invoca [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), que sigue el [patrón del generador](https://wikipedia.org/wiki/Builder_pattern) para crear un host de web. El generador tiene métodos que definen el servidor web (por ejemplo, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) y la clase de inicio (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>). En el ejemplo anterior, se asigna automáticamente el servidor web [Kestrel](xref:fundamentals/servers/kestrel). El host web de ASP.NET Core intenta ejecutarse en [Internet Information Services (IIS)](https://www.iis.net/), si está disponible. Otros servidores web, como [HTTP.sys](xref:fundamentals/servers/httpsys), se pueden usar al invocar el método de extensión adecuado. `UseStartup` se explica con más detalle en la sección [Inicio](#startup).

<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, el tipo de valor devuelto de la invocación `WebHost.CreateDefaultBuilder`, proporciona muchos métodos opcionales. Algunos de estos métodos incluyen `UseHttpSys` para hospedar la aplicación en HTTP.sys y <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> para especificar el directorio de contenido raíz. Los métodos <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> y <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> crean el objeto <xref:Microsoft.AspNetCore.Hosting.IWebHost> que hospeda la aplicación y empieza a escuchar las solicitudes HTTP.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

Host de .NET Core:

* Carga el [entorno de ejecución de .NET Core](https://github.com/dotnet/coreclr).
* Usa el primer argumento de la línea de comandos como la ruta de acceso al binario administrado que contiene el punto de entrada (`Main`) e inicia la ejecución del código.

El método `Main` usa <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, que sigue el [patrón del generador](https://wikipedia.org/wiki/Builder_pattern) para crear un host de aplicación web. El generador tiene métodos que definen el servidor web (por ejemplo, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) y la clase de inicio (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>). En el ejemplo anterior, se usa el servidor web [Kestrel](xref:fundamentals/servers/kestrel). Si se invoca el método de extensión adecuado, se pueden usar otros servidores web, como [WebListener](xref:fundamentals/servers/weblistener). `UseStartup` se explica en la sección siguiente.

`WebHostBuilder` proporciona muchos métodos opcionales, incluido <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> para hospedar en IIS e IIS Express, y <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> para especificar el directorio de contenido raíz. Los métodos <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> y <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> crean el objeto <xref:Microsoft.AspNetCore.Hosting.IWebHost> que hospeda la aplicación y empieza a escuchar las solicitudes HTTP.

::: moniker-end

## <a name="startup"></a>Inicio

El método `UseStartup` de `WebHostBuilder` especifica la clase `Startup` para la aplicación:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

La clase `Startup` es donde se define la canalización de control de solicitudes y donde se configuran los servicios necesarios para la aplicación. La clase `Startup` debe ser pública y contener los siguientes métodos:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> define los [servicios](#dependency-injection-services) que usa la aplicación (por ejemplo, ASP.NET Core MVC, Entity Framework Core, Identity). <xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> define el [software intermedio](xref:fundamentals/middleware/index) al que se llama en la canalización de solicitudes.

Para obtener más información, vea <xref:fundamentals/startup>.

## <a name="content-root"></a>Raíz del contenido

La raíz del contenido es la ruta de acceso base a cualquier contenido que usa la aplicación, como [Razor Pages](xref:razor-pages/index), las vistas de MVC y los recursos estáticos. De forma predeterminada, la raíz del contenido es la misma ubicación que la ruta de acceso base de la aplicación para el archivo ejecutable que hospeda la aplicación.

## <a name="web-root-webroot"></a>Raíz web (webroot)

La raíz web de una aplicación es el directorio del proyecto que contiene recursos públicos y estáticos, como archivos de imagen, CSS y JavaScript. De forma predeterminada, la raíz web es *wwwroot*.

Para los archivos de Razor (*.cshtml*), la virgulilla `~/` apunta a la raíz web. Las rutas de acceso que empiezan por `~/` se conocen como rutas de acceso virtuales.

## <a name="dependency-injection-services"></a>Inserción de dependencias (servicios)

Un *servicio* es un componente que está pensado para su uso común en una aplicación. Los servicios se ponen a disposición a través de la [inserción de dependencias (DI)](xref:fundamentals/dependency-injection). ASP.NET Core incluye un contenedor de inversión del control (IoC) nativo que admite la [inserción de constructores](xref:mvc/controllers/dependency-injection#constructor-injection) de forma predeterminada. Si lo prefiere, puede reemplazar el contenedor predeterminado. Además de la [ventaja de acoplamiento flexible](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI hace que los servicios estén disponibles en toda la aplicación, por ejemplo, el [registro](xref:fundamentals/logging/index).

Para obtener más información, vea <xref:fundamentals/dependency-injection>.

## <a name="middleware"></a>Software intermedio

En ASP.NET Core, se crea la canalización de solicitudes mediante [software intermedio](xref:fundamentals/middleware/index). El software intermedio de ASP.NET Core lleva a cabo las operaciones asincrónicas en un `HttpContext` y, después, invoca el siguiente software intermedio de la canalización o finaliza la solicitud.

Normalmente, se agrega un componente de software intermedio denominado "XYZ" a la canalización al invocar a un método de extensión `UseXYZ` en el método `Configure`.

ASP.NET Core incluye una gran variedad de software intermedio integrado. Además, puede escribir su propio software intermedio personalizado. La [interfaz web abierta para .NET (OWIN)](xref:fundamentals/owin), que permite desacoplar las aplicaciones web de servidores web, es compatible con las aplicaciones de ASP.NET Core.

Para obtener más información, vea <xref:fundamentals/middleware/index> y <xref:fundamentals/owin>.

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a>Inicio de solicitudes HTTP

<xref:System.Net.Http.IHttpClientFactory> está disponible para acceder a instancias de <xref:System.Net.Http.HttpClient> con el fin de realizar solicitudes HTTP.

Para obtener más información, vea <xref:fundamentals/http-requests>.

::: moniker-end

## <a name="environments"></a>Entornos

Los entornos, como *Desarrollo* y *Producción*, son un concepto de primera clase en ASP.NET Core y se pueden establecer mediante una variable de entorno, un archivo de configuración o un argumento de línea de comandos.

Para obtener más información, vea <xref:fundamentals/environments>.

## <a name="hosting"></a>Hospedaje

Las aplicaciones ASP.NET Core configuran e inician un *host*. Dicho host es responsable de la administración de inicio y duración de la aplicación.

Para obtener más información, vea <xref:fundamentals/host/index>.

## <a name="servers"></a>Servidores

El modelo de hospedaje de ASP.NET Core no escucha directamente las solicitudes. El modelo de hospedaje se basa en una implementación de servidor HTTP para reenviar la solicitud a la aplicación.

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

ASP.NET Core proporciona las siguientes implementaciones de servidor:

* El servidor [Kestrel](xref:fundamentals/servers/kestrel) es un servidor web multiplataforma administrado. Kestrel se suele ejecutar en una configuración de proxy inverso con [IIS](https://www.iis.net/). Kestrel también puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet en ASP.NET Core 2.0 o versiones posteriores.
* El servidor HTTP de IIS (`IISHttpServer`) es un [servidor en proceso de IIS](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model).
* El servidor [HTTP.sys](xref:fundamentals/servers/httpsys) es un servidor web para ASP.NET Core en Windows.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ASP.NET Core usa la implementación del servidor [Kestrel](xref:fundamentals/servers/kestrel). Kestrel es un servidor web multiplataforma administrado. Kestrel también puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet en ASP.NET Core 2.0 o versiones posteriores.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

ASP.NET Core usa la implementación del servidor [Kestrel](xref:fundamentals/servers/kestrel). Kestrel es un servidor web multiplataforma administrado. Kestrel se suele ejecutar en una configuración de proxy inverso con [Nginx](http://nginx.org) o [Apache](https://httpd.apache.org/). Kestrel también puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet en ASP.NET Core 2.0 o versiones posteriores.

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

ASP.NET Core proporciona las siguientes implementaciones de servidor:

* El servidor [Kestrel](xref:fundamentals/servers/kestrel) es un servidor web multiplataforma administrado. Kestrel se suele ejecutar en una configuración de proxy inverso con [IIS](https://www.iis.net/). Kestrel también puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet en ASP.NET Core 2.0 o versiones posteriores.
* El servidor [HTTP.sys](xref:fundamentals/servers/httpsys) es un servidor web para ASP.NET Core en Windows.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ASP.NET Core usa la implementación del servidor [Kestrel](xref:fundamentals/servers/kestrel). Kestrel es un servidor web multiplataforma administrado. Kestrel también puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet en ASP.NET Core 2.0 o versiones posteriores.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

ASP.NET Core usa la implementación del servidor [Kestrel](xref:fundamentals/servers/kestrel). Kestrel es un servidor web multiplataforma administrado. Kestrel se suele ejecutar en una configuración de proxy inverso con [Nginx](http://nginx.org) o [Apache](https://httpd.apache.org/). Kestrel también puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet en ASP.NET Core 2.0 o versiones posteriores.

---

::: moniker-end

Para obtener más información, vea <xref:fundamentals/servers/index>.

## <a name="configuration"></a>Configuración

ASP.NET Core usa un modelo de configuración basado en pares de nombre-valor. El modelo de configuración no se basa en <xref:System.Configuration> o *web.config*. La configuración obtiene valores de un conjunto ordenado de proveedores de configuración. Los proveedores de configuración integrados admiten una gran variedad de formatos de archivo (XML, JSON, INI), variables de entorno y argumentos de línea de comandos. También puede escribir sus propios proveedores de configuración personalizados.

Para obtener más información, vea <xref:fundamentals/configuration/index>.

## <a name="logging"></a>Registro

ASP.NET Core es compatible con una API de registro que funciona con una gran variedad de proveedores de registro. Los proveedores integrados admiten el envío de registros a uno o varios destinos. Se pueden usar plataformas de registro de terceros.

Para obtener más información, vea <xref:fundamentals/logging/index>.

## <a name="error-handling"></a>Control de errores

ASP.NET Core tiene escenarios integrados para controlar los errores en las aplicaciones, incluidas una página de excepciones de desarrollador, páginas de errores personalizados, páginas de códigos de estado estáticos y control de excepciones de inicio.

Para obtener más información, vea <xref:fundamentals/error-handling>.

## <a name="routing"></a>Enrutamiento

ASP.NET Core ofrece casos para el enrutamiento de solicitudes de aplicación a los controladores de ruta.

Para obtener más información, vea <xref:fundamentals/routing>.

## <a name="background-tasks"></a>Tareas en segundo plano

Las tareas en segundo plano se implementan como *servicios hospedados*. Un servicio hospedado es una clase con lógica de tarea en segundo plano que implementa la interfaz <xref:Microsoft.Extensions.Hosting.IHostedService>.

Para obtener más información, vea <xref:fundamentals/host/hosted-services>.

## <a name="access-httpcontext"></a>Acceso a HttpContext

`HttpContext` está disponible automáticamente al procesar las solicitudes con Razor Pages y MVC. En los casos en los que `HttpContext` no esté disponible, podrá acceder a `HttpContext` a través de la interfaz de <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> y su implementación predeterminada, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.

Para obtener más información, vea <xref:fundamentals/httpcontext>.
