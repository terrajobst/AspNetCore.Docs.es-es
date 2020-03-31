---
title: Integración de componentes Razor de ASP.NET Core en aplicaciones Razor Pages y MVC
author: guardrex
description: Obtenga información sobre los escenarios de enlace de datos para componentes y elementos DOM en aplicaciones Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/17/2020
no-loc:
- Blazor
- SignalR
uid: blazor/integrate-components
ms.openlocfilehash: cf6056e0985d5433bddecac8dd183ca3f4c2af5b
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218939"
---
# <a name="integrate-aspnet-core-razor-components-into-razor-pages-and-mvc-apps"></a>Integración de componentes Razor de ASP.NET Core en aplicaciones Razor Pages y MVC

Por [Luke Latham](https://github.com/guardrex) y [Daniel Roth](https://github.com/danroth27)

Los componentes Razor se pueden integrar en aplicaciones de Razor Pages y MVC. Cuando se representa la página o la vista, los componentes se pueden representar previamente al mismo tiempo.

## <a name="prepare-the-app-to-use-components-in-pages-and-views"></a>Preparación de la aplicación para usar componentes en páginas y vistas

Una aplicación Razor Pages o MVC existente puede integrar componentes Razor en páginas y vistas:

1. En el archivo de diseño de la aplicación ( *_Layout.cshtml*):

   * Agregue la etiqueta `<base>` siguiente al elemento `<head>`:

     ```html
     <base href="~/" />
     ```

     El valor `href` (la *ruta de acceso base de la aplicación*) del ejemplo anterior da por hecho que la aplicación reside en la ruta de acceso URL raíz (`/`). Si la aplicación es una subaplicación, siga las instrucciones de la sección *Ruta de acceso base de la aplicación* del artículo <xref:host-and-deploy/blazor/index#app-base-path>.

     El archivo *_Layout.cshtml* se encuentra en la carpeta *Pages/Shared* en una aplicación Razor Pages o en la carpeta *Views/Shared* en una aplicación MVC.

   * Agregue una etiqueta `<script>` para el script *blazor.server.js* inmediatamente antes de la etiqueta `</body>` de cierre:

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     El marco agrega el script *blazor.server.js* a la aplicación. No es necesario agregar manualmente el script a la aplicación.

1. Agregue un archivo *_Imports.razor* a la carpeta raíz del proyecto con el contenido siguiente (cambie el último espacio de nombres, `MyAppNamespace`, al espacio de nombres de la aplicación):

   ```razor
   @using System.Net.Http
   @using Microsoft.AspNetCore.Authorization
   @using Microsoft.AspNetCore.Components.Authorization
   @using Microsoft.AspNetCore.Components.Forms
   @using Microsoft.AspNetCore.Components.Routing
   @using Microsoft.AspNetCore.Components.Web
   @using Microsoft.JSInterop
   @using MyAppNamespace
   ```

1. En `Startup.ConfigureServices`, registre el servicio Blazor Server:

   ```csharp
   services.AddServerSideBlazor();
   ```

1. En `Startup.Configure`, agregue el punto de conexión de Blazor Hub a `app.UseEndpoints`:

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. Integre los componentes en cualquier página o vista. Para obtener más información, vea la sección [Representación de componentes a partir de una página o vista](#render-components-from-a-page-or-view).

## <a name="use-routable-components-in-a-razor-pages-app"></a>Uso de componentes enrutables en una aplicación Razor Pages

*Esta sección pertenece a la incorporación de componentes que se pueden enrutar directamente desde las solicitudes del usuario.*

Para admitir componentes Razor enrutables en aplicaciones Razor Pages, haga lo siguiente:

1. Siga las instrucciones de la sección [Preparación de la aplicación para usar componentes en páginas y vistas](#prepare-the-app-to-use-components-in-pages-and-views).

1. Agregue un archivo *App.razor* a la raíz del proyecto con el contenido siguiente:

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. Agregue un archivo *_Host.cshtml* a la carpeta *Pages* con el contenido siguiente:

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   Los componentes usan el archivo compartido *_Layout.cshtml* para su diseño.

1. Agregue una ruta de prioridad baja para la página *_Host.cshtml* a la configuración del punto de conexión en `Startup.Configure`:

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. Agregue componentes enrutables a la aplicación. Por ejemplo:

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   Para obtener más información sobre los espacios de nombres, vea la sección [Espacios de nombres de componentes](#component-namespaces).

## <a name="use-routable-components-in-an-mvc-app"></a>Uso de componentes enrutables en una aplicación MVC

*Esta sección pertenece a la incorporación de componentes que se pueden enrutar directamente desde las solicitudes del usuario.*

Para admitir componentes Razor enrutables en aplicaciones MVC, haga lo siguiente:

1. Siga las instrucciones de la sección [Preparación de la aplicación para usar componentes en páginas y vistas](#prepare-the-app-to-use-components-in-pages-and-views).

1. Agregue un archivo *App.razor* a la raíz del proyecto con el contenido siguiente:

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. Agregue un archivo *_Host.cshtml* a la carpeta *Views/Home* con el contenido siguiente:

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   Los componentes usan el archivo compartido *_Layout.cshtml* para su diseño.

1. Agregue una acción al controlador de inicio:

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. Agregue una ruta de prioridad baja para la acción de controlador que devuelve la vista *_Host.cshtml* a la configuración del punto de conexión en `Startup.Configure`:

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. Cree una carpeta *Pages* y agregue componentes enrutables a la aplicación. Por ejemplo:

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   Para obtener más información sobre los espacios de nombres, vea la sección [Espacios de nombres de componentes](#component-namespaces).

## <a name="component-namespaces"></a>Espacios de nombres de componentes

Al usar una carpeta personalizada para contener los componentes de la aplicación, agregue el espacio de nombres que representa la carpeta a la página o vista, o al archivo *_ViewImports.cshtml*. En el ejemplo siguiente:

* Cambie `MyAppNamespace` en el espacio de nombres de la aplicación.
* Si no se usa una carpeta denominada *Components* para contener los componentes, cambie `Components` en la carpeta donde estos se encuentren.

```cshtml
@using MyAppNamespace.Components
```

El archivo *_ViewImports.cshtml* se encuentra en la carpeta *Pages* de una aplicación Razor Pages o en la carpeta *Views* de una aplicación MVC.

Para obtener más información, vea <xref:blazor/components#import-components>.

## <a name="render-components-from-a-page-or-view"></a>Representación de componentes a partir de una página o vista

*Esta sección pertenece a la adición de componentes a páginas o vistas, donde los componentes no son enrutables directamente desde las solicitudes del usuario.*

Para representar un componente a partir de una página o vista, use el [asistente de etiquetas de componente](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper).

Para obtener más información sobre cómo se representan los componentes, el estado del componente y el Asistente de etiquetas `Component`, vea los artículos siguientes:

* <xref:blazor/hosting-models>
* <xref:blazor/hosting-model-configuration>
* <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>
