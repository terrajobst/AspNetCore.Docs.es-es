---
title: Introducción a Razor Components
author: guardrex
description: 'Explore Razor Components de ASP.NET Core, una forma de compilar la interfaz de usuario web de cliente interactiva con .NET en una aplicación ASP.NET Core.'
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/27/2019
uid: razor-components/index
---
# <a name="introduction-to-razor-components"></a>Introducción a Razor Components

Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)

*Razor Components* es una manera de compilar la interfaz de usuario web de cliente interactiva con .NET:

* Compile interfaces de usuario completamente interactivas con C# en lugar de JavaScript.
* Comparta la lógica de aplicación del lado cliente y servidor escrita toda con. NET.
* Represente la interfaz de usuario como HTML y CSS para la compatibilidad con todos los exploradores, incluidos los móviles.

Razor Components admite elementos básicos requeridos por la mayoría de las aplicaciones, como:

* Parámetros
* Control de eventos
* Enlace de datos
* Enrutamiento
* Inserción de dependencias
* Diseños
* Plantillas
* Valores en cascada

Razor Components separa la lógica de representación de componentes del modo en que se aplican las actualizaciones de la interfaz de usuario. Razor Components de ASP.NET Core en .NET Core 3.0 agrega compatibilidad con el hospedaje de Razor Components en el servidor en una aplicación ASP.NET Core. Las actualizaciones de la interfaz de usuario se controlan a través de una conexión de SignalR.

Entorno de ejecución:

* Controla el envío de eventos de interfaz de usuario desde el explorador al servidor.
* Aplica las actualizaciones de la interfaz de usuario que el servidor devuelve al explorador después de ejecutar los componentes.

La conexión utilizada por Razor Components para comunicarse con el explorador también se usa para controlar las llamadas de interoperabilidad de JavaScript.

![Razor Components ejecuta código de .NET en el servidor e interactúa con Document Object Model en el cliente mediante una conexión de SignalR.](index/_static/aspnet-core-razor-components.png)

Para obtener más información, vea <xref:razor-components/hosting-models#server-side-hosting-model>.

*Blazor* es el modelo experimental de hospedaje del lado cliente de Razor Components. Blazor se ejecuta en .NET en el explorador mediante estándares web abiertos sin complementos ni transpilación de código. Para obtener más información, vea <xref:spa/blazor/index> y <xref:razor-components/hosting-models#client-side-hosting-model>.

## <a name="components"></a>Componentes

Un *componente Razor* es una parte de la interfaz de usuario, como una página, un cuadro de diálogo o un formulario de entrada de datos. Los componentes controlan los eventos de usuario y definen la lógica de representación flexible de la interfaz de usuario. Los componentes se pueden anidar y reutilizar.

Los componentes son clases .NET integradas en ensamblados .NET que se pueden compartir y distribuir como paquetes NuGet. La clase se escribe normalmente en forma de una página de marcado de Razor con una extensión de archivo *.razor*.

[Razor](xref:mvc/views/razor) es una sintaxis que permite combinar marcado HTML con código de C#. Razor se ha diseñado para mejorar la productividad de los desarrolladores, ya que permite cambiar entre marcado y C# en el mismo archivo, además de admitir [IntelliSense](/visualstudio/ide/using-intellisense). Las vistas de Razor Pages y MVC también usan Razor. A diferencia de las vistas de Razor Pages y MVC, que se compilan en torno a un modelo de solicitud y respuesta, los componentes se usan específicamente para gestionar la composición de la interfaz de usuario. Razor Components se puede usar específicamente para la composición y la lógica de la interfaz de usuario del lado cliente.

El marcado que aparece a continuación es un ejemplo de un componente de cuadro de diálogo personalizado en un archivo de Razor (*DialogComponent.razor*):

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

En el caso de aplicaciones que necesitan bibliotecas de JavaScript de terceros y API de explorador, los componentes interoperan con JavaScript. Los componentes pueden usar cualquier biblioteca o API que pueda utilizar JavaScript. El código de C# puede llamar a código JavaScript y, a su vez, el código JavaScript puede llamar a código de C#. Para obtener más información, vea [Interoperabilidad de JavaScript](xref:razor-components/javascript-interop).

## <a name="additional-resources"></a>Recursos adicionales

* <xref:spa/blazor/index>
* [WebAssembly](http://webassembly.org/)
* [Guía de C#](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
