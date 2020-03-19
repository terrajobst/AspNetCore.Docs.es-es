---
title: Introducción a ASP.NET Core Blazor
author: guardrex
description: Para empezar a usar Blazor, compile una aplicación Blazor con las herramientas que prefiera.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/10/2020
no-loc:
- Blazor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: 89c7529d2b8ec97db731f7c7268e19937c398115
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083242"
---
# <a name="get-started-with-aspnet-core-blazor"></a>Introducción a ASP.NET Core Blazor

Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Primeros pasos con Blazor:

1. Instale el [SDK de .NET Core 3.1](https://dotnet.microsoft.com/download/dotnet-core/3.1).

1. Opcionalmente, instale la plantilla de [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly):
   * Instale el [SDK de .NET Core 3.1.102 o una versión posterior (versión preliminar)](https://dotnet.microsoft.com/download/dotnet-core/3.1).
   * Ejecute el comando siguiente en un shell de comandos. El paquete [Microsoft.AspNetCore.Components.WebAssembly.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Templates/) tiene una versión preliminar, mientras que Blazor WebAssembly se encuentra en versión preliminar.

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview2.20160.5
   ```

   > [!NOTE]
   > Se **necesita** la versión 3.1.102 o una posterior del SDK de .NET Core para usar la plantilla de WebAssembly de Blazor 3.2 (versión preliminar 2). Ejecute `dotnet --version` en un shell de comandos para confirmar la versión instalada del SDK de .NET Core.

1. Siga las instrucciones para su elección de herramientas:

   # <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

   1\. Instale la [versión 16.4 o una posterior de Visual Studio 2019](https://visualstudio.microsoft.com/vs/preview/) con la carga de trabajo **desarrollo web y ASP.NET**.

   2\. Cree un nuevo proyecto.

   3\. Seleccione **Aplicación Blazor**. Seleccione **Siguiente**.

   4\. Proporcione un nombre para el proyecto en el campo **Nombre del proyecto** o acepte el predeterminado. Confirme que la entrada de **Ubicación** es correcta o proporcione una ubicación para el proyecto. Seleccione **Crear**.

   5\. Para disfrutar de una experiencia de Blazor WebAssembly, elija la plantilla de **Aplicación Blazor WebAssembly**. Para disfrutar de una experiencia de Blazor Server, elija la plantilla de **Aplicación Blazor Server**. Seleccione **Crear**. Para obtener más información sobre los dos modelos de hospedaje de Blazor, *Blazor Server* y *Blazor WebAssembly*, vea <xref:blazor/hosting-models>. Si no aparece la plantilla de WebAssembly de Blazor, vuelva al paso anterior y reinstale la plantilla.

   6\. Presione **Ctrl**+**F5** para ejecutar la aplicación.

   > [!NOTE]
   > Si ha instalado la extensión Blazor de Visual Studio para una versión preliminar anterior de ASP.NET Core Blazor (versión preliminar 6 o anterior), puede desinstalar la extensión. Instalar las plantillas de Blazor en un shell de comandos ahora es suficiente para exponer las plantillas en Visual Studio.

   # <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

   1\. Instale [Visual Studio Code](https://code.visualstudio.com/).

   2\. Instale la [extensión de C# para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) más reciente.

   3\. Para disfrutar de una experiencia de Blazor WebAssembly, ejecute el comando siguiente en un shell de comandos:

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      Para disfrutar de una experiencia de Blazor Server, ejecute el comando siguiente en un shell de comandos:

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      Para obtener más información sobre los dos modelos de hospedaje de Blazor, *Blazor Server* y *Blazor WebAssembly*, vea <xref:blazor/hosting-models>.

   4\. Abra la carpeta *WebApplication1* en Visual Studio Code.

   5\. En el caso de un proyecto de Blazor Server, el IDE solicita que agregue recursos para compilar y depurar el proyecto. Seleccione **Sí**.

   6\. Si usa una aplicación Blazor Server, ejecute la aplicación con el depurador de Visual Studio Code. Si usa una aplicación Blazor WebAssembly, ejecute `dotnet run` desde la carpeta de proyecto de la aplicación.

   7\. En un navegador, vaya a `https://localhost:5001`.

   # <a name="visual-studio-for-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

   1\. Instale [Visual Studio para Mac](https://visualstudio.microsoft.com/vs/mac/).

   2\. Seleccione **Archivo** > **Nueva solución** o cree un **Nuevo proyecto**.

   3\. En la barra lateral, seleccione **.NET Core** > **Aplicación**.

   4\. Seleccione la plantilla de **Aplicación Blazor Server**. En este momento, en Visual Studio para Mac solo está disponible la plantilla de Blazor Server. Para disfrutar de una experiencia de Blazor WebAssembly, siga las instrucciones de la pestaña **CLI de .NET Core**. Después de seleccionar la plantilla de Blazor Server, seleccione **Siguiente**. Para obtener más información sobre los dos modelos de hospedaje de Blazor, *Blazor Server* y *Blazor WebAssembly*, vea <xref:blazor/hosting-models>.

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   5\. Establezca el **Marco de destino** en **.NET Core 3.1** y seleccione **Siguiente**.

   6\. En el campo **Nombre del proyecto**, asigne un nombre a la aplicación `WebApplication1`. Seleccione **Crear**.

   7\. Seleccione **Ejecutar** > **Ejecutar sin depuración** para ejecutar la aplicación *sin el depurador*. Ejecute la aplicación con **Iniciar depuración** para ejecutarla *con el depurador*.

   Si aparece un mensaje para que confíe en el certificado de desarrollo, hágalo y continúe.

   # <a name="net-core-cli"></a>[CLI de .NET Core](#tab/netcore-cli/)

   Para disfrutar de una experiencia Blazor WebAssembly, ejecute los comandos siguientes en un shell de comandos:

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Para disfrutar de una experiencia de Blazor Server, ejecute los comandos siguientes en un shell de comandos:

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Para obtener más información sobre los dos modelos de hospedaje de Blazor, *Blazor Server* y *Blazor WebAssembly*, vea <xref:blazor/hosting-models>.

   En un navegador, vaya a `https://localhost:5001`.

   ---

Hay varias páginas disponibles en las pestañas de la barra lateral:

* Página principal
* Contador
* Captura de datos

En la página Contador, seleccione el botón **Click me** para aumentar el contador sin una actualización de página. Para incrementar un contador en una página web suele ser necesario escribir JavaScript, pero con Blazor se puede usar C#.

*Pages/Counter.razor*:

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

Una solicitud de `/counter` en el explorador, tal y como se especifica en la directiva de `@page` en la parte superior, hace que el componente `Counter` represente su contenido. Los componentes se representan en una representación en memoria del árbol de representación que se puede usar para actualizar la interfaz de usuario de una manera eficaz y flexible.

Cada vez que se selecciona el botón **Hacer clic aquí**, ocurre lo siguiente:

* Se desencadena el evento `onclick`.
* Se llama al método `IncrementCount` .
* El elemento `currentCount` se incrementa.
* El componente se representa de nuevo.

El tiempo de ejecución compara el contenido nuevo con el anterior y solo aplica el contenido cambiado al Document Object Model (DOM).

Agregue un componente a otro mediante sintaxis HTML. Por ejemplo, para agregar el componente `Counter` a la página de inicio de la aplicación, incorpore un elemento `<Counter />` al componente `Index`.

*Pages/Index.razor*:

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

Ejecutar la aplicación. La página principal tiene su propio contador que proporciona el componente `Counter`.

Los parámetros del componente se especifican mediante atributos o [contenido secundario](xref:blazor/components#child-content), que permiten establecer propiedades en el componente secundario. Para agregar un parámetro al componente `Counter`, actualice el bloque `@code` del componente:

* Agregue una propiedad pública para `IncrementAmount` con un atributo `[Parameter]`.
* Cambie el método `IncrementCount` para usar `IncrementAmount` al aumentar el valor de `currentCount`.

*Pages/Counter.razor*:

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

Especifique `IncrementAmount` en el elemento `<Counter>` del componente `Index` mediante un atributo.

*Pages/Index.razor*:

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

Ejecutar la aplicación. El componente `Index` tiene su propio contador que se incrementa en diez cada vez que se selecciona el botón **Hacer clic aquí**. El componente `Counter` (*Counter.razor*) en `/counter` sigue incrementándose en uno.

## <a name="next-steps"></a>Pasos siguientes

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a>Recursos adicionales

* <xref:blazor/templates>
* <xref:signalr/introduction>
