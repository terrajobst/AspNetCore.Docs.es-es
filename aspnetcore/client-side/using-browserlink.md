---
title: Vínculo del explorador en ASP.NET Core
author: ncarandini
description: Explica cómo el vínculo del explorador es una característica de Visual Studio que vincula el entorno de desarrollo con uno o más exploradores Web.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 11/12/2019
no-loc:
- SignalR
uid: client-side/using-browserlink
ms.openlocfilehash: b21b698d49e72b559cd9cd3753c48a38c99db24d
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2019
ms.locfileid: "73962782"
---
# <a name="browser-link-in-aspnet-core"></a><span data-ttu-id="3c32d-103">Vínculo del explorador en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3c32d-103">Browser Link in ASP.NET Core</span></span>

<span data-ttu-id="3c32d-104">Por [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson)y [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="3c32d-104">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="3c32d-105">Vínculo del explorador es una característica de Visual Studio que crea un canal de comunicación entre el entorno de desarrollo y uno o varios exploradores Web.</span><span class="sxs-lookup"><span data-stu-id="3c32d-105">Browser Link is a feature in Visual Studio that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="3c32d-106">Puede usar el vínculo del explorador para actualizar la aplicación web en varios exploradores a la vez, lo que resulta útil para las pruebas entre exploradores.</span><span class="sxs-lookup"><span data-stu-id="3c32d-106">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="3c32d-107">Configuración del vínculo del explorador</span><span class="sxs-lookup"><span data-stu-id="3c32d-107">Browser Link setup</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="3c32d-108">Al convertir un proyecto ASP.NET Core 2,0 en ASP.NET Core 2,1 y pasar al [metapaquete Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), instale el paquete [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) para la funcionalidad BrowserLink.</span><span class="sxs-lookup"><span data-stu-id="3c32d-108">When converting an ASP.NET Core 2.0 project to ASP.NET Core 2.1 and transitioning to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), install the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package for BrowserLink functionality.</span></span> <span data-ttu-id="3c32d-109">De forma predeterminada, las plantillas de proyecto de ASP.NET Core 2,1 usan el metapaquete `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="3c32d-109">The ASP.NET Core 2.1 project templates use the `Microsoft.AspNetCore.App` metapackage by default.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="3c32d-110">Las plantillas de proyecto **aplicación web**ASP.net Core 2,0, **vacía**y **API Web** usan el [metapaquete Microsoft. AspNetCore. All](xref:fundamentals/metapackage), que contiene una referencia de paquete para [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span><span class="sxs-lookup"><span data-stu-id="3c32d-110">The ASP.NET Core 2.0 **Web Application**, **Empty**, and **Web API** project templates use the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), which contains a package reference for [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span></span> <span data-ttu-id="3c32d-111">Por lo tanto, el uso del metapaquete `Microsoft.AspNetCore.All` no requiere ninguna acción adicional para que el vínculo del explorador esté disponible para su uso.</span><span class="sxs-lookup"><span data-stu-id="3c32d-111">Therefore, using the `Microsoft.AspNetCore.All` metapackage requires no further action to make Browser Link available for use.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="3c32d-112">La plantilla de proyecto de **aplicación web** de ASP.net Core 1. x tiene una referencia de paquete para el paquete [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) .</span><span class="sxs-lookup"><span data-stu-id="3c32d-112">The ASP.NET Core 1.x **Web Application** project template has a package reference for the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package.</span></span> <span data-ttu-id="3c32d-113">Los proyectos de plantilla de **API Web** o **vacíos** requieren que se agregue una referencia de paquete a `Microsoft.VisualStudio.Web.BrowserLink`.</span><span class="sxs-lookup"><span data-stu-id="3c32d-113">The **Empty** or **Web API** template projects require you to add a package reference to `Microsoft.VisualStudio.Web.BrowserLink`.</span></span>

<span data-ttu-id="3c32d-114">Dado que se trata de una característica de Visual Studio, la manera más fácil de agregar el paquete a un proyecto de plantilla de **API Web** o **vacío** es abrir la **consola del administrador de paquetes** (**Ver** > otra consola del **Administrador de paquetes**de **Windows** >) y ejecutar el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="3c32d-114">Since this is a Visual Studio feature, the easiest way to add the package to an **Empty** or **Web API** template project is to open the **Package Manager Console** (**View** > **Other Windows** > **Package Manager Console**) and run the following command:</span></span>

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

<span data-ttu-id="3c32d-115">Como alternativa, puede usar el **Administrador de paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="3c32d-115">Alternatively, you can use **NuGet Package Manager**.</span></span> <span data-ttu-id="3c32d-116">Haga clic con el botón derecho en el nombre del proyecto en **Explorador de soluciones** y elija **administrar paquetes NuGet**:</span><span class="sxs-lookup"><span data-stu-id="3c32d-116">Right-click the project name in **Solution Explorer** and choose **Manage NuGet Packages**:</span></span>

![Abrir el administrador de paquetes NuGet](using-browserlink/_static/open-nuget-package-manager.png)

<span data-ttu-id="3c32d-118">Busque e instale el paquete:</span><span class="sxs-lookup"><span data-stu-id="3c32d-118">Find and install the package:</span></span>

![Agregar paquete con el administrador de paquetes NuGet](using-browserlink/_static/add-package-with-nuget-package-manager.png)

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="3c32d-120">Configuración</span><span class="sxs-lookup"><span data-stu-id="3c32d-120">Configuration</span></span>

<span data-ttu-id="3c32d-121">En el método `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="3c32d-121">In the `Startup.Configure` method:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="3c32d-122">Normalmente, el código está dentro de un bloque `if` que solo habilita el vínculo del explorador en el entorno de desarrollo, como se muestra aquí:</span><span class="sxs-lookup"><span data-stu-id="3c32d-122">Usually the code is inside an `if` block that only enables Browser Link in the Development environment, as shown here:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

<span data-ttu-id="3c32d-123">Para obtener más información, consulte [Uso de varios entornos](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="3c32d-123">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="3c32d-124">Cómo usar el vínculo del explorador</span><span class="sxs-lookup"><span data-stu-id="3c32d-124">How to use Browser Link</span></span>

<span data-ttu-id="3c32d-125">Cuando tiene un proyecto de ASP.NET Core abierto, Visual Studio muestra el control de la barra de herramientas de vínculos del explorador junto al control de barra de herramientas de **destino de depuración** :</span><span class="sxs-lookup"><span data-stu-id="3c32d-125">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![Menú desplegable de vínculos del explorador](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="3c32d-127">En el control de barra de herramientas de vínculo del explorador, puede:</span><span class="sxs-lookup"><span data-stu-id="3c32d-127">From the Browser Link toolbar control, you can:</span></span>

* <span data-ttu-id="3c32d-128">Actualice la aplicación web en varios exploradores a la vez.</span><span class="sxs-lookup"><span data-stu-id="3c32d-128">Refresh the web application in several browsers at once.</span></span>
* <span data-ttu-id="3c32d-129">Abra el **Panel vínculo del explorador**.</span><span class="sxs-lookup"><span data-stu-id="3c32d-129">Open the **Browser Link Dashboard**.</span></span>
* <span data-ttu-id="3c32d-130">Habilitar o deshabilitar el **vínculo del explorador**.</span><span class="sxs-lookup"><span data-stu-id="3c32d-130">Enable or disable **Browser Link**.</span></span> <span data-ttu-id="3c32d-131">Nota: el vínculo del explorador está deshabilitado de forma predeterminada en Visual Studio 2017 (15,3).</span><span class="sxs-lookup"><span data-stu-id="3c32d-131">Note: Browser Link is disabled by default in Visual Studio 2017 (15.3).</span></span>
* <span data-ttu-id="3c32d-132">Habilitar o deshabilitar [la sincronización automática de CSS](#enable-or-disable-css-auto-sync).</span><span class="sxs-lookup"><span data-stu-id="3c32d-132">Enable or disable [CSS Auto-Sync](#enable-or-disable-css-auto-sync).</span></span>

> [!NOTE]
> <span data-ttu-id="3c32d-133">Algunos complementos de Visual Studio, especialmente el *paquete de extensiones web 2015* y el *paquete de extensión web 2017*, ofrecen funcionalidad ampliada para el vínculo del explorador, pero algunas de las características adicionales no funcionan con proyectos de ASP.net Core.</span><span class="sxs-lookup"><span data-stu-id="3c32d-133">Some Visual Studio plug-ins, most notably *Web Extension Pack 2015* and *Web Extension Pack 2017*, offer extended functionality for Browser Link, but some of the additional features don't work with ASP.NET Core projects.</span></span>

## <a name="refresh-the-web-app-in-several-browsers-at-once"></a><span data-ttu-id="3c32d-134">Actualización de la aplicación web en varios exploradores a la vez</span><span class="sxs-lookup"><span data-stu-id="3c32d-134">Refresh the web app in several browsers at once</span></span>

<span data-ttu-id="3c32d-135">Para elegir un solo explorador Web para iniciar al iniciar el proyecto, use el menú desplegable del control de barra de herramientas de **destino de depuración** :</span><span class="sxs-lookup"><span data-stu-id="3c32d-135">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![Menú desplegable F5](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="3c32d-137">Para abrir varios exploradores a la vez, elija **examinar con...** en la lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="3c32d-137">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span> <span data-ttu-id="3c32d-138">Mantenga presionada la tecla CTRL para seleccionar los exploradores que desee y, a continuación, haga clic en **examinar**:</span><span class="sxs-lookup"><span data-stu-id="3c32d-138">Hold down the CTRL key to select the browsers you want, and then click **Browse**:</span></span>

![Abrir muchos exploradores a la vez](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="3c32d-140">Esta es una captura de pantalla que muestra Visual Studio con la vista de índice abierta y dos exploradores abiertos:</span><span class="sxs-lookup"><span data-stu-id="3c32d-140">Here's a screenshot showing Visual Studio with the Index view open and two open browsers:</span></span>

![Ejemplo de sincronización con dos exploradores](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="3c32d-142">Mantenga el mouse sobre el control de la barra de herramientas de vínculos del explorador para ver los exploradores que están conectados al proyecto:</span><span class="sxs-lookup"><span data-stu-id="3c32d-142">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![Sugerencia de desplazamiento](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="3c32d-144">Cambiar la vista de índice y todos los exploradores conectados se actualizan al hacer clic en el botón actualizar vínculo del explorador:</span><span class="sxs-lookup"><span data-stu-id="3c32d-144">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![exploradores: sincronizar con cambios](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="3c32d-146">El vínculo del explorador también funciona con los exploradores que se inician desde fuera de Visual Studio y se navega a la dirección URL de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3c32d-146">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the application URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="3c32d-147">Panel de vínculos del explorador</span><span class="sxs-lookup"><span data-stu-id="3c32d-147">The Browser Link Dashboard</span></span>

<span data-ttu-id="3c32d-148">Abra el panel vínculo del explorador en el menú desplegable del vínculo del explorador para administrar la conexión con exploradores abiertos:</span><span class="sxs-lookup"><span data-stu-id="3c32d-148">Open the Browser Link Dashboard from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![Open-browserslink-Dashboard](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="3c32d-150">Si no hay ningún explorador conectado, puede iniciar una sesión que no sea de depuración seleccionando el vínculo *ver en el explorador* :</span><span class="sxs-lookup"><span data-stu-id="3c32d-150">If no browser is connected, you can start a non-debugging session by selecting the *View in Browser* link:</span></span>

![browserlink-panel-sin conexiones](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="3c32d-152">De lo contrario, los exploradores conectados se muestran con la ruta de acceso a la página que se muestra en cada explorador:</span><span class="sxs-lookup"><span data-stu-id="3c32d-152">Otherwise, the connected browsers are shown with the path to the page that each browser is showing:</span></span>

![browserlink-Dashboard-dos conexiones](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="3c32d-154">Si lo desea, puede hacer clic en el nombre de un explorador de la lista para actualizar ese explorador único.</span><span class="sxs-lookup"><span data-stu-id="3c32d-154">If you like, you can click on a listed browser name to refresh that single browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="3c32d-155">Habilitar o deshabilitar el vínculo del explorador</span><span class="sxs-lookup"><span data-stu-id="3c32d-155">Enable or disable Browser Link</span></span>

<span data-ttu-id="3c32d-156">Al volver a habilitar el vínculo del explorador después de deshabilitarlo, debe actualizar los exploradores para volver a conectarlos.</span><span class="sxs-lookup"><span data-stu-id="3c32d-156">When you re-enable Browser Link after disabling it, you must refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="3c32d-157">Habilitar o deshabilitar la sincronización automática de CSS</span><span class="sxs-lookup"><span data-stu-id="3c32d-157">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="3c32d-158">Cuando se habilita la sincronización automática de CSS, los exploradores conectados se actualizan automáticamente cuando se realiza cualquier cambio en los archivos CSS.</span><span class="sxs-lookup"><span data-stu-id="3c32d-158">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="3c32d-159">Cómo funciona</span><span class="sxs-lookup"><span data-stu-id="3c32d-159">How it works</span></span>

<span data-ttu-id="3c32d-160">El vínculo del explorador usa SignalR para crear un canal de comunicación entre Visual Studio y el explorador.</span><span class="sxs-lookup"><span data-stu-id="3c32d-160">Browser Link uses SignalR to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="3c32d-161">Cuando el vínculo del explorador está habilitado, Visual Studio actúa como un servidor de SignalR al que se pueden conectar varios clientes (exploradores).</span><span class="sxs-lookup"><span data-stu-id="3c32d-161">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="3c32d-162">El vínculo del explorador también registra un componente de middleware en la canalización de solicitudes ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3c32d-162">Browser Link also registers a middleware component in the ASP.NET Core request pipeline.</span></span> <span data-ttu-id="3c32d-163">Este componente inserta referencias de `<script>` especiales en cada solicitud de página del servidor.</span><span class="sxs-lookup"><span data-stu-id="3c32d-163">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="3c32d-164">Puede ver las referencias de script seleccionando **Ver código fuente** en el explorador y desplazándose hasta el final del contenido de la etiqueta `<body>`:</span><span class="sxs-lookup"><span data-stu-id="3c32d-164">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```html
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="3c32d-165">Los archivos de origen no se modifican.</span><span class="sxs-lookup"><span data-stu-id="3c32d-165">Your source files aren't modified.</span></span> <span data-ttu-id="3c32d-166">El componente middleware inserta las referencias de script dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="3c32d-166">The middleware component injects the script references dynamically.</span></span>

<span data-ttu-id="3c32d-167">Dado que el código del explorador es todo JavaScript, funciona en todos los exploradores que SignalR admite sin necesidad de un complemento de explorador.</span><span class="sxs-lookup"><span data-stu-id="3c32d-167">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports without requiring a browser plug-in.</span></span>
