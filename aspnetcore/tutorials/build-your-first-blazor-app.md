---
title: Creación de la primera aplicación Blazor
author: guardrex
description: Cree una aplicación Blazor paso a paso.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: c357b324905ee3a4c9f4bd167dbbcacaf7e1bc76
ms.sourcegitcommit: 35a86ce48041caaf6396b1e88b0472578ba24483
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72391205"
---
# <a name="build-your-first-blazor-app"></a><span data-ttu-id="5e120-103">Creación de la primera aplicación Blazor</span><span class="sxs-lookup"><span data-stu-id="5e120-103">Build your first Blazor app</span></span>

<span data-ttu-id="5e120-104">Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5e120-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="5e120-105">En este tutorial se muestra cómo crear y modificar una aplicación de Blazor.</span><span class="sxs-lookup"><span data-stu-id="5e120-105">This tutorial shows you how to build and modify a Blazor app.</span></span>

<span data-ttu-id="5e120-106">Siga las instrucciones del artículo <xref:blazor/get-started> para crear un proyecto de Blazor en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="5e120-106">Follow the guidance in the <xref:blazor/get-started> article to create a Blazor project for this tutorial.</span></span> <span data-ttu-id="5e120-107">Denomine el proyecto *ToDoList*.</span><span class="sxs-lookup"><span data-stu-id="5e120-107">Name the project *ToDoList*.</span></span>

## <a name="build-components"></a><span data-ttu-id="5e120-108">Creación de componentes</span><span class="sxs-lookup"><span data-stu-id="5e120-108">Build components</span></span>

1. <span data-ttu-id="5e120-109">Vaya a cada una de las tres páginas de la aplicación en la carpeta *Pages*: Home (Inicio), Counter (Contador) y Fetch data (Recuperar datos).</span><span class="sxs-lookup"><span data-stu-id="5e120-109">Browse to each of the app's three pages in the *Pages* folder: Home, Counter, and Fetch data.</span></span> <span data-ttu-id="5e120-110">Estas páginas se implementan mediante los archivos de componente de Razor *Index.razor*, *Counter.razor* y *FetchData.razor*.</span><span class="sxs-lookup"><span data-stu-id="5e120-110">These pages are implemented by the Razor component files *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span>

1. <span data-ttu-id="5e120-111">En la página Contador, seleccione el botón **Click me** para aumentar el contador sin una actualización de página.</span><span class="sxs-lookup"><span data-stu-id="5e120-111">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="5e120-112">Aumentar un contador en una página web suele requerir la escritura de JavaScript, pero Blazor proporciona una mejor manera de usar C#.</span><span class="sxs-lookup"><span data-stu-id="5e120-112">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

1. <span data-ttu-id="5e120-113">Examine la implementación del componente `Counter` en el archivo *Counter.razor*.</span><span class="sxs-lookup"><span data-stu-id="5e120-113">Examine the implementation of the `Counter` component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="5e120-114">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="5e120-114">*Pages/Counter.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="5e120-115">la interfaz de usuario del componente `Counter` se define mediante HTML.</span><span class="sxs-lookup"><span data-stu-id="5e120-115">The UI of the `Counter` component is defined using HTML.</span></span> <span data-ttu-id="5e120-116">La lógica de la representación dinámica (por ejemplo, bucles, instrucciones condicionales, expresiones) se agrega mediante una sintaxis de C# insertada denominada [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="5e120-116">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="5e120-117">El marcado HTML y la lógica de representación de C# se convierten en una clase de componente en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="5e120-117">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="5e120-118">El nombre de la clase de .NET generada coincide con el nombre del archivo.</span><span class="sxs-lookup"><span data-stu-id="5e120-118">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="5e120-119">Los miembros de la clase de componente se definen en un bloque `@code`.</span><span class="sxs-lookup"><span data-stu-id="5e120-119">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="5e120-120">En el bloque `@code`, se especifica el estado del componente (propiedades, campos) y los métodos para el tratamiento de eventos o para definir otra lógica del componente.</span><span class="sxs-lookup"><span data-stu-id="5e120-120">In the `@code` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="5e120-121">Estos miembros se utilizan como parte de la lógica de representación del componente y para el tratamiento de eventos.</span><span class="sxs-lookup"><span data-stu-id="5e120-121">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="5e120-122">Al seleccionarse el botón **Click me**:</span><span class="sxs-lookup"><span data-stu-id="5e120-122">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="5e120-123">Se llama al controlador `onclick` registrado del componente `Counter` (el método `IncrementCount`).</span><span class="sxs-lookup"><span data-stu-id="5e120-123">The `Counter` component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="5e120-124">El componente `Counter` regenera su árbol de representación.</span><span class="sxs-lookup"><span data-stu-id="5e120-124">The `Counter` component regenerates its render tree.</span></span>
   * <span data-ttu-id="5e120-125">El nuevo árbol de representación se compara con el anterior.</span><span class="sxs-lookup"><span data-stu-id="5e120-125">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="5e120-126">Únicamente se aplican modificaciones en Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="5e120-126">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="5e120-127">Se actualiza el recuento mostrado.</span><span class="sxs-lookup"><span data-stu-id="5e120-127">The displayed count is updated.</span></span>

1. <span data-ttu-id="5e120-128">Modifique la lógica de C# del componente `Counter` para hacer que el recuento se incremente en dos en lugar de uno.</span><span class="sxs-lookup"><span data-stu-id="5e120-128">Modify the C# logic of the `Counter` component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="5e120-129">Recompile y ejecute la aplicación para ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="5e120-129">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="5e120-130">Seleccione el botón **Hacer clic aquí**.</span><span class="sxs-lookup"><span data-stu-id="5e120-130">Select the **Click me** button.</span></span> <span data-ttu-id="5e120-131">El contador se incrementa en dos.</span><span class="sxs-lookup"><span data-stu-id="5e120-131">The counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="5e120-132">Uso de componentes</span><span class="sxs-lookup"><span data-stu-id="5e120-132">Use components</span></span>

<span data-ttu-id="5e120-133">Incluya un componente en otro componente mediante una sintaxis HTML.</span><span class="sxs-lookup"><span data-stu-id="5e120-133">Include a component in another component using an HTML syntax.</span></span>

1. <span data-ttu-id="5e120-134">Agregue el componente `Counter` al componente `Index` de la aplicación al agregar un elemento `<Counter />` al componente `Index` (*Index.razor*).</span><span class="sxs-lookup"><span data-stu-id="5e120-134">Add the `Counter` component to the app's `Index` component by adding a `<Counter />` element to the `Index` component (*Index.razor*).</span></span>

   <span data-ttu-id="5e120-135">Si va a usar Blazor WebAssembly para esta experiencia, el componente `Index` usa un componente `SurveyPrompt`.</span><span class="sxs-lookup"><span data-stu-id="5e120-135">If you're using Blazor WebAssembly for this experience, a `SurveyPrompt` component is used by the `Index` component.</span></span> <span data-ttu-id="5e120-136">Reemplace el elemento `<SurveyPrompt>` por un elemento `<Counter />`.</span><span class="sxs-lookup"><span data-stu-id="5e120-136">Replace the `<SurveyPrompt>` element with a `<Counter />` element.</span></span> <span data-ttu-id="5e120-137">Si va a usar la aplicación Blazor Server para esta experiencia, agregue el elemento `<Counter />` al componente `Index`:</span><span class="sxs-lookup"><span data-stu-id="5e120-137">If you're using a Blazor Server app for this experience, add the `<Counter />` element to the `Index` component:</span></span>

   <span data-ttu-id="5e120-138">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="5e120-138">*Pages/Index.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. <span data-ttu-id="5e120-139">Recompile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5e120-139">Rebuild and run the app.</span></span> <span data-ttu-id="5e120-140">El componente `Index` tiene su propio contador.</span><span class="sxs-lookup"><span data-stu-id="5e120-140">The `Index` component has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="5e120-141">Parámetros del componente</span><span class="sxs-lookup"><span data-stu-id="5e120-141">Component parameters</span></span>

<span data-ttu-id="5e120-142">Los componentes también pueden tener parámetros.</span><span class="sxs-lookup"><span data-stu-id="5e120-142">Components can also have parameters.</span></span> <span data-ttu-id="5e120-143">Los parámetros de componente se definen mediante propiedades públicas en la clase de componentes con el atributo `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="5e120-143">Component parameters are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="5e120-144">Use atributos para especificar argumentos para un componente en el marcado.</span><span class="sxs-lookup"><span data-stu-id="5e120-144">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="5e120-145">Actualice el código de C# `@code` del componente:</span><span class="sxs-lookup"><span data-stu-id="5e120-145">Update the component's `@code` C# code:</span></span>

   * <span data-ttu-id="5e120-146">Agregue una propiedad `IncrementAmount` pública con el atributo `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="5e120-146">Add a public `IncrementAmount` property with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="5e120-147">Cambie el método `IncrementCount` para usar `IncrementAmount` al aumentar el valor de `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="5e120-147">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="5e120-148">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="5e120-148">*Pages/Counter.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="5e120-149">Especifique un parámetro `IncrementAmount` en el elemento `<Counter>` del componente `Index` mediante un atributo.</span><span class="sxs-lookup"><span data-stu-id="5e120-149">Specify an `IncrementAmount` parameter in the `Index` component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="5e120-150">Establezca el valor para incrementar el contador en diez.</span><span class="sxs-lookup"><span data-stu-id="5e120-150">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="5e120-151">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="5e120-151">*Pages/Index.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. <span data-ttu-id="5e120-152">Recargue el componente `Index`.</span><span class="sxs-lookup"><span data-stu-id="5e120-152">Reload the `Index` component.</span></span> <span data-ttu-id="5e120-153">El contador se incrementa en diez cada vez que se selecciona el botón **Click me**.</span><span class="sxs-lookup"><span data-stu-id="5e120-153">The counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="5e120-154">El contador del componente `Counter` sigue incrementándose en uno.</span><span class="sxs-lookup"><span data-stu-id="5e120-154">The counter in the `Counter` component continues to increment by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="5e120-155">Enrutamiento a los componentes</span><span class="sxs-lookup"><span data-stu-id="5e120-155">Route to components</span></span>

<span data-ttu-id="5e120-156">La directiva `@page` en la parte superior del archivo *Counter.razor* especifica que el componente `Counter` es un punto de conexión de enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="5e120-156">The `@page` directive at the top of the *Counter.razor* file specifies that the `Counter` component is a routing endpoint.</span></span> <span data-ttu-id="5e120-157">El componente `Counter` controla las solicitudes enviadas a `/counter`.</span><span class="sxs-lookup"><span data-stu-id="5e120-157">The `Counter` component handles requests sent to `/counter`.</span></span> <span data-ttu-id="5e120-158">Sin la directiva `@page`, el componente no controla las solicitudes enrutadas, pero otros componentes aún pueden usar el componente.</span><span class="sxs-lookup"><span data-stu-id="5e120-158">Without the `@page` directive, a component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="5e120-159">Inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="5e120-159">Dependency injection</span></span>

### <a name="blazor-server-experience"></a><span data-ttu-id="5e120-160">Experiencia del servidor Blazor</span><span class="sxs-lookup"><span data-stu-id="5e120-160">Blazor Server experience</span></span>

<span data-ttu-id="5e120-161">Si va a trabajar con una aplicación Blazor Server, el servicio `WeatherForecastService` se registra como [singleton](xref:fundamentals/dependency-injection#service-lifetimes) en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5e120-161">If working with a Blazor Server app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="5e120-162">Una instancia del servicio está disponible en toda la aplicación a través de la [inserción de dependencias (DI)](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="5e120-162">An instance of the service is available throughout the app via [dependency injection (DI)](xref:fundamentals/dependency-injection):</span></span>

[!code-csharp[](build-your-first-blazor-app/samples_snapshot/3.x/Startup.cs?highlight=5)]

<span data-ttu-id="5e120-163">La directiva `@inject` se usa para insertar la instancia del servicio `WeatherForecastService` en el componente `FetchData`.</span><span class="sxs-lookup"><span data-stu-id="5e120-163">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the `FetchData` component.</span></span>

<span data-ttu-id="5e120-164">*Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="5e120-164">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="5e120-165">El componente `FetchData` usa el servicio insertado, como `ForecastService`, para recuperar una matriz de objetos `WeatherForecast`:</span><span class="sxs-lookup"><span data-stu-id="5e120-165">The `FetchData` component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

### <a name="blazor-webassembly-experience"></a><span data-ttu-id="5e120-166">Experiencia de Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="5e120-166">Blazor WebAssembly experience</span></span>

<span data-ttu-id="5e120-167">Si trabaja con una aplicación Blazor WebAssembly, se inserta `HttpClient` para obtener datos de previsión del tiempo del archivo *weather.json* de la carpeta *wwwroot/sample-data*.</span><span class="sxs-lookup"><span data-stu-id="5e120-167">If working with a Blazor WebAssembly app, `HttpClient` is injected to obtain weather forecast data from the *weather.json* file in the *wwwroot/sample-data* folder.</span></span>

<span data-ttu-id="5e120-168">*Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="5e120-168">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-8)]

<span data-ttu-id="5e120-169">Se usa un bucle [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) para representar cada instancia de previsión como una fila en la tabla de datos meteorológicos:</span><span class="sxs-lookup"><span data-stu-id="5e120-169">An [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="5e120-170">Creación de una lista de tareas pendientes</span><span class="sxs-lookup"><span data-stu-id="5e120-170">Build a todo list</span></span>

<span data-ttu-id="5e120-171">Agregue un nuevo componente a la aplicación que implemente una simple lista de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="5e120-171">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="5e120-172">Agregue un archivo vacío denominado *Todo.razor* a la aplicación en la carpeta *Pages*:</span><span class="sxs-lookup"><span data-stu-id="5e120-172">Add an empty file named *Todo.razor* to the app in the *Pages* folder:</span></span>

1. <span data-ttu-id="5e120-173">Proporcione el marcado inicial para el componente:</span><span class="sxs-lookup"><span data-stu-id="5e120-173">Provide the initial markup for the component:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="5e120-174">Agregue el componente `Todo` a la barra de navegación.</span><span class="sxs-lookup"><span data-stu-id="5e120-174">Add the `Todo` component to the navigation bar.</span></span>

   <span data-ttu-id="5e120-175">El componente `NavMenu` (*Shared/NavMenu.razor*) se usa en el diseño de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5e120-175">The `NavMenu` component (*Shared/NavMenu.razor*) is used in the app's layout.</span></span> <span data-ttu-id="5e120-176">Los diseños son componentes que le permiten impedir la duplicación de contenido en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5e120-176">Layouts are components that allow you to avoid duplication of content in the app.</span></span>

   <span data-ttu-id="5e120-177">Agregue un elemento `<NavLink>` al componente `Todo` mediante la incorporación del siguiente marcado de elementos de lista debajo de los elementos de lista existentes en el archivo *Shared/NavMenu.razor*:</span><span class="sxs-lookup"><span data-stu-id="5e120-177">Add a `<NavLink>` element for the `Todo` component by adding the following list item markup below the existing list items in the *Shared/NavMenu.razor* file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="5e120-178">Recompile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5e120-178">Rebuild and run the app.</span></span> <span data-ttu-id="5e120-179">Visite la nueva página Todo para confirmar que el vínculo al componente `Todo` funcione.</span><span class="sxs-lookup"><span data-stu-id="5e120-179">Visit the new Todo page to confirm that the link to the `Todo` component works.</span></span>

1. <span data-ttu-id="5e120-180">Agregue un archivo *TodoItem.cs* a la raíz del proyecto para contener una clase que represente un elemento de la lista de tareas.</span><span class="sxs-lookup"><span data-stu-id="5e120-180">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="5e120-181">Use el siguiente código de C# para la clase `TodoItem`:</span><span class="sxs-lookup"><span data-stu-id="5e120-181">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. <span data-ttu-id="5e120-182">Vuelva al componente `Todo` (*Pages/Todo.razor*):</span><span class="sxs-lookup"><span data-stu-id="5e120-182">Return to the `Todo` component (*Pages/Todo.razor*):</span></span>

   * <span data-ttu-id="5e120-183">Agregue un campo a los elementos de tareas pendientes en un bloque `@code`.</span><span class="sxs-lookup"><span data-stu-id="5e120-183">Add a field for the todo items in an `@code` block.</span></span> <span data-ttu-id="5e120-184">El componente `Todo` utiliza este campo para mantener el estado de la lista de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="5e120-184">The `Todo` component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="5e120-185">Agregue el marcado de la lista no ordenada y un bucle `foreach` para que cada elemento de la lista se represente en un elemento de la lista de tareas pendientes (`<li>`).</span><span class="sxs-lookup"><span data-stu-id="5e120-185">Add unordered list markup and a `foreach` loop to render each todo item as a list item (`<li>`).</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="5e120-186">Para agregar elementos de tareas pendientes a la lista, la aplicación requiere elementos de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="5e120-186">The app requires UI elements for adding todo items to the list.</span></span> <span data-ttu-id="5e120-187">Agregue una entrada de texto (`<input>`) y un botón (`<button>`) debajo de la lista no ordenada (`<ul>...</ul>`):</span><span class="sxs-lookup"><span data-stu-id="5e120-187">Add a text input (`<input>`) and a button (`<button>`) below the unordered list (`<ul>...</ul>`):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="5e120-188">Recompile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5e120-188">Rebuild and run the app.</span></span> <span data-ttu-id="5e120-189">Cuando se selecciona el botón **Add todo** (Agregar tarea pendiente), no ocurre nada porque no hay ningún controlador de eventos conectado al botón.</span><span class="sxs-lookup"><span data-stu-id="5e120-189">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="5e120-190">Agregue un método `AddTodo` al componente `Todo` y regístrelo para seleccionar los botones mediante el atributo `@onclick`.</span><span class="sxs-lookup"><span data-stu-id="5e120-190">Add an `AddTodo` method to the `Todo` component and register it for button selections using the `@onclick` attribute.</span></span> <span data-ttu-id="5e120-191">El método `AddTodo` de C# se llama cuando se selecciona el botón:</span><span class="sxs-lookup"><span data-stu-id="5e120-191">The `AddTodo` C# method is called when the button is selected:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

1. <span data-ttu-id="5e120-192">Para obtener el título del nuevo elemento de tarea pendiente, agregue un campo de cadena `newTodo` en la parte superior del bloque `@code` y enlácelo al valor de la entrada de texto mediante el atributo `bind` en el elemento `<input>`:</span><span class="sxs-lookup"><span data-stu-id="5e120-192">To get the title of the new todo item, add a `newTodo` string field at the top of the `@code` block and bind it to the value of the text input using the `bind` attribute in the `<input>` element:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" @bind="newTodo" />
   ```

1. <span data-ttu-id="5e120-193">Actualice el método `AddTodo` para agregar el `TodoItem` con el título especificado a la lista.</span><span class="sxs-lookup"><span data-stu-id="5e120-193">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="5e120-194">Borre el valor de la entrada de texto mediante el establecimiento de `newTodo` en una cadena vacía:</span><span class="sxs-lookup"><span data-stu-id="5e120-194">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="5e120-195">Recompile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5e120-195">Rebuild and run the app.</span></span> <span data-ttu-id="5e120-196">Agregue algunos elementos de tareas pendientes a la lista de tareas pendientes para probar el nuevo código.</span><span class="sxs-lookup"><span data-stu-id="5e120-196">Add some todo items to the todo list to test the new code.</span></span>

1. <span data-ttu-id="5e120-197">Se puede hacer que el texto de título de cada elemento de tarea pendiente sea editable y una casilla puede ayudar al usuario a realizar un seguimiento de los elementos completados.</span><span class="sxs-lookup"><span data-stu-id="5e120-197">The title text for each todo item can be made editable, and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="5e120-198">Agregue una entrada de casilla a cada elemento de tarea pendiente y enlace su valor a la propiedad `IsDone`.</span><span class="sxs-lookup"><span data-stu-id="5e120-198">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="5e120-199">Cambie `@todo.Title` a un elemento `<input>` enlazado a `@todo.Title`:</span><span class="sxs-lookup"><span data-stu-id="5e120-199">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="5e120-200">Para comprobar que estos valores están enlazados, actualice el encabezado `<h1>` para mostrar un recuento del número de elementos de la lista de tareas pendientes que no se han completado (`IsDone` es `false`).</span><span class="sxs-lookup"><span data-stu-id="5e120-200">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="5e120-201">El componente `Todo` completado (*Pages/Todo.razor*):</span><span class="sxs-lookup"><span data-stu-id="5e120-201">The completed `Todo` component (*Pages/Todo.razor*):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. <span data-ttu-id="5e120-202">Recompile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5e120-202">Rebuild and run the app.</span></span> <span data-ttu-id="5e120-203">Agregue elementos de tarea pendiente para probar el nuevo código.</span><span class="sxs-lookup"><span data-stu-id="5e120-203">Add todo items to test the new code.</span></span>

> [!div class="nextstepaction"]
> <xref:blazor/components>
