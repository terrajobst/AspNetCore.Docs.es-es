---
title: Introducción a Blazor
author: guardrex
description: Obtenga información sobre cómo empezar a trabajar con Blazor mediante la creación y modificación de un proyecto Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: spa/blazor/get-started
ms.openlocfilehash: b3928c2812be6f34cdf2f17295a1251106f651e5
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068240"
---
# <a name="get-started-with-blazor"></a>Introducción a Blazor

Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

# [<a name="visual-studio"></a>Programa para la mejora](#tab/visual-studio)

Requisitos previos:

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

Para crear su primer proyecto Blazor en Visual Studio:

1. Instale la última versión [SDK de .NET Core 3.0 Preview](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.
1. Habilitar utilizar el SDK de versión preliminar de Visual Studio:
   1. Abra **herramientas** > **opciones** en la barra de menús.
   1. Abra el **proyectos y soluciones** nodo. Abra el **.NET Core** ficha.
   1. Active la casilla de **utilice las vistas previas de .NET Core SDK**. Seleccione **Aceptar**.
1. Instale la última versión [Blazor extensión](https://go.microsoft.com/fwlink/?linkid=870389) desde Visual Studio Marketplace. Este paso pone a disposición Blazor plantillas para Visual Studio.
1. Asegúrese de las plantillas de Blazor disponibles para su uso con la CLI de .NET Core ejecutando el siguiente comando en un shell de comandos:

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```
1. Cree un nuevo proyecto.
1. Seleccione **Aplicación web de ASP.NET Core**. Seleccione **Siguiente**.
1. Proporcione un nombre en el **nombre del proyecto** campo. Confirme la **ubicación** entrada es correcta o proporcionar una ubicación para el proyecto. Seleccione **Crear**.
1. Asegúrese de que **.NET Core** y **ASP.NET Core 3.0** están seleccionados en la parte superior.
1. Seleccione el **Blazor** plantilla y seleccione **crear**.
1. Presione **F5** para ejecutar la aplicación.

¡Enhorabuena! ¡Acaba de ejecutar su primera aplicación Blazor!

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

# [<a name="net-core-cli"></a>CLI de .NET Core](#tab/netcore-cli/)

Requisitos previos:

* [Obtener una vista previa de .NET core SDK 3.0](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. Agregar las plantillas Blazor ejecutando el siguiente comando en un shell de comandos:

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. Cree su primer proyecto Blazor en un shell de comandos:

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. En un navegador, vaya a `https://localhost:5001`.

¡Enhorabuena! ¡Acaba de ejecutar su primera aplicación Blazor!

---

## <a name="blazor-project"></a>Proyecto Blazor

Cuando se ejecuta la aplicación, están disponibles las fichas en la barra lateral de varias páginas:

* Página principal
* Contador
* Recuperar datos

En la página Contador, seleccione el botón **Click me** para aumentar el contador sin una actualización de página. Incrementar un contador en una página Web normalmente requiere escribir código JavaScript, pero Blazor proporciona un enfoque mejor usar C#.

*Pages/Counter.cshtml*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

Una solicitud para `/counter` en el explorador, según lo especificado por el `@page` directiva en la parte superior, hace que el componente de contador representar su contenido. Los componentes se representan en una representación en memoria del árbol de representación que puede usarse para actualizar la interfaz de usuario de una manera flexible y eficaz.

Cada vez que el **Click me** está seleccionado:

* El `onclick` desencadena el evento.
* Se llama al método `IncrementCount` .
* El `currentCount` se incrementa.
* El componente se representa de nuevo.

El tiempo de ejecución compara el contenido nuevo para el contenido anterior y solo aplica el contenido cambiado a Document Object Model (DOM).

Agregar un componente a otro componente mediante una sintaxis similar a HTML. Parámetros del componente se especifican mediante atributos o contenido secundario. Por ejemplo, un componente de contador se puede agregar a la página principal de la aplicación agregando un `<Counter />` elemento para el componente del índice.

En *Pages/index.cshtml*, reemplace el componente de encuesta de símbolo del sistema con un componente de contador:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

Ejecutar la aplicación. La página principal tiene su propio contador.

Para agregar un parámetro para el componente de contador, actualice el componente `@functions` bloque:

* Agregue una propiedad para `IncrementAmount` decorada con el `[Parameter]` atributo.
* Cambie el método `IncrementCount` para usar `IncrementAmount` al aumentar el valor de `currentCount`.

*Pages/Counter.cshtml*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

Especifique un parámetro `IncrementAmount` en el elemento `<Counter>` del componente Home mediante un atributo.

*Páginas/Index.cshtml*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

Ejecutar la aplicación. La página principal tiene su propio contador que se incrementa en diez cada vez que el **Click me** botón está seleccionado.

## <a name="next-steps"></a>Pasos siguientes

<xref:tutorials/first-razor-components-app>
