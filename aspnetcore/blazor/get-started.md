---
title: Introducción a ASP.NET Core extraordinarias
author: guardrex
description: Para empezar a trabajar con más increíble, cree una aplicación increíblemente con las herramientas de su elección.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: blazor/get-started
ms.openlocfilehash: cce91b6332295f77c639f881fe342b625fee7fca
ms.sourcegitcommit: 92c901c7f32ee9efb335d99ec4c3add2cc9f3142
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/16/2019
ms.locfileid: "71025511"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="5d2e0-103">Introducción a ASP.NET Core extraordinarias</span><span class="sxs-lookup"><span data-stu-id="5d2e0-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="5d2e0-104">Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5d2e0-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="5d2e0-105">Introducción a más increíble:</span><span class="sxs-lookup"><span data-stu-id="5d2e0-105">Get started with Blazor:</span></span>

1. <span data-ttu-id="5d2e0-106">Instale la versión más reciente del [SDK de .net Core 3,0 Preview](https://dotnet.microsoft.com/download/dotnet-core/3.0) .</span><span class="sxs-lookup"><span data-stu-id="5d2e0-106">Install the latest [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>

1. <span data-ttu-id="5d2e0-107">Instale las plantillas extraordinarias ejecutando el comando siguiente en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="5d2e0-107">Install the Blazor templates by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.0.0-preview9.19457.4
   ```

1. <span data-ttu-id="5d2e0-108">Siga las instrucciones para su elección de herramientas:</span><span class="sxs-lookup"><span data-stu-id="5d2e0-108">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5d2e0-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5d2e0-109">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="5d2e0-110">1 \.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-110">1\.</span></span> <span data-ttu-id="5d2e0-111">Instale la versión preliminar más reciente de [Visual Studio](https://visualstudio.com/vs/preview) con la carga de trabajo de **desarrollo web y ASP.net** .</span><span class="sxs-lookup"><span data-stu-id="5d2e0-111">Install the latest [Visual Studio preview](https://visualstudio.com/vs/preview) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="5d2e0-112">2 \.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-112">2\.</span></span> <span data-ttu-id="5d2e0-113">Cree un nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-113">Create a new project.</span></span>

   <span data-ttu-id="5d2e0-114">3 \.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-114">3\.</span></span> <span data-ttu-id="5d2e0-115">Seleccione **aplicación extraordinaria**.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-115">Select **Blazor App**.</span></span> <span data-ttu-id="5d2e0-116">Seleccione **Next** (Siguiente).</span><span class="sxs-lookup"><span data-stu-id="5d2e0-116">Select **Next**.</span></span>

   <span data-ttu-id="5d2e0-117">4 \.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-117">4\.</span></span> <span data-ttu-id="5d2e0-118">Proporcione un nombre para el proyecto en el campo **Nombre del proyecto** o acepte el predeterminado.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-118">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="5d2e0-119">Confirme que la entrada de **Ubicación** es correcta o proporcione una ubicación para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-119">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="5d2e0-120">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-120">Select **Create**.</span></span>

   <span data-ttu-id="5d2e0-121">5 \.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-121">5\.</span></span> <span data-ttu-id="5d2e0-122">Para disfrutar de una experiencia de webassembly increíblemente rápida, elija la plantilla de **aplicación de Webassemble más brillante** .</span><span class="sxs-lookup"><span data-stu-id="5d2e0-122">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="5d2e0-123">Para obtener una experiencia de servidor increíblemente rápida, elija la plantilla de **aplicación de servidor de extraordinarias** .</span><span class="sxs-lookup"><span data-stu-id="5d2e0-123">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="5d2e0-124">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-124">Select **Create**.</span></span> <span data-ttu-id="5d2e0-125">Para obtener más información sobre los dos modelos de hospedaje más increíbles, el *servidor* y el *webensamblado*increíble, vea <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-125">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="5d2e0-126">6 \.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-126">6\.</span></span> <span data-ttu-id="5d2e0-127">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-127">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="5d2e0-128">Si ha instalado la extensión de Visual Studio de extraordinarias para una versión preliminar anterior de ASP.NET Core extraordinaria (versión preliminar 6 o anterior), puede desinstalar la extensión.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-128">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="5d2e0-129">Instalar las plantillas de extraordinarias en un shell de comandos ahora es suficiente para mostrar las plantillas en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-129">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5d2e0-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5d2e0-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="5d2e0-131">1 \.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-131">1\.</span></span> <span data-ttu-id="5d2e0-132">Instale [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="5d2e0-132">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="5d2e0-133">2 \.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-133">2\.</span></span> <span data-ttu-id="5d2e0-134">Instale la [ C# extensión de Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)más reciente.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-134">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="5d2e0-135">3 \.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-135">3\.</span></span> <span data-ttu-id="5d2e0-136">En el caso de una experiencia de webassembler extraordinaria, ejecute el siguiente comando en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="5d2e0-136">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```console
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="5d2e0-137">Para obtener una experiencia de servidor extraordinaria, ejecute el siguiente comando en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="5d2e0-137">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```console
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="5d2e0-138">Para obtener más información sobre los dos modelos de hospedaje más increíbles, el *servidor* y el *webensamblado*increíble, vea <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-138">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="5d2e0-139">4 \.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-139">4\.</span></span> <span data-ttu-id="5d2e0-140">Abra la carpeta *WebApplication1* en Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-140">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="5d2e0-141">5 \.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-141">5\.</span></span> <span data-ttu-id="5d2e0-142">En el caso de un proyecto de servidor más rápido, el IDE solicita que agregue recursos para compilar y depurar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-142">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="5d2e0-143">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-143">Select **Yes**.</span></span>

   <span data-ttu-id="5d2e0-144">6 \.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-144">6\.</span></span> <span data-ttu-id="5d2e0-145">Si usa una aplicación de servidor más brillante, ejecute la aplicación con el depurador de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-145">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="5d2e0-146">Si usa una aplicación webassembly increíblemente, ejecute `dotnet run` desde la carpeta de proyecto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-146">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="5d2e0-147">7 \.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-147">7\.</span></span> <span data-ttu-id="5d2e0-148">En un navegador, vaya a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-148">In a browser, navigate to `https://localhost:5001`.</span></span>

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor Server experience, select the **Blazor Server App** template. For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="5d2e0-149">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="5d2e0-149">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="5d2e0-150">En el caso de una experiencia de webassembler extraordinaria, ejecute los siguientes comandos en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="5d2e0-150">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```console
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="5d2e0-151">Para obtener una experiencia de servidor extraordinaria, ejecute los siguientes comandos en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="5d2e0-151">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```console
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="5d2e0-152">Para obtener más información sobre los dos modelos de hospedaje más increíbles, el *servidor* y el *webensamblado*increíble, vea <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-152">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="5d2e0-153">En un navegador, vaya a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-153">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

<span data-ttu-id="5d2e0-154">Hay varias páginas disponibles en las pestañas de la barra lateral:</span><span class="sxs-lookup"><span data-stu-id="5d2e0-154">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="5d2e0-155">Página principal</span><span class="sxs-lookup"><span data-stu-id="5d2e0-155">Home</span></span>
* <span data-ttu-id="5d2e0-156">Contador</span><span class="sxs-lookup"><span data-stu-id="5d2e0-156">Counter</span></span>
* <span data-ttu-id="5d2e0-157">Capturar datos</span><span class="sxs-lookup"><span data-stu-id="5d2e0-157">Fetch data</span></span>

<span data-ttu-id="5d2e0-158">En la página Contador, seleccione el botón **Click me** para aumentar el contador sin una actualización de página.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-158">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="5d2e0-159">El incremento de un contador en una página web normalmente requiere la escritura de JavaScript, pero los componentes de C#Razor proporcionan un enfoque mejor mediante.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-159">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor components provide a better approach using C#.</span></span>

<span data-ttu-id="5d2e0-160">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="5d2e0-160">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="5d2e0-161">Una solicitud de `/counter` en el explorador, tal como se especifica `@page` en la Directiva en la parte superior `Counter` , hace que el componente represente su contenido.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-161">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="5d2e0-162">Los componentes se representan en una representación en memoria del árbol de representación que se puede usar para actualizar la interfaz de usuario de una forma flexible y eficaz.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-162">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="5d2e0-163">Cada vez que se selecciona el botón **click me** :</span><span class="sxs-lookup"><span data-stu-id="5d2e0-163">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="5d2e0-164">Se `onclick` desencadena el evento.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-164">The `onclick` event is fired.</span></span>
* <span data-ttu-id="5d2e0-165">Se llama al método `IncrementCount` .</span><span class="sxs-lookup"><span data-stu-id="5d2e0-165">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="5d2e0-166">`currentCount` Se incrementa.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-166">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="5d2e0-167">El componente se representará de nuevo.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-167">The component is rendered again.</span></span>

<span data-ttu-id="5d2e0-168">El tiempo de ejecución compara el nuevo contenido con el contenido anterior y solo aplica el contenido cambiado al Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="5d2e0-168">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="5d2e0-169">Agregar un componente a otro componente mediante la sintaxis HTML.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-169">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="5d2e0-170">Por ejemplo, agregue el `Counter` componente a la Página principal de la aplicación agregando un `<Counter />` elemento `Index` al componente.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-170">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="5d2e0-171">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="5d2e0-171">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="5d2e0-172">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-172">Run the app.</span></span> <span data-ttu-id="5d2e0-173">La Página principal tiene su propio contador proporcionado por `Counter` el componente.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-173">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="5d2e0-174">Los parámetros de componente se especifican mediante atributos o [contenido secundario](xref:blazor/components#child-content), que permiten establecer propiedades en el componente secundario.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-174">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="5d2e0-175">Para agregar un parámetro al `Counter` componente, actualice el bloque del `@code` componente:</span><span class="sxs-lookup"><span data-stu-id="5d2e0-175">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="5d2e0-176">Agregue una propiedad pública para `IncrementAmount` con un `[Parameter]` atributo.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-176">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="5d2e0-177">Cambie el método `IncrementCount` para usar `IncrementAmount` al aumentar el valor de `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-177">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="5d2e0-178">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="5d2e0-178">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="5d2e0-179">Especifique en el `Index` elemento del `<Counter>` componente mediante un atributo. `IncrementAmount`</span><span class="sxs-lookup"><span data-stu-id="5d2e0-179">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="5d2e0-180">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="5d2e0-180">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="5d2e0-181">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-181">Run the app.</span></span> <span data-ttu-id="5d2e0-182">El `Index` componente tiene su propio contador que se incrementa en diez cada vez que se selecciona el botón **click me** .</span><span class="sxs-lookup"><span data-stu-id="5d2e0-182">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="5d2e0-183">El `Counter` componente (*Counter. Razor*) de `/counter` continúa aumentando en uno.</span><span class="sxs-lookup"><span data-stu-id="5d2e0-183">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5d2e0-184">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="5d2e0-184">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="5d2e0-185">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="5d2e0-185">Additional resources</span></span>

* <xref:signalr/introduction>
