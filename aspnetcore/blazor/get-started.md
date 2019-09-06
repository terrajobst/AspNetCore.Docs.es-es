---
title: Introducción a ASP.NET Core extraordinarias
author: guardrex
description: Para empezar a trabajar con más increíble, cree una aplicación increíblemente con las herramientas de su elección.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2019
uid: blazor/get-started
ms.openlocfilehash: 030c289e072efad43d4f6cdd63cba07cc623a090
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310431"
---
# <a name="get-started-with-aspnet-core-blazor"></a>Introducción a ASP.NET Core extraordinarias

Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)

Introducción a más increíble:

1. Instale la versión más reciente del [SDK de .net Core 3,0 Preview](https://dotnet.microsoft.com/download/dotnet-core/3.0) .

1. Instale las plantillas extraordinarias ejecutando el comando siguiente en un shell de comandos:

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.0.0-preview9.19424.4
   ```

1. Siga las instrucciones para su elección de herramientas:

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   1 \. Instale la versión preliminar más reciente de [Visual Studio](https://visualstudio.com/vs/preview) con la carga de trabajo de **desarrollo web y ASP.net** .

   2 \. Cree un nuevo proyecto.

   3 \. Seleccione **aplicación extraordinaria**. Seleccione **Next** (Siguiente).

   4 \. Proporcione un nombre para el proyecto en el campo **Nombre del proyecto** o acepte el predeterminado. Confirme que la entrada de **Ubicación** es correcta o proporcione una ubicación para el proyecto. Seleccione **Crear**.

   5 \. En el caso de una experiencia del lado del cliente, elija la plantilla de **aplicación de Webassemble más brillante** . Para disfrutar de una experiencia de servidor increíblemente brillante, elija la plantilla **aplicación de servidor increíblemente** alta. Seleccione **Crear**. Para obtener información sobre los dos modelos de hospedaje más increíbles, en el lado servidor y en el <xref:blazor/hosting-models>lado cliente, vea.

   6 \. Presione **F5** para ejecutar la aplicación.

   > [!NOTE]
   > Si ha instalado la extensión de Visual Studio de extraordinarias para una versión preliminar anterior de ASP.NET Core extraordinaria (versión preliminar 6 o anterior), puede desinstalar la extensión. Instalar las plantillas de extraordinarias en un shell de comandos ahora es suficiente para mostrar las plantillas en Visual Studio.

   # <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

   1 \. Instale [Visual Studio Code](https://code.visualstudio.com/).

   2 \. Instale la [ C# extensión de Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)más reciente.

   3 \. Para disfrutar de una experiencia del lado del cliente, ejecute el siguiente comando en un shell de comandos:

      ```console
      dotnet new blazorwasm -o WebApplication1
      ```

      Para disfrutar de una experiencia del lado del servidor, ejecute el siguiente comando en un shell de comandos:

      ```console
      dotnet new blazorserver -o WebApplication1
      ```

      Para obtener información sobre los dos modelos de hospedaje más increíbles, en el lado servidor y en el <xref:blazor/hosting-models>lado cliente, vea.

   4 \. Abra la carpeta *WebApplication1* en Visual Studio Code.

   5 \. En el caso de un proyecto de servidor increíblemente rápido, el IDE solicita que agregue recursos para compilar y depurar el proyecto. Seleccione **Sí**.

   6 \. Si usa una aplicación del lado servidor increíblemente alta, ejecute la aplicación con el depurador de Visual Studio Code. Si usa una aplicación del lado cliente increíblemente alta, ejecute `dotnet run` desde la carpeta de proyecto de la aplicación.

   7 \. En un navegador, vaya a `https://localhost:5001`.

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor server-side experience, select the **Blazor Server App** template. For a Blazor client-side experience, select the **Blazor WebAssembly App** template. Select **Next**. For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli/)

   Para disfrutar de una experiencia del lado del cliente, ejecute los siguientes comandos en un shell de comandos:

   ```console
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Para disfrutar de una experiencia del lado del servidor, ejecute los siguientes comandos en un shell de comandos:

   ```console
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Para obtener información sobre los dos modelos de hospedaje más increíbles, en el lado servidor y en el <xref:blazor/hosting-models>lado cliente, vea.

   En un navegador, vaya a `https://localhost:5001`.

   ---

Hay varias páginas disponibles en las pestañas de la barra lateral:

* Página principal
* Contador
* Capturar datos

En la página Contador, seleccione el botón **Click me** para aumentar el contador sin una actualización de página. El incremento de un contador en una página web normalmente requiere la escritura de JavaScript, pero los componentes de C#Razor proporcionan un enfoque mejor mediante.

*Pages/Counter.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

Una solicitud de `/counter` en el explorador, tal como se especifica `@page` en la Directiva en la parte superior `Counter` , hace que el componente represente su contenido. Los componentes se representan en una representación en memoria del árbol de representación que se puede usar para actualizar la interfaz de usuario de una forma flexible y eficaz.

Cada vez que se selecciona el botón **click me** :

* Se `onclick` desencadena el evento.
* Se llama al método `IncrementCount` .
* `currentCount` Se incrementa.
* El componente se representará de nuevo.

El tiempo de ejecución compara el nuevo contenido con el contenido anterior y solo aplica el contenido cambiado al Document Object Model (DOM).

Agregar un componente a otro componente mediante la sintaxis HTML. Por ejemplo, agregue el `Counter` componente a la Página principal de la aplicación agregando un `<Counter />` elemento `Index` al componente.

*Pages/Index.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

Ejecutar la aplicación. La Página principal tiene su propio contador proporcionado por `Counter` el componente.

Los parámetros de componente se especifican mediante atributos o [contenido secundario](xref:blazor/components#child-content), que permiten establecer propiedades en el componente secundario. Para agregar un parámetro al `Counter` componente, actualice el bloque del `@code` componente:

* Agregue una propiedad pública para `IncrementAmount` con un `[Parameter]` atributo.
* Cambie el método `IncrementCount` para usar `IncrementAmount` al aumentar el valor de `currentCount`.

*Pages/Counter.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

Especifique en el `Index` elemento del `<Counter>` componente mediante un atributo. `IncrementAmount`

*Pages/Index.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

Ejecutar la aplicación. El `Index` componente tiene su propio contador que se incrementa en diez cada vez que se selecciona el botón **click me** . El `Counter` componente (*Counter. Razor*) de `/counter` continúa aumentando en uno.

## <a name="next-steps"></a>Pasos siguientes

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a>Recursos adicionales

* <xref:signalr/introduction>
