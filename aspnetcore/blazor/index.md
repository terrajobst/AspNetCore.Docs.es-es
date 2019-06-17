---
title: Introducción a Blazor en ASP.NET Core
author: guardrex
description: Explore Blazor en ASP.NET Core, una forma de compilar la interfaz de usuario web interactiva del lado cliente con .NET en una aplicación ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc, seoapril2019
ms.date: 05/01/2019
uid: blazor/index
ms.openlocfilehash: d58115b17536cad0b3927e6d32b7dbe8db8e4b0f
ms.sourcegitcommit: 335a88c1b6e7f0caa8a3a27db57c56664d676d34
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/12/2019
ms.locfileid: "67034425"
---
# <a name="introduction-to-blazor"></a><span data-ttu-id="2af4a-103">Introducción a Blazor</span><span class="sxs-lookup"><span data-stu-id="2af4a-103">Introduction to Blazor</span></span>

<span data-ttu-id="2af4a-104">Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2af4a-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="2af4a-105">*Le damos la bienvenida a Blazor*.</span><span class="sxs-lookup"><span data-stu-id="2af4a-105">*Welcome to Blazor!*</span></span>

<span data-ttu-id="2af4a-106">Blazor es un marco de trabajo para la creación de interfaces de usuario web interactivas del lado cliente con .NET:</span><span class="sxs-lookup"><span data-stu-id="2af4a-106">Blazor is a framework for building interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="2af4a-107">Cree interfaces de usuario completamente interactivas con C# en lugar de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2af4a-107">Create rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="2af4a-108">Comparta la lógica de aplicación del lado cliente y servidor escrita con. NET.</span><span class="sxs-lookup"><span data-stu-id="2af4a-108">Share server-side and client-side app logic written with .NET.</span></span>
* <span data-ttu-id="2af4a-109">Represente la interfaz de usuario como HTML y CSS para la compatibilidad con todos los exploradores, incluidos los móviles.</span><span class="sxs-lookup"><span data-stu-id="2af4a-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="2af4a-110">El uso de .NET para el desarrollo web en el lado cliente ofrece las siguientes ventajas:</span><span class="sxs-lookup"><span data-stu-id="2af4a-110">Using .NET for client-side web development offers the following advantages:</span></span>

* <span data-ttu-id="2af4a-111">escriba código en C# en lugar de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2af4a-111">Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="2af4a-112">Aprovechamiento del ecosistema y las bibliotecas .NET existentes.</span><span class="sxs-lookup"><span data-stu-id="2af4a-112">Leverage the existing .NET ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="2af4a-113">Uso compartido de la lógica de aplicación en el servidor y el cliente.</span><span class="sxs-lookup"><span data-stu-id="2af4a-113">Share app logic across the server and client.</span></span>
* <span data-ttu-id="2af4a-114">Beneficios de rendimiento, confiabilidad y seguridad de .NET.</span><span class="sxs-lookup"><span data-stu-id="2af4a-114">Benefit from .NET's performance, reliability, and security.</span></span>
* <span data-ttu-id="2af4a-115">mantenga la productividad con Visual Studio en Windows, Linux y macOS.</span><span class="sxs-lookup"><span data-stu-id="2af4a-115">Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="2af4a-116">compile sobre un conjunto común de lenguajes, marcos y herramientas que son estables, completos y fáciles de usar.</span><span class="sxs-lookup"><span data-stu-id="2af4a-116">Build on a common set of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

## <a name="components"></a><span data-ttu-id="2af4a-117">Componentes</span><span class="sxs-lookup"><span data-stu-id="2af4a-117">Components</span></span>

<span data-ttu-id="2af4a-118">Las aplicaciones de Blazor se basan en *componentes*.</span><span class="sxs-lookup"><span data-stu-id="2af4a-118">Blazor apps are based on *components*.</span></span> <span data-ttu-id="2af4a-119">Un componente en Blazor es un elemento de la interfaz de usuario, como una página, un cuadro de diálogo o un formulario de entrada de datos.</span><span class="sxs-lookup"><span data-stu-id="2af4a-119">A component in Blazor is an element of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="2af4a-120">Los componentes controlan los eventos de usuario y definen la lógica de representación flexible de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="2af4a-120">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="2af4a-121">Los componentes se pueden anidar y reutilizar.</span><span class="sxs-lookup"><span data-stu-id="2af4a-121">Components can be nested and reused.</span></span>

<span data-ttu-id="2af4a-122">Los componentes son clases .NET integradas en ensamblados .NET que se pueden compartir y distribuir como [paquetes NuGet](/nuget/what-is-nuget).</span><span class="sxs-lookup"><span data-stu-id="2af4a-122">Components are .NET classes built into .NET assemblies that can be shared and distributed as [NuGet packages](/nuget/what-is-nuget).</span></span> <span data-ttu-id="2af4a-123">La clase del componente se escribe normalmente en forma de una página de marcado de Razor con una extensión de archivo *.razor*.</span><span class="sxs-lookup"><span data-stu-id="2af4a-123">The component class is usually written in the form of a Razor markup page with a *.razor* file extension.</span></span>

<span data-ttu-id="2af4a-124">A veces, se hace referencia a los componentes de Blazor como *componentes de Razor*.</span><span class="sxs-lookup"><span data-stu-id="2af4a-124">Components in Blazor are sometimes referred to as *Razor components*.</span></span> <span data-ttu-id="2af4a-125">[Razor](xref:mvc/views/razor) es una sintaxis para combinar marcado HTML con código de C# diseñada para aumentar la productividad del desarrollador.</span><span class="sxs-lookup"><span data-stu-id="2af4a-125">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code designed for developer productivity.</span></span> <span data-ttu-id="2af4a-126">Razor le permite cambiar entre marcado HTML y C# en el mismo archivo gracias a la compatibilidad con [IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="2af4a-126">Razor allows you to switch between HTML markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="2af4a-127">Razor Pages y MVC también usan Razor.</span><span class="sxs-lookup"><span data-stu-id="2af4a-127">Razor Pages and MVC also use Razor.</span></span> <span data-ttu-id="2af4a-128">A diferencia de Razor Pages y MVC, que se compilan en torno a un modelo de solicitud y respuesta, los componentes se usan específicamente en la composición y la lógica de la interfaz de usuario del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="2af4a-128">Unlike Razor Pages and MVC, which are built around a request/response model, components are used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="2af4a-129">El siguiente marcado de Razor muestra un componente (*Dialog.razor*), que se puede anidar dentro de otro componente:</span><span class="sxs-lookup"><span data-stu-id="2af4a-129">The following Razor markup demonstrates a component (*Dialog.razor*), which can be nested within another component:</span></span>

```cshtml
<div>
    <h1>@Title</h1>

    @ChildContent

    <button @onclick="@OnYes">Yes!</button>
</div>

@code {
    [Parameter]
    private string Title { get; set; }

    [Parameter]
    private RenderFragment ChildContent { get; set; }

    private void OnYes()
    {
        Console.WriteLine("Write to the console in C#!");
    }
}
```

<span data-ttu-id="2af4a-130">El contenido del cuerpo (`ChildContent`) y el título (`Title`) del cuadro de diálogo los proporciona el componente que usa este componente en su interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="2af4a-130">The dialog's body content (`ChildContent`) and title (`Title`) are provided by the component that uses this component in its UI.</span></span> <span data-ttu-id="2af4a-131">`OnYes` es un método de C# desencadenado por el evento `onclick` del botón.</span><span class="sxs-lookup"><span data-stu-id="2af4a-131">`OnYes` is a C# method triggered by the button's `onclick` event.</span></span>

<span data-ttu-id="2af4a-132">Blazor usa etiquetas HTML naturales para la composición de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="2af4a-132">Blazor uses natural HTML tags for UI composition.</span></span> <span data-ttu-id="2af4a-133">Los elementos HTML especifican componentes y los atributos de la etiqueta pasan valores a las propiedades del componente.</span><span class="sxs-lookup"><span data-stu-id="2af4a-133">HTML elements specify components, and a tag's attributes pass values to a component's properties.</span></span> <span data-ttu-id="2af4a-134">`ChildContent` y `Title` se establecen mediante el componente que usa el componente Dialog (*Index.razor*):</span><span class="sxs-lookup"><span data-stu-id="2af4a-134">`ChildContent` and `Title` are set by the component that uses the Dialog component (*Index.razor*):</span></span>

```cshtml
@page "/"

<h1>Hello, world!</h1>

Welcome to your new app.

<Dialog Title="Blazor">
    Do you want to <i>learn more</i> about Blazor?
</Dialog>
```

<span data-ttu-id="2af4a-135">El cuadro de diálogo se representa cuando se accede al elemento principal (*Index.razor*) en un explorador:</span><span class="sxs-lookup"><span data-stu-id="2af4a-135">The dialog is rendered when the parent (*Index.razor*) is accessed in a browser:</span></span>

![Componente Dialog representado en el explorador](index/_static/dialog.png)

<span data-ttu-id="2af4a-137">Cuando este componente se usa en la aplicación, IntelliSense en [Visual Studio](/visualstudio/ide/using-intellisense) y [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) acelera el desarrollo con la finalización de parámetros y sintaxis.</span><span class="sxs-lookup"><span data-stu-id="2af4a-137">When this component is used in the app, IntelliSense in [Visual Studio](/visualstudio/ide/using-intellisense) and [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="2af4a-138">Los componentes se representan en una representación en memoria del DOM del explorador conocida como *árbol de representación* que se usa para actualizar la interfaz de usuario de una manera eficaz y flexible.</span><span class="sxs-lookup"><span data-stu-id="2af4a-138">Components render into an in-memory representation of the browser DOM called a *render tree* that's used to update the UI in a flexible and efficient way.</span></span>

## <a name="blazor-client-side"></a><span data-ttu-id="2af4a-139">Lado cliente de Blazor</span><span class="sxs-lookup"><span data-stu-id="2af4a-139">Blazor client-side</span></span>

<span data-ttu-id="2af4a-140">El lado cliente de Blazor es un marco de trabajo de aplicación de una única página para la creación de aplicaciones web interactivas en el lado cliente con. NET.</span><span class="sxs-lookup"><span data-stu-id="2af4a-140">Blazor client-side is a single-page app framework for building interactive client-side web apps with .NET.</span></span> <span data-ttu-id="2af4a-141">El cliente de Blazor usa estándares web abiertos sin complementos o transpilación de código y funciona en todos los exploradores web modernos, incluidos los exploradores para dispositivos móviles.</span><span class="sxs-lookup"><span data-stu-id="2af4a-141">Blazor client-side uses open web standards without plugins or code transpilation and works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="2af4a-142">La ejecución de código .NET dentro de exploradores web se consigue mediante [WebAssembly](http://webassembly.org) (abreviado como *wasm*).</span><span class="sxs-lookup"><span data-stu-id="2af4a-142">Running .NET code inside web browsers is made possible by [WebAssembly](http://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="2af4a-143">WebAssembly es un estándar web abierto y se admite en exploradores web sin complementos.</span><span class="sxs-lookup"><span data-stu-id="2af4a-143">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span> <span data-ttu-id="2af4a-144">WebAssembly es un formato de código de bytes compacto optimizado para descargas rápidas y una velocidad de ejecución máxima.</span><span class="sxs-lookup"><span data-stu-id="2af4a-144">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span>

<span data-ttu-id="2af4a-145">El código de WebAssembly puede acceder a toda la funcionalidad del explorador mediante la *interoperabilidad de JavaScript*.</span><span class="sxs-lookup"><span data-stu-id="2af4a-145">WebAssembly code can access the full functionality of the browser via JavaScript, called *JavaScript interoperability* (or *JavaScript interop*).</span></span> <span data-ttu-id="2af4a-146">El código de .NET que se ejecuta mediante WebAssembly en el explorador lo hace en el mismo espacio aislado de confianza que JavaScript, lo que elimina prácticamente la posibilidad de que una aplicación realice acciones malintencionadas en la máquina cliente.</span><span class="sxs-lookup"><span data-stu-id="2af4a-146">.NET code executed via WebAssembly in the browser runs in the same trusted sandbox as JavaScript, which virtually eliminates the opportunity for an app to perform malicious actions on the client machine.</span></span>

![El lado cliente de Blazor ejecuta código de .NET en el explorador con WebAssembly.](index/_static/blazor-client-side.png)

<span data-ttu-id="2af4a-148">Cuando se compila y ejecuta una aplicación del lado cliente de Blazor en un explorador:</span><span class="sxs-lookup"><span data-stu-id="2af4a-148">When a Blazor client-side app is built and run in a browser:</span></span>

* <span data-ttu-id="2af4a-149">Los archivos de código de C# y los archivos de Razor se compilan en ensamblados de .NET.</span><span class="sxs-lookup"><span data-stu-id="2af4a-149">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="2af4a-150">Los ensamblados y el entorno de ejecución de .NET se descargan en el explorador.</span><span class="sxs-lookup"><span data-stu-id="2af4a-150">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="2af4a-151">El lado cliente de Blazor arranca el entorno de ejecución .NET y lo configura para cargar los ensamblados de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2af4a-151">Blazor client-side bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="2af4a-152">El entorno de ejecución del lado cliente de Blazor emplea la interoperabilidad de JavaScript para gestionar la manipulación de Document Object Model (DOM) y las llamadas API del explorador.</span><span class="sxs-lookup"><span data-stu-id="2af4a-152">The Blazor client-side runtime uses JavaScript interop to handle Document Object Model (DOM) manipulation and browser API calls.</span></span>

<span data-ttu-id="2af4a-153">El tamaño de la aplicación publicada, su *tamaño de carga*, es un factor de rendimiento crítico para la facilidad de uso de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="2af4a-153">The size of the published app, its *payload size*, is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="2af4a-154">Una aplicación grande tarda un tiempo relativamente largo en descargarse en un explorador, lo que repercute en la experiencia del usuario.</span><span class="sxs-lookup"><span data-stu-id="2af4a-154">A large app takes a relatively long time to download to a browser, which hurts the user experience.</span></span> <span data-ttu-id="2af4a-155">El lado cliente de Blazor optimiza el tamaño de carga para reducir los tiempos de descarga:</span><span class="sxs-lookup"><span data-stu-id="2af4a-155">Blazor client-side optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="2af4a-156">se ha quitado el código sin usar de la aplicación cuando se publica mediante el [enlazador del lenguaje intermedio (IL)](xref:host-and-deploy/blazor/configure-linker).</span><span class="sxs-lookup"><span data-stu-id="2af4a-156">Unused code is stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/blazor/configure-linker).</span></span>
* <span data-ttu-id="2af4a-157">Las respuestas HTTP se comprimen.</span><span class="sxs-lookup"><span data-stu-id="2af4a-157">HTTP responses are compressed.</span></span>
* <span data-ttu-id="2af4a-158">El entorno de ejecución .NET y los ensamblados se almacenan en caché en el explorador.</span><span class="sxs-lookup"><span data-stu-id="2af4a-158">The .NET runtime and assemblies are cached in the browser.</span></span>

<span data-ttu-id="2af4a-159">Parar más información e instrucciones sobre cómo elegir un modelo de hospedaje, consulte <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="2af4a-159">For more information and guidance on choosing a hosting model, see <xref:blazor/hosting-models>.</span></span>

## <a name="blazor-server-side"></a><span data-ttu-id="2af4a-160">Lado servidor de Blazor</span><span class="sxs-lookup"><span data-stu-id="2af4a-160">Blazor server-side</span></span>

<span data-ttu-id="2af4a-161">Blazor separa la lógica de representación de componentes del modo en que se aplican las actualizaciones de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="2af4a-161">Blazor decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="2af4a-162">El lado servidor de Blazor agrega compatibilidad con el hospedaje de componentes de Razor en el servidor en una aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2af4a-162">Blazor server-side provides support for hosting Razor components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="2af4a-163">Las actualizaciones de la interfaz de usuario se controlan mediante una conexión de [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="2af4a-163">UI updates are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="2af4a-164">El entorno de ejecución controla el envío de eventos de la interfaz de usuario del explorador al servidor y, después de ejecutar los componentes, vuelve a aplicar al explorador las actualizaciones de la interfaz de usuario enviadas por el servidor.</span><span class="sxs-lookup"><span data-stu-id="2af4a-164">The runtime handles sending UI events from the browser to the server and applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="2af4a-165">La conexión utilizada por el lado servidor de Blazor para comunicarse con el explorador también se usa para controlar las llamadas de interoperabilidad de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2af4a-165">The connection used by Blazor server-side to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![El lado servidor de Blazor ejecuta código de .NET en el servidor e interactúa con Document Object Model en el cliente mediante una conexión de SignalR.](index/_static/blazor-server-side.png)

<span data-ttu-id="2af4a-167">Para más información e instrucciones sobre cómo elegir un modelo de hospedaje, consulte <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="2af4a-167">For more information and guidance on choosing a hosting model, see <xref:blazor/hosting-models>.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="2af4a-168">Interoperabilidad de JavaScript</span><span class="sxs-lookup"><span data-stu-id="2af4a-168">JavaScript interop</span></span>

<span data-ttu-id="2af4a-169">En el caso de aplicaciones que necesitan bibliotecas de JavaScript de terceros y API de explorador, los componentes interoperan con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2af4a-169">For apps that require third-party JavaScript libraries and browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="2af4a-170">Los componentes pueden usar cualquier biblioteca o API que pueda utilizar JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2af4a-170">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="2af4a-171">El código de C# puede llamar a código JavaScript y, a su vez, el código JavaScript puede llamar a código de C#.</span><span class="sxs-lookup"><span data-stu-id="2af4a-171">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="2af4a-172">Para obtener más información, vea <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="2af4a-172">For more information, see <xref:blazor/javascript-interop>.</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="2af4a-173">Uso compartido de código y .NET Standard</span><span class="sxs-lookup"><span data-stu-id="2af4a-173">Code sharing and .NET Standard</span></span>

<span data-ttu-id="2af4a-174">Blazor implementa [.NET Standard 2.0](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="2af4a-174">Blazor implements [.NET Standard 2.0](/dotnet/standard/net-standard).</span></span> <span data-ttu-id="2af4a-175">.NET Standard es una especificación formal de las API de .NET comunes entre las implementaciones de .NET.</span><span class="sxs-lookup"><span data-stu-id="2af4a-175">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="2af4a-176">Las bibliotecas de clase .NET Standard pueden compartirse a través de diferentes plataformas .NET, como Blazor, .NET Framework, .NET Core, Xamarin, Mono y Unity.</span><span class="sxs-lookup"><span data-stu-id="2af4a-176">.NET Standard class libraries can be shared across different .NET platforms, such as Blazor, .NET Framework, .NET Core, Xamarin, Mono, and Unity.</span></span>

<span data-ttu-id="2af4a-177">Las API que no pueden aplicarse dentro de un explorador web (por ejemplo, para acceder al sistema de archivos, abrir un socket y ejecutar subprocesos) generan una excepción <xref:System.PlatformNotSupportedException>.</span><span class="sxs-lookup"><span data-stu-id="2af4a-177">APIs that aren't applicable inside a web browser (for example, accessing the file system, opening a socket, and threading) throw a <xref:System.PlatformNotSupportedException>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2af4a-178">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="2af4a-178">Additional resources</span></span>

* [<span data-ttu-id="2af4a-179">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="2af4a-179">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="2af4a-180">Guía de C#</span><span class="sxs-lookup"><span data-stu-id="2af4a-180">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="2af4a-181">HTML</span><span class="sxs-lookup"><span data-stu-id="2af4a-181">HTML</span></span>](https://www.w3.org/html/)
