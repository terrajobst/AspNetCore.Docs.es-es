---
title: Modelos de hospedaje increíblemente ASP.NET Core
author: guardrex
description: Comprenda los modelos de hospedaje de lado cliente y de servidor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: blazor/hosting-models
ms.openlocfilehash: bf2bce4f89e8bfe6e5aeeb4860c85a60c5eb4b7c
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310396"
---
# <a name="aspnet-core-blazor-hosting-models"></a><span data-ttu-id="65d31-103">Modelos de hospedaje increíblemente ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="65d31-103">ASP.NET Core Blazor hosting models</span></span>

<span data-ttu-id="65d31-104">Por [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="65d31-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="65d31-105">Increíble es un marco de trabajo web diseñado para ejecutar el lado cliente en el explorador en un entorno de tiempo de ejecución .net basado en [WebAssembly](https://webassembly.org/) (*cliente más rápido*) o en el lado servidor en ASP.net Core (*servidor más rápido*).</span><span class="sxs-lookup"><span data-stu-id="65d31-105">Blazor is a web framework designed to run client-side in the browser on a [WebAssembly](https://webassembly.org/)-based .NET runtime (*Blazor client-side*) or server-side in ASP.NET Core (*Blazor server-side*).</span></span> <span data-ttu-id="65d31-106">Independientemente del modelo de hospedaje, los modelos de aplicación y de componente *son los mismos*.</span><span class="sxs-lookup"><span data-stu-id="65d31-106">Regardless of the hosting model, the app and component models *are the same*.</span></span>

<span data-ttu-id="65d31-107">Para crear un proyecto para los modelos de hospedaje descritos en este artículo <xref:blazor/get-started>, vea.</span><span class="sxs-lookup"><span data-stu-id="65d31-107">To create a project for the hosting models described in this article, see <xref:blazor/get-started>.</span></span>

## <a name="client-side"></a><span data-ttu-id="65d31-108">Lado cliente</span><span class="sxs-lookup"><span data-stu-id="65d31-108">Client-side</span></span>

<span data-ttu-id="65d31-109">El modelo de hospedaje principal de increíbles se está ejecutando en el lado cliente en el explorador de webassembly.</span><span class="sxs-lookup"><span data-stu-id="65d31-109">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="65d31-110">La aplicación Blazor, sus dependencias y el entorno de ejecución de .NET se descargan en el explorador.</span><span class="sxs-lookup"><span data-stu-id="65d31-110">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="65d31-111">La aplicación se ejecuta directamente en el subproceso de interfaz de usuario del explorador.</span><span class="sxs-lookup"><span data-stu-id="65d31-111">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="65d31-112">Las actualizaciones de la interfaz de usuario y el control de eventos se producen dentro del mismo proceso.</span><span class="sxs-lookup"><span data-stu-id="65d31-112">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="65d31-113">Los recursos de la aplicación se implementan como archivos estáticos en un servidor web o servicio capaz de servir contenido estático a los clientes.</span><span class="sxs-lookup"><span data-stu-id="65d31-113">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![Impresionante cliente: La aplicación extraordinaria se ejecuta en un subproceso de interfaz de usuario dentro del explorador.](hosting-models/_static/client-side.png)

<span data-ttu-id="65d31-115">Para crear una aplicación increíblemente alta con el modelo de hospedaje de cliente, use la plantilla de **aplicación de Webassemble de extraordinarias** ([dotnet New blazorwasm](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="65d31-115">To create a Blazor app using the client-side hosting model, use the **Blazor WebAssembly App** template ([dotnet new blazorwasm](/dotnet/core/tools/dotnet-new)).</span></span>

<span data-ttu-id="65d31-116">Después de seleccionar la plantilla de **aplicación Webassembly** de increíble, tiene la opción de configurar la aplicación para usar un back-end de ASP.net Core activando la casilla **ASP.net Core hospedado** ([dotnet New blazorwasm--Hosted](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="65d31-116">After selecting the **Blazor WebAssembly App** template, you have the option of configuring the app to use an ASP.NET Core backend by selecting the **ASP.NET Core hosted** check box ([dotnet new blazorwasm --hosted](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="65d31-117">La aplicación ASP.NET Core envía la aplicación increíblemente a los clientes.</span><span class="sxs-lookup"><span data-stu-id="65d31-117">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="65d31-118">La aplicación del lado cliente más increíble puede interactuar con el servidor a través de la red mediante llamadas API web o [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="65d31-118">The Blazor client-side app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="65d31-119">Las plantillas incluyen el script *increíblemente. webassembly. js* que controla:</span><span class="sxs-lookup"><span data-stu-id="65d31-119">The templates include the *blazor.webassembly.js* script that handles:</span></span>

* <span data-ttu-id="65d31-120">Descarga del tiempo de ejecución de .NET, la aplicación y las dependencias de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="65d31-120">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="65d31-121">Inicialización del tiempo de ejecución para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="65d31-121">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="65d31-122">El modelo de hospedaje del lado cliente ofrece varias ventajas:</span><span class="sxs-lookup"><span data-stu-id="65d31-122">The client-side hosting model offers several benefits:</span></span>

* <span data-ttu-id="65d31-123">No hay ninguna dependencia de servidor .NET.</span><span class="sxs-lookup"><span data-stu-id="65d31-123">There's no .NET server-side dependency.</span></span> <span data-ttu-id="65d31-124">La aplicación funciona totalmente después de descargarla en el cliente.</span><span class="sxs-lookup"><span data-stu-id="65d31-124">The app is fully functioning after downloaded to the client.</span></span>
* <span data-ttu-id="65d31-125">Los recursos de cliente y las capacidades se aprovechan por completo.</span><span class="sxs-lookup"><span data-stu-id="65d31-125">Client resources and capabilities are fully leveraged.</span></span>
* <span data-ttu-id="65d31-126">El trabajo se descarga del servidor al cliente.</span><span class="sxs-lookup"><span data-stu-id="65d31-126">Work is offloaded from the server to the client.</span></span>
* <span data-ttu-id="65d31-127">No es necesario un servidor Web ASP.NET Core para hospedar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="65d31-127">An ASP.NET Core web server isn't required to host the app.</span></span> <span data-ttu-id="65d31-128">Los escenarios de implementación sin servidor son posibles (por ejemplo, para dar servicio a la aplicación desde una red CDN).</span><span class="sxs-lookup"><span data-stu-id="65d31-128">Serverless deployment scenarios are possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="65d31-129">Hay desventajas en el hospedaje del lado cliente:</span><span class="sxs-lookup"><span data-stu-id="65d31-129">There are downsides to client-side hosting:</span></span>

* <span data-ttu-id="65d31-130">La aplicación está restringida a las capacidades del explorador.</span><span class="sxs-lookup"><span data-stu-id="65d31-130">The app is restricted to the capabilities of the browser.</span></span>
* <span data-ttu-id="65d31-131">Se requiere hardware y software de cliente compatible (por ejemplo, compatibilidad con webassembly).</span><span class="sxs-lookup"><span data-stu-id="65d31-131">Capable client hardware and software (for example, WebAssembly support) is required.</span></span>
* <span data-ttu-id="65d31-132">El tamaño de descarga es mayor y las aplicaciones tardan más tiempo en cargarse.</span><span class="sxs-lookup"><span data-stu-id="65d31-132">Download size is larger, and apps take longer to load.</span></span>
* <span data-ttu-id="65d31-133">La compatibilidad con las herramientas y el tiempo de ejecución de .NET es menos madura.</span><span class="sxs-lookup"><span data-stu-id="65d31-133">.NET runtime and tooling support is less mature.</span></span> <span data-ttu-id="65d31-134">Por ejemplo, existen limitaciones en [.net Standard](/dotnet/standard/net-standard) la compatibilidad y la depuración.</span><span class="sxs-lookup"><span data-stu-id="65d31-134">For example, limitations exist in [.NET Standard](/dotnet/standard/net-standard) support and debugging.</span></span>

## <a name="server-side"></a><span data-ttu-id="65d31-135">Lado servidor</span><span class="sxs-lookup"><span data-stu-id="65d31-135">Server-side</span></span>

<span data-ttu-id="65d31-136">Con el modelo de hospedaje del lado servidor, la aplicación se ejecuta en el servidor desde una aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="65d31-136">With the server-side hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="65d31-137">Las actualizaciones de la interfaz de usuario, el control de eventos y las llamadas de JavaScript se controlan mediante una conexión de [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="65d31-137">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![El explorador interactúa con la aplicación (hospedada en una aplicación ASP.NET Core) en el servidor a través de una conexión Signalr.](hosting-models/_static/server-side.png)

<span data-ttu-id="65d31-139">Para crear una aplicación increíblemente alta con el modelo de hospedaje del lado servidor, use la plantilla de **aplicación de servidor** de ASP.net Core increíble ([dotnet New blazorserver](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="65d31-139">To create a Blazor app using the server-side hosting model, use the ASP.NET Core **Blazor Server App** template ([dotnet new blazorserver](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="65d31-140">La aplicación ASP.NET Core hospeda la aplicación del lado servidor y crea el punto de conexión de Signalr al que se conectan los clientes.</span><span class="sxs-lookup"><span data-stu-id="65d31-140">The ASP.NET Core app hosts the server-side app and creates the SignalR endpoint where clients connect.</span></span>

<span data-ttu-id="65d31-141">La aplicación ASP.net Core hace referencia a la `Startup` clase de la aplicación que se va a agregar:</span><span class="sxs-lookup"><span data-stu-id="65d31-141">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="65d31-142">Servicios del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="65d31-142">Server-side services.</span></span>
* <span data-ttu-id="65d31-143">La aplicación a la canalización de control de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="65d31-143">The app to the request handling pipeline.</span></span>

<span data-ttu-id="65d31-144">El script&dagger; *increíblemente. Server. js* establece la conexión de cliente.</span><span class="sxs-lookup"><span data-stu-id="65d31-144">The *blazor.server.js* script&dagger; establishes the client connection.</span></span> <span data-ttu-id="65d31-145">Es responsabilidad de la aplicación conservar y restaurar el estado de la aplicación según sea necesario (por ejemplo, en caso de que se pierda una conexión de red).</span><span class="sxs-lookup"><span data-stu-id="65d31-145">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="65d31-146">El modelo de hospedaje del lado servidor ofrece varias ventajas:</span><span class="sxs-lookup"><span data-stu-id="65d31-146">The server-side hosting model offers several benefits:</span></span>

* <span data-ttu-id="65d31-147">El tamaño de la descarga es significativamente menor que el de una aplicación del lado cliente y la aplicación se carga mucho más rápido.</span><span class="sxs-lookup"><span data-stu-id="65d31-147">Download size is significantly smaller than a client-side app, and the app loads much faster.</span></span>
* <span data-ttu-id="65d31-148">La aplicación aprovecha al máximo las funcionalidades del servidor, incluido el uso de cualquier API compatible con .NET Core.</span><span class="sxs-lookup"><span data-stu-id="65d31-148">The app takes full advantage of server capabilities, including use of any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="65d31-149">.NET Core en el servidor se usa para ejecutar la aplicación, por lo que las herramientas de .NET existentes, como la depuración, funcionan según lo previsto.</span><span class="sxs-lookup"><span data-stu-id="65d31-149">.NET Core on the server is used to run the app, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="65d31-150">Se admiten clientes ligeros.</span><span class="sxs-lookup"><span data-stu-id="65d31-150">Thin clients are supported.</span></span> <span data-ttu-id="65d31-151">Por ejemplo, las aplicaciones del lado servidor funcionan con los exploradores que no admiten webassembly y en los dispositivos con restricción de recursos.</span><span class="sxs-lookup"><span data-stu-id="65d31-151">For example, server-side apps work with browsers that don't support WebAssembly and on resource-constrained devices.</span></span>
* <span data-ttu-id="65d31-152">La base de .NET/C# Code de la aplicación, incluido el código de componente de la aplicación, no se sirve a los clientes.</span><span class="sxs-lookup"><span data-stu-id="65d31-152">The app's .NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="65d31-153">Hay desventajas en el hospedaje del lado servidor:</span><span class="sxs-lookup"><span data-stu-id="65d31-153">There are downsides to server-side hosting:</span></span>

* <span data-ttu-id="65d31-154">Normalmente existe una mayor latencia.</span><span class="sxs-lookup"><span data-stu-id="65d31-154">Higher latency usually exists.</span></span> <span data-ttu-id="65d31-155">Cada interacción del usuario implica un salto de red.</span><span class="sxs-lookup"><span data-stu-id="65d31-155">Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="65d31-156">No hay compatibilidad sin conexión.</span><span class="sxs-lookup"><span data-stu-id="65d31-156">There's no offline support.</span></span> <span data-ttu-id="65d31-157">Si se produce un error en la conexión de cliente, la aplicación deja de funcionar.</span><span class="sxs-lookup"><span data-stu-id="65d31-157">If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="65d31-158">La escalabilidad es desafiante para aplicaciones con muchos usuarios.</span><span class="sxs-lookup"><span data-stu-id="65d31-158">Scalability is challenging for apps with many users.</span></span> <span data-ttu-id="65d31-159">El servidor debe administrar varias conexiones de cliente y controlar el estado del cliente.</span><span class="sxs-lookup"><span data-stu-id="65d31-159">The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="65d31-160">Se requiere un servidor ASP.NET Core para atender la aplicación.</span><span class="sxs-lookup"><span data-stu-id="65d31-160">An ASP.NET Core server is required to serve the app.</span></span> <span data-ttu-id="65d31-161">Los escenarios de implementación sin servidor no son posibles (por ejemplo, para dar servicio a la aplicación desde una red CDN).</span><span class="sxs-lookup"><span data-stu-id="65d31-161">Serverless deployment scenarios aren't possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="65d31-162">&dagger;El script *increíblemente. Server. js* se sirve desde un recurso incrustado en la ASP.net Core marco de trabajo compartido.</span><span class="sxs-lookup"><span data-stu-id="65d31-162">&dagger;The *blazor.server.js* script is served from an embedded resource in the ASP.NET Core shared framework.</span></span>

### <a name="reconnection-to-the-same-server"></a><span data-ttu-id="65d31-163">Reconexión al mismo servidor</span><span class="sxs-lookup"><span data-stu-id="65d31-163">Reconnection to the same server</span></span>

<span data-ttu-id="65d31-164">Las aplicaciones de servidor increíblemente precisas requieren una conexión de Signalr activa con el servidor.</span><span class="sxs-lookup"><span data-stu-id="65d31-164">Blazor server-side apps require an active SignalR connection to the server.</span></span> <span data-ttu-id="65d31-165">Si se pierde la conexión, la aplicación intenta volver a conectarse al servidor.</span><span class="sxs-lookup"><span data-stu-id="65d31-165">If the connection is lost, the app attempts to reconnect to the server.</span></span> <span data-ttu-id="65d31-166">Siempre que el estado del cliente todavía esté en la memoria, la sesión del cliente se reanudará sin perder el estado.</span><span class="sxs-lookup"><span data-stu-id="65d31-166">As long as the client's state is still in memory, the client session resumes without losing state.</span></span>
 
<span data-ttu-id="65d31-167">Cuando el cliente detecta que se ha perdido la conexión, se muestra al usuario una interfaz de usuario predeterminada mientras el cliente intenta volver a conectarse.</span><span class="sxs-lookup"><span data-stu-id="65d31-167">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="65d31-168">Si se produce un error en la reconexión, se proporciona al usuario la opción de volver a intentarlo.</span><span class="sxs-lookup"><span data-stu-id="65d31-168">If reconnection fails, the user is provided the option to retry.</span></span> <span data-ttu-id="65d31-169">Para personalizar la interfaz de usuario, defina un `components-reconnect-modal` elemento con `id` como su en la página de Razor *_Host. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="65d31-169">To customize the UI, define an element with `components-reconnect-modal` as its `id` in the *_Host.cshtml* Razor page.</span></span> <span data-ttu-id="65d31-170">El cliente actualiza este elemento con una de las siguientes clases CSS según el estado de la conexión:</span><span class="sxs-lookup"><span data-stu-id="65d31-170">The client updates this element with one of the following CSS classes based on the state of the connection:</span></span>
 
* <span data-ttu-id="65d31-171">`components-reconnect-show`&ndash; Muestre la interfaz de usuario para indicar que se perdió la conexión y que el cliente intenta volver a conectarse.</span><span class="sxs-lookup"><span data-stu-id="65d31-171">`components-reconnect-show` &ndash; Show the UI to indicate the connection was lost and the client is attempting to reconnect.</span></span>
* <span data-ttu-id="65d31-172">`components-reconnect-hide`&ndash; El cliente tiene una conexión activa, oculte la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="65d31-172">`components-reconnect-hide` &ndash; The client has an active connection, hide the UI.</span></span>
* <span data-ttu-id="65d31-173">`components-reconnect-failed`&ndash; Error de reconexión.</span><span class="sxs-lookup"><span data-stu-id="65d31-173">`components-reconnect-failed` &ndash; Reconnection failed.</span></span> <span data-ttu-id="65d31-174">Para intentar de nuevo la reconexión `window.Blazor.reconnect()`, llame a.</span><span class="sxs-lookup"><span data-stu-id="65d31-174">To attempt reconnection again, call `window.Blazor.reconnect()`.</span></span>

### <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="65d31-175">Reconexión con estado después de la representación previa</span><span class="sxs-lookup"><span data-stu-id="65d31-175">Stateful reconnection after prerendering</span></span>
 
<span data-ttu-id="65d31-176">Las aplicaciones de servidor increíbles se configuran de forma predeterminada para prerepresentar la interfaz de usuario en el servidor antes de que se establezca la conexión de cliente con el servidor.</span><span class="sxs-lookup"><span data-stu-id="65d31-176">Blazor server-side apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="65d31-177">Esto se configura en la página de Razor de *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="65d31-177">This is set up in the *_Host.cshtml* Razor page:</span></span>
 
```cshtml
<body>
    <app>@(await Html.RenderComponentAsync<App>(RenderMode.ServerPrerendered))</app>
 
    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="65d31-178">`RenderMode`configura si el componente:</span><span class="sxs-lookup"><span data-stu-id="65d31-178">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="65d31-179">Se representa en la página.</span><span class="sxs-lookup"><span data-stu-id="65d31-179">Is prerendered into the page.</span></span>
* <span data-ttu-id="65d31-180">Se representa como HTML estático en la página o si incluye la información necesaria para iniciar una aplicación extraordinaria desde el agente de usuario.</span><span class="sxs-lookup"><span data-stu-id="65d31-180">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="65d31-181">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="65d31-181">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="65d31-182">Representa el componente en código HTML estático e incluye un marcador para una aplicación del lado servidor increíblemente alta.</span><span class="sxs-lookup"><span data-stu-id="65d31-182">Renders the component into static HTML and includes a marker for a Blazor server-side app.</span></span> <span data-ttu-id="65d31-183">Cuando se inicia el agente de usuario, este marcador se usa para arrancar una aplicación increíblemente alta.</span><span class="sxs-lookup"><span data-stu-id="65d31-183">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="65d31-184">No se admiten los parámetros.</span><span class="sxs-lookup"><span data-stu-id="65d31-184">Parameters aren't supported.</span></span> |
| `Server`            | <span data-ttu-id="65d31-185">Representa un marcador para una aplicación del lado servidor increíblemente alta.</span><span class="sxs-lookup"><span data-stu-id="65d31-185">Renders a marker for a Blazor server-side app.</span></span> <span data-ttu-id="65d31-186">La salida del componente no está incluida.</span><span class="sxs-lookup"><span data-stu-id="65d31-186">Output from the component isn't included.</span></span> <span data-ttu-id="65d31-187">Cuando se inicia el agente de usuario, este marcador se usa para arrancar una aplicación increíblemente alta.</span><span class="sxs-lookup"><span data-stu-id="65d31-187">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="65d31-188">No se admiten los parámetros.</span><span class="sxs-lookup"><span data-stu-id="65d31-188">Parameters aren't supported.</span></span> |
| `Static`            | <span data-ttu-id="65d31-189">Representa el componente en HTML estático.</span><span class="sxs-lookup"><span data-stu-id="65d31-189">Renders the component into static HTML.</span></span> <span data-ttu-id="65d31-190">Se admiten los parámetros.</span><span class="sxs-lookup"><span data-stu-id="65d31-190">Parameters are supported.</span></span> |

<span data-ttu-id="65d31-191">No se admite la representación de componentes de servidor desde una página HTML estática.</span><span class="sxs-lookup"><span data-stu-id="65d31-191">Rendering server components from a static HTML page isn't supported.</span></span>
 
<span data-ttu-id="65d31-192">El cliente se vuelve a conectar al servidor con el mismo estado que se usó para representarla.</span><span class="sxs-lookup"><span data-stu-id="65d31-192">The client reconnects to the server with the same state that was used to prerender the app.</span></span> <span data-ttu-id="65d31-193">Si el estado de la aplicación todavía está en la memoria, el estado del componente no se representará una vez establecida la conexión de Signalr.</span><span class="sxs-lookup"><span data-stu-id="65d31-193">If the app's state is still in memory, the component state isn't rerendered after the SignalR connection is established.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="65d31-194">Representación de componentes interactivos con estado desde vistas y páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="65d31-194">Render stateful interactive components from Razor pages and views</span></span>
 
<span data-ttu-id="65d31-195">Los componentes interactivos con estado se pueden agregar a una página o vista de Razor.</span><span class="sxs-lookup"><span data-stu-id="65d31-195">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="65d31-196">Cuando se representa la página o la vista:</span><span class="sxs-lookup"><span data-stu-id="65d31-196">When the page or view renders:</span></span>

* <span data-ttu-id="65d31-197">El componente se representará con la página o la vista.</span><span class="sxs-lookup"><span data-stu-id="65d31-197">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="65d31-198">Se pierde el estado inicial del componente usado para la representación previa.</span><span class="sxs-lookup"><span data-stu-id="65d31-198">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="65d31-199">El nuevo estado del componente se crea cuando se establece la conexión de Signalr.</span><span class="sxs-lookup"><span data-stu-id="65d31-199">New component state is created when the SignalR connection is established.</span></span>
 
<span data-ttu-id="65d31-200">La siguiente página de Razor representa un `Counter` componente:</span><span class="sxs-lookup"><span data-stu-id="65d31-200">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>
 
@(await Html.RenderComponentAsync<Counter>(RenderMode.ServerPrerendered))
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="65d31-201">Representación de componentes no interactivos desde páginas y vistas de Razor</span><span class="sxs-lookup"><span data-stu-id="65d31-201">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="65d31-202">En la siguiente página de Razor, `MyComponent` el componente se representa estáticamente con un valor inicial que se especifica mediante un formulario:</span><span class="sxs-lookup"><span data-stu-id="65d31-202">In the following Razor page, the `MyComponent` component is statically rendered with an initial value that's specified using a form:</span></span>
 
```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>
 
@(await Html.RenderComponentAsync<MyComponent>(RenderMode.Static, 
    new { InitialValue = InitialValue }))
 
@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

<span data-ttu-id="65d31-203">Dado `MyComponent` que se representa estáticamente, el componente no puede ser interactivo.</span><span class="sxs-lookup"><span data-stu-id="65d31-203">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="65d31-204">Detectar cuándo se está preprocesando la aplicación</span><span class="sxs-lookup"><span data-stu-id="65d31-204">Detect when the app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-signalr-client-for-blazor-server-side-apps"></a><span data-ttu-id="65d31-205">Configurar el cliente de Signalr para las aplicaciones del lado servidor increíblemente</span><span class="sxs-lookup"><span data-stu-id="65d31-205">Configure the SignalR client for Blazor server-side apps</span></span>
 
<span data-ttu-id="65d31-206">A veces, debe configurar el cliente de Signalr que usan las aplicaciones del lado servidor increíblemente.</span><span class="sxs-lookup"><span data-stu-id="65d31-206">Sometimes, you need to configure the SignalR client used by Blazor server-side apps.</span></span> <span data-ttu-id="65d31-207">Por ejemplo, puede que desee configurar el registro en el cliente de Signalr para diagnosticar un problema de conexión.</span><span class="sxs-lookup"><span data-stu-id="65d31-207">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>
 
<span data-ttu-id="65d31-208">Para configurar el cliente de Signalr en el archivo *pages/_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="65d31-208">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="65d31-209">Agregue un `autostart="false"` atributo a la `<script>` etiqueta para el script *increíblemente. Server. js* .</span><span class="sxs-lookup"><span data-stu-id="65d31-209">Add an `autostart="false"` attribute to the `<script>` tag for the *blazor.server.js* script.</span></span>
* <span data-ttu-id="65d31-210">Llame `Blazor.start` a y pase un objeto de configuración que especifique el generador de signalr.</span><span class="sxs-lookup"><span data-stu-id="65d31-210">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>
 
```html
<script src="_framework/blazor.server.js" autostart="false"></script>
<script>
  Blazor.start({
    configureSignalR: function (builder) {
      builder.configureLogging(2); // LogLevel.Information
    }
  });
</script>
```

## <a name="additional-resources"></a><span data-ttu-id="65d31-211">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="65d31-211">Additional resources</span></span>

* <xref:blazor/get-started>
* <xref:signalr/introduction>
