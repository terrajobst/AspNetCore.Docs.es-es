---
title: Creación de la primera aplicación Blazor
author: guardrex
description: Cree una aplicación Blazor paso a paso.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/18/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: d235fec4e128ad8622a06d301eeac15c4862c159
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087735"
---
# <a name="build-your-first-blazor-app"></a><span data-ttu-id="4e5d9-103">Creación de la primera aplicación Blazor</span><span class="sxs-lookup"><span data-stu-id="4e5d9-103">Build your first Blazor app</span></span>

<span data-ttu-id="4e5d9-104">Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4e5d9-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4e5d9-105">En este tutorial se muestra cómo crear y modificar una aplicación de Blazor.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-105">This tutorial shows you how to build and modify a Blazor app.</span></span>

<span data-ttu-id="4e5d9-106">Siga las instrucciones del artículo <xref:blazor/get-started> para crear un proyecto de Blazor en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-106">Follow the guidance in the <xref:blazor/get-started> article to create a Blazor project for this tutorial.</span></span>

## <a name="build-components"></a><span data-ttu-id="4e5d9-107">Creación de componentes</span><span class="sxs-lookup"><span data-stu-id="4e5d9-107">Build components</span></span>

1. <span data-ttu-id="4e5d9-108">Vaya a cada una de las tres páginas de la aplicación en la carpeta *Pages*: Home (Inicio), Counter (Contador) y Fetch data (Recuperar datos).</span><span class="sxs-lookup"><span data-stu-id="4e5d9-108">Browse to each of the app's three pages in the *Pages* folder: Home, Counter, and Fetch data.</span></span> <span data-ttu-id="4e5d9-109">Estas páginas se implementan mediante los archivos de componente de Razor *Index.razor*, *Counter.razor* y *FetchData.razor*.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-109">These pages are implemented by the Razor component files *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span>

1. <span data-ttu-id="4e5d9-110">En la página Contador, seleccione el botón **Click me** para aumentar el contador sin una actualización de página.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-110">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="4e5d9-111">Aumentar un contador en una página web suele requerir la escritura de JavaScript, pero Blazor proporciona una mejor manera de usar C#.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-111">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

1. <span data-ttu-id="4e5d9-112">Examine la implementación del componente Counter en el archivo *Counter.razor*.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-112">Examine the implementation of the Counter component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="4e5d9-113">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="4e5d9-113">*Pages/Counter.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="4e5d9-114">la interfaz de usuario del componente Counter se define mediante HTML.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-114">The UI of the Counter component is defined using HTML.</span></span> <span data-ttu-id="4e5d9-115">La lógica de la representación dinámica (por ejemplo, bucles, instrucciones condicionales, expresiones) se agrega mediante una sintaxis de C# insertada denominada [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="4e5d9-115">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="4e5d9-116">El marcado HTML y la lógica de representación de C# se convierten en una clase de componente en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-116">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="4e5d9-117">El nombre de la clase de .NET generada coincide con el nombre del archivo.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-117">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="4e5d9-118">Los miembros de la clase de componente se definen en un bloque `@functions`.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-118">Members of the component class are defined in an `@functions` block.</span></span> <span data-ttu-id="4e5d9-119">En el bloque `@functions`, se especifica el estado del componente (propiedades, campos) y los métodos para el tratamiento de eventos o para definir otra lógica del componente.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-119">In the `@functions` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="4e5d9-120">Estos miembros se utilizan como parte de la lógica de representación del componente y para el tratamiento de eventos.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-120">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="4e5d9-121">Al seleccionarse el botón **Click me**:</span><span class="sxs-lookup"><span data-stu-id="4e5d9-121">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="4e5d9-122">Se llama al controlador `onclick` registrado del componente Counter (el método `IncrementCount`).</span><span class="sxs-lookup"><span data-stu-id="4e5d9-122">The Counter component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="4e5d9-123">El componente Counter regenera su árbol de representación.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-123">The Counter component regenerates its render tree.</span></span>
   * <span data-ttu-id="4e5d9-124">El nuevo árbol de representación se compara con el anterior.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-124">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="4e5d9-125">Únicamente se aplican modificaciones en Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="4e5d9-125">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="4e5d9-126">Se actualiza el recuento mostrado.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-126">The displayed count is updated.</span></span>

1. <span data-ttu-id="4e5d9-127">Modifique la lógica de C# del componente Counter para hacer que el recuento se incremente en dos en lugar de uno.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-127">Modify the C# logic of the Counter component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="4e5d9-128">Recompile y ejecute la aplicación para ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-128">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="4e5d9-129">Seleccione el botón **Hacer clic aquí**.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-129">Select the **Click me** button.</span></span> <span data-ttu-id="4e5d9-130">El contador se incrementa en dos.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-130">The counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="4e5d9-131">Uso de componentes</span><span class="sxs-lookup"><span data-stu-id="4e5d9-131">Use components</span></span>

<span data-ttu-id="4e5d9-132">Incluya un componente en otro componente mediante una sintaxis HTML.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-132">Include a component in another component using an HTML syntax.</span></span>

1. <span data-ttu-id="4e5d9-133">Agregue el componente Counter al componente Index de la aplicación; para ello, agregue un elemento `<Counter />` al componente Index (*Index.razor*).</span><span class="sxs-lookup"><span data-stu-id="4e5d9-133">Add the Counter component to the app's Index component by adding a `<Counter />` element to the Index component (*Index.razor*).</span></span>

   <span data-ttu-id="4e5d9-134">Si usa Blazor para esta experiencia, hay un componente Survey Prompt (elemento `<SurveyPrompt>`) en el componente Index.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-134">If you're using Blazor client-side for this experience, a Survey Prompt component (`<SurveyPrompt>` element) is in the Index component.</span></span> <span data-ttu-id="4e5d9-135">Reemplace el elemento `<SurveyPrompt>` por el elemento `<Counter>`.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-135">Replace the `<SurveyPrompt>` element with the `<Counter>` element.</span></span> <span data-ttu-id="4e5d9-136">Si usa una aplicación de servidor de Blazor para esta experiencia, agregue el elemento `<Counter>` al componente Index:</span><span class="sxs-lookup"><span data-stu-id="4e5d9-136">If you're using a Blazor server-side app for this experience, add the `<Counter>` element to the Index component:</span></span>

   <span data-ttu-id="4e5d9-137">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="4e5d9-137">*Pages/Index.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. <span data-ttu-id="4e5d9-138">Recompile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-138">Rebuild and run the app.</span></span> <span data-ttu-id="4e5d9-139">El componente Index tiene su propio contador.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-139">The Index component has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="4e5d9-140">Parámetros del componente</span><span class="sxs-lookup"><span data-stu-id="4e5d9-140">Component parameters</span></span>

<span data-ttu-id="4e5d9-141">Los componentes también pueden tener parámetros.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-141">Components can also have parameters.</span></span> <span data-ttu-id="4e5d9-142">Los parámetros del componente se definen mediante propiedades privadas en la clase de componentes decorada con `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-142">Component parameters are defined using non-public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="4e5d9-143">Use atributos para especificar argumentos para un componente en el marcado.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-143">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="4e5d9-144">Actualice el código de C# `@functions` del componente:</span><span class="sxs-lookup"><span data-stu-id="4e5d9-144">Update the component's `@functions` C# code:</span></span>

   * <span data-ttu-id="4e5d9-145">Agregue una propiedad `IncrementAmount` decorada con el atributo `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-145">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="4e5d9-146">Cambie el método `IncrementCount` para usar `IncrementAmount` al aumentar el valor de `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-146">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="4e5d9-147">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="4e5d9-147">*Pages/Counter.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="4e5d9-148">Especifique un parámetro `IncrementAmount` en el elemento `<Counter>` del componente Index mediante un atributo.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-148">Specify an `IncrementAmount` parameter in the Index component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="4e5d9-149">Establezca el valor para incrementar el contador en diez.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-149">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="4e5d9-150">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="4e5d9-150">*Pages/Index.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. <span data-ttu-id="4e5d9-151">Vuelva a cargar el componente Index.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-151">Reload the Index component.</span></span> <span data-ttu-id="4e5d9-152">El contador se incrementa en diez cada vez que se selecciona el botón **Click me**.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-152">The counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="4e5d9-153">El contador del componente Counter sigue incrementándose en uno.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-153">The counter in the Counter component continues to increment by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="4e5d9-154">Enrutamiento a los componentes</span><span class="sxs-lookup"><span data-stu-id="4e5d9-154">Route to components</span></span>

<span data-ttu-id="4e5d9-155">La directiva `@page` en la parte superior del archivo *Counter.razor* especifica que el componente Counter es un punto de conexión de enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-155">The `@page` directive at the top of the *Counter.razor* file specifies that the Counter component is a routing endpoint.</span></span> <span data-ttu-id="4e5d9-156">El componente Counter controla las solicitudes enviadas a `/counter`.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-156">The Counter component handles requests sent to `/counter`.</span></span> <span data-ttu-id="4e5d9-157">Sin la directiva `@page`, el componente no controla las solicitudes enrutadas, pero otros componentes aún pueden usar el componente.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-157">Without the `@page` directive, a component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="4e5d9-158">Inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="4e5d9-158">Dependency injection</span></span>

<span data-ttu-id="4e5d9-159">Los servicios registrados en el contenedor de servicios de la aplicación están disponibles para los componentes mediante una [inserción de dependencia (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="4e5d9-159">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="4e5d9-160">Inserte servicios en un componente mediante la directiva `@inject`.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-160">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="4e5d9-161">Examine las directivas del componente FetchData.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-161">Examine the directives of the FetchData component.</span></span>

<span data-ttu-id="4e5d9-162">Si trabaja con la aplicación de servidor de Blazor, el servicio `WeatherForecastService` se registra como [singleton](xref:fundamentals/dependency-injection#service-lifetimes), de modo que una instancia del servicio está disponible en toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-162">If working with a Blazor server-side app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span> <span data-ttu-id="4e5d9-163">La directiva `@inject` se usa para insertar la instancia del servicio `WeatherForecastService` en el componente.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-163">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component.</span></span>

<span data-ttu-id="4e5d9-164">*Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="4e5d9-164">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="4e5d9-165">El componente FetchData usa el servicio insertado, como `ForecastService`, para recuperar una matriz de objetos `WeatherForecast`:</span><span class="sxs-lookup"><span data-stu-id="4e5d9-165">The FetchData component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

<span data-ttu-id="4e5d9-166">Si trabaja con la aplicación cliente de Blazor, se inserta `HttpClient` para obtener datos de previsión del tiempo del archivo *weather.json* de la carpeta *wwwroot/sample-data*:</span><span class="sxs-lookup"><span data-stu-id="4e5d9-166">If working with a Blazor client-side app, `HttpClient` is injected to obtain weather forecast data from the *weather.json* file in the *wwwroot/sample-data* folder:</span></span>

<span data-ttu-id="4e5d9-167">*Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="4e5d9-167">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-8)]

<span data-ttu-id="4e5d9-168">Se usa un bucle [\@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) para representar cada instancia de previsión como una fila de la tabla de datos meteorológicos:</span><span class="sxs-lookup"><span data-stu-id="4e5d9-168">A [\@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]


## <a name="build-a-todo-list"></a><span data-ttu-id="4e5d9-169">Creación de una lista de tareas pendientes</span><span class="sxs-lookup"><span data-stu-id="4e5d9-169">Build a todo list</span></span>

<span data-ttu-id="4e5d9-170">Agregue un nuevo componente a la aplicación que implemente una simple lista de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-170">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="4e5d9-171">Agregue un archivo vacío denominado *Todo.razor* a la aplicación en la carpeta *Pages*:</span><span class="sxs-lookup"><span data-stu-id="4e5d9-171">Add an empty file named *Todo.razor* to the app in the *Pages* folder:</span></span>

1. <span data-ttu-id="4e5d9-172">Proporcione el marcado inicial para el componente:</span><span class="sxs-lookup"><span data-stu-id="4e5d9-172">Provide the initial markup for the component:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="4e5d9-173">Agregue el componente Todo a la barra de navegación.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-173">Add the Todo component to the navigation bar.</span></span>

   <span data-ttu-id="4e5d9-174">El componente NavMenu (*Shared/NavMenu.razor*) se usa en el diseño de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-174">The NavMenu component (*Shared/NavMenu.razor*) is used in the app's layout.</span></span> <span data-ttu-id="4e5d9-175">Los diseños son componentes que le permiten impedir la duplicación de contenido en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-175">Layouts are components that allow you to avoid duplication of content in the app.</span></span> <span data-ttu-id="4e5d9-176">Para obtener más información, vea <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-176">For more information, see <xref:blazor/layouts>.</span></span>

   <span data-ttu-id="4e5d9-177">Agregue un elemento `<NavLink>` a la página Todo mediante la adición del siguiente marcado de elementos de lista debajo de los elementos de lista existentes en el archivo *Shared/NavMenu.razor*:</span><span class="sxs-lookup"><span data-stu-id="4e5d9-177">Add a `<NavLink>` for the Todo component by adding the following list item markup below the existing list items in the *Shared/NavMenu.razor* file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="4e5d9-178">Recompile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-178">Rebuild and run the app.</span></span> <span data-ttu-id="4e5d9-179">Visite la nueva página Todo para confirmar que el vínculo al componente Todo funcione.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-179">Visit the new Todo page to confirm that the link to the Todo component works.</span></span>

1. <span data-ttu-id="4e5d9-180">Agregue un archivo *TodoItem.cs* a la raíz del proyecto para contener una clase que represente un elemento de la lista de tareas.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-180">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="4e5d9-181">Use el siguiente código de C# para la clase `TodoItem`:</span><span class="sxs-lookup"><span data-stu-id="4e5d9-181">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. <span data-ttu-id="4e5d9-182">Vuelva al componente Todo (*Pages/Todo.razor*):</span><span class="sxs-lookup"><span data-stu-id="4e5d9-182">Return to the Todo component (*Pages/Todo.razor*):</span></span>

   * <span data-ttu-id="4e5d9-183">Agregue un campo a los elementos de tareas pendientes en un bloque `@functions`.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-183">Add a field for the todo items in an `@functions` block.</span></span> <span data-ttu-id="4e5d9-184">El componente Todo utiliza este campo para mantener el estado de la lista de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-184">The Todo component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="4e5d9-185">Agregue el marcado de la lista no ordenada y un bucle `foreach` para que cada elemento de la lista se represente en un elemento de la lista de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-185">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="4e5d9-186">Para agregar elementos de tareas pendientes a la lista, la aplicación requiere elementos de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-186">The app requires UI elements for adding todo items to the list.</span></span> <span data-ttu-id="4e5d9-187">Agregue una entrada de texto y un botón debajo de la lista:</span><span class="sxs-lookup"><span data-stu-id="4e5d9-187">Add a text input and a button below the list:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="4e5d9-188">Recompile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-188">Rebuild and run the app.</span></span> <span data-ttu-id="4e5d9-189">Cuando se selecciona el botón **Add todo** (Agregar tarea pendiente), no ocurre nada porque no hay ningún controlador de eventos conectado al botón.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-189">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="4e5d9-190">Agregue un método `AddTodo` al componente Todo y regístrelo para hacer clic en los botones mediante el atributo `onclick`:</span><span class="sxs-lookup"><span data-stu-id="4e5d9-190">Add an `AddTodo` method to the Todo component and register it for button clicks using the `onclick` attribute:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

   <span data-ttu-id="4e5d9-191">El método `AddTodo` de C# se llama cuando se selecciona el botón.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-191">The `AddTodo` C# method is called when the button is selected.</span></span>

1. <span data-ttu-id="4e5d9-192">Para obtener el título del nuevo elemento de tarea pendiente, agregue un campo de cadena `newTodo` y enlácelo al valor de la entrada de texto mediante el atributo `bind`:</span><span class="sxs-lookup"><span data-stu-id="4e5d9-192">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo">
   ```

1. <span data-ttu-id="4e5d9-193">Actualice el método `AddTodo` para agregar el `TodoItem` con el título especificado a la lista.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-193">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="4e5d9-194">Borre el valor de la entrada de texto mediante el establecimiento de `newTodo` en una cadena vacía:</span><span class="sxs-lookup"><span data-stu-id="4e5d9-194">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="4e5d9-195">Recompile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-195">Rebuild and run the app.</span></span> <span data-ttu-id="4e5d9-196">Agregue algunos elementos de tareas pendientes a la lista de tareas pendientes para probar el nuevo código.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-196">Add some todo items to the todo list to test the new code.</span></span>

1. <span data-ttu-id="4e5d9-197">Se puede hacer que el texto de título de cada elemento de tarea pendiente sea editable y una casilla puede ayudar al usuario a realizar un seguimiento de los elementos completados.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-197">The title text for each todo item can be made editable and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="4e5d9-198">Agregue una entrada de casilla a cada elemento de tarea pendiente y enlace su valor a la propiedad `IsDone`.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-198">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="4e5d9-199">Cambie `@todo.Title` a un elemento `<input>` enlazado a `@todo.Title`:</span><span class="sxs-lookup"><span data-stu-id="4e5d9-199">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="4e5d9-200">Para comprobar que estos valores están enlazados, actualice el encabezado `<h1>` para mostrar un recuento del número de elementos de la lista de tareas pendientes que no se han completado (`IsDone` es `false`).</span><span class="sxs-lookup"><span data-stu-id="4e5d9-200">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="4e5d9-201">El componente Todo completado (*Pages/Todo.razor*):</span><span class="sxs-lookup"><span data-stu-id="4e5d9-201">The completed Todo component (*Pages/Todo.razor*):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. <span data-ttu-id="4e5d9-202">Recompile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-202">Rebuild and run the app.</span></span> <span data-ttu-id="4e5d9-203">Agregue elementos de tarea pendiente para probar el nuevo código.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-203">Add todo items to test the new code.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="4e5d9-204">Publicar e implementar la aplicación</span><span class="sxs-lookup"><span data-stu-id="4e5d9-204">Publish and deploy the app</span></span>

<span data-ttu-id="4e5d9-205">Para publicar la aplicación, consulte <xref:host-and-deploy/blazor/index>.</span><span class="sxs-lookup"><span data-stu-id="4e5d9-205">To publish the app, see <xref:host-and-deploy/blazor/index>.</span></span>
