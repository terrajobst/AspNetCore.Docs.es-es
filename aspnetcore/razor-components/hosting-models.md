---
title: Los modelos de hospedaje de componentes de Razor
author: guardrex
description: Comprender Blazor del lado cliente y servidor ASP.NET Core Razor componentes, modelos de hospedaje.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: razor-components/hosting-models
ms.openlocfilehash: 8ed70117c94bf1a3e4c208f70310bbf0473bae44
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59515649"
---
# <a name="razor-components-hosting-models"></a><span data-ttu-id="204fb-103">Los modelos de hospedaje de componentes de Razor</span><span class="sxs-lookup"><span data-stu-id="204fb-103">Razor Components hosting models</span></span>

<span data-ttu-id="204fb-104">Por [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="204fb-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="204fb-105">Componentes de Razor es un marco web diseñado para ejecutarse del lado cliente en el explorador en un [WebAssembly](http://webassembly.org/)-basado en tiempo de ejecución .NET (*Blazor*) o de servidor en ASP.NET Core (*Razor de ASP.NET Core Componentes*).</span><span class="sxs-lookup"><span data-stu-id="204fb-105">Razor Components is a web framework designed to run client-side in the browser on a [WebAssembly](http://webassembly.org/)-based .NET runtime (*Blazor*) or server-side in ASP.NET Core (*ASP.NET Core Razor Components*).</span></span> <span data-ttu-id="204fb-106">Independientemente de los modelos de hospedaje de modelo, la aplicación y el componente *siguen siendo los mismos*.</span><span class="sxs-lookup"><span data-stu-id="204fb-106">Regardless of the hosting model, the app and component models *remain the same*.</span></span> <span data-ttu-id="204fb-107">En este artículo se describe los modelos de hospedaje disponibles:</span><span class="sxs-lookup"><span data-stu-id="204fb-107">This article discusses the available hosting models:</span></span>

* [<span data-ttu-id="204fb-108">Blazor del lado cliente</span><span class="sxs-lookup"><span data-stu-id="204fb-108">Client-side Blazor</span></span>](#client-side-hosting-model)
* [<span data-ttu-id="204fb-109">Razor Components de ASP.NET Core del lado servidor</span><span class="sxs-lookup"><span data-stu-id="204fb-109">Server-side ASP.NET Core Razor Components</span></span>](#server-side-hosting-model)

## <a name="client-side-hosting-model"></a><span data-ttu-id="204fb-110">Modelo de hospedaje del lado cliente</span><span class="sxs-lookup"><span data-stu-id="204fb-110">Client-side hosting model</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="204fb-111">El modelo de hospedaje principal para Blazor es la ejecución del cliente en el explorador en WebAssembly.</span><span class="sxs-lookup"><span data-stu-id="204fb-111">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="204fb-112">La aplicación Blazor, sus dependencias y el entorno de ejecución de .NET se descargan en el explorador.</span><span class="sxs-lookup"><span data-stu-id="204fb-112">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="204fb-113">La aplicación se ejecuta directamente en el subproceso de interfaz de usuario del explorador.</span><span class="sxs-lookup"><span data-stu-id="204fb-113">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="204fb-114">Las actualizaciones de la interfaz de usuario y control de eventos se producen dentro del mismo proceso.</span><span class="sxs-lookup"><span data-stu-id="204fb-114">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="204fb-115">Recursos de la aplicación se implementan como archivos estáticos en un servidor web o servicio capaz de atender contenido estático a los clientes.</span><span class="sxs-lookup"><span data-stu-id="204fb-115">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![Blazor de cliente: La aplicación Blazor se ejecuta en un subproceso de interfaz de usuario dentro del explorador.](hosting-models/_static/client-side.png)

<span data-ttu-id="204fb-117">Para crear una aplicación de Blazor con el modelo de hospedaje del lado cliente, use cualquiera de las siguientes plantillas:</span><span class="sxs-lookup"><span data-stu-id="204fb-117">To create a Blazor app using the client-side hosting model, use either of the following templates:</span></span>

* <span data-ttu-id="204fb-118">**Blazor** ([dotnet nuevo blazor](/dotnet/core/tools/dotnet-new)) &ndash; implementado como un conjunto de archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="204fb-118">**Blazor** ([dotnet new blazor](/dotnet/core/tools/dotnet-new)) &ndash; Deployed as a set of static files.</span></span>
* <span data-ttu-id="204fb-119">**Blazor (ASP.NET Core se hospedan)** ([dotnet nuevo blazorhosted](/dotnet/core/tools/dotnet-new)) &ndash; hospedadas por un servidor de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="204fb-119">**Blazor (ASP.NET Core Hosted)** ([dotnet new blazorhosted](/dotnet/core/tools/dotnet-new)) &ndash; Hosted by an ASP.NET Core server.</span></span> <span data-ttu-id="204fb-120">La aplicación de ASP.NET Core sirve la aplicación Blazor a los clientes.</span><span class="sxs-lookup"><span data-stu-id="204fb-120">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="204fb-121">La aplicación de Blazor del lado cliente puede interactuar con el servidor a través de la red mediante llamadas a la API web o [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="204fb-121">The client-side Blazor app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="204fb-122">Las plantillas incluyen el *components.webassembly.js* que controla la secuencia de comandos:</span><span class="sxs-lookup"><span data-stu-id="204fb-122">The templates include the *components.webassembly.js* script that handles:</span></span>

* <span data-ttu-id="204fb-123">Descargando el tiempo de ejecución. NET, la aplicación y las dependencias de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="204fb-123">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="204fb-124">Inicialización del tiempo de ejecución para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="204fb-124">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="204fb-125">El modelo de hospedaje del lado cliente ofrece varias ventajas.</span><span class="sxs-lookup"><span data-stu-id="204fb-125">The client-side hosting model offers several benefits.</span></span> <span data-ttu-id="204fb-126">Blazor del lado cliente:</span><span class="sxs-lookup"><span data-stu-id="204fb-126">Client-side Blazor:</span></span>

* <span data-ttu-id="204fb-127">No tiene ninguna dependencia de .NET del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="204fb-127">Has no .NET server-side dependency.</span></span>
* <span data-ttu-id="204fb-128">Aprovecha completamente las capacidades y los recursos del cliente.</span><span class="sxs-lookup"><span data-stu-id="204fb-128">Fully leverages client resources and capabilities.</span></span>
* <span data-ttu-id="204fb-129">Las descargas de trabajan desde el servidor al cliente.</span><span class="sxs-lookup"><span data-stu-id="204fb-129">Offloads work from the server to the client.</span></span>
* <span data-ttu-id="204fb-130">Es compatible con escenarios sin conexión.</span><span class="sxs-lookup"><span data-stu-id="204fb-130">Supports offline scenarios.</span></span>

<span data-ttu-id="204fb-131">Existen desventajas al hospedaje del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="204fb-131">There are downsides to client-side hosting.</span></span> <span data-ttu-id="204fb-132">Blazor del lado cliente:</span><span class="sxs-lookup"><span data-stu-id="204fb-132">Client-side Blazor:</span></span>

* <span data-ttu-id="204fb-133">Restringe la aplicación a las capacidades del explorador.</span><span class="sxs-lookup"><span data-stu-id="204fb-133">Restricts the app to the capabilities of the browser.</span></span>
* <span data-ttu-id="204fb-134">Requiere hardware de cliente compatible con y software (por ejemplo, WebAssembly soporte).</span><span class="sxs-lookup"><span data-stu-id="204fb-134">Requires capable client hardware and software (for example, WebAssembly support).</span></span>
* <span data-ttu-id="204fb-135">Tiene un tamaño de descarga y aplicación mayor tiempo de carga.</span><span class="sxs-lookup"><span data-stu-id="204fb-135">Has a larger download size and longer app load time.</span></span>
* <span data-ttu-id="204fb-136">Tiene menos madurando en tiempo de ejecución de .NET y la compatibilidad con las herramientas (por ejemplo, las limitaciones de [.NET Standard](/dotnet/standard/net-standard) soporte técnico y depuración).</span><span class="sxs-lookup"><span data-stu-id="204fb-136">Has less mature .NET runtime and tooling support (for example, limitations in [.NET Standard](/dotnet/standard/net-standard) support and debugging).</span></span>

## <a name="server-side-hosting-model"></a><span data-ttu-id="204fb-137">Modelo de hospedaje del lado servidor</span><span class="sxs-lookup"><span data-stu-id="204fb-137">Server-side hosting model</span></span>

<span data-ttu-id="204fb-138">Con el modelo de hospedaje de servidor de componentes de Razor de ASP.NET Core, la aplicación se ejecuta en el servidor desde dentro de una aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="204fb-138">With the ASP.NET Core Razor Components server-side hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="204fb-139">Actualizaciones de la interfaz de usuario, el control de eventos y llamadas de JavaScript se controlan a través de un [SignalR](xref:signalr/introduction) conexión.</span><span class="sxs-lookup"><span data-stu-id="204fb-139">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![ASP.NET Core Razor componentes del lado servidor: El explorador se interactúa con la aplicación (hospedada dentro de una aplicación ASP.NET Core) en el servidor a través de una conexión de SignalR.](hosting-models/_static/server-side.png)

<span data-ttu-id="204fb-141">Para crear una aplicación de los componentes de Razor con el modelo de hospedaje de servidor, use ASP.NET Core **Razor componentes** plantilla ([dotnet nuevo razorcomponents](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="204fb-141">To create a Razor Components app using the server-side hosting model, use the ASP.NET Core **Razor Components** template ([dotnet new razorcomponents](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="204fb-142">La aplicación de ASP.NET Core hospeda la aplicación de servidor de componentes de Razor y establece el punto de conexión de SignalR que se conectan los clientes.</span><span class="sxs-lookup"><span data-stu-id="204fb-142">The ASP.NET Core app hosts the Razor Components server-side app and sets up the SignalR endpoint where clients connect.</span></span> <span data-ttu-id="204fb-143">La aplicación hace referencia a la aplicación de ASP.NET Core `Startup` clase para agregar:</span><span class="sxs-lookup"><span data-stu-id="204fb-143">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="204fb-144">Servicios de componentes de Razor del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="204fb-144">Server-side Razor Components services.</span></span>
* <span data-ttu-id="204fb-145">La aplicación a la solicitud de canalización de control.</span><span class="sxs-lookup"><span data-stu-id="204fb-145">The app to the request handling pipeline.</span></span>

[!code-csharp[](hosting-models/samples_snapshot/Startup.cs?highlight=5,27)]

<span data-ttu-id="204fb-146">El *components.server.js* script&dagger; establece la conexión de cliente.</span><span class="sxs-lookup"><span data-stu-id="204fb-146">The *components.server.js* script&dagger; establishes the client connection.</span></span> <span data-ttu-id="204fb-147">Es responsabilidad de la aplicación para conservar y restaurar el estado de la aplicación según sea necesario (por ejemplo, en el caso de una conexión de red perdidas).</span><span class="sxs-lookup"><span data-stu-id="204fb-147">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="204fb-148">El modelo de hospedaje de servidor ofrece varias ventajas.</span><span class="sxs-lookup"><span data-stu-id="204fb-148">The server-side hosting model offers several benefits.</span></span> <span data-ttu-id="204fb-149">Componentes del lado servidor Razor:</span><span class="sxs-lookup"><span data-stu-id="204fb-149">Server-side Razor Components:</span></span>

* <span data-ttu-id="204fb-150">Tiene un tamaño de la aplicación significativamente menor que una aplicación de cliente Blazor y mucho más rápida.</span><span class="sxs-lookup"><span data-stu-id="204fb-150">Have a significantly smaller app size than a client-side Blazor app and load much faster.</span></span>
* <span data-ttu-id="204fb-151">Puede aprovechar al máximo las capacidades de servidor, incluido el uso de las API compatibles de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="204fb-151">Can take full advantage of server capabilities, including using any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="204fb-152">Ejecutar en .NET Core en el servidor, por lo que las herramientas, como la depuración, de .NET existente funciona según lo previsto.</span><span class="sxs-lookup"><span data-stu-id="204fb-152">Run on .NET Core on the server, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="204fb-153">Funciona con clientes ligeros (por ejemplo, los exploradores que no son compatibles con WebAssembly y recursos restringir dispositivos).</span><span class="sxs-lookup"><span data-stu-id="204fb-153">Works with thin clients (for example, browsers that don't support WebAssembly and resource constrained devices).</span></span>
* <span data-ttu-id="204fb-154">.NET /C# código base, incluido el código de componente de la aplicación, no se proporcione a los clientes.</span><span class="sxs-lookup"><span data-stu-id="204fb-154">.NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="204fb-155">Existen desventajas al lado del servidor de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="204fb-155">There are downsides to server-side hosting.</span></span> <span data-ttu-id="204fb-156">Componentes del lado servidor Razor:</span><span class="sxs-lookup"><span data-stu-id="204fb-156">Server-side Razor Components:</span></span>

* <span data-ttu-id="204fb-157">Tienen una latencia mayor: Cada interacción del usuario implica un salto de red.</span><span class="sxs-lookup"><span data-stu-id="204fb-157">Have higher latency: Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="204fb-158">No hay compatibilidad sin conexión con la oferta: Si se produce un error en la conexión de cliente, la aplicación deja de funcionar.</span><span class="sxs-lookup"><span data-stu-id="204fb-158">Offer no offline support: If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="204fb-159">Ha reducido la escalabilidad: El servidor debe administrar varias conexiones de cliente y administrar el estado de cliente.</span><span class="sxs-lookup"><span data-stu-id="204fb-159">Have reduced scalability: The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="204fb-160">Requiere que un servidor de ASP.NET Core para atender la aplicación.</span><span class="sxs-lookup"><span data-stu-id="204fb-160">Require an ASP.NET Core server to serve the app.</span></span> <span data-ttu-id="204fb-161">No es posible la implementación sin un servidor (por ejemplo, desde una red CDN).</span><span class="sxs-lookup"><span data-stu-id="204fb-161">Deployment without a server (for example, from a CDN) isn't possible.</span></span>

<span data-ttu-id="204fb-162">&dagger;El *components.server.js* script se publica en la siguiente ruta: *bin / {depurar | La versión} / {.NET FRAMEWORK de destino} /publish/ {nombre de la aplicación}. Dist/aplicación/platafor_ma*.</span><span class="sxs-lookup"><span data-stu-id="204fb-162">&dagger;The *components.server.js* script is published to the following path: *bin/{Debug|Release}/{TARGET FRAMEWORK}/publish/{APPLICATION NAME}.App/dist/_framework*.</span></span>
