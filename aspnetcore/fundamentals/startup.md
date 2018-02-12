---
title: "Inicio de la aplicación en ASP.NET Core"
author: ardalis
description: "Descubra cómo la clase Startup de ASP.NET Core configura los servicios y la canalización de solicitudes de la aplicación."
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 12/08/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/startup
ms.openlocfilehash: c324918b33af82b619bb2251f32308e4a57c27e5
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2018
---
# <a name="application-startup-in-aspnet-core"></a>Inicio de la aplicación en ASP.NET Core

Por [Steve Smith](https://ardalis.com), [Tom Dykstra](https://github.com/tdykstra) y [Luke Latham](https://github.com/guardrex)

La clase `Startup` configura los servicios y la canalización de solicitudes de la aplicación.

## <a name="the-startup-class"></a>Clase Startup

Las aplicaciones de ASP.NET Core utilizan una clase `Startup`, que se denomina `Startup` por convención. La clase `Startup`:

* Puede incluir opcionalmente un método [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) para configurar los servicios de la aplicación.
* Debe incluir un método [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) para crear la canalización de procesamiento de solicitudes de la aplicación.

El runtime llama a `ConfigureServices` y `Configure` al iniciarse la aplicación:

[!code-csharp[Main](startup/snapshot_sample/Startup1.cs)]

Especifique la clase `Startup` con el método [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_):

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

El constructor de clase `Startup` acepta dependencias definidas por el host. Un uso común de la [inserción de dependencias](xref:fundamentals/dependency-injection) en la clase `Startup` consiste en insertar:

* [IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) para configurar servicios según el entorno;
* [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) para configurar la aplicación durante el inicio.

[!code-csharp[Main](startup/snapshot_sample/Startup2.cs)]

Una alternativa a la inserción de `IHostingEnvironment` consiste en utilizar un enfoque basado en convenciones. La aplicación puede definir clases `Startup` independientes para los distintos entornos (por ejemplo, `StartupDevelopment`), mientras que la clase de inicio correspondiente se selecciona en tiempo de ejecución. La clase cuyo sufijo de nombre coincide con el entorno actual se establece como prioritaria. Si la aplicación se ejecuta en el entorno de desarrollo e incluye tanto la clase `Startup` como la clase `StartupDevelopment`, se utiliza la clase `StartupDevelopment`. Para obtener más información, consulte [Working with multiple environments](xref:fundamentals/environments#startup-conventions) (Trabajo con varios entornos).

Para obtener más información sobre `WebHostBuilder`, consulte el tema [Hospedaje](xref:fundamentals/hosting). Para obtener información sobre cómo controlar los errores que se producen durante el inicio, consulte [Control de excepciones de inicio](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>Método ConfigureServices

Características del método [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices):

* Opcional.
* Lo llama el host de web antes del método `Configure` para configurar los servicios de la aplicación.
* Es donde se establecen por convención las [opciones de configuración](xref:fundamentals/configuration/index).

La adición de servicios al contenedor de servicios hace que estén disponibles en la aplicación y en el método `Configure`. Los servicios se resuelven mediante [inserción de dependencias](xref:fundamentals/dependency-injection) o desde [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices).

El host de web puede configurar algunos servicios antes de que se llame a los métodos `Startup`. En el tema [Hospedaje](xref:fundamentals/hosting) hay disponible más información. 

Para las características que requieren una configuración sustancial, hay métodos de extensión `Add[Service]` en [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection). Una aplicación web típica registra los servicios de Entity Framework, Identity y MVC:

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

## <a name="services-available-in-startup"></a>Servicios disponibles en Startup

El host de web proporciona algunos servicios que están disponibles para el constructor de clase `Startup`. La aplicación agrega servicios adicionales a través de `ConfigureServices`. Los servicios de la aplicación y el host están disponibles en `Configure` y en toda la aplicación.

## <a name="the-configure-method"></a>El método Configure

El método [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) se utiliza para especificar la forma en que la aplicación responde a las solicitudes HTTP. La canalización de solicitudes se configura mediante la adición de componentes de [middleware](xref:fundamentals/middleware/index) a una instancia de [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder). `IApplicationBuilder` está disponible para el método `Configure`, pero no está registrado en el contenedor de servicios. Hospedaje crea un `IApplicationBuilder` y lo pasa directamente a `Configure` ([origen de referencia](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192)).

Las [plantillas ASP.NET Core](/dotnet/core/tools/dotnet-new) configuran la canalización con compatibilidad para una página de excepción para desarrolladores, [BrowserLink](http://vswebessentials.com/features/browserlink), páginas de error, archivos estáticos y ASP.NET MVC:

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

Cada método de extensión `Use` agrega un componente de middleware a la canalización de solicitudes. Por ejemplo, el método de extensión `UseMvc` agrega el [middleware de enrutamiento](xref:fundamentals/routing) a la canalización de solicitudes y establece [MVC](xref:mvc/overview) como controlador predeterminado.

Servicios adicionales, como `IHostingEnvironment` y `ILoggerFactory`, también se pueden especificar en la firma del método. Cuando se especifican, esos servicios adicionales se insertan si están disponibles.

Para más información sobre cómo usar `IApplicationBuilder`, consulte [Middleware](xref:fundamentals/middleware/index).

## <a name="convenience-methods"></a>Métodos de conveniencia

Los métodos de conveniencia [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) y [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) pueden usarse en lugar de especificar una clase `Startup`. Varias llamadas a `ConfigureServices` se anexan entre sí. Varias llamadas a `Configure` usan la última llamada al método.

[!code-csharp[Main](startup/snapshot_sample/Program.cs?highlight=18,22)]

## <a name="startup-filters"></a>Filtros de Startup

Use [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) para configurar el middleware al principio o al final de la canalización de middleware [Configure](#the-configure-method) de una aplicación. `IStartupFilter` es útil para garantizar que un middleware se ejecuta antes o después del middleware agregado por bibliotecas al principio o al final de la canalización de procesamiento de solicitudes de la aplicación.

`IStartupFilter` implementa un método único, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure), que recibe y devuelve `Action<IApplicationBuilder>`. [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) define una clase para configurar la canalización de solicitudes de una aplicación. Para obtener más información, consulte [Creación de una canalización de middleware con IApplicationBuilder](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder).

Cada `IStartupFilter` implementa uno o más middleware en la canalización de solicitudes. Los filtros se invocan en el orden en que se agregaron al contenedor de servicios. Los filtros pueden agregar middleware antes o después de pasar el control al siguiente filtro, por lo que se anexan al principio o al final de la canalización de la aplicación.

La [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample)) muestra cómo se registra un middleware con `IStartupFilter`. La aplicación de ejemplo incluye un middleware que establece un valor de opciones de un parámetro de cadena de consulta:

[!code-csharp[Main](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

`RequestSetOptionsMiddleware` está configurado en las clase `RequestSetOptionsStartupFilter`:

[!code-csharp[Main](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

`IStartupFilter` está registrado en el contenedor de servicios en `ConfigureServices`:

[!code-csharp[Main](startup/sample/Startup.cs?name=snippet1&highlight=3)]

Cuando se proporciona un parámetro de cadena de consulta para `option`, el middleware procesa el valor asignado antes de que el middleware MVC represente la respuesta:

![Ventana del explorador que muestra la página de índice representada. El valor de Option se representa como "From Middleware" basándose en una solicitud a la página con el parámetro de cadena de consulta y el valor de la opción establecido en "From Middleware".](startup/_static/index.png)

El orden de ejecución de middleware se establece según el orden de registros de `IStartupFilter`:

* Varias implementaciones de `IStartupFilter` pueden interactuar con los mismos objetos. Si el orden es importante, ordene los registros de servicio de `IStartupFilter` para que coincidan con el orden en que se deben ejecutar los middleware.
* Las bibliotecas pueden agregar middleware con una o varias implementaciones de `IStartupFilter` que se ejecuten antes o después de otro middleware de aplicación registrado con `IStartupFilter`. Para invocar un middleware `IStartupFilter` antes que un middleware agregado por el `IStartupFilter` de una biblioteca, coloque el registro del servicio antes de que la biblioteca se agregue al contenedor de servicios. Para invocarlo posteriormente, coloque el registro del servicio después de que se agregue la biblioteca.

## <a name="additional-resources"></a>Recursos adicionales

* [Hospedar aplicaciones de WPF](xref:fundamentals/hosting)
* [Trabajar con varios entornos](xref:fundamentals/environments)
* [Middleware](xref:fundamentals/middleware/index)
* [Registro](xref:fundamentals/logging/index)
* [Configuración](xref:fundamentals/configuration/index)
* [Clase StartupLoader: método FindStartupType (origen de referencia)](https://github.com/aspnet/Hosting/blob/rel/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs#L66-L116)
