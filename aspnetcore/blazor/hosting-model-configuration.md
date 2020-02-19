---
title: Configuración del modelo de hospedaje de Blazor de ASP.NET Core
author: guardrex
description: Obtenga información acerca de la configuración del modelo de hospedaje de Blazor, incluida la integración de componentes de Razor en aplicaciones Razor Pages y MVC.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-model-configuration
ms.openlocfilehash: bd44643877e45c5b48b0972bcc2f637fbc5d98f2
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447040"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a><span data-ttu-id="401b3-103">Configuración del modelo de hospedaje de ASP.NET Core increíblemente</span><span class="sxs-lookup"><span data-stu-id="401b3-103">ASP.NET Core Blazor hosting model configuration</span></span>

<span data-ttu-id="401b3-104">Por [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="401b3-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="401b3-105">En este artículo se trata la configuración del modelo de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="401b3-105">This article covers hosting model configuration.</span></span>

<!-- For future use:

## Blazor WebAssembly

-->

## <a name="blazor-server"></a><span data-ttu-id="401b3-106">Servidor Blazor</span><span class="sxs-lookup"><span data-stu-id="401b3-106">Blazor Server</span></span>

### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="401b3-107">Reflejar el estado de conexión en la interfaz de usuario</span><span class="sxs-lookup"><span data-stu-id="401b3-107">Reflect the connection state in the UI</span></span>

<span data-ttu-id="401b3-108">Cuando el cliente detecta que se ha perdido la conexión, se muestra al usuario una interfaz de usuario predeterminada mientras el cliente intenta volver a conectarse.</span><span class="sxs-lookup"><span data-stu-id="401b3-108">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="401b3-109">Si se produce un error en la reconexión, se proporciona al usuario la opción de volver a intentarlo.</span><span class="sxs-lookup"><span data-stu-id="401b3-109">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="401b3-110">Para personalizar la interfaz de usuario, defina un elemento con un `id` de `components-reconnect-modal` en la `<body>` de la página de Razor de *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="401b3-110">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="401b3-111">En la tabla siguiente se describen las clases CSS aplicadas al elemento `components-reconnect-modal`.</span><span class="sxs-lookup"><span data-stu-id="401b3-111">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="401b3-112">Clase CSS</span><span class="sxs-lookup"><span data-stu-id="401b3-112">CSS class</span></span>                       | <span data-ttu-id="401b3-113">Indica&hellip;</span><span class="sxs-lookup"><span data-stu-id="401b3-113">Indicates&hellip;</span></span> |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | <span data-ttu-id="401b3-114">Una conexión perdida.</span><span class="sxs-lookup"><span data-stu-id="401b3-114">A lost connection.</span></span> <span data-ttu-id="401b3-115">El cliente está intentando volver a conectarse.</span><span class="sxs-lookup"><span data-stu-id="401b3-115">The client is attempting to reconnect.</span></span> <span data-ttu-id="401b3-116">Muestre el modal.</span><span class="sxs-lookup"><span data-stu-id="401b3-116">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="401b3-117">Una conexión activa se restablece en el servidor.</span><span class="sxs-lookup"><span data-stu-id="401b3-117">An active connection is re-established to the server.</span></span> <span data-ttu-id="401b3-118">Oculte el modal.</span><span class="sxs-lookup"><span data-stu-id="401b3-118">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="401b3-119">Error de reconexión, probablemente debido a un error de red.</span><span class="sxs-lookup"><span data-stu-id="401b3-119">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="401b3-120">Para intentar la reconexión, llame a `window.Blazor.reconnect()`.</span><span class="sxs-lookup"><span data-stu-id="401b3-120">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="401b3-121">Reconexión rechazada.</span><span class="sxs-lookup"><span data-stu-id="401b3-121">Reconnection rejected.</span></span> <span data-ttu-id="401b3-122">Se alcanzó el servidor pero se rechazó la conexión y se pierde el estado del usuario en el servidor.</span><span class="sxs-lookup"><span data-stu-id="401b3-122">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="401b3-123">Para volver a cargar la aplicación, llame a `location.reload()`.</span><span class="sxs-lookup"><span data-stu-id="401b3-123">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="401b3-124">Este estado de conexión puede producirse cuando:</span><span class="sxs-lookup"><span data-stu-id="401b3-124">This connection state may result when:</span></span><ul><li><span data-ttu-id="401b3-125">Se produce un bloqueo en el circuito del servidor.</span><span class="sxs-lookup"><span data-stu-id="401b3-125">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="401b3-126">El cliente se desconecta el tiempo suficiente para que el servidor Quite el estado del usuario.</span><span class="sxs-lookup"><span data-stu-id="401b3-126">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="401b3-127">Se eliminan las instancias de los componentes con los que el usuario está interactuando.</span><span class="sxs-lookup"><span data-stu-id="401b3-127">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="401b3-128">El servidor se reinicia o se recicla el proceso de trabajo de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="401b3-128">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="render-mode"></a><span data-ttu-id="401b3-129">Modo de representación</span><span class="sxs-lookup"><span data-stu-id="401b3-129">Render mode</span></span>

<span data-ttu-id="401b3-130">Las aplicaciones de servidor increíbles se configuran de forma predeterminada para prerepresentar la interfaz de usuario en el servidor antes de que se establezca la conexión de cliente con el servidor.</span><span class="sxs-lookup"><span data-stu-id="401b3-130">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="401b3-131">Esto se configura en la página de Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="401b3-131">This is set up in the *_Host.cshtml* Razor page:</span></span>

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="401b3-132">`RenderMode` configura si el componente:</span><span class="sxs-lookup"><span data-stu-id="401b3-132">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="401b3-133">Se representa en la página.</span><span class="sxs-lookup"><span data-stu-id="401b3-133">Is prerendered into the page.</span></span>
* <span data-ttu-id="401b3-134">Se representa como HTML estático en la página o si incluye la información necesaria para iniciar una aplicación extraordinaria desde el agente de usuario.</span><span class="sxs-lookup"><span data-stu-id="401b3-134">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="401b3-135">Descripción</span><span class="sxs-lookup"><span data-stu-id="401b3-135">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="401b3-136">Representa el componente en código HTML estático e incluye un marcador para una aplicación de Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="401b3-136">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="401b3-137">Cuando se inicia el agente de usuario, este marcador se usa para arrancar una aplicación Blazor.</span><span class="sxs-lookup"><span data-stu-id="401b3-137">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="401b3-138">Representa un marcador para una aplicación de Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="401b3-138">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="401b3-139">La salida del componente no está incluida.</span><span class="sxs-lookup"><span data-stu-id="401b3-139">Output from the component isn't included.</span></span> <span data-ttu-id="401b3-140">Cuando se inicia el agente de usuario, este marcador se usa para arrancar una aplicación Blazor.</span><span class="sxs-lookup"><span data-stu-id="401b3-140">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="401b3-141">Representa el componente en HTML estático.</span><span class="sxs-lookup"><span data-stu-id="401b3-141">Renders the component into static HTML.</span></span> |

<span data-ttu-id="401b3-142">No se admite la representación de componentes de servidor desde una página HTML estática.</span><span class="sxs-lookup"><span data-stu-id="401b3-142">Rendering server components from a static HTML page isn't supported.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="401b3-143">Representación de componentes interactivos con estado desde vistas y páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="401b3-143">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="401b3-144">Los componentes interactivos con estado se pueden agregar a una página o vista de Razor.</span><span class="sxs-lookup"><span data-stu-id="401b3-144">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="401b3-145">Cuando se representa la página o la vista:</span><span class="sxs-lookup"><span data-stu-id="401b3-145">When the page or view renders:</span></span>

* <span data-ttu-id="401b3-146">El componente se representará con la página o la vista.</span><span class="sxs-lookup"><span data-stu-id="401b3-146">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="401b3-147">Se pierde el estado inicial del componente usado para la representación previa.</span><span class="sxs-lookup"><span data-stu-id="401b3-147">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="401b3-148">Cuando se establece la conexión SignalR, se crea un nuevo estado del componente.</span><span class="sxs-lookup"><span data-stu-id="401b3-148">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="401b3-149">La siguiente página de Razor representa un componente de `Counter`:</span><span class="sxs-lookup"><span data-stu-id="401b3-149">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="401b3-150">Representación de componentes no interactivos desde páginas y vistas de Razor</span><span class="sxs-lookup"><span data-stu-id="401b3-150">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="401b3-151">En la siguiente página de Razor, el componente de `Counter` se representa estáticamente con un valor inicial que se especifica mediante un formulario:</span><span class="sxs-lookup"><span data-stu-id="401b3-151">In the following Razor page, the `Counter` component is statically rendered with an initial value that's specified using a form:</span></span>

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

<span data-ttu-id="401b3-152">Como `MyComponent` se representa estáticamente, el componente no puede ser interactivo.</span><span class="sxs-lookup"><span data-stu-id="401b3-152">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="401b3-153">Configuración del cliente de SignalR para aplicaciones de Blazor Server</span><span class="sxs-lookup"><span data-stu-id="401b3-153">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="401b3-154">A veces, debe configurar el cliente de SignalR que usan las aplicaciones de Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="401b3-154">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="401b3-155">Por ejemplo, puede que desee configurar el registro en el SignalR cliente para diagnosticar un problema de conexión.</span><span class="sxs-lookup"><span data-stu-id="401b3-155">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="401b3-156">Para configurar el cliente de SignalR en el archivo *pages/_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="401b3-156">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="401b3-157">Agregue un atributo `autostart="false"` a la etiqueta de `<script>` para el script de `blazor.server.js`.</span><span class="sxs-lookup"><span data-stu-id="401b3-157">Add an `autostart="false"` attribute to the `<script>` tag for the `blazor.server.js` script.</span></span>
* <span data-ttu-id="401b3-158">Llame a `Blazor.start` y pase un objeto de configuración que especifique el generador de SignalR.</span><span class="sxs-lookup"><span data-stu-id="401b3-158">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

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
