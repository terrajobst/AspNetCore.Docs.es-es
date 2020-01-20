---
title: Modelos de hospedaje de Blazor de ASP.NET Core
author: guardrex
description: Comprenda Blazor los modelos de hospedaje de webassembly y Blazor Server.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
- blazor.webassembly.js
uid: blazor/hosting-models
ms.openlocfilehash: c9521acf40317c90d1197660bfa516710263cfc9
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2020
ms.locfileid: "76160046"
---
# <a name="aspnet-core-opno-locblazor-hosting-models"></a><span data-ttu-id="3f666-103">Modelos de hospedaje de Blazor de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f666-103">ASP.NET Core Blazor hosting models</span></span>

<span data-ttu-id="3f666-104">Por [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="3f666-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor<span data-ttu-id="3f666-105"> es un marco de trabajo web diseñado para ejecutar el lado cliente en el explorador en un entorno de tiempo de ejecución .NET basado en [Webassembly](https://webassembly.org/)( *Blazor webassembly*) o en el servidor en ASP.net Core ( *Blazor Server*).</span><span class="sxs-lookup"><span data-stu-id="3f666-105"> is a web framework designed to run client-side in the browser on a [WebAssembly](https://webassembly.org/)-based .NET runtime (*Blazor WebAssembly*) or server-side in ASP.NET Core (*Blazor Server*).</span></span> <span data-ttu-id="3f666-106">Independientemente del modelo de hospedaje, los modelos de aplicación y de componente *son los mismos*.</span><span class="sxs-lookup"><span data-stu-id="3f666-106">Regardless of the hosting model, the app and component models *are the same*.</span></span>

<span data-ttu-id="3f666-107">Para crear un proyecto para los modelos de hospedaje descritos en este artículo, consulte <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="3f666-107">To create a project for the hosting models described in this article, see <xref:blazor/get-started>.</span></span>

## <a name="opno-locblazor-webassembly"></a>Blazor<span data-ttu-id="3f666-108"> WebAssembly</span><span class="sxs-lookup"><span data-stu-id="3f666-108"> WebAssembly</span></span>

<span data-ttu-id="3f666-109">El modelo de hospedaje principal de Blazor se está ejecutando en el lado cliente en el explorador de webassembly.</span><span class="sxs-lookup"><span data-stu-id="3f666-109">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="3f666-110">La aplicación Blazor, sus dependencias y el entorno de ejecución de .NET se descargan en el explorador.</span><span class="sxs-lookup"><span data-stu-id="3f666-110">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="3f666-111">La aplicación se ejecuta directamente en el subproceso de interfaz de usuario del explorador.</span><span class="sxs-lookup"><span data-stu-id="3f666-111">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="3f666-112">Las actualizaciones de la interfaz de usuario y el control de eventos se producen dentro del mismo proceso.</span><span class="sxs-lookup"><span data-stu-id="3f666-112">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="3f666-113">Los recursos de la aplicación se implementan como archivos estáticos en un servidor web o servicio capaz de servir contenido estático a los clientes.</span><span class="sxs-lookup"><span data-stu-id="3f666-113">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![[! Operador. Webassembly NO-LOC (increíble)]<span data-ttu-id="3f666-114">: [! Operador. NO-LOC (increíble)]: la aplicación se ejecuta en un subproceso de interfaz de usuario dentro del explorador.</span><span class="sxs-lookup"><span data-stu-id="3f666-114"> WebAssembly: The Blazor app runs on a UI thread inside the browser.</span></span>](hosting-models/_static/blazor-webassembly.png)

<span data-ttu-id="3f666-115">Para crear una aplicación Blazor mediante el modelo de hospedaje del lado cliente, use la plantilla de **aplicación de WebassembleBlazor** ([dotnet New blazorwasm](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="3f666-115">To create a Blazor app using the client-side hosting model, use the **Blazor WebAssembly App** template ([dotnet new blazorwasm](/dotnet/core/tools/dotnet-new)).</span></span>

<span data-ttu-id="3f666-116">Después de seleccionar la plantilla de **aplicaciónBlazor Webassembly** , tiene la opción de configurar la aplicación para usar un back-end de ASP.net Core activando la casilla **ASP.net Core hospedado** ([dotnet New blazorwasm--Hosted](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="3f666-116">After selecting the **Blazor WebAssembly App** template, you have the option of configuring the app to use an ASP.NET Core backend by selecting the **ASP.NET Core hosted** check box ([dotnet new blazorwasm --hosted](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="3f666-117">La aplicación ASP.NET Core sirve a la aplicación de Blazor a los clientes.</span><span class="sxs-lookup"><span data-stu-id="3f666-117">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="3f666-118">La aplicación Blazor webassembly puede interactuar con el servidor a través de la red mediante llamadas API Web o [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="3f666-118">The Blazor WebAssembly app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="3f666-119">Las plantillas incluyen el script de `blazor.webassembly.js` que controla:</span><span class="sxs-lookup"><span data-stu-id="3f666-119">The templates include the `blazor.webassembly.js` script that handles:</span></span>

* <span data-ttu-id="3f666-120">Descarga del tiempo de ejecución de .NET, la aplicación y las dependencias de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3f666-120">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="3f666-121">Inicialización del tiempo de ejecución para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3f666-121">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="3f666-122">El modelo de hospedaje de webassembly de Blazor ofrece varias ventajas:</span><span class="sxs-lookup"><span data-stu-id="3f666-122">The Blazor WebAssembly hosting model offers several benefits:</span></span>

* <span data-ttu-id="3f666-123">No hay ninguna dependencia de servidor .NET.</span><span class="sxs-lookup"><span data-stu-id="3f666-123">There's no .NET server-side dependency.</span></span> <span data-ttu-id="3f666-124">La aplicación funciona totalmente después de descargarla en el cliente.</span><span class="sxs-lookup"><span data-stu-id="3f666-124">The app is fully functioning after downloaded to the client.</span></span>
* <span data-ttu-id="3f666-125">Los recursos de cliente y las capacidades se aprovechan por completo.</span><span class="sxs-lookup"><span data-stu-id="3f666-125">Client resources and capabilities are fully leveraged.</span></span>
* <span data-ttu-id="3f666-126">El trabajo se descarga del servidor al cliente.</span><span class="sxs-lookup"><span data-stu-id="3f666-126">Work is offloaded from the server to the client.</span></span>
* <span data-ttu-id="3f666-127">No es necesario un servidor Web ASP.NET Core para hospedar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3f666-127">An ASP.NET Core web server isn't required to host the app.</span></span> <span data-ttu-id="3f666-128">Los escenarios de implementación sin servidor son posibles (por ejemplo, para dar servicio a la aplicación desde una red CDN).</span><span class="sxs-lookup"><span data-stu-id="3f666-128">Serverless deployment scenarios are possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="3f666-129">Hay desventajas para Blazor hospedaje de webassembly:</span><span class="sxs-lookup"><span data-stu-id="3f666-129">There are downsides to Blazor WebAssembly hosting:</span></span>

* <span data-ttu-id="3f666-130">La aplicación está restringida a las capacidades del explorador.</span><span class="sxs-lookup"><span data-stu-id="3f666-130">The app is restricted to the capabilities of the browser.</span></span>
* <span data-ttu-id="3f666-131">Se requiere hardware y software de cliente compatible (por ejemplo, compatibilidad con webassembly).</span><span class="sxs-lookup"><span data-stu-id="3f666-131">Capable client hardware and software (for example, WebAssembly support) is required.</span></span>
* <span data-ttu-id="3f666-132">El tamaño de descarga es mayor y las aplicaciones tardan más tiempo en cargarse.</span><span class="sxs-lookup"><span data-stu-id="3f666-132">Download size is larger, and apps take longer to load.</span></span>
* <span data-ttu-id="3f666-133">La compatibilidad con las herramientas y el tiempo de ejecución de .NET es menos madura.</span><span class="sxs-lookup"><span data-stu-id="3f666-133">.NET runtime and tooling support is less mature.</span></span> <span data-ttu-id="3f666-134">Por ejemplo, existen limitaciones en [.net Standard](/dotnet/standard/net-standard) la compatibilidad y la depuración.</span><span class="sxs-lookup"><span data-stu-id="3f666-134">For example, limitations exist in [.NET Standard](/dotnet/standard/net-standard) support and debugging.</span></span>

## <a name="opno-locblazor-server"></a><span data-ttu-id="3f666-135">Servidor de Blazor</span><span class="sxs-lookup"><span data-stu-id="3f666-135">Blazor Server</span></span>

<span data-ttu-id="3f666-136">Con el modelo de hospedaje de servidor de Blazor, la aplicación se ejecuta en el servidor desde una aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3f666-136">With the Blazor Server hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="3f666-137">Las actualizaciones de la interfaz de usuario, el control de eventos y las llamadas de JavaScript se controlan mediante una conexión [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="3f666-137">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![El explorador interactúa con la aplicación (hospedada dentro de una aplicación ASP.NET Core) en el servidor a través de un [! Operador. Conexión NO-LOC (Signalr)].](hosting-models/_static/blazor-server.png)

<span data-ttu-id="3f666-139">Para crear una aplicación de Blazor con el modelo de hospedaje de Blazor Server, use la plantilla de **aplicación de servidor** de ASP.net CoreBlazor ([dotnet New blazorserver](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="3f666-139">To create a Blazor app using the Blazor Server hosting model, use the ASP.NET Core **Blazor Server App** template ([dotnet new blazorserver](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="3f666-140">La aplicación ASP.NET Core hospeda la aplicación de Blazor Server y crea el punto de conexión de SignalR donde se conectan los clientes.</span><span class="sxs-lookup"><span data-stu-id="3f666-140">The ASP.NET Core app hosts the Blazor Server app and creates the SignalR endpoint where clients connect.</span></span>

<span data-ttu-id="3f666-141">La aplicación ASP.NET Core hace referencia a la clase `Startup` de la aplicación que se va a agregar:</span><span class="sxs-lookup"><span data-stu-id="3f666-141">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="3f666-142">Servicios del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="3f666-142">Server-side services.</span></span>
* <span data-ttu-id="3f666-143">La aplicación a la canalización de control de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="3f666-143">The app to the request handling pipeline.</span></span>

<span data-ttu-id="3f666-144">El script de `blazor.server.js`&dagger; establece la conexión del cliente.</span><span class="sxs-lookup"><span data-stu-id="3f666-144">The `blazor.server.js` script&dagger; establishes the client connection.</span></span> <span data-ttu-id="3f666-145">Es responsabilidad de la aplicación conservar y restaurar el estado de la aplicación según sea necesario (por ejemplo, en caso de que se pierda una conexión de red).</span><span class="sxs-lookup"><span data-stu-id="3f666-145">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="3f666-146">El modelo de hospedaje de Blazor Server ofrece varias ventajas:</span><span class="sxs-lookup"><span data-stu-id="3f666-146">The Blazor Server hosting model offers several benefits:</span></span>

* <span data-ttu-id="3f666-147">El tamaño de la descarga es significativamente menor que una aplicación Blazor webassembly y la aplicación se carga mucho más rápido.</span><span class="sxs-lookup"><span data-stu-id="3f666-147">Download size is significantly smaller than a Blazor WebAssembly app, and the app loads much faster.</span></span>
* <span data-ttu-id="3f666-148">La aplicación aprovecha al máximo las funcionalidades del servidor, incluido el uso de cualquier API compatible con .NET Core.</span><span class="sxs-lookup"><span data-stu-id="3f666-148">The app takes full advantage of server capabilities, including use of any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="3f666-149">.NET Core en el servidor se usa para ejecutar la aplicación, por lo que las herramientas de .NET existentes, como la depuración, funcionan según lo previsto.</span><span class="sxs-lookup"><span data-stu-id="3f666-149">.NET Core on the server is used to run the app, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="3f666-150">Se admiten clientes ligeros.</span><span class="sxs-lookup"><span data-stu-id="3f666-150">Thin clients are supported.</span></span> <span data-ttu-id="3f666-151">Por ejemplo, las aplicaciones de Blazor Server funcionan con los exploradores que no admiten webassembly y en los dispositivos con restricción de recursos.</span><span class="sxs-lookup"><span data-stu-id="3f666-151">For example, Blazor Server apps work with browsers that don't support WebAssembly and on resource-constrained devices.</span></span>
* <span data-ttu-id="3f666-152">La base de .NET/C# Code de la aplicación, incluido el código de componente de la aplicación, no se sirve a los clientes.</span><span class="sxs-lookup"><span data-stu-id="3f666-152">The app's .NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="3f666-153">Hay desventajas para Blazor servidor que hospeda:</span><span class="sxs-lookup"><span data-stu-id="3f666-153">There are downsides to Blazor Server hosting:</span></span>

* <span data-ttu-id="3f666-154">Normalmente existe una mayor latencia.</span><span class="sxs-lookup"><span data-stu-id="3f666-154">Higher latency usually exists.</span></span> <span data-ttu-id="3f666-155">Cada interacción del usuario implica un salto de red.</span><span class="sxs-lookup"><span data-stu-id="3f666-155">Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="3f666-156">No hay compatibilidad sin conexión.</span><span class="sxs-lookup"><span data-stu-id="3f666-156">There's no offline support.</span></span> <span data-ttu-id="3f666-157">Si se produce un error en la conexión de cliente, la aplicación deja de funcionar.</span><span class="sxs-lookup"><span data-stu-id="3f666-157">If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="3f666-158">La escalabilidad es desafiante para aplicaciones con muchos usuarios.</span><span class="sxs-lookup"><span data-stu-id="3f666-158">Scalability is challenging for apps with many users.</span></span> <span data-ttu-id="3f666-159">El servidor debe administrar varias conexiones de cliente y controlar el estado del cliente.</span><span class="sxs-lookup"><span data-stu-id="3f666-159">The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="3f666-160">Se requiere un servidor ASP.NET Core para atender la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3f666-160">An ASP.NET Core server is required to serve the app.</span></span> <span data-ttu-id="3f666-161">Los escenarios de implementación sin servidor no son posibles (por ejemplo, para dar servicio a la aplicación desde una red CDN).</span><span class="sxs-lookup"><span data-stu-id="3f666-161">Serverless deployment scenarios aren't possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="3f666-162">&dagger;el script de `blazor.server.js` se sirve desde un recurso incrustado en el marco de ASP.NET Core Shared.</span><span class="sxs-lookup"><span data-stu-id="3f666-162">&dagger;The `blazor.server.js` script is served from an embedded resource in the ASP.NET Core shared framework.</span></span>

### <a name="comparison-to-server-rendered-ui"></a><span data-ttu-id="3f666-163">Comparación con la interfaz de usuario representada por el servidor</span><span class="sxs-lookup"><span data-stu-id="3f666-163">Comparison to server-rendered UI</span></span>

<span data-ttu-id="3f666-164">Una manera de comprender las aplicaciones de Blazor Server es comprender cómo difiere de los modelos tradicionales para representar la interfaz de usuario en ASP.NET Core aplicaciones con vistas de Razor o Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="3f666-164">One way to understand Blazor Server apps is to understand how it differs from traditional models for rendering UI in ASP.NET Core apps using Razor views or Razor Pages.</span></span> <span data-ttu-id="3f666-165">Ambos modelos usan el lenguaje Razor para describir el contenido HTML, pero difieren significativamente en el modo en que se representa el marcado.</span><span class="sxs-lookup"><span data-stu-id="3f666-165">Both models use the Razor language to describe HTML content, but they significantly differ in how markup is rendered.</span></span>

<span data-ttu-id="3f666-166">Cuando se representa una página o vista de Razor, cada línea de código Razor emite HTML en forma de texto.</span><span class="sxs-lookup"><span data-stu-id="3f666-166">When a Razor Page or view is rendered, every line of Razor code emits HTML in text form.</span></span> <span data-ttu-id="3f666-167">Después de la representación, el servidor desecha la instancia de la página o la vista, incluido cualquier Estado que se haya producido.</span><span class="sxs-lookup"><span data-stu-id="3f666-167">After rendering, the server disposes of the page or view instance, including any state that was produced.</span></span> <span data-ttu-id="3f666-168">Cuando se produce otra solicitud para la página, por ejemplo, cuando se produce un error en la validación del servidor y se muestra el Resumen de la validación:</span><span class="sxs-lookup"><span data-stu-id="3f666-168">When another request for the page occurs, for instance when server validation fails and the validation summary is displayed:</span></span>

* <span data-ttu-id="3f666-169">La página completa se vuelve a representar en el texto HTML de nuevo.</span><span class="sxs-lookup"><span data-stu-id="3f666-169">The entire page is rerendered to HTML text again.</span></span>
* <span data-ttu-id="3f666-170">La página se envía al cliente.</span><span class="sxs-lookup"><span data-stu-id="3f666-170">The page is sent to the client.</span></span>

<span data-ttu-id="3f666-171">Una aplicación Blazor se compone de elementos reutilizables de la interfaz de usuario denominada *componentes*.</span><span class="sxs-lookup"><span data-stu-id="3f666-171">A Blazor app is composed of reusable elements of UI called *components*.</span></span> <span data-ttu-id="3f666-172">Un componente contiene C# código, marcado y otros componentes.</span><span class="sxs-lookup"><span data-stu-id="3f666-172">A component contains C# code, markup, and other components.</span></span> <span data-ttu-id="3f666-173">Cuando se representa un componente, Blazor genera un gráfico de los componentes incluidos de forma similar a una Document Object Model HTML o XML (DOM).</span><span class="sxs-lookup"><span data-stu-id="3f666-173">When a component is rendered, Blazor produces a graph of the included components similar to an HTML or XML Document Object Model (DOM).</span></span> <span data-ttu-id="3f666-174">Este gráfico incluye el estado del componente contenido en propiedades y campos.</span><span class="sxs-lookup"><span data-stu-id="3f666-174">This graph includes component state held in properties and fields.</span></span> Blazor<span data-ttu-id="3f666-175"> evalúa el gráfico de componentes para generar una representación binaria del marcado.</span><span class="sxs-lookup"><span data-stu-id="3f666-175"> evaluates the component graph to produce a binary representation of the markup.</span></span> <span data-ttu-id="3f666-176">El formato binario puede ser:</span><span class="sxs-lookup"><span data-stu-id="3f666-176">The binary format can be:</span></span>

* <span data-ttu-id="3f666-177">Se convierte en texto HTML (durante la representación previa).</span><span class="sxs-lookup"><span data-stu-id="3f666-177">Turned into HTML text (during prerendering).</span></span>
* <span data-ttu-id="3f666-178">Se usa para actualizar de forma eficaz el marcado durante la representación normal.</span><span class="sxs-lookup"><span data-stu-id="3f666-178">Used to efficiently update the markup during regular rendering.</span></span>

<span data-ttu-id="3f666-179">Una actualización de la interfaz de usuario en Blazor se desencadena mediante:</span><span class="sxs-lookup"><span data-stu-id="3f666-179">A UI update in Blazor is triggered by:</span></span>

* <span data-ttu-id="3f666-180">Interacción con el usuario, como la selección de un botón.</span><span class="sxs-lookup"><span data-stu-id="3f666-180">User interaction, such as selecting a button.</span></span>
* <span data-ttu-id="3f666-181">Desencadenadores de aplicaciones, como un temporizador.</span><span class="sxs-lookup"><span data-stu-id="3f666-181">App triggers, such as a timer.</span></span>

<span data-ttu-id="3f666-182">El gráfico se representará y se calculará *una diferencia de interfaz de* usuario (diferencia).</span><span class="sxs-lookup"><span data-stu-id="3f666-182">The graph is rerendered, and a UI *diff* (difference) is calculated.</span></span> <span data-ttu-id="3f666-183">Esta diferencia es el conjunto más pequeño de ediciones DOM necesarias para actualizar la interfaz de usuario en el cliente.</span><span class="sxs-lookup"><span data-stu-id="3f666-183">This diff is the smallest set of DOM edits required to update the UI on the client.</span></span> <span data-ttu-id="3f666-184">La diferencia se envía al cliente en un formato binario y se aplica mediante el explorador.</span><span class="sxs-lookup"><span data-stu-id="3f666-184">The diff is sent to the client in a binary format and applied by the browser.</span></span>

<span data-ttu-id="3f666-185">Un componente se desecha una vez que el usuario sale de él en el cliente.</span><span class="sxs-lookup"><span data-stu-id="3f666-185">A component is disposed after the user navigates away from it on the client.</span></span> <span data-ttu-id="3f666-186">Mientras que un usuario interactúa con un componente, el estado del componente (servicios, recursos) debe mantenerse en la memoria del servidor.</span><span class="sxs-lookup"><span data-stu-id="3f666-186">While a user is interacting with a component, the component's state (services, resources) must be held in the server's memory.</span></span> <span data-ttu-id="3f666-187">Dado que el servidor puede mantener el estado de muchos componentes al mismo tiempo, el agotamiento de la memoria es un problema que se debe solucionar.</span><span class="sxs-lookup"><span data-stu-id="3f666-187">Because the state of many components might be maintained by the server concurrently, memory exhaustion is a concern that must be addressed.</span></span> <span data-ttu-id="3f666-188">Para obtener instrucciones sobre cómo crear una aplicación de Blazor Server para garantizar el mejor uso de la memoria del servidor, consulte <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="3f666-188">For guidance on how to author a Blazor Server app to ensure the best use of server memory, see <xref:security/blazor/server>.</span></span>

### <a name="integrate-razor-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="3f666-189">Integre componentes de Razor en aplicaciones de Razor Pages y MVC</span><span class="sxs-lookup"><span data-stu-id="3f666-189">Integrate Razor components into Razor Pages and MVC apps</span></span>

#### <a name="use-components-in-pages-and-views"></a><span data-ttu-id="3f666-190">Usar componentes en páginas y vistas</span><span class="sxs-lookup"><span data-stu-id="3f666-190">Use components in pages and views</span></span>

<span data-ttu-id="3f666-191">Una aplicación Razor Pages o MVC existente puede integrar los componentes de Razor en páginas y vistas:</span><span class="sxs-lookup"><span data-stu-id="3f666-191">An existing Razor Pages or MVC app can integrate Razor components into pages and views:</span></span>

1. <span data-ttu-id="3f666-192">En el archivo de diseño de la aplicación ( *_Layout. cshtml*):</span><span class="sxs-lookup"><span data-stu-id="3f666-192">In the app's layout file (*_Layout.cshtml*):</span></span>

   * <span data-ttu-id="3f666-193">Agregue la siguiente etiqueta de `<base>` al elemento `<head>`:</span><span class="sxs-lookup"><span data-stu-id="3f666-193">Add the following `<base>` tag to the `<head>` element:</span></span>

     ```html
     <base href="~/" />
     ```

     <span data-ttu-id="3f666-194">El valor `href` (la *ruta de acceso base*de la aplicación) en el ejemplo anterior supone que la aplicación reside en la ruta de acceso URL raíz (`/`).</span><span class="sxs-lookup"><span data-stu-id="3f666-194">The `href` value (the *app base path*) in the preceding example assumes that the app resides at the root URL path (`/`).</span></span> <span data-ttu-id="3f666-195">Si la aplicación es una aplicación secundaria, siga las instrucciones de la sección *ruta de acceso base* de la aplicación del artículo <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="3f666-195">If the app is a sub-application, follow the guidance in the *App base path* section of the <xref:host-and-deploy/blazor/index#app-base-path> article.</span></span>

     <span data-ttu-id="3f666-196">El archivo *_Layout. cshtml* se encuentra en la carpeta *páginas/compartidas* de una aplicación de Razor pages o en una carpeta de *vistas/compartidas* en una aplicación MVC.</span><span class="sxs-lookup"><span data-stu-id="3f666-196">The *_Layout.cshtml* file is located in the *Pages/Shared* folder in a Razor Pages app or *Views/Shared* folder in an MVC app.</span></span>

   * <span data-ttu-id="3f666-197">Agregue una etiqueta de `<script>` para el script *increíblemente. Server. js* dentro de la etiqueta de cierre `</body>`:</span><span class="sxs-lookup"><span data-stu-id="3f666-197">Add a `<script>` tag for the *blazor.server.js* script inside of the closing `</body>` tag:</span></span>

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     <span data-ttu-id="3f666-198">El marco de trabajo agrega el script *extraordinaria. Server. js* a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3f666-198">The framework adds the *blazor.server.js* script to the app.</span></span> <span data-ttu-id="3f666-199">No es necesario agregar manualmente el script a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3f666-199">There's no need to manually add the script to the app.</span></span>

1. <span data-ttu-id="3f666-200">Agregue un archivo *_Imports. Razor* a la carpeta raíz del proyecto con el siguiente contenido (cambie el último espacio de nombres, `MyAppNamespace`, al espacio de nombres de la aplicación):</span><span class="sxs-lookup"><span data-stu-id="3f666-200">Add an *_Imports.razor* file to the root folder of the project with the following content (change the last namespace, `MyAppNamespace`, to the namespace of the app):</span></span>

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

1. <span data-ttu-id="3f666-201">En `Startup.ConfigureServices`, agregue el servicio servidor de Blazor:</span><span class="sxs-lookup"><span data-stu-id="3f666-201">In `Startup.ConfigureServices`, add the Blazor Server service:</span></span>

   ```csharp
   services.AddServerSideBlazor();
   ```

1. <span data-ttu-id="3f666-202">En `Startup.Configure`, agregue el punto de conexión del concentrador de Blazor a `app.UseEndpoints`:</span><span class="sxs-lookup"><span data-stu-id="3f666-202">In `Startup.Configure`, add the Blazor Hub endpoint to `app.UseEndpoints`:</span></span>

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. <span data-ttu-id="3f666-203">Integre los componentes en cualquier página o vista.</span><span class="sxs-lookup"><span data-stu-id="3f666-203">Integrate components into any page or view.</span></span> <span data-ttu-id="3f666-204">Para más información, consulte la sección *integración de componentes en Razor pages y aplicaciones MVC* del artículo <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="3f666-204">For more information, see the *Integrate components into Razor Pages and MVC apps* section of the <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps> article.</span></span>

#### <a name="use-routable-components-in-a-razor-pages-app"></a><span data-ttu-id="3f666-205">Uso de componentes enrutables en una aplicación Razor Pages</span><span class="sxs-lookup"><span data-stu-id="3f666-205">Use routable components in a Razor Pages app</span></span>

<span data-ttu-id="3f666-206">Para admitir componentes de Razor enrutables en Razor Pages aplicaciones:</span><span class="sxs-lookup"><span data-stu-id="3f666-206">To support routable Razor components in Razor Pages apps:</span></span>

1. <span data-ttu-id="3f666-207">Siga las instrucciones de la sección [uso de componentes en páginas y vistas](#use-components-in-pages-and-views) .</span><span class="sxs-lookup"><span data-stu-id="3f666-207">Follow the guidance in the [Use components in pages and views](#use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="3f666-208">Agregue un archivo *app. Razor* a la raíz del proyecto con el siguiente contenido:</span><span class="sxs-lookup"><span data-stu-id="3f666-208">Add an *App.razor* file to the root of the project with the following content:</span></span>

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

1. <span data-ttu-id="3f666-209">Agregue un archivo *_Host. cshtml* a la carpeta *pages* con el siguiente contenido:</span><span class="sxs-lookup"><span data-stu-id="3f666-209">Add a *_Host.cshtml* file to the *Pages* folder with the following content:</span></span>

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="3f666-210">Los componentes de usan el archivo Shared *_Layout. cshtml* para su diseño.</span><span class="sxs-lookup"><span data-stu-id="3f666-210">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="3f666-211">Agregue una ruta de prioridad baja para la página *_Host. cshtml* a la configuración del punto de conexión en `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="3f666-211">Add a low-priority route for the *_Host.cshtml* page to endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. <span data-ttu-id="3f666-212">Agregue componentes enrutables a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3f666-212">Add routable components to the app.</span></span> <span data-ttu-id="3f666-213">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="3f666-213">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="3f666-214">Al usar una carpeta personalizada para contener los componentes de la aplicación, agregue el espacio de nombres que representa la carpeta al archivo *pages/_ViewImports. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="3f666-214">When using a custom folder to hold the app's components, add the namespace representing the folder to the *Pages/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="3f666-215">Para obtener más información, vea <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="3f666-215">For more information, see <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span></span>

#### <a name="use-routable-components-in-an-mvc-app"></a><span data-ttu-id="3f666-216">Uso de componentes enrutables en una aplicación MVC</span><span class="sxs-lookup"><span data-stu-id="3f666-216">Use routable components in an MVC app</span></span>

<span data-ttu-id="3f666-217">Para admitir componentes de Razor enrutables en aplicaciones MVC:</span><span class="sxs-lookup"><span data-stu-id="3f666-217">To support routable Razor components in MVC apps:</span></span>

1. <span data-ttu-id="3f666-218">Siga las instrucciones de la sección [uso de componentes en páginas y vistas](#use-components-in-pages-and-views) .</span><span class="sxs-lookup"><span data-stu-id="3f666-218">Follow the guidance in the [Use components in pages and views](#use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="3f666-219">Agregue un archivo *app. Razor* a la raíz del proyecto con el siguiente contenido:</span><span class="sxs-lookup"><span data-stu-id="3f666-219">Add an *App.razor* file to the root of the project with the following content:</span></span>

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

1. <span data-ttu-id="3f666-220">Agregue un archivo *_Host. cshtml* a la carpeta *views/Home* con el siguiente contenido:</span><span class="sxs-lookup"><span data-stu-id="3f666-220">Add a *_Host.cshtml* file to the *Views/Home* folder with the following content:</span></span>

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="3f666-221">Los componentes de usan el archivo Shared *_Layout. cshtml* para su diseño.</span><span class="sxs-lookup"><span data-stu-id="3f666-221">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="3f666-222">Agregue una acción al controlador Home:</span><span class="sxs-lookup"><span data-stu-id="3f666-222">Add an action to the Home controller:</span></span>

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. <span data-ttu-id="3f666-223">Agregue una ruta de prioridad baja para la acción de controlador que devuelve la vista *_Host. cshtml* a la configuración del punto de conexión en `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="3f666-223">Add a low-priority route for the controller action that returns the *_Host.cshtml* view to the endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. <span data-ttu-id="3f666-224">Cree una carpeta de *páginas* y agregue componentes enrutables a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3f666-224">Create a *Pages* folder and add routable components to the app.</span></span> <span data-ttu-id="3f666-225">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="3f666-225">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="3f666-226">Al usar una carpeta personalizada para contener los componentes de la aplicación, agregue el espacio de nombres que representa la carpeta al archivo *views/_ViewImports. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="3f666-226">When using a custom folder to hold the app's components, add the namespace representing the folder to the *Views/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="3f666-227">Para obtener más información, vea <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="3f666-227">For more information, see <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span></span>

### <a name="circuits"></a><span data-ttu-id="3f666-228">Circuitos</span><span class="sxs-lookup"><span data-stu-id="3f666-228">Circuits</span></span>

<span data-ttu-id="3f666-229">Una aplicación Blazor Server se basa en [ASP.NET Core SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="3f666-229">A Blazor Server app is built on top of [ASP.NET Core SignalR](xref:signalr/introduction).</span></span> <span data-ttu-id="3f666-230">Cada cliente se comunica con el servidor a través de una o varias conexiones SignalR llamadas un *circuito*.</span><span class="sxs-lookup"><span data-stu-id="3f666-230">Each client communicates to the server over one or more SignalR connections called a *circuit*.</span></span> <span data-ttu-id="3f666-231">Un circuito es la abstracción de Blazoren SignalR conexiones que pueden tolerar interrupciones de red temporales.</span><span class="sxs-lookup"><span data-stu-id="3f666-231">A circuit is Blazor's abstraction over SignalR connections that can tolerate temporary network interruptions.</span></span> <span data-ttu-id="3f666-232">Cuando un cliente de Blazor ve que la conexión de SignalR está desconectada, intenta volver a conectarse al servidor mediante una nueva conexión de SignalR.</span><span class="sxs-lookup"><span data-stu-id="3f666-232">When a Blazor client sees that the SignalR connection is disconnected, it attempts to reconnect to the server using a new SignalR connection.</span></span>

<span data-ttu-id="3f666-233">Cada pantalla del explorador (pestaña del explorador o iframe) que está conectada a una aplicación de Blazor Server usa una conexión SignalR.</span><span class="sxs-lookup"><span data-stu-id="3f666-233">Each browser screen (browser tab or iframe) that is connected to a Blazor Server app uses a SignalR connection.</span></span> <span data-ttu-id="3f666-234">Esta es otra diferencia importante en comparación con las aplicaciones típicas representadas por el servidor.</span><span class="sxs-lookup"><span data-stu-id="3f666-234">This is yet another important distinction compared to typical server-rendered apps.</span></span> <span data-ttu-id="3f666-235">En una aplicación representada por el servidor, la apertura de la misma aplicación en varias pantallas del explorador normalmente no se traduce en demandas de recursos adicionales en el servidor.</span><span class="sxs-lookup"><span data-stu-id="3f666-235">In a server-rendered app, opening the same app in multiple browser screens typically doesn't translate into additional resource demands on the server.</span></span> <span data-ttu-id="3f666-236">En una aplicación de Blazor Server, cada pantalla del explorador requiere un circuito independiente e instancias independientes del estado del componente que administra el servidor.</span><span class="sxs-lookup"><span data-stu-id="3f666-236">In a Blazor Server app, each browser screen requires a separate circuit and separate instances of component state to be managed by the server.</span></span>

Blazor<span data-ttu-id="3f666-237"> considera cerrar una pestaña del explorador o desplazarse a una dirección URL externa como terminación *correcta* .</span><span class="sxs-lookup"><span data-stu-id="3f666-237"> considers closing a browser tab or navigating to an external URL a *graceful* termination.</span></span> <span data-ttu-id="3f666-238">En el caso de una terminación correcta, el circuito y los recursos asociados se liberan inmediatamente.</span><span class="sxs-lookup"><span data-stu-id="3f666-238">In the event of a graceful termination, the circuit and associated resources are immediately released.</span></span> <span data-ttu-id="3f666-239">Un cliente también puede desconectarse de manera no correcta, por ejemplo, debido a una interrupción de la red.</span><span class="sxs-lookup"><span data-stu-id="3f666-239">A client may also disconnect non-gracefully, for instance due to a network interruption.</span></span> Blazor<span data-ttu-id="3f666-240"> Server almacena los circuitos desconectados durante un intervalo configurable para permitir que el cliente vuelva a conectarse.</span><span class="sxs-lookup"><span data-stu-id="3f666-240"> Server stores disconnected circuits for a configurable interval to allow the client to reconnect.</span></span> <span data-ttu-id="3f666-241">Para obtener más información, consulte la sección [reconexión al mismo servidor](#reconnection-to-the-same-server) .</span><span class="sxs-lookup"><span data-stu-id="3f666-241">For more information, see the [Reconnection to the same server](#reconnection-to-the-same-server) section.</span></span>

### <a name="ui-latency"></a><span data-ttu-id="3f666-242">Latencia de IU</span><span class="sxs-lookup"><span data-stu-id="3f666-242">UI Latency</span></span>

<span data-ttu-id="3f666-243">La latencia de la interfaz de usuario es el tiempo que tarda una acción iniciada en el momento en que se actualiza la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="3f666-243">UI latency is the time it takes from an initiated action to the time the UI is updated.</span></span> <span data-ttu-id="3f666-244">Los valores más pequeños para la latencia de la interfaz de usuario son imprescindibles para que una aplicación pueda responder a un usuario.</span><span class="sxs-lookup"><span data-stu-id="3f666-244">Smaller values for UI latency are imperative for an app to feel responsive to a user.</span></span> <span data-ttu-id="3f666-245">En una aplicación de Blazor Server, cada acción se envía al servidor, se procesa y se devuelve una diferencia de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="3f666-245">In a Blazor Server app, each action is sent to the server, processed, and a UI diff is sent back.</span></span> <span data-ttu-id="3f666-246">Por lo tanto, la latencia de la interfaz de usuario es la suma de la latencia de red y la latencia del servidor en el procesamiento de la acción.</span><span class="sxs-lookup"><span data-stu-id="3f666-246">Consequently, UI latency is the sum of network latency and the server latency in processing the action.</span></span>

<span data-ttu-id="3f666-247">En el caso de una aplicación de línea de negocio que está limitada a una red corporativa privada, el efecto en las percepciones de usuario de latencia debido a la latencia de red suele ser imperceptibles.</span><span class="sxs-lookup"><span data-stu-id="3f666-247">For a line of business app that's limited to a private corporate network, the effect on user perceptions of latency due to network latency are usually imperceptible.</span></span> <span data-ttu-id="3f666-248">En el caso de una aplicación implementada a través de Internet, la latencia puede ser apreciable para los usuarios, especialmente si los usuarios están ampliamente distribuidos geográficamente.</span><span class="sxs-lookup"><span data-stu-id="3f666-248">For an app deployed over the Internet, latency may become noticeable to users, particularly if users are widely distributed geographically.</span></span>

<span data-ttu-id="3f666-249">El uso de memoria también puede contribuir a la latencia de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3f666-249">Memory usage can also contribute to app latency.</span></span> <span data-ttu-id="3f666-250">El aumento del uso de memoria da como resultado la recolección frecuente de elementos no utilizados o la paginación de memoria en el disco, y ambos degradan el rendimiento de la aplicación y, por consiguiente, aumentan la latencia</span><span class="sxs-lookup"><span data-stu-id="3f666-250">Increased memory usage results in frequent garbage collection or paging memory to disk, both of which degrade app performance and consequently increase UI latency.</span></span> <span data-ttu-id="3f666-251">Para obtener más información, vea <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="3f666-251">For more information, see <xref:security/blazor/server>.</span></span>

<span data-ttu-id="3f666-252">las aplicaciones de Blazor Server deben optimizarse para minimizar la latencia de la interfaz de usuario al reducir la latencia de red y el uso de memoria.</span><span class="sxs-lookup"><span data-stu-id="3f666-252">Blazor Server apps should be optimized to minimize UI latency by reducing network latency and memory usage.</span></span> <span data-ttu-id="3f666-253">Para obtener información acerca de cómo medir la latencia de red, consulte <xref:host-and-deploy/blazor/server#measure-network-latency>.</span><span class="sxs-lookup"><span data-stu-id="3f666-253">For an approach to measuring network latency, see <xref:host-and-deploy/blazor/server#measure-network-latency>.</span></span> <span data-ttu-id="3f666-254">Para obtener más información sobre SignalR y Blazor, vea:</span><span class="sxs-lookup"><span data-stu-id="3f666-254">For more information on SignalR and Blazor, see:</span></span>

* <xref:host-and-deploy/blazor/server>
* <xref:security/blazor/server>

### <a name="connection-to-the-server"></a><span data-ttu-id="3f666-255">Conexión al servidor</span><span class="sxs-lookup"><span data-stu-id="3f666-255">Connection to the server</span></span>

<span data-ttu-id="3f666-256">las aplicaciones de Blazor Server requieren una conexión de SignalR activa con el servidor.</span><span class="sxs-lookup"><span data-stu-id="3f666-256">Blazor Server apps require an active SignalR connection to the server.</span></span> <span data-ttu-id="3f666-257">Si se pierde la conexión, la aplicación intenta volver a conectarse al servidor.</span><span class="sxs-lookup"><span data-stu-id="3f666-257">If the connection is lost, the app attempts to reconnect to the server.</span></span> <span data-ttu-id="3f666-258">Siempre que el estado del cliente todavía esté en la memoria, la sesión del cliente se reanudará sin perder el estado.</span><span class="sxs-lookup"><span data-stu-id="3f666-258">As long as the client's state is still in memory, the client session resumes without losing state.</span></span>

#### <a name="reconnection-to-the-same-server"></a><span data-ttu-id="3f666-259">Reconexión al mismo servidor</span><span class="sxs-lookup"><span data-stu-id="3f666-259">Reconnection to the same server</span></span>

<span data-ttu-id="3f666-260">Una aplicación de Blazor Server se representa en respuesta a la primera solicitud de cliente, que configura el estado de la interfaz de usuario en el servidor.</span><span class="sxs-lookup"><span data-stu-id="3f666-260">A Blazor Server app prerenders in response to the first client request, which sets up the UI state on the server.</span></span> <span data-ttu-id="3f666-261">Cuando el cliente intenta crear una conexión SignalR, el cliente debe volver a conectarse al mismo servidor.</span><span class="sxs-lookup"><span data-stu-id="3f666-261">When the client attempts to create a SignalR connection, the client must reconnect to the same server.</span></span> <span data-ttu-id="3f666-262">las aplicaciones de Blazor Server que usan más de un servidor back-end deben implementar *sesiones permanentes* para conexiones SignalR.</span><span class="sxs-lookup"><span data-stu-id="3f666-262">Blazor Server apps that use more than one backend server should implement *sticky sessions* for SignalR connections.</span></span>

<span data-ttu-id="3f666-263">Se recomienda usar [Azure SignalR Service](/azure/azure-signalr) para las aplicaciones Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="3f666-263">We recommend using the [Azure SignalR Service](/azure/azure-signalr) for Blazor Server apps.</span></span> <span data-ttu-id="3f666-264">El servicio permite el escalado vertical de una aplicación Blazor Server a un gran número de conexiones SignalR simultáneas.</span><span class="sxs-lookup"><span data-stu-id="3f666-264">The service allows for scaling up a Blazor Server app to a large number of concurrent SignalR connections.</span></span> <span data-ttu-id="3f666-265">Las sesiones permanentes están habilitadas para el servicio Azure SignalR estableciendo la opción de `ServerStickyMode` o el valor de configuración del servicio en `Required`.</span><span class="sxs-lookup"><span data-stu-id="3f666-265">Sticky sessions are enabled for the Azure SignalR Service by setting the service's `ServerStickyMode` option or configuration value to `Required`.</span></span> <span data-ttu-id="3f666-266">Para obtener más información, vea <xref:host-and-deploy/blazor/server#signalr-configuration>.</span><span class="sxs-lookup"><span data-stu-id="3f666-266">For more information, see <xref:host-and-deploy/blazor/server#signalr-configuration>.</span></span>

<span data-ttu-id="3f666-267">Cuando se usa IIS, las sesiones permanentes se habilitan con el enrutamiento de solicitud de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="3f666-267">When using IIS, sticky sessions are enabled with Application Request Routing.</span></span> <span data-ttu-id="3f666-268">Para más información, vea [Equilibrio de carga HTTP mediante el enrutamiento de solicitud de aplicaciones](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).</span><span class="sxs-lookup"><span data-stu-id="3f666-268">For more information, see [HTTP Load Balancing using Application Request Routing](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).</span></span>

#### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="3f666-269">Reflejar el estado de conexión en la interfaz de usuario</span><span class="sxs-lookup"><span data-stu-id="3f666-269">Reflect the connection state in the UI</span></span>

<span data-ttu-id="3f666-270">Cuando el cliente detecta que se ha perdido la conexión, se muestra al usuario una interfaz de usuario predeterminada mientras el cliente intenta volver a conectarse.</span><span class="sxs-lookup"><span data-stu-id="3f666-270">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="3f666-271">Si se produce un error en la reconexión, se proporciona al usuario la opción de volver a intentarlo.</span><span class="sxs-lookup"><span data-stu-id="3f666-271">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="3f666-272">Para personalizar la interfaz de usuario, defina un elemento con un `id` de `components-reconnect-modal` en la `<body>` de la página de Razor de *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="3f666-272">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="3f666-273">En la tabla siguiente se describen las clases CSS aplicadas al elemento `components-reconnect-modal`.</span><span class="sxs-lookup"><span data-stu-id="3f666-273">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="3f666-274">Clase CSS</span><span class="sxs-lookup"><span data-stu-id="3f666-274">CSS class</span></span>                       | <span data-ttu-id="3f666-275">Indica&hellip;</span><span class="sxs-lookup"><span data-stu-id="3f666-275">Indicates&hellip;</span></span> |
| ------------------------------- | ------------------------- |
| `components-reconnect-show`     | <span data-ttu-id="3f666-276">Una conexión perdida.</span><span class="sxs-lookup"><span data-stu-id="3f666-276">A lost connection.</span></span> <span data-ttu-id="3f666-277">El cliente está intentando volver a conectarse.</span><span class="sxs-lookup"><span data-stu-id="3f666-277">The client is attempting to reconnect.</span></span> <span data-ttu-id="3f666-278">Muestre el modal.</span><span class="sxs-lookup"><span data-stu-id="3f666-278">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="3f666-279">Una conexión activa se restablece en el servidor.</span><span class="sxs-lookup"><span data-stu-id="3f666-279">An active connection is re-established to the server.</span></span> <span data-ttu-id="3f666-280">Oculte el modal.</span><span class="sxs-lookup"><span data-stu-id="3f666-280">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="3f666-281">Error de reconexión, probablemente debido a un error de red.</span><span class="sxs-lookup"><span data-stu-id="3f666-281">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="3f666-282">Para intentar la reconexión, llame a `window.Blazor.reconnect()`.</span><span class="sxs-lookup"><span data-stu-id="3f666-282">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="3f666-283">Reconexión rechazada.</span><span class="sxs-lookup"><span data-stu-id="3f666-283">Reconnection rejected.</span></span> <span data-ttu-id="3f666-284">Se alcanzó el servidor pero se rechazó la conexión y se pierde el estado del usuario en el servidor.</span><span class="sxs-lookup"><span data-stu-id="3f666-284">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="3f666-285">Para volver a cargar la aplicación, llame a `location.reload()`.</span><span class="sxs-lookup"><span data-stu-id="3f666-285">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="3f666-286">Este estado de conexión puede producirse cuando:</span><span class="sxs-lookup"><span data-stu-id="3f666-286">This connection state may result when:</span></span><ul><li><span data-ttu-id="3f666-287">Se produce un bloqueo en el circuito del servidor.</span><span class="sxs-lookup"><span data-stu-id="3f666-287">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="3f666-288">El cliente se desconecta el tiempo suficiente para que el servidor Quite el estado del usuario.</span><span class="sxs-lookup"><span data-stu-id="3f666-288">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="3f666-289">Se eliminan las instancias de los componentes con los que el usuario está interactuando.</span><span class="sxs-lookup"><span data-stu-id="3f666-289">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="3f666-290">El servidor se reinicia o se recicla el proceso de trabajo de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3f666-290">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="3f666-291">Reconexión con estado después de la representación previa</span><span class="sxs-lookup"><span data-stu-id="3f666-291">Stateful reconnection after prerendering</span></span>

<span data-ttu-id="3f666-292">las aplicaciones de Blazor Server se configuran de forma predeterminada para prerepresentar la interfaz de usuario en el servidor antes de establecer la conexión de cliente con el servidor.</span><span class="sxs-lookup"><span data-stu-id="3f666-292">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="3f666-293">Esto se configura en la página de Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="3f666-293">This is set up in the *_Host.cshtml* Razor page:</span></span>

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="3f666-294">`RenderMode` configura si el componente:</span><span class="sxs-lookup"><span data-stu-id="3f666-294">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="3f666-295">Se representa en la página.</span><span class="sxs-lookup"><span data-stu-id="3f666-295">Is prerendered into the page.</span></span>
* <span data-ttu-id="3f666-296">Se representa como HTML estático en la página o si incluye la información necesaria para arrancar una aplicación Blazor desde el agente de usuario.</span><span class="sxs-lookup"><span data-stu-id="3f666-296">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="3f666-297">Descripción</span><span class="sxs-lookup"><span data-stu-id="3f666-297">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="3f666-298">Representa el componente en código HTML estático e incluye un marcador para una aplicación de Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="3f666-298">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="3f666-299">Cuando se inicia el agente de usuario, este marcador se usa para arrancar una aplicación Blazor.</span><span class="sxs-lookup"><span data-stu-id="3f666-299">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="3f666-300">Representa un marcador para una aplicación de Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="3f666-300">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="3f666-301">La salida del componente no está incluida.</span><span class="sxs-lookup"><span data-stu-id="3f666-301">Output from the component isn't included.</span></span> <span data-ttu-id="3f666-302">Cuando se inicia el agente de usuario, este marcador se usa para arrancar una aplicación Blazor.</span><span class="sxs-lookup"><span data-stu-id="3f666-302">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="3f666-303">Representa el componente en HTML estático.</span><span class="sxs-lookup"><span data-stu-id="3f666-303">Renders the component into static HTML.</span></span> |

<span data-ttu-id="3f666-304">No se admite la representación de componentes de servidor desde una página HTML estática.</span><span class="sxs-lookup"><span data-stu-id="3f666-304">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="3f666-305">Cuando se `ServerPrerendered`la `RenderMode`, el componente se representa inicialmente estáticamente como parte de la página.</span><span class="sxs-lookup"><span data-stu-id="3f666-305">When `RenderMode` is `ServerPrerendered`, the component is initially rendered statically as part of the page.</span></span> <span data-ttu-id="3f666-306">Una vez que el explorador vuelve a establecer una conexión con el servidor, el componente se *vuelve*a representar y el componente ahora es interactivo.</span><span class="sxs-lookup"><span data-stu-id="3f666-306">Once the browser establishes a connection back to the server, the component is rendered *again*, and the component is now interactive.</span></span> <span data-ttu-id="3f666-307">Si el método de ciclo de vida [{Async} inicializado](xref:blazor/lifecycle#component-initialization-methods) para inicializar el componente está presente, el método se ejecuta *dos veces*:</span><span class="sxs-lookup"><span data-stu-id="3f666-307">If the [OnInitialized{Async}](xref:blazor/lifecycle#component-initialization-methods) lifecycle method for initializing the component is present, the method is executed *twice*:</span></span>

* <span data-ttu-id="3f666-308">Cuando el componente se representa de forma estática.</span><span class="sxs-lookup"><span data-stu-id="3f666-308">When the component is prerendered statically.</span></span>
* <span data-ttu-id="3f666-309">Una vez establecida la conexión al servidor.</span><span class="sxs-lookup"><span data-stu-id="3f666-309">After the server connection has been established.</span></span>

<span data-ttu-id="3f666-310">Esto puede dar lugar a un cambio notable en los datos mostrados en la interfaz de usuario cuando el componente se representa finalmente.</span><span class="sxs-lookup"><span data-stu-id="3f666-310">This can result in a noticeable change in the data displayed in the UI when the component is finally rendered.</span></span>

<span data-ttu-id="3f666-311">Para evitar el escenario de representación doble en una aplicación de Blazor Server:</span><span class="sxs-lookup"><span data-stu-id="3f666-311">To avoid the double-rendering scenario in a Blazor Server app:</span></span>

* <span data-ttu-id="3f666-312">Pase un identificador que se puede usar para almacenar en caché el estado durante la representación previa y recuperar el estado después de que se reinicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3f666-312">Pass in an identifier that can be used to cache the state during prerendering and to retrieve the state after the app restarts.</span></span>
* <span data-ttu-id="3f666-313">Use el identificador durante la representación previa para guardar el estado del componente.</span><span class="sxs-lookup"><span data-stu-id="3f666-313">Use the identifier during prerendering to save component state.</span></span>
* <span data-ttu-id="3f666-314">Use el identificador después de la representación previa para recuperar el estado almacenado en caché.</span><span class="sxs-lookup"><span data-stu-id="3f666-314">Use the identifier after prerendering to retrieve the cached state.</span></span>

<span data-ttu-id="3f666-315">En el código siguiente se muestra una `WeatherForecastService` actualizada en una aplicación de Blazor Server basada en plantillas que evita la representación doble:</span><span class="sxs-lookup"><span data-stu-id="3f666-315">The following code demonstrates an updated `WeatherForecastService` in a template-based Blazor Server app that avoids the double rendering:</span></span>

```csharp
public class WeatherForecastService
{
    private static readonly string[] Summaries = new[]
    {
        "Freezing", "Bracing", "Chilly", "Cool", "Mild",
        "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
    };
    
    public WeatherForecastService(IMemoryCache memoryCache)
    {
        MemoryCache = memoryCache;
    }
    
    public IMemoryCache MemoryCache { get; }

    public Task<WeatherForecast[]> GetForecastAsync(DateTime startDate)
    {
        return MemoryCache.GetOrCreateAsync(startDate, async e =>
        {
            e.SetOptions(new MemoryCacheEntryOptions
            {
                AbsoluteExpirationRelativeToNow = 
                    TimeSpan.FromSeconds(30)
            });

            var rng = new Random();

            await Task.Delay(TimeSpan.FromSeconds(10));

            return Enumerable.Range(1, 5).Select(index => new WeatherForecast
            {
                Date = startDate.AddDays(index),
                TemperatureC = rng.Next(-20, 55),
                Summary = Summaries[rng.Next(Summaries.Length)]
            }).ToArray();
        });
    }
}
```

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="3f666-316">Representación de componentes interactivos con estado desde vistas y páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="3f666-316">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="3f666-317">Los componentes interactivos con estado se pueden agregar a una página o vista de Razor.</span><span class="sxs-lookup"><span data-stu-id="3f666-317">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="3f666-318">Cuando se representa la página o la vista:</span><span class="sxs-lookup"><span data-stu-id="3f666-318">When the page or view renders:</span></span>

* <span data-ttu-id="3f666-319">El componente se representará con la página o la vista.</span><span class="sxs-lookup"><span data-stu-id="3f666-319">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="3f666-320">Se pierde el estado inicial del componente usado para la representación previa.</span><span class="sxs-lookup"><span data-stu-id="3f666-320">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="3f666-321">Cuando se establece la conexión SignalR, se crea un nuevo estado del componente.</span><span class="sxs-lookup"><span data-stu-id="3f666-321">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="3f666-322">La siguiente página de Razor representa un componente de `Counter`:</span><span class="sxs-lookup"><span data-stu-id="3f666-322">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="3f666-323">Representación de componentes no interactivos desde páginas y vistas de Razor</span><span class="sxs-lookup"><span data-stu-id="3f666-323">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="3f666-324">En la siguiente página de Razor, el componente de `Counter` se representa estáticamente con un valor inicial que se especifica mediante un formulario:</span><span class="sxs-lookup"><span data-stu-id="3f666-324">In the following Razor page, the `Counter` component is statically rendered with an initial value that's specified using a form:</span></span>

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

<span data-ttu-id="3f666-325">Como `MyComponent` se representa estáticamente, el componente no puede ser interactivo.</span><span class="sxs-lookup"><span data-stu-id="3f666-325">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="3f666-326">Detectar cuándo se está preprocesando la aplicación</span><span class="sxs-lookup"><span data-stu-id="3f666-326">Detect when the app is prerendering</span></span>

[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="3f666-327">Configuración del cliente de SignalR para aplicaciones de Blazor Server</span><span class="sxs-lookup"><span data-stu-id="3f666-327">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="3f666-328">A veces, debe configurar el cliente de SignalR que usan las aplicaciones de Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="3f666-328">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="3f666-329">Por ejemplo, puede que desee configurar el registro en el SignalR cliente para diagnosticar un problema de conexión.</span><span class="sxs-lookup"><span data-stu-id="3f666-329">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="3f666-330">Para configurar el cliente de SignalR en el archivo *pages/_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="3f666-330">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="3f666-331">Agregue un atributo `autostart="false"` a la etiqueta de `<script>` para el script de `blazor.server.js`.</span><span class="sxs-lookup"><span data-stu-id="3f666-331">Add an `autostart="false"` attribute to the `<script>` tag for the `blazor.server.js` script.</span></span>
* <span data-ttu-id="3f666-332">Llame a `Blazor.start` y pase un objeto de configuración que especifique el generador de SignalR.</span><span class="sxs-lookup"><span data-stu-id="3f666-332">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="3f666-333">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="3f666-333">Additional resources</span></span>

* <xref:blazor/get-started>
* <xref:signalr/introduction>
