---
title: Introducción a Blazor
author: guardrex
description: Obtenga información sobre cómo empezar a trabajar con Blazor mediante la creación y modificación de un proyecto Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2019
uid: spa/blazor/get-started
ms.openlocfilehash: 8c984bab8a13b4fc2d87fd1a7e0b285dfa25ba09
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159604"
---
# <a name="get-started-with-blazor"></a><span data-ttu-id="d345c-103">Introducción a Blazor</span><span class="sxs-lookup"><span data-stu-id="d345c-103">Get started with Blazor</span></span>

<span data-ttu-id="d345c-104">Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d345c-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d345c-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d345c-105">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d345c-106">Requisitos previos:</span><span class="sxs-lookup"><span data-stu-id="d345c-106">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="d345c-107">Para crear su primer proyecto Blazor en Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="d345c-107">To create your first Blazor project in Visual Studio:</span></span>

1. <span data-ttu-id="d345c-108">Instale la última versión [extensión Blazor Language Services](https://go.microsoft.com/fwlink/?linkid=870389) desde Visual Studio Marketplace.</span><span class="sxs-lookup"><span data-stu-id="d345c-108">Install the latest [Blazor Language Services extension](https://go.microsoft.com/fwlink/?linkid=870389) from the Visual Studio Marketplace.</span></span> <span data-ttu-id="d345c-109">Este paso pone a disposición Blazor plantillas para Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d345c-109">This step makes Blazor templates available to Visual Studio.</span></span>
1. <span data-ttu-id="d345c-110">Asegúrese de las plantillas de Blazor disponibles para su uso con la CLI de .NET Core ejecutando el siguiente comando en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="d345c-110">Make the Blazor templates available for use with the .NET Core CLI by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates
   ```

1. <span data-ttu-id="d345c-111">Seleccione **archivo** > **nuevo proyecto** > **Web** > **aplicación Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="d345c-111">Select **File** > **New Project** > **Web** > **ASP.NET Core Web Application**.</span></span>
1. <span data-ttu-id="d345c-112">Asegúrese de que **.NET Core** y **ASP.NET Core 3.0** están seleccionados en la parte superior.</span><span class="sxs-lookup"><span data-stu-id="d345c-112">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="d345c-113">Elija la plantilla **Blazor** y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="d345c-113">Choose the **Blazor** template and select **OK**.</span></span>
1. <span data-ttu-id="d345c-114">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d345c-114">Press **F5** to run the app.</span></span>

<span data-ttu-id="d345c-115">¡Enhorabuena!</span><span class="sxs-lookup"><span data-stu-id="d345c-115">Congratulations!</span></span> <span data-ttu-id="d345c-116">¡Acaba de ejecutar su primera aplicación Blazor!</span><span class="sxs-lookup"><span data-stu-id="d345c-116">You just ran your first Blazor app!</span></span>

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

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d345c-117">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="d345c-117">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="d345c-118">Requisitos previos:</span><span class="sxs-lookup"><span data-stu-id="d345c-118">Prerequisites:</span></span>

* [<span data-ttu-id="d345c-119">Obtener una vista previa de .NET core SDK 3.0</span><span class="sxs-lookup"><span data-stu-id="d345c-119">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="d345c-120">Agregar las plantillas Blazor ejecutando el siguiente comando en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="d345c-120">Add the Blazor templates by running the following command in a command shell:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates
   ```

1. <span data-ttu-id="d345c-121">Cree su primer proyecto Blazor en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="d345c-121">Create your first Blazor project in a command shell:</span></span>

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="d345c-122">En un navegador, vaya a `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="d345c-122">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="d345c-123">¡Enhorabuena!</span><span class="sxs-lookup"><span data-stu-id="d345c-123">Congratulations!</span></span> <span data-ttu-id="d345c-124">¡Acaba de ejecutar su primera aplicación Blazor!</span><span class="sxs-lookup"><span data-stu-id="d345c-124">You just ran your first Blazor app!</span></span>

---

## <a name="blazor-project"></a><span data-ttu-id="d345c-125">Proyecto Blazor</span><span class="sxs-lookup"><span data-stu-id="d345c-125">Blazor project</span></span>

<span data-ttu-id="d345c-126">Cuando se ejecuta la aplicación, están disponibles las fichas en la barra lateral de varias páginas:</span><span class="sxs-lookup"><span data-stu-id="d345c-126">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="d345c-127">Página principal</span><span class="sxs-lookup"><span data-stu-id="d345c-127">Home</span></span>
* <span data-ttu-id="d345c-128">Contador</span><span class="sxs-lookup"><span data-stu-id="d345c-128">Counter</span></span>
* <span data-ttu-id="d345c-129">Recuperar datos</span><span class="sxs-lookup"><span data-stu-id="d345c-129">Fetch data</span></span>

<span data-ttu-id="d345c-130">En la página de contadores, seleccione el **Click me** botón incrementar el contador sin una actualización de la página.</span><span class="sxs-lookup"><span data-stu-id="d345c-130">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="d345c-131">Incrementar un contador en una página Web normalmente requiere escribir código JavaScript, pero Blazor proporciona un enfoque mejor usar C#.</span><span class="sxs-lookup"><span data-stu-id="d345c-131">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

<span data-ttu-id="d345c-132">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d345c-132">*Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

<span data-ttu-id="d345c-133">Una solicitud para `/counter` en el explorador, según lo especificado por el `@page` directiva en la parte superior, hace que el componente de contador representar su contenido.</span><span class="sxs-lookup"><span data-stu-id="d345c-133">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="d345c-134">Los componentes se representan en una representación en memoria del árbol de representación que puede usarse para actualizar la interfaz de usuario de una manera flexible y eficaz.</span><span class="sxs-lookup"><span data-stu-id="d345c-134">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="d345c-135">Cada vez que el **Click me** está seleccionado:</span><span class="sxs-lookup"><span data-stu-id="d345c-135">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="d345c-136">El `onclick` desencadena el evento.</span><span class="sxs-lookup"><span data-stu-id="d345c-136">The `onclick` event is fired.</span></span>
* <span data-ttu-id="d345c-137">Se llama al método `IncrementCount` .</span><span class="sxs-lookup"><span data-stu-id="d345c-137">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="d345c-138">El `currentCount` se incrementa.</span><span class="sxs-lookup"><span data-stu-id="d345c-138">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="d345c-139">El componente se representa de nuevo.</span><span class="sxs-lookup"><span data-stu-id="d345c-139">The component is rendered again.</span></span>

<span data-ttu-id="d345c-140">El tiempo de ejecución compara el contenido nuevo para el contenido anterior y solo aplica el contenido cambiado a Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="d345c-140">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="d345c-141">Agregar un componente a otro componente mediante una sintaxis similar a HTML.</span><span class="sxs-lookup"><span data-stu-id="d345c-141">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="d345c-142">Parámetros del componente se especifican mediante atributos o contenido secundario.</span><span class="sxs-lookup"><span data-stu-id="d345c-142">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="d345c-143">Por ejemplo, un componente de contador se puede agregar a la página principal de la aplicación agregando un `<Counter />` elemento para el componente del índice.</span><span class="sxs-lookup"><span data-stu-id="d345c-143">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="d345c-144">En *Pages/index.cshtml*, reemplace el componente de encuesta de símbolo del sistema con un componente de contador:</span><span class="sxs-lookup"><span data-stu-id="d345c-144">In *Pages/Index.cshtml*, replace the Survey Prompt component with a Counter component:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

<span data-ttu-id="d345c-145">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d345c-145">Run the app.</span></span> <span data-ttu-id="d345c-146">La página principal tiene su propio contador.</span><span class="sxs-lookup"><span data-stu-id="d345c-146">The homepage has its own counter.</span></span>

<span data-ttu-id="d345c-147">Para agregar un parámetro para el componente de contador, actualice el componente `@functions` bloque:</span><span class="sxs-lookup"><span data-stu-id="d345c-147">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="d345c-148">Agregue una propiedad para `IncrementAmount` decorada con el `[Parameter]` atributo.</span><span class="sxs-lookup"><span data-stu-id="d345c-148">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="d345c-149">Cambiar el `IncrementCount` método que se usará el `IncrementAmount` al aumentar el valor de `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="d345c-149">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="d345c-150">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d345c-150">*Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

<span data-ttu-id="d345c-151">Especifique un `IncrementAmount` parámetro en el componente principal `<Counter>` utilizando un atributo del elemento.</span><span class="sxs-lookup"><span data-stu-id="d345c-151">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="d345c-152">*Páginas/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d345c-152">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

<span data-ttu-id="d345c-153">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d345c-153">Run the app.</span></span> <span data-ttu-id="d345c-154">La página principal tiene su propio contador que se incrementa en diez cada vez que el **Click me** botón está seleccionado.</span><span class="sxs-lookup"><span data-stu-id="d345c-154">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d345c-155">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="d345c-155">Next steps</span></span>

<xref:tutorials/first-razor-components-app>
