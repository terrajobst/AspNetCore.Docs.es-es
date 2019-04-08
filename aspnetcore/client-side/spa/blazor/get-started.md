---
title: Introducción a Blazor
author: guardrex
description: Obtenga información sobre cómo empezar a trabajar con Blazor mediante la creación y modificación de un proyecto Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: spa/blazor/get-started
ms.openlocfilehash: b3928c2812be6f34cdf2f17295a1251106f651e5
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068240"
---
# <a name="get-started-with-blazor"></a><span data-ttu-id="ee0e8-103">Introducción a Blazor</span><span class="sxs-lookup"><span data-stu-id="ee0e8-103">Get started with Blazor</span></span>

<span data-ttu-id="ee0e8-104">Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ee0e8-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

# [<a name="visual-studio"></a><span data-ttu-id="ee0e8-105">Programa para la mejora</span><span class="sxs-lookup"><span data-stu-id="ee0e8-105">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ee0e8-106">Requisitos previos:</span><span class="sxs-lookup"><span data-stu-id="ee0e8-106">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="ee0e8-107">Para crear su primer proyecto Blazor en Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="ee0e8-107">To create your first Blazor project in Visual Studio:</span></span>

1. <span data-ttu-id="ee0e8-108">Instale la última versión [SDK de .NET Core 3.0 Preview](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-108">Install the latest [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>
1. <span data-ttu-id="ee0e8-109">Habilitar utilizar el SDK de versión preliminar de Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="ee0e8-109">Enable Visual Studio to use preview SDKs:</span></span>
   1. <span data-ttu-id="ee0e8-110">Abra **herramientas** > **opciones** en la barra de menús.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-110">Open **Tools** > **Options** in the menu bar.</span></span>
   1. <span data-ttu-id="ee0e8-111">Abra el **proyectos y soluciones** nodo.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-111">Open the **Projects and Solutions** node.</span></span> <span data-ttu-id="ee0e8-112">Abra el **.NET Core** ficha.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-112">Open the **.NET Core** tab.</span></span>
   1. <span data-ttu-id="ee0e8-113">Active la casilla de **utilice las vistas previas de .NET Core SDK**.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-113">Check the box for **Use previews of the .NET Core SDK**.</span></span> <span data-ttu-id="ee0e8-114">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-114">Select **OK**.</span></span>
1. <span data-ttu-id="ee0e8-115">Instale la última versión [Blazor extensión](https://go.microsoft.com/fwlink/?linkid=870389) desde Visual Studio Marketplace.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-115">Install the latest [Blazor extension](https://go.microsoft.com/fwlink/?linkid=870389) from the Visual Studio Marketplace.</span></span> <span data-ttu-id="ee0e8-116">Este paso pone a disposición Blazor plantillas para Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-116">This step makes Blazor templates available to Visual Studio.</span></span>
1. <span data-ttu-id="ee0e8-117">Asegúrese de las plantillas de Blazor disponibles para su uso con la CLI de .NET Core ejecutando el siguiente comando en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="ee0e8-117">Make the Blazor templates available for use with the .NET Core CLI by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```
1. <span data-ttu-id="ee0e8-118">Cree un nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-118">Create a new project.</span></span>
1. <span data-ttu-id="ee0e8-119">Seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-119">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="ee0e8-120">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-120">Select **Next**.</span></span>
1. <span data-ttu-id="ee0e8-121">Proporcione un nombre en el **nombre del proyecto** campo.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-121">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="ee0e8-122">Confirme la **ubicación** entrada es correcta o proporcionar una ubicación para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-122">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="ee0e8-123">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-123">Select **Create**.</span></span>
1. <span data-ttu-id="ee0e8-124">Asegúrese de que **.NET Core** y **ASP.NET Core 3.0** están seleccionados en la parte superior.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-124">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="ee0e8-125">Seleccione el **Blazor** plantilla y seleccione **crear**.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-125">Select the **Blazor** template and select **Create**.</span></span>
1. <span data-ttu-id="ee0e8-126">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-126">Press **F5** to run the app.</span></span>

<span data-ttu-id="ee0e8-127">¡Enhorabuena!</span><span class="sxs-lookup"><span data-stu-id="ee0e8-127">Congratulations!</span></span> <span data-ttu-id="ee0e8-128">¡Acaba de ejecutar su primera aplicación Blazor!</span><span class="sxs-lookup"><span data-stu-id="ee0e8-128">You just ran your first Blazor app!</span></span>

<!--

# [Visual Studio Code](#tab/visual-studio-code)

Prerequisites:

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

To create your first Blazor project in Visual Studio Code:

1. Execute the following command in a command shell:

   ```console
   dotnet new blazor -o WebApplication1
   ```

1. Open the *WebApplication1* folder in Visual Studio Code.

1. Visual Studio code offers to create assets to build and debug the app, which includes the *tasks.json* and *launch.json* files. Select **Yes** to add the assets.

1. Execute the app using the Visual Studio Code debugger.

1. In a browser, navigate to `https://localhost:5001`.

Congratulations! You just ran your first Blazor app!

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:razor-components/get-started?tabs=netcore-cli) on macOS.

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Blazor project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **Blazor** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

Congratulations! You just ran your first Blazor app!
-->

# [<a name="net-core-cli"></a><span data-ttu-id="ee0e8-129">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="ee0e8-129">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="ee0e8-130">Requisitos previos:</span><span class="sxs-lookup"><span data-stu-id="ee0e8-130">Prerequisites:</span></span>

* [<span data-ttu-id="ee0e8-131">Obtener una vista previa de .NET core SDK 3.0</span><span class="sxs-lookup"><span data-stu-id="ee0e8-131">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="ee0e8-132">Agregar las plantillas Blazor ejecutando el siguiente comando en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="ee0e8-132">Add the Blazor templates by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. <span data-ttu-id="ee0e8-133">Cree su primer proyecto Blazor en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="ee0e8-133">Create your first Blazor project in a command shell:</span></span>

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="ee0e8-134">En un navegador, vaya a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-134">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="ee0e8-135">¡Enhorabuena!</span><span class="sxs-lookup"><span data-stu-id="ee0e8-135">Congratulations!</span></span> <span data-ttu-id="ee0e8-136">¡Acaba de ejecutar su primera aplicación Blazor!</span><span class="sxs-lookup"><span data-stu-id="ee0e8-136">You just ran your first Blazor app!</span></span>

---

## <a name="blazor-project"></a><span data-ttu-id="ee0e8-137">Proyecto Blazor</span><span class="sxs-lookup"><span data-stu-id="ee0e8-137">Blazor project</span></span>

<span data-ttu-id="ee0e8-138">Cuando se ejecuta la aplicación, están disponibles las fichas en la barra lateral de varias páginas:</span><span class="sxs-lookup"><span data-stu-id="ee0e8-138">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="ee0e8-139">Página principal</span><span class="sxs-lookup"><span data-stu-id="ee0e8-139">Home</span></span>
* <span data-ttu-id="ee0e8-140">Contador</span><span class="sxs-lookup"><span data-stu-id="ee0e8-140">Counter</span></span>
* <span data-ttu-id="ee0e8-141">Recuperar datos</span><span class="sxs-lookup"><span data-stu-id="ee0e8-141">Fetch data</span></span>

<span data-ttu-id="ee0e8-142">En la página Contador, seleccione el botón **Click me** para aumentar el contador sin una actualización de página.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-142">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="ee0e8-143">Incrementar un contador en una página Web normalmente requiere escribir código JavaScript, pero Blazor proporciona un enfoque mejor usar C#.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-143">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

<span data-ttu-id="ee0e8-144">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ee0e8-144">*Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

<span data-ttu-id="ee0e8-145">Una solicitud para `/counter` en el explorador, según lo especificado por el `@page` directiva en la parte superior, hace que el componente de contador representar su contenido.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-145">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="ee0e8-146">Los componentes se representan en una representación en memoria del árbol de representación que puede usarse para actualizar la interfaz de usuario de una manera flexible y eficaz.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-146">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="ee0e8-147">Cada vez que el **Click me** está seleccionado:</span><span class="sxs-lookup"><span data-stu-id="ee0e8-147">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="ee0e8-148">El `onclick` desencadena el evento.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-148">The `onclick` event is fired.</span></span>
* <span data-ttu-id="ee0e8-149">Se llama al método `IncrementCount` .</span><span class="sxs-lookup"><span data-stu-id="ee0e8-149">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="ee0e8-150">El `currentCount` se incrementa.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-150">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="ee0e8-151">El componente se representa de nuevo.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-151">The component is rendered again.</span></span>

<span data-ttu-id="ee0e8-152">El tiempo de ejecución compara el contenido nuevo para el contenido anterior y solo aplica el contenido cambiado a Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="ee0e8-152">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="ee0e8-153">Agregar un componente a otro componente mediante una sintaxis similar a HTML.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-153">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="ee0e8-154">Parámetros del componente se especifican mediante atributos o contenido secundario.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-154">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="ee0e8-155">Por ejemplo, un componente de contador se puede agregar a la página principal de la aplicación agregando un `<Counter />` elemento para el componente del índice.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-155">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="ee0e8-156">En *Pages/index.cshtml*, reemplace el componente de encuesta de símbolo del sistema con un componente de contador:</span><span class="sxs-lookup"><span data-stu-id="ee0e8-156">In *Pages/Index.cshtml*, replace the Survey Prompt component with a Counter component:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

<span data-ttu-id="ee0e8-157">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-157">Run the app.</span></span> <span data-ttu-id="ee0e8-158">La página principal tiene su propio contador.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-158">The homepage has its own counter.</span></span>

<span data-ttu-id="ee0e8-159">Para agregar un parámetro para el componente de contador, actualice el componente `@functions` bloque:</span><span class="sxs-lookup"><span data-stu-id="ee0e8-159">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="ee0e8-160">Agregue una propiedad para `IncrementAmount` decorada con el `[Parameter]` atributo.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-160">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="ee0e8-161">Cambie el método `IncrementCount` para usar `IncrementAmount` al aumentar el valor de `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-161">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="ee0e8-162">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ee0e8-162">*Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

<span data-ttu-id="ee0e8-163">Especifique un parámetro `IncrementAmount` en el elemento `<Counter>` del componente Home mediante un atributo.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-163">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="ee0e8-164">*Páginas/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ee0e8-164">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

<span data-ttu-id="ee0e8-165">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-165">Run the app.</span></span> <span data-ttu-id="ee0e8-166">La página principal tiene su propio contador que se incrementa en diez cada vez que el **Click me** botón está seleccionado.</span><span class="sxs-lookup"><span data-stu-id="ee0e8-166">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ee0e8-167">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="ee0e8-167">Next steps</span></span>

<xref:tutorials/first-razor-components-app>
