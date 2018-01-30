---
title: "Vínculo de explorador en ASP.NET Core"
author: ncarandini
description: "Explica cómo vínculo de explorador es una característica de Visual Studio que se vincula el entorno de desarrollo con uno o varios exploradores web."
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/using-browserlink
ms.openlocfilehash: 3e62bdd180bb1f5e2ce0645a8cf13c9ffe76197e
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="browser-link-in-aspnet-core"></a><span data-ttu-id="8e1c5-103">Vínculo de explorador en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8e1c5-103">Browser Link in ASP.NET Core</span></span> 

<span data-ttu-id="8e1c5-104">Por [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), y [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="8e1c5-104">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="8e1c5-105">Vínculo de explorador es una característica de Visual Studio que crea un canal de comunicación entre el entorno de desarrollo y uno o varios exploradores web.</span><span class="sxs-lookup"><span data-stu-id="8e1c5-105">Browser Link is a feature in Visual Studio that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="8e1c5-106">Puede usar el vínculo de explorador para actualizar la aplicación web en varios exploradores a la vez, lo que resulta útil para las pruebas de varios exploradores.</span><span class="sxs-lookup"><span data-stu-id="8e1c5-106">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="8e1c5-107">Programa de instalación de vínculo de explorador</span><span class="sxs-lookup"><span data-stu-id="8e1c5-107">Browser Link setup</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8e1c5-108">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8e1c5-108">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8e1c5-109">El núcleo de ASP.NET 2.x **aplicación Web**, **vacía**, y **API Web** plantilla proyectos utilizan la [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) paquete de Meta, que contiene una referencia de paquete para [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span><span class="sxs-lookup"><span data-stu-id="8e1c5-109">The ASP.NET Core 2.x **Web Application**, **Empty**, and **Web API** template projects use the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) meta-package, which contains a package reference for [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span></span> <span data-ttu-id="8e1c5-110">Por tanto, se usan las `Microsoft.AspNetCore.All` meta-package no requiere ninguna acción adicional para que estén disponibles para su uso vínculo de explorador.</span><span class="sxs-lookup"><span data-stu-id="8e1c5-110">Therefore, using the `Microsoft.AspNetCore.All` meta-package requires no further action to make Browser Link available for use.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8e1c5-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8e1c5-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8e1c5-112">El núcleo de ASP.NET 1.x **aplicación Web** plantilla de proyecto tiene una referencia de paquete para la [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) paquete.</span><span class="sxs-lookup"><span data-stu-id="8e1c5-112">The ASP.NET Core 1.x **Web Application** project template has a package reference for the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package.</span></span> <span data-ttu-id="8e1c5-113">El **vacía** o **API Web** proyectos de plantilla requieren que agregue una referencia de paquete a `Microsoft.VisualStudio.Web.BrowserLink`.</span><span class="sxs-lookup"><span data-stu-id="8e1c5-113">The **Empty** or **Web API** template projects require you to add a package reference to `Microsoft.VisualStudio.Web.BrowserLink`.</span></span>

<span data-ttu-id="8e1c5-114">Puesto que esta es una característica de Visual Studio, la manera más fácil para agregar el paquete a un **vacía** o **API Web** proyecto de plantilla es abrir el **Package Manager Console** (**Vista** > **otras ventanas** > **Package Manager Console**) y ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="8e1c5-114">Since this is a Visual Studio feature, the easiest way to add the package to an **Empty** or **Web API** template project is to open the **Package Manager Console** (**View** > **Other Windows** > **Package Manager Console**) and run the following command:</span></span>

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

<span data-ttu-id="8e1c5-115">Como alternativa, puede usar **Administrador de paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="8e1c5-115">Alternatively, you can use **NuGet Package Manager**.</span></span> <span data-ttu-id="8e1c5-116">Haga clic en el nombre del proyecto en **el Explorador de soluciones** y elija **administrar paquetes de NuGet**:</span><span class="sxs-lookup"><span data-stu-id="8e1c5-116">Right-click the project name in **Solution Explorer** and choose **Manage NuGet Packages**:</span></span>

![Administrador de paquetes de NuGet abierto](using-browserlink/_static/open-nuget-package-manager.png)

<span data-ttu-id="8e1c5-118">Buscar e instalar el paquete:</span><span class="sxs-lookup"><span data-stu-id="8e1c5-118">Find and install the package:</span></span>

![Agregue el paquete con el Administrador de paquetes de NuGet](using-browserlink/_static/add-package-with-nuget-package-manager.png)

---

### <a name="configuration"></a><span data-ttu-id="8e1c5-120">Configuración</span><span class="sxs-lookup"><span data-stu-id="8e1c5-120">Configuration</span></span>

<span data-ttu-id="8e1c5-121">En el `Configure` método de la *Startup.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="8e1c5-121">In the `Configure` method of the *Startup.cs* file:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="8e1c5-122">Normalmente es el código dentro de un `if` bloque que solo habilita el vínculo de explorador en el entorno de desarrollo, como se muestra aquí:</span><span class="sxs-lookup"><span data-stu-id="8e1c5-122">Usually the code is inside an `if` block that only enables Browser Link in the Development environment, as shown here:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

<span data-ttu-id="8e1c5-123">Para obtener más información, consulte [Trabajar con varios entornos](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="8e1c5-123">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="8e1c5-124">Cómo usar el vínculo de explorador</span><span class="sxs-lookup"><span data-stu-id="8e1c5-124">How to use Browser Link</span></span>

<span data-ttu-id="8e1c5-125">Cuando hay un proyecto de ASP.NET Core abierta, Visual Studio muestra el control de barra de herramientas de vínculo de explorador junto a la **destino de depuración** control de barra de herramientas:</span><span class="sxs-lookup"><span data-stu-id="8e1c5-125">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![Menú desplegable de vínculo de explorador](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="8e1c5-127">Desde el control de barra de herramientas de vínculo de explorador, hacer lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="8e1c5-127">From the Browser Link toolbar control, you can:</span></span>

* <span data-ttu-id="8e1c5-128">Actualizar la aplicación web en varios exploradores a la vez.</span><span class="sxs-lookup"><span data-stu-id="8e1c5-128">Refresh the web application in several browsers at once.</span></span>
* <span data-ttu-id="8e1c5-129">Abra la **panel de vínculos de explorador**.</span><span class="sxs-lookup"><span data-stu-id="8e1c5-129">Open the **Browser Link Dashboard**.</span></span>
* <span data-ttu-id="8e1c5-130">Habilitar o deshabilitar **vínculo de explorador**.</span><span class="sxs-lookup"><span data-stu-id="8e1c5-130">Enable or disable **Browser Link**.</span></span> <span data-ttu-id="8e1c5-131">Nota: El vínculo de explorador está deshabilitada de forma predeterminada en Visual Studio 2017 (15.3).</span><span class="sxs-lookup"><span data-stu-id="8e1c5-131">Note: Browser Link is disabled by default in Visual Studio 2017 (15.3).</span></span>
* <span data-ttu-id="8e1c5-132">Habilitar o deshabilitar [sincronización automática de CSS](#enable-or-disable-css-auto-sync).</span><span class="sxs-lookup"><span data-stu-id="8e1c5-132">Enable or disable [CSS Auto-Sync](#enable-or-disable-css-auto-sync).</span></span>

> [!NOTE]
> <span data-ttu-id="8e1c5-133">Algunos complementos de Visual Studio, sobre todo *2015 de paquete de extensión de Web* y *2017 de paquete de extensión de Web*, ofrecen una funcionalidad extendida de vínculo de explorador, pero algunas de las características adicionales no funcionan con ASP. Proyectos de red principal.</span><span class="sxs-lookup"><span data-stu-id="8e1c5-133">Some Visual Studio plug-ins, most notably *Web Extension Pack 2015* and *Web Extension Pack 2017*, offer extended functionality for Browser Link, but some of the additional features don't work with ASP.NET Core projects.</span></span>

## <a name="refresh-the-web-application-in-several-browsers-at-once"></a><span data-ttu-id="8e1c5-134">Actualizar la aplicación web en varios exploradores a la vez</span><span class="sxs-lookup"><span data-stu-id="8e1c5-134">Refresh the web application in several browsers at once</span></span>

<span data-ttu-id="8e1c5-135">Para elegir un explorador web único para iniciar al iniciar el proyecto, utilice el menú desplegable de la **destino de depuración** control de barra de herramientas:</span><span class="sxs-lookup"><span data-stu-id="8e1c5-135">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![Menú desplegable de F5](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="8e1c5-137">Para abrir varios exploradores a la vez, elija **examinar con...**  en la misma lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="8e1c5-137">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span> <span data-ttu-id="8e1c5-138">Mantenga presionada la tecla CTRL para seleccionar los exploradores que desee y, a continuación, haga clic en **examinar**:</span><span class="sxs-lookup"><span data-stu-id="8e1c5-138">Hold down the CTRL key to select the browsers you want, and then click **Browse**:</span></span>

![Abrir a la vez muchos exploradores](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="8e1c5-140">Esta es una captura de pantalla que muestra Visual Studio con la vista de índice abierto y dos exploradores abiertos:</span><span class="sxs-lookup"><span data-stu-id="8e1c5-140">Here's a screenshot showing Visual Studio with the Index view open and two open browsers:</span></span>

![Sincronizar con el ejemplo de dos exploradores](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="8e1c5-142">Mantenga el mouse sobre el control de barra de herramientas de vínculo de explorador para ver los exploradores que están conectados al proyecto:</span><span class="sxs-lookup"><span data-stu-id="8e1c5-142">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![Sugerencia al mantener el mouse](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="8e1c5-144">Cambie la vista de índice y se actualizan todos los exploradores conectados al hacer clic en el botón de actualización de vínculo de explorador:</span><span class="sxs-lookup"><span data-stu-id="8e1c5-144">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![sincronización de exploradores de cambios](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="8e1c5-146">Vínculo de explorador también funciona con exploradores que inicie desde fuera de Visual Studio y navegue a la dirección URL de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8e1c5-146">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the application URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="8e1c5-147">El panel de vínculos de explorador</span><span class="sxs-lookup"><span data-stu-id="8e1c5-147">The Browser Link Dashboard</span></span>

<span data-ttu-id="8e1c5-148">Abrir el panel de vínculo de explorador desde el menú para administrar la conexión con exploradores abiertos desplegable de vínculo de explorador:</span><span class="sxs-lookup"><span data-stu-id="8e1c5-148">Open the Browser Link Dashboard from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![open-browserslink-dashboard](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="8e1c5-150">Si no hay ningún explorador está conectado, puede iniciar una sesión de depuración no seleccionando la *ver en el explorador* vínculo:</span><span class="sxs-lookup"><span data-stu-id="8e1c5-150">If no browser is connected, you can start a non-debugging session by selecting the *View in Browser* link:</span></span>

![conexiones browserlink-panel-n](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="8e1c5-152">En caso contrario, se muestran los exploradores conectados con la ruta de acceso a la página que se muestra cada explorador:</span><span class="sxs-lookup"><span data-stu-id="8e1c5-152">Otherwise, the connected browsers are shown with the path to the page that each browser is showing:</span></span>

![browserlink-dashboard-two-connections](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="8e1c5-154">Si lo desea, puede hacer clic en un nombre de lista del explorador para actualizar esa única del explorador.</span><span class="sxs-lookup"><span data-stu-id="8e1c5-154">If you like, you can click on a listed browser name to refresh that single browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="8e1c5-155">Habilitar o deshabilitar el vínculo de explorador</span><span class="sxs-lookup"><span data-stu-id="8e1c5-155">Enable or disable Browser Link</span></span>

<span data-ttu-id="8e1c5-156">Al volver a habilitar vínculo de explorador después de deshabilitarlo, debe actualizar los exploradores para volver a conectarlos.</span><span class="sxs-lookup"><span data-stu-id="8e1c5-156">When you re-enable Browser Link after disabling it, you must refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="8e1c5-157">Habilitar o deshabilitar la sincronización automática de CSS</span><span class="sxs-lookup"><span data-stu-id="8e1c5-157">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="8e1c5-158">Cuando se habilita la sincronización automática de CSS, exploradores conectados se actualizan automáticamente cuando se realiza cualquier cambio a los archivos CSS.</span><span class="sxs-lookup"><span data-stu-id="8e1c5-158">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-does-it-work"></a><span data-ttu-id="8e1c5-159">¿Cómo funciona?</span><span class="sxs-lookup"><span data-stu-id="8e1c5-159">How does it work?</span></span>

<span data-ttu-id="8e1c5-160">Vínculo de explorador usa SignalR para crear un canal de comunicación entre Visual Studio y el explorador.</span><span class="sxs-lookup"><span data-stu-id="8e1c5-160">Browser Link uses SignalR to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="8e1c5-161">Cuando se habilita el vínculo de explorador, Visual Studio actúa como un servidor de SignalR que varios clientes (exploradores) pueden conectarse a.</span><span class="sxs-lookup"><span data-stu-id="8e1c5-161">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="8e1c5-162">Vínculo de explorador también registra un componente de middleware en la canalización de solicitudes ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8e1c5-162">Browser Link also registers a middleware component in the ASP.NET request pipeline.</span></span> <span data-ttu-id="8e1c5-163">Este componente inserta especial `<script>` referencias en cada solicitud de página desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="8e1c5-163">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="8e1c5-164">Puede ver las referencias de script seleccionando **ver código fuente** en el explorador y el desplazamiento hasta el final de la `<body>` etiquetar contenido:</span><span class="sxs-lookup"><span data-stu-id="8e1c5-164">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```javascript
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="8e1c5-165">No modifican los archivos de origen.</span><span class="sxs-lookup"><span data-stu-id="8e1c5-165">Your source files aren't modified.</span></span> <span data-ttu-id="8e1c5-166">El componente de middleware inserta dinámicamente las referencias de script.</span><span class="sxs-lookup"><span data-stu-id="8e1c5-166">The middleware component injects the script references dynamically.</span></span> 

<span data-ttu-id="8e1c5-167">Dado que el código del lado del explorador es JavaScript todos, funciona en todos los exploradores compatibles con SignalR sin necesidad de un complemento de explorador.</span><span class="sxs-lookup"><span data-stu-id="8e1c5-167">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports without requiring a browser plug-in.</span></span>
