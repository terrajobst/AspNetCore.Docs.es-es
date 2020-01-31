---
title: Introducción a ASP.NET Core Blazor
author: guardrex
description: Para empezar a trabajar con Blazor, cree una aplicación Blazor con las herramientas que prefiera.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/28/2019
no-loc:
- Blazor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: bd33d874b3d6122f2ab820e9b147b0e62ba03a58
ms.sourcegitcommit: fe41cff0b99f3920b727286944e5b652ca301640
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76869585"
---
# <a name="get-started-with-aspnet-core-blazor"></a>Introducción a ASP.NET Core extraordinarias

Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Introducción a más increíble:

1. Instale el [SDK de .net Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1).

1. Opcionalmente, instale la plantilla de [Webassembler de increíbles](xref:blazor/hosting-models#blazor-webassembly) :
   * Instale el [SDK de .net Core 3,1 o posterior (versión preliminar)](https://dotnet.microsoft.com/download/dotnet-core/3.1).
   * Ejecute el siguiente comando en un shell de comandos. El paquete [Microsoft. AspNetCore. extraordinarias. templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) tiene una versión preliminar, mientras que el webassembly de extraordinarias está en versión preliminar.

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.2.0-preview1.20073.1
   ```

1. Siga las instrucciones para su elección de herramientas:

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   1 \. Instale [Visual Studio 2019 versión 16,4 o posterior](https://visualstudio.microsoft.com/vs/preview/) con la carga de trabajo de **desarrollo web y ASP.net** .

   2 \. Cree un nuevo proyecto.

   3 \. Seleccione **aplicación extraordinaria**. Seleccione **Siguiente**.

   4 \. Proporcione un nombre para el proyecto en el campo **Nombre del proyecto** o acepte el predeterminado. Confirme que la entrada de **Ubicación** es correcta o proporcione una ubicación para el proyecto. Seleccione **Crear**.

   5 \. Para disfrutar de una experiencia de webassembly increíblemente rápida, elija la plantilla de **aplicación de Webassemble más brillante** . Para obtener una experiencia de servidor increíblemente rápida, elija la plantilla de **aplicación de servidor de extraordinarias** . Seleccione **Crear**. Para obtener más información sobre los dos modelos de hospedaje más increíbles, el *servidor* y el *webensamblado*increíble, consulte <xref:blazor/hosting-models>.

   6 \. Presione **Ctrl**+**F5** para ejecutar la aplicación.

   > [!NOTE]
   > Si ha instalado la extensión de Visual Studio de extraordinarias para una versión preliminar anterior de ASP.NET Core extraordinaria (versión preliminar 6 o anterior), puede desinstalar la extensión. Instalar las plantillas de extraordinarias en un shell de comandos ahora es suficiente para mostrar las plantillas en Visual Studio.

   # <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

   1 \. Instale [Visual Studio Code](https://code.visualstudio.com/).

   2 \. Instale la [ C# extensión de Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)más reciente.

   3 \. En el caso de una experiencia de webassembler extraordinaria, ejecute el siguiente comando en un shell de comandos:

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      Para obtener una experiencia de servidor extraordinaria, ejecute el siguiente comando en un shell de comandos:

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      Para obtener más información sobre los dos modelos de hospedaje más increíbles, el *servidor* y el *webensamblado*increíble, consulte <xref:blazor/hosting-models>.

   4 \. Abra la carpeta *WebApplication1* en Visual Studio Code.

   5 \. En el caso de un proyecto de servidor más rápido, el IDE solicita que agregue recursos para compilar y depurar el proyecto. Seleccione **Sí**.

   6 \. Si usa una aplicación de servidor más brillante, ejecute la aplicación con el depurador de Visual Studio Code. Si usa una aplicación de webassembly extraordinaria, ejecute `dotnet run` desde la carpeta de proyecto de la aplicación.

   7 \. En un navegador, vaya a `https://localhost:5001`.

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

   1 \. Instale [Visual Studio para Mac](https://visualstudio.microsoft.com/vs/mac/).

   2 \. Seleccione **archivo** > **nueva solución** o cree un **nuevo proyecto**.

   3 \. En la barra lateral, seleccione **.net Core** > **aplicación**.

   4 \. Seleccione la plantilla **aplicación de servidor increíbles** . En este momento, solo está disponible en Visual Studio para Mac la plantilla de servidor de extraordinarias. Para disfrutar de una experiencia de webassembly extraordinaria, siga las instrucciones de la pestaña **CLI de .net Core** . Después de seleccionar la plantilla de servidor de extraordinarias, seleccione **siguiente**. Para obtener más información sobre los dos modelos de hospedaje más increíbles, el *servidor* y el *webensamblado*increíble, consulte <xref:blazor/hosting-models>.

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   5 \. Establezca la **plataforma de destino** en **.net Core 3,1** y seleccione **siguiente**.

   6 \. En el campo **nombre del proyecto** , asigne un nombre a la aplicación `WebApplication1`. Seleccione **Crear**.

   7 \. Seleccione **ejecutar** > **ejecutar sin depurar** para ejecutar la aplicación *sin el depurador*. Ejecute la aplicación con **iniciar depuración** para ejecutar la aplicación *con el depurador*.

   Si aparece un mensaje que confía en el certificado de desarrollo, confíe en el certificado y continúe.

   # <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli/)

   En el caso de una experiencia de webassembler extraordinaria, ejecute los siguientes comandos en un shell de comandos:

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Para obtener una experiencia de servidor extraordinaria, ejecute los siguientes comandos en un shell de comandos:

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Para obtener más información sobre los dos modelos de hospedaje más increíbles, el *servidor* y el *webensamblado*increíble, consulte <xref:blazor/hosting-models>.

   En un navegador, vaya a `https://localhost:5001`.

   ---

Hay varias páginas disponibles en las pestañas de la barra lateral:

* Página de inicio de
* Contador
* Capturar datos

En la página Contador, seleccione el botón **Click me** para aumentar el contador sin una actualización de página. El incremento de un contador en una página web normalmente requiere la escritura de JavaScript, pero con C#Blazor puede usar.

*Pages/Counter.razor*:

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

Una solicitud de `/counter` en el explorador, tal y como se especifica en la Directiva de `@page` en la parte superior, hace que el componente `Counter` represente su contenido. Los componentes se representan en una representación en memoria del árbol de representación que se puede usar para actualizar la interfaz de usuario de una forma flexible y eficaz.

Cada vez que se selecciona el botón **click me** :

* Se desencadena el evento `onclick`.
* Se llama al método `IncrementCount`.
* Se incrementa el `currentCount`.
* El componente se representará de nuevo.

El tiempo de ejecución compara el nuevo contenido con el contenido anterior y solo aplica el contenido cambiado al Document Object Model (DOM).

Agregar un componente a otro componente mediante la sintaxis HTML. Por ejemplo, agregue el componente `Counter` a la Página principal de la aplicación agregando un elemento `<Counter />` al componente `Index`.

*Pages/Index.razor*:

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

Ejecutar la aplicación. La Página principal tiene su propio contador proporcionado por el componente de `Counter`.

Los parámetros de componente se especifican mediante atributos o [contenido secundario](xref:blazor/components#child-content), que permiten establecer propiedades en el componente secundario. Para agregar un parámetro al componente de `Counter`, actualice el bloque de `@code` del componente:

* Agregue una propiedad pública para `IncrementAmount` con un atributo de `[Parameter]`.
* Cambie el método `IncrementCount` para usar `IncrementAmount` al aumentar el valor de `currentCount`.

*Pages/Counter.razor*:

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

Especifique el `IncrementAmount` en el elemento `<Counter>` del componente de `Index` mediante un atributo.

*Pages/Index.razor*:

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

Ejecutar la aplicación. El componente de `Index` tiene su propio contador que se incrementa en diez cada vez que se selecciona el botón **click me** . El componente de `Counter` (*Counter. Razor*) en `/counter` continúa aumentando en uno.

## <a name="next-steps"></a>Pasos siguientes

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a>Recursos adicionales

* <xref:blazor/templates>
* <xref:signalr/introduction>
