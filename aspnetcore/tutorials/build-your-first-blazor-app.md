---
title: Creación de la primera aplicación Blazor
author: guardrex
description: Cree una aplicación Blazor paso a paso y aprenda rápidamente las características básicas de la plataforma de Razor Components.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: 99396e200eb121524abccc20a7d461062c94ecc7
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668029"
---
# <a name="build-your-first-blazor-app"></a><span data-ttu-id="8e345-103">Creación de la primera aplicación Blazor</span><span class="sxs-lookup"><span data-stu-id="8e345-103">Build your first Blazor app</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="8e345-104">En este tutorial, va a crear una aplicación Blazor paso a paso y aprender rápidamente las características básicas de la plataforma de Razor Components.</span><span class="sxs-lookup"><span data-stu-id="8e345-104">In this tutorial, you build a Blazor app step-by-step and quickly learn the basic features of the Razor Components framework.</span></span>

<span data-ttu-id="8e345-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-blazor-app/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="8e345-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-blazor-app/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="8e345-106">Consulte el tema [Introducción](xref:razor-components/get-started) para ver los requisitos previos.</span><span class="sxs-lookup"><span data-stu-id="8e345-106">See the [Get started](xref:razor-components/get-started) topic for prerequisites.</span></span>

<span data-ttu-id="8e345-107">Para crear el proyecto en Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="8e345-107">To create the project in Visual Studio:</span></span>

1. <span data-ttu-id="8e345-108">Seleccione **Archivo** > **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="8e345-108">Select **File** > **New** > **Project**.</span></span> <span data-ttu-id="8e345-109">Seleccione **Web** > **Aplicación web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="8e345-109">Select **Web** > **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="8e345-110">Llame al proyecto "BlazorApp1" en el campo **Nombre**.</span><span class="sxs-lookup"><span data-stu-id="8e345-110">Name the project "BlazorApp1" in the **Name** field.</span></span> <span data-ttu-id="8e345-111">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="8e345-111">Select **OK**.</span></span>

    ![Nuevo proyecto de ASP.NET Core](build-your-first-blazor-app/_static/new-aspnet-core-project.png)

1. <span data-ttu-id="8e345-113">Se muestra el cuadro de diálogo **Nueva aplicación web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="8e345-113">The **New ASP.NET Core Web Application** dialog appears.</span></span> <span data-ttu-id="8e345-114">Asegúrese de que **.NET Core** está seleccionado en la parte superior.</span><span class="sxs-lookup"><span data-stu-id="8e345-114">Make sure **.NET Core** is selected at the top.</span></span> <span data-ttu-id="8e345-115">Seleccione **ASP.NET Core 2.1**.</span><span class="sxs-lookup"><span data-stu-id="8e345-115">Select **ASP.NET Core 2.1**.</span></span> <span data-ttu-id="8e345-116">Elija la plantilla **Blazor** y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="8e345-116">Choose the **Blazor** template and select **OK**.</span></span>

    ![Nuevo cuadro de diálogo de la aplicación Blazor](build-your-first-blazor-app/_static/new-blazor-app-dialog.png)

1. <span data-ttu-id="8e345-118">Una vez creado el proyecto, presione **Ctrl-F5** para ejecutar la aplicación *sin el depurador*.</span><span class="sxs-lookup"><span data-stu-id="8e345-118">Once the project is created, press **Ctrl-F5** to run the app *without the debugger*.</span></span> <span data-ttu-id="8e345-119">En este momento no se admite la ejecución con el depurador (**F5**).</span><span class="sxs-lookup"><span data-stu-id="8e345-119">Running with the debugger (**F5**) isn't supported at this time.</span></span>

> [!NOTE]
> <span data-ttu-id="8e345-120">Si no usa Visual Studio, cree la aplicación de Blazor en un símbolo del sistema en Windows, macOS o Linux:</span><span class="sxs-lookup"><span data-stu-id="8e345-120">If not using Visual Studio, create the Blazor app at a command prompt on Windows, macOS, or Linux:</span></span>
>
> ```console
> dotnet new --install "Microsoft.AspNetCore.Blazor.Templates"
> dotnet new blazor -o BlazorApp1
> cd BlazorApp1
> dotnet run
> ```
>
> <span data-ttu-id="8e345-121">Vaya la aplicación con la dirección y el puerto del host local proporcionados en la salida de la ventana de la consola después de que se ejecute `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="8e345-121">Navigate to the app using the localhost address and port provided in the console window output after `dotnet run` is executed.</span></span> <span data-ttu-id="8e345-122">Use **Ctrl-C** en la ventana de la consola para cerrar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8e345-122">Use **Ctrl-C** in the console window to shutdown the app.</span></span>

<span data-ttu-id="8e345-123">La aplicación Blazor se ejecuta en el explorador:</span><span class="sxs-lookup"><span data-stu-id="8e345-123">The Blazor app runs in the browser:</span></span>

![Página Home (Inicio) de la aplicación Blazor](https://user-images.githubusercontent.com/1874516/39509497-5515c3ea-4d9b-11e8-887f-019ea4fdb3ee.png)

## <a name="build-components"></a><span data-ttu-id="8e345-125">Creación de componentes</span><span class="sxs-lookup"><span data-stu-id="8e345-125">Build components</span></span>

1. <span data-ttu-id="8e345-126">Vaya a cada una de tres páginas de la aplicación: Home (Inicio), Counter (Contador) y Fetch data (Recuperar datos).</span><span class="sxs-lookup"><span data-stu-id="8e345-126">Browse to each of the app's three pages: Home, Counter, and Fetch data.</span></span>

    <span data-ttu-id="8e345-127">Estas tres páginas se implementan mediante los tres archivos de Razor en la carpeta *Pages*: *Index.cshtml*, *Counter.cshtml* y *FetchData.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8e345-127">These three pages are implemented by the three Razor files in the *Pages* folder: *Index.cshtml*, *Counter.cshtml*, and *FetchData.cshtml*.</span></span> <span data-ttu-id="8e345-128">Cada uno de estos archivos implementa un componente que se compila y ejecuta en el lado cliente en el navegador.</span><span class="sxs-lookup"><span data-stu-id="8e345-128">Each of these files implements a component that's compiled and executed client-side in the browser.</span></span>

1. <span data-ttu-id="8e345-129">Seleccione el botón en la página Counter (Contador).</span><span class="sxs-lookup"><span data-stu-id="8e345-129">Select the button on the Counter page.</span></span>

    ![Página Home (Inicio) de la aplicación Blazor](https://user-images.githubusercontent.com/1874516/39509525-6e367c66-4d9b-11e8-9978-e52a9750c34b.png)

    <span data-ttu-id="8e345-131">Cada vez que se selecciona el botón, el contador se incrementa sin una actualización de la página.</span><span class="sxs-lookup"><span data-stu-id="8e345-131">Each time the button is selected, the counter is incremented without a page refresh.</span></span> <span data-ttu-id="8e345-132">Normalmente, este tipo de comportamiento del lado cliente se controla en JavaScript; pero aquí se implementa en C# y .NET por el componente `Counter`.</span><span class="sxs-lookup"><span data-stu-id="8e345-132">Normally, this kind of client-side behavior is handled in JavaScript; but here, it's implemented in C# and .NET by the `Counter` component.</span></span>

1. <span data-ttu-id="8e345-133">Eche un vistazo a la implementación del componente `Counter` en el archivo *Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8e345-133">Take a look at the implementation of the `Counter` component in the *Counter.cshtml* file:</span></span>

    ```cshtml
    @page "/counter"

    <h1>Counter</h1>

    <p>Current count: @currentCount</p>

    <button class="btn btn-primary" onclick="@IncrementCount">Click me</button>

    @functions {
        int currentCount = 0;

        void IncrementCount()
        {
            currentCount++;
        }
    }
    ```

    <span data-ttu-id="8e345-134">La interfaz de usuario del componente `Counter` se define mediante código HTML normal.</span><span class="sxs-lookup"><span data-stu-id="8e345-134">The UI for the `Counter` component is defined using normal HTML.</span></span> <span data-ttu-id="8e345-135">La lógica de la representación dinámica (por ejemplo, bucles, instrucciones condicionales, expresiones) se agrega mediante una sintaxis de C# insertada denominada [Razor](https://docs.microsoft.com/aspnet/core/mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="8e345-135">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](https://docs.microsoft.com/aspnet/core/mvc/views/razor).</span></span> <span data-ttu-id="8e345-136">El marcado HTML y la lógica de representación de C# se convierten en una clase de componente en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="8e345-136">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="8e345-137">El nombre de la clase de .NET generada coincide con el nombre del archivo.</span><span class="sxs-lookup"><span data-stu-id="8e345-137">The name of the generated .NET class matches the name of the file.</span></span>

    <span data-ttu-id="8e345-138">Los miembros de la clase de componente se definen en un bloque `@functions`.</span><span class="sxs-lookup"><span data-stu-id="8e345-138">Members of the component class are defined in a `@functions` block.</span></span> <span data-ttu-id="8e345-139">En el bloque `@functions`, se puede especificar el estado del componente (propiedades, campos) y los métodos para el tratamiento de eventos o para definir otra lógica del componente.</span><span class="sxs-lookup"><span data-stu-id="8e345-139">In the `@functions` block, component state (properties, fields) and methods can be specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="8e345-140">Estos miembros se pueden utilizar como parte de la lógica de representación del componente y para el tratamiento de eventos.</span><span class="sxs-lookup"><span data-stu-id="8e345-140">These members can then be used as part of the component's rendering logic and for handling events.</span></span>

    <span data-ttu-id="8e345-141">Cuando se selecciona el botón, se llama al controlador `onclick` registrado del componente `Counter` (el método `IncrementCount`) y el componente `Counter` regenera su árbol de representación.</span><span class="sxs-lookup"><span data-stu-id="8e345-141">When the button is selected, the `Counter` component's registered `onclick` handler is called (the `IncrementCount` method) and the `Counter` component regenerates its render tree.</span></span> <span data-ttu-id="8e345-142">Blazor compara el nuevo árbol de representación con el anterior y aplica las modificaciones necesarias al explorador de Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="8e345-142">Blazor compares the new render tree against the previous one and applies any modifications to the browser Document Object Model (DOM).</span></span> <span data-ttu-id="8e345-143">Se actualiza el recuento mostrado.</span><span class="sxs-lookup"><span data-stu-id="8e345-143">The displayed count is updated.</span></span>

1. <span data-ttu-id="8e345-144">Actualice el marcado para el componente `Counter` para que el encabezado de nivel superior sea más  *interesante*.</span><span class="sxs-lookup"><span data-stu-id="8e345-144">Update the markup for the `Counter` component to make the top-level header more *exciting*.</span></span>

    ```cshtml
    @page "/counter"
    <h1><em>Counter!!</em></h1>
    ```

1. <span data-ttu-id="8e345-145">También modifique la lógica de C# del componente `Counter` para hacer que el recuento se incremente en dos en lugar de uno.</span><span class="sxs-lookup"><span data-stu-id="8e345-145">Also modify the C# logic of the `Counter` component to make the count increment by two instead of one.</span></span>

    ```cshtml
    @functions {
        int currentCount = 0;

        void IncrementCount()
        {
            currentCount += 2;
        }
    }
    ```

1. <span data-ttu-id="8e345-146">Actualice la página del contador en el explorador para ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="8e345-146">Refresh the counter page in the browser to see the changes.</span></span>

    ![Contador interesante](https://user-images.githubusercontent.com/1874516/39509668-e8949a92-4d9b-11e8-91e9-b6a494695d92.png)

## <a name="use-components"></a><span data-ttu-id="8e345-148">Uso de componentes</span><span class="sxs-lookup"><span data-stu-id="8e345-148">Use components</span></span>

<span data-ttu-id="8e345-149">Después de definir un componente, este se puede utilizar para implementar otros componentes.</span><span class="sxs-lookup"><span data-stu-id="8e345-149">After a component is defined, the component can be used to implement other components.</span></span> <span data-ttu-id="8e345-150">El marcado para utilizar un componente se parece a una etiqueta HTML en la que el nombre de la etiqueta es el tipo de componente.</span><span class="sxs-lookup"><span data-stu-id="8e345-150">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

1. <span data-ttu-id="8e345-151">Agregue un componente `Counter` a la página Home (Inicio) de la aplicación (*Index.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="8e345-151">Add a `Counter` component to the Home page of the app (*Index.cshtml*).</span></span>

    ```cshtml
    @page "/"

    <h1>Hello, world!</h1>

    Welcome to your new app.

    <SurveyPrompt Title="How is Blazor working for you?" />

    <Counter />
    ```

1. <span data-ttu-id="8e345-152">Actualice la página Home (Inicio) en el explorador.</span><span class="sxs-lookup"><span data-stu-id="8e345-152">Refresh the home page in the browser.</span></span> <span data-ttu-id="8e345-153">Observe la instancia independiente del componente `Counter` en la página Home (Inicio).</span><span class="sxs-lookup"><span data-stu-id="8e345-153">Note the separate instance of the `Counter` component on the Home page.</span></span>

    ![Página Home (Inicio) de Blazor con el contador](https://user-images.githubusercontent.com/1874516/39509718-224483f6-4d9c-11e8-9030-b4c7228d669d.png)

## <a name="component-parameters"></a><span data-ttu-id="8e345-155">Parámetros del componente</span><span class="sxs-lookup"><span data-stu-id="8e345-155">Component parameters</span></span>

<span data-ttu-id="8e345-156">Los componentes también pueden tener parámetros, que se definen mediante propiedades privadas en la clase de componentes decorada con `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="8e345-156">Components can also have parameters, which are defined using private properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="8e345-157">Use atributos para especificar argumentos para un componente en el marcado.</span><span class="sxs-lookup"><span data-stu-id="8e345-157">Use attributes to specify arguments for a component in markup.</span></span> 

1. <span data-ttu-id="8e345-158">Actualice el componente `Counter` para que tenga un parámetro `IncrementAmount` cuyo valor predeterminado es 1.</span><span class="sxs-lookup"><span data-stu-id="8e345-158">Update the `Counter` component to have an `IncrementAmount` parameter that defaults to 1.</span></span>

    ```cshtml
    @functions {
        int currentCount = 0;

        [Parameter]
        private int IncrementAmount { get; set; } = 1;

        void IncrementCount()
        {
            currentCount += IncrementAmount;
        }
    }
    ```

    > [!NOTE]
    > <span data-ttu-id="8e345-159">Desde Visual Studio, puede agregar rápidamente un parámetro de componente mediante el fragmento `para`.</span><span class="sxs-lookup"><span data-stu-id="8e345-159">From Visual Studio, you can quickly add a component parameter by using the `para` snippet.</span></span> <span data-ttu-id="8e345-160">Escriba `para` y, después, presione la tecla `Tab` dos veces.</span><span class="sxs-lookup"><span data-stu-id="8e345-160">Type `para` and then press the `Tab` key twice.</span></span>

1. <span data-ttu-id="8e345-161">En la página Home (*Index.cshtml*), cambie la cantidad de incremento para `Counter` en 10 mediante el establecimiento de un atributo que coincida con el nombre de la propiedad del componente para `IncrementCount`.</span><span class="sxs-lookup"><span data-stu-id="8e345-161">On the Home page (*Index.cshtml*), change the increment amount for the `Counter` to 10 by setting an attribute that matches the name of the component's property for `IncrementCount`.</span></span>

    ```cshtml
    <Counter IncrementAmount="10" />
    ```

1. <span data-ttu-id="8e345-162">Recargue la página.</span><span class="sxs-lookup"><span data-stu-id="8e345-162">Reload the page.</span></span>

    <span data-ttu-id="8e345-163">El contador de la página Home (Inicio) ahora se incrementa en 10, mientras que el contador de la página Counter (Contador) todavía se incrementa en 1.</span><span class="sxs-lookup"><span data-stu-id="8e345-163">The counter on the Home page now increments by 10, while the counter on the Counter page still increments by 1.</span></span>

    ![Contador de Blazor por diez](https://user-images.githubusercontent.com/1874516/39509798-618f0720-4d9c-11e8-9125-3d4c634dff46.png)

## <a name="route-to-components"></a><span data-ttu-id="8e345-165">Enrutamiento a los componentes</span><span class="sxs-lookup"><span data-stu-id="8e345-165">Route to components</span></span>

<span data-ttu-id="8e345-166">La directiva `@page` en la parte superior del archivo *Counter.cshtml* especifica que este componente es una página a la que se pueden enrutar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="8e345-166">The `@page` directive at the top of the *Counter.cshtml* file specifies that this component is a page to which requests can be routed.</span></span> <span data-ttu-id="8e345-167">En concreto, el componente `Counter` controla las solicitudes enviadas a `/Counter`.</span><span class="sxs-lookup"><span data-stu-id="8e345-167">Specifically, the `Counter` component handles requests sent to `/Counter`.</span></span> <span data-ttu-id="8e345-168">Sin la directiva `@page`, el componente no controlaría las solicitudes de enrutado, pero el componente todavía podría usarse por otros componentes.</span><span class="sxs-lookup"><span data-stu-id="8e345-168">Without the `@page` directive, the component wouldn't handle routed requests, but the component could still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="8e345-169">Inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="8e345-169">Dependency injection</span></span>

<span data-ttu-id="8e345-170">Los servicios registrados con el proveedor de servicios de la aplicación están disponibles para los componentes mediante una [inserción de dependencia (DI)](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="8e345-170">Services registered with the app's service provider are available to components via [dependency injection (DI)](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection).</span></span> <span data-ttu-id="8e345-171">Los servicios se pueden insertar en un componente mediante la directiva `@inject`.</span><span class="sxs-lookup"><span data-stu-id="8e345-171">Services can be injected into a component using the `@inject` directive.</span></span>

<span data-ttu-id="8e345-172">Eche un vistazo a la implementación del componente `FetchData` en *FetchData.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8e345-172">Take a look at the implementation of the `FetchData` component in *FetchData.cshtml*.</span></span> <span data-ttu-id="8e345-173">La directiva `@inject` se utiliza para insertar una instancia de [HttpClient](https://docs.microsoft.com/dotnet/api/system.net.http.httpclient) en el componente.</span><span class="sxs-lookup"><span data-stu-id="8e345-173">The `@inject` directive is used to inject an [HttpClient](https://docs.microsoft.com/dotnet/api/system.net.http.httpclient) instance into the component.</span></span>

```cshtml
@page "/fetchdata"
@inject HttpClient Http
```

<span data-ttu-id="8e345-174">El componente `FetchData` utiliza el `HttpClient` insertado para recuperar los datos JSON del servidor cuando se inicializa el componente.</span><span class="sxs-lookup"><span data-stu-id="8e345-174">The `FetchData` component uses the injected `HttpClient` to retrieve JSON data from the server when the component is initialized.</span></span> <span data-ttu-id="8e345-175">En segundo plano, el `HttpClient` proporcionado por el tiempo de ejecución de Blazor se implementa mediante la interoperabilidad de JavaScript para llamar a Fetch API del navegador subyacente para enviar la solicitud (desde C#, es posible llamar a cualquier biblioteca JavaScript o a la API del navegador).</span><span class="sxs-lookup"><span data-stu-id="8e345-175">Under the covers, the `HttpClient` provided by the Blazor runtime is implemented using JavaScript interop to call the underlying browser's Fetch API to send the request (from C#, it's possible to call any JavaScript library or browser API).</span></span> <span data-ttu-id="8e345-176">Los datos recuperados se deserializan en la variable `forecasts` de C# como una matriz de objetos `WeatherForecast`.</span><span class="sxs-lookup"><span data-stu-id="8e345-176">The retrieved data is deserialized into the `forecasts` C# variable as an array of `WeatherForecast` objects.</span></span>

```cshtml
@functions {
    WeatherForecast[] forecasts;

    protected override async Task OnInitAsync()
    {
        forecasts = await Http.GetJsonAsync<WeatherForecast[]>("/sample-data/weather.json");
    }

    class WeatherForecast
    {
        public DateTime Date { get; set; }
        public int TemperatureC { get; set; }
        public int TemperatureF { get; set; }
        public string Summary { get; set; }
    }
}
```

<span data-ttu-id="8e345-177">Se utiliza un bucle `@foreach` para representar cada instancia de previsión como una fila en la tabla meteorológica.</span><span class="sxs-lookup"><span data-stu-id="8e345-177">A `@foreach` loop is used to render each forecast instance as a row in the weather table.</span></span>

```cshtml
<table class="table">
    <thead>
        <tr>
            <th>Date</th>
            <th>Temp. (C)</th>
            <th>Temp. (F)</th>
            <th>Summary</th>
        </tr>
    </thead>
    <tbody>
        @foreach (var forecast in forecasts)
        {
            <tr>
                <td>@forecast.Date.ToShortDateString()</td>
                <td>@forecast.TemperatureC</td>
                <td>@forecast.TemperatureF</td>
                <td>@forecast.Summary</td>
            </tr>
        }
    </tbody>
</table>
```

## <a name="build-a-todo-list"></a><span data-ttu-id="8e345-178">Creación de una lista de tareas pendientes</span><span class="sxs-lookup"><span data-stu-id="8e345-178">Build a todo list</span></span>

<span data-ttu-id="8e345-179">Agregue una nueva página a la aplicación que implemente una simple lista de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="8e345-179">Add a new page to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="8e345-180">Agregue un archivo de texto vacío a la carpeta *Pages* llamada *Todo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8e345-180">Add an empty text file to the *Pages* folder named *Todo.cshtml*.</span></span>

1. <span data-ttu-id="8e345-181">Proporcione el marcado inicial para la página.</span><span class="sxs-lookup"><span data-stu-id="8e345-181">Provide the initial markup for the page.</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>
    ```

1. <span data-ttu-id="8e345-182">Agregue la página Todo (Tareas pendientes) a la barra de navegación mediante la actualización de *Shared/NavMenu.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8e345-182">Add the Todo page to the navigation bar by updating *Shared/NavMenu.cshtml*.</span></span> <span data-ttu-id="8e345-183">Agregue un `NavLink` a la página Todo (Tareas pendientes) mediante la adición del siguiente marcado de elementos de lista debajo de los elementos de lista existentes.</span><span class="sxs-lookup"><span data-stu-id="8e345-183">Add a `NavLink` for the Todo page by adding the following list item markup below the existing list items.</span></span>

    ```cshtml
    <li class="nav-item px-3">
        <NavLink class="nav-link" href="todo">
            <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
        </NavLink>
    </li>
    ```

1. <span data-ttu-id="8e345-184">Actualice la aplicación en el explorador.</span><span class="sxs-lookup"><span data-stu-id="8e345-184">Refresh the app in the browser.</span></span> <span data-ttu-id="8e345-185">Consulte la nueva página Todo (Tareas pendientes).</span><span class="sxs-lookup"><span data-stu-id="8e345-185">See the new Todo page.</span></span>

    ![Inicio de Todo (Tareas pendientes) de Blazor](https://user-images.githubusercontent.com/1874516/39509907-bb27e77a-4d9c-11e8-91e7-ea1e7c01729e.png)

1. <span data-ttu-id="8e345-187">Agregue un archivo *TodoItem.cs* a la raíz del proyecto para contener una clase que represente los elementos de la lista de tareas.</span><span class="sxs-lookup"><span data-stu-id="8e345-187">Add a *TodoItem.cs* file to the root of the project to hold a class to represent the todo items.</span></span>

1. <span data-ttu-id="8e345-188">Use el siguiente C# de código para la clase `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="8e345-188">Use the following C# code for the `TodoItem` class.</span></span>

    ```csharp
    public class TodoItem
    {
        public string Title { get; set; }
        public bool IsDone { get; set; }
    }
    ```

1. <span data-ttu-id="8e345-189">Vuelva al componente `Todo` en *Todo.cshtml* y agregue un campo para las tareas pendientes en un bloque `@functions`.</span><span class="sxs-lookup"><span data-stu-id="8e345-189">Go back to the `Todo` component in *Todo.cshtml* and add a field for the todos in a `@functions` block.</span></span> <span data-ttu-id="8e345-190">El componente `Todo` utiliza este campo para mantener el estado de la lista de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="8e345-190">The `Todo` component uses this field to maintain the state of the todo list.</span></span>

    ```cshtml
    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
    }
    ```

1. <span data-ttu-id="8e345-191">Agregue el marcado de la lista no ordenada y un bucle `foreach` para que cada elemento de la lista se represente en un elemento de la lista de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="8e345-191">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>@todo.Title</li>
        }
    </ul>
    ```

1. <span data-ttu-id="8e345-192">La aplicación requiere elementos de la interfaz de usuario para agregar tareas pendientes a la lista.</span><span class="sxs-lookup"><span data-stu-id="8e345-192">The app requires UI elements for adding todos to the list.</span></span> <span data-ttu-id="8e345-193">Agregue una entrada de texto y un botón debajo de la lista.</span><span class="sxs-lookup"><span data-stu-id="8e345-193">Add a text input and a button below the list.</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>@todo.Title</li>
        }
    </ul>

    <input placeholder="Something todo" />
    <button>Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
    }
    ```

1. <span data-ttu-id="8e345-194">Actualice el explorador.</span><span class="sxs-lookup"><span data-stu-id="8e345-194">Refresh the browser.</span></span>

    ![Botón Add todo (Agregar tarea pendiente)](https://user-images.githubusercontent.com/1874516/39512402-bc88ab46-4da5-11e8-9e3f-87b875b56383.png)

    <span data-ttu-id="8e345-196">No ocurre nada cuando se selecciona el botón **Add todo** (Agregar tarea pendiente) porque no hay ningún controlador de eventos conectado al botón.</span><span class="sxs-lookup"><span data-stu-id="8e345-196">Nothing happens when the **Add todo** button is selected because no event handler is wired up to the button.</span></span>

1. <span data-ttu-id="8e345-197">Agregue un método `AddTodo` al componente `Todo` y regístrelo para hacer clic en los botones mediante el atributo `onclick`.</span><span class="sxs-lookup"><span data-stu-id="8e345-197">Add an `AddTodo` method to the `Todo` component and register it for button clicks using the `onclick` attribute.</span></span>

    ```cshtml
    <input placeholder="Something todo" />
    <button onclick="@AddTodo">Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();

        void AddTodo()
        {
            // Todo: Add the todo
        }
    }
    ```

    <span data-ttu-id="8e345-198">El método `AddTodo` de C# se llama cada vez que se selecciona el botón.</span><span class="sxs-lookup"><span data-stu-id="8e345-198">The `AddTodo` C# method is called every time the button is selected.</span></span>

1. <span data-ttu-id="8e345-199">Para obtener el título del nuevo elemento de tarea pendiente, agregue un campo de cadena `newTodo` y enlácelo al valor de la entrada de texto mediante el atributo `bind`.</span><span class="sxs-lookup"><span data-stu-id="8e345-199">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute.</span></span>

    ```csharp
    IList<TodoItem> todos = new List<TodoItem>();
    string newTodo;
    ```

    ```cshtml
    <input placeholder="Something todo" bind="@newTodo" />
    ```

1. <span data-ttu-id="8e345-200">Actualice el método `AddTodo` para agregar el `TodoItem` con el título especificado a la lista.</span><span class="sxs-lookup"><span data-stu-id="8e345-200">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="8e345-201">No se olvide de borrar el valor de la entrada de texto mediante el establecimiento de `newTodo` en una cadena vacía.</span><span class="sxs-lookup"><span data-stu-id="8e345-201">Don't forget to clear the value of the text input by setting `newTodo` to an empty string.</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>@todo.Title</li>
        }
    </ul>

    <input placeholder="Something todo" bind="@newTodo" />
    <button onclick="@AddTodo">Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
        string newTodo;

        void AddTodo()
        {
            if (!string.IsNullOrWhiteSpace(newTodo))
            {
                todos.Add(new TodoItem { Title = newTodo });
                newTodo = string.Empty;
            }
        }
    }
    ```

1. <span data-ttu-id="8e345-202">Actualice el explorador.</span><span class="sxs-lookup"><span data-stu-id="8e345-202">Refresh the browser.</span></span> <span data-ttu-id="8e345-203">Agregue algunas tareas pendientes a la lista de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="8e345-203">Add some todos to the todo list.</span></span>

    ![Agregar tareas pendientes](https://user-images.githubusercontent.com/1874516/39512531-2d2ff62e-4da6-11e8-8b83-291b0efc821b.png)

1. <span data-ttu-id="8e345-205">Por último, ¿qué es una lista de tareas pendientes sin casillas?</span><span class="sxs-lookup"><span data-stu-id="8e345-205">Lastly, what's a todo list without check boxes?</span></span> <span data-ttu-id="8e345-206">El texto del título de cada elemento de la lista de tareas pendientes también se puede editar.</span><span class="sxs-lookup"><span data-stu-id="8e345-206">The title text for each todo item can be made editable as well.</span></span> <span data-ttu-id="8e345-207">Agregue una entrada de casilla y una entrada de texto para cada elemento de la lista de tareas pendiente y enlace sus valores a las propiedades `Title` y `IsDone`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="8e345-207">Add a check box input and text input for each todo item and bind their values to the `Title` and `IsDone` properties, respectively.</span></span>

    ```cshtml
    <ul>
        @foreach (var todo in todos)
        {
            <li>
                <input type="checkbox" bind="@todo.IsDone" />
                <input bind="@todo.Title" />
            </li>
        }
    </ul>
    ```

1. <span data-ttu-id="8e345-208">Para comprobar que estos valores están enlazados, actualice el encabezado `h1` para mostrar un recuento del número de elementos de la lista de tareas pendientes que aún no se han hecho (`IsDone` es `false`).</span><span class="sxs-lookup"><span data-stu-id="8e345-208">To verify that these values are bound, update the `h1` header to show a count of the number of todo items that are not yet done (`IsDone` is `false`).</span></span>

    ```cshtml
    <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
    ```

1. <span data-ttu-id="8e345-209">El componente `Todo` completado debería tener este aspecto:</span><span class="sxs-lookup"><span data-stu-id="8e345-209">The completed `Todo` component should look like this:</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>
                <input type="checkbox" bind="@todo.IsDone" />
                <input bind="@todo.Title" />
            </li>
        }
    </ul>

    <input placeholder="Something todo" bind="@newTodo" />
    <button onclick="@AddTodo">Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
        string newTodo;

        void AddTodo()
        {
            if (!string.IsNullOrWhiteSpace(newTodo))
            {
                todos.Add(new TodoItem { Title = newTodo });
                newTodo = string.Empty;
            }
        }
    }
    ```

<span data-ttu-id="8e345-210">Actualice la aplicación en el explorador.</span><span class="sxs-lookup"><span data-stu-id="8e345-210">Refresh the app in the browser.</span></span> <span data-ttu-id="8e345-211">Pruebe a agregar algunos elementos de lista de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="8e345-211">Try adding some todo items.</span></span>

![Lista de tareas pendientes de Blazor finalizada](https://user-images.githubusercontent.com/1874516/39512621-83406508-4da6-11e8-8244-5bae2ac6b22d.png)

## <a name="publish-and-deploy"></a><span data-ttu-id="8e345-213">Publicación e implementación</span><span class="sxs-lookup"><span data-stu-id="8e345-213">Publish and deploy</span></span>

<span data-ttu-id="8e345-214">Cuando utilice Visual Studio, realice los siguientes pasos para publicar la aplicación Todo (Tareas pendientes) de Blazor en Azure:</span><span class="sxs-lookup"><span data-stu-id="8e345-214">When using Visual Studio, perform the following steps to publish the Todo Blazor app to Azure:</span></span>

1. <span data-ttu-id="8e345-215">Desde el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="8e345-215">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span>

1. <span data-ttu-id="8e345-216">En el cuadro de diálogo **Elegir un destino de publicación**, seleccione **App Service** y **Crear nuevo**.</span><span class="sxs-lookup"><span data-stu-id="8e345-216">In the **Pick a publish target** dialog, select **App Service** and **Create New**.</span></span> <span data-ttu-id="8e345-217">Seleccione **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="8e345-217">Select **Publish**.</span></span>

    ![Elegir un destino de publicación](build-your-first-blazor-app/_static/blazor-publish-pick-target.png)

1. <span data-ttu-id="8e345-219">En el cuadro de diálogo **Crear servicio de aplicaciones**, seleccione un nombre para la aplicación y seleccione la suscripción, el grupo de recursos y el plan de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="8e345-219">In the **Create App Service** dialog, choose a name for the app and select the subscription, resource group, and hosting plan.</span></span> <span data-ttu-id="8e345-220">Seleccione **Crear** para crear el servicio de aplicaciones y publicar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8e345-220">Select **Create** to create the app service and publish the app.</span></span>

    ![Crear servicio de aplicaciones](build-your-first-blazor-app/_static/blazor-publish-create-appservice2.png)

<span data-ttu-id="8e345-222">Espera un minuto más o menos a que la aplicación se implemente.</span><span class="sxs-lookup"><span data-stu-id="8e345-222">Wait a minute or so for the app to be deployed.</span></span>

<span data-ttu-id="8e345-223">La aplicación debería estar ejecutándose en Azure.</span><span class="sxs-lookup"><span data-stu-id="8e345-223">The app should now be running in Azure.</span></span> <span data-ttu-id="8e345-224">Marque el elemento de la lista de tareas pendientes para crear la primera aplicación Blazor como *Listo*.</span><span class="sxs-lookup"><span data-stu-id="8e345-224">Mark the todo item to build your first Blazor app as *done*.</span></span> <span data-ttu-id="8e345-225">¡Buen trabajo!</span><span class="sxs-lookup"><span data-stu-id="8e345-225">Nice job!</span></span>

![Blazor en Azure](https://user-images.githubusercontent.com/1874516/39512621-83406508-4da6-11e8-8244-5bae2ac6b22d.png)

> [!NOTE]
> <span data-ttu-id="8e345-227">Si no usa Visual Studio, publique la aplicación de Blazor en un símbolo del sistema en Windows, macOS o Linux:</span><span class="sxs-lookup"><span data-stu-id="8e345-227">If not using Visual Studio, publish the Blazor app at a command prompt on Windows, macOS, or Linux:</span></span>
>
> ```console
> dotnet publish -c Release
> ```
>
> <span data-ttu-id="8e345-228">La implementación se crea en la carpeta */bin/Release/\<plataforma-de-destino>/publish*.</span><span class="sxs-lookup"><span data-stu-id="8e345-228">The deployment is created in the */bin/Release/\<target-framework>/publish* folder.</span></span> <span data-ttu-id="8e345-229">Mueva el contenido de la carpeta *publish* al servidor o al servicio de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="8e345-229">Move the contents of the *publish* folder to the server or hosting service.</span></span>
>
> <span data-ttu-id="8e345-230">Para más información, consulte el tema [Hospedaje e implementación](xref:host-and-deploy/razor-components/index#publish-the-app).</span><span class="sxs-lookup"><span data-stu-id="8e345-230">For more information, see the [Host and deploy](xref:host-and-deploy/razor-components/index#publish-the-app) topic.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8e345-231">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="8e345-231">Additional resources</span></span>

<span data-ttu-id="8e345-232">Para una aplicación de ejemplo de Blazor más complicada, consulte el [ejemplo de buscador de vuelos](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor) en GitHub.</span><span class="sxs-lookup"><span data-stu-id="8e345-232">For a more involved Blazor sample app, check out the [Flight Finder sample](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor) on GitHub.</span></span>

![Buscador de vuelos de Blazor](https://msdnshared.blob.core.windows.net/media/2018/03/blazor-flight-finder.png)
