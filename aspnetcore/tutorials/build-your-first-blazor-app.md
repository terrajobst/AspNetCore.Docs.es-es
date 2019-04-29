---
title: Creación de la primera aplicación Blazor
author: guardrex
description: Cree una aplicación Blazor paso a paso.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: 310eb211f68076d6f52d6427940e07736d833e5b
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614754"
---
# <a name="build-your-first-blazor-app"></a><span data-ttu-id="bb31c-103">Creación de la primera aplicación Blazor</span><span class="sxs-lookup"><span data-stu-id="bb31c-103">Build your first Blazor app</span></span>

<span data-ttu-id="bb31c-104">Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="bb31c-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="bb31c-105">En este tutorial se muestra cómo crear una aplicación con Razor Components del lado servidor o con Blazor del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="bb31c-105">This tutorial shows you how to build an app with server-side Razor Components or client-side Blazor.</span></span>

<span data-ttu-id="bb31c-106">Para una experiencia con Razor Components de ASP.NET Core del lado servidor (*recomendado*):</span><span class="sxs-lookup"><span data-stu-id="bb31c-106">For an experience using server-side ASP.NET Core Razor Components (*recommended*):</span></span>

* <span data-ttu-id="bb31c-107">Siga la guía de *experiencia de Razor Components* de <xref:blazor/get-started#server-side-razor-components-experience> para crear un proyecto basado en Razor Components.</span><span class="sxs-lookup"><span data-stu-id="bb31c-107">Follow the *Razor Components experience* guidance in <xref:blazor/get-started#server-side-razor-components-experience> to create a Razor Components-based project.</span></span>
* <span data-ttu-id="bb31c-108">Dé un nombre al proyecto `RazorComponents`.</span><span class="sxs-lookup"><span data-stu-id="bb31c-108">Name the project `RazorComponents`.</span></span>

<span data-ttu-id="bb31c-109">Para una experiencia con Blazor:</span><span class="sxs-lookup"><span data-stu-id="bb31c-109">For an experience using Blazor:</span></span>

* <span data-ttu-id="bb31c-110">Siga la guía de *experiencia de Blazor* de <xref:blazor/get-started#client-side-blazor-experience> para crear un proyecto basado en Blazor.</span><span class="sxs-lookup"><span data-stu-id="bb31c-110">Follow the *Blazor experience* guidance in <xref:blazor/get-started#client-side-blazor-experience> to create a Blazor-based project.</span></span>
* <span data-ttu-id="bb31c-111">Dé un nombre al proyecto `Blazor`.</span><span class="sxs-lookup"><span data-stu-id="bb31c-111">Name the project `Blazor`.</span></span>

<span data-ttu-id="bb31c-112">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-blazor-app/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="bb31c-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-blazor-app/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="bb31c-113">Para ver los requisitos previos, consulte los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="bb31c-113">See the following topics for prerequisites:</span></span>

## <a name="build-components"></a><span data-ttu-id="bb31c-114">Creación de componentes</span><span class="sxs-lookup"><span data-stu-id="bb31c-114">Build components</span></span>

1. <span data-ttu-id="bb31c-115">Vaya a cada una de las tres páginas de la aplicación en la carpeta *Components/Pages* (*Pages* en Blazor): Home (Inicio), Counter (Contador) y Fetch data (Recuperar datos).</span><span class="sxs-lookup"><span data-stu-id="bb31c-115">Browse to each of the app's three pages in the *Components/Pages* folder (*Pages* in Blazor): Home, Counter, and Fetch data.</span></span> <span data-ttu-id="bb31c-116">Estas páginas se implementan mediante los archivos de componentes de Razor: *Index.razor*, *Counter.razor* y *FetchData.razor*</span><span class="sxs-lookup"><span data-stu-id="bb31c-116">These pages are implemented by Razor Component files: *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span> <span data-ttu-id="bb31c-117">(Blazor continúa usando la extensión de archivo *.cshtml*: *Index.cshtml*, *Counter.cshtml* y *FetchData.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="bb31c-117">(Blazor continues to use the *.cshtml* file extension: *Index.cshtml*, *Counter.cshtml*, and *FetchData.cshtml*.)</span></span>

1. <span data-ttu-id="bb31c-118">En la página Contador, seleccione el botón **Click me** para aumentar el contador sin una actualización de página.</span><span class="sxs-lookup"><span data-stu-id="bb31c-118">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="bb31c-119">Aumentar un contador en una página web suele requerir la escritura de JavaScript, pero Blazor proporciona una mejor manera de usar C#.</span><span class="sxs-lookup"><span data-stu-id="bb31c-119">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

1. <span data-ttu-id="bb31c-120">Examine la implementación del componente Counter en el archivo *Counter.razor*.</span><span class="sxs-lookup"><span data-stu-id="bb31c-120">Examine the implementation of the Counter component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="bb31c-121">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* en Blazor):</span><span class="sxs-lookup"><span data-stu-id="bb31c-121">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="bb31c-122">la interfaz de usuario del componente Counter se define mediante HTML.</span><span class="sxs-lookup"><span data-stu-id="bb31c-122">The UI of the Counter component is defined using HTML.</span></span> <span data-ttu-id="bb31c-123">La lógica de la representación dinámica (por ejemplo, bucles, instrucciones condicionales, expresiones) se agrega mediante una sintaxis de C# insertada denominada [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="bb31c-123">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="bb31c-124">El marcado HTML y la lógica de representación de C# se convierten en una clase de componente en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="bb31c-124">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="bb31c-125">El nombre de la clase de .NET generada coincide con el nombre del archivo.</span><span class="sxs-lookup"><span data-stu-id="bb31c-125">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="bb31c-126">Los miembros de la clase de componente se definen en un bloque `@functions`.</span><span class="sxs-lookup"><span data-stu-id="bb31c-126">Members of the component class are defined in a `@functions` block.</span></span> <span data-ttu-id="bb31c-127">En el bloque `@functions`, se especifica el estado del componente (propiedades, campos) y los métodos para el tratamiento de eventos o para definir otra lógica del componente.</span><span class="sxs-lookup"><span data-stu-id="bb31c-127">In the `@functions` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="bb31c-128">Estos miembros se utilizan como parte de la lógica de representación del componente y para el tratamiento de eventos.</span><span class="sxs-lookup"><span data-stu-id="bb31c-128">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="bb31c-129">Al seleccionarse el botón **Click me**:</span><span class="sxs-lookup"><span data-stu-id="bb31c-129">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="bb31c-130">Se llama al controlador `onclick` registrado del componente Counter (el método `IncrementCount`).</span><span class="sxs-lookup"><span data-stu-id="bb31c-130">The Counter component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="bb31c-131">El componente Counter regenera su árbol de representación.</span><span class="sxs-lookup"><span data-stu-id="bb31c-131">The Counter component regenerates its render tree.</span></span>
   * <span data-ttu-id="bb31c-132">El nuevo árbol de representación se compara con el anterior.</span><span class="sxs-lookup"><span data-stu-id="bb31c-132">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="bb31c-133">Únicamente se aplican modificaciones en Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="bb31c-133">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="bb31c-134">Se actualiza el recuento mostrado.</span><span class="sxs-lookup"><span data-stu-id="bb31c-134">The displayed count is updated.</span></span>

1. <span data-ttu-id="bb31c-135">Modifique la lógica de C# del componente Counter para hacer que el recuento se incremente en dos en lugar de uno.</span><span class="sxs-lookup"><span data-stu-id="bb31c-135">Modify the C# logic of the Counter component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="bb31c-136">Recompile y ejecute la aplicación para ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="bb31c-136">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="bb31c-137">Seleccione el botón **Click me** y el contador se incrementará en dos.</span><span class="sxs-lookup"><span data-stu-id="bb31c-137">Select the **Click me** button, and the counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="bb31c-138">Uso de componentes</span><span class="sxs-lookup"><span data-stu-id="bb31c-138">Use components</span></span>

<span data-ttu-id="bb31c-139">Incluya un componente en otro componente mediante una sintaxis similar a HTML.</span><span class="sxs-lookup"><span data-stu-id="bb31c-139">Include a component into another component using an HTML-like syntax.</span></span>

1. <span data-ttu-id="bb31c-140">Agregue el componente Counter al componente Index (Home) de la aplicación, agregando a su vez un elemento `<Counter />` al componente Index.</span><span class="sxs-lookup"><span data-stu-id="bb31c-140">Add the Counter component to the app's Index (Home) component by adding a `<Counter />` element to the Index component.</span></span>

   <span data-ttu-id="bb31c-141">Si usa Blazor para esta experiencia, un componente Survey Prompt (elemento `<SurveyPrompt>`) está en el componente Index.</span><span class="sxs-lookup"><span data-stu-id="bb31c-141">If you're using Blazor for this experience, a Survey Prompt component (`<SurveyPrompt>` element) is in the Index component.</span></span> <span data-ttu-id="bb31c-142">Reemplace el elemento `<SurveyPrompt>` por el elemento `<Counter>`.</span><span class="sxs-lookup"><span data-stu-id="bb31c-142">Replace the `<SurveyPrompt>` element with the `<Counter>` element.</span></span>

   <span data-ttu-id="bb31c-143">*Components/Pages/Index.razor* (*Pages/Index.cshtml* en Blazor):</span><span class="sxs-lookup"><span data-stu-id="bb31c-143">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index.razor?highlight=7)]

1. <span data-ttu-id="bb31c-144">Recompile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bb31c-144">Rebuild and run the app.</span></span> <span data-ttu-id="bb31c-145">La página Home tiene su propio contador.</span><span class="sxs-lookup"><span data-stu-id="bb31c-145">The Home page has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="bb31c-146">Parámetros del componente</span><span class="sxs-lookup"><span data-stu-id="bb31c-146">Component parameters</span></span>

<span data-ttu-id="bb31c-147">Los componentes también pueden tener parámetros.</span><span class="sxs-lookup"><span data-stu-id="bb31c-147">Components can also have parameters.</span></span> <span data-ttu-id="bb31c-148">Los parámetros del componente se definen mediante propiedades privadas en la clase de componentes decorada con `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="bb31c-148">Component parameters are defined using non-public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="bb31c-149">Use atributos para especificar argumentos para un componente en el marcado.</span><span class="sxs-lookup"><span data-stu-id="bb31c-149">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="bb31c-150">Actualice el código de C# `@functions` del componente:</span><span class="sxs-lookup"><span data-stu-id="bb31c-150">Update the component's `@functions` C# code:</span></span>

   * <span data-ttu-id="bb31c-151">Agregue una propiedad `IncrementAmount` decorada con el atributo `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="bb31c-151">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="bb31c-152">Cambie el método `IncrementCount` para usar `IncrementAmount` al aumentar el valor de `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="bb31c-152">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="bb31c-153">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* en Blazor):</span><span class="sxs-lookup"><span data-stu-id="bb31c-153">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples/3.x/RazorComponents/Components/Pages/Counter.razor?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="bb31c-154">Especifique un parámetro `IncrementAmount` en el elemento `<Counter>` del componente Home mediante un atributo.</span><span class="sxs-lookup"><span data-stu-id="bb31c-154">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="bb31c-155">Establezca el valor para incrementar el contador en diez.</span><span class="sxs-lookup"><span data-stu-id="bb31c-155">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="bb31c-156">*Components/Pages/Index.razor* (*Pages/Index.cshtml* en Blazor):</span><span class="sxs-lookup"><span data-stu-id="bb31c-156">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples/3.x/RazorComponents/Components/Pages/Index.razor?highlight=7)]

1. <span data-ttu-id="bb31c-157">Vuelva a cargar la página Home.</span><span class="sxs-lookup"><span data-stu-id="bb31c-157">Reload the Home page.</span></span> <span data-ttu-id="bb31c-158">El contador se incrementa en diez cada vez que se selecciona el botón **Click me**.</span><span class="sxs-lookup"><span data-stu-id="bb31c-158">The counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="bb31c-159">El contador de la página Counter se incrementa en uno.</span><span class="sxs-lookup"><span data-stu-id="bb31c-159">The counter on the Counter page increments by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="bb31c-160">Enrutamiento a los componentes</span><span class="sxs-lookup"><span data-stu-id="bb31c-160">Route to components</span></span>

<span data-ttu-id="bb31c-161">La directiva `@page` en la parte superior del archivo *Counter.razor* especifica que este componente es un punto de conexión de enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="bb31c-161">The `@page` directive at the top of the *Counter.razor* file specifies that this component is a routing endpoint.</span></span> <span data-ttu-id="bb31c-162">El componente Counter controla las solicitudes enviadas a `/Counter`.</span><span class="sxs-lookup"><span data-stu-id="bb31c-162">The Counter component handles requests sent to `/Counter`.</span></span> <span data-ttu-id="bb31c-163">Sin la directiva `@page`, el componente no controla las solicitudes de enrutado, pero el componente todavía puede usarse por otros componentes.</span><span class="sxs-lookup"><span data-stu-id="bb31c-163">Without the `@page` directive, the component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="bb31c-164">Inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="bb31c-164">Dependency injection</span></span>

<span data-ttu-id="bb31c-165">Los servicios registrados en el contenedor de servicios de la aplicación están disponibles para los componentes mediante una [inserción de dependencia (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="bb31c-165">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="bb31c-166">Inserte servicios en un componente mediante la directiva `@inject`.</span><span class="sxs-lookup"><span data-stu-id="bb31c-166">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="bb31c-167">Examine las directivas del componente FetchData de la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="bb31c-167">Examine the directives of the FetchData component in the sample app.</span></span>

<span data-ttu-id="bb31c-168">En la aplicación de ejemplo Razor Components, el servicio `WeatherForecastService` se registra como [singleton](xref:fundamentals/dependency-injection#service-lifetimes), de modo que una instancia del servicio está disponible en toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bb31c-168">In the Razor Components sample app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span> <span data-ttu-id="bb31c-169">La directiva `@inject` se usa para insertar la instancia del servicio `WeatherForecastService` en el componente.</span><span class="sxs-lookup"><span data-stu-id="bb31c-169">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component.</span></span>

<span data-ttu-id="bb31c-170">*Components/Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="bb31c-170">*Components/Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="bb31c-171">El componente FetchData usa el servicio insertado, como `ForecastService`, para recuperar una matriz de objetos `WeatherForecast`:</span><span class="sxs-lookup"><span data-stu-id="bb31c-171">The FetchData component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

<span data-ttu-id="bb31c-172">En la versión de Blazor de la aplicación de ejemplo, se inyecta `HttpClient` para obtener los datos de la previsión meteorológica del archivo *weather.json* de la carpeta *wwwroot/ejemplo-data*:</span><span class="sxs-lookup"><span data-stu-id="bb31c-172">In the Blazor version of the sample app, `HttpClient` is injected to obtain weather forecast data from the *weather.json* file in the *wwwroot/sample-data* folder:</span></span>

<span data-ttu-id="bb31c-173">*Pages/FetchData.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="bb31c-173">*Pages/FetchData.cshtml*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.cshtml?highlight=7)]

<span data-ttu-id="bb31c-174">En ambas aplicaciones de ejemplo se usa un bucle [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) para representar cada instancia de previsión como una fila en la tabla de datos meteorológicos:</span><span class="sxs-lookup"><span data-stu-id="bb31c-174">In both sample apps, a [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="bb31c-175">Creación de una lista de tareas pendientes</span><span class="sxs-lookup"><span data-stu-id="bb31c-175">Build a todo list</span></span>

<span data-ttu-id="bb31c-176">Agregue un nuevo componente a la aplicación que implemente una simple lista de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="bb31c-176">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="bb31c-177">Agregue un archivo vacío a la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bb31c-177">Add an empty file to the sample app:</span></span>

   * <span data-ttu-id="bb31c-178">Para la experiencia de Razor Components, agregue un archivo *Todo.razor* a la carpeta *Components/Pages*.</span><span class="sxs-lookup"><span data-stu-id="bb31c-178">For the Razor Components experience, add a *Todo.razor* file to the *Components/Pages* folder.</span></span>
   * <span data-ttu-id="bb31c-179">Para la experiencia de Blazor, agregue un archivo *Todo.cshtml* a la carpeta *Pages*.</span><span class="sxs-lookup"><span data-stu-id="bb31c-179">For the Blazor experience, add a *Todo.cshtml* file to the *Pages* folder.</span></span>

1. <span data-ttu-id="bb31c-180">Proporcione el marcado inicial para el componente:</span><span class="sxs-lookup"><span data-stu-id="bb31c-180">Provide the initial markup for the component:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="bb31c-181">Agregue el componente Todo a la barra de navegación.</span><span class="sxs-lookup"><span data-stu-id="bb31c-181">Add the Todo component to the navigation bar.</span></span>

   <span data-ttu-id="bb31c-182">El componente NavMenu (*Components/Shared/NavMenu.razor* o *Shared/NavMenu.cshtml* en Blazor) se usa en el diseño de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bb31c-182">The NavMenu component (*Components/Shared/NavMenu.razor* or *Shared/NavMenu.cshtml* in Blazor) is used in the app's layout.</span></span> <span data-ttu-id="bb31c-183">Los diseños son componentes que le permiten impedir la duplicación de contenido en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bb31c-183">Layouts are components that allow you to avoid duplication of content in the app.</span></span> <span data-ttu-id="bb31c-184">Para obtener más información, vea <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="bb31c-184">For more information, see <xref:blazor/layouts>.</span></span>

   <span data-ttu-id="bb31c-185">Agregue un vínculo `<NavLink>` al componente Todo mediante la incorporación del siguiente marcado de elementos de lista debajo de los elementos de lista existentes en el archivo *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* en Blazor):</span><span class="sxs-lookup"><span data-stu-id="bb31c-185">Add a `<NavLink>` for the Todo component by adding the following list item markup below the existing list items in the *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* in Blazor) file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="bb31c-186">Recompile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bb31c-186">Rebuild and run the app.</span></span> <span data-ttu-id="bb31c-187">Visite la nueva página Todo para confirmar que el vínculo al componente Todo funcione.</span><span class="sxs-lookup"><span data-stu-id="bb31c-187">Visit the new Todo page to confirm that the link to the Todo component works.</span></span>

1. <span data-ttu-id="bb31c-188">Agregue un archivo *TodoItem.cs* a la raíz del proyecto para contener una clase que represente un elemento de la lista de tareas.</span><span class="sxs-lookup"><span data-stu-id="bb31c-188">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="bb31c-189">Use el siguiente código de C# para la clase `TodoItem`:</span><span class="sxs-lookup"><span data-stu-id="bb31c-189">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples/3.x/RazorComponents/TodoItem.cs)]

1. <span data-ttu-id="bb31c-190">Vuelva al componente Todo (*Components/Pages/Todo.razor* o *Pages/Todo.cshtml* en Blazor):</span><span class="sxs-lookup"><span data-stu-id="bb31c-190">Return to the Todo component (*Components/Pages/Todo.razor* or *Pages/Todo.cshtml* in Blazor):</span></span>

   * <span data-ttu-id="bb31c-191">Agregue un campo a las tareas pendientes en un bloque `@functions`.</span><span class="sxs-lookup"><span data-stu-id="bb31c-191">Add a field for the todos in an `@functions` block.</span></span> <span data-ttu-id="bb31c-192">El componente Todo utiliza este campo para mantener el estado de la lista de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="bb31c-192">The Todo component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="bb31c-193">Agregue el marcado de la lista no ordenada y un bucle `foreach` para que cada elemento de la lista se represente en un elemento de la lista de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="bb31c-193">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="bb31c-194">La aplicación requiere elementos de la interfaz de usuario para agregar tareas pendientes a la lista.</span><span class="sxs-lookup"><span data-stu-id="bb31c-194">The app requires UI elements for adding todos to the list.</span></span> <span data-ttu-id="bb31c-195">Agregue una entrada de texto y un botón debajo de la lista:</span><span class="sxs-lookup"><span data-stu-id="bb31c-195">Add a text input and a button below the list:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="bb31c-196">Recompile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bb31c-196">Rebuild and run the app.</span></span> <span data-ttu-id="bb31c-197">Cuando se selecciona el botón **Add todo** (Agregar tarea pendiente), no ocurre nada porque no hay ningún controlador de eventos conectado al botón.</span><span class="sxs-lookup"><span data-stu-id="bb31c-197">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="bb31c-198">Agregue un método `AddTodo` al componente Todo y regístrelo para hacer clic en los botones mediante el atributo `onclick`:</span><span class="sxs-lookup"><span data-stu-id="bb31c-198">Add an `AddTodo` method to the Todo component and register it for button clicks using the `onclick` attribute:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

   <span data-ttu-id="bb31c-199">El método `AddTodo` de C# se llama cuando se selecciona el botón.</span><span class="sxs-lookup"><span data-stu-id="bb31c-199">The `AddTodo` C# method is called when the button is selected.</span></span>

1. <span data-ttu-id="bb31c-200">Para obtener el título del nuevo elemento de tarea pendiente, agregue un campo de cadena `newTodo` y enlácelo al valor de la entrada de texto mediante el atributo `bind`:</span><span class="sxs-lookup"><span data-stu-id="bb31c-200">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo">
   ```

1. <span data-ttu-id="bb31c-201">Actualice el método `AddTodo` para agregar el `TodoItem` con el título especificado a la lista.</span><span class="sxs-lookup"><span data-stu-id="bb31c-201">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="bb31c-202">Borre el valor de la entrada de texto mediante el establecimiento de `newTodo` en una cadena vacía:</span><span class="sxs-lookup"><span data-stu-id="bb31c-202">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="bb31c-203">Recompile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bb31c-203">Rebuild and run the app.</span></span> <span data-ttu-id="bb31c-204">Agregue algunas tareas pendientes a la lista de tareas pendientes para probar el nuevo código.</span><span class="sxs-lookup"><span data-stu-id="bb31c-204">Add some todos to the todo list to test the new code.</span></span>

1. <span data-ttu-id="bb31c-205">Se puede hacer que el texto de título de cada elemento de tarea pendiente sea editable y una casilla puede ayudar al usuario a realizar un seguimiento de los elementos completados.</span><span class="sxs-lookup"><span data-stu-id="bb31c-205">The title text for each todo item can be made editable and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="bb31c-206">Agregue una entrada de casilla a cada elemento de tarea pendiente y enlace su valor a la propiedad `IsDone`.</span><span class="sxs-lookup"><span data-stu-id="bb31c-206">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="bb31c-207">Cambie `@todo.Title` a un elemento `<input>` enlazado a `@todo.Title`:</span><span class="sxs-lookup"><span data-stu-id="bb31c-207">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="bb31c-208">Para comprobar que estos valores están enlazados, actualice el encabezado `<h1>` para mostrar un recuento del número de elementos de la lista de tareas pendientes que no se han completado (`IsDone` es `false`).</span><span class="sxs-lookup"><span data-stu-id="bb31c-208">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="bb31c-209">El componente Todo completado (*Components/Pages/Todo.razor* o *Pages/Todo.cshtml* en Blazor):</span><span class="sxs-lookup"><span data-stu-id="bb31c-209">The completed Todo component (*Components/Pages/Todo.razor* or *Pages/Todo.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples/3.x/RazorComponents/Components/Pages/Todo.razor)]

1. <span data-ttu-id="bb31c-210">Recompile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bb31c-210">Rebuild and run the app.</span></span> <span data-ttu-id="bb31c-211">Agregue elementos de tarea pendiente para probar el nuevo código.</span><span class="sxs-lookup"><span data-stu-id="bb31c-211">Add todo items to test the new code.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="bb31c-212">Publicar e implementar la aplicación</span><span class="sxs-lookup"><span data-stu-id="bb31c-212">Publish and deploy the app</span></span>

<span data-ttu-id="bb31c-213">Para publicar la aplicación, consulte <xref:host-and-deploy/blazor/index>.</span><span class="sxs-lookup"><span data-stu-id="bb31c-213">To publish the app, see <xref:host-and-deploy/blazor/index>.</span></span>
