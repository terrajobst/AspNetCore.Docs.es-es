---
title: Vínculo del explorador en ASP.NET Core
author: ncarandini
description: Explica cómo el vínculo del explorador es una característica de Visual Studio que vincula el entorno de desarrollo con uno o más exploradores Web.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 01/09/2020
no-loc:
- SignalR
uid: client-side/using-browserlink
ms.openlocfilehash: 19cc3c2ed91bd9e05df3c036123c78ecbf81fcc0
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828275"
---
# <a name="browser-link-in-aspnet-core"></a><span data-ttu-id="5cc13-103">Vínculo del explorador en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5cc13-103">Browser Link in ASP.NET Core</span></span>

<span data-ttu-id="5cc13-104">Por [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson)y [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="5cc13-104">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="5cc13-105">El vínculo del explorador es una característica de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5cc13-105">Browser Link is a Visual Studio feature.</span></span> <span data-ttu-id="5cc13-106">Crea un canal de comunicación entre el entorno de desarrollo y uno o varios exploradores Web.</span><span class="sxs-lookup"><span data-stu-id="5cc13-106">It creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="5cc13-107">Puede usar el vínculo del explorador para actualizar la aplicación web en varios exploradores a la vez, lo que resulta útil para las pruebas entre exploradores.</span><span class="sxs-lookup"><span data-stu-id="5cc13-107">You can use Browser Link to refresh your web app in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="5cc13-108">Configuración del vínculo del explorador</span><span class="sxs-lookup"><span data-stu-id="5cc13-108">Browser Link setup</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5cc13-109">Agregue el paquete [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) al proyecto.</span><span class="sxs-lookup"><span data-stu-id="5cc13-109">Add the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package to your project.</span></span> <span data-ttu-id="5cc13-110">Para los proyectos de ASP.NET Core Razor Pages o MVC, habilite también la compilación en tiempo de ejecución de los archivos Razor ( *. cshtml*) como se describe en <xref:mvc/views/view-compilation>.</span><span class="sxs-lookup"><span data-stu-id="5cc13-110">For ASP.NET Core Razor Pages or MVC projects, also enable runtime compilation of Razor (*.cshtml*) files as described in <xref:mvc/views/view-compilation>.</span></span> <span data-ttu-id="5cc13-111">Sintaxis Razor cambios solo se aplican cuando se ha habilitado la compilación en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="5cc13-111">Razor syntax changes are applied only when runtime compilation has been enabled.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="5cc13-112">Al convertir un proyecto ASP.NET Core 2,0 en ASP.NET Core 2,1 y pasar al [metapaquete Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), instale el paquete [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) para la funcionalidad de vínculo de explorador.</span><span class="sxs-lookup"><span data-stu-id="5cc13-112">When converting an ASP.NET Core 2.0 project to ASP.NET Core 2.1 and transitioning to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), install the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package for Browser Link functionality.</span></span> <span data-ttu-id="5cc13-113">De forma predeterminada, las plantillas de proyecto de ASP.NET Core 2,1 usan el metapaquete `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="5cc13-113">The ASP.NET Core 2.1 project templates use the `Microsoft.AspNetCore.App` metapackage by default.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="5cc13-114">Las plantillas de proyecto **aplicación web**ASP.net Core 2,0, **vacía**y **API Web** usan el [metapaquete Microsoft. AspNetCore. All](xref:fundamentals/metapackage), que contiene una referencia de paquete para [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span><span class="sxs-lookup"><span data-stu-id="5cc13-114">The ASP.NET Core 2.0 **Web Application**, **Empty**, and **Web API** project templates use the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), which contains a package reference for [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span></span> <span data-ttu-id="5cc13-115">Por lo tanto, el uso del metapaquete `Microsoft.AspNetCore.All` no requiere ninguna acción adicional para que el vínculo del explorador esté disponible para su uso.</span><span class="sxs-lookup"><span data-stu-id="5cc13-115">Therefore, using the `Microsoft.AspNetCore.All` metapackage requires no further action to make Browser Link available for use.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="5cc13-116">La plantilla de proyecto de **aplicación web** de ASP.net Core 1. x tiene una referencia de paquete para el paquete [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) .</span><span class="sxs-lookup"><span data-stu-id="5cc13-116">The ASP.NET Core 1.x **Web Application** project template has a package reference for the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package.</span></span> <span data-ttu-id="5cc13-117">Otros tipos de proyectos requieren que agregue una referencia de paquete a `Microsoft.VisualStudio.Web.BrowserLink`.</span><span class="sxs-lookup"><span data-stu-id="5cc13-117">Other project types require you to add a package reference to `Microsoft.VisualStudio.Web.BrowserLink`.</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="5cc13-118">Configuración de</span><span class="sxs-lookup"><span data-stu-id="5cc13-118">Configuration</span></span>

<span data-ttu-id="5cc13-119">Llame a `UseBrowserLink` en el método `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="5cc13-119">Call `UseBrowserLink` in the `Startup.Configure` method:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="5cc13-120">Normalmente, la llamada `UseBrowserLink` se coloca dentro de un bloque `if` que solo habilita el vínculo del explorador en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="5cc13-120">The `UseBrowserLink` call is typically placed inside an `if` block that only enables Browser Link in the Development environment.</span></span> <span data-ttu-id="5cc13-121">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="5cc13-121">For example:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

<span data-ttu-id="5cc13-122">Para obtener más información, vea <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="5cc13-122">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="5cc13-123">Cómo usar el vínculo del explorador</span><span class="sxs-lookup"><span data-stu-id="5cc13-123">How to use Browser Link</span></span>

<span data-ttu-id="5cc13-124">Cuando tiene un proyecto de ASP.NET Core abierto, Visual Studio muestra el control de la barra de herramientas de vínculos del explorador junto al control de barra de herramientas de **destino de depuración** :</span><span class="sxs-lookup"><span data-stu-id="5cc13-124">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![Menú desplegable de vínculos del explorador](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="5cc13-126">En el control de barra de herramientas de vínculo del explorador, puede:</span><span class="sxs-lookup"><span data-stu-id="5cc13-126">From the Browser Link toolbar control, you can:</span></span>

* <span data-ttu-id="5cc13-127">Actualice la aplicación web en varios exploradores a la vez.</span><span class="sxs-lookup"><span data-stu-id="5cc13-127">Refresh the web app in several browsers at once.</span></span>
* <span data-ttu-id="5cc13-128">Abra el **Panel vínculo del explorador**.</span><span class="sxs-lookup"><span data-stu-id="5cc13-128">Open the **Browser Link Dashboard**.</span></span>
* <span data-ttu-id="5cc13-129">Habilitar o deshabilitar el **vínculo del explorador**.</span><span class="sxs-lookup"><span data-stu-id="5cc13-129">Enable or disable **Browser Link**.</span></span> <span data-ttu-id="5cc13-130">Nota: el vínculo del explorador está deshabilitado de forma predeterminada en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5cc13-130">Note: Browser Link is disabled by default in Visual Studio.</span></span>
* <span data-ttu-id="5cc13-131">Habilitar o deshabilitar [la sincronización automática de CSS](#enable-or-disable-css-auto-sync).</span><span class="sxs-lookup"><span data-stu-id="5cc13-131">Enable or disable [CSS Auto-Sync](#enable-or-disable-css-auto-sync).</span></span>

## <a name="refresh-the-web-app-in-several-browsers-at-once"></a><span data-ttu-id="5cc13-132">Actualización de la aplicación web en varios exploradores a la vez</span><span class="sxs-lookup"><span data-stu-id="5cc13-132">Refresh the web app in several browsers at once</span></span>

<span data-ttu-id="5cc13-133">Para elegir un solo explorador Web para iniciar al iniciar el proyecto, use el menú desplegable del control de barra de herramientas de **destino de depuración** :</span><span class="sxs-lookup"><span data-stu-id="5cc13-133">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![Menú desplegable F5](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="5cc13-135">Para abrir varios exploradores a la vez, elija **examinar con...** en la lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="5cc13-135">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span> <span data-ttu-id="5cc13-136">Mantenga presionada la tecla <kbd>Ctrl</kbd> para seleccionar los exploradores que desee y, a continuación, haga clic en **examinar**:</span><span class="sxs-lookup"><span data-stu-id="5cc13-136">Hold down the <kbd>Ctrl</kbd> key to select the browsers you want, and then click **Browse**:</span></span>

![Abrir muchos exploradores a la vez](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="5cc13-138">En la captura de pantalla siguiente se muestra Visual Studio con la vista de índice abierta y dos exploradores abiertos:</span><span class="sxs-lookup"><span data-stu-id="5cc13-138">The following screenshot shows Visual Studio with the Index view open and two open browsers:</span></span>

![Ejemplo de sincronización con dos exploradores](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="5cc13-140">Mantenga el mouse sobre el control de la barra de herramientas de vínculos del explorador para ver los exploradores que están conectados al proyecto:</span><span class="sxs-lookup"><span data-stu-id="5cc13-140">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![Sugerencia de desplazamiento](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="5cc13-142">Cambiar la vista de índice y todos los exploradores conectados se actualizan al hacer clic en el botón actualizar vínculo del explorador:</span><span class="sxs-lookup"><span data-stu-id="5cc13-142">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![exploradores: sincronizar con cambios](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="5cc13-144">El vínculo del explorador también funciona con los exploradores que se inician desde fuera de Visual Studio y se navega a la dirección URL de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5cc13-144">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the app URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="5cc13-145">Panel de vínculos del explorador</span><span class="sxs-lookup"><span data-stu-id="5cc13-145">The Browser Link Dashboard</span></span>

<span data-ttu-id="5cc13-146">Abra la ventana **Panel de vínculos del explorador** en el menú desplegable del vínculo del explorador para administrar la conexión con exploradores abiertos:</span><span class="sxs-lookup"><span data-stu-id="5cc13-146">Open the **Browser Link Dashboard** window from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![open-browserslink-dashboard](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="5cc13-148">Si no hay ningún explorador conectado, puede iniciar una sesión que no sea de depuración seleccionando el vínculo **ver en el explorador** :</span><span class="sxs-lookup"><span data-stu-id="5cc13-148">If no browser is connected, you can start a non-debugging session by selecting the **View in Browser** link:</span></span>

![browserlink-dashboard-no-connections](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="5cc13-150">De lo contrario, los exploradores conectados se muestran con la ruta de acceso a la página que se muestra en cada explorador:</span><span class="sxs-lookup"><span data-stu-id="5cc13-150">Otherwise, the connected browsers are shown with the path to the page that each browser is showing:</span></span>

![browserlink-dashboard-two-connections](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="5cc13-152">También puede hacer clic en un nombre de explorador individual para actualizar solo ese explorador.</span><span class="sxs-lookup"><span data-stu-id="5cc13-152">You can also click on an individual browser name to refresh only that browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="5cc13-153">Habilitar o deshabilitar el vínculo del explorador</span><span class="sxs-lookup"><span data-stu-id="5cc13-153">Enable or disable Browser Link</span></span>

<span data-ttu-id="5cc13-154">Al volver a habilitar el vínculo del explorador después de deshabilitarlo, debe actualizar los exploradores para volver a conectarlos.</span><span class="sxs-lookup"><span data-stu-id="5cc13-154">When you re-enable Browser Link after disabling it, you must refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="5cc13-155">Habilitar o deshabilitar la sincronización automática de CSS</span><span class="sxs-lookup"><span data-stu-id="5cc13-155">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="5cc13-156">Cuando se habilita la sincronización automática de CSS, los exploradores conectados se actualizan automáticamente cuando se realiza cualquier cambio en los archivos CSS.</span><span class="sxs-lookup"><span data-stu-id="5cc13-156">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="5cc13-157">Cómo funciona</span><span class="sxs-lookup"><span data-stu-id="5cc13-157">How it works</span></span>

<span data-ttu-id="5cc13-158">El vínculo del explorador usa [SignalR](xref:signalr/introduction) para crear un canal de comunicación entre Visual Studio y el explorador.</span><span class="sxs-lookup"><span data-stu-id="5cc13-158">Browser Link uses [SignalR](xref:signalr/introduction) to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="5cc13-159">Cuando el vínculo del explorador está habilitado, Visual Studio actúa como un servidor de SignalR al que se pueden conectar varios clientes (exploradores).</span><span class="sxs-lookup"><span data-stu-id="5cc13-159">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="5cc13-160">El vínculo del explorador también registra un componente de middleware en la canalización de solicitudes ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5cc13-160">Browser Link also registers a middleware component in the ASP.NET Core request pipeline.</span></span> <span data-ttu-id="5cc13-161">Este componente inserta referencias de `<script>` especiales en cada solicitud de página del servidor.</span><span class="sxs-lookup"><span data-stu-id="5cc13-161">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="5cc13-162">Puede ver las referencias de script seleccionando **Ver código fuente** en el explorador y desplazándose hasta el final del contenido de la etiqueta `<body>`:</span><span class="sxs-lookup"><span data-stu-id="5cc13-162">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```html
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="5cc13-163">Los archivos de origen no se modifican.</span><span class="sxs-lookup"><span data-stu-id="5cc13-163">Your source files aren't modified.</span></span> <span data-ttu-id="5cc13-164">El componente middleware inserta las referencias de script dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="5cc13-164">The middleware component injects the script references dynamically.</span></span>

<span data-ttu-id="5cc13-165">Dado que el código del explorador es todo JavaScript, funciona en todos los exploradores que SignalR admite sin necesidad de un complemento de explorador.</span><span class="sxs-lookup"><span data-stu-id="5cc13-165">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports without requiring a browser plug-in.</span></span>
