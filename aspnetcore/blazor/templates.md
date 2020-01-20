---
title: Plantillas de ASP.NET Core Blazor
author: guardrex
description: Obtenga información sobre ASP.NET Core Blazor plantillas de aplicación y Blazor estructura del proyecto.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/templates
ms.openlocfilehash: 2a95b986450471b474d93ead252255f2bd9d4918
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2020
ms.locfileid: "76160124"
---
# <a name="aspnet-core-opno-locblazor-templates"></a>Plantillas de ASP.NET Core Blazor

Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

El marco de Blazor proporciona plantillas para desarrollar aplicaciones para cada uno de los modelos de hospedaje de Blazor:

* Blazor webassembly (`blazorwasm`)
* Servidor de Blazor (`blazorserver`)

Para obtener más información sobre los modelos de hospedaje de Blazor, vea <xref:blazor/hosting-models>.

Para obtener instrucciones paso a paso sobre cómo crear una aplicación Blazor a partir de una plantilla, consulte <xref:blazor/get-started>.

## <a name="opno-locblazor-project-structure"></a>Blazor estructura del proyecto

Los siguientes archivos y carpetas componen una aplicación Blazor generada a partir de una plantilla de Blazor:

* *Program.cs* &ndash; el punto de entrada de la aplicación que configura el [host](xref:fundamentals/host/generic-host)de ASP.net Core. El código de este archivo es común a todas ASP.NET Core aplicaciones generadas a partir de plantillas de ASP.NET Core.

* *Startup.cs* &ndash; contiene la lógica de inicio de la aplicación. La clase `Startup` define dos métodos:

  * `ConfigureServices` &ndash; configura los servicios de inserción de [dependencias (di)](xref:fundamentals/dependency-injection) de la aplicación. En las aplicaciones de Blazor Server, los servicios se agregan llamando a <xref:Microsoft.Extensions.DependencyInjection.ComponentServiceCollectionExtensions.AddServerSideBlazor*>y el `WeatherForecastService` se agrega al contenedor de servicios para que lo use el componente `FetchData` de ejemplo.
  * `Configure` &ndash; configura la canalización de control de solicitudes de la aplicación:
    * Blazor webassembly &ndash; agrega el componente `App` (especificado como el elemento DOM `app` al método `AddComponent`), que es el componente raíz de la aplicación.
    * Servidor de Blazor
      * se llama a <xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub*> para configurar un extremo para la conexión en tiempo real con el explorador. La conexión se crea con [SignalR](xref:signalr/introduction), que es un marco para agregar funcionalidad web en tiempo real a las aplicaciones.
      * Se llama a [MapFallbackToPage ("/_Host")](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapFallbackToPage*) para configurar la página raíz de la aplicación (*pages/_Host. cshtml*) y habilitar la navegación.

* *wwwroot/index.html* (Blazor webassembly) &ndash; la página raíz de la aplicación implementada como una página HTML:
  * Cuando se solicita inicialmente cualquier página de la aplicación, esta página se representa y se devuelve en la respuesta.
  * La página especifica dónde se representa el componente de `App` raíz. El componente `App` (*app. Razor*) se especifica como el elemento `app` Dom en el método `AddComponent` en `Startup.Configure`.
  * Se carga el archivo `_framework/blazor.webassembly.js` JavaScript, que:
    * Descarga el tiempo de ejecución de .NET, la aplicación y las dependencias de la aplicación.
    * Inicializa el tiempo de ejecución para ejecutar la aplicación.

* *Pages/_Host. cshtml* (Blazor Server) &ndash; la página raíz de la aplicación implementada como una página de Razor:
  * Cuando se solicita inicialmente cualquier página de la aplicación, esta página se representa y se devuelve en la respuesta.
  * Se carga el archivo `_framework/blazor.server.js` JavaScript, que configura la conexión SignalR en tiempo real entre el explorador y el servidor.
  * La página host especifica dónde se representa el componente de `App` raíz (*app. Razor*).

* *App. razor* &ndash; el componente raíz de la aplicación que configura el enrutamiento del lado cliente mediante el componente <xref:Microsoft.AspNetCore.Components.Routing.Router>. El componente `Router` intercepta la navegación del explorador y representa la página que coincide con la dirección solicitada.

* La carpeta *pages* &ndash; contiene los componentes o páginas enrutables ( *. Razor*) que componen la aplicación Blazor. La ruta de cada página se especifica mediante la directiva [`@page`](xref:mvc/views/razor#page) . La plantilla incluye los componentes siguientes:
  * `Index` (*index. Razor*) &ndash; implementa la Página principal.
  * `Counter` (*Counter. Razor*) &ndash; implementa la página Counter.
  * `Error` (*error. Razor*, solo aplicación de Blazor Server) &ndash; representa cuando se produce una excepción no controlada en la aplicación.
  * `FetchData` (*FetchData. Razor*) &ndash; implementa la página capturar datos.

* La carpeta *compartida* &ndash; contiene otros componentes de interfaz de usuario ( *. Razor*) usados por la aplicación:
  * `MainLayout` (*MainLayout. Razor*) &ndash; el componente de diseño de la aplicación.
  * `NavMenu` (*NavMenu. Razor*) &ndash; implementa la navegación de la barra lateral. Incluye el [componente NavLink](xref:blazor/routing#navlink-component) (<xref:Microsoft.AspNetCore.Components.Routing.NavLink>), que representa los vínculos de navegación a otros componentes de Razor. El componente `NavLink` indica automáticamente un estado seleccionado cuando se carga su componente, lo que ayuda al usuario a entender qué componente se muestra actualmente.

* *_Imports. razor* &ndash; incluye directivas de Razor comunes para incluir en los componentes de la aplicación ( *. Razor*), como las directivas de [`@using`](xref:mvc/views/razor#using) para los espacios de nombres.

* La carpeta de *datos* (servidorBlazor) &ndash; contiene la clase `WeatherForecast` y la implementación del `WeatherForecastService` que proporcionan datos meteorológicos de ejemplo al componente `FetchData` de la aplicación.

* *wwwroot* &ndash; la carpeta [raíz Web](xref:fundamentals/index#web-root) de la aplicación que contiene los activos estáticos públicos de la aplicación.

* *appSettings. JSON* (servidorBlazor) &ndash; valores de configuración de la aplicación.
