---
title: Inicio de la aplicación en ASP.NET Core
author: tdykstra
description: Obtenga información sobre cómo la clase Startup de ASP.NET Core configura los servicios y la canalización de solicitudes de la aplicación.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/17/2019
uid: fundamentals/startup
ms.openlocfilehash: cfd0a57d5d0b60862b017a170b6d5cbddf56f15a
ms.sourcegitcommit: b3894b65e313570e97a2ab78b8addd22f427cac8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/23/2019
ms.locfileid: "56744188"
---
# <a name="app-startup-in-aspnet-core"></a>Inicio de la aplicación en ASP.NET Core

[Tom Dykstra](https://github.com/tdykstra), [Luke Latham](https://github.com/guardrex) y [Steve Smith](https://ardalis.com)

La clase `Startup` configura los servicios y la canalización de solicitudes de la aplicación.

## <a name="the-startup-class"></a>Clase Startup

Las aplicaciones de ASP.NET Core utilizan una clase `Startup`, que se denomina `Startup` por convención. La clase `Startup`:

* Incluye opcionalmente un método <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> para configurar los *servicios* de la aplicación. Un servicio es un componente reutilizable que proporciona funcionalidades de la aplicación. Los servicios se configuran o, como también se denomina, se *registran* en `ConfigureServices` y se usan en la aplicación a través de la [inserción de dependencias (DI)](xref:fundamentals/dependency-injection) o <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.
* Incluye un método <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> para crear la canalización de procesamiento de solicitudes de la aplicación.

El runtime llama a `ConfigureServices` y `Configure` al iniciarse la aplicación:

[!code-csharp[](startup/sample_snapshot/Startup1.cs?highlight=4,10)]

La clase `Startup` se especifica para la aplicación al generar su [host](xref:fundamentals/index#host). El host de la aplicación se genera cuando `Build` se llama en el generador de host de la clase `Program`. Habitualmente, la clase `Startup` se especifica mediante una llamada al método [WebHostBuilderExtensions.UseStartup\<TStartup >](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) en el generador de host:

[!code-csharp[](startup/sample_snapshot/Program3.cs?name=snippet_Program&highlight=10)]

El host proporciona servicios que están disponibles para el constructor de clase `Startup`. La aplicación agrega servicios adicionales a través de `ConfigureServices`. Después, los servicios de la aplicación y el host están disponibles en `Configure` y en toda la aplicación.

Un uso común de la [inserción de dependencias](xref:fundamentals/dependency-injection) en la clase `Startup` consiste en insertar:

* <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> para configurar servicios según el entorno.
* <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> para leer la configuración.
* <xref:Microsoft.Extensions.Logging.ILoggerFactory> para crear un registrador en `Startup.ConfigureServices`.

[!code-csharp[](startup/sample_snapshot/Startup2.cs?highlight=7-8)]

Una alternativa a la inserción de `IHostingEnvironment` consiste en utilizar un enfoque basado en convenciones. Cuando la aplicación define clases `Startup` independientes para otros entornos (por ejemplo, `StartupDevelopment`), la clase `Startup` correspondiente se selecciona en tiempo de ejecución. La clase cuyo sufijo de nombre coincide con el entorno actual se establece como prioritaria. Si la aplicación se ejecuta en el entorno de desarrollo e incluye tanto la clase `Startup` como la clase `StartupDevelopment`, se utiliza la clase `StartupDevelopment`. Para obtener más información, consulte [Uso de varios entornos](xref:fundamentals/environments#environment-based-startup-class-and-methods).

Para obtener más información sobre el host, vea [El host](xref:fundamentals/index#host). Para obtener información sobre cómo controlar los errores que se producen durante el inicio, consulte [Control de excepciones de inicio](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>Método ConfigureServices

El método <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> es:

* Opcional.
* Lo llama el host antes del método `Configure` para configurar los servicios de la aplicación.
* Es donde se establecen por convención las [opciones de configuración](xref:fundamentals/configuration/index).

El patrón habitual consiste en llamar a todos los métodos `Add{Service}` y, luego, a todos los métodos `services.Configure{Service}`. Por ejemplo, consulte [Configurar servicios de identidad](xref:security/authentication/identity#pw).

El host puede configurar algunos servicios antes de que se llame a los métodos `Startup`. Para obtener más información, vea [El host](xref:fundamentals/index#host).

Para las características que requieren una configuración sustancial, hay métodos de extensión `Add{Service}` en <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>. Una aplicación ASP.NET Core típica registra los servicios de Entity Framework, Identity y MVC:

[!code-csharp[](startup/sample_snapshot/Startup3.cs?highlight=4,7,11)]

La adición de servicios al contenedor de servicios hace que estén disponibles en la aplicación y en el método `Configure`. Los servicios se resuelven a través de la [inserción de dependencias](xref:fundamentals/dependency-injection) o desde <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.

## <a name="the-configure-method"></a>El método Configure

El método <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> se usa para especificar la forma en que la aplicación responde a las solicitudes HTTP. La canalización de solicitudes se configura mediante la adición de componentes de [middleware](xref:fundamentals/middleware/index) a una instancia de <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>. `IApplicationBuilder` está disponible para el método `Configure`, pero no está registrado en el contenedor de servicios. El hospedaje crea un elemento `IApplicationBuilder` y lo pasa directamente a `Configure`.

Las [plantillas de ASP.NET Core](/dotnet/core/tools/dotnet-new) configuran la canalización con compatibilidad para lo siguiente:

* [Página de excepciones para el desarrollador](xref:fundamentals/error-handling#the-developer-exception-page)
* [Controlador de excepciones](xref:fundamentals/error-handling#configure-a-custom-exception-handling-page)
* [Seguridad de transporte estricta de HTTP (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts)
* [Redireccionamiento de HTTPS](xref:security/enforcing-ssl)
* [Archivos estáticos](xref:fundamentals/static-files)
* [Reglamento General de Protección de Datos (GDPR)](xref:security/gdpr)
* ASP.NET Core [MVC](xref:mvc/overview) y [Razor Pages](xref:razor-pages/index)

[!code-csharp[](startup/sample_snapshot/Startup4.cs)]

Cada método de extensión `Use` agrega uno o más componentes de middleware a la canalización de solicitudes. Por ejemplo, el método de extensión `UseMvc` agrega el [middleware de enrutamiento](xref:fundamentals/routing) a la canalización de solicitudes y establece [MVC](xref:mvc/overview) como controlador predeterminado.

Cada componente de middleware de la canalización de solicitudes es responsable de invocar al siguiente componente de la canalización o de cortocircuitar la cadena en caso de ser necesario. Si el cortocircuito no se produce a lo largo de la cadena de middleware, cada middleware tiene una segunda oportunidad de procesar la solicitud antes de que se envíe al cliente.

También se pueden especificar servicios adicionales, como `IHostingEnvironment` y `ILoggerFactory`, en la firma del método `Configure`. Cuando se especifican, esos servicios adicionales se insertan si están disponibles.

Para obtener más información sobre cómo usar `IApplicationBuilder` y el orden de procesamiento de middleware, consulte <xref:fundamentals/middleware/index>.

## <a name="convenience-methods"></a>Métodos de conveniencia

Para configurar los servicios y la canalización de procesamiento de solicitudes sin usar una clase `Startup`, llame a los métodos de conveniencia `ConfigureServices` y `Configure` en el generador de host. Varias llamadas a `ConfigureServices` se anexan entre sí. Si hay varias llamadas al método `Configure`, se usa la última llamada a `Configure`.

[!code-csharp[](startup/sample_snapshot/Program1.cs?highlight=18,22)]

## <a name="extend-startup-with-startup-filters"></a>Extensión del inicio con filtros de inicio

Use <xref:Microsoft.AspNetCore.Hosting.IStartupFilter> para configurar el middleware al principio o al final de la canalización de middleware [Configure](#the-configure-method) de una aplicación. `IStartupFilter` es útil para garantizar que un middleware se ejecuta antes o después del middleware agregado por bibliotecas al principio o al final de la canalización de procesamiento de solicitudes de la aplicación.

`IStartupFilter` implementa un método único, <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>, que recibe y devuelve `Action<IApplicationBuilder>`. <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> define una clase para configurar la canalización de solicitudes de una aplicación. Para más información, vea [Creación de una canalización de middleware con IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).

Cada `IStartupFilter` implementa uno o más middleware en la canalización de solicitudes. Los filtros se invocan en el orden en que se agregaron al contenedor de servicios. Los filtros pueden agregar middleware antes o después de pasar el control al siguiente filtro, por lo que se anexan al principio o al final de la canalización de la aplicación.

En el ejemplo siguiente, se muestra cómo registrar un componente de middleware con `IStartupFilter`.

El componente de middleware `RequestSetOptionsMiddleware` establece un valor de opciones de un parámetro de cadena de consulta:

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsMiddleware.cs?name=snippet1&highlight=21)]

`RequestSetOptionsMiddleware` está configurado en las clase `RequestSetOptionsStartupFilter`:

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

`IStartupFilter` está registrado en el contenedor de servicios de <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> y aumenta `Startup` desde fuera de la clase `Startup`:

[!code-csharp[](startup/sample_snapshot/Program2.cs?name=snippet1&highlight=4-5)]

Cuando se proporciona un parámetro de cadena de consulta para `option`, el middleware procesa el valor asignado antes de que el middleware MVC represente la respuesta:

![Ventana del explorador que muestra la página de índice representada. El valor de Option se representa como "From Middleware" basándose en una solicitud a la página con el parámetro de cadena de consulta y el valor de la opción establecido en "From Middleware".](startup/_static/index.png)

El orden de ejecución de middleware se establece según el orden de registros de `IStartupFilter`:

* Varias implementaciones de `IStartupFilter` pueden interactuar con los mismos objetos. Si el orden es importante, ordene los registros de servicio de `IStartupFilter` para que coincidan con el orden en que se deben ejecutar los middleware.
* Las bibliotecas pueden agregar middleware con una o varias implementaciones de `IStartupFilter` que se ejecuten antes o después de otro middleware de aplicación registrado con `IStartupFilter`. Para invocar un middleware `IStartupFilter` antes que un middleware agregado por el `IStartupFilter` de una biblioteca, coloque el registro del servicio antes de que la biblioteca se agregue al contenedor de servicios. Para invocarlo posteriormente, coloque el registro del servicio después de que se agregue la biblioteca.

## <a name="add-configuration-at-startup-from-an-external-assembly"></a>Agregar opciones de configuración en el inicio desde un ensamblado externo

Una implementación de <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> permite agregar mejoras a una aplicación al iniciarla a partir de un ensamblado externo fuera de la clase `Startup` de esta. Para obtener más información, vea <xref:fundamentals/configuration/platform-specific-configuration>.

## <a name="additional-resources"></a>Recursos adicionales

* [El host](xref:fundamentals/index#host)
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>
