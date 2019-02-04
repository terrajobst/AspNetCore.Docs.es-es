---
title: Introducción a Razor Components
author: guardrex
description: Explore Blazor, un marco web de .NET que usa C#/Razor y HTML que se ejecuta en el explorador con WebAssembly.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/index
ms.openlocfilehash: 0f7a4b2ec404b6ceec2e643fea9e0635de6ad716
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667937"
---
# <a name="introduction-to-razor-components"></a>Introducción a Razor Components

Por [Steve Sanderson](http://blog.stevensanderson.com), [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

*ASP.NET Core Razor Components* se ejecuta en el lado servidor en ASP.NET Core, mientras que *Blazor* (Razor Components ejecutado en el lado cliente) es un marco web de .NET experimental que usa C#/Razor y HTML que se ejecuta en el explorador con WebAssembly. Blazor proporciona todas las ventajas de un marco de interfaz de usuario web del lado cliente que usa .NET en el cliente.

El desarrollo web se ha mejorado en muchos aspectos a lo largo de los años, pero la creación de aplicaciones web modernas aún implica ciertos retos. Usar .NET en el explorador ofrece muchas ventajas que pueden conseguir que el desarrollo web sea más sencillo y productivo:

* **Estabilidad y coherencia**: .NET proporciona marcos de programación estandarizados y multiplataforma que son estables, fáciles de usar y de características avanzadas.
* **Lenguajes innovadores y modernos**: los lenguajes de .NET están mejorando continuamente con características nuevas e innovadoras.
* **Herramientas líderes del sector**: la familia de productos de Visual Studio ofrece una fantástica experiencia de desarrollo de .NET multiplataforma en equipos Windows, Linux y macOS.
* **Velocidad y escalabilidad**: .NET posee un sólido historial de rendimiento, confiabilidad y seguridad para el desarrollo de aplicaciones. Usar .NET como una solución de pila completa permite crear fácilmente aplicaciones rápidas, confiables y seguras.
* **Desarrollo de pila completa que aprovecha las aptitudes existentes**: los desarrolladores de C#/Razor usan sus aptitudes de C#/Razor para escribir código del lado cliente y compartir lógica del lado cliente y del lado servidor entre aplicaciones.
* **Amplia compatibilidad con exploradores**: Razor Components representa la interfaz de usuario como marcado normal y JavaScript. Blazor se ejecuta en .NET en el explorador mediante el uso de estándares web abiertos sin complementos y sin transpilación de código. Blazor funciona en todos los exploradores web modernos, incluidos los exploradores móviles.

## <a name="hosting-models"></a>Modelos de hospedaje

### <a name="server-side-hosting-model"></a>Modelo de hospedaje del lado servidor

Como Razor Components separa la lógica de representación de un componente de la forma en que se aplican las actualizaciones de la interfaz de usuario, esto ofrece flexibilidad para hospedar Razor Components. ASP.NET Core Razor Components en .NET Core 3.0 proporciona compatibilidad para hospedar Razor Components en el servidor en una aplicación de ASP.NET Core, donde todas las actualizaciones de la interfaz de usuario se controlan mediante una conexión de SignalR. El entorno de ejecución controla el envío de eventos de la interfaz de usuario desde el explorador al servidor y, después de ejecutar los componentes, vuelve a aplicar en el explorador las actualizaciones de la interfaz de usuario enviadas por el servidor. La misma conexión también se usa para controlar llamadas de interoperabilidad de JavaScript.

![Razor Components ejecuta código de .NET en el servidor e interactúa con Document Object Model en el cliente mediante una conexión de SignalR.](index/_static/aspnet-core-razor-components.png)

Para obtener más información, vea <xref:razor-components/hosting-models#server-side-hosting-model>.

### <a name="client-side-hosting-model"></a>Modelo de hospedaje del lado cliente

La ejecución de código de .NET dentro de exploradores web se consigue mediante el uso de una tecnología relativamente nueva, [WebAssembly](http://webassembly.org) (abreviada *wasm*). WebAssembly es un estándar web abierto y es compatible con exploradores web sin complementos. WebAssembly es un formato de código de bytes compacto optimizado para descargas rápidas y una velocidad de ejecución máxima.

El código de WebAssembly puede acceder a todas las funciones del explorador mediante la interoperabilidad de JavaScript. Al mismo tiempo, el código de WebAssembly se ejecuta en el mismo espacio aislado de confianza que JavaScript para impedir la ejecución de acciones malintencionadas en el equipo cliente.

![Blazor ejecuta código de .NET en el explorador con WebAssembly.](index/_static/blazor.png)

Cuando se compila y ejecuta una aplicación de Blazor en un explorador:

1. Los archivos de código de C# y los archivos de Razor se compilan en ensamblados de .NET.
1. Los ensamblados y el entorno de ejecución de .NET se descargan en el explorador.
1. Blazor usa JavaScript para arrancar el entorno de ejecución de .NET y configura el entorno de ejecución para cargar las referencias de ensamblado necesarias. El entorno de ejecución de Blazor controla la manipulación de Document Object Model (DOM) y las llamadas API del explorador mediante la interoperabilidad de JavaScript.

Para admitir exploradores anteriores no compatibles con WebAssembly, puede usar el [modelo de hospedaje del lado servidor](#server-side-hosting-model) de ASP.NET Core Razor Components.

Para obtener más información, vea <xref:razor-components/hosting-models#client-side-hosting-model>.

## <a name="components"></a>Componentes

Las aplicaciones se crean mediante *componentes*. Un componente es una parte de la interfaz de usuario, como una página, un cuadro de diálogo o un formulario de entrada de datos. Los componentes se pueden anidar, reutilizar y compartir entre proyectos.

Un *componente* es una clase de .NET. La clase se puede escribir directamente, como una clase de C# (*\*.cs*), o de forma más habitual en forma de una página de marcado de Razor (*\*.cshtml*).

[Razor](/aspnet/core/mvc/views/razor) es una sintaxis que permite combinar marcado HTML con código de C#. Razor se ha diseñado para mejorar la productividad de los desarrolladores, ya que permite cambiar entre marcado y C# en el mismo archivo, además de admitir [IntelliSense](/visualstudio/ide/using-intellisense). El marcado que aparece a continuación es un ejemplo de un componente de cuadro de diálogo personalizado básico en un archivo de Razor (*DialogComponent.cshtml*):

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

Los componentes se pueden:

* Anidar.
* Crear con código de Razor (*\*.cshtml*) o de C# (*\*.cs*).
* Compartir mediante bibliotecas de clases.
* Probar de forma unitaria sin que sea necesario un DOM del explorador.

## <a name="infrastructure"></a>Infraestructura

Razor Components ofrece los recursos principales que necesitan la mayoría de las aplicaciones, entre otros:

* Diseños
* Enrutamiento
* Inserción de dependencias

Todas estas características son opcionales. Cuando una de estas características no se usa en una aplicación, la implementación se elimina de la aplicación después de que el [enlazador de lenguaje intermedio (IL)](xref:host-and-deploy/razor-components/configure-linker) la publique.

## <a name="code-sharing-and-net-standard"></a>Uso compartido de código y .NET Standard

Las aplicaciones pueden hacer referencia a bibliotecas de [.NET Standard](/dotnet/standard/net-standard) existentes y usarlas. .NET Standard es una especificación formal de las API de .NET comunes entre las implementaciones de .NET. Se admite .NET Standard 2.0 o versiones posteriores. Las API que no pueden aplicarse dentro de un explorador web (por ejemplo, para acceder al sistema de archivos, abrir un socket, ejecutar subprocesos y otras características) generan un error [PlatformNotSupportedException](/dotnet/api/system.platformnotsupportedexception). Las bibliotecas de clases de .NET Standard se pueden compartir entre código de servidor y aplicaciones basadas en explorador.

## <a name="javascript-interop"></a>Interoperabilidad de JavaScript

En el caso de las aplicaciones que necesitan bibliotecas de JavaScript de terceros y API de explorador, WebAssembly se ha diseñado para interoperar con JavaScript. Razor Components puede usar cualquier biblioteca o API que pueda usar JavaScript. El código de C# puede llamar a código JavaScript y, a su vez, el código JavaScript puede llamar a código de C#. Para obtener más información, vea [Interoperabilidad de JavaScript](xref:razor-components/javascript-interop).

## <a name="optimization"></a>Optimización

En el caso de las aplicaciones del lado cliente, el tamaño de la carga es crítico. Blazor optimiza el tamaño de la carga para reducir los tiempos de descarga. Por ejemplo, las partes no usadas de ensamblados de .NET se quitan durante el proceso de compilación, se comprimen las respuestas HTTP y los ensamblados y el entorno de ejecución de .NET se almacenan en la caché del explorador.

Razor Components proporciona un tamaño de carga incluso más pequeño que Blazor al mantener los ensamblados de .NET, el ensamblado de la aplicación y el entorno de ejecución en el lado servidor. Las aplicaciones de Razor Components solo proporcionan marcado, scripts y hojas de estilo a los clientes.

## <a name="deployment"></a>Implementación

Use Blazor para crear una aplicación del lado cliente independiente pura o una aplicación de ASP.NET Core de pila completa que contenga aplicaciones cliente y del lado servidor:

* En una **aplicación del lado cliente independiente**, la aplicación de Blazor se compila en la carpeta *dist*, que contiene únicamente archivos estáticos. Los archivos se pueden hospedar en Azure App Service, GitHub Pages, IIS (configurado como un servidor de archivos estáticos), servidores de Node.js y muchos otros servidores y servicios. .NET no se necesita en el servidor en producción.
* En una **aplicación de ASP.NET Core de pila completa**, el código se puede compartir entre las aplicaciones cliente y las aplicaciones del lado servidor. La aplicación resultante de ASP.NET Core Razor Components, que proporciona la interfaz de usuario del lado cliente y otros puntos de conexión de la API del lado servidor, se puede compilar e implementar en cualquier host local o en la nube compatible con ASP.NET Core.

## <a name="suggest-a-feature-or-file-a-bug-report"></a>Sugerir una característica o enviar un informe de errores

Para realizar sugerencias y enviar informes de errores, [cree una incidencia](https://github.com/aspnet/AspNetCore/issues/new). Para obtener ayuda general y respuestas de la comunidad, únase a la conversación en [Gitter](https://gitter.im/aspnet/Blazor).

## <a name="additional-resources"></a>Recursos adicionales

* [WebAssembly](http://webassembly.org/)
* [Guía de C#](/dotnet/csharp/)
* [Razor](/aspnet/core/mvc/views/razor)
* [HTML](https://www.w3.org/html/)
