---
title: Integre ASP.NET Core componentes de Razor en aplicaciones de Razor Pages y MVC
author: guardrex
description: Obtenga información sobre los escenarios de enlace de datos para componentes y elementos DOM en Blazor aplicaciones.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/integrate-components
ms.openlocfilehash: de1a37ffd9456c956e3d84fcc69431ecb794513c
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453215"
---
# <a name="integrate-aspnet-core-razor-components-into-razor-pages-and-mvc-apps"></a>Integre ASP.NET Core componentes de Razor en aplicaciones de Razor Pages y MVC

Por [Luke Latham](https://github.com/guardrex) y [Daniel Roth](https://github.com/danroth27)

Los componentes de Razor se pueden integrar en aplicaciones de Razor Pages y MVC. Cuando se representa la página o la vista, los componentes se pueden representar al mismo tiempo.

## <a name="prepare-the-app-to-use-components-in-pages-and-views"></a>Preparar la aplicación para usar componentes en páginas y vistas

Una aplicación Razor Pages o MVC existente puede integrar los componentes de Razor en páginas y vistas:

1. En el archivo de diseño de la aplicación ( *_Layout. cshtml*):

   * Agregue la siguiente etiqueta de `<base>` al elemento `<head>`:

     ```html
     <base href="~/" />
     ```

     El valor `href` (la *ruta de acceso base*de la aplicación) en el ejemplo anterior supone que la aplicación reside en la ruta de acceso URL raíz (`/`). Si la aplicación es una aplicación secundaria, siga las instrucciones de la sección *ruta de acceso base* de la aplicación del artículo <xref:host-and-deploy/blazor/index#app-base-path>.

     El archivo *_Layout. cshtml* se encuentra en la carpeta *páginas/compartidas* de una aplicación de Razor pages o en una carpeta de *vistas/compartidas* en una aplicación MVC.

   * Agregue una etiqueta de `<script>` para el script *increíblemente. Server. js* inmediatamente antes de la etiqueta de cierre `</body>`:

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     El marco de trabajo agrega el script *extraordinaria. Server. js* a la aplicación. No es necesario agregar manualmente el script a la aplicación.

1. Agregue un archivo *_Imports. Razor* a la carpeta raíz del proyecto con el siguiente contenido (cambie el último espacio de nombres, `MyAppNamespace`, al espacio de nombres de la aplicación):

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

1. En `Startup.ConfigureServices`, registre el servicio de servidor de extraordinarias:

   ```csharp
   services.AddServerSideBlazor();
   ```

1. En `Startup.Configure`, agregue el punto de conexión del concentrador de increíble a `app.UseEndpoints`:

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. Integre los componentes en cualquier página o vista. Para obtener más información, vea la sección [representar componentes de una página o una vista](#render-components-from-a-page-or-view) .

## <a name="use-routable-components-in-a-razor-pages-app"></a>Uso de componentes enrutables en una aplicación Razor Pages

*Esta sección se aplica a la adición de componentes que se pueden enrutar directamente desde las solicitudes del usuario.*

Para admitir componentes de Razor enrutables en Razor Pages aplicaciones:

1. Siga las instrucciones de la sección [preparación de la aplicación para usar componentes en páginas y vistas](#prepare-the-app-to-use-components-in-pages-and-views) .

1. Agregue un archivo *app. Razor* a la raíz del proyecto con el siguiente contenido:

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

1. Agregue un archivo *_Host. cshtml* a la carpeta *pages* con el siguiente contenido:

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   Los componentes de usan el archivo Shared *_Layout. cshtml* para su diseño.

1. Agregue una ruta de prioridad baja para la página *_Host. cshtml* a la configuración del punto de conexión en `Startup.Configure`:

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

   Para obtener más información sobre los espacios de nombres, vea la sección [espacios de nombres de componentes](#component-namespaces) .

## <a name="use-routable-components-in-an-mvc-app"></a>Uso de componentes enrutables en una aplicación MVC

*Esta sección se aplica a la adición de componentes que se pueden enrutar directamente desde las solicitudes del usuario.*

Para admitir componentes de Razor enrutables en aplicaciones MVC:

1. Siga las instrucciones de la sección [preparación de la aplicación para usar componentes en páginas y vistas](#prepare-the-app-to-use-components-in-pages-and-views) .

1. Agregue un archivo *app. Razor* a la raíz del proyecto con el siguiente contenido:

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

1. Agregue un archivo *_Host. cshtml* a la carpeta *views/Home* con el siguiente contenido:

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   Los componentes de usan el archivo Shared *_Layout. cshtml* para su diseño.

1. Agregue una acción al controlador Home:

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. Agregue una ruta de prioridad baja para la acción de controlador que devuelve la vista *_Host. cshtml* a la configuración del punto de conexión en `Startup.Configure`:

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. Cree una carpeta de *páginas* y agregue componentes enrutables a la aplicación. Por ejemplo:

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   Para obtener más información sobre los espacios de nombres, vea la sección [espacios de nombres de componentes](#component-namespaces) .

## <a name="component-namespaces"></a>Espacios de nombres de componentes

Al usar una carpeta personalizada para contener los componentes de la aplicación, agregue el espacio de nombres que representa la carpeta a la página o vista, o al archivo *_ViewImports. cshtml* . En el ejemplo siguiente:

* Cambie `MyAppNamespace` al espacio de nombres de la aplicación.
* Si no se usa una carpeta denominada *componentes* para contener los componentes, cambie `Components` a la carpeta donde residen los componentes.

```cshtml
@using MyAppNamespace.Components
```

El archivo *_ViewImports. cshtml* se encuentra en la carpeta *pages* de una aplicación Razor pages o en la carpeta *views* de una aplicación MVC.

Para más información, consulte <xref:blazor/components#import-components>.

## <a name="render-components-from-a-page-or-view"></a>Representar componentes de una página o vista

*En esta sección se hace referencia a la adición de componentes a páginas o vistas, donde los componentes no son enrutables directamente desde las solicitudes del usuario.*

Para representar un componente de una página o vista, use la aplicación auxiliar de etiquetas `Component`:

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

El tipo de parámetro debe ser serializable de JSON, lo que normalmente significa que el tipo debe tener un constructor predeterminado y las propiedades configurables. Por ejemplo, puede especificar un valor para `IncrementAmount` porque el tipo de `IncrementAmount` es un `int`, que es un tipo primitivo admitido por el serializador JSON.

`RenderMode` configura si el componente:

* Se representa en la página.
* Se representa como HTML estático en la página o si incluye la información necesaria para iniciar una aplicación extraordinaria desde el agente de usuario.

| `RenderMode`        | Descripción |
| ------------------- | ----------- |
| `ServerPrerendered` | Representa el componente en código HTML estático e incluye un marcador para una aplicación de Blazor Server. Cuando se inicia el agente de usuario, este marcador se usa para arrancar una aplicación Blazor. |
| `Server`            | Representa un marcador para una aplicación de Blazor Server. La salida del componente no está incluida. Cuando se inicia el agente de usuario, este marcador se usa para arrancar una aplicación Blazor. |
| `Static`            | Representa el componente en HTML estático. |

Mientras que las páginas y las vistas pueden utilizar componentes, el opuesto no es cierto. Los componentes no pueden usar escenarios específicos de la página y de la vista, como vistas y secciones parciales. Para usar la lógica de la vista parcial en un componente, se debe factorizar la lógica de vista parcial en un componente.

No se admite la representación de componentes de servidor desde una página HTML estática.

Para obtener más información sobre cómo se representan los componentes, el estado del componente y la aplicación auxiliar de etiquetas `Component`, consulte los siguientes artículos:

* <xref:blazor/hosting-models>
* <xref:blazor/hosting-model-configuration>
