---
title: "Inicio de la aplicación de ASP.NET Core"
author: ardalis
description: "Descubra cómo la clase de inicio de ASP.NET Core configura servicios y la canalización de solicitud de la aplicación."
ms.author: tdykstra
manager: wpickett
ms.custom: mvc
ms.date: 12/08/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/startup
ms.openlocfilehash: dd2eb3d3996bc0bf277c8d5e772c8568ef9f147e
ms.sourcegitcommit: f5a7f0198628f0d152257d90dba6c3a0747a355a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/19/2017
---
# <a name="application-startup-in-aspnet-core"></a>Inicio de la aplicación de ASP.NET Core

Por [Steve Smith](https://ardalis.com), [Tom Dykstra](https://github.com/tdykstra), y [Luke Latham](https://github.com/guardrex)

La `Startup` clase configura servicios y la canalización de solicitud de la aplicación.

## <a name="the-startup-class"></a>La clase de inicio

Uso de aplicaciones de ASP.NET Core un `Startup` (clase), que se denomina `Startup` por convención. La `Startup` clase:

* Puede incluir opcionalmente un [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) método para configurar los servicios de la aplicación.
* Debe incluir un [configurar](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) método para crear la canalización de procesamiento de solicitudes de la aplicación.

`ConfigureServices`y `Configure` el tiempo de ejecución llama al iniciarse la aplicación:

[!code-csharp[Main](startup/snapshot_sample/Startup1.cs)]

Especifique el `Startup` clase con la [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt; ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) método:

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

El `Startup` constructor de clase acepta dependencias definidas por el host. Un uso común de [inyección de dependencia](xref:fundamentals/dependency-injection) en el `Startup` clase consiste en Insertar [IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) para configurar servicios de entorno:

[!code-csharp[Main](startup/snapshot_sample/Startup2.cs)]

Alternativa al insertar `IHostingStartup` consiste en utilizar un enfoque basado en convenciones. La aplicación puede definir independiente `Startup` clases para los entornos diferentes (por ejemplo, `StartupDevelopment`), y la clase de inicio correspondiente se selecciona en tiempo de ejecución. Se establece una prioridad de la clase cuyo sufijo de nombre coincide con el entorno actual. Si la aplicación se ejecuta en el entorno de desarrollo e incluye tanto una `Startup` clase y un `StartupDevelopment` (clase), el `StartupDevelopment` se utiliza la clase. Para obtener más información, consulte [trabajar con varios entornos](xref:fundamentals/environments#startup-conventions).

Para obtener más información acerca de `WebHostBuilder`, consulte el [hospedaje](xref:fundamentals/hosting) tema. Para obtener información sobre cómo controlar errores durante el inicio, consulte [control de excepciones de inicio](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>El método ConfigureServices

El [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) método es:

* Opcional.
* Lo llama el host de web antes de la `Configure` método para configurar los servicios de la aplicación.
* Donde [opciones de configuración](xref:fundamentals/configuration/index) se establecen por convención.

Agregar servicios al contenedor de servicios pone a disposición de la aplicación y de la `Configure` método. Los servicios se resuelven a través de [inyección de dependencia](xref:fundamentals/dependency-injection) o de [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices).

Puede configurar el host de web algunos servicios antes `Startup` se llaman a métodos. Los detalles están disponibles en la [hospedaje](xref:fundamentals/hosting) tema. 

Para las características que requieren el programa de instalación sustancial, hay `Add[Service]` métodos de extensión en [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection). Una aplicación web típica registra los servicios de Entity Framework, identidad y MVC:

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

## <a name="services-available-in-startup"></a>Servicios disponibles en el inicio

El host de web proporciona algunos servicios que están disponibles para el `Startup` constructor de clase. La aplicación agrega servicios adicionales a través de `ConfigureServices`. A continuación, están disponibles en el host y la aplicación servicios `Configure` y a lo largo de la aplicación.

## <a name="the-configure-method"></a>El método Configure

El [configurar](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) método se utiliza para especificar la forma en que la aplicación responde a las solicitudes HTTP. La canalización de solicitudes está configurada mediante la adición de [middleware](xref:fundamentals/middleware) componentes a un [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) instancia. `IApplicationBuilder`está disponible para el `Configure` método, pero no está registrado en el contenedor de servicios. Hospedaje crea un `IApplicationBuilder` y lo pasa directamente a `Configure` ([origen de referencia](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192)).

El [plantillas ASP.NET Core](/dotnet/core/tools/dotnet-new) configurar la canalización con compatibilidad para una página de excepción para desarrolladores, [BrowserLink](http://vswebessentials.com/features/browserlink), ASP.NET MVC, archivos estáticos y las páginas de error:

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

Cada `Use` método de extensión agrega un componente de middleware a la canalización de solicitudes. Por ejemplo, el `UseMvc` método de extensión agrega la [middleware enrutamiento](xref:fundamentals/routing) a la canalización de solicitud y configura [MVC](xref:mvc/overview) como controlador predeterminado.

Servicios adicionales, como `IHostingEnvironment` y `ILoggerFactory`, también se pueden especificar en la firma del método. Cuando se especifica, se insertan servicios adicionales si están disponibles.

Para obtener más información sobre cómo usar `IApplicationBuilder`, consulte [Middleware](xref:fundamentals/middleware).

## <a name="convenience-methods"></a>Métodos útiles

[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) y [configurar](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) métodos útiles que pueden usarse en lugar de especificar un `Startup` clase. Varias llamadas a `ConfigureServices` anexar entre sí. Varias llamadas a `Configure` usar la última llamada de método.

[!code-csharp[Main](startup/snapshot_sample/Program.cs?highlight=16,20)]

## <a name="startup-filters"></a>Filtros de inicio

Use [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) para configurar el middleware al principio o al final de una aplicación [configurar](#the-configure-method) canalización de middleware. `IStartupFilter`es útil para garantizar que se ejecuta un middleware antes o después de agregar bibliotecas al principio o al final de la canalización de procesamiento de solicitudes de la aplicación de middleware.

`IStartupFilter`implementa un método único, [configurar](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure), que recibe y devuelve un `Action<IApplicationBuilder>`. Un [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) define una clase para configurar la canalización de solicitud de una aplicación. Para obtener más información, consulte [crear una canalización de middleware con IApplicationBuilder](xref:fundamentals/middleware#creating-a-middleware-pipeline-with-iapplicationbuilder).

Cada `IStartupFilter` implementa middlewares uno o más en la canalización de solicitudes. Los filtros se invocan en el orden en que se agregaron al contenedor de servicios. Los filtros pueden producir middleware antes o después de pasar el control al siguiente filtro, por lo tanto anexa al principio o al final de la canalización de aplicación.

El [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample)) se muestra cómo registrar un middleware con `IStartupFilter`. La aplicación de ejemplo incluye un middleware que establece un valor de opciones de un parámetro de cadena de consulta:

[!code-csharp[Main](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

El `RequestSetOptionsMiddleware` está configurado en el `RequestSetOptionsStartupFilter` clase:

[!code-csharp[Main](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

El `IStartupFilter` está registrado en el contenedor de servicios en `ConfigureServices`:

[!code-csharp[Main](startup/sample/Startup.cs?name=snippet1&highlight=3)]

Cuando un parámetro de cadena de consulta para `option` es siempre el middleware procesa el valor asignado antes de que el middleware MVC representa la respuesta:

![Ventana del explorador que muestra la página de índice representada. El valor de opción se representa como 'De Middleware' basadas en solicitar la página con el parámetro de cadena de consulta y el valor de la opción establecida en 'De Middleware'.](startup/_static/index.png)

Orden de ejecución de middleware se establece por el orden de `IStartupFilter` registros:

* Varias `IStartupFilter` las implementaciones pueden interactuar con los mismos objetos. Si el orden es importante, ordenar sus `IStartupFilter` registros para que coincida con el orden en que se debe ejecutar sus middlewares de servicio.
* Las bibliotecas pueden agregar middleware con uno o varios `IStartupFilter` implementaciones que se ejecuten antes o después de otro middleware de aplicación registrado con `IStartupFilter`. Para invocar un `IStartupFilter` middleware antes un middleware agregado una biblioteca `IStartupFilter`, colocar el registro del servicio antes de que la biblioteca se agrega al contenedor de servicios. Para invocar a continuación, colocar el registro del servicio después de agrega la biblioteca.

## <a name="additional-resources"></a>Recursos adicionales

* [Hospedar aplicaciones de WPF](xref:fundamentals/hosting)
* [Trabajar con varios entornos](xref:fundamentals/environments)
* [Middleware](xref:fundamentals/middleware)
* [Registro](xref:fundamentals/logging/index)
* [Configuración](xref:fundamentals/configuration/index)
* [Clase StartupLoader: método FindStartupType (origen de referencia)](https://github.com/aspnet/Hosting/blob/rel/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs#L66-L116)
