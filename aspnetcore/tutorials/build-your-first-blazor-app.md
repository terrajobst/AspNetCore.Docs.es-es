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
# <a name="build-your-first-blazor-app"></a>Creación de la primera aplicación Blazor

Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

En este tutorial se muestra cómo crear una aplicación con Razor Components del lado servidor o con Blazor del lado cliente.

Para una experiencia con Razor Components de ASP.NET Core del lado servidor (*recomendado*):

* Siga la guía de *experiencia de Razor Components* de <xref:blazor/get-started#server-side-razor-components-experience> para crear un proyecto basado en Razor Components.
* Dé un nombre al proyecto `RazorComponents`.

Para una experiencia con Blazor:

* Siga la guía de *experiencia de Blazor* de <xref:blazor/get-started#client-side-blazor-experience> para crear un proyecto basado en Blazor.
* Dé un nombre al proyecto `Blazor`.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-blazor-app/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample)). Para ver los requisitos previos, consulte los temas siguientes:

## <a name="build-components"></a>Creación de componentes

1. Vaya a cada una de las tres páginas de la aplicación en la carpeta *Components/Pages* (*Pages* en Blazor): Home (Inicio), Counter (Contador) y Fetch data (Recuperar datos). Estas páginas se implementan mediante los archivos de componentes de Razor: *Index.razor*, *Counter.razor* y *FetchData.razor* (Blazor continúa usando la extensión de archivo *.cshtml*: *Index.cshtml*, *Counter.cshtml* y *FetchData.cshtml*).

1. En la página Contador, seleccione el botón **Click me** para aumentar el contador sin una actualización de página. Aumentar un contador en una página web suele requerir la escritura de JavaScript, pero Blazor proporciona una mejor manera de usar C#.

1. Examine la implementación del componente Counter en el archivo *Counter.razor*.

   *Components/Pages/Counter.razor* (*Pages/Counter.cshtml* en Blazor):

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   la interfaz de usuario del componente Counter se define mediante HTML. La lógica de la representación dinámica (por ejemplo, bucles, instrucciones condicionales, expresiones) se agrega mediante una sintaxis de C# insertada denominada [Razor](xref:mvc/views/razor). El marcado HTML y la lógica de representación de C# se convierten en una clase de componente en tiempo de compilación. El nombre de la clase de .NET generada coincide con el nombre del archivo.

   Los miembros de la clase de componente se definen en un bloque `@functions`. En el bloque `@functions`, se especifica el estado del componente (propiedades, campos) y los métodos para el tratamiento de eventos o para definir otra lógica del componente. Estos miembros se utilizan como parte de la lógica de representación del componente y para el tratamiento de eventos.

   Al seleccionarse el botón **Click me**:

   * Se llama al controlador `onclick` registrado del componente Counter (el método `IncrementCount`).
   * El componente Counter regenera su árbol de representación.
   * El nuevo árbol de representación se compara con el anterior.
   * Únicamente se aplican modificaciones en Document Object Model (DOM). Se actualiza el recuento mostrado.

1. Modifique la lógica de C# del componente Counter para hacer que el recuento se incremente en dos en lugar de uno.

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. Recompile y ejecute la aplicación para ver los cambios. Seleccione el botón **Click me** y el contador se incrementará en dos.

## <a name="use-components"></a>Uso de componentes

Incluya un componente en otro componente mediante una sintaxis similar a HTML.

1. Agregue el componente Counter al componente Index (Home) de la aplicación, agregando a su vez un elemento `<Counter />` al componente Index.

   Si usa Blazor para esta experiencia, un componente Survey Prompt (elemento `<SurveyPrompt>`) está en el componente Index. Reemplace el elemento `<SurveyPrompt>` por el elemento `<Counter>`.

   *Components/Pages/Index.razor* (*Pages/Index.cshtml* en Blazor):

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index.razor?highlight=7)]

1. Recompile y ejecute la aplicación. La página Home tiene su propio contador.

## <a name="component-parameters"></a>Parámetros del componente

Los componentes también pueden tener parámetros. Los parámetros del componente se definen mediante propiedades privadas en la clase de componentes decorada con `[Parameter]`. Use atributos para especificar argumentos para un componente en el marcado.

1. Actualice el código de C# `@functions` del componente:

   * Agregue una propiedad `IncrementAmount` decorada con el atributo `[Parameter]`.
   * Cambie el método `IncrementCount` para usar `IncrementAmount` al aumentar el valor de `currentCount`.

   *Components/Pages/Counter.razor* (*Pages/Counter.cshtml* en Blazor):

   [!code-cshtml[](build-your-first-blazor-app/samples/3.x/RazorComponents/Components/Pages/Counter.razor?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. Especifique un parámetro `IncrementAmount` en el elemento `<Counter>` del componente Home mediante un atributo. Establezca el valor para incrementar el contador en diez.

   *Components/Pages/Index.razor* (*Pages/Index.cshtml* en Blazor):

   [!code-cshtml[](build-your-first-blazor-app/samples/3.x/RazorComponents/Components/Pages/Index.razor?highlight=7)]

1. Vuelva a cargar la página Home. El contador se incrementa en diez cada vez que se selecciona el botón **Click me**. El contador de la página Counter se incrementa en uno.

## <a name="route-to-components"></a>Enrutamiento a los componentes

La directiva `@page` en la parte superior del archivo *Counter.razor* especifica que este componente es un punto de conexión de enrutamiento. El componente Counter controla las solicitudes enviadas a `/Counter`. Sin la directiva `@page`, el componente no controla las solicitudes de enrutado, pero el componente todavía puede usarse por otros componentes.

## <a name="dependency-injection"></a>Inserción de dependencias

Los servicios registrados en el contenedor de servicios de la aplicación están disponibles para los componentes mediante una [inserción de dependencia (DI)](xref:fundamentals/dependency-injection). Inserte servicios en un componente mediante la directiva `@inject`.

Examine las directivas del componente FetchData de la aplicación de ejemplo.

En la aplicación de ejemplo Razor Components, el servicio `WeatherForecastService` se registra como [singleton](xref:fundamentals/dependency-injection#service-lifetimes), de modo que una instancia del servicio está disponible en toda la aplicación. La directiva `@inject` se usa para insertar la instancia del servicio `WeatherForecastService` en el componente.

*Components/Pages/FetchData.razor*:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

El componente FetchData usa el servicio insertado, como `ForecastService`, para recuperar una matriz de objetos `WeatherForecast`:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

En la versión de Blazor de la aplicación de ejemplo, se inyecta `HttpClient` para obtener los datos de la previsión meteorológica del archivo *weather.json* de la carpeta *wwwroot/ejemplo-data*:

*Pages/FetchData.cshtml*:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.cshtml?highlight=7)]

En ambas aplicaciones de ejemplo se usa un bucle [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) para representar cada instancia de previsión como una fila en la tabla de datos meteorológicos:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a>Creación de una lista de tareas pendientes

Agregue un nuevo componente a la aplicación que implemente una simple lista de tareas pendientes.

1. Agregue un archivo vacío a la aplicación de ejemplo:

   * Para la experiencia de Razor Components, agregue un archivo *Todo.razor* a la carpeta *Components/Pages*.
   * Para la experiencia de Blazor, agregue un archivo *Todo.cshtml* a la carpeta *Pages*.

1. Proporcione el marcado inicial para el componente:

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. Agregue el componente Todo a la barra de navegación.

   El componente NavMenu (*Components/Shared/NavMenu.razor* o *Shared/NavMenu.cshtml* en Blazor) se usa en el diseño de la aplicación. Los diseños son componentes que le permiten impedir la duplicación de contenido en la aplicación. Para obtener más información, vea <xref:blazor/layouts>.

   Agregue un vínculo `<NavLink>` al componente Todo mediante la incorporación del siguiente marcado de elementos de lista debajo de los elementos de lista existentes en el archivo *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* en Blazor):

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. Recompile y ejecute la aplicación. Visite la nueva página Todo para confirmar que el vínculo al componente Todo funcione.

1. Agregue un archivo *TodoItem.cs* a la raíz del proyecto para contener una clase que represente un elemento de la lista de tareas. Use el siguiente código de C# para la clase `TodoItem`:

   [!code-cshtml[](build-your-first-blazor-app/samples/3.x/RazorComponents/TodoItem.cs)]

1. Vuelva al componente Todo (*Components/Pages/Todo.razor* o *Pages/Todo.cshtml* en Blazor):

   * Agregue un campo a las tareas pendientes en un bloque `@functions`. El componente Todo utiliza este campo para mantener el estado de la lista de tareas pendientes.
   * Agregue el marcado de la lista no ordenada y un bucle `foreach` para que cada elemento de la lista se represente en un elemento de la lista de tareas pendientes.

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. La aplicación requiere elementos de la interfaz de usuario para agregar tareas pendientes a la lista. Agregue una entrada de texto y un botón debajo de la lista:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. Recompile y ejecute la aplicación. Cuando se selecciona el botón **Add todo** (Agregar tarea pendiente), no ocurre nada porque no hay ningún controlador de eventos conectado al botón.

1. Agregue un método `AddTodo` al componente Todo y regístrelo para hacer clic en los botones mediante el atributo `onclick`:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

   El método `AddTodo` de C# se llama cuando se selecciona el botón.

1. Para obtener el título del nuevo elemento de tarea pendiente, agregue un campo de cadena `newTodo` y enlácelo al valor de la entrada de texto mediante el atributo `bind`:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo">
   ```

1. Actualice el método `AddTodo` para agregar el `TodoItem` con el título especificado a la lista. Borre el valor de la entrada de texto mediante el establecimiento de `newTodo` en una cadena vacía:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. Recompile y ejecute la aplicación. Agregue algunas tareas pendientes a la lista de tareas pendientes para probar el nuevo código.

1. Se puede hacer que el texto de título de cada elemento de tarea pendiente sea editable y una casilla puede ayudar al usuario a realizar un seguimiento de los elementos completados. Agregue una entrada de casilla a cada elemento de tarea pendiente y enlace su valor a la propiedad `IsDone`. Cambie `@todo.Title` a un elemento `<input>` enlazado a `@todo.Title`:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. Para comprobar que estos valores están enlazados, actualice el encabezado `<h1>` para mostrar un recuento del número de elementos de la lista de tareas pendientes que no se han completado (`IsDone` es `false`).

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. El componente Todo completado (*Components/Pages/Todo.razor* o *Pages/Todo.cshtml* en Blazor):

   [!code-cshtml[](build-your-first-blazor-app/samples/3.x/RazorComponents/Components/Pages/Todo.razor)]

1. Recompile y ejecute la aplicación. Agregue elementos de tarea pendiente para probar el nuevo código.

## <a name="publish-and-deploy-the-app"></a>Publicar e implementar la aplicación

Para publicar la aplicación, consulte <xref:host-and-deploy/blazor/index>.
