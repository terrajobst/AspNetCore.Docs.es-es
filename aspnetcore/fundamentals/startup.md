---
title: Inicio de la aplicación en ASP.NET Core
author: ardalis
description: Explica cómo la clase Startup de ASP.NET Core configura los servicios y la canalización de solicitudes de la aplicación.
ms.author: tdykstra
ms.custom: mvc
ms.date: 4/13/2018
uid: fundamentals/startup
ms.openlocfilehash: 392dc83666bc6b9012adc6c32169ae7bdc7ed8d7
ms.sourcegitcommit: f43f430a166a7ec137fcad12ded0372747227498
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49391120"
---
# <a name="application-startup-in-aspnet-core"></a>Inicio de la aplicación en ASP.NET Core

Por [Steve Smith](https://ardalis.com), [Tom Dykstra](https://github.com/tdykstra) y [Luke Latham](https://github.com/guardrex)

La clase `Startup` configura los servicios y la canalización de solicitudes de la aplicación.

## <a name="the-startup-class"></a>Clase Startup

Las aplicaciones de ASP.NET Core utilizan una clase `Startup`, que se denomina `Startup` por convención. La clase `Startup`:

* Puede incluir opcionalmente un método [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) para configurar los servicios de la aplicación.
* Debe incluir un método [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) para crear la canalización de procesamiento de solicitudes de la aplicación.

El runtime llama a `ConfigureServices` y `Configure` al iniciarse la aplicación:

[!code-csharp[](startup/snapshot_sample/Startup1.cs)]

Especifique la clase `Startup` con el método [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_):

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

El host de web proporciona algunos servicios que están disponibles para el constructor de clase `Startup`. La aplicación agrega servicios adicionales a través de `ConfigureServices`. Después, los servicios de la aplicación y el host están disponibles en `Configure` y en toda la aplicación.

Un uso común de la [inserción de dependencias](xref:fundamentals/dependency-injection) en la clase `Startup` consiste en insertar:

* [IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) para configurar servicios según el entorno;
* [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) para leer la configuración.
* [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) para crear un registrador en `Startup.ConfigureServices`.

[!code-csharp[](startup/snapshot_sample/Startup2.cs)]

Una alternativa a la inserción de `IHostingEnvironment` consiste en utilizar un enfoque basado en convenciones. La aplicación puede definir clases `Startup` independientes para distintos entornos (por ejemplo, `StartupDevelopment`) y la clase `Startup` correspondiente se selecciona en tiempo de ejecución. La clase cuyo sufijo de nombre coincide con el entorno actual se establece como prioritaria. Si la aplicación se ejecuta en el entorno de desarrollo e incluye tanto la clase `Startup` como la clase `StartupDevelopment`, se utiliza la clase `StartupDevelopment`. Para obtener más información, consulte [Uso de varios entornos](xref:fundamentals/environments#environment-based-startup-class-and-methods).

Para obtener más información sobre `WebHostBuilder`, consulte el tema [Hospedaje](xref:fundamentals/host/index). Para obtener información sobre cómo controlar los errores que se producen durante el inicio, consulte [Control de excepciones de inicio](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>Método ConfigureServices

Características del método [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices):

* Optional
* Lo llama el host de web antes del método `Configure` para configurar los servicios de la aplicación.
* Es donde se establecen por convención las [opciones de configuración](xref:fundamentals/configuration/index).

El patrón habitual consiste en llamar a todos los métodos `Add{Service}` y, luego, a todos los métodos `services.Configure{Service}`. Por ejemplo, consulte [Configurar servicios de identidad](xref:security/authentication/identity#pw).

La adición de servicios al contenedor de servicios hace que estén disponibles en la aplicación y en el método `Configure`. Los servicios se resuelven mediante [inserción de dependencias](xref:fundamentals/dependency-injection) o desde [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices).

El host de web puede configurar algunos servicios antes de que se llame a los métodos `Startup`. Los detalles están disponibles en el tema [Hospedaje en ASP.NET Core](xref:fundamentals/host/index).

Para las características que requieren una configuración sustancial, hay métodos de extensión `Add[Service]` en [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection). Una aplicación ASP.NET Core típica registra los servicios de Entity Framework, Identity y MVC:

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

## <a name="the-configure-method"></a>El método Configure

El método [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) se utiliza para especificar la forma en que la aplicación responde a las solicitudes HTTP. La canalización de solicitudes se configura mediante la adición de componentes de [middleware](xref:fundamentals/middleware/index) a una instancia de [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder). `IApplicationBuilder` está disponible para el método `Configure`, pero no está registrado en el contenedor de servicios. El hospedaje crea un elemento `IApplicationBuilder` y lo pasa directamente a `Configure`.

Las [plantillas ASP.NET Core](/dotnet/core/tools/dotnet-new) configuran la canalización con compatibilidad para una página de excepción para desarrolladores, [BrowserLink](http://vswebessentials.com/features/browserlink), páginas de error, archivos estáticos y ASP.NET Core MVC:

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

Cada método de extensión `Use` agrega un componente de middleware a la canalización de solicitudes. Por ejemplo, el método de extensión `UseMvc` agrega el [middleware de enrutamiento](xref:fundamentals/routing) a la canalización de solicitudes y establece [MVC](xref:mvc/overview) como controlador predeterminado.

Cada componente de middleware de la canalización de solicitudes es responsable de invocar al siguiente componente de la canalización o de cortocircuitar la cadena en caso de ser necesario. Si el cortocircuito no se produce a lo largo de la cadena de middleware, cada middleware tiene una segunda oportunidad de procesar la solicitud antes de que se envíe al cliente.

Servicios adicionales, como `IHostingEnvironment` y `ILoggerFactory`, también se pueden especificar en la firma del método. Cuando se especifican, esos servicios adicionales se insertan si están disponibles.

Para más información sobre cómo usar `IApplicationBuilder` y el orden de procesamiento de middleware, vea [Middleware](xref:fundamentals/middleware/index).

## <a name="convenience-methods"></a>Métodos de conveniencia

Los métodos de conveniencia [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) y [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) pueden usarse en lugar de especificar una clase `Startup`. Varias llamadas a `ConfigureServices` se anexan entre sí. Varias llamadas a `Configure` usan la última llamada al método.

[!code-csharp[](startup/snapshot_sample/Program.cs?highlight=18,22)]

## <a name="extend-startup-with-startup-filters"></a>Extensión del inicio con filtros de inicio

Use [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) para configurar el middleware al principio o al final de la canalización de middleware [Configure](#the-configure-method) de una aplicación. `IStartupFilter` es útil para garantizar que un middleware se ejecuta antes o después del middleware agregado por bibliotecas al principio o al final de la canalización de procesamiento de solicitudes de la aplicación.

`IStartupFilter` implementa un método único, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure), que recibe y devuelve `Action<IApplicationBuilder>`. [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) define una clase para configurar la canalización de solicitudes de una aplicación. Para más información, vea [Creación de una canalización de middleware con IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).

Cada `IStartupFilter` implementa uno o más middleware en la canalización de solicitudes. Los filtros se invocan en el orden en que se agregaron al contenedor de servicios. Los filtros pueden agregar middleware antes o después de pasar el control al siguiente filtro, por lo que se anexan al principio o al final de la canalización de la aplicación.

La [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample)) muestra cómo se registra un middleware con `IStartupFilter`. La aplicación de ejemplo incluye un middleware que establece un valor de opciones de un parámetro de cadena de consulta:

[!code-csharp[](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

`RequestSetOptionsMiddleware` está configurado en las clase `RequestSetOptionsStartupFilter`:

[!code-csharp[](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

`IStartupFilter` está registrado en el contenedor de servicios en [IWebHostBuilder.ConfigureServices](xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.ConfigureServices*) para demostrar cómo el filtro de inicio aumenta `Startup` desde fuera de la clase `Startup`:

[!code-csharp[](startup/sample/Program.cs?name=snippet1&highlight=4-5)]

Cuando se proporciona un parámetro de cadena de consulta para `option`, el middleware procesa el valor asignado antes de que el middleware MVC represente la respuesta:

![Ventana del explorador que muestra la página de índice representada. El valor de Option se representa como "From Middleware" basándose en una solicitud a la página con el parámetro de cadena de consulta y el valor de la opción establecido en "From Middleware".](startup/_static/index.png)

El orden de ejecución de middleware se establece según el orden de registros de `IStartupFilter`:

* Varias implementaciones de `IStartupFilter` pueden interactuar con los mismos objetos. Si el orden es importante, ordene los registros de servicio de `IStartupFilter` para que coincidan con el orden en que se deben ejecutar los middleware.
* Las bibliotecas pueden agregar middleware con una o varias implementaciones de `IStartupFilter` que se ejecuten antes o después de otro middleware de aplicación registrado con `IStartupFilter`. Para invocar un middleware `IStartupFilter` antes que un middleware agregado por el `IStartupFilter` de una biblioteca, coloque el registro del servicio antes de que la biblioteca se agregue al contenedor de servicios. Para invocarlo posteriormente, coloque el registro del servicio después de que se agregue la biblioteca.

## <a name="add-configuration-at-startup-from-an-external-assembly"></a>Agregar opciones de configuración en el inicio desde un ensamblado externo

Una implementación de [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) permite agregar mejoras a una aplicación al iniciarla a partir de un ensamblado externo fuera de la clase `Startup` de esta. Para obtener más información, consulte [Mejora de una aplicación a partir de un ensamblado externo](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/host/index>
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>
