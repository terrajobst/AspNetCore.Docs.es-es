---
title: Compilación de su primera aplicación Razor Components
author: guardrex
description: Compile una aplicación Razor Components paso a paso y aprenda conceptos básicos de Razor Components.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/11/2019
uid: tutorials/first-razor-components-app
ms.openlocfilehash: 0c3dd2366581d73bad44e2911602e13c6c0daf9a
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159348"
---
# <a name="build-your-first-razor-components-app"></a><span data-ttu-id="aac1a-103">Compilación de su primera aplicación Razor Components</span><span class="sxs-lookup"><span data-stu-id="aac1a-103">Build your first Razor Components app</span></span>

<span data-ttu-id="aac1a-104">Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="aac1a-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="aac1a-105">En este tutorial se le muestra cómo compilar una aplicación con Razor Components. En él, además, se muestran conceptos básicos de Razor Components.</span><span class="sxs-lookup"><span data-stu-id="aac1a-105">This tutorial shows you how to build an app with Razor Components and demonstrates basic Razor Components concepts.</span></span> <span data-ttu-id="aac1a-106">Puede disfrutar este tutorial mediante un proyecto basado en Razor Components (admitido en .NET Core 3.0 o posterior) o mediante un proyecto basado en Blazor (admitido en una versión futura de .NET Core).</span><span class="sxs-lookup"><span data-stu-id="aac1a-106">You can enjoy this tutorial using either a Razor Components-based project (supported in .NET Core 3.0 or later) or using a Blazor-based project (supported in a future release of .NET Core).</span></span>

<span data-ttu-id="aac1a-107">Para una experiencia con Razor Components de ASP.NET Core (*se recomienda*):</span><span class="sxs-lookup"><span data-stu-id="aac1a-107">For an experience using ASP.NET Core Razor Components (*recommended*):</span></span>

* <span data-ttu-id="aac1a-108">Siga la guía en <xref:razor-components/get-started> para crear un proyecto basado en Razor Components.</span><span class="sxs-lookup"><span data-stu-id="aac1a-108">Follow the guidance in <xref:razor-components/get-started> to create a Razor Components-based project.</span></span>
* <span data-ttu-id="aac1a-109">Dé un nombre al proyecto `RazorComponents`.</span><span class="sxs-lookup"><span data-stu-id="aac1a-109">Name the project `RazorComponents`.</span></span>
* <span data-ttu-id="aac1a-110">Se crea una solución de varios proyectos a partir de la plantilla de Razor Components.</span><span class="sxs-lookup"><span data-stu-id="aac1a-110">A multi-project solution is created from the Razor Components template.</span></span> <span data-ttu-id="aac1a-111">El proyecto de Razor Components se genera como *RazorComponents.App*.</span><span class="sxs-lookup"><span data-stu-id="aac1a-111">The Razor Components project is generated as *RazorComponents.App*.</span></span>

<span data-ttu-id="aac1a-112">Para una experiencia con Blazor:</span><span class="sxs-lookup"><span data-stu-id="aac1a-112">For an experience using Blazor:</span></span>

* <span data-ttu-id="aac1a-113">Siga la guía en <xref:spa/blazor/get-started> para crear un proyecto basado en Blazor.</span><span class="sxs-lookup"><span data-stu-id="aac1a-113">Follow the guidance in <xref:spa/blazor/get-started> to create a Blazor-based project.</span></span>
* <span data-ttu-id="aac1a-114">Dé un nombre al proyecto `Blazor`.</span><span class="sxs-lookup"><span data-stu-id="aac1a-114">Name the project `Blazor`.</span></span>
* <span data-ttu-id="aac1a-115">Se crea una solución de un solo proyecto a partir de la plantilla de Blazor.</span><span class="sxs-lookup"><span data-stu-id="aac1a-115">A single-project solution is created from the Blazor template.</span></span>

<span data-ttu-id="aac1a-116">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="aac1a-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="aac1a-117">Para ver los requisitos previos, consulte los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="aac1a-117">See the following topics for prerequisites:</span></span>

## <a name="build-components"></a><span data-ttu-id="aac1a-118">Creación de componentes</span><span class="sxs-lookup"><span data-stu-id="aac1a-118">Build components</span></span>

1. <span data-ttu-id="aac1a-119">Vaya a cada una de tres páginas de la aplicación: Home (Inicio), Counter (Contador) y Fetch data (Recuperar datos).</span><span class="sxs-lookup"><span data-stu-id="aac1a-119">Browse to each of the app's three pages: Home, Counter, and Fetch data.</span></span> <span data-ttu-id="aac1a-120">Estas páginas se implementan mediante los archivos de Razor en la carpeta *Pages*: *Index.cshtml*, *Counter.cshtml* y *FetchData.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="aac1a-120">These pages are implemented by Razor files in the *Pages* folder: *Index.cshtml*, *Counter.cshtml*, and *FetchData.cshtml*.</span></span>

1. <span data-ttu-id="aac1a-121">En la página Contador, seleccione el botón **Click me** para aumentar el contador sin una actualización de página.</span><span class="sxs-lookup"><span data-stu-id="aac1a-121">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="aac1a-122">Aumentar un contador en una página web suele requerir la escritura de JavaScript, pero Razor Components proporciona una mejor manera de usar C#.</span><span class="sxs-lookup"><span data-stu-id="aac1a-122">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

1. <span data-ttu-id="aac1a-123">Examine la implementación del componente Counter en el archivo *Counter.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="aac1a-123">Examine the implementation of the Counter component in the *Counter.cshtml* file.</span></span>

   <span data-ttu-id="aac1a-124">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="aac1a-124">*Pages/Counter.cshtml*:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter1.cshtml)]

   <span data-ttu-id="aac1a-125">la interfaz de usuario del componente Counter se define mediante HTML.</span><span class="sxs-lookup"><span data-stu-id="aac1a-125">The UI of the Counter component is defined using HTML.</span></span> <span data-ttu-id="aac1a-126">La lógica de la representación dinámica (por ejemplo, bucles, instrucciones condicionales, expresiones) se agrega mediante una sintaxis de C# insertada denominada [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="aac1a-126">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="aac1a-127">El marcado HTML y la lógica de representación de C# se convierten en una clase de componente en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="aac1a-127">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="aac1a-128">El nombre de la clase de .NET generada coincide con el nombre del archivo.</span><span class="sxs-lookup"><span data-stu-id="aac1a-128">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="aac1a-129">Los miembros de la clase de componente se definen en un bloque `@functions`.</span><span class="sxs-lookup"><span data-stu-id="aac1a-129">Members of the component class are defined in a `@functions` block.</span></span> <span data-ttu-id="aac1a-130">En el bloque `@functions`, se especifica el estado del componente (propiedades, campos) y los métodos para el tratamiento de eventos o para definir otra lógica del componente.</span><span class="sxs-lookup"><span data-stu-id="aac1a-130">In the `@functions` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="aac1a-131">Estos miembros se utilizan como parte de la lógica de representación del componente y para el tratamiento de eventos.</span><span class="sxs-lookup"><span data-stu-id="aac1a-131">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="aac1a-132">Al seleccionarse el botón **Click me**:</span><span class="sxs-lookup"><span data-stu-id="aac1a-132">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="aac1a-133">Se llama al controlador `onclick` registrado del componente Counter (el método `IncrementCount`).</span><span class="sxs-lookup"><span data-stu-id="aac1a-133">The Counter component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="aac1a-134">El componente Counter regenera su árbol de representación.</span><span class="sxs-lookup"><span data-stu-id="aac1a-134">The Counter component regenerates its render tree.</span></span>
   * <span data-ttu-id="aac1a-135">El nuevo árbol de representación se compara con el anterior.</span><span class="sxs-lookup"><span data-stu-id="aac1a-135">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="aac1a-136">Únicamente se aplican modificaciones en Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="aac1a-136">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="aac1a-137">Se actualiza el recuento mostrado.</span><span class="sxs-lookup"><span data-stu-id="aac1a-137">The displayed count is updated.</span></span>

1. <span data-ttu-id="aac1a-138">Modifique la lógica de C# del componente Counter para hacer que el recuento se incremente en dos en lugar de uno.</span><span class="sxs-lookup"><span data-stu-id="aac1a-138">Modify the C# logic of the Counter component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter2.cshtml?highlight=14)]

1. <span data-ttu-id="aac1a-139">Recompile y ejecute la aplicación para ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="aac1a-139">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="aac1a-140">Seleccione el botón **Click me** y el contador se incrementará en dos.</span><span class="sxs-lookup"><span data-stu-id="aac1a-140">Select the **Click me** button, and the counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="aac1a-141">Uso de componentes</span><span class="sxs-lookup"><span data-stu-id="aac1a-141">Use components</span></span>

<span data-ttu-id="aac1a-142">Incluya un componente en otro componente mediante una sintaxis similar a HTML.</span><span class="sxs-lookup"><span data-stu-id="aac1a-142">Include a component into another component using an HTML-like syntax.</span></span>

1. <span data-ttu-id="aac1a-143">Agregue el componente Counter al componente Index (página principal) de la aplicación agregando a su vez un elemento `<Counter />` al componente Index.</span><span class="sxs-lookup"><span data-stu-id="aac1a-143">Add the Counter component to the app's Index (home page) component by adding a `<Counter />` element to the Index component.</span></span>

   <span data-ttu-id="aac1a-144">Si usa Blazor para esta experiencia, un componente Survey Prompt (elemento `<SurveyPrompt>`) está en el componente Index.</span><span class="sxs-lookup"><span data-stu-id="aac1a-144">If you're using Blazor for this experience, a Survey Prompt component (`<SurveyPrompt>` element) is in the Index component.</span></span> <span data-ttu-id="aac1a-145">Reemplace el elemento `<SurveyPrompt>` por el elemento `<Counter>`.</span><span class="sxs-lookup"><span data-stu-id="aac1a-145">Replace the `<SurveyPrompt>` element with the `<Counter>` element.</span></span>

   <span data-ttu-id="aac1a-146">*Páginas/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="aac1a-146">*Pages/Index.cshtml*:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Index.cshtml?highlight=7)]

1. <span data-ttu-id="aac1a-147">Recompile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="aac1a-147">Rebuild and run the app.</span></span> <span data-ttu-id="aac1a-148">La página principal tiene su propio contador.</span><span class="sxs-lookup"><span data-stu-id="aac1a-148">The home page has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="aac1a-149">Parámetros del componente</span><span class="sxs-lookup"><span data-stu-id="aac1a-149">Component parameters</span></span>

<span data-ttu-id="aac1a-150">Los componentes también pueden tener parámetros.</span><span class="sxs-lookup"><span data-stu-id="aac1a-150">Components can also have parameters.</span></span> <span data-ttu-id="aac1a-151">Los parámetros del componente se definen mediante propiedades privadas en la clase de componentes decorada con `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="aac1a-151">Component parameters are defined using non-public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="aac1a-152">Use atributos para especificar argumentos para un componente en el marcado.</span><span class="sxs-lookup"><span data-stu-id="aac1a-152">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="aac1a-153">Actualice el código de C# `@functions` del componente:</span><span class="sxs-lookup"><span data-stu-id="aac1a-153">Update the component's `@functions` C# code:</span></span>

   * <span data-ttu-id="aac1a-154">Agregue una propiedad `IncrementAmount` decorada con el atributo `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="aac1a-154">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="aac1a-155">Cambie el método `IncrementCount` para usar `IncrementAmount` al aumentar el valor de `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="aac1a-155">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="aac1a-156">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="aac1a-156">*Pages/Counter.cshtml*:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/Pages/Counter.cshtml?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="aac1a-157">Especifique un parámetro `IncrementAmount` en el elemento `<Counter>` del componente Home mediante un atributo.</span><span class="sxs-lookup"><span data-stu-id="aac1a-157">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="aac1a-158">Establezca el valor para incrementar el contador en diez.</span><span class="sxs-lookup"><span data-stu-id="aac1a-158">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="aac1a-159">*Páginas/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="aac1a-159">*Pages/Index.cshtml*:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/Pages/Index.cshtml?highlight=7)]

1. <span data-ttu-id="aac1a-160">Recargue la página.</span><span class="sxs-lookup"><span data-stu-id="aac1a-160">Reload the page.</span></span> <span data-ttu-id="aac1a-161">El contador de la página principal se incrementa en diez cada vez que se selecciona el botón **Click me**.</span><span class="sxs-lookup"><span data-stu-id="aac1a-161">The home page counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="aac1a-162">El contador de la página *Contador* se incrementa en uno.</span><span class="sxs-lookup"><span data-stu-id="aac1a-162">The counter on the *Counter* page increments by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="aac1a-163">Enrutamiento a los componentes</span><span class="sxs-lookup"><span data-stu-id="aac1a-163">Route to components</span></span>

<span data-ttu-id="aac1a-164">La directiva `@page` en la parte superior del archivo *Counter.cshtml* especifica que este componente es un punto de conexión de enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="aac1a-164">The `@page` directive at the top of the *Counter.cshtml* file specifies that this component is a routing endpoint.</span></span> <span data-ttu-id="aac1a-165">El componente Counter controla las solicitudes enviadas a `/Counter`.</span><span class="sxs-lookup"><span data-stu-id="aac1a-165">The Counter component handles requests sent to `/Counter`.</span></span> <span data-ttu-id="aac1a-166">Sin la directiva `@page`, el componente no controla las solicitudes de enrutado, pero el componente todavía puede usarse por otros componentes.</span><span class="sxs-lookup"><span data-stu-id="aac1a-166">Without the `@page` directive, the component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="aac1a-167">Inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="aac1a-167">Dependency injection</span></span>

<span data-ttu-id="aac1a-168">Los servicios registrados en el contenedor de servicios de la aplicación están disponibles para los componentes mediante una [inserción de dependencia (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="aac1a-168">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="aac1a-169">Inserte servicios en un componente mediante la directiva `@inject`.</span><span class="sxs-lookup"><span data-stu-id="aac1a-169">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="aac1a-170">Examine las directivas del componente FetchData (*Pages/FetchData.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="aac1a-170">Examine the directives of the FetchData component (*Pages/FetchData.cshtml*).</span></span> <span data-ttu-id="aac1a-171">La directiva `@inject` se utiliza para insertar la instancia del servicio `WeatherForecastService` en el componente:</span><span class="sxs-lookup"><span data-stu-id="aac1a-171">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.cshtml?highlight=3)]

<span data-ttu-id="aac1a-172">El servicio `WeatherForecastService` se registra como [singleton](xref:fundamentals/dependency-injection#service-lifetimes), de modo que una instancia del servicio está disponible en toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="aac1a-172">The `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span>

<span data-ttu-id="aac1a-173">El componente FetchData usa el servicio insertado, como `ForecastService`, para recuperar una matriz de objetos `WeatherForecast`:</span><span class="sxs-lookup"><span data-stu-id="aac1a-173">The FetchData component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData2.cshtml?highlight=6)]

<span data-ttu-id="aac1a-174">Se utiliza un bucle [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) para representar cada instancia de previsión como una fila en la tabla de datos meteorológicos:</span><span class="sxs-lookup"><span data-stu-id="aac1a-174">A [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData3.cshtml?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="aac1a-175">Creación de una lista de tareas pendientes</span><span class="sxs-lookup"><span data-stu-id="aac1a-175">Build a todo list</span></span>

<span data-ttu-id="aac1a-176">Agregue una nueva página a la aplicación que implemente una simple lista de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="aac1a-176">Add a new page to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="aac1a-177">Agregue un archivo vacío a la carpeta *Pages* llamada *Todo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="aac1a-177">Add an empty file to the *Pages* folder named *Todo.cshtml*.</span></span>

1. <span data-ttu-id="aac1a-178">Proporcione el marcado inicial para la página:</span><span class="sxs-lookup"><span data-stu-id="aac1a-178">Provide the initial markup for the page:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="aac1a-179">Agregue la página Todo (Tareas pendientes) a la barra de navegación.</span><span class="sxs-lookup"><span data-stu-id="aac1a-179">Add the Todo page to the navigation bar.</span></span>

   <span data-ttu-id="aac1a-180">El componente NavMenu (*Shared/NavMenu.csthml*) se usa en el diseño de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="aac1a-180">The NavMenu component (*Shared/NavMenu.csthml*) is used in the app's layout.</span></span> <span data-ttu-id="aac1a-181">Los diseños son componentes que le permiten impedir la duplicación de contenido en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="aac1a-181">Layouts are components that allow you to avoid duplication of content in the app.</span></span> <span data-ttu-id="aac1a-182">Para obtener más información, vea <xref:razor-components/layouts>.</span><span class="sxs-lookup"><span data-stu-id="aac1a-182">For more information, see <xref:razor-components/layouts>.</span></span>

   <span data-ttu-id="aac1a-183">Agregue un `<NavLink>` a la página Todo mediante la adición del siguiente marcado de elementos de lista debajo de los elementos de lista existentes en el archivo *Shared/NavMenu.csthml*:</span><span class="sxs-lookup"><span data-stu-id="aac1a-183">Add a `<NavLink>` for the Todo page by adding the following list item markup below the existing list items in the *Shared/NavMenu.csthml* file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="aac1a-184">Recompile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="aac1a-184">Rebuild and run the app.</span></span> <span data-ttu-id="aac1a-185">Visite la nueva página Todo para confirmar que el vínculo a la página Todo funciona.</span><span class="sxs-lookup"><span data-stu-id="aac1a-185">Visit the new Todo page to confirm that the link to the Todo page works.</span></span>

1. <span data-ttu-id="aac1a-186">Agregue un archivo *TodoItem.cs* a la raíz del proyecto para contener una clase que represente un elemento de la lista de tareas.</span><span class="sxs-lookup"><span data-stu-id="aac1a-186">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="aac1a-187">Use el siguiente código de C# para la clase `TodoItem`:</span><span class="sxs-lookup"><span data-stu-id="aac1a-187">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/TodoItem.cs)]

1. <span data-ttu-id="aac1a-188">Vuelva al componente Todo (*Todo.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="aac1a-188">Return to the Todo component (*Todo.cshtml*):</span></span>

   * <span data-ttu-id="aac1a-189">Agregue un campo a las tareas pendientes en un bloque `@functions`.</span><span class="sxs-lookup"><span data-stu-id="aac1a-189">Add a field for the todos in an `@functions` block.</span></span> <span data-ttu-id="aac1a-190">El componente Todo utiliza este campo para mantener el estado de la lista de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="aac1a-190">The Todo component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="aac1a-191">Agregue el marcado de la lista no ordenada y un bucle `foreach` para que cada elemento de la lista se represente en un elemento de la lista de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="aac1a-191">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData4.cshtml?highlight=5-10,12-14)]

1. <span data-ttu-id="aac1a-192">La aplicación requiere elementos de la interfaz de usuario para agregar tareas pendientes a la lista.</span><span class="sxs-lookup"><span data-stu-id="aac1a-192">The app requires UI elements for adding todos to the list.</span></span> <span data-ttu-id="aac1a-193">Agregue una entrada de texto y un botón debajo de la lista:</span><span class="sxs-lookup"><span data-stu-id="aac1a-193">Add a text input and a button below the list:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData5.cshtml?highlight=12-13)]

1. <span data-ttu-id="aac1a-194">Recompile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="aac1a-194">Rebuild and run the app.</span></span> <span data-ttu-id="aac1a-195">Cuando se selecciona el botón **Add todo** (Agregar tarea pendiente), no ocurre nada porque no hay ningún controlador de eventos conectado al botón.</span><span class="sxs-lookup"><span data-stu-id="aac1a-195">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="aac1a-196">Agregue un método `AddTodo` al componente Todo y regístrelo para hacer clic en los botones mediante el atributo `onclick`:</span><span class="sxs-lookup"><span data-stu-id="aac1a-196">Add an `AddTodo` method to the Todo component and register it for button clicks using the `onclick` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData6.cshtml?highlight=2,7-10)]

   <span data-ttu-id="aac1a-197">El método `AddTodo` de C# se llama cuando se selecciona el botón.</span><span class="sxs-lookup"><span data-stu-id="aac1a-197">The `AddTodo` C# method is called when the button is selected.</span></span>

1. <span data-ttu-id="aac1a-198">Para obtener el título del nuevo elemento de tarea pendiente, agregue un campo de cadena `newTodo` y enlácelo al valor de la entrada de texto mediante el atributo `bind`:</span><span class="sxs-lookup"><span data-stu-id="aac1a-198">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData7.cshtml?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo" />
   ```

1. <span data-ttu-id="aac1a-199">Actualice el método `AddTodo` para agregar el `TodoItem` con el título especificado a la lista.</span><span class="sxs-lookup"><span data-stu-id="aac1a-199">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="aac1a-200">Borre el valor de la entrada de texto mediante el establecimiento de `newTodo` en una cadena vacía:</span><span class="sxs-lookup"><span data-stu-id="aac1a-200">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData8.cshtml?highlight=19-26)]

1. <span data-ttu-id="aac1a-201">Recompile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="aac1a-201">Rebuild and run the app.</span></span> <span data-ttu-id="aac1a-202">Agregue algunas tareas pendientes a la lista de tareas pendientes para probar el nuevo código.</span><span class="sxs-lookup"><span data-stu-id="aac1a-202">Add some todos to the todo list to test the new code.</span></span>

1. <span data-ttu-id="aac1a-203">Se puede hacer que el texto de título de cada elemento de tarea pendiente sea editable y una casilla puede ayudar al usuario a realizar un seguimiento de los elementos completados.</span><span class="sxs-lookup"><span data-stu-id="aac1a-203">The title text for each todo item can be made editable and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="aac1a-204">Agregue una entrada de casilla a cada elemento de tarea pendiente y enlace su valor a la propiedad `IsDone`.</span><span class="sxs-lookup"><span data-stu-id="aac1a-204">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="aac1a-205">Cambie `@todo.Title` a un elemento `<input>` enlazado a `@todo.Title`:</span><span class="sxs-lookup"><span data-stu-id="aac1a-205">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData9.cshtml?highlight=5-6)]

1. <span data-ttu-id="aac1a-206">Para comprobar que estos valores están enlazados, actualice el encabezado `<h1>` para mostrar un recuento del número de elementos de la lista de tareas pendientes que no se han completado (`IsDone` es `false`).</span><span class="sxs-lookup"><span data-stu-id="aac1a-206">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="aac1a-207">El componente Todo completado (*Todo.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="aac1a-207">The completed Todo component (*Todo.cshtml*):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/Pages/Todo.cshtml)]

1. <span data-ttu-id="aac1a-208">Recompile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="aac1a-208">Rebuild and run the app.</span></span> <span data-ttu-id="aac1a-209">Agregue elementos de tarea pendiente para probar el nuevo código.</span><span class="sxs-lookup"><span data-stu-id="aac1a-209">Add todo items to test the new code.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="aac1a-210">Publicar e implementar la aplicación</span><span class="sxs-lookup"><span data-stu-id="aac1a-210">Publish and deploy the app</span></span>

<span data-ttu-id="aac1a-211">Para publicar la aplicación, consulte <xref:host-and-deploy/razor-components/index#publish-the-app>.</span><span class="sxs-lookup"><span data-stu-id="aac1a-211">To publish the app, see <xref:host-and-deploy/razor-components/index#publish-the-app>.</span></span>
