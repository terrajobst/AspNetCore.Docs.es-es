---
title: "Inicio de la aplicación de ASP.NET Core"
author: ardalis
description: Explica la clase de inicio de ASP.NET Core.
keywords: "Núcleo de ASP.NET, inicio, el método Configure, ConfigureServices (método)"
ms.author: tdykstra
manager: wpickett
ms.date: 02/29/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/startup
ms.openlocfilehash: 009df1416c822018d6e88912cc77e525c7349c34
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2017
---
# <a name="application-startup-in-aspnet-core"></a>Inicio de la aplicación de ASP.NET Core

Por [Steve Smith](https://ardalis.com/) y [Tom Dykstra](https://github.com/tdykstra/)

La `Startup` clase configura servicios y la canalización de solicitud de la aplicación. 

## <a name="the-startup-class"></a>La clase de inicio

Las aplicaciones de ASP.NET Core requieren un `Startup` clase. Por convención, el `Startup` clase se denomina "Inicio". Especifique el nombre de clase de inicio en el `Main` del programa [WebHostBuilderExtensions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderextensions) [ `UseStartup<TStartup>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) método. Vea [hospedaje](xref:fundamentals/hosting) para obtener más información acerca de `WebHostBuilder`, que se ejecuta antes de `Startup`.

Puede definir independiente `Startup` clases para diferentes entornos y uno se seleccionará en tiempo de ejecución adecuado. Si especifica `startupAssembly` en el [configuración WebHost](https://docs.microsoft.com/aspnet/core/fundamentals/hosting?tabs=aspnetcore2x#configuring-a-host) opciones de hospedaje, se carga dicho ensamblado de inicio y buscar un `Startup` o `Startup[Environment]` tipo. La clase cuyos coincidencias de sufijo de nombre se les dará prioridad el entorno actual, por lo que si la aplicación se ejecuta en el *desarrollo* entorno e incluye tanto una `Startup` y un `StartupDevelopment` (clase), el `StartupDevelopment` clase será usar. Vea [FindStartupType](https://github.com/aspnet/Hosting/blob/rel/1.1.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs) en `StartupLoader` y [trabajar con varios entornos](environments.md#startup-conventions).

Como alternativa, puede definir un fijo `Startup` clase que se usará independientemente del entorno mediante una llamada a `UseStartup<TStartup>`. Éste es el enfoque recomendado.

El `Startup` constructor de clase puede aceptar las dependencias que se proporcionan a través de [inyección de dependencia](xref:fundamentals/dependency-injection). Un enfoque común consiste en usar `IHostingEnvironment` configurar [configuración](xref:fundamentals/configuration) orígenes.

El `Startup` clase debe incluir un `Configure` método y, opcionalmente, puede incluir un `ConfigureServices` método, que se llaman cuando se inicia la aplicación. También puede incluir la clase [versiones específicas del entorno de estos métodos](xref:fundamentals/environments#startup-conventions). `ConfigureServices`, si está presente, se llama antes de `Configure`.

Obtenga información acerca de [control de excepciones durante el inicio de la aplicación](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>El método ConfigureServices

El [ConfigureServices](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.startupbase#Microsoft_AspNetCore_Hosting_StartupBase_ConfigureServices_Microsoft_Extensions_DependencyInjection_IServiceCollection_) método es opcional; pero si utiliza, se llama antes de la `Configure` método por el host de web. Puede configurar el host de web algunos servicios antes de ``Startup`` se denominan métodos (consulte [hospedaje](xref:fundamentals/hosting)). Por convención, [opciones de configuración](xref:fundamentals/configuration) se establecen en este método.

Funciones que requieren el programa de instalación sustancial hay `Add[Service]` métodos de extensión en [IServiceCollection](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.iservicecollection). Este ejemplo de la plantilla de sitio web predeterminado configura la aplicación para usar los servicios de Entity Framework, identidad y MVC:

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

Agregar servicios al contenedor de servicios hace que estén disponibles dentro de la aplicación a través de [inyección de dependencia](xref:fundamentals/dependency-injection).

## <a name="services-available-in-startup"></a>Servicios disponibles en el inicio

Inserción de dependencias de ASP.NET Core proporciona servicios durante el inicio de la aplicación. Puede solicitar estos servicios mediante la inclusión de la interfaz adecuada como un parámetro en su `Startup` constructor de la clase o su `Configure` método. El `ConfigureServices` método solo toma un `IServiceCollection` parámetro (pero cualquier can servicio registrado recuperarse de esta colección, por lo que los parámetros adicionales no son necesarios).

A continuación se muestran algunos de los servicios normalmente solicitados por `Startup` métodos:

* En el constructor: `IHostingEnvironment`,`ILogger<Startup>`
* En `ConfigureServices`:`IServiceCollection`
* En `Configure`: `IApplicationBuilder`, `IHostingEnvironment`,`ILoggerFactory`

Los servicios agregados por la ``WebHostBuilder`` ``ConfigureServices`` método pueden solicitar la ``Startup`` constructor de clase o su ``Configure`` método. Use `WebHostBuilder` proporcionar cualquier otro servicio necesita durante `Startup` métodos.

## <a name="the-configure-method"></a>El método Configure

El `Configure` método se utiliza para especificar cómo la aplicación ASP.NET responderá a las solicitudes HTTP. La canalización de solicitudes está configurada mediante la adición de [middleware](middleware.md) componentes a un `IApplicationBuilder` instancia proporcionada por inyección de dependencia.

En el siguiente ejemplo de la plantilla de sitio web predeterminado, varios métodos de extensión se utilizan para configurar la canalización con compatibilidad para [BrowserLink](http://vswebessentials.com/features/browserlink), páginas de errores, archivos estáticos, ASP.NET MVC e identidad.

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

Cada `Use` método de extensión se agrega un [middleware](xref:fundamentals/middleware) componente a la canalización de solicitud. Por ejemplo, el `UseMvc` método de extensión agrega la [enrutamiento](routing.md) middleware a la canalización de solicitud y configura [MVC](xref:mvc/overview) como controlador predeterminado.

Para obtener más información sobre cómo usar `IApplicationBuilder`, consulte [Middleware](xref:fundamentals/middleware).

Servicios adicionales, como `IHostingEnvironment` y `ILoggerFactory` también se pueden especificar en la firma del método, en cuyo caso estos servicios serán [insertado](dependency-injection.md) si están disponibles. 

## <a name="additional-resources"></a>Recursos adicionales

* [Trabajar con varios entornos](xref:fundamentals/environments)
* [Middleware](xref:fundamentals/middleware)
* [Registro](xref:fundamentals/logging)
* [Configuración](xref:fundamentals/configuration)
