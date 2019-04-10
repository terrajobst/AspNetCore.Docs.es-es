---
title: Introducción a Blazor
author: guardrex
description: Explore Blazor de ASP.NET Core, una nueva forma de compilar aplicaciones cliente interactivas con .NET que se ejecutan en el explorador con WebAssembly.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: spa/blazor/index
ms.openlocfilehash: 87e294a975e4168add046166a95e02e485db131d
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068253"
---
# <a name="introduction-to-blazor"></a>Introducción a Blazor

Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Blazor es un marco de aplicaciones de una sola página para compilar aplicaciones web interactivas del lado cliente con. NET. Blazor usa estándares web abiertos sin complementos ni transpilación de código. Blazor funciona en todos los exploradores web modernos, incluidos los exploradores móviles.

El uso de .NET en el explorador para el desarrollo web en el lado cliente ofrece muchas ventajas:

* **Lenguaje C#**: escriba código en C# en lugar de JavaScript.
* **Ecosistema .NET**: aproveche el ecosistema existente de bibliotecas .NET.
* **Desarrollo de la pila completa**: comparta la lógica de cliente y servidor.
* **Velocidad y escalabilidad**: NET se creó pensando en el rendimiento, la confiabilidad y la seguridad.
* **Herramientas líderes del sector**: mantenga la productividad con Visual Studio en Windows, Linux y macOS.
* **Estabilidad y coherencia**:  compile sobre un conjunto común de lenguajes, marcos y herramientas que son estables, completos y fáciles de usar.

La ejecución de código .NET dentro de exploradores web se consigue mediante [WebAssembly](http://webassembly.org) (abreviado como *wasm*). WebAssembly es un estándar web abierto y se admite en exploradores web sin complementos. WebAssembly es un formato de código de bytes compacto optimizado para descargas rápidas y una velocidad de ejecución máxima.

El código de WebAssembly puede acceder a todas las funciones del explorador mediante la interoperabilidad de JavaScript. Al mismo tiempo, el código .NET ejecutado mediante WebAssembly funciona en el mismo espacio aislado de confianza que JavaScript para impedir acciones malintencionadas en la máquina cliente.

![Blazor ejecuta código de .NET en el explorador con WebAssembly.](index/_static/blazor.png)

Cuando se compila y ejecuta una aplicación de Blazor en un explorador:

* Los archivos de código de C# y los archivos de Razor se compilan en ensamblados de .NET.
* Los ensamblados y el entorno de ejecución de .NET se descargan en el explorador.
* Blazor arranca el entorno de ejecución .NET y lo configura para cargar los ensamblados de la aplicación. El entorno de ejecución de Blazor controla la manipulación de Document Object Model (DOM) y las llamadas API del explorador mediante la interoperabilidad de JavaScript.

Blazor admite medios básicos requeridos por la mayoría de las aplicaciones, incluidos:

* Parámetros
* Control de eventos
* Enlace de datos
* Enrutamiento
* Inserción de dependencias
* Diseños
* Plantillas
* Valores en cascada

Para reducir el tamaño de la aplicación descargada, se ha quitado el código sin usar de la aplicación cuando se publica mediante el [enlazador del lenguaje intermedio (IL)](xref:host-and-deploy/razor-components-blazor/configure-linker).

Blazor es el modelo de hospedaje del lado cliente para Razor Components. Como Razor Components separa la lógica de representación de un componente de la forma en que se aplican las actualizaciones de la interfaz de usuario, existe flexibilidad para hospedar Razor Components. Use Razor Components de ASP.NET Core para hospedar Razor Components en el servidor en una aplicación de ASP.NET Core, donde todas las actualizaciones de la interfaz de usuario se controlan mediante una conexión de SignalR. Para obtener más información, vea <xref:razor-components/index> y <xref:razor-components/hosting-models#server-side-hosting-model>. 

## <a name="components"></a>Componentes

Un *componente Razor* es una parte de la interfaz de usuario, como una página, un cuadro de diálogo o un formulario de entrada de datos. Los componentes controlan los eventos de usuario y definen la lógica de representación flexible de la interfaz de usuario. Los componentes se pueden anidar y reutilizar.

Los componentes son clases .NET integradas en ensamblados .NET que se pueden compartir y distribuir como paquetes NuGet. La clase se puede escribir como una página de marcado de Razor (*.cshtml*) o como una clase de C# (*.cs*).

[Razor](xref:mvc/views/razor) es una sintaxis que permite combinar marcado HTML con código de C#. Razor se ha diseñado para mejorar la productividad de los desarrolladores, ya que permite cambiar entre marcado y C# en el mismo archivo, además de admitir [IntelliSense](/visualstudio/ide/using-intellisense). Las vistas de Razor Pages y MVC también usan Razor. A diferencia de las vistas de Razor Pages y MVC, que se compilan en torno a un modelo de solicitud y respuesta, los componentes se usan específicamente para gestionar la composición de la interfaz de usuario. Razor Components se puede usar específicamente para la composición y la lógica de la interfaz de usuario del lado cliente.

El marcado que aparece a continuación es un ejemplo de un componente de cuadro de diálogo personalizado en un archivo de Razor (*DialogComponent.cshtml*):

```cshtml
<div>
    <h2>@Title</h2>
    @BodyContent
    <button onclick=@OnOK>OK</button>
</div>

@functions {
    public string Title { get; set; }
    public RenderFragment BodyContent { get; set; }
    public Action OnOK { get; set; }
}
```

Cuando este componente se usa en otra parte de la aplicación, IntelliSense agiliza el desarrollo con la finalización de parámetros y sintaxis.

Los componentes se representan en una representación en memoria del DOM del explorador conocida como *árbol de representación* que se puede usar para actualizar la interfaz de usuario de una manera eficaz y flexible.

## <a name="javascript-interop"></a>Interoperabilidad de JavaScript

En el caso de aplicaciones que necesitan bibliotecas de JavaScript de terceros y API de explorador, Blazor interopera con JavaScript. Los componentes pueden usar cualquier biblioteca o API que pueda utilizar JavaScript. El código de C# puede llamar a código JavaScript y, a su vez, el código JavaScript puede llamar a código de C#. Para obtener más información, vea [Interoperabilidad de JavaScript](xref:razor-components/javascript-interop).

## <a name="code-sharing-and-net-standard"></a>Uso compartido de código y .NET Standard

Las aplicaciones pueden hacer referencia a bibliotecas de [.NET Standard](/dotnet/standard/net-standard) existentes y usarlas. .NET Standard es una especificación formal de las API de .NET comunes entre las implementaciones de .NET. Blazor implementa .NET Standard 2.0. Las API que no pueden aplicarse dentro de un explorador web (por ejemplo, para acceder al sistema de archivos, abrir un socket, ejecutar subprocesos y otras características) generan una excepción <xref:System.PlatformNotSupportedException>. Las bibliotecas de clase .NET Standard pueden compartirse a través de diferentes plataformas .NET, como Blazor, .NET Framework, .NET Core, Xamarin, Mono y Unity.

## <a name="optimization"></a>Optimización

El tamaño de carga es un factor de rendimiento críticos para la facilidad de uso de una aplicación. Blazor optimiza el tamaño de carga para reducir los tiempos de descarga:

* Las partes no usadas de los ensamblados .NET se quitan durante el proceso de compilación.
* Las respuestas HTTP se comprimen.
* El entorno de ejecución .NET y los ensamblados se almacenan en caché en el explorador.

[Razor Components del lado servidor](xref:razor-components/index) proporciona un tamaño de carga incluso más pequeño que Blazor al mantener los ensamblados .NET, el ensamblado de la aplicación y el entorno de ejecución en el lado servidor. Las aplicaciones de Razor Components solo proporcionan archivos de marcado y recursos estáticos a los clientes.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:razor-components/index>
* [WebAssembly](http://webassembly.org/)
* [Guía de C#](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
