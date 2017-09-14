---
title: "Conceptos básicos de ASP.NET Core"
author: rick-anderson
description: "En este artículo se proporciona información general de los conceptos fundamentales que deben conocerse al compilar aplicaciones de ASP.NET Core."
keywords: "ASP.NET Core, conceptos básicos, información general"
ms.author: riande
manager: wpickett
ms.date: 08/18/2017
ms.topic: get-started-article
ms.assetid: a19b7836-63e4-44e8-8250-50d426dd1070
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/index
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5d8ca35b0e2e4b6e9b1ec745a3a7cf7c3983c461
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2017
---
# <a name="aspnet-core-fundamentals-overview"></a>Información general de los conceptos básicos de ASP.NET Core

Una aplicación de ASP.NET Core es una aplicación de consola que crea un servidor web en su método `Main`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

El método `Main` invoca a `WebHost.CreateDefaultBuilder`, que sigue el patrón de generador para crear un host de aplicación web. El generador tiene métodos que definen el servidor web (por ejemplo, `UseKestrel`) y la clase de inicio (`UseStartup`). En el ejemplo anterior, se asigna automáticamente un servidor web [Kestrel](xref:fundamentals/servers/kestrel). El host web de ASP.NET Core intentará ejecutarse en IIS, si está disponible. Otros servidores web, como [HTTP.sys](xref:fundamentals/servers/httpsys), se pueden usar al invocar el método de extensión adecuado. `UseStartup` se explica en la sección siguiente.

`IWebHostBuilder`, el tipo de valor devuelto de la invocación `WebHost.CreateDefaultBuilder`, proporciona muchos métodos opcionales. Algunos de estos métodos incluyen `UseHttpSys` para hospedar la aplicación en HTTP.sys y `UseContentRoot` para especificar el directorio de contenido raíz. Los métodos `Build` y `Run` crean el objeto `IWebHost` que hospedará la aplicación y empiezan a escuchar las solicitudes HTTP.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]

El método `Main` usa `WebHostBuilder`, que sigue el patrón de generador para crear un host de aplicación web. El generador tiene métodos que definen el servidor web (por ejemplo, `UseKestrel`) y la clase de inicio (`UseStartup`). En el ejemplo anterior, se usa el servidor web [Kestrel](xref:fundamentals/servers/kestrel). Si se invoca el método de extensión adecuado, se pueden usar otros servidores web, como [WebListener](xref:fundamentals/servers/weblistener). `UseStartup` se explica en la sección siguiente.

`WebHostBuilder` proporciona muchos métodos opcionales, incluidos `UseIISIntegration` para hospedar en IIS e IIS Express y `UseContentRoot` para especificar el directorio de contenido raíz. Los métodos `Build` y `Run` crean el objeto `IWebHost` que hospedará la aplicación y empiezan a escuchar las solicitudes HTTP.

---

## <a name="startup"></a>Inicio

El método `UseStartup` de `WebHostBuilder` especifica la clase `Startup` para la aplicación:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

La clase `Startup` es donde se define la canalización de control de la solicitud y donde se configuran los servicios necesarios para la aplicación. La clase `Startup` debe ser pública y contener los siguientes métodos:

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

* `ConfigureServices` define los [Servicios](#services) usados por la aplicación (por ejemplo, ASP.NET Core MVC, Entity Framework Core, Identity, etc.).

* `Configure` define el [software intermedio](xref:fundamentals/middleware) en la canalización de solicitudes.

Para obtener más información, vea [Application startup](xref:fundamentals/startup) (Inicio de la aplicación).

## <a name="services"></a>Servicios

Un servicio es un componente que está pensado para su uso común en una aplicación. Los servicios se ponen a disposición a través de la [inserción de dependencias](xref:fundamentals/dependency-injection) (DI). ASP.NET Core incluye un controlador de inversión nativa del control (IoC) que admite la [inserción de constructores](xref:mvc/controllers/dependency-injection#constructor-injection) de forma predeterminada. El contenedor nativo puede reemplazarse con el contenedor que quiera. Además de la ventaja de acoplamiento flexible, DI hace que los servicios estén disponibles en toda la aplicación. Por ejemplo, el [registro](xref:fundamentals/logging) está disponible en toda la aplicación.

Para obtener más información, consulte [Inserción de dependencias](xref:fundamentals/dependency-injection).

## <a name="middleware"></a>Software intermedio

En ASP.NET Core, se crea la canalización de solicitudes mediante [software intermedio](xref:fundamentals/middleware). El software intermedio de ASP.NET Core lleva a cabo la lógica asincrónica en `HttpContext` y después invoca al siguiente software intermedio de la secuencia o finaliza la solicitud directamente. Se agrega un componente de software intermedio denominado "XYZ" al invocar a un método de extensión `UseXYZ` en el método `Configure`.

ASP.NET Core incluye un amplio conjunto de software intermedio integrado:

* [Archivos estáticos](xref:fundamentals/static-files)

* [Enrutamiento](xref:fundamentals/routing)

* [Autenticación](xref:security/authentication/index)

Puede usar cualquier software intermedio basado en [OWIN](http://owin.org) con ASP.NET Core y puede escribir su propio software intermedio personalizado.

Para obtener más información, consulte [Middleware](xref:fundamentals/middleware) (Software intermedio) y [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin) [Interfaz web abierta para .NET (OWIN)].

## <a name="servers"></a>Servidores

El modelo de hospedaje de ASP.NET Core no escucha directamente las solicitudes; en su lugar, se basa en una implementación de servidor HTTP para reenviar la solicitud a la aplicación. La solicitud reenviada se empaqueta como un conjunto de objetos de característica al que se puede tener acceso a través de las interfaces. La aplicación crea este conjunto en un `HttpContext`. ASP.NET Core incluye un servidor web administrado multiplataforma, denominado [Kestrel](xref:fundamentals/servers/kestrel). Kestrel se suele ejecutar detrás de un servidor web de producción como [IIS](https://www.iis.net/) o [nginx](http://nginx.org).

Para obtener más información, consulte [Servers](xref:fundamentals/servers/index) (Servidores) y [Hosting](xref:fundamentals/hosting) (Hospedaje).

## <a name="content-root"></a>Raíz del contenido

La raíz del contenido es la ruta de acceso base a cualquier contenido que usa la aplicación, como vistas, [páginas de Razor](xref:mvc/razor-pages/index) y activos estáticos. De forma predeterminada, la raíz del contenido es la misma que la ruta de acceso base de la aplicación para el ejecutable que hospeda la aplicación. Con `WebHostBuilder` se especifica una ubicación alternativa para la raíz del contenido.

## <a name="web-root"></a>Raíz web

La raíz web de una aplicación es el directorio en el proyecto que contiene recursos públicos y estáticos, como archivos de imagen, CSS y JavaScript. De forma predeterminada, el software intermedio de archivos estáticos solo ofrecerá archivos desde el directorio raíz web y sus subdirectorios. Consulte [Trabajar con archivos estáticos](xref:fundamentals/static-files) para obtener más información. El valor predeterminado de la ruta de acceso de raíz web es */wwwroot*, pero puede especificar una ubicación diferente mediante `WebHostBuilder`.

## <a name="configuration"></a>Configuración

ASP.NET Core usa un nuevo modelo de configuración para administrar pares nombre-valor simples. El nuevo modelo de configuración no se basa en `System.Configuration` o *web.config*; en su lugar, extrae de un conjunto ordenado de proveedores de configuración. Los proveedores de configuración integrados admiten una variedad de formatos de archivo (XML, JSON, INI) y variables de entorno para habilitar la configuración basada en el entorno. También puede escribir sus propios proveedores de configuración personalizados.

Para obtener más información, vea [Configuración](xref:fundamentals/configuration).

## <a name="environments"></a>Entornos

Los entornos, como "Desarrollo" y "Producción", son un concepto de primera clase en ASP.NET Core y se pueden establecer mediante variables de entorno.

Para obtener más información, consulte [Trabajar con varios entornos](xref:fundamentals/environments).

## <a name="net-core-vs-net-framework-runtime"></a>Entorno de ejecución de .NET Core frente a .NET Framework

Una aplicación de ASP.NET Core puede tener como destino el entorno de ejecución de .NET Core o .NET Framework. Para obtener más información, consulte [Selección entre .NET Core y .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).

## <a name="additional-information"></a>Información adicional

También puede consultar los temas siguientes:

- [Control de errores](xref:fundamentals/error-handling)
- [Proveedores de archivos](xref:fundamentals/file-providers)
- [Globalización y localización](xref:fundamentals/localization)
- [Registro](xref:fundamentals/logging)
- [Administración del estado de la aplicación](xref:fundamentals/app-state)
