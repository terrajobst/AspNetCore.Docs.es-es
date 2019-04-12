---
title: Empezar a trabajar con componentes de Razor
author: guardrex
description: Obtenga información sobre cómo empezar a trabajar con componentes de Razor mediante la creación y modificación de un proyecto de componentes de Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: razor-components/get-started
ms.openlocfilehash: 151e58497b0f22fa7c5a9bde1f665eeb73fd5dc3
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59515652"
---
# <a name="get-started-with-razor-components"></a><span data-ttu-id="f27db-103">Empezar a trabajar con componentes de Razor</span><span class="sxs-lookup"><span data-stu-id="f27db-103">Get started with Razor Components</span></span>

<span data-ttu-id="f27db-104">Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f27db-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

# [<a name="visual-studio"></a><span data-ttu-id="f27db-105">Programa para la mejora</span><span class="sxs-lookup"><span data-stu-id="f27db-105">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f27db-106">Requisitos previos:</span><span class="sxs-lookup"><span data-stu-id="f27db-106">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="f27db-107">Para crear su primer proyecto de componentes de Razor en Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="f27db-107">To create your first Razor Components project in Visual Studio:</span></span>

1. <span data-ttu-id="f27db-108">Instale la última versión [SDK de .NET Core 3.0 Preview](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span><span class="sxs-lookup"><span data-stu-id="f27db-108">Install the latest [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>
1. <span data-ttu-id="f27db-109">Habilitar utilizar el SDK de versión preliminar de Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="f27db-109">Enable Visual Studio to use preview SDKs:</span></span>
   1. <span data-ttu-id="f27db-110">Abra **herramientas** > **opciones** en la barra de menús.</span><span class="sxs-lookup"><span data-stu-id="f27db-110">Open **Tools** > **Options** in the menu bar.</span></span>
   1. <span data-ttu-id="f27db-111">Abra el **proyectos y soluciones** nodo.</span><span class="sxs-lookup"><span data-stu-id="f27db-111">Open the **Projects and Solutions** node.</span></span> <span data-ttu-id="f27db-112">Abra el **.NET Core** ficha.</span><span class="sxs-lookup"><span data-stu-id="f27db-112">Open the **.NET Core** tab.</span></span>
   1. <span data-ttu-id="f27db-113">Active la casilla de **utilice las vistas previas de .NET Core SDK**.</span><span class="sxs-lookup"><span data-stu-id="f27db-113">Check the box for **Use previews of the .NET Core SDK**.</span></span> <span data-ttu-id="f27db-114">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="f27db-114">Select **OK**.</span></span>
1. <span data-ttu-id="f27db-115">Cree un nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="f27db-115">Create a new project.</span></span>
1. <span data-ttu-id="f27db-116">Seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="f27db-116">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="f27db-117">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="f27db-117">Select **Next**.</span></span>
1. <span data-ttu-id="f27db-118">Proporcione un nombre en el **nombre del proyecto** campo.</span><span class="sxs-lookup"><span data-stu-id="f27db-118">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="f27db-119">Confirme la **ubicación** entrada es correcta o proporcionar una ubicación para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="f27db-119">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="f27db-120">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="f27db-120">Select **Create**.</span></span>
1. <span data-ttu-id="f27db-121">Asegúrese de que **.NET Core** y **ASP.NET Core 3.0** están seleccionados en la parte superior.</span><span class="sxs-lookup"><span data-stu-id="f27db-121">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="f27db-122">Elija la **Razor componentes** plantilla y seleccione **crear**.</span><span class="sxs-lookup"><span data-stu-id="f27db-122">Choose the **Razor Components** template and select **Create**.</span></span>
1. <span data-ttu-id="f27db-123">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f27db-123">Press **F5** to run the app.</span></span>

<span data-ttu-id="f27db-124">¡Enhorabuena!</span><span class="sxs-lookup"><span data-stu-id="f27db-124">Congratulations!</span></span> <span data-ttu-id="f27db-125">Acaba de ejecutar su primera aplicación de los componentes de Razor.</span><span class="sxs-lookup"><span data-stu-id="f27db-125">You just ran your first Razor Components app!</span></span>

<!--

# [Visual Studio Code](#tab/visual-studio-code)

Prerequisites:

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

To create your first Razor Components project in Visual Studio Code:

1. Execute the following command from a command shell:

   ```console
   dotnet new razorcomponents -o WebApplication1
   ```

1. Open the *WebApplication1* folder in Visual Studio Code.

1. Add a *.vscode* folder.

1. Add a *tasks.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/tasks.json)]

1. Add a *launch.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/launch.json)]

1. Execute the app using the Visual Studio Code debugger.

1. In a browser, navigate to `https://localhost:5001`.

Congratulations! You just ran your first Razor Components app!

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:razor-components/get-started?tabs=netcore-cli) on macOS.

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Razor Components project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **ASP.NET Core Razor Components** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

Congratulations! You just ran your first Razor Components app!
-->

# [<a name="net-core-cli"></a><span data-ttu-id="f27db-126">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="f27db-126">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="f27db-127">Requisitos previos:</span><span class="sxs-lookup"><span data-stu-id="f27db-127">Prerequisites:</span></span>

* [<span data-ttu-id="f27db-128">Obtener una vista previa de .NET core SDK 3.0</span><span class="sxs-lookup"><span data-stu-id="f27db-128">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="f27db-129">Para crear su primer proyecto de Razor componentes desde un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="f27db-129">To create your first Razor Components project from a command shell:</span></span>

   ```console
   dotnet new razorcomponents -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="f27db-130">En un navegador, vaya a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="f27db-130">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="f27db-131">¡Enhorabuena!</span><span class="sxs-lookup"><span data-stu-id="f27db-131">Congratulations!</span></span> <span data-ttu-id="f27db-132">Acaba de ejecutar su primera aplicación de los componentes de Razor.</span><span class="sxs-lookup"><span data-stu-id="f27db-132">You just ran your first Razor Components app!</span></span>

---

## <a name="razor-components-project"></a><span data-ttu-id="f27db-133">Proyecto de componentes de Razor</span><span class="sxs-lookup"><span data-stu-id="f27db-133">Razor Components project</span></span>

<span data-ttu-id="f27db-134">Componentes de Razor se crean mediante la sintaxis de Razor, pero se compilan de forma diferente a las vistas de páginas de Razor y MVC.</span><span class="sxs-lookup"><span data-stu-id="f27db-134">Razor Components are authored using Razor syntax but are compiled differently than Razor Pages and MVC views.</span></span> <span data-ttu-id="f27db-135">El *.razor* extensión de archivo se usa para especificar un componente de Razor.</span><span class="sxs-lookup"><span data-stu-id="f27db-135">The *.razor* file extension is used to specify a Razor Component.</span></span> <span data-ttu-id="f27db-136">Las páginas de Razor y MVC vistas seguirán usando el *.cshtml* la extensión de archivo.</span><span class="sxs-lookup"><span data-stu-id="f27db-136">Razor Pages and MVC views continue to use the *.cshtml* file extension.</span></span>

> [!NOTE]
> <span data-ttu-id="f27db-137">Se pueden crear componentes de Razor con el *.cshtml* extensión de archivo siempre que esos archivos se identifican como archivos de Razor componente mediante el `_RazorComponentInclude` propiedad de MSBuild.</span><span class="sxs-lookup"><span data-stu-id="f27db-137">Razor Components can be authored using the *.cshtml* file extension as long as those files are identified as Razor Component files using the `_RazorComponentInclude` MSBuild property.</span></span> <span data-ttu-id="f27db-138">Por ejemplo, una aplicación creada con la plantilla de Razor componente especifica que todos los *.cshtml* archivos bajo la *componentes* carpeta debe tratarse como componentes de Razor:</span><span class="sxs-lookup"><span data-stu-id="f27db-138">For example, an app created using the Razor Component template specifies that all *.cshtml* files under the *Components* folder should be treated as Razor Components:</span></span>
>
> ```xml
> <_RazorComponentInclude>Components\**\*.cshtml</_RazorComponentInclude>
> ```

<span data-ttu-id="f27db-139">Cuando se ejecuta la aplicación, están disponibles las fichas en la barra lateral de varias páginas:</span><span class="sxs-lookup"><span data-stu-id="f27db-139">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="f27db-140">Página principal</span><span class="sxs-lookup"><span data-stu-id="f27db-140">Home</span></span>
* <span data-ttu-id="f27db-141">Contador</span><span class="sxs-lookup"><span data-stu-id="f27db-141">Counter</span></span>
* <span data-ttu-id="f27db-142">Recuperar datos</span><span class="sxs-lookup"><span data-stu-id="f27db-142">Fetch data</span></span>

<span data-ttu-id="f27db-143">En la página Contador, seleccione el botón **Click me** para aumentar el contador sin una actualización de página.</span><span class="sxs-lookup"><span data-stu-id="f27db-143">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="f27db-144">Aumentar un contador en una página web suele requerir la escritura de JavaScript, pero Razor Components proporciona una mejor manera de usar C#.</span><span class="sxs-lookup"><span data-stu-id="f27db-144">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

<span data-ttu-id="f27db-145">*WebApplication1/Components/Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="f27db-145">*WebApplication1/Components/Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor)]

<span data-ttu-id="f27db-146">Una solicitud para `/counter` en el explorador, según lo especificado por el `@page` directiva en la parte superior, hace que el componente de contador representar su contenido.</span><span class="sxs-lookup"><span data-stu-id="f27db-146">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="f27db-147">Los componentes se representan en una representación en memoria del árbol de representación que puede usarse para actualizar la interfaz de usuario de una manera flexible y eficaz.</span><span class="sxs-lookup"><span data-stu-id="f27db-147">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="f27db-148">Cada vez que el **Click me** está seleccionado:</span><span class="sxs-lookup"><span data-stu-id="f27db-148">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="f27db-149">El `onclick` desencadena el evento.</span><span class="sxs-lookup"><span data-stu-id="f27db-149">The `onclick` event is fired.</span></span>
* <span data-ttu-id="f27db-150">Se llama al método `IncrementCount` .</span><span class="sxs-lookup"><span data-stu-id="f27db-150">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="f27db-151">El `currentCount` se incrementa.</span><span class="sxs-lookup"><span data-stu-id="f27db-151">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="f27db-152">El componente se representa de nuevo.</span><span class="sxs-lookup"><span data-stu-id="f27db-152">The component is rendered again.</span></span>

<span data-ttu-id="f27db-153">El tiempo de ejecución compara el contenido nuevo para el contenido anterior y solo aplica el contenido cambiado a Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="f27db-153">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="f27db-154">Agregar un componente a otro componente mediante una sintaxis similar a HTML.</span><span class="sxs-lookup"><span data-stu-id="f27db-154">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="f27db-155">Parámetros del componente se especifican mediante atributos o contenido secundario.</span><span class="sxs-lookup"><span data-stu-id="f27db-155">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="f27db-156">Por ejemplo, un componente de contador se puede agregar a la página principal de la aplicación agregando un `<Counter />` elemento para el componente del índice.</span><span class="sxs-lookup"><span data-stu-id="f27db-156">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="f27db-157">*WebApplication1/Components/Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="f27db-157">*WebApplication1/Components/Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="f27db-158">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f27db-158">Run the app.</span></span> <span data-ttu-id="f27db-159">La página principal tiene su propio contador.</span><span class="sxs-lookup"><span data-stu-id="f27db-159">The homepage has its own counter.</span></span>

<span data-ttu-id="f27db-160">Para agregar un parámetro para el componente de contador, actualice el componente `@functions` bloque:</span><span class="sxs-lookup"><span data-stu-id="f27db-160">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="f27db-161">Agregue una propiedad para `IncrementAmount` decorada con el `[Parameter]` atributo.</span><span class="sxs-lookup"><span data-stu-id="f27db-161">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="f27db-162">Cambie el método `IncrementCount` para usar `IncrementAmount` al aumentar el valor de `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="f27db-162">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="f27db-163">*WebApplication1/Components/Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="f27db-163">*WebApplication1/Components/Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=4,8)]

<span data-ttu-id="f27db-164">Especifique un parámetro `IncrementAmount` en el elemento `<Counter>` del componente Home mediante un atributo.</span><span class="sxs-lookup"><span data-stu-id="f27db-164">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="f27db-165">*WebApplication1/Components/Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="f27db-165">*WebApplication1/Components/Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor)]

<span data-ttu-id="f27db-166">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f27db-166">Run the app.</span></span> <span data-ttu-id="f27db-167">La página principal tiene su propio contador que se incrementa en diez cada vez que el **Click me** botón está seleccionado.</span><span class="sxs-lookup"><span data-stu-id="f27db-167">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f27db-168">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="f27db-168">Next steps</span></span>

<xref:tutorials/first-razor-components-app>
