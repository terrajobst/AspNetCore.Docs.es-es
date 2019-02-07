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
# <a name="build-your-first-blazor-app"></a>Creación de la primera aplicación Blazor

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

En este tutorial, va a crear una aplicación Blazor paso a paso y aprender rápidamente las características básicas de la plataforma de Razor Components.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-blazor-app/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample)). Consulte el tema [Introducción](xref:razor-components/get-started) para ver los requisitos previos.

Para crear el proyecto en Visual Studio:

1. Seleccione **Archivo** > **Nuevo** > **Proyecto**. Seleccione **Web** > **Aplicación web ASP.NET Core**. Llame al proyecto "BlazorApp1" en el campo **Nombre**. Seleccione **Aceptar**.

    ![Nuevo proyecto de ASP.NET Core](build-your-first-blazor-app/_static/new-aspnet-core-project.png)

1. Se muestra el cuadro de diálogo **Nueva aplicación web ASP.NET Core**. Asegúrese de que **.NET Core** está seleccionado en la parte superior. Seleccione **ASP.NET Core 2.1**. Elija la plantilla **Blazor** y seleccione **Aceptar**.

    ![Nuevo cuadro de diálogo de la aplicación Blazor](build-your-first-blazor-app/_static/new-blazor-app-dialog.png)

1. Una vez creado el proyecto, presione **Ctrl-F5** para ejecutar la aplicación *sin el depurador*. En este momento no se admite la ejecución con el depurador (**F5**).

> [!NOTE]
> Si no usa Visual Studio, cree la aplicación de Blazor en un símbolo del sistema en Windows, macOS o Linux:
>
> ```console
> dotnet new --install "Microsoft.AspNetCore.Blazor.Templates"
> dotnet new blazor -o BlazorApp1
> cd BlazorApp1
> dotnet run
> ```
>
> Vaya la aplicación con la dirección y el puerto del host local proporcionados en la salida de la ventana de la consola después de que se ejecute `dotnet run`. Use **Ctrl-C** en la ventana de la consola para cerrar la aplicación.

La aplicación Blazor se ejecuta en el explorador:

![Página Home (Inicio) de la aplicación Blazor](https://user-images.githubusercontent.com/1874516/39509497-5515c3ea-4d9b-11e8-887f-019ea4fdb3ee.png)

## <a name="build-components"></a>Creación de componentes

1. Vaya a cada una de tres páginas de la aplicación: Home (Inicio), Counter (Contador) y Fetch data (Recuperar datos).

    Estas tres páginas se implementan mediante los tres archivos de Razor en la carpeta *Pages*: *Index.cshtml*, *Counter.cshtml* y *FetchData.cshtml*. Cada uno de estos archivos implementa un componente que se compila y ejecuta en el lado cliente en el navegador.

1. Seleccione el botón en la página Counter (Contador).

    ![Página Home (Inicio) de la aplicación Blazor](https://user-images.githubusercontent.com/1874516/39509525-6e367c66-4d9b-11e8-9978-e52a9750c34b.png)

    Cada vez que se selecciona el botón, el contador se incrementa sin una actualización de la página. Normalmente, este tipo de comportamiento del lado cliente se controla en JavaScript; pero aquí se implementa en C# y .NET por el componente `Counter`.

1. Eche un vistazo a la implementación del componente `Counter` en el archivo *Counter.cshtml*:

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

    La interfaz de usuario del componente `Counter` se define mediante código HTML normal. La lógica de la representación dinámica (por ejemplo, bucles, instrucciones condicionales, expresiones) se agrega mediante una sintaxis de C# insertada denominada [Razor](https://docs.microsoft.com/aspnet/core/mvc/views/razor). El marcado HTML y la lógica de representación de C# se convierten en una clase de componente en tiempo de compilación. El nombre de la clase de .NET generada coincide con el nombre del archivo.

    Los miembros de la clase de componente se definen en un bloque `@functions`. En el bloque `@functions`, se puede especificar el estado del componente (propiedades, campos) y los métodos para el tratamiento de eventos o para definir otra lógica del componente. Estos miembros se pueden utilizar como parte de la lógica de representación del componente y para el tratamiento de eventos.

    Cuando se selecciona el botón, se llama al controlador `onclick` registrado del componente `Counter` (el método `IncrementCount`) y el componente `Counter` regenera su árbol de representación. Blazor compara el nuevo árbol de representación con el anterior y aplica las modificaciones necesarias al explorador de Document Object Model (DOM). Se actualiza el recuento mostrado.

1. Actualice el marcado para el componente `Counter` para que el encabezado de nivel superior sea más  *interesante*.

    ```cshtml
    @page "/counter"
    <h1><em>Counter!!</em></h1>
    ```

1. También modifique la lógica de C# del componente `Counter` para hacer que el recuento se incremente en dos en lugar de uno.

    ```cshtml
    @functions {
        int currentCount = 0;

        void IncrementCount()
        {
            currentCount += 2;
        }
    }
    ```

1. Actualice la página del contador en el explorador para ver los cambios.

    ![Contador interesante](https://user-images.githubusercontent.com/1874516/39509668-e8949a92-4d9b-11e8-91e9-b6a494695d92.png)

## <a name="use-components"></a>Uso de componentes

Después de definir un componente, este se puede utilizar para implementar otros componentes. El marcado para utilizar un componente se parece a una etiqueta HTML en la que el nombre de la etiqueta es el tipo de componente.

1. Agregue un componente `Counter` a la página Home (Inicio) de la aplicación (*Index.cshtml*).

    ```cshtml
    @page "/"

    <h1>Hello, world!</h1>

    Welcome to your new app.

    <SurveyPrompt Title="How is Blazor working for you?" />

    <Counter />
    ```

1. Actualice la página Home (Inicio) en el explorador. Observe la instancia independiente del componente `Counter` en la página Home (Inicio).

    ![Página Home (Inicio) de Blazor con el contador](https://user-images.githubusercontent.com/1874516/39509718-224483f6-4d9c-11e8-9030-b4c7228d669d.png)

## <a name="component-parameters"></a>Parámetros del componente

Los componentes también pueden tener parámetros, que se definen mediante propiedades privadas en la clase de componentes decorada con `[Parameter]`. Use atributos para especificar argumentos para un componente en el marcado. 

1. Actualice el componente `Counter` para que tenga un parámetro `IncrementAmount` cuyo valor predeterminado es 1.

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
    > Desde Visual Studio, puede agregar rápidamente un parámetro de componente mediante el fragmento `para`. Escriba `para` y, después, presione la tecla `Tab` dos veces.

1. En la página Home (*Index.cshtml*), cambie la cantidad de incremento para `Counter` en 10 mediante el establecimiento de un atributo que coincida con el nombre de la propiedad del componente para `IncrementCount`.

    ```cshtml
    <Counter IncrementAmount="10" />
    ```

1. Recargue la página.

    El contador de la página Home (Inicio) ahora se incrementa en 10, mientras que el contador de la página Counter (Contador) todavía se incrementa en 1.

    ![Contador de Blazor por diez](https://user-images.githubusercontent.com/1874516/39509798-618f0720-4d9c-11e8-9125-3d4c634dff46.png)

## <a name="route-to-components"></a>Enrutamiento a los componentes

La directiva `@page` en la parte superior del archivo *Counter.cshtml* especifica que este componente es una página a la que se pueden enrutar las solicitudes. En concreto, el componente `Counter` controla las solicitudes enviadas a `/Counter`. Sin la directiva `@page`, el componente no controlaría las solicitudes de enrutado, pero el componente todavía podría usarse por otros componentes.

## <a name="dependency-injection"></a>Inserción de dependencias

Los servicios registrados con el proveedor de servicios de la aplicación están disponibles para los componentes mediante una [inserción de dependencia (DI)](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection). Los servicios se pueden insertar en un componente mediante la directiva `@inject`.

Eche un vistazo a la implementación del componente `FetchData` en *FetchData.cshtml*. La directiva `@inject` se utiliza para insertar una instancia de [HttpClient](https://docs.microsoft.com/dotnet/api/system.net.http.httpclient) en el componente.

```cshtml
@page "/fetchdata"
@inject HttpClient Http
```

El componente `FetchData` utiliza el `HttpClient` insertado para recuperar los datos JSON del servidor cuando se inicializa el componente. En segundo plano, el `HttpClient` proporcionado por el tiempo de ejecución de Blazor se implementa mediante la interoperabilidad de JavaScript para llamar a Fetch API del navegador subyacente para enviar la solicitud (desde C#, es posible llamar a cualquier biblioteca JavaScript o a la API del navegador). Los datos recuperados se deserializan en la variable `forecasts` de C# como una matriz de objetos `WeatherForecast`.

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

Se utiliza un bucle `@foreach` para representar cada instancia de previsión como una fila en la tabla meteorológica.

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

## <a name="build-a-todo-list"></a>Creación de una lista de tareas pendientes

Agregue una nueva página a la aplicación que implemente una simple lista de tareas pendientes.

1. Agregue un archivo de texto vacío a la carpeta *Pages* llamada *Todo.cshtml*.

1. Proporcione el marcado inicial para la página.

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>
    ```

1. Agregue la página Todo (Tareas pendientes) a la barra de navegación mediante la actualización de *Shared/NavMenu.cshtml*. Agregue un `NavLink` a la página Todo (Tareas pendientes) mediante la adición del siguiente marcado de elementos de lista debajo de los elementos de lista existentes.

    ```cshtml
    <li class="nav-item px-3">
        <NavLink class="nav-link" href="todo">
            <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
        </NavLink>
    </li>
    ```

1. Actualice la aplicación en el explorador. Consulte la nueva página Todo (Tareas pendientes).

    ![Inicio de Todo (Tareas pendientes) de Blazor](https://user-images.githubusercontent.com/1874516/39509907-bb27e77a-4d9c-11e8-91e7-ea1e7c01729e.png)

1. Agregue un archivo *TodoItem.cs* a la raíz del proyecto para contener una clase que represente los elementos de la lista de tareas.

1. Use el siguiente C# de código para la clase `TodoItem`.

    ```csharp
    public class TodoItem
    {
        public string Title { get; set; }
        public bool IsDone { get; set; }
    }
    ```

1. Vuelva al componente `Todo` en *Todo.cshtml* y agregue un campo para las tareas pendientes en un bloque `@functions`. El componente `Todo` utiliza este campo para mantener el estado de la lista de tareas pendientes.

    ```cshtml
    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
    }
    ```

1. Agregue el marcado de la lista no ordenada y un bucle `foreach` para que cada elemento de la lista se represente en un elemento de la lista de tareas pendientes.

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

1. La aplicación requiere elementos de la interfaz de usuario para agregar tareas pendientes a la lista. Agregue una entrada de texto y un botón debajo de la lista.

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

1. Actualice el explorador.

    ![Botón Add todo (Agregar tarea pendiente)](https://user-images.githubusercontent.com/1874516/39512402-bc88ab46-4da5-11e8-9e3f-87b875b56383.png)

    No ocurre nada cuando se selecciona el botón **Add todo** (Agregar tarea pendiente) porque no hay ningún controlador de eventos conectado al botón.

1. Agregue un método `AddTodo` al componente `Todo` y regístrelo para hacer clic en los botones mediante el atributo `onclick`.

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

    El método `AddTodo` de C# se llama cada vez que se selecciona el botón.

1. Para obtener el título del nuevo elemento de tarea pendiente, agregue un campo de cadena `newTodo` y enlácelo al valor de la entrada de texto mediante el atributo `bind`.

    ```csharp
    IList<TodoItem> todos = new List<TodoItem>();
    string newTodo;
    ```

    ```cshtml
    <input placeholder="Something todo" bind="@newTodo" />
    ```

1. Actualice el método `AddTodo` para agregar el `TodoItem` con el título especificado a la lista. No se olvide de borrar el valor de la entrada de texto mediante el establecimiento de `newTodo` en una cadena vacía.

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

1. Actualice el explorador. Agregue algunas tareas pendientes a la lista de tareas pendientes.

    ![Agregar tareas pendientes](https://user-images.githubusercontent.com/1874516/39512531-2d2ff62e-4da6-11e8-8b83-291b0efc821b.png)

1. Por último, ¿qué es una lista de tareas pendientes sin casillas? El texto del título de cada elemento de la lista de tareas pendientes también se puede editar. Agregue una entrada de casilla y una entrada de texto para cada elemento de la lista de tareas pendiente y enlace sus valores a las propiedades `Title` y `IsDone`, respectivamente.

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

1. Para comprobar que estos valores están enlazados, actualice el encabezado `h1` para mostrar un recuento del número de elementos de la lista de tareas pendientes que aún no se han hecho (`IsDone` es `false`).

    ```cshtml
    <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
    ```

1. El componente `Todo` completado debería tener este aspecto:

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

Actualice la aplicación en el explorador. Pruebe a agregar algunos elementos de lista de tareas pendientes.

![Lista de tareas pendientes de Blazor finalizada](https://user-images.githubusercontent.com/1874516/39512621-83406508-4da6-11e8-8244-5bae2ac6b22d.png)

## <a name="publish-and-deploy"></a>Publicación e implementación

Cuando utilice Visual Studio, realice los siguientes pasos para publicar la aplicación Todo (Tareas pendientes) de Blazor en Azure:

1. Desde el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Publicar**.

1. En el cuadro de diálogo **Elegir un destino de publicación**, seleccione **App Service** y **Crear nuevo**. Seleccione **Publicar**.

    ![Elegir un destino de publicación](build-your-first-blazor-app/_static/blazor-publish-pick-target.png)

1. En el cuadro de diálogo **Crear servicio de aplicaciones**, seleccione un nombre para la aplicación y seleccione la suscripción, el grupo de recursos y el plan de hospedaje. Seleccione **Crear** para crear el servicio de aplicaciones y publicar la aplicación.

    ![Crear servicio de aplicaciones](build-your-first-blazor-app/_static/blazor-publish-create-appservice2.png)

Espera un minuto más o menos a que la aplicación se implemente.

La aplicación debería estar ejecutándose en Azure. Marque el elemento de la lista de tareas pendientes para crear la primera aplicación Blazor como *Listo*. ¡Buen trabajo!

![Blazor en Azure](https://user-images.githubusercontent.com/1874516/39512621-83406508-4da6-11e8-8244-5bae2ac6b22d.png)

> [!NOTE]
> Si no usa Visual Studio, publique la aplicación de Blazor en un símbolo del sistema en Windows, macOS o Linux:
>
> ```console
> dotnet publish -c Release
> ```
>
> La implementación se crea en la carpeta */bin/Release/\<plataforma-de-destino>/publish*. Mueva el contenido de la carpeta *publish* al servidor o al servicio de hospedaje.
>
> Para más información, consulte el tema [Hospedaje e implementación](xref:host-and-deploy/razor-components/index#publish-the-app).

## <a name="additional-resources"></a>Recursos adicionales

Para una aplicación de ejemplo de Blazor más complicada, consulte el [ejemplo de buscador de vuelos](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor) en GitHub.

![Buscador de vuelos de Blazor](https://msdnshared.blob.core.windows.net/media/2018/03/blazor-flight-finder.png)
