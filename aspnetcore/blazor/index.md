---
title: Introducción a Blazor en ASP.NET Core
author: guardrex
description: Explore Blazor en ASP.NET Core, una forma de compilar la interfaz de usuario web interactiva del lado cliente con .NET en una aplicación ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: seoapril2019
ms.date: 04/15/2019
uid: blazor/index
ms.openlocfilehash: a5b12a5c5c10a74ab192d0d67a2ba9a5617232c7
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614601"
---
# <a name="introduction-to-blazor"></a><span data-ttu-id="bfb1c-103">Introducción a Blazor</span><span class="sxs-lookup"><span data-stu-id="bfb1c-103">Introduction to Blazor</span></span>

<span data-ttu-id="bfb1c-104">Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="bfb1c-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

## <a name="razor-components"></a><span data-ttu-id="bfb1c-105">Componentes de Razor</span><span class="sxs-lookup"><span data-stu-id="bfb1c-105">Razor Components</span></span>

<span data-ttu-id="bfb1c-106">El modelo de hospedaje del lado servidor, *Razor Components*, es una manera de compilar la interfaz de usuario web de cliente interactiva con .NET:</span><span class="sxs-lookup"><span data-stu-id="bfb1c-106">The server-side hosting model of Blazor, *Razor Components*, is a way to build interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="bfb1c-107">Compile interfaces de usuario completamente interactivas con C# en lugar de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-107">Build rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="bfb1c-108">Comparta la lógica de aplicación del lado cliente y servidor escrita toda con. NET.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-108">Share server-side and client-side app logic all written with .NET.</span></span>
* <span data-ttu-id="bfb1c-109">Represente la interfaz de usuario como HTML y CSS para la compatibilidad con todos los exploradores, incluidos los móviles.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="bfb1c-110">Razor Components admite elementos básicos requeridos por la mayoría de las aplicaciones, como:</span><span class="sxs-lookup"><span data-stu-id="bfb1c-110">Razor Components supports core facilities required by most apps, including:</span></span>

* <span data-ttu-id="bfb1c-111">Parámetros</span><span class="sxs-lookup"><span data-stu-id="bfb1c-111">Parameters</span></span>
* <span data-ttu-id="bfb1c-112">Control de eventos</span><span class="sxs-lookup"><span data-stu-id="bfb1c-112">Event handling</span></span>
* <span data-ttu-id="bfb1c-113">Enlace de datos</span><span class="sxs-lookup"><span data-stu-id="bfb1c-113">Data binding</span></span>
* <span data-ttu-id="bfb1c-114">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="bfb1c-114">Routing</span></span>
* <span data-ttu-id="bfb1c-115">Inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="bfb1c-115">Dependency injection</span></span>
* <span data-ttu-id="bfb1c-116">Diseños</span><span class="sxs-lookup"><span data-stu-id="bfb1c-116">Layouts</span></span>
* <span data-ttu-id="bfb1c-117">Plantillas</span><span class="sxs-lookup"><span data-stu-id="bfb1c-117">Templating</span></span>
* <span data-ttu-id="bfb1c-118">Valores en cascada</span><span class="sxs-lookup"><span data-stu-id="bfb1c-118">Cascading values</span></span>

<span data-ttu-id="bfb1c-119">Razor Components separa la lógica de representación de componentes del modo en que se aplican las actualizaciones de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-119">Razor Components decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="bfb1c-120">Razor Components de ASP.NET Core en .NET Core 3.0 agrega compatibilidad con el hospedaje de Razor Components en el servidor en una aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-120">ASP.NET Core Razor Components in .NET Core 3.0 adds support for hosting Razor Components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="bfb1c-121">Las actualizaciones de la interfaz de usuario se controlan a través de una conexión de SignalR.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-121">UI updates are handled over a SignalR connection.</span></span>

<span data-ttu-id="bfb1c-122">Entorno de ejecución:</span><span class="sxs-lookup"><span data-stu-id="bfb1c-122">The runtime:</span></span>

* <span data-ttu-id="bfb1c-123">Controla el envío de eventos de interfaz de usuario desde el explorador al servidor.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-123">Handles sending UI events from the browser to the server.</span></span>
* <span data-ttu-id="bfb1c-124">Aplica las actualizaciones de la interfaz de usuario que el servidor devuelve al explorador después de ejecutar los componentes.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-124">Applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="bfb1c-125">La conexión utilizada por Razor Components para comunicarse con el explorador también se usa para controlar las llamadas de interoperabilidad de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-125">The connection used by Razor Components to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Razor Components ejecuta código de .NET en el servidor e interactúa con Document Object Model en el cliente mediante una conexión de SignalR.](index/_static/aspnet-core-razor-components.png)

<span data-ttu-id="bfb1c-127">Para obtener más información, vea <xref:blazor/hosting-models#server-side-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-127">For more information, see <xref:blazor/hosting-models#server-side-hosting-model>.</span></span>

## <a name="components"></a><span data-ttu-id="bfb1c-128">Componentes</span><span class="sxs-lookup"><span data-stu-id="bfb1c-128">Components</span></span>

<span data-ttu-id="bfb1c-129">Un *componente Razor* es una parte de la interfaz de usuario, como una página, un cuadro de diálogo o un formulario de entrada de datos.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-129">A *Razor Component* is a piece of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="bfb1c-130">Los componentes controlan los eventos de usuario y definen la lógica de representación flexible de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-130">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="bfb1c-131">Los componentes se pueden anidar y reutilizar.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-131">Components can be nested and reused.</span></span>

<span data-ttu-id="bfb1c-132">Los componentes son clases .NET integradas en ensamblados .NET que se pueden compartir y distribuir como paquetes NuGet.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-132">Components are .NET classes built into .NET assemblies that can be shared and distributed as NuGet packages.</span></span> <span data-ttu-id="bfb1c-133">La clase normalmente se escribe con la forma de una página de marcado de Razor con una extensión de archivo *.razor* (Razor Components) o una página de marcado de Razor con una extensión de archivo *.cshtml* (Blazor).</span><span class="sxs-lookup"><span data-stu-id="bfb1c-133">The class is normally written in the form of a Razor markup page with a *.razor* file extension (Razor Components) or Razor markup page with a *.cshtml* file extension (Blazor).</span></span>

<span data-ttu-id="bfb1c-134">[Razor](xref:mvc/views/razor) es una sintaxis que permite combinar marcado HTML con código de C#.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-134">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="bfb1c-135">Razor se ha diseñado para mejorar la productividad de los desarrolladores, ya que permite cambiar entre marcado y C# en el mismo archivo, además de admitir [IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="bfb1c-135">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="bfb1c-136">Las vistas de Razor Pages y MVC también usan Razor.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-136">Razor Pages and MVC views also use Razor.</span></span> <span data-ttu-id="bfb1c-137">A diferencia de las vistas de Razor Pages y MVC, que se compilan en torno a un modelo de solicitud y respuesta, los componentes se usan específicamente para gestionar la composición de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-137">Unlike Razor Pages and MVC views, which are built around a request/response model, components are used specifically for handling UI composition.</span></span> <span data-ttu-id="bfb1c-138">Razor Components se puede usar específicamente para la composición y la lógica de la interfaz de usuario del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-138">Razor Components can be used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="bfb1c-139">El marcado que aparece a continuación es un ejemplo de un componente de cuadro de diálogo personalizado:</span><span class="sxs-lookup"><span data-stu-id="bfb1c-139">The following markup is an example of a custom dialog component:</span></span>

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

<span data-ttu-id="bfb1c-140">Cuando este componente se usa en otra parte de la aplicación, IntelliSense agiliza el desarrollo con la finalización de parámetros y sintaxis.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-140">When this component is used elsewhere in the app, IntelliSense speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="bfb1c-141">Los componentes se representan en una representación en memoria del DOM del explorador conocida como *árbol de representación* que se puede usar para actualizar la interfaz de usuario de una manera eficaz y flexible.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-141">Components render into an in-memory representation of the browser DOM called a *render tree* that can then be used to update the UI in a flexible and efficient way.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="bfb1c-142">Interoperabilidad de JavaScript</span><span class="sxs-lookup"><span data-stu-id="bfb1c-142">JavaScript interop</span></span>

<span data-ttu-id="bfb1c-143">En el caso de aplicaciones que necesitan bibliotecas de JavaScript de terceros y API de explorador, los componentes interoperan con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-143">For apps that require third-party JavaScript libraries and browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="bfb1c-144">Los componentes pueden usar cualquier biblioteca o API que pueda utilizar JavaScript.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-144">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="bfb1c-145">El código de C# puede llamar a código JavaScript y, a su vez, el código JavaScript puede llamar a código de C#.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-145">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="bfb1c-146">Para obtener más información, vea [Interoperabilidad de JavaScript](xref:blazor/javascript-interop).</span><span class="sxs-lookup"><span data-stu-id="bfb1c-146">For more information, see [JavaScript interop](xref:blazor/javascript-interop).</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="bfb1c-147">Uso compartido de código y .NET Standard</span><span class="sxs-lookup"><span data-stu-id="bfb1c-147">Code sharing and .NET Standard</span></span>

<span data-ttu-id="bfb1c-148">Las aplicaciones pueden hacer referencia a bibliotecas de [.NET Standard](/dotnet/standard/net-standard) existentes y usarlas.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-148">Apps can reference and use existing [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="bfb1c-149">.NET Standard es una especificación formal de las API de .NET comunes entre las implementaciones de .NET.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-149">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="bfb1c-150">Blazor implementa .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-150">Blazor implements .NET Standard 2.0.</span></span> <span data-ttu-id="bfb1c-151">Las API que no pueden aplicarse dentro de un explorador web (por ejemplo, para acceder al sistema de archivos, abrir un socket, ejecutar subprocesos y otras características) generan una excepción <xref:System.PlatformNotSupportedException>.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-151">APIs that aren't applicable inside a web browser (for example, accessing the file system, opening a socket, threading, and other features) throw <xref:System.PlatformNotSupportedException>.</span></span> <span data-ttu-id="bfb1c-152">Las bibliotecas de clase .NET Standard pueden compartirse a través de diferentes plataformas .NET, como Blazor, .NET Framework, .NET Core, Xamarin, Mono y Unity.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-152">.NET Standard class libraries can be shared across different .NET platforms, like Blazor, .NET Framework, .NET Core, Xamarin, Mono, and Unity.</span></span>

## <a name="blazor"></a><span data-ttu-id="bfb1c-153">Blazor</span><span class="sxs-lookup"><span data-stu-id="bfb1c-153">Blazor</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="bfb1c-154">Blazor es un marco de aplicaciones de una sola página para compilar aplicaciones web interactivas del lado cliente con. NET.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-154">Blazor is a single-page app framework for building interactive client-side Web apps with .NET.</span></span> <span data-ttu-id="bfb1c-155">Blazor usa estándares web abiertos sin complementos ni transpilación de código.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-155">Blazor uses open web standards without plugins or code transpilation.</span></span> <span data-ttu-id="bfb1c-156">Blazor funciona en todos los exploradores web modernos, incluidos los exploradores móviles.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-156">Blazor works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="bfb1c-157">El uso de .NET en el explorador para el desarrollo web en el lado cliente ofrece muchas ventajas:</span><span class="sxs-lookup"><span data-stu-id="bfb1c-157">Using .NET in the browser for client-side web development offers many advantages:</span></span>

* <span data-ttu-id="bfb1c-158">**Lenguaje C#**: escriba código en C# en lugar de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-158">**C# language**: Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="bfb1c-159">**Ecosistema .NET**: aproveche el ecosistema existente de bibliotecas .NET.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-159">**.NET Ecosystem**: Leverage the existing ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="bfb1c-160">**Desarrollo de la pila completa**: comparta la lógica de cliente y servidor.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-160">**Full-stack development**: Share server and client-side logic.</span></span>
* <span data-ttu-id="bfb1c-161">**Velocidad y escalabilidad**: NET se creó pensando en el rendimiento, la confiabilidad y la seguridad.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-161">**Speed and scalability**: .NET was built for performance, reliability, and security.</span></span>
* <span data-ttu-id="bfb1c-162">**Herramientas líderes del sector**: mantenga la productividad con Visual Studio en Windows, Linux y macOS.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-162">**Industry-leading tools**: Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="bfb1c-163">**Estabilidad y coherencia**:  compile sobre un conjunto común de lenguajes, marcos y herramientas que son estables, completos y fáciles de usar.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-163">**Stability and consistency**:  Build on a commonset of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

<span data-ttu-id="bfb1c-164">La ejecución de código .NET dentro de exploradores web se consigue mediante [WebAssembly](http://webassembly.org) (abreviado como *wasm*).</span><span class="sxs-lookup"><span data-stu-id="bfb1c-164">Running .NET code inside web browsers is made possible by [WebAssembly](http://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="bfb1c-165">WebAssembly es un estándar web abierto y se admite en exploradores web sin complementos.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-165">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span> <span data-ttu-id="bfb1c-166">WebAssembly es un formato de código de bytes compacto optimizado para descargas rápidas y una velocidad de ejecución máxima.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-166">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span>

<span data-ttu-id="bfb1c-167">El código de WebAssembly puede acceder a todas las funciones del explorador mediante la interoperabilidad de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-167">WebAssembly code can access the full functionality of the browser via JavaScript interop.</span></span> <span data-ttu-id="bfb1c-168">Al mismo tiempo, el código .NET ejecutado mediante WebAssembly funciona en el mismo espacio aislado de confianza que JavaScript para impedir acciones malintencionadas en la máquina cliente.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-168">At the same time, .NET code executed via WebAssembly runs in the same trusted sandbox as JavaScript to prevent malicious actions on the client machine.</span></span>

![Blazor ejecuta código de .NET en el explorador con WebAssembly.](index/_static/blazor.png)

<span data-ttu-id="bfb1c-170">Cuando se compila y ejecuta una aplicación de Blazor en un explorador:</span><span class="sxs-lookup"><span data-stu-id="bfb1c-170">When a Blazor app is built and run in a browser:</span></span>

* <span data-ttu-id="bfb1c-171">Los archivos de código de C# y los archivos de Razor se compilan en ensamblados de .NET.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-171">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="bfb1c-172">Los ensamblados y el entorno de ejecución de .NET se descargan en el explorador.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-172">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="bfb1c-173">Blazor arranca el entorno de ejecución .NET y lo configura para cargar los ensamblados de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-173">Blazor bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="bfb1c-174">El entorno de ejecución de Blazor controla la manipulación de Document Object Model (DOM) y las llamadas API del explorador mediante la interoperabilidad de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-174">Document Object Model (DOM) manipulation and browser API calls are handled by the Blazor runtime via JavaScript interop.</span></span>

<span data-ttu-id="bfb1c-175">Blazor admite medios básicos requeridos por la mayoría de las aplicaciones, incluidos:</span><span class="sxs-lookup"><span data-stu-id="bfb1c-175">Blazor supports core facilities required by most apps, including:</span></span>

* <span data-ttu-id="bfb1c-176">Parámetros</span><span class="sxs-lookup"><span data-stu-id="bfb1c-176">Parameters</span></span>
* <span data-ttu-id="bfb1c-177">Control de eventos</span><span class="sxs-lookup"><span data-stu-id="bfb1c-177">Event handling</span></span>
* <span data-ttu-id="bfb1c-178">Enlace de datos</span><span class="sxs-lookup"><span data-stu-id="bfb1c-178">Data binding</span></span>
* <span data-ttu-id="bfb1c-179">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="bfb1c-179">Routing</span></span>
* <span data-ttu-id="bfb1c-180">Inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="bfb1c-180">Dependency injection</span></span>
* <span data-ttu-id="bfb1c-181">Diseños</span><span class="sxs-lookup"><span data-stu-id="bfb1c-181">Layouts</span></span>
* <span data-ttu-id="bfb1c-182">Plantillas</span><span class="sxs-lookup"><span data-stu-id="bfb1c-182">Templating</span></span>
* <span data-ttu-id="bfb1c-183">Valores en cascada</span><span class="sxs-lookup"><span data-stu-id="bfb1c-183">Cascading values</span></span>

<span data-ttu-id="bfb1c-184">Para reducir el tamaño de la aplicación descargada, se ha quitado el código sin usar de la aplicación cuando se publica mediante el [enlazador del lenguaje intermedio (IL)](xref:host-and-deploy/blazor/configure-linker).</span><span class="sxs-lookup"><span data-stu-id="bfb1c-184">To reduce the size of the downloaded app, unused code is stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/blazor/configure-linker).</span></span>

<span data-ttu-id="bfb1c-185">El lado de cliente de Blazor es un modelo de hospedaje del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-185">Blazor client-side is a client-side hosting model.</span></span> <span data-ttu-id="bfb1c-186">Como Razor separa la lógica de representación de un componente de la forma en que se aplican las actualizaciones de la interfaz de usuario, existe flexibilidad para hospedar Razor.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-186">Because Blazor decouples a component's rendering logic from how UI updates are applied, there's flexibility in how Blazor can be hosted.</span></span> <span data-ttu-id="bfb1c-187">Use [Razor Components](#razor-components) de ASP.NET Core para hospedar Razor Components en el servidor en una aplicación de ASP.NET Core, donde todas las actualizaciones de la interfaz de usuario se controlan mediante una conexión de SignalR.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-187">Use ASP.NET Core [Razor Components](#razor-components) to host Razor Components on the server in an ASP.NET Core app where UI updates are handled over a SignalR connection.</span></span> <span data-ttu-id="bfb1c-188">Para obtener más información, vea <xref:blazor/hosting-models#server-side-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-188">For more information, see <xref:blazor/hosting-models#server-side-hosting-model>.</span></span> 

<span data-ttu-id="bfb1c-189">El tamaño de carga es un factor de rendimiento críticos para la facilidad de uso de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-189">Payload size is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="bfb1c-190">Blazor optimiza el tamaño de carga para reducir los tiempos de descarga:</span><span class="sxs-lookup"><span data-stu-id="bfb1c-190">Blazor optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="bfb1c-191">Las partes no usadas de los ensamblados .NET se quitan durante el proceso de compilación.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-191">Unused parts of .NET assemblies are removed during the build process.</span></span>
* <span data-ttu-id="bfb1c-192">Las respuestas HTTP se comprimen.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-192">HTTP responses are compressed.</span></span>
* <span data-ttu-id="bfb1c-193">El entorno de ejecución .NET y los ensamblados se almacenan en caché en el explorador.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-193">The .NET runtime and assemblies are cached in the browser.</span></span>

<span data-ttu-id="bfb1c-194">[Razor Components](#razor-components) proporciona un tamaño de carga más pequeño que Blazor al mantener los ensamblados de .NET, el ensamblado de la aplicación y el entorno de ejecución en el lado servidor.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-194">[Razor Components](#razor-components) provides a smaller payload size than Blazor by maintaining .NET assemblies, the app's assembly, and the runtime server-side.</span></span> <span data-ttu-id="bfb1c-195">Las aplicaciones de Razor Components solo proporcionan archivos de marcado y recursos estáticos a los clientes.</span><span class="sxs-lookup"><span data-stu-id="bfb1c-195">Razor Components apps only serve markup files and static assets to clients.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bfb1c-196">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="bfb1c-196">Additional resources</span></span>

* [<span data-ttu-id="bfb1c-197">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="bfb1c-197">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="bfb1c-198">Guía de C#</span><span class="sxs-lookup"><span data-stu-id="bfb1c-198">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="bfb1c-199">HTML</span><span class="sxs-lookup"><span data-stu-id="bfb1c-199">HTML</span></span>](https://www.w3.org/html/)
