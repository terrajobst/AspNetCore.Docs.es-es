---
title: Integración de componentes Razor de ASP.NET Core en aplicaciones Razor Pages y MVC
author: guardrex
description: Obtenga información sobre los escenarios de enlace de datos para componentes y elementos DOM en aplicaciones de Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/17/2020
no-loc:
- Blazor
- SignalR
uid: blazor/integrate-components
ms.openlocfilehash: cf6056e0985d5433bddecac8dd183ca3f4c2af5b
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80218939"
---
# <a name="integrate-aspnet-core-razor-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="57782-103">Integración de componentes Razor de ASP.NET Core en aplicaciones Razor Pages y MVC</span><span class="sxs-lookup"><span data-stu-id="57782-103">Integrate ASP.NET Core Razor components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="57782-104">Por [Luke Latham](https://github.com/guardrex) y [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="57782-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="57782-105">Los componentes Razor se pueden integrar en aplicaciones de Razor Pages y MVC.</span><span class="sxs-lookup"><span data-stu-id="57782-105">Razor components can be integrated into Razor Pages and MVC apps.</span></span> <span data-ttu-id="57782-106">Cuando se representa la página o la vista, los componentes se pueden representar previamente al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="57782-106">When the page or view is rendered, components can be prerendered at the same time.</span></span>

## <a name="prepare-the-app-to-use-components-in-pages-and-views"></a><span data-ttu-id="57782-107">Preparación de la aplicación para usar componentes en páginas y vistas</span><span class="sxs-lookup"><span data-stu-id="57782-107">Prepare the app to use components in pages and views</span></span>

<span data-ttu-id="57782-108">Una aplicación Razor Pages o MVC existente puede integrar componentes Razor en páginas y vistas:</span><span class="sxs-lookup"><span data-stu-id="57782-108">An existing Razor Pages or MVC app can integrate Razor components into pages and views:</span></span>

1. <span data-ttu-id="57782-109">En el archivo de diseño de la aplicación ( *_Layout.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="57782-109">In the app's layout file (*_Layout.cshtml*):</span></span>

   * <span data-ttu-id="57782-110">Agregue la etiqueta `<base>` siguiente al elemento `<head>`:</span><span class="sxs-lookup"><span data-stu-id="57782-110">Add the following `<base>` tag to the `<head>` element:</span></span>

     ```html
     <base href="~/" />
     ```

     <span data-ttu-id="57782-111">El valor `href` (la *ruta de acceso base de la aplicación*) del ejemplo anterior da por hecho que la aplicación reside en la ruta de acceso URL raíz (`/`).</span><span class="sxs-lookup"><span data-stu-id="57782-111">The `href` value (the *app base path*) in the preceding example assumes that the app resides at the root URL path (`/`).</span></span> <span data-ttu-id="57782-112">Si la aplicación es una subaplicación, siga las instrucciones de la sección *Ruta de acceso base de la aplicación* del artículo <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="57782-112">If the app is a sub-application, follow the guidance in the *App base path* section of the <xref:host-and-deploy/blazor/index#app-base-path> article.</span></span>

     <span data-ttu-id="57782-113">El archivo *_Layout.cshtml* se encuentra en la carpeta *Pages/Shared* en una aplicación Razor Pages o en la carpeta *Views/Shared* en una aplicación MVC.</span><span class="sxs-lookup"><span data-stu-id="57782-113">The *_Layout.cshtml* file is located in the *Pages/Shared* folder in a Razor Pages app or *Views/Shared* folder in an MVC app.</span></span>

   * <span data-ttu-id="57782-114">Agregue una etiqueta `<script>` para el script *blazor.server.js* inmediatamente antes de la etiqueta `</body>` de cierre:</span><span class="sxs-lookup"><span data-stu-id="57782-114">Add a `<script>` tag for the *blazor.server.js* script immediately before of the closing `</body>` tag:</span></span>

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     <span data-ttu-id="57782-115">El marco agrega el script *blazor.server.js* a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="57782-115">The framework adds the *blazor.server.js* script to the app.</span></span> <span data-ttu-id="57782-116">No es necesario agregar manualmente el script a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="57782-116">There's no need to manually add the script to the app.</span></span>

1. <span data-ttu-id="57782-117">Agregue un archivo *_Imports.razor* a la carpeta raíz del proyecto con el contenido siguiente (cambie el último espacio de nombres, `MyAppNamespace`, al espacio de nombres de la aplicación):</span><span class="sxs-lookup"><span data-stu-id="57782-117">Add an *_Imports.razor* file to the root folder of the project with the following content (change the last namespace, `MyAppNamespace`, to the namespace of the app):</span></span>

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

1. <span data-ttu-id="57782-118">En `Startup.ConfigureServices`, registre el servicio Blazor Server:</span><span class="sxs-lookup"><span data-stu-id="57782-118">In `Startup.ConfigureServices`, register the Blazor Server service:</span></span>

   ```csharp
   services.AddServerSideBlazor();
   ```

1. <span data-ttu-id="57782-119">En `Startup.Configure`, agregue el punto de conexión de Blazor Hub a `app.UseEndpoints`:</span><span class="sxs-lookup"><span data-stu-id="57782-119">In `Startup.Configure`, add the Blazor Hub endpoint to `app.UseEndpoints`:</span></span>

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. <span data-ttu-id="57782-120">Integre los componentes en cualquier página o vista.</span><span class="sxs-lookup"><span data-stu-id="57782-120">Integrate components into any page or view.</span></span> <span data-ttu-id="57782-121">Para obtener más información, vea la sección [Representación de componentes a partir de una página o vista](#render-components-from-a-page-or-view).</span><span class="sxs-lookup"><span data-stu-id="57782-121">For more information, see the [Render components from a page or view](#render-components-from-a-page-or-view) section.</span></span>

## <a name="use-routable-components-in-a-razor-pages-app"></a><span data-ttu-id="57782-122">Uso de componentes enrutables en una aplicación Razor Pages</span><span class="sxs-lookup"><span data-stu-id="57782-122">Use routable components in a Razor Pages app</span></span>

<span data-ttu-id="57782-123">*Esta sección pertenece a la incorporación de componentes que se pueden enrutar directamente desde las solicitudes del usuario.*</span><span class="sxs-lookup"><span data-stu-id="57782-123">*This section pertains to adding components that are directly routable from user requests.*</span></span>

<span data-ttu-id="57782-124">Para admitir componentes Razor enrutables en aplicaciones Razor Pages, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="57782-124">To support routable Razor components in Razor Pages apps:</span></span>

1. <span data-ttu-id="57782-125">Siga las instrucciones de la sección [Preparación de la aplicación para usar componentes en páginas y vistas](#prepare-the-app-to-use-components-in-pages-and-views).</span><span class="sxs-lookup"><span data-stu-id="57782-125">Follow the guidance in the [Prepare the app to use components in pages and views](#prepare-the-app-to-use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="57782-126">Agregue un archivo *App.razor* a la raíz del proyecto con el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="57782-126">Add an *App.razor* file to the project root with the following content:</span></span>

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

1. <span data-ttu-id="57782-127">Agregue un archivo *_Host.cshtml* a la carpeta *Pages* con el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="57782-127">Add a *_Host.cshtml* file to the *Pages* folder with the following content:</span></span>

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="57782-128">Los componentes usan el archivo compartido *_Layout.cshtml* para su diseño.</span><span class="sxs-lookup"><span data-stu-id="57782-128">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="57782-129">Agregue una ruta de prioridad baja para la página *_Host.cshtml* a la configuración del punto de conexión en `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="57782-129">Add a low-priority route for the *_Host.cshtml* page to endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. <span data-ttu-id="57782-130">Agregue componentes enrutables a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="57782-130">Add routable components to the app.</span></span> <span data-ttu-id="57782-131">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="57782-131">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="57782-132">Para obtener más información sobre los espacios de nombres, vea la sección [Espacios de nombres de componentes](#component-namespaces).</span><span class="sxs-lookup"><span data-stu-id="57782-132">For more information on namespaces, see the [Component namespaces](#component-namespaces) section.</span></span>

## <a name="use-routable-components-in-an-mvc-app"></a><span data-ttu-id="57782-133">Uso de componentes enrutables en una aplicación MVC</span><span class="sxs-lookup"><span data-stu-id="57782-133">Use routable components in an MVC app</span></span>

<span data-ttu-id="57782-134">*Esta sección pertenece a la incorporación de componentes que se pueden enrutar directamente desde las solicitudes del usuario.*</span><span class="sxs-lookup"><span data-stu-id="57782-134">*This section pertains to adding components that are directly routable from user requests.*</span></span>

<span data-ttu-id="57782-135">Para admitir componentes Razor enrutables en aplicaciones MVC, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="57782-135">To support routable Razor components in MVC apps:</span></span>

1. <span data-ttu-id="57782-136">Siga las instrucciones de la sección [Preparación de la aplicación para usar componentes en páginas y vistas](#prepare-the-app-to-use-components-in-pages-and-views).</span><span class="sxs-lookup"><span data-stu-id="57782-136">Follow the guidance in the [Prepare the app to use components in pages and views](#prepare-the-app-to-use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="57782-137">Agregue un archivo *App.razor* a la raíz del proyecto con el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="57782-137">Add an *App.razor* file to the root of the project with the following content:</span></span>

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

1. <span data-ttu-id="57782-138">Agregue un archivo *_Host.cshtml* a la carpeta *Views/Home* con el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="57782-138">Add a *_Host.cshtml* file to the *Views/Home* folder with the following content:</span></span>

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="57782-139">Los componentes usan el archivo compartido *_Layout.cshtml* para su diseño.</span><span class="sxs-lookup"><span data-stu-id="57782-139">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="57782-140">Agregue una acción al controlador de inicio:</span><span class="sxs-lookup"><span data-stu-id="57782-140">Add an action to the Home controller:</span></span>

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. <span data-ttu-id="57782-141">Agregue una ruta de prioridad baja para la acción de controlador que devuelve la vista *_Host.cshtml* a la configuración del punto de conexión en `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="57782-141">Add a low-priority route for the controller action that returns the *_Host.cshtml* view to the endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. <span data-ttu-id="57782-142">Cree una carpeta *Pages* y agregue componentes enrutables a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="57782-142">Create a *Pages* folder and add routable components to the app.</span></span> <span data-ttu-id="57782-143">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="57782-143">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="57782-144">Para obtener más información sobre los espacios de nombres, vea la sección [Espacios de nombres de componentes](#component-namespaces).</span><span class="sxs-lookup"><span data-stu-id="57782-144">For more information on namespaces, see the [Component namespaces](#component-namespaces) section.</span></span>

## <a name="component-namespaces"></a><span data-ttu-id="57782-145">Espacios de nombres de componentes</span><span class="sxs-lookup"><span data-stu-id="57782-145">Component namespaces</span></span>

<span data-ttu-id="57782-146">Al usar una carpeta personalizada para contener los componentes de la aplicación, agregue el espacio de nombres que representa la carpeta a la página o vista, o al archivo *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="57782-146">When using a custom folder to hold the app's components, add the namespace representing the folder to either the page/view or to the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="57782-147">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="57782-147">In the following example:</span></span>

* <span data-ttu-id="57782-148">Cambie `MyAppNamespace` en el espacio de nombres de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="57782-148">Change `MyAppNamespace` to the app's namespace.</span></span>
* <span data-ttu-id="57782-149">Si no se usa una carpeta denominada *Components* para contener los componentes, cambie `Components` en la carpeta donde estos se encuentren.</span><span class="sxs-lookup"><span data-stu-id="57782-149">If a folder named *Components* isn't used to hold the components, change `Components` to the folder where the components reside.</span></span>

```cshtml
@using MyAppNamespace.Components
```

<span data-ttu-id="57782-150">El archivo *_ViewImports.cshtml* se encuentra en la carpeta *Pages* de una aplicación Razor Pages o en la carpeta *Views* de una aplicación MVC.</span><span class="sxs-lookup"><span data-stu-id="57782-150">The *_ViewImports.cshtml* file is located in the *Pages* folder of a Razor Pages app or the *Views* folder of an MVC app.</span></span>

<span data-ttu-id="57782-151">Para obtener más información, consulta <xref:blazor/components#import-components>.</span><span class="sxs-lookup"><span data-stu-id="57782-151">For more information, see <xref:blazor/components#import-components>.</span></span>

## <a name="render-components-from-a-page-or-view"></a><span data-ttu-id="57782-152">Representación de componentes a partir de una página o vista</span><span class="sxs-lookup"><span data-stu-id="57782-152">Render components from a page or view</span></span>

<span data-ttu-id="57782-153">*Esta sección pertenece a la adición de componentes a páginas o vistas, donde los componentes no son enrutables directamente desde las solicitudes del usuario.*</span><span class="sxs-lookup"><span data-stu-id="57782-153">*This section pertains to adding components to pages or views, where the components aren't directly routable from user requests.*</span></span>

<span data-ttu-id="57782-154">Para representar un componente a partir de una página o vista, use el [asistente de etiquetas de componente](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="57782-154">To render a component from a page or view, use the [Component Tag Helper](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper).</span></span>

<span data-ttu-id="57782-155">Para obtener más información sobre cómo se representan los componentes, el estado del componente y el Asistente de etiquetas `Component`, vea los artículos siguientes:</span><span class="sxs-lookup"><span data-stu-id="57782-155">For more information on how components are rendered, component state, and the `Component` Tag Helper, see the following articles:</span></span>

* <xref:blazor/hosting-models>
* <xref:blazor/hosting-model-configuration>
* <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>
