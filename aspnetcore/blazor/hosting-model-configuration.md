---
title: Configuración del modelo de hospedaje de Blazor de ASP.NET Core
author: guardrex
description: Obtenga información acerca de la configuración del modelo de hospedaje de Blazor, incluida la integración de componentes de Razor en aplicaciones Razor Pages y MVC.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/04/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-model-configuration
ms.openlocfilehash: 91dd1aa32bb5165845eb4794377a50ccbd01adc3
ms.sourcegitcommit: d2ba66023884f0dca115ff010bd98d5ed6459283
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2020
ms.locfileid: "77214944"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a>Configuración del modelo de hospedaje de ASP.NET Core increíblemente

Por [Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

En este artículo se trata la configuración del modelo de hospedaje.

<!-- For future use:

## Blazor WebAssembly

-->

## <a name="blazor-server"></a>Servidor Blazor

### <a name="integrate-razor-components-into-razor-pages-and-mvc-apps"></a>Integre componentes de Razor en aplicaciones de Razor Pages y MVC

#### <a name="use-components-in-pages-and-views"></a>Usar componentes en páginas y vistas

Una aplicación Razor Pages o MVC existente puede integrar los componentes de Razor en páginas y vistas:

1. En el archivo de diseño de la aplicación ( *_Layout. cshtml*):

   * Agregue la siguiente etiqueta de `<base>` al elemento `<head>`:

     ```html
     <base href="~/" />
     ```

     El valor `href` (la *ruta de acceso base*de la aplicación) en el ejemplo anterior supone que la aplicación reside en la ruta de acceso URL raíz (`/`). Si la aplicación es una aplicación secundaria, siga las instrucciones de la sección *ruta de acceso base* de la aplicación del artículo <xref:host-and-deploy/blazor/index#app-base-path>.

     El archivo *_Layout. cshtml* se encuentra en la carpeta *páginas/compartidas* de una aplicación de Razor pages o en una carpeta de *vistas/compartidas* en una aplicación MVC.

   * Agregue una etiqueta de `<script>` para el script *increíblemente. Server. js* justo antes de la etiqueta de cierre `</body>`:

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     El marco de trabajo agrega el script *extraordinaria. Server. js* a la aplicación. No es necesario agregar manualmente el script a la aplicación.

1. Agregue un archivo *_Imports. Razor* a la carpeta raíz del proyecto con el siguiente contenido (cambie el último espacio de nombres, `MyAppNamespace`, al espacio de nombres de la aplicación):

   ```csharp
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

1. Integre los componentes en cualquier página o vista. Para más información, consulte la sección *integración de componentes en Razor pages y aplicaciones MVC* del artículo <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.

#### <a name="use-routable-components-in-a-razor-pages-app"></a>Uso de componentes enrutables en una aplicación Razor Pages

Para admitir componentes de Razor enrutables en Razor Pages aplicaciones:

1. Siga las instrucciones de la sección [uso de componentes en páginas y vistas](#use-components-in-pages-and-views) .

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

   Al usar una carpeta personalizada para contener los componentes de la aplicación, agregue el espacio de nombres que representa la carpeta al archivo *pages/_ViewImports. cshtml* . Para más información, consulte <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.

#### <a name="use-routable-components-in-an-mvc-app"></a>Uso de componentes enrutables en una aplicación MVC

Para admitir componentes de Razor enrutables en aplicaciones MVC:

1. Siga las instrucciones de la sección [uso de componentes en páginas y vistas](#use-components-in-pages-and-views) .

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

   Al usar una carpeta personalizada para contener los componentes de la aplicación, agregue el espacio de nombres que representa la carpeta al archivo *views/_ViewImports. cshtml* . Para más información, consulte <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.

### <a name="reflect-the-connection-state-in-the-ui"></a>Reflejar el estado de conexión en la interfaz de usuario

Cuando el cliente detecta que se ha perdido la conexión, se muestra al usuario una interfaz de usuario predeterminada mientras el cliente intenta volver a conectarse. Si se produce un error en la reconexión, se proporciona al usuario la opción de volver a intentarlo.

Para personalizar la interfaz de usuario, defina un elemento con un `id` de `components-reconnect-modal` en la `<body>` de la página de Razor de *_Host. cshtml* :

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

En la tabla siguiente se describen las clases CSS aplicadas al elemento `components-reconnect-modal`.

| Clase CSS                       | Indica&hellip; |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | Una conexión perdida. El cliente está intentando volver a conectarse. Muestre el modal. |
| `components-reconnect-hide`     | Una conexión activa se restablece en el servidor. Oculte el modal. |
| `components-reconnect-failed`   | Error de reconexión, probablemente debido a un error de red. Para intentar la reconexión, llame a `window.Blazor.reconnect()`. |
| `components-reconnect-rejected` | Reconexión rechazada. Se alcanzó el servidor pero se rechazó la conexión y se pierde el estado del usuario en el servidor. Para volver a cargar la aplicación, llame a `location.reload()`. Este estado de conexión puede producirse cuando:<ul><li>Se produce un bloqueo en el circuito del servidor.</li><li>El cliente se desconecta el tiempo suficiente para que el servidor Quite el estado del usuario. Se eliminan las instancias de los componentes con los que el usuario está interactuando.</li><li>El servidor se reinicia o se recicla el proceso de trabajo de la aplicación.</li></ul> |

### <a name="render-mode"></a>Modo de representación

Las aplicaciones de servidor increíbles se configuran de forma predeterminada para prerepresentar la interfaz de usuario en el servidor antes de que se establezca la conexión de cliente con el servidor. Esto se configura en la página de Razor *_Host. cshtml* :

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

`RenderMode` configura si el componente:

* Se representa en la página.
* Se representa como HTML estático en la página o si incluye la información necesaria para iniciar una aplicación extraordinaria desde el agente de usuario.

| `RenderMode`        | Descripción |
| ------------------- | ----------- |
| `ServerPrerendered` | Representa el componente en código HTML estático e incluye un marcador para una aplicación de Blazor Server. Cuando se inicia el agente de usuario, este marcador se usa para arrancar una aplicación Blazor. |
| `Server`            | Representa un marcador para una aplicación de Blazor Server. La salida del componente no está incluida. Cuando se inicia el agente de usuario, este marcador se usa para arrancar una aplicación Blazor. |
| `Static`            | Representa el componente en HTML estático. |

No se admite la representación de componentes de servidor desde una página HTML estática.

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a>Representación de componentes interactivos con estado desde vistas y páginas de Razor

Los componentes interactivos con estado se pueden agregar a una página o vista de Razor.

Cuando se representa la página o la vista:

* El componente se representará con la página o la vista.
* Se pierde el estado inicial del componente usado para la representación previa.
* Cuando se establece la conexión SignalR, se crea un nuevo estado del componente.

La siguiente página de Razor representa un componente de `Counter`:

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a>Representación de componentes no interactivos desde páginas y vistas de Razor

En la siguiente página de Razor, el componente de `Counter` se representa estáticamente con un valor inicial que se especifica mediante un formulario:

```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>

<component type="typeof(Counter)" render-mode="Static" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

Como `MyComponent` se representa estáticamente, el componente no puede ser interactivo.

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a>Configuración del cliente de SignalR para aplicaciones de Blazor Server

A veces, debe configurar el cliente de SignalR que usan las aplicaciones de Blazor Server. Por ejemplo, puede que desee configurar el registro en el SignalR cliente para diagnosticar un problema de conexión.

Para configurar el cliente de SignalR en el archivo *pages/_Host. cshtml* :

* Agregue un atributo `autostart="false"` a la etiqueta de `<script>` para el script de `blazor.server.js`.
* Llame a `Blazor.start` y pase un objeto de configuración que especifique el generador de SignalR.

```html
<script src="_framework/blazor.server.js" autostart="false"></script>
<script>
  Blazor.start({
    configureSignalR: function (builder) {
      builder.configureLogging("information"); // LogLevel.Information
    }
  });
</script>
```
