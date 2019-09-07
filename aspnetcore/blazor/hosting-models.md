---
title: Modelos de hospedaje increíblemente ASP.NET Core
author: guardrex
description: Comprenda los modelos de hospedaje de lado cliente y de servidor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: blazor/hosting-models
ms.openlocfilehash: f7a16d64e1f874a4f6b3c8db5217810b13c7c6ff
ms.sourcegitcommit: 43c6335b5859282f64d66a7696c5935a2bcdf966
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2019
ms.locfileid: "70800430"
---
# <a name="aspnet-core-blazor-hosting-models"></a>Modelos de hospedaje increíblemente ASP.NET Core

Por [Daniel Roth](https://github.com/danroth27)

Increíble es un marco de trabajo web diseñado para ejecutar el lado cliente en el explorador en un entorno de tiempo de ejecución .net basado en [WebAssembly](https://webassembly.org/) (*cliente más rápido*) o en el lado servidor en ASP.net Core (*servidor más rápido*). Independientemente del modelo de hospedaje, los modelos de aplicación y de componente *son los mismos*.

Para crear un proyecto para los modelos de hospedaje descritos en este artículo <xref:blazor/get-started>, vea.

## <a name="client-side"></a>Lado cliente

El modelo de hospedaje principal de increíbles se está ejecutando en el lado cliente en el explorador de webassembly. La aplicación Blazor, sus dependencias y el entorno de ejecución de .NET se descargan en el explorador. La aplicación se ejecuta directamente en el subproceso de interfaz de usuario del explorador. Las actualizaciones de la interfaz de usuario y el control de eventos se producen dentro del mismo proceso. Los recursos de la aplicación se implementan como archivos estáticos en un servidor web o servicio capaz de servir contenido estático a los clientes.

![Impresionante cliente: La aplicación extraordinaria se ejecuta en un subproceso de interfaz de usuario dentro del explorador.](hosting-models/_static/client-side.png)

Para crear una aplicación increíblemente alta con el modelo de hospedaje de cliente, use la plantilla de **aplicación de Webassemble de extraordinarias** ([dotnet New blazorwasm](/dotnet/core/tools/dotnet-new)).

Después de seleccionar la plantilla de **aplicación Webassembly** de increíble, tiene la opción de configurar la aplicación para usar un back-end de ASP.net Core activando la casilla **ASP.net Core hospedado** ([dotnet New blazorwasm--Hosted](/dotnet/core/tools/dotnet-new)). La aplicación ASP.NET Core envía la aplicación increíblemente a los clientes. La aplicación del lado cliente más increíble puede interactuar con el servidor a través de la red mediante llamadas API web o [SignalR](xref:signalr/introduction).

Las plantillas incluyen el script *increíblemente. webassembly. js* que controla:

* Descarga del tiempo de ejecución de .NET, la aplicación y las dependencias de la aplicación.
* Inicialización del tiempo de ejecución para ejecutar la aplicación.

El modelo de hospedaje del lado cliente ofrece varias ventajas:

* No hay ninguna dependencia de servidor .NET. La aplicación funciona totalmente después de descargarla en el cliente.
* Los recursos de cliente y las capacidades se aprovechan por completo.
* El trabajo se descarga del servidor al cliente.
* No es necesario un servidor Web ASP.NET Core para hospedar la aplicación. Los escenarios de implementación sin servidor son posibles (por ejemplo, para dar servicio a la aplicación desde una red CDN).

Hay desventajas en el hospedaje del lado cliente:

* La aplicación está restringida a las capacidades del explorador.
* Se requiere hardware y software de cliente compatible (por ejemplo, compatibilidad con webassembly).
* El tamaño de descarga es mayor y las aplicaciones tardan más tiempo en cargarse.
* La compatibilidad con las herramientas y el tiempo de ejecución de .NET es menos madura. Por ejemplo, existen limitaciones en [.net Standard](/dotnet/standard/net-standard) la compatibilidad y la depuración.

## <a name="server-side"></a>Lado servidor

Con el modelo de hospedaje del lado servidor, la aplicación se ejecuta en el servidor desde una aplicación ASP.NET Core. Las actualizaciones de la interfaz de usuario, el control de eventos y las llamadas de JavaScript se controlan mediante una conexión de [SignalR](xref:signalr/introduction).

![El explorador interactúa con la aplicación (hospedada en una aplicación ASP.NET Core) en el servidor a través de una conexión Signalr.](hosting-models/_static/server-side.png)

Para crear una aplicación increíblemente alta con el modelo de hospedaje del lado servidor, use la plantilla de **aplicación de servidor** de ASP.net Core increíble ([dotnet New blazorserver](/dotnet/core/tools/dotnet-new)). La aplicación ASP.NET Core hospeda la aplicación del lado servidor y crea el punto de conexión de Signalr al que se conectan los clientes.

La aplicación ASP.net Core hace referencia a la `Startup` clase de la aplicación que se va a agregar:

* Servicios del lado servidor.
* La aplicación a la canalización de control de solicitudes.

El script&dagger; *increíblemente. Server. js* establece la conexión de cliente. Es responsabilidad de la aplicación conservar y restaurar el estado de la aplicación según sea necesario (por ejemplo, en caso de que se pierda una conexión de red).

El modelo de hospedaje del lado servidor ofrece varias ventajas:

* El tamaño de la descarga es significativamente menor que el de una aplicación del lado cliente y la aplicación se carga mucho más rápido.
* La aplicación aprovecha al máximo las funcionalidades del servidor, incluido el uso de cualquier API compatible con .NET Core.
* .NET Core en el servidor se usa para ejecutar la aplicación, por lo que las herramientas de .NET existentes, como la depuración, funcionan según lo previsto.
* Se admiten clientes ligeros. Por ejemplo, las aplicaciones del lado servidor funcionan con los exploradores que no admiten webassembly y en los dispositivos con restricción de recursos.
* La base de .NET/C# Code de la aplicación, incluido el código de componente de la aplicación, no se sirve a los clientes.

Hay desventajas en el hospedaje del lado servidor:

* Normalmente existe una mayor latencia. Cada interacción del usuario implica un salto de red.
* No hay compatibilidad sin conexión. Si se produce un error en la conexión de cliente, la aplicación deja de funcionar.
* La escalabilidad es desafiante para aplicaciones con muchos usuarios. El servidor debe administrar varias conexiones de cliente y controlar el estado del cliente.
* Se requiere un servidor ASP.NET Core para atender la aplicación. Los escenarios de implementación sin servidor no son posibles (por ejemplo, para dar servicio a la aplicación desde una red CDN).

&dagger;El script *increíblemente. Server. js* se sirve desde un recurso incrustado en la ASP.net Core marco de trabajo compartido.

### <a name="reconnection-to-the-same-server"></a>Reconexión al mismo servidor

Las aplicaciones de servidor increíblemente precisas requieren una conexión de Signalr activa con el servidor. Si se pierde la conexión, la aplicación intenta volver a conectarse al servidor. Siempre que el estado del cliente todavía esté en la memoria, la sesión del cliente se reanudará sin perder el estado.
 
Cuando el cliente detecta que se ha perdido la conexión, se muestra al usuario una interfaz de usuario predeterminada mientras el cliente intenta volver a conectarse. Si se produce un error en la reconexión, se proporciona al usuario la opción de volver a intentarlo. Para personalizar la interfaz de usuario, defina un `components-reconnect-modal` elemento con `id` como su en la página de Razor *_Host. cshtml* . El cliente actualiza este elemento con una de las siguientes clases CSS según el estado de la conexión:
 
* `components-reconnect-show`&ndash; Muestre la interfaz de usuario para indicar que se perdió la conexión y que el cliente intenta volver a conectarse.
* `components-reconnect-hide`&ndash; El cliente tiene una conexión activa, oculte la interfaz de usuario.
* `components-reconnect-failed`&ndash; Error de reconexión. Para intentar de nuevo la reconexión `window.Blazor.reconnect()`, llame a.

### <a name="stateful-reconnection-after-prerendering"></a>Reconexión con estado después de la representación previa
 
Las aplicaciones de servidor increíbles se configuran de forma predeterminada para prerepresentar la interfaz de usuario en el servidor antes de que se establezca la conexión de cliente con el servidor. Esto se configura en la página de Razor de *_Host. cshtml* :
 
```cshtml
<body>
    <app>@(await Html.RenderComponentAsync<App>(RenderMode.ServerPrerendered))</app>
 
    <script src="_framework/blazor.server.js"></script>
</body>
```

`RenderMode`configura si el componente:

* Se representa en la página.
* Se representa como HTML estático en la página o si incluye la información necesaria para iniciar una aplicación extraordinaria desde el agente de usuario.

| `RenderMode`        | DESCRIPCIÓN |
| ------------------- | ----------- |
| `ServerPrerendered` | Representa el componente en código HTML estático e incluye un marcador para una aplicación del lado servidor increíblemente alta. Cuando se inicia el agente de usuario, este marcador se usa para arrancar una aplicación increíblemente alta. No se admiten los parámetros. |
| `Server`            | Representa un marcador para una aplicación del lado servidor increíblemente alta. La salida del componente no está incluida. Cuando se inicia el agente de usuario, este marcador se usa para arrancar una aplicación increíblemente alta. No se admiten los parámetros. |
| `Static`            | Representa el componente en HTML estático. Se admiten los parámetros. |

No se admite la representación de componentes de servidor desde una página HTML estática.
 
El cliente se vuelve a conectar al servidor con el mismo estado que se usó para representarla. Si el estado de la aplicación todavía está en la memoria, el estado del componente no se representará una vez establecida la conexión de Signalr.

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a>Representación de componentes interactivos con estado desde vistas y páginas de Razor
 
Los componentes interactivos con estado se pueden agregar a una página o vista de Razor.

Cuando se representa la página o la vista:

* El componente se representará con la página o la vista.
* Se pierde el estado inicial del componente usado para la representación previa.
* El nuevo estado del componente se crea cuando se establece la conexión de Signalr.
 
La siguiente página de Razor representa un `Counter` componente:

```cshtml
<h1>My Razor Page</h1>
 
@(await Html.RenderComponentAsync<Counter>(RenderMode.ServerPrerendered))
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a>Representación de componentes no interactivos desde páginas y vistas de Razor

En la siguiente página de Razor, `MyComponent` el componente se representa estáticamente con un valor inicial que se especifica mediante un formulario:
 
```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>
 
@(await Html.RenderComponentAsync<MyComponent>(RenderMode.Static, 
    new { InitialValue = InitialValue }))
 
@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

Dado `MyComponent` que se representa estáticamente, el componente no puede ser interactivo.

### <a name="detect-when-the-app-is-prerendering"></a>Detectar cuándo se está preprocesando la aplicación
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-signalr-client-for-blazor-server-side-apps"></a>Configurar el cliente de Signalr para las aplicaciones del lado servidor increíblemente
 
A veces, debe configurar el cliente de Signalr que usan las aplicaciones del lado servidor increíblemente. Por ejemplo, puede que desee configurar el registro en el cliente de Signalr para diagnosticar un problema de conexión.
 
Para configurar el cliente de Signalr en el archivo *pages/_Host. cshtml* :

* Agregue un `autostart="false"` atributo a la `<script>` etiqueta para el script *increíblemente. Server. js* .
* Llame `Blazor.start` a y pase un objeto de configuración que especifique el generador de signalr.
 
```html
<script src="_framework/blazor.server.js" autostart="false"></script>
<script>
  Blazor.start({
    configureSignalR: function (builder) {
      builder.configureLogging(2); // LogLevel.Information
    }
  });
</script>
```

## <a name="additional-resources"></a>Recursos adicionales

* <xref:blazor/get-started>
* <xref:signalr/introduction>
