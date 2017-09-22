---
title: "Vínculo de explorador en ASP.NET Core"
author: ncarandini
description: "Una característica de Visual Studio que se vincula el entorno de desarrollo con uno o varios exploradores web"
keywords: "Núcleo de ASP.NET, el vínculo de explorador, la sincronización de CSS"
ms.author: riande
manager: wpickett
ms.date: 12/28/2016
ms.topic: article
ms.assetid: 11813d4c-3f8a-445a-b23b-e4a57d001abc
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-browserlink
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 211dd5d03e6b8414e0b2ed3234d8970c92f72452
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-browser-link-in-aspnet-core"></a><span data-ttu-id="51646-104">Introducción al vínculo de explorador en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="51646-104">Introduction to Browser Link in ASP.NET Core</span></span> 

<span data-ttu-id="51646-105">Por [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), y [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="51646-105">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="51646-106">Vínculo de explorador es una característica de Visual Studio que crea un canal de comunicación entre el entorno de desarrollo y uno o varios exploradores web.</span><span class="sxs-lookup"><span data-stu-id="51646-106">Browser Link is a feature in Visual Studio that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="51646-107">Puede usar el vínculo de explorador para actualizar la aplicación web en varios exploradores a la vez, lo que resulta útil para las pruebas de varios exploradores.</span><span class="sxs-lookup"><span data-stu-id="51646-107">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="51646-108">Programa de instalación de vínculo de explorador</span><span class="sxs-lookup"><span data-stu-id="51646-108">Browser Link setup</span></span>

<span data-ttu-id="51646-109">El núcleo de ASP.NET **aplicación Web** plantillas de proyecto en Visual Studio 2015 y versiones posteriores incluyen todo lo necesario para el vínculo de explorador.</span><span class="sxs-lookup"><span data-stu-id="51646-109">The ASP.NET Core **Web Application** project templates in Visual Studio 2015 and later include everything needed for Browser Link.</span></span>

<span data-ttu-id="51646-110">Para agregar el vínculo de explorador a un proyecto que ha creado mediante ASP.NET Core **vacía** o **API Web** plantilla, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="51646-110">To add Browser Link to a project that you created by using the ASP.NET Core **Empty** or **Web API** template, follow these steps:</span></span>

1. <span data-ttu-id="51646-111">Agregar el *Microsoft.VisualStudio.Web.BrowserLink.Loader* paquete</span><span class="sxs-lookup"><span data-stu-id="51646-111">Add the *Microsoft.VisualStudio.Web.BrowserLink.Loader* package</span></span> 
2. <span data-ttu-id="51646-112">Agregue el código de configuración en el *Startup.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="51646-112">Add configuration code in the *Startup.cs* file.</span></span>

### <a name="add-the-package"></a><span data-ttu-id="51646-113">Agregue el paquete</span><span class="sxs-lookup"><span data-stu-id="51646-113">Add the package</span></span>

<span data-ttu-id="51646-114">Puesto que esta es una característica de Visual Studio, la manera más fácil de agregar el paquete es abrir el **Package Manager Console** (**Vista > otras ventanas > Package Manager Console**) y ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="51646-114">Since this is a Visual Studio feature, the easiest way to add the package is to open the **Package Manager Console** (**View > Other Windows > Package Manager Console**) and run the following command:</span></span>

```console
install-package Microsoft.VisualStudio.Web.BrowserLink.Loader
```

<span data-ttu-id="51646-115">Como alternativa, puede usar **Administrador de paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="51646-115">Alternatively, you can use **NuGet Package Manager**.</span></span>  <span data-ttu-id="51646-116">Haga clic en el nombre del proyecto en **el Explorador de soluciones**y elija **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="51646-116">Right-click the project name in **Solution Explorer**, and choose **Manage NuGet Packages**.</span></span> 

![Administrador de paquetes de NuGet abierto](using-browserlink/_static/open-nuget-package-manager.png)

<span data-ttu-id="51646-118">A continuación, buscar e instalar el paquete.</span><span class="sxs-lookup"><span data-stu-id="51646-118">Then find and install the package.</span></span>

![Agregue el paquete con el Administrador de paquetes de NuGet](using-browserlink/_static/add-package-with-nuget-package-manager.png)

### <a name="add-configuration-code"></a><span data-ttu-id="51646-120">Agregar código de configuración</span><span class="sxs-lookup"><span data-stu-id="51646-120">Add configuration code</span></span>

<span data-ttu-id="51646-121">Abra la *Startup.cs* archivo y en el `Configure` método agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="51646-121">Open the *Startup.cs* file, and in the `Configure` method add the following code:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="51646-122">Por lo general es que el código dentro de un `if` bloque que permite el vínculo de explorador únicamente en el entorno de desarrollo, como se muestra aquí:</span><span class="sxs-lookup"><span data-stu-id="51646-122">Usually that code is inside an `if` block that enables Browser Link only in the Development environment, as shown here:</span></span>

[!code-csharp[Main](./using-browserlink/sample/BrowserLinkSample/src/BrowserLinkSample/Startup.cs?highlight=1,4&range=40-44)]

<span data-ttu-id="51646-123">Para obtener más información, consulte [Trabajar con varios entornos](../fundamentals/environments.md).</span><span class="sxs-lookup"><span data-stu-id="51646-123">For more information, see [Working with Multiple Environments](../fundamentals/environments.md).</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="51646-124">Cómo usar el vínculo de explorador</span><span class="sxs-lookup"><span data-stu-id="51646-124">How to use Browser Link</span></span>

<span data-ttu-id="51646-125">Cuando hay un proyecto de ASP.NET Core abierta, Visual Studio muestra el control de barra de herramientas de vínculo de explorador junto a la **destino de depuración** control de barra de herramientas:</span><span class="sxs-lookup"><span data-stu-id="51646-125">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![Menú desplegable de vínculo de explorador](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="51646-127">Desde el control de barra de herramientas de vínculo de explorador, hacer lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="51646-127">From the Browser Link toolbar control, you can:</span></span>

- <span data-ttu-id="51646-128">Actualizar la aplicación web en varios exploradores a la vez</span><span class="sxs-lookup"><span data-stu-id="51646-128">Refresh the web application in several browsers at once</span></span>
- <span data-ttu-id="51646-129">Abra el **panel de vínculos de explorador**</span><span class="sxs-lookup"><span data-stu-id="51646-129">Open the **Browser Link Dashboard**</span></span>
- <span data-ttu-id="51646-130">Habilitar o deshabilitar **vínculo de explorador**</span><span class="sxs-lookup"><span data-stu-id="51646-130">Enable or disable **Browser Link**</span></span>
- <span data-ttu-id="51646-131">Habilitar o deshabilitar la sincronización automática de CSS</span><span class="sxs-lookup"><span data-stu-id="51646-131">Enable or disable CSS Auto-Sync</span></span>

> [!NOTE]
> <span data-ttu-id="51646-132">Algunos complementos de Visual Studio, sobre todo *2015 de paquete de extensión de Web* y *2017 de paquete de extensión de Web*, ofrecen una funcionalidad extendida de vínculo de explorador, pero algunas de las características adicionales no funcionan con ASP. Proyectos de red principal.</span><span class="sxs-lookup"><span data-stu-id="51646-132">Some Visual Studio plug-ins, most notably *Web Extension Pack 2015* and *Web Extension Pack 2017*, offer extended functionality for Browser Link, but some of the additional features don't work with ASP.NET Core projects.</span></span>

## <a name="refresh-the-web-application-in-several-browsers-at-once"></a><span data-ttu-id="51646-133">Actualizar la aplicación web en varios exploradores a la vez</span><span class="sxs-lookup"><span data-stu-id="51646-133">Refresh the web application in several browsers at once</span></span>

<span data-ttu-id="51646-134">Para elegir un explorador web único para iniciar al iniciar el proyecto, utilice el menú desplegable de la **destino de depuración** control de barra de herramientas:</span><span class="sxs-lookup"><span data-stu-id="51646-134">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![Menú desplegable de F5](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="51646-136">Para abrir varios exploradores a la vez, elija **examinar con... ** en la misma lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="51646-136">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span>  <span data-ttu-id="51646-137">Mantenga presionada la tecla CTRL para seleccionar los exploradores que desee y, a continuación, haga clic en **examinar**:</span><span class="sxs-lookup"><span data-stu-id="51646-137">Hold down the CTRL key to select the browsers you want, and then click **Browse**:</span></span>

![Abrir a la vez muchos exploradores](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="51646-139">Esta es una captura de pantalla de ejemplo que muestra Visual Studio con la vista de índice abierto y dos exploradores abiertos:</span><span class="sxs-lookup"><span data-stu-id="51646-139">Here's a sample screenshot showing Visual Studio with the Index view open and two open browsers:</span></span>

![Sincronizar con el ejemplo de dos exploradores](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="51646-141">Mantenga el mouse sobre el control de barra de herramientas de vínculo de explorador para ver los exploradores que están conectados al proyecto:</span><span class="sxs-lookup"><span data-stu-id="51646-141">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![Sugerencia al mantener el mouse](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="51646-143">Cambie la vista de índice y se actualizan todos los exploradores conectados al hacer clic en el botón de actualización de vínculo de explorador:</span><span class="sxs-lookup"><span data-stu-id="51646-143">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![sincronización de exploradores de cambios](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="51646-145">Vínculo de explorador también funciona con exploradores que inicie desde fuera de Visual Studio y navegue a la dirección URL de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="51646-145">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the application URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="51646-146">El panel de vínculos de explorador</span><span class="sxs-lookup"><span data-stu-id="51646-146">The Browser Link Dashboard</span></span>

<span data-ttu-id="51646-147">Abrir el panel de vínculo de explorador desde el menú para administrar la conexión con exploradores abiertos desplegable de vínculo de explorador:</span><span class="sxs-lookup"><span data-stu-id="51646-147">Open the Browser Link Dashboard from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![Abrir Panel de browserslink](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="51646-149">Si no hay ningún explorador está conectado, puede iniciar una sesión de depuración no clic en el _ver en el explorador_ vínculo:</span><span class="sxs-lookup"><span data-stu-id="51646-149">If no browser is connected, you can start a non debugging session clicking the _View in Browser_ link:</span></span>

![conexiones browserlink-panel-n](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="51646-151">En caso contrario, se muestran los exploradores conectados con la ruta de acceso a la página que se muestra cada explorador:</span><span class="sxs-lookup"><span data-stu-id="51646-151">Otherwise, the connected browsers are shown, with the path to the page that each browser is showing:</span></span>

![browserlink panel dos conexiones](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="51646-153">Si lo desea, puede hacer clic en un nombre de lista del explorador para actualizar esa única del explorador.</span><span class="sxs-lookup"><span data-stu-id="51646-153">If you like, you can click on a listed browser name to refresh that single browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="51646-154">Habilitar o deshabilitar el vínculo de explorador</span><span class="sxs-lookup"><span data-stu-id="51646-154">Enable or disable Browser Link</span></span>

<span data-ttu-id="51646-155">Al volver a habilitar vínculo de explorador después de deshabilitarlo, tendrá que actualizar los exploradores para volver a conectarlos.</span><span class="sxs-lookup"><span data-stu-id="51646-155">When you re-enable Browser Link after disabling it, you have to refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="51646-156">Habilitar o deshabilitar la sincronización automática de CSS</span><span class="sxs-lookup"><span data-stu-id="51646-156">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="51646-157">Cuando se habilita la sincronización automática de CSS, exploradores conectados se actualizan automáticamente cuando se realiza cualquier cambio a los archivos CSS.</span><span class="sxs-lookup"><span data-stu-id="51646-157">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-does-it-work"></a><span data-ttu-id="51646-158">¿Cómo funciona?</span><span class="sxs-lookup"><span data-stu-id="51646-158">How does it work?</span></span>

<span data-ttu-id="51646-159">Vínculo de explorador usa SignalR para crear un canal de comunicación entre Visual Studio y el explorador.</span><span class="sxs-lookup"><span data-stu-id="51646-159">Browser Link uses SignalR to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="51646-160">Cuando se habilita el vínculo de explorador, Visual Studio actúa como un servidor de SignalR que varios clientes (exploradores) pueden conectarse a.</span><span class="sxs-lookup"><span data-stu-id="51646-160">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="51646-161">Vínculo de explorador también registra un componente de middleware en la canalización de solicitudes ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="51646-161">Browser Link also registers a middleware component in the ASP.NET request pipeline.</span></span> <span data-ttu-id="51646-162">Este componente inserta especial `<script>` referencias en cada solicitud de página desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="51646-162">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="51646-163">Puede ver las referencias de script seleccionando **ver código fuente** en el explorador y el desplazamiento hasta el final de la `<body>` etiquetar contenido:</span><span class="sxs-lookup"><span data-stu-id="51646-163">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```javascript
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="51646-164">No se modifican los archivos de origen.</span><span class="sxs-lookup"><span data-stu-id="51646-164">Your source files are not modified.</span></span> <span data-ttu-id="51646-165">El componente de middleware inserta dinámicamente las referencias de script.</span><span class="sxs-lookup"><span data-stu-id="51646-165">The middleware component injects the script references dynamically.</span></span> 

<span data-ttu-id="51646-166">Dado que el código del lado del explorador es JavaScript todos, funciona en todos los exploradores compatibles con SignalR, sin necesidad de cualquier complemento de explorador.</span><span class="sxs-lookup"><span data-stu-id="51646-166">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports, without requiring any browser plug-in.</span></span>
