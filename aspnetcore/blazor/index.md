---
title: Introducción a Blazor en ASP.NET Core
author: guardrex
description: Explore Blazor en ASP.NET Core, una forma de compilar la interfaz de usuario web interactiva del lado cliente con .NET en una aplicación ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: seoapril2019
ms.date: 04/18/2019
uid: blazor/index
ms.openlocfilehash: 74eeb003c249fc9ff8267ac855455f875806ccd9
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "59983002"
---
# <a name="introduction-to-blazor"></a>Introducción a Blazor

Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)

Le damos la bienvenida a Blazor

Compile la interfaz de usuario web del lado cliente interactiva con .NET:

* Compile interfaces de usuario completamente interactivas con C# en lugar de JavaScript.
* Comparta la lógica de aplicación del lado cliente y servidor escrita con. NET.
* Represente la interfaz de usuario como HTML y CSS para la compatibilidad con todos los exploradores, incluidos los móviles.

Blazor admite escenarios básicos requeridos por la mayoría de las aplicaciones:

* Parámetros
* Control de eventos
* Enlace de datos
* Enrutamiento
* Inserción de dependencias
* Diseños
* Plantillas
* Valores en cascada

## <a name="components"></a>Componentes

Un *componente* en Blazor es un elemento de la interfaz de usuario, como una página, un cuadro de diálogo o un formulario de entrada de datos. Los componentes controlan los eventos de usuario y definen la lógica de representación flexible de la interfaz de usuario. Los componentes se pueden anidar y reutilizar.

Los componentes son clases .NET integradas en ensamblados .NET que se pueden compartir y distribuir como paquetes NuGet. La clase del componente se escribe normalmente en forma de una página de marcado de Razor con una extensión de archivo *.razor*. A veces, se hace referencia a los componentes de Blazor como componentes de Razor.

[Razor](xref:mvc/views/razor) es una sintaxis que permite combinar marcado HTML con código de C#. Razor se ha diseñado para mejorar la productividad de los desarrolladores, ya que permite cambiar entre marcado y C# en el mismo archivo, además de admitir [IntelliSense](/visualstudio/ide/using-intellisense). Las vistas de Razor Pages y MVC también usan Razor. A diferencia de las vistas de Razor Pages y MVC, que se compilan en torno a un modelo de solicitud y respuesta, los componentes se usan específicamente para gestionar la composición de la interfaz de usuario. Los componentes de Razor se pueden usar específicamente para la composición y la lógica de la interfaz de usuario del lado cliente.

El marcado que aparece a continuación es un ejemplo de un componente de cuadro de diálogo personalizado:

```cshtml
<div>
    <h2>@Title</h2>
    @BodyContent
    <button onclick="@OnOK">OK</button>
</div>

@functions {
    public string Title { get; set; }
    public RenderFragment BodyContent { get; set; }
    public Action OnOK { get; set; }
}
```

Cuando este componente se usa en otra parte de la aplicación, IntelliSense en [Visual Studio](https://visualstudio.microsoft.com/vs/) agiliza el desarrollo con la finalización de parámetros y sintaxis.

Los componentes se representan en una representación en memoria del DOM del explorador conocida como *árbol de representación* que se puede usar para actualizar la interfaz de usuario de una manera eficaz y flexible.

## <a name="blazor-server-side"></a>Lado servidor de Blazor

Blazor separa la lógica de representación de componentes del modo en que se aplican las actualizaciones de la interfaz de usuario. El lado servidor de Blazor agrega compatibilidad con el hospedaje de componentes de Razor en el servidor en una aplicación ASP.NET Core. Las actualizaciones de la interfaz de usuario se controlan a través de una conexión de SignalR.

Entorno de ejecución:

* Controla el envío de eventos de interfaz de usuario desde el explorador al servidor.
* Aplica las actualizaciones de la interfaz de usuario que el servidor devuelve al explorador después de ejecutar los componentes.

La conexión utilizada por el lado servidor de Blazor para comunicarse con el explorador también se usa para controlar las llamadas de interoperabilidad de JavaScript.

![El lado servidor de Blazor ejecuta código de .NET en el servidor e interactúa con Document Object Model en el cliente mediante una conexión de SignalR.](index/_static/blazor-server-side.png)

Para obtener más información, vea <xref:blazor/hosting-models#server-side>.

## <a name="blazor-client-side"></a>Lado cliente de Blazor

El lado cliente de Blazor es un marco de aplicaciones de una sola página para compilar aplicaciones web interactivas del lado cliente con. NET. El lado cliente de Blazor usa estándares web abiertos sin complementos ni transpilación de código. El lado cliente de Blazor funciona en todos los exploradores web modernos, incluidos los exploradores móviles.

El uso de .NET en el explorador para el desarrollo web en el lado cliente ofrece muchas ventajas:

* **Lenguaje C#**: escriba código en C# en lugar de JavaScript.
* **Ecosistema .NET**: aproveche el ecosistema existente de bibliotecas .NET.
* **Desarrollo de la pila completa**: comparta la lógica de cliente y servidor.
* **Velocidad y escalabilidad**: NET se creó pensando en el rendimiento, la confiabilidad y la seguridad.
* **Herramientas líderes del sector**: mantenga la productividad con Visual Studio en Windows, Linux y macOS.
* **Estabilidad y coherencia**:  compile sobre un conjunto común de lenguajes, marcos y herramientas que son estables, completos y fáciles de usar.

La ejecución de código .NET dentro de exploradores web se consigue mediante [WebAssembly](http://webassembly.org) (abreviado como *wasm*). WebAssembly es un estándar web abierto y se admite en exploradores web sin complementos. WebAssembly es un formato de código de bytes compacto optimizado para descargas rápidas y una velocidad de ejecución máxima.

El código de WebAssembly puede acceder a todas las funciones del explorador mediante la interoperabilidad de JavaScript. Al mismo tiempo, el código .NET ejecutado mediante WebAssembly funciona en el mismo espacio aislado de confianza que JavaScript para impedir acciones malintencionadas en la máquina cliente.

![El lado cliente de Blazor ejecuta código de .NET en el explorador con WebAssembly.](index/_static/blazor-client-side.png)

Cuando se compila y ejecuta una aplicación del lado cliente de Blazor en un explorador:

* Los archivos de código de C# y los archivos de Razor se compilan en ensamblados de .NET.
* Los ensamblados y el entorno de ejecución de .NET se descargan en el explorador.
* El lado cliente de Blazor arranca el entorno de ejecución .NET y lo configura para cargar los ensamblados de la aplicación. El entorno de ejecución del lado cliente de Blazor controla la manipulación de Document Object Model (DOM) y las llamadas a API del explorador mediante la interoperabilidad de JavaScript.

Para reducir el tamaño de la aplicación descargada, se ha quitado el código sin usar de la aplicación cuando se publica mediante el [enlazador del lenguaje intermedio (IL)](xref:host-and-deploy/blazor/configure-linker).

El lado de cliente de Blazor es un modelo de hospedaje del lado cliente. Como Razor separa la lógica de representación de un componente de la forma en que se aplican las actualizaciones de la interfaz de usuario, existe flexibilidad para hospedar Razor. Use el [lado servidor de Blazor](#blazor-server-side) para hospedar Blazor en el servidor en una aplicación de ASP.NET Core, donde todas las actualizaciones de la interfaz de usuario se controlan mediante una conexión de SignalR. Para obtener más información, vea <xref:blazor/hosting-models#server-side>. 

El tamaño de carga es un factor de rendimiento críticos para la facilidad de uso de una aplicación. El lado cliente de Blazor optimiza el tamaño de carga para reducir los tiempos de descarga:

* Las partes no usadas de los ensamblados .NET se quitan durante el proceso de compilación.
* Las respuestas HTTP se comprimen.
* El entorno de ejecución .NET y los ensamblados se almacenan en caché en el explorador.

El [lado servidor de Blazor](#blazor-server-side) proporciona un tamaño de carga más pequeño que el lado cliente Blazor al mantener los ensamblados de .NET, el ensamblado de la aplicación y el entorno de ejecución en el lado servidor. Las aplicaciones del lado servidor de Blazor solo proporcionan archivos de marcado y recursos estáticos a los clientes.

## <a name="javascript-interop"></a>Interoperabilidad de JavaScript

En el caso de aplicaciones que necesitan bibliotecas de JavaScript de terceros y API de explorador, los componentes interoperan con JavaScript. Los componentes pueden usar cualquier biblioteca o API que pueda utilizar JavaScript. El código de C# puede llamar a código JavaScript y, a su vez, el código JavaScript puede llamar a código de C#. Para obtener más información, vea [Interoperabilidad de JavaScript](xref:blazor/javascript-interop).

## <a name="code-sharing-and-net-standard"></a>Uso compartido de código y .NET Standard

Las aplicaciones pueden hacer referencia a bibliotecas de [.NET Standard](/dotnet/standard/net-standard) existentes y usarlas. .NET Standard es una especificación formal de las API de .NET comunes entre las implementaciones de .NET. Blazor implementa .NET Standard 2.0. Las API que no pueden aplicarse dentro de un explorador web (por ejemplo, para acceder al sistema de archivos, abrir un socket, ejecutar subprocesos y otras características) generan una excepción <xref:System.PlatformNotSupportedException>. Las bibliotecas de clase .NET Standard pueden compartirse a través de diferentes plataformas .NET, como Blazor, .NET Framework, .NET Core, Xamarin, Mono y Unity.

## <a name="additional-resources"></a>Recursos adicionales

* [WebAssembly](http://webassembly.org/)
* [Guía de C#](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
