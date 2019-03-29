---
title: Compilación de su primera aplicación Razor Components
author: guardrex
description: Compile una aplicación Razor Components paso a paso y aprenda conceptos básicos de Razor Components.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/24/2019
uid: tutorials/first-razor-components-app
ms.openlocfilehash: 2a987b3f2e687cd9d4dffa2c573c938e68ea3cc8
ms.sourcegitcommit: 7d6019f762fc5b8cbedcd69801e8310f51a17c18
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2019
ms.locfileid: "58419370"
---
# <a name="build-your-first-razor-components-app"></a><span data-ttu-id="22902-103">Compilación de su primera aplicación Razor Components</span><span class="sxs-lookup"><span data-stu-id="22902-103">Build your first Razor Components app</span></span>

<span data-ttu-id="22902-104">Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="22902-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="22902-105">En este tutorial se le muestra cómo compilar una aplicación con Razor Components. En él, además, se muestran conceptos básicos de Razor Components.</span><span class="sxs-lookup"><span data-stu-id="22902-105">This tutorial shows you how to build an app with Razor Components and demonstrates basic Razor Components concepts.</span></span> <span data-ttu-id="22902-106">Puede disfrutar este tutorial mediante un proyecto basado en Razor Components (admitido en .NET Core 3.0 o posterior) o mediante un proyecto basado en Blazor (admitido en una versión futura de .NET Core).</span><span class="sxs-lookup"><span data-stu-id="22902-106">You can enjoy this tutorial using either a Razor Components-based project (supported in .NET Core 3.0 or later) or using a Blazor-based project (supported in a future release of .NET Core).</span></span>

<span data-ttu-id="22902-107">Para una experiencia con Razor Components de ASP.NET Core (*se recomienda*):</span><span class="sxs-lookup"><span data-stu-id="22902-107">For an experience using ASP.NET Core Razor Components (*recommended*):</span></span>

* <span data-ttu-id="22902-108">Siga la guía en <xref:razor-components/get-started> para crear un proyecto basado en Razor Components.</span><span class="sxs-lookup"><span data-stu-id="22902-108">Follow the guidance in <xref:razor-components/get-started> to create a Razor Components-based project.</span></span>
* <span data-ttu-id="22902-109">Dé un nombre al proyecto `RazorComponents`.</span><span class="sxs-lookup"><span data-stu-id="22902-109">Name the project `RazorComponents`.</span></span>

<span data-ttu-id="22902-110">Para una experiencia con Blazor:</span><span class="sxs-lookup"><span data-stu-id="22902-110">For an experience using Blazor:</span></span>

* <span data-ttu-id="22902-111">Siga la guía en <xref:spa/blazor/get-started> para crear un proyecto basado en Blazor.</span><span class="sxs-lookup"><span data-stu-id="22902-111">Follow the guidance in <xref:spa/blazor/get-started> to create a Blazor-based project.</span></span>
* <span data-ttu-id="22902-112">Dé un nombre al proyecto `Blazor`.</span><span class="sxs-lookup"><span data-stu-id="22902-112">Name the project `Blazor`.</span></span>

<span data-ttu-id="22902-113">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="22902-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="22902-114">Para ver los requisitos previos, consulte los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="22902-114">See the following topics for prerequisites:</span></span>

## <a name="build-components"></a><span data-ttu-id="22902-115">Creación de componentes</span><span class="sxs-lookup"><span data-stu-id="22902-115">Build components</span></span>

1. <span data-ttu-id="22902-116">Vaya a cada una de las tres páginas de la aplicación en la carpeta *Components/Pages* (*Pages* en Blazor): Home (Inicio), Counter (Contador) y Fetch data (Recuperar datos).</span><span class="sxs-lookup"><span data-stu-id="22902-116">Browse to each of the app's three pages in the *Components/Pages* folder (*Pages* in Blazor): Home, Counter, and Fetch data.</span></span> <span data-ttu-id="22902-117">Estas páginas se implementan mediante los archivos de componentes de Razor: *Index.razor*, *Counter.razor* y *FetchData.razor*</span><span class="sxs-lookup"><span data-stu-id="22902-117">These pages are implemented by Razor Component files: *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span> <span data-ttu-id="22902-118">(Blazor continúa usando la extensión de archivo *.cshtml*: *Index.cshtml*, *Counter.cshtml* y *FetchData.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="22902-118">(Blazor continues to use the *.cshtml* file extension: *Index.cshtml*, *Counter.cshtml*, and *FetchData.cshtml*.)</span></span>

1. <span data-ttu-id="22902-119">En la página Contador, seleccione el botón **Click me** para aumentar el contador sin una actualización de página.</span><span class="sxs-lookup"><span data-stu-id="22902-119">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="22902-120">Aumentar un contador en una página web suele requerir la escritura de JavaScript, pero Razor Components proporciona una mejor manera de usar C#.</span><span class="sxs-lookup"><span data-stu-id="22902-120">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

1. <span data-ttu-id="22902-121">Examine la implementación del componente Counter en el archivo *Counter.razor*.</span><span class="sxs-lookup"><span data-stu-id="22902-121">Examine the implementation of the Counter component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="22902-122">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* en Blazor):</span><span class="sxs-lookup"><span data-stu-id="22902-122">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="22902-123">la interfaz de usuario del componente Counter se define mediante HTML.</span><span class="sxs-lookup"><span data-stu-id="22902-123">The UI of the Counter component is defined using HTML.</span></span> <span data-ttu-id="22902-124">La lógica de la representación dinámica (por ejemplo, bucles, instrucciones condicionales, expresiones) se agrega mediante una sintaxis de C# insertada denominada [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="22902-124">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="22902-125">El marcado HTML y la lógica de representación de C# se convierten en una clase de componente en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="22902-125">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="22902-126">El nombre de la clase de .NET generada coincide con el nombre del archivo.</span><span class="sxs-lookup"><span data-stu-id="22902-126">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="22902-127">Los miembros de la clase de componente se definen en un bloque `@functions`.</span><span class="sxs-lookup"><span data-stu-id="22902-127">Members of the component class are defined in a `@functions` block.</span></span> <span data-ttu-id="22902-128">En el bloque `@functions`, se especifica el estado del componente (propiedades, campos) y los métodos para el tratamiento de eventos o para definir otra lógica del componente.</span><span class="sxs-lookup"><span data-stu-id="22902-128">In the `@functions` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="22902-129">Estos miembros se utilizan como parte de la lógica de representación del componente y para el tratamiento de eventos.</span><span class="sxs-lookup"><span data-stu-id="22902-129">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="22902-130">Al seleccionarse el botón **Click me**:</span><span class="sxs-lookup"><span data-stu-id="22902-130">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="22902-131">Se llama al controlador `onclick` registrado del componente Counter (el método `IncrementCount`).</span><span class="sxs-lookup"><span data-stu-id="22902-131">The Counter component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="22902-132">El componente Counter regenera su árbol de representación.</span><span class="sxs-lookup"><span data-stu-id="22902-132">The Counter component regenerates its render tree.</span></span>
   * <span data-ttu-id="22902-133">El nuevo árbol de representación se compara con el anterior.</span><span class="sxs-lookup"><span data-stu-id="22902-133">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="22902-134">Únicamente se aplican modificaciones en Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="22902-134">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="22902-135">Se actualiza el recuento mostrado.</span><span class="sxs-lookup"><span data-stu-id="22902-135">The displayed count is updated.</span></span>

1. <span data-ttu-id="22902-136">Modifique la lógica de C# del componente Counter para hacer que el recuento se incremente en dos en lugar de uno.</span><span class="sxs-lookup"><span data-stu-id="22902-136">Modify the C# logic of the Counter component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="22902-137">Recompile y ejecute la aplicación para ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="22902-137">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="22902-138">Seleccione el botón **Click me** y el contador se incrementará en dos.</span><span class="sxs-lookup"><span data-stu-id="22902-138">Select the **Click me** button, and the counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="22902-139">Uso de componentes</span><span class="sxs-lookup"><span data-stu-id="22902-139">Use components</span></span>

<span data-ttu-id="22902-140">Incluya un componente en otro componente mediante una sintaxis similar a HTML.</span><span class="sxs-lookup"><span data-stu-id="22902-140">Include a component into another component using an HTML-like syntax.</span></span>

1. <span data-ttu-id="22902-141">Agregue el componente Counter al componente Index (Home) de la aplicación, agregando a su vez un elemento `<Counter />` al componente Index.</span><span class="sxs-lookup"><span data-stu-id="22902-141">Add the Counter component to the app's Index (Home) component by adding a `<Counter />` element to the Index component.</span></span>

   <span data-ttu-id="22902-142">Si usa Blazor para esta experiencia, un componente Survey Prompt (elemento `<SurveyPrompt>`) está en el componente Index.</span><span class="sxs-lookup"><span data-stu-id="22902-142">If you're using Blazor for this experience, a Survey Prompt component (`<SurveyPrompt>` element) is in the Index component.</span></span> <span data-ttu-id="22902-143">Reemplace el elemento `<SurveyPrompt>` por el elemento `<Counter>`.</span><span class="sxs-lookup"><span data-stu-id="22902-143">Replace the `<SurveyPrompt>` element with the `<Counter>` element.</span></span>

   <span data-ttu-id="22902-144">*Components/Pages/Index.razor* (*Pages/Index.cshtml* en Blazor):</span><span class="sxs-lookup"><span data-stu-id="22902-144">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Index.razor?highlight=7)]

1. <span data-ttu-id="22902-145">Recompile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="22902-145">Rebuild and run the app.</span></span> <span data-ttu-id="22902-146">La página Home tiene su propio contador.</span><span class="sxs-lookup"><span data-stu-id="22902-146">The Home page has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="22902-147">Parámetros del componente</span><span class="sxs-lookup"><span data-stu-id="22902-147">Component parameters</span></span>

<span data-ttu-id="22902-148">Los componentes también pueden tener parámetros.</span><span class="sxs-lookup"><span data-stu-id="22902-148">Components can also have parameters.</span></span> <span data-ttu-id="22902-149">Los parámetros del componente se definen mediante propiedades privadas en la clase de componentes decorada con `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="22902-149">Component parameters are defined using non-public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="22902-150">Use atributos para especificar argumentos para un componente en el marcado.</span><span class="sxs-lookup"><span data-stu-id="22902-150">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="22902-151">Actualice el código de C# `@functions` del componente:</span><span class="sxs-lookup"><span data-stu-id="22902-151">Update the component's `@functions` C# code:</span></span>

   * <span data-ttu-id="22902-152">Agregue una propiedad `IncrementAmount` decorada con el atributo `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="22902-152">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="22902-153">Cambie el método `IncrementCount` para usar `IncrementAmount` al aumentar el valor de `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="22902-153">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="22902-154">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* en Blazor):</span><span class="sxs-lookup"><span data-stu-id="22902-154">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Counter.razor?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="22902-155">Especifique un parámetro `IncrementAmount` en el elemento `<Counter>` del componente Home mediante un atributo.</span><span class="sxs-lookup"><span data-stu-id="22902-155">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="22902-156">Establezca el valor para incrementar el contador en diez.</span><span class="sxs-lookup"><span data-stu-id="22902-156">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="22902-157">*Components/Pages/Index.razor* (*Pages/Index.cshtml* en Blazor):</span><span class="sxs-lookup"><span data-stu-id="22902-157">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Index.razor?highlight=7)]

1. <span data-ttu-id="22902-158">Vuelva a cargar la página Home.</span><span class="sxs-lookup"><span data-stu-id="22902-158">Reload the Home page.</span></span> <span data-ttu-id="22902-159">El contador se incrementa en diez cada vez que se selecciona el botón **Click me**.</span><span class="sxs-lookup"><span data-stu-id="22902-159">The counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="22902-160">El contador de la página Counter se incrementa en uno.</span><span class="sxs-lookup"><span data-stu-id="22902-160">The counter on the Counter page increments by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="22902-161">Enrutamiento a los componentes</span><span class="sxs-lookup"><span data-stu-id="22902-161">Route to components</span></span>

<span data-ttu-id="22902-162">La directiva `@page` en la parte superior del archivo *Counter.razor* especifica que este componente es un punto de conexión de enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="22902-162">The `@page` directive at the top of the *Counter.razor* file specifies that this component is a routing endpoint.</span></span> <span data-ttu-id="22902-163">El componente Counter controla las solicitudes enviadas a `/Counter`.</span><span class="sxs-lookup"><span data-stu-id="22902-163">The Counter component handles requests sent to `/Counter`.</span></span> <span data-ttu-id="22902-164">Sin la directiva `@page`, el componente no controla las solicitudes de enrutado, pero el componente todavía puede usarse por otros componentes.</span><span class="sxs-lookup"><span data-stu-id="22902-164">Without the `@page` directive, the component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="22902-165">Inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="22902-165">Dependency injection</span></span>

<span data-ttu-id="22902-166">Los servicios registrados en el contenedor de servicios de la aplicación están disponibles para los componentes mediante una [inserción de dependencia (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="22902-166">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="22902-167">Inserte servicios en un componente mediante la directiva `@inject`.</span><span class="sxs-lookup"><span data-stu-id="22902-167">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="22902-168">Examine las directivas del componente FetchData de la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="22902-168">Examine the directives of the FetchData component in the sample app.</span></span>

<span data-ttu-id="22902-169">En la aplicación de ejemplo Razor Components, el servicio `WeatherForecastService` se registra como [singleton](xref:fundamentals/dependency-injection#service-lifetimes), de modo que una instancia del servicio está disponible en toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="22902-169">In the Razor Components sample app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span> <span data-ttu-id="22902-170">La directiva `@inject` se usa para insertar la instancia del servicio `WeatherForecastService` en el componente.</span><span class="sxs-lookup"><span data-stu-id="22902-170">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component.</span></span>

<span data-ttu-id="22902-171">*Components/Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="22902-171">*Components/Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="22902-172">El componente FetchData usa el servicio insertado, como `ForecastService`, para recuperar una matriz de objetos `WeatherForecast`:</span><span class="sxs-lookup"><span data-stu-id="22902-172">The FetchData component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

<span data-ttu-id="22902-173">En la versión de Blazor de la aplicación de ejemplo, se inyecta `HttpClient` para obtener los datos de la previsión meteorológica del archivo *weather.json* de la carpeta *wwwroot/ejemplo-data*:</span><span class="sxs-lookup"><span data-stu-id="22902-173">In the Blazor version of the sample app, `HttpClient` is injected to obtain weather forecast data from the *weather.json* file in the *wwwroot/sample-data* folder:</span></span>

<span data-ttu-id="22902-174">*Pages/FetchData.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="22902-174">*Pages/FetchData.cshtml*:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.cshtml?highlight=7)]

<span data-ttu-id="22902-175">En ambas aplicaciones de ejemplo se usa un bucle [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) para representar cada instancia de previsión como una fila en la tabla de datos meteorológicos:</span><span class="sxs-lookup"><span data-stu-id="22902-175">In both sample apps, a [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="22902-176">Creación de una lista de tareas pendientes</span><span class="sxs-lookup"><span data-stu-id="22902-176">Build a todo list</span></span>

<span data-ttu-id="22902-177">Agregue un nuevo componente a la aplicación que implemente una simple lista de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="22902-177">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="22902-178">Agregue un archivo vacío a la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="22902-178">Add an empty file to the sample app:</span></span>

   * <span data-ttu-id="22902-179">Para la experiencia de Razor Components, agregue un archivo *Todo.razor* a la carpeta *Components/Pages*.</span><span class="sxs-lookup"><span data-stu-id="22902-179">For the Razor Components experience, add a *Todo.razor* file to the *Components/Pages* folder.</span></span>
   * <span data-ttu-id="22902-180">Para la experiencia de Blazor, agregue un archivo *Todo.cshtml* a la carpeta *Pages*.</span><span class="sxs-lookup"><span data-stu-id="22902-180">For the Blazor experience, add a *Todo.cshtml* file to the *Pages* folder.</span></span>

1. <span data-ttu-id="22902-181">Proporcione el marcado inicial para el componente:</span><span class="sxs-lookup"><span data-stu-id="22902-181">Provide the initial markup for the component:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="22902-182">Agregue el componente Todo a la barra de navegación.</span><span class="sxs-lookup"><span data-stu-id="22902-182">Add the Todo component to the navigation bar.</span></span>

   <span data-ttu-id="22902-183">El componente NavMenu (*Components/Shared/NavMenu.razor* o *Shared/NavMenu.cshtml* en Blazor) se usa en el diseño de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="22902-183">The NavMenu component (*Components/Shared/NavMenu.razor* or *Shared/NavMenu.cshtml* in Blazor) is used in the app's layout.</span></span> <span data-ttu-id="22902-184">Los diseños son componentes que le permiten impedir la duplicación de contenido en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="22902-184">Layouts are components that allow you to avoid duplication of content in the app.</span></span> <span data-ttu-id="22902-185">Para obtener más información, vea <xref:razor-components/layouts>.</span><span class="sxs-lookup"><span data-stu-id="22902-185">For more information, see <xref:razor-components/layouts>.</span></span>

   <span data-ttu-id="22902-186">Agregue un vínculo `<NavLink>` al componente Todo mediante la incorporación del siguiente marcado de elementos de lista debajo de los elementos de lista existentes en el archivo *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* en Blazor):</span><span class="sxs-lookup"><span data-stu-id="22902-186">Add a `<NavLink>` for the Todo component by adding the following list item markup below the existing list items in the *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* in Blazor) file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="22902-187">Recompile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="22902-187">Rebuild and run the app.</span></span> <span data-ttu-id="22902-188">Visite la nueva página Todo para confirmar que el vínculo al componente Todo funcione.</span><span class="sxs-lookup"><span data-stu-id="22902-188">Visit the new Todo page to confirm that the link to the Todo component works.</span></span>

1. <span data-ttu-id="22902-189">Agregue un archivo *TodoItem.cs* a la raíz del proyecto para contener una clase que represente un elemento de la lista de tareas.</span><span class="sxs-lookup"><span data-stu-id="22902-189">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="22902-190">Use el siguiente código de C# para la clase `TodoItem`:</span><span class="sxs-lookup"><span data-stu-id="22902-190">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/TodoItem.cs)]

1. <span data-ttu-id="22902-191">Vuelva al componente Todo (*Components/Pages/Todo.razor* o *Pages/Todo.cshtml* en Blazor):</span><span class="sxs-lookup"><span data-stu-id="22902-191">Return to the Todo component (*Components/Pages/Todo.razor* or *Pages/Todo.cshtml* in Blazor):</span></span>

   * <span data-ttu-id="22902-192">Agregue un campo a las tareas pendientes en un bloque `@functions`.</span><span class="sxs-lookup"><span data-stu-id="22902-192">Add a field for the todos in an `@functions` block.</span></span> <span data-ttu-id="22902-193">El componente Todo utiliza este campo para mantener el estado de la lista de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="22902-193">The Todo component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="22902-194">Agregue el marcado de la lista no ordenada y un bucle `foreach` para que cada elemento de la lista se represente en un elemento de la lista de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="22902-194">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="22902-195">La aplicación requiere elementos de la interfaz de usuario para agregar tareas pendientes a la lista.</span><span class="sxs-lookup"><span data-stu-id="22902-195">The app requires UI elements for adding todos to the list.</span></span> <span data-ttu-id="22902-196">Agregue una entrada de texto y un botón debajo de la lista:</span><span class="sxs-lookup"><span data-stu-id="22902-196">Add a text input and a button below the list:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="22902-197">Recompile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="22902-197">Rebuild and run the app.</span></span> <span data-ttu-id="22902-198">Cuando se selecciona el botón **Add todo** (Agregar tarea pendiente), no ocurre nada porque no hay ningún controlador de eventos conectado al botón.</span><span class="sxs-lookup"><span data-stu-id="22902-198">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="22902-199">Agregue un método `AddTodo` al componente Todo y regístrelo para hacer clic en los botones mediante el atributo `onclick`:</span><span class="sxs-lookup"><span data-stu-id="22902-199">Add an `AddTodo` method to the Todo component and register it for button clicks using the `onclick` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

   <span data-ttu-id="22902-200">El método `AddTodo` de C# se llama cuando se selecciona el botón.</span><span class="sxs-lookup"><span data-stu-id="22902-200">The `AddTodo` C# method is called when the button is selected.</span></span>

1. <span data-ttu-id="22902-201">Para obtener el título del nuevo elemento de tarea pendiente, agregue un campo de cadena `newTodo` y enlácelo al valor de la entrada de texto mediante el atributo `bind`:</span><span class="sxs-lookup"><span data-stu-id="22902-201">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo" />
   ```

1. <span data-ttu-id="22902-202">Actualice el método `AddTodo` para agregar el `TodoItem` con el título especificado a la lista.</span><span class="sxs-lookup"><span data-stu-id="22902-202">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="22902-203">Borre el valor de la entrada de texto mediante el establecimiento de `newTodo` en una cadena vacía:</span><span class="sxs-lookup"><span data-stu-id="22902-203">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="22902-204">Recompile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="22902-204">Rebuild and run the app.</span></span> <span data-ttu-id="22902-205">Agregue algunas tareas pendientes a la lista de tareas pendientes para probar el nuevo código.</span><span class="sxs-lookup"><span data-stu-id="22902-205">Add some todos to the todo list to test the new code.</span></span>

1. <span data-ttu-id="22902-206">Se puede hacer que el texto de título de cada elemento de tarea pendiente sea editable y una casilla puede ayudar al usuario a realizar un seguimiento de los elementos completados.</span><span class="sxs-lookup"><span data-stu-id="22902-206">The title text for each todo item can be made editable and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="22902-207">Agregue una entrada de casilla a cada elemento de tarea pendiente y enlace su valor a la propiedad `IsDone`.</span><span class="sxs-lookup"><span data-stu-id="22902-207">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="22902-208">Cambie `@todo.Title` a un elemento `<input>` enlazado a `@todo.Title`:</span><span class="sxs-lookup"><span data-stu-id="22902-208">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="22902-209">Para comprobar que estos valores están enlazados, actualice el encabezado `<h1>` para mostrar un recuento del número de elementos de la lista de tareas pendientes que no se han completado (`IsDone` es `false`).</span><span class="sxs-lookup"><span data-stu-id="22902-209">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="22902-210">El componente Todo completado (*Components/Pages/Todo.razor* o *Pages/Todo.cshtml* en Blazor):</span><span class="sxs-lookup"><span data-stu-id="22902-210">The completed Todo component (*Components/Pages/Todo.razor* or *Pages/Todo.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Todo.razor)]

1. <span data-ttu-id="22902-211">Recompile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="22902-211">Rebuild and run the app.</span></span> <span data-ttu-id="22902-212">Agregue elementos de tarea pendiente para probar el nuevo código.</span><span class="sxs-lookup"><span data-stu-id="22902-212">Add todo items to test the new code.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="22902-213">Publicar e implementar la aplicación</span><span class="sxs-lookup"><span data-stu-id="22902-213">Publish and deploy the app</span></span>

<span data-ttu-id="22902-214">Para publicar la aplicación, consulte <xref:host-and-deploy/razor-components/index#publish-the-app>.</span><span class="sxs-lookup"><span data-stu-id="22902-214">To publish the app, see <xref:host-and-deploy/razor-components/index#publish-the-app>.</span></span>
