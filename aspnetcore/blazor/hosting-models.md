---
title: Modelos de hospedaje de Blazor de ASP.NET Core
author: guardrex
description: Comprenda Blazor los modelos de hospedaje de webassembly y Blazor Server.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
- blazor.webassembly.js
uid: blazor/hosting-models
ms.openlocfilehash: c9521acf40317c90d1197660bfa516710263cfc9
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2020
ms.locfileid: "76160046"
---
# <a name="aspnet-core-opno-locblazor-hosting-models"></a>Modelos de hospedaje de Blazor de ASP.NET Core

Por [Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor es un marco de trabajo web diseñado para ejecutar el lado cliente en el explorador en un entorno de tiempo de ejecución .NET basado en [Webassembly](https://webassembly.org/)( *Blazor webassembly*) o en el servidor en ASP.net Core ( *Blazor Server*). Independientemente del modelo de hospedaje, los modelos de aplicación y de componente *son los mismos*.

Para crear un proyecto para los modelos de hospedaje descritos en este artículo, consulte <xref:blazor/get-started>.

## <a name="opno-locblazor-webassembly"></a>Blazor WebAssembly

El modelo de hospedaje principal de Blazor se está ejecutando en el lado cliente en el explorador de webassembly. La aplicación Blazor, sus dependencias y el entorno de ejecución de .NET se descargan en el explorador. La aplicación se ejecuta directamente en el subproceso de interfaz de usuario del explorador. Las actualizaciones de la interfaz de usuario y el control de eventos se producen dentro del mismo proceso. Los recursos de la aplicación se implementan como archivos estáticos en un servidor web o servicio capaz de servir contenido estático a los clientes.

![[! Operador. Webassembly NO-LOC (increíble)]: [! Operador. NO-LOC (increíble)]: la aplicación se ejecuta en un subproceso de interfaz de usuario dentro del explorador.](hosting-models/_static/blazor-webassembly.png)

Para crear una aplicación Blazor mediante el modelo de hospedaje del lado cliente, use la plantilla de **aplicación de WebassembleBlazor** ([dotnet New blazorwasm](/dotnet/core/tools/dotnet-new)).

Después de seleccionar la plantilla de **aplicaciónBlazor Webassembly** , tiene la opción de configurar la aplicación para usar un back-end de ASP.net Core activando la casilla **ASP.net Core hospedado** ([dotnet New blazorwasm--Hosted](/dotnet/core/tools/dotnet-new)). La aplicación ASP.NET Core sirve a la aplicación de Blazor a los clientes. La aplicación Blazor webassembly puede interactuar con el servidor a través de la red mediante llamadas API Web o [SignalR](xref:signalr/introduction).

Las plantillas incluyen el script de `blazor.webassembly.js` que controla:

* Descarga del tiempo de ejecución de .NET, la aplicación y las dependencias de la aplicación.
* Inicialización del tiempo de ejecución para ejecutar la aplicación.

El modelo de hospedaje de webassembly de Blazor ofrece varias ventajas:

* No hay ninguna dependencia de servidor .NET. La aplicación funciona totalmente después de descargarla en el cliente.
* Los recursos de cliente y las capacidades se aprovechan por completo.
* El trabajo se descarga del servidor al cliente.
* No es necesario un servidor Web ASP.NET Core para hospedar la aplicación. Los escenarios de implementación sin servidor son posibles (por ejemplo, para dar servicio a la aplicación desde una red CDN).

Hay desventajas para Blazor hospedaje de webassembly:

* La aplicación está restringida a las capacidades del explorador.
* Se requiere hardware y software de cliente compatible (por ejemplo, compatibilidad con webassembly).
* El tamaño de descarga es mayor y las aplicaciones tardan más tiempo en cargarse.
* La compatibilidad con las herramientas y el tiempo de ejecución de .NET es menos madura. Por ejemplo, existen limitaciones en [.net Standard](/dotnet/standard/net-standard) la compatibilidad y la depuración.

## <a name="opno-locblazor-server"></a>Servidor de Blazor

Con el modelo de hospedaje de servidor de Blazor, la aplicación se ejecuta en el servidor desde una aplicación ASP.NET Core. Las actualizaciones de la interfaz de usuario, el control de eventos y las llamadas de JavaScript se controlan mediante una conexión [SignalR](xref:signalr/introduction).

![El explorador interactúa con la aplicación (hospedada dentro de una aplicación ASP.NET Core) en el servidor a través de un [! Operador. Conexión NO-LOC (Signalr)].](hosting-models/_static/blazor-server.png)

Para crear una aplicación de Blazor con el modelo de hospedaje de Blazor Server, use la plantilla de **aplicación de servidor** de ASP.net CoreBlazor ([dotnet New blazorserver](/dotnet/core/tools/dotnet-new)). La aplicación ASP.NET Core hospeda la aplicación de Blazor Server y crea el punto de conexión de SignalR donde se conectan los clientes.

La aplicación ASP.NET Core hace referencia a la clase `Startup` de la aplicación que se va a agregar:

* Servicios del lado servidor.
* La aplicación a la canalización de control de solicitudes.

El script de `blazor.server.js`&dagger; establece la conexión del cliente. Es responsabilidad de la aplicación conservar y restaurar el estado de la aplicación según sea necesario (por ejemplo, en caso de que se pierda una conexión de red).

El modelo de hospedaje de Blazor Server ofrece varias ventajas:

* El tamaño de la descarga es significativamente menor que una aplicación Blazor webassembly y la aplicación se carga mucho más rápido.
* La aplicación aprovecha al máximo las funcionalidades del servidor, incluido el uso de cualquier API compatible con .NET Core.
* .NET Core en el servidor se usa para ejecutar la aplicación, por lo que las herramientas de .NET existentes, como la depuración, funcionan según lo previsto.
* Se admiten clientes ligeros. Por ejemplo, las aplicaciones de Blazor Server funcionan con los exploradores que no admiten webassembly y en los dispositivos con restricción de recursos.
* La base de .NET/C# Code de la aplicación, incluido el código de componente de la aplicación, no se sirve a los clientes.

Hay desventajas para Blazor servidor que hospeda:

* Normalmente existe una mayor latencia. Cada interacción del usuario implica un salto de red.
* No hay compatibilidad sin conexión. Si se produce un error en la conexión de cliente, la aplicación deja de funcionar.
* La escalabilidad es desafiante para aplicaciones con muchos usuarios. El servidor debe administrar varias conexiones de cliente y controlar el estado del cliente.
* Se requiere un servidor ASP.NET Core para atender la aplicación. Los escenarios de implementación sin servidor no son posibles (por ejemplo, para dar servicio a la aplicación desde una red CDN).

&dagger;el script de `blazor.server.js` se sirve desde un recurso incrustado en el marco de ASP.NET Core Shared.

### <a name="comparison-to-server-rendered-ui"></a>Comparación con la interfaz de usuario representada por el servidor

Una manera de comprender las aplicaciones de Blazor Server es comprender cómo difiere de los modelos tradicionales para representar la interfaz de usuario en ASP.NET Core aplicaciones con vistas de Razor o Razor Pages. Ambos modelos usan el lenguaje Razor para describir el contenido HTML, pero difieren significativamente en el modo en que se representa el marcado.

Cuando se representa una página o vista de Razor, cada línea de código Razor emite HTML en forma de texto. Después de la representación, el servidor desecha la instancia de la página o la vista, incluido cualquier Estado que se haya producido. Cuando se produce otra solicitud para la página, por ejemplo, cuando se produce un error en la validación del servidor y se muestra el Resumen de la validación:

* La página completa se vuelve a representar en el texto HTML de nuevo.
* La página se envía al cliente.

Una aplicación Blazor se compone de elementos reutilizables de la interfaz de usuario denominada *componentes*. Un componente contiene C# código, marcado y otros componentes. Cuando se representa un componente, Blazor genera un gráfico de los componentes incluidos de forma similar a una Document Object Model HTML o XML (DOM). Este gráfico incluye el estado del componente contenido en propiedades y campos. Blazor evalúa el gráfico de componentes para generar una representación binaria del marcado. El formato binario puede ser:

* Se convierte en texto HTML (durante la representación previa).
* Se usa para actualizar de forma eficaz el marcado durante la representación normal.

Una actualización de la interfaz de usuario en Blazor se desencadena mediante:

* Interacción con el usuario, como la selección de un botón.
* Desencadenadores de aplicaciones, como un temporizador.

El gráfico se representará y se calculará *una diferencia de interfaz de* usuario (diferencia). Esta diferencia es el conjunto más pequeño de ediciones DOM necesarias para actualizar la interfaz de usuario en el cliente. La diferencia se envía al cliente en un formato binario y se aplica mediante el explorador.

Un componente se desecha una vez que el usuario sale de él en el cliente. Mientras que un usuario interactúa con un componente, el estado del componente (servicios, recursos) debe mantenerse en la memoria del servidor. Dado que el servidor puede mantener el estado de muchos componentes al mismo tiempo, el agotamiento de la memoria es un problema que se debe solucionar. Para obtener instrucciones sobre cómo crear una aplicación de Blazor Server para garantizar el mejor uso de la memoria del servidor, consulte <xref:security/blazor/server>.

### <a name="integrate-razor-components-into-razor-pages-and-mvc-apps"></a>Integre componentes de Razor en aplicaciones de Razor Pages y MVC

#### <a name="use-components-in-pages-and-views"></a>Usar componentes en páginas y vistas

Una aplicación Razor Pages o MVC existente puede integrar los componentes de Razor en páginas y vistas:

1. En el archivo de diseño de la aplicación ( *_Layout. cshtml*):

   * Agregue la siguiente etiqueta de `<base>` al elemento `<head>`:

     ```html
     <base href="~/" />
     ```

     El valor `href` (la *ruta de acceso base*de la aplicación) en el ejemplo anterior supone que la aplicación reside en la ruta de acceso URL raíz (`/`). Si la aplicación es una aplicación secundaria, siga las instrucciones de la sección *ruta de acceso base* de la aplicación del artículo <xref:host-and-deploy/blazor/index#app-base-path>.

     El archivo *_Layout. cshtml* se encuentra en la carpeta *páginas/compartidas* de una aplicación de Razor pages o en una carpeta de *vistas/compartidas* en una aplicación MVC.

   * Agregue una etiqueta de `<script>` para el script *increíblemente. Server. js* dentro de la etiqueta de cierre `</body>`:

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     El marco de trabajo agrega el script *extraordinaria. Server. js* a la aplicación. No es necesario agregar manualmente el script a la aplicación.

1. Agregue un archivo *_Imports. Razor* a la carpeta raíz del proyecto con el siguiente contenido (cambie el último espacio de nombres, `MyAppNamespace`, al espacio de nombres de la aplicación):

   ```csharp
   @using System.Net.Http
   @using Microsoft.AspNetCore.Authorization
   @using Microsoft.AspNetCore.Components.Authorization
   @using Microsoft.AspNetCore.Components.Forms
   @using Microsoft.AspNetCore.Components.Routing
   @using Microsoft.AspNetCore.Components.Web
   @using Microsoft.JSInterop
   @using MyAppNamespace
   ```

1. En `Startup.ConfigureServices`, agregue el servicio servidor de Blazor:

   ```csharp
   services.AddServerSideBlazor();
   ```

1. En `Startup.Configure`, agregue el punto de conexión del concentrador de Blazor a `app.UseEndpoints`:

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. Integre los componentes en cualquier página o vista. Para más información, consulte la sección *integración de componentes en Razor pages y aplicaciones MVC* del artículo <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.

#### <a name="use-routable-components-in-a-razor-pages-app"></a>Uso de componentes enrutables en una aplicación Razor Pages

Para admitir componentes de Razor enrutables en Razor Pages aplicaciones:

1. Siga las instrucciones de la sección [uso de componentes en páginas y vistas](#use-components-in-pages-and-views) .

1. Agregue un archivo *app. Razor* a la raíz del proyecto con el siguiente contenido:

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. Agregue un archivo *_Host. cshtml* a la carpeta *pages* con el siguiente contenido:

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   Los componentes de usan el archivo Shared *_Layout. cshtml* para su diseño.

1. Agregue una ruta de prioridad baja para la página *_Host. cshtml* a la configuración del punto de conexión en `Startup.Configure`:

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. Agregue componentes enrutables a la aplicación. Por ejemplo:

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   Al usar una carpeta personalizada para contener los componentes de la aplicación, agregue el espacio de nombres que representa la carpeta al archivo *pages/_ViewImports. cshtml* . Para obtener más información, vea <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.

#### <a name="use-routable-components-in-an-mvc-app"></a>Uso de componentes enrutables en una aplicación MVC

Para admitir componentes de Razor enrutables en aplicaciones MVC:

1. Siga las instrucciones de la sección [uso de componentes en páginas y vistas](#use-components-in-pages-and-views) .

1. Agregue un archivo *app. Razor* a la raíz del proyecto con el siguiente contenido:

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. Agregue un archivo *_Host. cshtml* a la carpeta *views/Home* con el siguiente contenido:

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   Los componentes de usan el archivo Shared *_Layout. cshtml* para su diseño.

1. Agregue una acción al controlador Home:

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. Agregue una ruta de prioridad baja para la acción de controlador que devuelve la vista *_Host. cshtml* a la configuración del punto de conexión en `Startup.Configure`:

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. Cree una carpeta de *páginas* y agregue componentes enrutables a la aplicación. Por ejemplo:

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   Al usar una carpeta personalizada para contener los componentes de la aplicación, agregue el espacio de nombres que representa la carpeta al archivo *views/_ViewImports. cshtml* . Para obtener más información, vea <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.

### <a name="circuits"></a>Circuitos

Una aplicación Blazor Server se basa en [ASP.NET Core SignalR](xref:signalr/introduction). Cada cliente se comunica con el servidor a través de una o varias conexiones SignalR llamadas un *circuito*. Un circuito es la abstracción de Blazoren SignalR conexiones que pueden tolerar interrupciones de red temporales. Cuando un cliente de Blazor ve que la conexión de SignalR está desconectada, intenta volver a conectarse al servidor mediante una nueva conexión de SignalR.

Cada pantalla del explorador (pestaña del explorador o iframe) que está conectada a una aplicación de Blazor Server usa una conexión SignalR. Esta es otra diferencia importante en comparación con las aplicaciones típicas representadas por el servidor. En una aplicación representada por el servidor, la apertura de la misma aplicación en varias pantallas del explorador normalmente no se traduce en demandas de recursos adicionales en el servidor. En una aplicación de Blazor Server, cada pantalla del explorador requiere un circuito independiente e instancias independientes del estado del componente que administra el servidor.

Blazor considera cerrar una pestaña del explorador o desplazarse a una dirección URL externa como terminación *correcta* . En el caso de una terminación correcta, el circuito y los recursos asociados se liberan inmediatamente. Un cliente también puede desconectarse de manera no correcta, por ejemplo, debido a una interrupción de la red. Blazor Server almacena los circuitos desconectados durante un intervalo configurable para permitir que el cliente vuelva a conectarse. Para obtener más información, consulte la sección [reconexión al mismo servidor](#reconnection-to-the-same-server) .

### <a name="ui-latency"></a>Latencia de IU

La latencia de la interfaz de usuario es el tiempo que tarda una acción iniciada en el momento en que se actualiza la interfaz de usuario. Los valores más pequeños para la latencia de la interfaz de usuario son imprescindibles para que una aplicación pueda responder a un usuario. En una aplicación de Blazor Server, cada acción se envía al servidor, se procesa y se devuelve una diferencia de la interfaz de usuario. Por lo tanto, la latencia de la interfaz de usuario es la suma de la latencia de red y la latencia del servidor en el procesamiento de la acción.

En el caso de una aplicación de línea de negocio que está limitada a una red corporativa privada, el efecto en las percepciones de usuario de latencia debido a la latencia de red suele ser imperceptibles. En el caso de una aplicación implementada a través de Internet, la latencia puede ser apreciable para los usuarios, especialmente si los usuarios están ampliamente distribuidos geográficamente.

El uso de memoria también puede contribuir a la latencia de la aplicación. El aumento del uso de memoria da como resultado la recolección frecuente de elementos no utilizados o la paginación de memoria en el disco, y ambos degradan el rendimiento de la aplicación y, por consiguiente, aumentan la latencia Para obtener más información, vea <xref:security/blazor/server>.

las aplicaciones de Blazor Server deben optimizarse para minimizar la latencia de la interfaz de usuario al reducir la latencia de red y el uso de memoria. Para obtener información acerca de cómo medir la latencia de red, consulte <xref:host-and-deploy/blazor/server#measure-network-latency>. Para obtener más información sobre SignalR y Blazor, vea:

* <xref:host-and-deploy/blazor/server>
* <xref:security/blazor/server>

### <a name="connection-to-the-server"></a>Conexión al servidor

las aplicaciones de Blazor Server requieren una conexión de SignalR activa con el servidor. Si se pierde la conexión, la aplicación intenta volver a conectarse al servidor. Siempre que el estado del cliente todavía esté en la memoria, la sesión del cliente se reanudará sin perder el estado.

#### <a name="reconnection-to-the-same-server"></a>Reconexión al mismo servidor

Una aplicación de Blazor Server se representa en respuesta a la primera solicitud de cliente, que configura el estado de la interfaz de usuario en el servidor. Cuando el cliente intenta crear una conexión SignalR, el cliente debe volver a conectarse al mismo servidor. las aplicaciones de Blazor Server que usan más de un servidor back-end deben implementar *sesiones permanentes* para conexiones SignalR.

Se recomienda usar [Azure SignalR Service](/azure/azure-signalr) para las aplicaciones Blazor Server. El servicio permite el escalado vertical de una aplicación Blazor Server a un gran número de conexiones SignalR simultáneas. Las sesiones permanentes están habilitadas para el servicio Azure SignalR estableciendo la opción de `ServerStickyMode` o el valor de configuración del servicio en `Required`. Para obtener más información, vea <xref:host-and-deploy/blazor/server#signalr-configuration>.

Cuando se usa IIS, las sesiones permanentes se habilitan con el enrutamiento de solicitud de aplicaciones. Para más información, vea [Equilibrio de carga HTTP mediante el enrutamiento de solicitud de aplicaciones](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).

#### <a name="reflect-the-connection-state-in-the-ui"></a>Reflejar el estado de conexión en la interfaz de usuario

Cuando el cliente detecta que se ha perdido la conexión, se muestra al usuario una interfaz de usuario predeterminada mientras el cliente intenta volver a conectarse. Si se produce un error en la reconexión, se proporciona al usuario la opción de volver a intentarlo.

Para personalizar la interfaz de usuario, defina un elemento con un `id` de `components-reconnect-modal` en la `<body>` de la página de Razor de *_Host. cshtml* :

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

En la tabla siguiente se describen las clases CSS aplicadas al elemento `components-reconnect-modal`.

| Clase CSS                       | Indica&hellip; |
| ------------------------------- | ------------------------- |
| `components-reconnect-show`     | Una conexión perdida. El cliente está intentando volver a conectarse. Muestre el modal. |
| `components-reconnect-hide`     | Una conexión activa se restablece en el servidor. Oculte el modal. |
| `components-reconnect-failed`   | Error de reconexión, probablemente debido a un error de red. Para intentar la reconexión, llame a `window.Blazor.reconnect()`. |
| `components-reconnect-rejected` | Reconexión rechazada. Se alcanzó el servidor pero se rechazó la conexión y se pierde el estado del usuario en el servidor. Para volver a cargar la aplicación, llame a `location.reload()`. Este estado de conexión puede producirse cuando:<ul><li>Se produce un bloqueo en el circuito del servidor.</li><li>El cliente se desconecta el tiempo suficiente para que el servidor Quite el estado del usuario. Se eliminan las instancias de los componentes con los que el usuario está interactuando.</li><li>El servidor se reinicia o se recicla el proceso de trabajo de la aplicación.</li></ul> |

### <a name="stateful-reconnection-after-prerendering"></a>Reconexión con estado después de la representación previa

las aplicaciones de Blazor Server se configuran de forma predeterminada para prerepresentar la interfaz de usuario en el servidor antes de establecer la conexión de cliente con el servidor. Esto se configura en la página de Razor *_Host. cshtml* :

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

`RenderMode` configura si el componente:

* Se representa en la página.
* Se representa como HTML estático en la página o si incluye la información necesaria para arrancar una aplicación Blazor desde el agente de usuario.

| `RenderMode`        | Descripción |
| ------------------- | ----------- |
| `ServerPrerendered` | Representa el componente en código HTML estático e incluye un marcador para una aplicación de Blazor Server. Cuando se inicia el agente de usuario, este marcador se usa para arrancar una aplicación Blazor. |
| `Server`            | Representa un marcador para una aplicación de Blazor Server. La salida del componente no está incluida. Cuando se inicia el agente de usuario, este marcador se usa para arrancar una aplicación Blazor. |
| `Static`            | Representa el componente en HTML estático. |

No se admite la representación de componentes de servidor desde una página HTML estática.

Cuando se `ServerPrerendered`la `RenderMode`, el componente se representa inicialmente estáticamente como parte de la página. Una vez que el explorador vuelve a establecer una conexión con el servidor, el componente se *vuelve*a representar y el componente ahora es interactivo. Si el método de ciclo de vida [{Async} inicializado](xref:blazor/lifecycle#component-initialization-methods) para inicializar el componente está presente, el método se ejecuta *dos veces*:

* Cuando el componente se representa de forma estática.
* Una vez establecida la conexión al servidor.

Esto puede dar lugar a un cambio notable en los datos mostrados en la interfaz de usuario cuando el componente se representa finalmente.

Para evitar el escenario de representación doble en una aplicación de Blazor Server:

* Pase un identificador que se puede usar para almacenar en caché el estado durante la representación previa y recuperar el estado después de que se reinicie la aplicación.
* Use el identificador durante la representación previa para guardar el estado del componente.
* Use el identificador después de la representación previa para recuperar el estado almacenado en caché.

En el código siguiente se muestra una `WeatherForecastService` actualizada en una aplicación de Blazor Server basada en plantillas que evita la representación doble:

```csharp
public class WeatherForecastService
{
    private static readonly string[] Summaries = new[]
    {
        "Freezing", "Bracing", "Chilly", "Cool", "Mild",
        "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
    };
    
    public WeatherForecastService(IMemoryCache memoryCache)
    {
        MemoryCache = memoryCache;
    }
    
    public IMemoryCache MemoryCache { get; }

    public Task<WeatherForecast[]> GetForecastAsync(DateTime startDate)
    {
        return MemoryCache.GetOrCreateAsync(startDate, async e =>
        {
            e.SetOptions(new MemoryCacheEntryOptions
            {
                AbsoluteExpirationRelativeToNow = 
                    TimeSpan.FromSeconds(30)
            });

            var rng = new Random();

            await Task.Delay(TimeSpan.FromSeconds(10));

            return Enumerable.Range(1, 5).Select(index => new WeatherForecast
            {
                Date = startDate.AddDays(index),
                TemperatureC = rng.Next(-20, 55),
                Summary = Summaries[rng.Next(Summaries.Length)]
            }).ToArray();
        });
    }
}
```

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a>Representación de componentes interactivos con estado desde vistas y páginas de Razor

Los componentes interactivos con estado se pueden agregar a una página o vista de Razor.

Cuando se representa la página o la vista:

* El componente se representará con la página o la vista.
* Se pierde el estado inicial del componente usado para la representación previa.
* Cuando se establece la conexión SignalR, se crea un nuevo estado del componente.

La siguiente página de Razor representa un componente de `Counter`:

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a>Representación de componentes no interactivos desde páginas y vistas de Razor

En la siguiente página de Razor, el componente de `Counter` se representa estáticamente con un valor inicial que se especifica mediante un formulario:

```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>

<component type="typeof(Counter)" render-mode="Static" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

Como `MyComponent` se representa estáticamente, el componente no puede ser interactivo.

### <a name="detect-when-the-app-is-prerendering"></a>Detectar cuándo se está preprocesando la aplicación

[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a>Configuración del cliente de SignalR para aplicaciones de Blazor Server

A veces, debe configurar el cliente de SignalR que usan las aplicaciones de Blazor Server. Por ejemplo, puede que desee configurar el registro en el SignalR cliente para diagnosticar un problema de conexión.

Para configurar el cliente de SignalR en el archivo *pages/_Host. cshtml* :

* Agregue un atributo `autostart="false"` a la etiqueta de `<script>` para el script de `blazor.server.js`.
* Llame a `Blazor.start` y pase un objeto de configuración que especifique el generador de SignalR.

```html
<script src="_framework/blazor.server.js" autostart="false"></script>
<script>
  Blazor.start({
    configureSignalR: function (builder) {
      builder.configureLogging("information"); // LogLevel.Information
    }
  });
</script>
```

## <a name="additional-resources"></a>Recursos adicionales

* <xref:blazor/get-started>
* <xref:signalr/introduction>
