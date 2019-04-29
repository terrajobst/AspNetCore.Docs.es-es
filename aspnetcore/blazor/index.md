---
title: Introducción a Blazor en ASP.NET Core
author: guardrex
description: Explore Blazor en ASP.NET Core, una forma de compilar la interfaz de usuario web interactiva del lado cliente con .NET en una aplicación ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: seoapril2019
ms.date: 04/18/2019
uid: blazor/index
ms.openlocfilehash: 74eeb003c249fc9ff8267ac855455f875806ccd9
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "59983002"
---
# <a name="introduction-to-blazor"></a><span data-ttu-id="62b4a-103">Introducción a Blazor</span><span class="sxs-lookup"><span data-stu-id="62b4a-103">Introduction to Blazor</span></span>

<span data-ttu-id="62b4a-104">Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="62b4a-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="62b4a-105">Le damos la bienvenida a Blazor</span><span class="sxs-lookup"><span data-stu-id="62b4a-105">Welcome to Blazor!</span></span>

<span data-ttu-id="62b4a-106">Compile la interfaz de usuario web del lado cliente interactiva con .NET:</span><span class="sxs-lookup"><span data-stu-id="62b4a-106">Build interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="62b4a-107">Compile interfaces de usuario completamente interactivas con C# en lugar de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="62b4a-107">Build rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="62b4a-108">Comparta la lógica de aplicación del lado cliente y servidor escrita con. NET.</span><span class="sxs-lookup"><span data-stu-id="62b4a-108">Share server-side and client-side app logic written with .NET.</span></span>
* <span data-ttu-id="62b4a-109">Represente la interfaz de usuario como HTML y CSS para la compatibilidad con todos los exploradores, incluidos los móviles.</span><span class="sxs-lookup"><span data-stu-id="62b4a-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="62b4a-110">Blazor admite escenarios básicos requeridos por la mayoría de las aplicaciones:</span><span class="sxs-lookup"><span data-stu-id="62b4a-110">Blazor supports core scenarios required by most apps:</span></span>

* <span data-ttu-id="62b4a-111">Parámetros</span><span class="sxs-lookup"><span data-stu-id="62b4a-111">Parameters</span></span>
* <span data-ttu-id="62b4a-112">Control de eventos</span><span class="sxs-lookup"><span data-stu-id="62b4a-112">Event handling</span></span>
* <span data-ttu-id="62b4a-113">Enlace de datos</span><span class="sxs-lookup"><span data-stu-id="62b4a-113">Data binding</span></span>
* <span data-ttu-id="62b4a-114">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="62b4a-114">Routing</span></span>
* <span data-ttu-id="62b4a-115">Inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="62b4a-115">Dependency injection</span></span>
* <span data-ttu-id="62b4a-116">Diseños</span><span class="sxs-lookup"><span data-stu-id="62b4a-116">Layouts</span></span>
* <span data-ttu-id="62b4a-117">Plantillas</span><span class="sxs-lookup"><span data-stu-id="62b4a-117">Templates</span></span>
* <span data-ttu-id="62b4a-118">Valores en cascada</span><span class="sxs-lookup"><span data-stu-id="62b4a-118">Cascading values</span></span>

## <a name="components"></a><span data-ttu-id="62b4a-119">Componentes</span><span class="sxs-lookup"><span data-stu-id="62b4a-119">Components</span></span>

<span data-ttu-id="62b4a-120">Un *componente* en Blazor es un elemento de la interfaz de usuario, como una página, un cuadro de diálogo o un formulario de entrada de datos.</span><span class="sxs-lookup"><span data-stu-id="62b4a-120">A *component* in Blazor is an element of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="62b4a-121">Los componentes controlan los eventos de usuario y definen la lógica de representación flexible de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="62b4a-121">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="62b4a-122">Los componentes se pueden anidar y reutilizar.</span><span class="sxs-lookup"><span data-stu-id="62b4a-122">Components can be nested and reused.</span></span>

<span data-ttu-id="62b4a-123">Los componentes son clases .NET integradas en ensamblados .NET que se pueden compartir y distribuir como paquetes NuGet.</span><span class="sxs-lookup"><span data-stu-id="62b4a-123">Components are .NET classes built into .NET assemblies that can be shared and distributed as NuGet packages.</span></span> <span data-ttu-id="62b4a-124">La clase del componente se escribe normalmente en forma de una página de marcado de Razor con una extensión de archivo *.razor*.</span><span class="sxs-lookup"><span data-stu-id="62b4a-124">The component class is usually written in the form of a Razor markup page with a *.razor* file extension.</span></span> <span data-ttu-id="62b4a-125">A veces, se hace referencia a los componentes de Blazor como componentes de Razor.</span><span class="sxs-lookup"><span data-stu-id="62b4a-125">Components in Blazor are sometimes referred to as Razor components.</span></span>

<span data-ttu-id="62b4a-126">[Razor](xref:mvc/views/razor) es una sintaxis que permite combinar marcado HTML con código de C#.</span><span class="sxs-lookup"><span data-stu-id="62b4a-126">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="62b4a-127">Razor se ha diseñado para mejorar la productividad de los desarrolladores, ya que permite cambiar entre marcado y C# en el mismo archivo, además de admitir [IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="62b4a-127">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="62b4a-128">Las vistas de Razor Pages y MVC también usan Razor.</span><span class="sxs-lookup"><span data-stu-id="62b4a-128">Razor Pages and MVC views also use Razor.</span></span> <span data-ttu-id="62b4a-129">A diferencia de las vistas de Razor Pages y MVC, que se compilan en torno a un modelo de solicitud y respuesta, los componentes se usan específicamente para gestionar la composición de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="62b4a-129">Unlike Razor Pages and MVC views, which are built around a request/response model, components are used specifically for handling UI composition.</span></span> <span data-ttu-id="62b4a-130">Los componentes de Razor se pueden usar específicamente para la composición y la lógica de la interfaz de usuario del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="62b4a-130">Razor components can be used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="62b4a-131">El marcado que aparece a continuación es un ejemplo de un componente de cuadro de diálogo personalizado:</span><span class="sxs-lookup"><span data-stu-id="62b4a-131">The following markup is an example of a custom dialog component:</span></span>

```cshtml
<div>
    <h2>@Title</h2>
    @BodyContent
    <button onclick="@OnOK">OK</button>
</div>

@functions {
    public string Title { get; set; }
    public RenderFragment BodyContent { get; set; }
    public Action OnOK { get; set; }
}
```

<span data-ttu-id="62b4a-132">Cuando este componente se usa en otra parte de la aplicación, IntelliSense en [Visual Studio](https://visualstudio.microsoft.com/vs/) agiliza el desarrollo con la finalización de parámetros y sintaxis.</span><span class="sxs-lookup"><span data-stu-id="62b4a-132">When this component is used elsewhere in the app, IntelliSense in [Visual Studio](https://visualstudio.microsoft.com/vs/) speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="62b4a-133">Los componentes se representan en una representación en memoria del DOM del explorador conocida como *árbol de representación* que se puede usar para actualizar la interfaz de usuario de una manera eficaz y flexible.</span><span class="sxs-lookup"><span data-stu-id="62b4a-133">Components render into an in-memory representation of the browser DOM called a *render tree* that can then be used to update the UI in a flexible and efficient way.</span></span>

## <a name="blazor-server-side"></a><span data-ttu-id="62b4a-134">Lado servidor de Blazor</span><span class="sxs-lookup"><span data-stu-id="62b4a-134">Blazor server-side</span></span>

<span data-ttu-id="62b4a-135">Blazor separa la lógica de representación de componentes del modo en que se aplican las actualizaciones de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="62b4a-135">Blazor decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="62b4a-136">El lado servidor de Blazor agrega compatibilidad con el hospedaje de componentes de Razor en el servidor en una aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="62b4a-136">Blazor server-side provides support for hosting Razor components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="62b4a-137">Las actualizaciones de la interfaz de usuario se controlan a través de una conexión de SignalR.</span><span class="sxs-lookup"><span data-stu-id="62b4a-137">UI updates are handled over a SignalR connection.</span></span>

<span data-ttu-id="62b4a-138">Entorno de ejecución:</span><span class="sxs-lookup"><span data-stu-id="62b4a-138">The runtime:</span></span>

* <span data-ttu-id="62b4a-139">Controla el envío de eventos de interfaz de usuario desde el explorador al servidor.</span><span class="sxs-lookup"><span data-stu-id="62b4a-139">Handles sending UI events from the browser to the server.</span></span>
* <span data-ttu-id="62b4a-140">Aplica las actualizaciones de la interfaz de usuario que el servidor devuelve al explorador después de ejecutar los componentes.</span><span class="sxs-lookup"><span data-stu-id="62b4a-140">Applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="62b4a-141">La conexión utilizada por el lado servidor de Blazor para comunicarse con el explorador también se usa para controlar las llamadas de interoperabilidad de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="62b4a-141">The connection used by Blazor server-side to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![El lado servidor de Blazor ejecuta código de .NET en el servidor e interactúa con Document Object Model en el cliente mediante una conexión de SignalR.](index/_static/blazor-server-side.png)

<span data-ttu-id="62b4a-143">Para obtener más información, vea <xref:blazor/hosting-models#server-side>.</span><span class="sxs-lookup"><span data-stu-id="62b4a-143">For more information, see <xref:blazor/hosting-models#server-side>.</span></span>

## <a name="blazor-client-side"></a><span data-ttu-id="62b4a-144">Lado cliente de Blazor</span><span class="sxs-lookup"><span data-stu-id="62b4a-144">Blazor client-side</span></span>

<span data-ttu-id="62b4a-145">El lado cliente de Blazor es un marco de aplicaciones de una sola página para compilar aplicaciones web interactivas del lado cliente con. NET.</span><span class="sxs-lookup"><span data-stu-id="62b4a-145">Blazor client-side is a single-page app framework for building interactive client-side Web apps with .NET.</span></span> <span data-ttu-id="62b4a-146">El lado cliente de Blazor usa estándares web abiertos sin complementos ni transpilación de código.</span><span class="sxs-lookup"><span data-stu-id="62b4a-146">Blazor client-side uses open web standards without plugins or code transpilation.</span></span> <span data-ttu-id="62b4a-147">El lado cliente de Blazor funciona en todos los exploradores web modernos, incluidos los exploradores móviles.</span><span class="sxs-lookup"><span data-stu-id="62b4a-147">Blazor client-side works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="62b4a-148">El uso de .NET en el explorador para el desarrollo web en el lado cliente ofrece muchas ventajas:</span><span class="sxs-lookup"><span data-stu-id="62b4a-148">Using .NET in the browser for client-side web development offers many advantages:</span></span>

* <span data-ttu-id="62b4a-149">**Lenguaje C#**: escriba código en C# en lugar de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="62b4a-149">**C# language**: Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="62b4a-150">**Ecosistema .NET**: aproveche el ecosistema existente de bibliotecas .NET.</span><span class="sxs-lookup"><span data-stu-id="62b4a-150">**.NET Ecosystem**: Leverage the existing ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="62b4a-151">**Desarrollo de la pila completa**: comparta la lógica de cliente y servidor.</span><span class="sxs-lookup"><span data-stu-id="62b4a-151">**Full-stack development**: Share server and client-side logic.</span></span>
* <span data-ttu-id="62b4a-152">**Velocidad y escalabilidad**: NET se creó pensando en el rendimiento, la confiabilidad y la seguridad.</span><span class="sxs-lookup"><span data-stu-id="62b4a-152">**Speed and scalability**: .NET was built for performance, reliability, and security.</span></span>
* <span data-ttu-id="62b4a-153">**Herramientas líderes del sector**: mantenga la productividad con Visual Studio en Windows, Linux y macOS.</span><span class="sxs-lookup"><span data-stu-id="62b4a-153">**Industry-leading tools**: Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="62b4a-154">**Estabilidad y coherencia**:  compile sobre un conjunto común de lenguajes, marcos y herramientas que son estables, completos y fáciles de usar.</span><span class="sxs-lookup"><span data-stu-id="62b4a-154">**Stability and consistency**:  Build on a common set of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

<span data-ttu-id="62b4a-155">La ejecución de código .NET dentro de exploradores web se consigue mediante [WebAssembly](http://webassembly.org) (abreviado como *wasm*).</span><span class="sxs-lookup"><span data-stu-id="62b4a-155">Running .NET code inside web browsers is made possible by [WebAssembly](http://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="62b4a-156">WebAssembly es un estándar web abierto y se admite en exploradores web sin complementos.</span><span class="sxs-lookup"><span data-stu-id="62b4a-156">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span> <span data-ttu-id="62b4a-157">WebAssembly es un formato de código de bytes compacto optimizado para descargas rápidas y una velocidad de ejecución máxima.</span><span class="sxs-lookup"><span data-stu-id="62b4a-157">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span>

<span data-ttu-id="62b4a-158">El código de WebAssembly puede acceder a todas las funciones del explorador mediante la interoperabilidad de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="62b4a-158">WebAssembly code can access the full functionality of the browser via JavaScript interop.</span></span> <span data-ttu-id="62b4a-159">Al mismo tiempo, el código .NET ejecutado mediante WebAssembly funciona en el mismo espacio aislado de confianza que JavaScript para impedir acciones malintencionadas en la máquina cliente.</span><span class="sxs-lookup"><span data-stu-id="62b4a-159">At the same time, .NET code executed via WebAssembly runs in the same trusted sandbox as JavaScript to prevent malicious actions on the client machine.</span></span>

![El lado cliente de Blazor ejecuta código de .NET en el explorador con WebAssembly.](index/_static/blazor-client-side.png)

<span data-ttu-id="62b4a-161">Cuando se compila y ejecuta una aplicación del lado cliente de Blazor en un explorador:</span><span class="sxs-lookup"><span data-stu-id="62b4a-161">When a Blazor client-side app is built and run in a browser:</span></span>

* <span data-ttu-id="62b4a-162">Los archivos de código de C# y los archivos de Razor se compilan en ensamblados de .NET.</span><span class="sxs-lookup"><span data-stu-id="62b4a-162">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="62b4a-163">Los ensamblados y el entorno de ejecución de .NET se descargan en el explorador.</span><span class="sxs-lookup"><span data-stu-id="62b4a-163">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="62b4a-164">El lado cliente de Blazor arranca el entorno de ejecución .NET y lo configura para cargar los ensamblados de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="62b4a-164">Blazor client-side bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="62b4a-165">El entorno de ejecución del lado cliente de Blazor controla la manipulación de Document Object Model (DOM) y las llamadas a API del explorador mediante la interoperabilidad de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="62b4a-165">Document Object Model (DOM) manipulation and browser API calls are handled by the Blazor client-side runtime via JavaScript interop.</span></span>

<span data-ttu-id="62b4a-166">Para reducir el tamaño de la aplicación descargada, se ha quitado el código sin usar de la aplicación cuando se publica mediante el [enlazador del lenguaje intermedio (IL)](xref:host-and-deploy/blazor/configure-linker).</span><span class="sxs-lookup"><span data-stu-id="62b4a-166">To reduce the size of the downloaded app, unused code is stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/blazor/configure-linker).</span></span>

<span data-ttu-id="62b4a-167">El lado de cliente de Blazor es un modelo de hospedaje del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="62b4a-167">Blazor client-side is a client-side hosting model.</span></span> <span data-ttu-id="62b4a-168">Como Razor separa la lógica de representación de un componente de la forma en que se aplican las actualizaciones de la interfaz de usuario, existe flexibilidad para hospedar Razor.</span><span class="sxs-lookup"><span data-stu-id="62b4a-168">Because Blazor decouples a component's rendering logic from how UI updates are applied, there's flexibility in how Blazor can be hosted.</span></span> <span data-ttu-id="62b4a-169">Use el [lado servidor de Blazor](#blazor-server-side) para hospedar Blazor en el servidor en una aplicación de ASP.NET Core, donde todas las actualizaciones de la interfaz de usuario se controlan mediante una conexión de SignalR.</span><span class="sxs-lookup"><span data-stu-id="62b4a-169">Use [Blazor server-side](#blazor-server-side) to host Blazor on the server in an ASP.NET Core app where UI updates are handled over a SignalR connection.</span></span> <span data-ttu-id="62b4a-170">Para obtener más información, vea <xref:blazor/hosting-models#server-side>.</span><span class="sxs-lookup"><span data-stu-id="62b4a-170">For more information, see <xref:blazor/hosting-models#server-side>.</span></span> 

<span data-ttu-id="62b4a-171">El tamaño de carga es un factor de rendimiento críticos para la facilidad de uso de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="62b4a-171">Payload size is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="62b4a-172">El lado cliente de Blazor optimiza el tamaño de carga para reducir los tiempos de descarga:</span><span class="sxs-lookup"><span data-stu-id="62b4a-172">Blazor client-side optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="62b4a-173">Las partes no usadas de los ensamblados .NET se quitan durante el proceso de compilación.</span><span class="sxs-lookup"><span data-stu-id="62b4a-173">Unused parts of .NET assemblies are removed during the build process.</span></span>
* <span data-ttu-id="62b4a-174">Las respuestas HTTP se comprimen.</span><span class="sxs-lookup"><span data-stu-id="62b4a-174">HTTP responses are compressed.</span></span>
* <span data-ttu-id="62b4a-175">El entorno de ejecución .NET y los ensamblados se almacenan en caché en el explorador.</span><span class="sxs-lookup"><span data-stu-id="62b4a-175">The .NET runtime and assemblies are cached in the browser.</span></span>

<span data-ttu-id="62b4a-176">El [lado servidor de Blazor](#blazor-server-side) proporciona un tamaño de carga más pequeño que el lado cliente Blazor al mantener los ensamblados de .NET, el ensamblado de la aplicación y el entorno de ejecución en el lado servidor.</span><span class="sxs-lookup"><span data-stu-id="62b4a-176">[Blazor server-side](#blazor-server-side) provides a smaller payload size than Blazor client-side by maintaining .NET assemblies, the app's assembly, and the runtime server-side.</span></span> <span data-ttu-id="62b4a-177">Las aplicaciones del lado servidor de Blazor solo proporcionan archivos de marcado y recursos estáticos a los clientes.</span><span class="sxs-lookup"><span data-stu-id="62b4a-177">Blazor server-side apps only serve markup files and static assets to clients.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="62b4a-178">Interoperabilidad de JavaScript</span><span class="sxs-lookup"><span data-stu-id="62b4a-178">JavaScript interop</span></span>

<span data-ttu-id="62b4a-179">En el caso de aplicaciones que necesitan bibliotecas de JavaScript de terceros y API de explorador, los componentes interoperan con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="62b4a-179">For apps that require third-party JavaScript libraries and browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="62b4a-180">Los componentes pueden usar cualquier biblioteca o API que pueda utilizar JavaScript.</span><span class="sxs-lookup"><span data-stu-id="62b4a-180">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="62b4a-181">El código de C# puede llamar a código JavaScript y, a su vez, el código JavaScript puede llamar a código de C#.</span><span class="sxs-lookup"><span data-stu-id="62b4a-181">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="62b4a-182">Para obtener más información, vea [Interoperabilidad de JavaScript](xref:blazor/javascript-interop).</span><span class="sxs-lookup"><span data-stu-id="62b4a-182">For more information, see [JavaScript interop](xref:blazor/javascript-interop).</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="62b4a-183">Uso compartido de código y .NET Standard</span><span class="sxs-lookup"><span data-stu-id="62b4a-183">Code sharing and .NET Standard</span></span>

<span data-ttu-id="62b4a-184">Las aplicaciones pueden hacer referencia a bibliotecas de [.NET Standard](/dotnet/standard/net-standard) existentes y usarlas.</span><span class="sxs-lookup"><span data-stu-id="62b4a-184">Apps can reference and use existing [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="62b4a-185">.NET Standard es una especificación formal de las API de .NET comunes entre las implementaciones de .NET.</span><span class="sxs-lookup"><span data-stu-id="62b4a-185">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="62b4a-186">Blazor implementa .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="62b4a-186">Blazor implements .NET Standard 2.0.</span></span> <span data-ttu-id="62b4a-187">Las API que no pueden aplicarse dentro de un explorador web (por ejemplo, para acceder al sistema de archivos, abrir un socket, ejecutar subprocesos y otras características) generan una excepción <xref:System.PlatformNotSupportedException>.</span><span class="sxs-lookup"><span data-stu-id="62b4a-187">APIs that aren't applicable inside a web browser (for example, accessing the file system, opening a socket, threading, and other features) throw <xref:System.PlatformNotSupportedException>.</span></span> <span data-ttu-id="62b4a-188">Las bibliotecas de clase .NET Standard pueden compartirse a través de diferentes plataformas .NET, como Blazor, .NET Framework, .NET Core, Xamarin, Mono y Unity.</span><span class="sxs-lookup"><span data-stu-id="62b4a-188">.NET Standard class libraries can be shared across different .NET platforms, such as Blazor, .NET Framework, .NET Core, Xamarin, Mono, and Unity.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="62b4a-189">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="62b4a-189">Additional resources</span></span>

* [<span data-ttu-id="62b4a-190">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="62b4a-190">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="62b4a-191">Guía de C#</span><span class="sxs-lookup"><span data-stu-id="62b4a-191">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="62b4a-192">HTML</span><span class="sxs-lookup"><span data-stu-id="62b4a-192">HTML</span></span>](https://www.w3.org/html/)
