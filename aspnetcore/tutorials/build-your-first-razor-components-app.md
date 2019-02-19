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
# <a name="build-your-first-razor-components-app"></a>Compilación de su primera aplicación Razor Components

Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

En este tutorial se le muestra cómo compilar una aplicación con Razor Components. En él, además, se muestran conceptos básicos de Razor Components. Puede disfrutar este tutorial mediante un proyecto basado en Razor Components (admitido en .NET Core 3.0 o posterior) o mediante un proyecto basado en Blazor (admitido en una versión futura de .NET Core).

Para una experiencia con Razor Components de ASP.NET Core (*se recomienda*):

* Siga la guía en <xref:razor-components/get-started> para crear un proyecto basado en Razor Components.
* Dé un nombre al proyecto `RazorComponents`.
* Se crea una solución de varios proyectos a partir de la plantilla de Razor Components. El proyecto de Razor Components se genera como *RazorComponents.App*.

Para una experiencia con Blazor:

* Siga la guía en <xref:spa/blazor/get-started> para crear un proyecto basado en Blazor.
* Dé un nombre al proyecto `Blazor`.
* Se crea una solución de un solo proyecto a partir de la plantilla de Blazor.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample)). Para ver los requisitos previos, consulte los temas siguientes:

## <a name="build-components"></a>Creación de componentes

1. Vaya a cada una de tres páginas de la aplicación: Home (Inicio), Counter (Contador) y Fetch data (Recuperar datos). Estas páginas se implementan mediante los archivos de Razor en la carpeta *Pages*: *Index.cshtml*, *Counter.cshtml* y *FetchData.cshtml*.

1. En la página Contador, seleccione el botón **Click me** para aumentar el contador sin una actualización de página. Aumentar un contador en una página web suele requerir la escritura de JavaScript, pero Razor Components proporciona una mejor manera de usar C#.

1. Examine la implementación del componente Counter en el archivo *Counter.cshtml*.

   *Pages/Counter.cshtml*:

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter1.cshtml)]

   la interfaz de usuario del componente Counter se define mediante HTML. La lógica de la representación dinámica (por ejemplo, bucles, instrucciones condicionales, expresiones) se agrega mediante una sintaxis de C# insertada denominada [Razor](xref:mvc/views/razor). El marcado HTML y la lógica de representación de C# se convierten en una clase de componente en tiempo de compilación. El nombre de la clase de .NET generada coincide con el nombre del archivo.

   Los miembros de la clase de componente se definen en un bloque `@functions`. En el bloque `@functions`, se especifica el estado del componente (propiedades, campos) y los métodos para el tratamiento de eventos o para definir otra lógica del componente. Estos miembros se utilizan como parte de la lógica de representación del componente y para el tratamiento de eventos.

   Al seleccionarse el botón **Click me**:

   * Se llama al controlador `onclick` registrado del componente Counter (el método `IncrementCount`).
   * El componente Counter regenera su árbol de representación.
   * El nuevo árbol de representación se compara con el anterior.
   * Únicamente se aplican modificaciones en Document Object Model (DOM). Se actualiza el recuento mostrado.

1. Modifique la lógica de C# del componente Counter para hacer que el recuento se incremente en dos en lugar de uno.

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter2.cshtml?highlight=14)]

1. Recompile y ejecute la aplicación para ver los cambios. Seleccione el botón **Click me** y el contador se incrementará en dos.

## <a name="use-components"></a>Uso de componentes

Incluya un componente en otro componente mediante una sintaxis similar a HTML.

1. Agregue el componente Counter al componente Index (página principal) de la aplicación agregando a su vez un elemento `<Counter />` al componente Index.

   Si usa Blazor para esta experiencia, un componente Survey Prompt (elemento `<SurveyPrompt>`) está en el componente Index. Reemplace el elemento `<SurveyPrompt>` por el elemento `<Counter>`.

   *Páginas/Index.cshtml*:

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Index.cshtml?highlight=7)]

1. Recompile y ejecute la aplicación. La página principal tiene su propio contador.

## <a name="component-parameters"></a>Parámetros del componente

Los componentes también pueden tener parámetros. Los parámetros del componente se definen mediante propiedades privadas en la clase de componentes decorada con `[Parameter]`. Use atributos para especificar argumentos para un componente en el marcado.

1. Actualice el código de C# `@functions` del componente:

   * Agregue una propiedad `IncrementAmount` decorada con el atributo `[Parameter]`.
   * Cambie el método `IncrementCount` para usar `IncrementAmount` al aumentar el valor de `currentCount`.

   *Pages/Counter.cshtml*:

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/Pages/Counter.cshtml?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. Especifique un parámetro `IncrementAmount` en el elemento `<Counter>` del componente Home mediante un atributo. Establezca el valor para incrementar el contador en diez.

   *Páginas/Index.cshtml*:

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/Pages/Index.cshtml?highlight=7)]

1. Recargue la página. El contador de la página principal se incrementa en diez cada vez que se selecciona el botón **Click me**. El contador de la página *Contador* se incrementa en uno.

## <a name="route-to-components"></a>Enrutamiento a los componentes

La directiva `@page` en la parte superior del archivo *Counter.cshtml* especifica que este componente es un punto de conexión de enrutamiento. El componente Counter controla las solicitudes enviadas a `/Counter`. Sin la directiva `@page`, el componente no controla las solicitudes de enrutado, pero el componente todavía puede usarse por otros componentes.

## <a name="dependency-injection"></a>Inserción de dependencias

Los servicios registrados en el contenedor de servicios de la aplicación están disponibles para los componentes mediante una [inserción de dependencia (DI)](xref:fundamentals/dependency-injection). Inserte servicios en un componente mediante la directiva `@inject`.

Examine las directivas del componente FetchData (*Pages/FetchData.cshtml*). La directiva `@inject` se utiliza para insertar la instancia del servicio `WeatherForecastService` en el componente:

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.cshtml?highlight=3)]

El servicio `WeatherForecastService` se registra como [singleton](xref:fundamentals/dependency-injection#service-lifetimes), de modo que una instancia del servicio está disponible en toda la aplicación.

El componente FetchData usa el servicio insertado, como `ForecastService`, para recuperar una matriz de objetos `WeatherForecast`:

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData2.cshtml?highlight=6)]

Se utiliza un bucle [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) para representar cada instancia de previsión como una fila en la tabla de datos meteorológicos:

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData3.cshtml?highlight=11-19)]

## <a name="build-a-todo-list"></a>Creación de una lista de tareas pendientes

Agregue una nueva página a la aplicación que implemente una simple lista de tareas pendientes.

1. Agregue un archivo vacío a la carpeta *Pages* llamada *Todo.cshtml*.

1. Proporcione el marcado inicial para la página:

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. Agregue la página Todo (Tareas pendientes) a la barra de navegación.

   El componente NavMenu (*Shared/NavMenu.csthml*) se usa en el diseño de la aplicación. Los diseños son componentes que le permiten impedir la duplicación de contenido en la aplicación. Para obtener más información, vea <xref:razor-components/layouts>.

   Agregue un `<NavLink>` a la página Todo mediante la adición del siguiente marcado de elementos de lista debajo de los elementos de lista existentes en el archivo *Shared/NavMenu.csthml*:

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. Recompile y ejecute la aplicación. Visite la nueva página Todo para confirmar que el vínculo a la página Todo funciona.

1. Agregue un archivo *TodoItem.cs* a la raíz del proyecto para contener una clase que represente un elemento de la lista de tareas. Use el siguiente código de C# para la clase `TodoItem`:

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/TodoItem.cs)]

1. Vuelva al componente Todo (*Todo.cshtml*):

   * Agregue un campo a las tareas pendientes en un bloque `@functions`. El componente Todo utiliza este campo para mantener el estado de la lista de tareas pendientes.
   * Agregue el marcado de la lista no ordenada y un bucle `foreach` para que cada elemento de la lista se represente en un elemento de la lista de tareas pendientes.

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData4.cshtml?highlight=5-10,12-14)]

1. La aplicación requiere elementos de la interfaz de usuario para agregar tareas pendientes a la lista. Agregue una entrada de texto y un botón debajo de la lista:

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData5.cshtml?highlight=12-13)]

1. Recompile y ejecute la aplicación. Cuando se selecciona el botón **Add todo** (Agregar tarea pendiente), no ocurre nada porque no hay ningún controlador de eventos conectado al botón.

1. Agregue un método `AddTodo` al componente Todo y regístrelo para hacer clic en los botones mediante el atributo `onclick`:

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData6.cshtml?highlight=2,7-10)]

   El método `AddTodo` de C# se llama cuando se selecciona el botón.

1. Para obtener el título del nuevo elemento de tarea pendiente, agregue un campo de cadena `newTodo` y enlácelo al valor de la entrada de texto mediante el atributo `bind`:

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData7.cshtml?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo" />
   ```

1. Actualice el método `AddTodo` para agregar el `TodoItem` con el título especificado a la lista. Borre el valor de la entrada de texto mediante el establecimiento de `newTodo` en una cadena vacía:

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData8.cshtml?highlight=19-26)]

1. Recompile y ejecute la aplicación. Agregue algunas tareas pendientes a la lista de tareas pendientes para probar el nuevo código.

1. Se puede hacer que el texto de título de cada elemento de tarea pendiente sea editable y una casilla puede ayudar al usuario a realizar un seguimiento de los elementos completados. Agregue una entrada de casilla a cada elemento de tarea pendiente y enlace su valor a la propiedad `IsDone`. Cambie `@todo.Title` a un elemento `<input>` enlazado a `@todo.Title`:

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData9.cshtml?highlight=5-6)]

1. Para comprobar que estos valores están enlazados, actualice el encabezado `<h1>` para mostrar un recuento del número de elementos de la lista de tareas pendientes que no se han completado (`IsDone` es `false`).

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. El componente Todo completado (*Todo.cshtml*):

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/Pages/Todo.cshtml)]

1. Recompile y ejecute la aplicación. Agregue elementos de tarea pendiente para probar el nuevo código.

## <a name="publish-and-deploy-the-app"></a>Publicar e implementar la aplicación

Para publicar la aplicación, consulte <xref:host-and-deploy/razor-components/index#publish-the-app>.
