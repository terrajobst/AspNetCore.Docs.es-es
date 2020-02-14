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
# <a name="aspnet-core-blazor-hosting-model-configuration"></a><span data-ttu-id="439a9-103">Configuración del modelo de hospedaje de ASP.NET Core increíblemente</span><span class="sxs-lookup"><span data-stu-id="439a9-103">ASP.NET Core Blazor hosting model configuration</span></span>

<span data-ttu-id="439a9-104">Por [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="439a9-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="439a9-105">En este artículo se trata la configuración del modelo de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="439a9-105">This article covers hosting model configuration.</span></span>

<!-- For future use:

## Blazor WebAssembly

-->

## <a name="blazor-server"></a><span data-ttu-id="439a9-106">Servidor Blazor</span><span class="sxs-lookup"><span data-stu-id="439a9-106">Blazor Server</span></span>

### <a name="integrate-razor-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="439a9-107">Integre componentes de Razor en aplicaciones de Razor Pages y MVC</span><span class="sxs-lookup"><span data-stu-id="439a9-107">Integrate Razor components into Razor Pages and MVC apps</span></span>

#### <a name="use-components-in-pages-and-views"></a><span data-ttu-id="439a9-108">Usar componentes en páginas y vistas</span><span class="sxs-lookup"><span data-stu-id="439a9-108">Use components in pages and views</span></span>

<span data-ttu-id="439a9-109">Una aplicación Razor Pages o MVC existente puede integrar los componentes de Razor en páginas y vistas:</span><span class="sxs-lookup"><span data-stu-id="439a9-109">An existing Razor Pages or MVC app can integrate Razor components into pages and views:</span></span>

1. <span data-ttu-id="439a9-110">En el archivo de diseño de la aplicación ( *_Layout. cshtml*):</span><span class="sxs-lookup"><span data-stu-id="439a9-110">In the app's layout file (*_Layout.cshtml*):</span></span>

   * <span data-ttu-id="439a9-111">Agregue la siguiente etiqueta de `<base>` al elemento `<head>`:</span><span class="sxs-lookup"><span data-stu-id="439a9-111">Add the following `<base>` tag to the `<head>` element:</span></span>

     ```html
     <base href="~/" />
     ```

     <span data-ttu-id="439a9-112">El valor `href` (la *ruta de acceso base*de la aplicación) en el ejemplo anterior supone que la aplicación reside en la ruta de acceso URL raíz (`/`).</span><span class="sxs-lookup"><span data-stu-id="439a9-112">The `href` value (the *app base path*) in the preceding example assumes that the app resides at the root URL path (`/`).</span></span> <span data-ttu-id="439a9-113">Si la aplicación es una aplicación secundaria, siga las instrucciones de la sección *ruta de acceso base* de la aplicación del artículo <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="439a9-113">If the app is a sub-application, follow the guidance in the *App base path* section of the <xref:host-and-deploy/blazor/index#app-base-path> article.</span></span>

     <span data-ttu-id="439a9-114">El archivo *_Layout. cshtml* se encuentra en la carpeta *páginas/compartidas* de una aplicación de Razor pages o en una carpeta de *vistas/compartidas* en una aplicación MVC.</span><span class="sxs-lookup"><span data-stu-id="439a9-114">The *_Layout.cshtml* file is located in the *Pages/Shared* folder in a Razor Pages app or *Views/Shared* folder in an MVC app.</span></span>

   * <span data-ttu-id="439a9-115">Agregue una etiqueta de `<script>` para el script *increíblemente. Server. js* justo antes de la etiqueta de cierre `</body>`:</span><span class="sxs-lookup"><span data-stu-id="439a9-115">Add a `<script>` tag for the *blazor.server.js* script right before of the closing `</body>` tag:</span></span>

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     <span data-ttu-id="439a9-116">El marco de trabajo agrega el script *extraordinaria. Server. js* a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="439a9-116">The framework adds the *blazor.server.js* script to the app.</span></span> <span data-ttu-id="439a9-117">No es necesario agregar manualmente el script a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="439a9-117">There's no need to manually add the script to the app.</span></span>

1. <span data-ttu-id="439a9-118">Agregue un archivo *_Imports. Razor* a la carpeta raíz del proyecto con el siguiente contenido (cambie el último espacio de nombres, `MyAppNamespace`, al espacio de nombres de la aplicación):</span><span class="sxs-lookup"><span data-stu-id="439a9-118">Add an *_Imports.razor* file to the root folder of the project with the following content (change the last namespace, `MyAppNamespace`, to the namespace of the app):</span></span>

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

1. <span data-ttu-id="439a9-119">En `Startup.ConfigureServices`, registre el servicio de servidor de extraordinarias:</span><span class="sxs-lookup"><span data-stu-id="439a9-119">In `Startup.ConfigureServices`, register the Blazor Server service:</span></span>

   ```csharp
   services.AddServerSideBlazor();
   ```

1. <span data-ttu-id="439a9-120">En `Startup.Configure`, agregue el punto de conexión del concentrador de increíble a `app.UseEndpoints`:</span><span class="sxs-lookup"><span data-stu-id="439a9-120">In `Startup.Configure`, add the Blazor Hub endpoint to `app.UseEndpoints`:</span></span>

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. <span data-ttu-id="439a9-121">Integre los componentes en cualquier página o vista.</span><span class="sxs-lookup"><span data-stu-id="439a9-121">Integrate components into any page or view.</span></span> <span data-ttu-id="439a9-122">Para más información, consulte la sección *integración de componentes en Razor pages y aplicaciones MVC* del artículo <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="439a9-122">For more information, see the *Integrate components into Razor Pages and MVC apps* section of the <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps> article.</span></span>

#### <a name="use-routable-components-in-a-razor-pages-app"></a><span data-ttu-id="439a9-123">Uso de componentes enrutables en una aplicación Razor Pages</span><span class="sxs-lookup"><span data-stu-id="439a9-123">Use routable components in a Razor Pages app</span></span>

<span data-ttu-id="439a9-124">Para admitir componentes de Razor enrutables en Razor Pages aplicaciones:</span><span class="sxs-lookup"><span data-stu-id="439a9-124">To support routable Razor components in Razor Pages apps:</span></span>

1. <span data-ttu-id="439a9-125">Siga las instrucciones de la sección [uso de componentes en páginas y vistas](#use-components-in-pages-and-views) .</span><span class="sxs-lookup"><span data-stu-id="439a9-125">Follow the guidance in the [Use components in pages and views](#use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="439a9-126">Agregue un archivo *app. Razor* a la raíz del proyecto con el siguiente contenido:</span><span class="sxs-lookup"><span data-stu-id="439a9-126">Add an *App.razor* file to the root of the project with the following content:</span></span>

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

1. <span data-ttu-id="439a9-127">Agregue un archivo *_Host. cshtml* a la carpeta *pages* con el siguiente contenido:</span><span class="sxs-lookup"><span data-stu-id="439a9-127">Add a *_Host.cshtml* file to the *Pages* folder with the following content:</span></span>

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="439a9-128">Los componentes de usan el archivo Shared *_Layout. cshtml* para su diseño.</span><span class="sxs-lookup"><span data-stu-id="439a9-128">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="439a9-129">Agregue una ruta de prioridad baja para la página *_Host. cshtml* a la configuración del punto de conexión en `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="439a9-129">Add a low-priority route for the *_Host.cshtml* page to endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. <span data-ttu-id="439a9-130">Agregue componentes enrutables a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="439a9-130">Add routable components to the app.</span></span> <span data-ttu-id="439a9-131">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="439a9-131">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="439a9-132">Al usar una carpeta personalizada para contener los componentes de la aplicación, agregue el espacio de nombres que representa la carpeta al archivo *pages/_ViewImports. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="439a9-132">When using a custom folder to hold the app's components, add the namespace representing the folder to the *Pages/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="439a9-133">Para más información, consulte <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="439a9-133">For more information, see <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span></span>

#### <a name="use-routable-components-in-an-mvc-app"></a><span data-ttu-id="439a9-134">Uso de componentes enrutables en una aplicación MVC</span><span class="sxs-lookup"><span data-stu-id="439a9-134">Use routable components in an MVC app</span></span>

<span data-ttu-id="439a9-135">Para admitir componentes de Razor enrutables en aplicaciones MVC:</span><span class="sxs-lookup"><span data-stu-id="439a9-135">To support routable Razor components in MVC apps:</span></span>

1. <span data-ttu-id="439a9-136">Siga las instrucciones de la sección [uso de componentes en páginas y vistas](#use-components-in-pages-and-views) .</span><span class="sxs-lookup"><span data-stu-id="439a9-136">Follow the guidance in the [Use components in pages and views](#use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="439a9-137">Agregue un archivo *app. Razor* a la raíz del proyecto con el siguiente contenido:</span><span class="sxs-lookup"><span data-stu-id="439a9-137">Add an *App.razor* file to the root of the project with the following content:</span></span>

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

1. <span data-ttu-id="439a9-138">Agregue un archivo *_Host. cshtml* a la carpeta *views/Home* con el siguiente contenido:</span><span class="sxs-lookup"><span data-stu-id="439a9-138">Add a *_Host.cshtml* file to the *Views/Home* folder with the following content:</span></span>

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="439a9-139">Los componentes de usan el archivo Shared *_Layout. cshtml* para su diseño.</span><span class="sxs-lookup"><span data-stu-id="439a9-139">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="439a9-140">Agregue una acción al controlador Home:</span><span class="sxs-lookup"><span data-stu-id="439a9-140">Add an action to the Home controller:</span></span>

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. <span data-ttu-id="439a9-141">Agregue una ruta de prioridad baja para la acción de controlador que devuelve la vista *_Host. cshtml* a la configuración del punto de conexión en `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="439a9-141">Add a low-priority route for the controller action that returns the *_Host.cshtml* view to the endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. <span data-ttu-id="439a9-142">Cree una carpeta de *páginas* y agregue componentes enrutables a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="439a9-142">Create a *Pages* folder and add routable components to the app.</span></span> <span data-ttu-id="439a9-143">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="439a9-143">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="439a9-144">Al usar una carpeta personalizada para contener los componentes de la aplicación, agregue el espacio de nombres que representa la carpeta al archivo *views/_ViewImports. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="439a9-144">When using a custom folder to hold the app's components, add the namespace representing the folder to the *Views/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="439a9-145">Para más información, consulte <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="439a9-145">For more information, see <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span></span>

### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="439a9-146">Reflejar el estado de conexión en la interfaz de usuario</span><span class="sxs-lookup"><span data-stu-id="439a9-146">Reflect the connection state in the UI</span></span>

<span data-ttu-id="439a9-147">Cuando el cliente detecta que se ha perdido la conexión, se muestra al usuario una interfaz de usuario predeterminada mientras el cliente intenta volver a conectarse.</span><span class="sxs-lookup"><span data-stu-id="439a9-147">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="439a9-148">Si se produce un error en la reconexión, se proporciona al usuario la opción de volver a intentarlo.</span><span class="sxs-lookup"><span data-stu-id="439a9-148">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="439a9-149">Para personalizar la interfaz de usuario, defina un elemento con un `id` de `components-reconnect-modal` en la `<body>` de la página de Razor de *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="439a9-149">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="439a9-150">En la tabla siguiente se describen las clases CSS aplicadas al elemento `components-reconnect-modal`.</span><span class="sxs-lookup"><span data-stu-id="439a9-150">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="439a9-151">Clase CSS</span><span class="sxs-lookup"><span data-stu-id="439a9-151">CSS class</span></span>                       | <span data-ttu-id="439a9-152">Indica&hellip;</span><span class="sxs-lookup"><span data-stu-id="439a9-152">Indicates&hellip;</span></span> |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | <span data-ttu-id="439a9-153">Una conexión perdida.</span><span class="sxs-lookup"><span data-stu-id="439a9-153">A lost connection.</span></span> <span data-ttu-id="439a9-154">El cliente está intentando volver a conectarse.</span><span class="sxs-lookup"><span data-stu-id="439a9-154">The client is attempting to reconnect.</span></span> <span data-ttu-id="439a9-155">Muestre el modal.</span><span class="sxs-lookup"><span data-stu-id="439a9-155">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="439a9-156">Una conexión activa se restablece en el servidor.</span><span class="sxs-lookup"><span data-stu-id="439a9-156">An active connection is re-established to the server.</span></span> <span data-ttu-id="439a9-157">Oculte el modal.</span><span class="sxs-lookup"><span data-stu-id="439a9-157">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="439a9-158">Error de reconexión, probablemente debido a un error de red.</span><span class="sxs-lookup"><span data-stu-id="439a9-158">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="439a9-159">Para intentar la reconexión, llame a `window.Blazor.reconnect()`.</span><span class="sxs-lookup"><span data-stu-id="439a9-159">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="439a9-160">Reconexión rechazada.</span><span class="sxs-lookup"><span data-stu-id="439a9-160">Reconnection rejected.</span></span> <span data-ttu-id="439a9-161">Se alcanzó el servidor pero se rechazó la conexión y se pierde el estado del usuario en el servidor.</span><span class="sxs-lookup"><span data-stu-id="439a9-161">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="439a9-162">Para volver a cargar la aplicación, llame a `location.reload()`.</span><span class="sxs-lookup"><span data-stu-id="439a9-162">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="439a9-163">Este estado de conexión puede producirse cuando:</span><span class="sxs-lookup"><span data-stu-id="439a9-163">This connection state may result when:</span></span><ul><li><span data-ttu-id="439a9-164">Se produce un bloqueo en el circuito del servidor.</span><span class="sxs-lookup"><span data-stu-id="439a9-164">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="439a9-165">El cliente se desconecta el tiempo suficiente para que el servidor Quite el estado del usuario.</span><span class="sxs-lookup"><span data-stu-id="439a9-165">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="439a9-166">Se eliminan las instancias de los componentes con los que el usuario está interactuando.</span><span class="sxs-lookup"><span data-stu-id="439a9-166">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="439a9-167">El servidor se reinicia o se recicla el proceso de trabajo de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="439a9-167">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="render-mode"></a><span data-ttu-id="439a9-168">Modo de representación</span><span class="sxs-lookup"><span data-stu-id="439a9-168">Render mode</span></span>

<span data-ttu-id="439a9-169">Las aplicaciones de servidor increíbles se configuran de forma predeterminada para prerepresentar la interfaz de usuario en el servidor antes de que se establezca la conexión de cliente con el servidor.</span><span class="sxs-lookup"><span data-stu-id="439a9-169">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="439a9-170">Esto se configura en la página de Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="439a9-170">This is set up in the *_Host.cshtml* Razor page:</span></span>

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="439a9-171">`RenderMode` configura si el componente:</span><span class="sxs-lookup"><span data-stu-id="439a9-171">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="439a9-172">Se representa en la página.</span><span class="sxs-lookup"><span data-stu-id="439a9-172">Is prerendered into the page.</span></span>
* <span data-ttu-id="439a9-173">Se representa como HTML estático en la página o si incluye la información necesaria para iniciar una aplicación extraordinaria desde el agente de usuario.</span><span class="sxs-lookup"><span data-stu-id="439a9-173">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="439a9-174">Descripción</span><span class="sxs-lookup"><span data-stu-id="439a9-174">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="439a9-175">Representa el componente en código HTML estático e incluye un marcador para una aplicación de Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="439a9-175">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="439a9-176">Cuando se inicia el agente de usuario, este marcador se usa para arrancar una aplicación Blazor.</span><span class="sxs-lookup"><span data-stu-id="439a9-176">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="439a9-177">Representa un marcador para una aplicación de Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="439a9-177">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="439a9-178">La salida del componente no está incluida.</span><span class="sxs-lookup"><span data-stu-id="439a9-178">Output from the component isn't included.</span></span> <span data-ttu-id="439a9-179">Cuando se inicia el agente de usuario, este marcador se usa para arrancar una aplicación Blazor.</span><span class="sxs-lookup"><span data-stu-id="439a9-179">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="439a9-180">Representa el componente en HTML estático.</span><span class="sxs-lookup"><span data-stu-id="439a9-180">Renders the component into static HTML.</span></span> |

<span data-ttu-id="439a9-181">No se admite la representación de componentes de servidor desde una página HTML estática.</span><span class="sxs-lookup"><span data-stu-id="439a9-181">Rendering server components from a static HTML page isn't supported.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="439a9-182">Representación de componentes interactivos con estado desde vistas y páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="439a9-182">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="439a9-183">Los componentes interactivos con estado se pueden agregar a una página o vista de Razor.</span><span class="sxs-lookup"><span data-stu-id="439a9-183">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="439a9-184">Cuando se representa la página o la vista:</span><span class="sxs-lookup"><span data-stu-id="439a9-184">When the page or view renders:</span></span>

* <span data-ttu-id="439a9-185">El componente se representará con la página o la vista.</span><span class="sxs-lookup"><span data-stu-id="439a9-185">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="439a9-186">Se pierde el estado inicial del componente usado para la representación previa.</span><span class="sxs-lookup"><span data-stu-id="439a9-186">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="439a9-187">Cuando se establece la conexión SignalR, se crea un nuevo estado del componente.</span><span class="sxs-lookup"><span data-stu-id="439a9-187">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="439a9-188">La siguiente página de Razor representa un componente de `Counter`:</span><span class="sxs-lookup"><span data-stu-id="439a9-188">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="439a9-189">Representación de componentes no interactivos desde páginas y vistas de Razor</span><span class="sxs-lookup"><span data-stu-id="439a9-189">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="439a9-190">En la siguiente página de Razor, el componente de `Counter` se representa estáticamente con un valor inicial que se especifica mediante un formulario:</span><span class="sxs-lookup"><span data-stu-id="439a9-190">In the following Razor page, the `Counter` component is statically rendered with an initial value that's specified using a form:</span></span>

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

<span data-ttu-id="439a9-191">Como `MyComponent` se representa estáticamente, el componente no puede ser interactivo.</span><span class="sxs-lookup"><span data-stu-id="439a9-191">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="439a9-192">Configuración del cliente de SignalR para aplicaciones de Blazor Server</span><span class="sxs-lookup"><span data-stu-id="439a9-192">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="439a9-193">A veces, debe configurar el cliente de SignalR que usan las aplicaciones de Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="439a9-193">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="439a9-194">Por ejemplo, puede que desee configurar el registro en el SignalR cliente para diagnosticar un problema de conexión.</span><span class="sxs-lookup"><span data-stu-id="439a9-194">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="439a9-195">Para configurar el cliente de SignalR en el archivo *pages/_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="439a9-195">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="439a9-196">Agregue un atributo `autostart="false"` a la etiqueta de `<script>` para el script de `blazor.server.js`.</span><span class="sxs-lookup"><span data-stu-id="439a9-196">Add an `autostart="false"` attribute to the `<script>` tag for the `blazor.server.js` script.</span></span>
* <span data-ttu-id="439a9-197">Llame a `Blazor.start` y pase un objeto de configuración que especifique el generador de SignalR.</span><span class="sxs-lookup"><span data-stu-id="439a9-197">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

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
