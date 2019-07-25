---
title: Introducción a Blazor en ASP.NET Core
author: guardrex
description: Explore Blazor en ASP.NET Core, una forma de compilar la interfaz de usuario web interactiva del lado cliente con .NET en una aplicación ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc, seoapril2019
ms.date: 07/01/2019
uid: blazor/index
ms.openlocfilehash: 69a82bebdb787003e36568ca03e1104b9f2edf15
ms.sourcegitcommit: f30b18442ed12831c7e86b0db249183ccd749f59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2019
ms.locfileid: "68412403"
---
# <a name="introduction-to-blazor"></a>Introducción a Blazor

Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)

*Le damos la bienvenida a Blazor*.

Blazor es un marco de trabajo para la creación de interfaces de usuario web interactivas del lado cliente con .NET:

* Cree interfaces de usuario completamente interactivas con C# en lugar de JavaScript.
* Comparta la lógica de aplicación del lado cliente y servidor escrita con .NET.
* Represente la interfaz de usuario como HTML y CSS para la compatibilidad con todos los exploradores, incluidos los móviles.

El uso de .NET para el desarrollo web en el lado cliente ofrece las siguientes ventajas:

* escriba código en C# en lugar de JavaScript.
* Aprovechamiento del ecosistema y las bibliotecas .NET existentes.
* Uso compartido de la lógica de aplicación en el servidor y el cliente.
* Beneficios de rendimiento, confiabilidad y seguridad de .NET.
* mantenga la productividad con Visual Studio en Windows, Linux y macOS.
* compile sobre un conjunto común de lenguajes, marcos y herramientas que son estables, completos y fáciles de usar.

## <a name="components"></a>Componentes

Las aplicaciones de Blazor se basan en *componentes*. Un componente en Blazor es un elemento de la interfaz de usuario, como una página, un cuadro de diálogo o un formulario de entrada de datos.

Los componentes son clases de .NET compiladas en ensamblados de .NET que:

* definen la lógica de representación de la interfaz de usuario flexible;
* controlan acciones del usuario;
* se pueden anidar y reutilizar;
* se pueden compartir y distribuir como [bibliotecas de clases de Razor](xref:razor-pages/ui-class) o [paquetes de NuGet](/nuget/what-is-nuget).

La clase del componente se escribe normalmente en forma de una página de marcado de [Razor](xref:mvc/views/razor) con una extensión de archivo *.razor*. Se hace referencia a los componentes de Blazor como *componentes de Razor*. Razor es una sintaxis para combinar marcado HTML con código de C# diseñada para aumentar la productividad del desarrollador. Razor le permite cambiar entre marcado HTML y C# en el mismo archivo gracias a la compatibilidad con [IntelliSense](/visualstudio/ide/using-intellisense). Razor Pages y MVC también usan Razor. A diferencia de Razor Pages y MVC, que se compilan en torno a un modelo de solicitud y respuesta, los componentes se usan específicamente en la composición y la lógica de la interfaz de usuario del lado cliente.

El siguiente marcado de Razor muestra un componente (*Dialog.razor*), que se puede anidar dentro de otro componente:

```cshtml
<div>
    <h1>@Title</h1>

    @ChildContent

    <button @onclick="OnYes">Yes!</button>
</div>

@code {
    [Parameter]
    private string Title { get; set; }

    [Parameter]
    private RenderFragment ChildContent { get; set; }

    private void OnYes()
    {
        Console.WriteLine("Write to the console in C#! 'Yes' button was selected.");
    }
}
```

El contenido del cuerpo (`ChildContent`) y el título (`Title`) del cuadro de diálogo los proporciona el componente que usa este componente en su interfaz de usuario. `OnYes` es un método de C# desencadenado por el evento `onclick` del botón.

Blazor usa etiquetas HTML naturales para la composición de la interfaz de usuario. Los elementos HTML especifican componentes y los atributos de la etiqueta pasan valores a las propiedades del componente.

En el ejemplo siguiente, el componente `Index` utiliza el componente `Dialog`. `ChildContent` y `Title` se establecen mediante los atributos y el contenido del elemento `<Dialog>`.

*Index.razor*:

```cshtml
@page "/"

<h1>Hello, world!</h1>

Welcome to your new app.

<Dialog Title="Blazor">
    Do you want to <i>learn more</i> about Blazor?
</Dialog>
```

El cuadro de diálogo se representa cuando se accede al elemento principal (*Index.razor*) en un explorador:

![Componente Dialog representado en el explorador](index/_static/dialog.png)

Cuando este componente se usa en la aplicación, IntelliSense en [Visual Studio](/visualstudio/ide/using-intellisense) y [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) acelera el desarrollo con la finalización de parámetros y sintaxis.

Los componentes se representan en una representación en memoria de la especificación Document Object Model (DOM) del explorador llamada *árbol de representación*, que se usa para actualizar la interfaz de usuario de una manera eficaz y flexible.

## <a name="blazor-client-side"></a>Lado cliente de Blazor

El lado cliente de Blazor es un marco de trabajo de aplicación de una única página para la creación de aplicaciones web interactivas en el lado cliente con. NET. El cliente de Blazor usa estándares web abiertos sin complementos o transpilación de código y funciona en todos los exploradores web modernos, incluidos los exploradores para dispositivos móviles.

La ejecución de código .NET dentro de exploradores web se consigue mediante [WebAssembly](https://webassembly.org) (abreviado como *wasm*). WebAssembly es un formato de código de bytes compacto optimizado para descargas rápidas y una velocidad de ejecución máxima. WebAssembly es un estándar web abierto y se admite en exploradores web sin complementos.

El código de WebAssembly puede acceder a toda la funcionalidad del explorador mediante la *interoperabilidad de JavaScript* ** . El código de .NET que se ejecuta a través de WebAssembly en el explorador se ejecuta a su vez en el espacio aislado de JavaScript del explorador con las protecciones que proporciona dicho espacio aislado contra acciones malintencionadas en la máquina cliente.

![El lado cliente de Blazor ejecuta código de .NET en el explorador con WebAssembly.](index/_static/blazor-client-side.png)

Cuando se compila y ejecuta una aplicación del lado cliente de Blazor en un explorador:

* Los archivos de código de C# y los archivos de Razor se compilan en ensamblados de .NET.
* Los ensamblados y el entorno de ejecución de .NET se descargan en el explorador.
* El lado cliente de Blazor arranca el entorno de ejecución .NET y lo configura para cargar los ensamblados de la aplicación. El entorno de ejecución del lado cliente de Blazor emplea la interoperabilidad de JavaScript para gestionar la manipulación de DOM y las llamadas API del explorador.

El tamaño de la aplicación publicada, su *tamaño de carga*, es un factor de rendimiento crítico para la facilidad de uso de una aplicación. Una aplicación grande tarda un tiempo relativamente largo en descargarse en un explorador, lo que repercute en la experiencia del usuario. El lado cliente de Blazor optimiza el tamaño de carga para reducir los tiempos de descarga:

* se ha quitado el código sin usar de la aplicación cuando se publica mediante el [enlazador del lenguaje intermedio (IL)](xref:host-and-deploy/blazor/configure-linker).
* Las respuestas HTTP se comprimen.
* El entorno de ejecución .NET y los ensamblados se almacenan en caché en el explorador.

## <a name="blazor-server-side"></a>Lado servidor de Blazor

Blazor separa la lógica de representación de componentes del modo en que se aplican las actualizaciones de la interfaz de usuario. El lado servidor de Blazor agrega compatibilidad con el hospedaje de componentes de Razor en el servidor en una aplicación ASP.NET Core. Las actualizaciones de la interfaz de usuario se controlan mediante una conexión de [SignalR](xref:signalr/introduction).

El entorno de ejecución controla el envío de eventos de la interfaz de usuario del explorador al servidor y, después de ejecutar los componentes, vuelve a aplicar al explorador las actualizaciones de la interfaz de usuario enviadas por el servidor.

La conexión utilizada por el lado servidor de Blazor para comunicarse con el explorador también se usa para controlar las llamadas de interoperabilidad de JavaScript.

![El lado servidor de Blazor ejecuta código de .NET en el servidor e interactúa con Document Object Model en el cliente mediante una conexión de SignalR.](index/_static/blazor-server-side.png)

## <a name="javascript-interop"></a>Interoperabilidad de JavaScript

En el caso de aplicaciones que necesitan bibliotecas de JavaScript de terceros y acceso a las API de explorador, los componentes interoperan con JavaScript. Los componentes pueden usar cualquier biblioteca o API que pueda utilizar JavaScript. El código de C# puede llamar a código JavaScript y, a su vez, el código JavaScript puede llamar a código de C#. Para más información, consulte <xref:blazor/javascript-interop>.

## <a name="code-sharing-and-net-standard"></a>Uso compartido de código y .NET Standard

Blazor implementa [.NET Standard 2.0](/dotnet/standard/net-standard). .NET Standard es una especificación formal de las API de .NET comunes entre las implementaciones de .NET. Las bibliotecas de clase .NET Standard pueden compartirse a través de diferentes plataformas .NET, como Blazor, .NET Framework, .NET Core, Xamarin, Mono y Unity.

Las API que no pueden aplicarse dentro de un explorador web (por ejemplo, para acceder al sistema de archivos, abrir un socket y ejecutar subprocesos) generan una excepción <xref:System.PlatformNotSupportedException>.

## <a name="additional-resources"></a>Recursos adicionales

* [WebAssembly](https://webassembly.org/)
* <xref:blazor/hosting-models>
* [Guía de C#](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
