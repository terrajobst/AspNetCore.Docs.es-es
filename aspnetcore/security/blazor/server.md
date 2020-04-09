---
title: Aplicaciones seguras Blazor de ASP.NET Core Server
author: guardrex
description: Obtenga información sobre cómo Blazor mitigar las amenazas de seguridad de las aplicaciones de servidor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/02/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/server
ms.openlocfilehash: bd03f811d0425fdfdb7bbbc24fea5481b49b8ed3
ms.sourcegitcommit: 9675db7bf4b67ae269f9226b6f6f439b5cce4603
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2020
ms.locfileid: "80626016"
---
# <a name="secure-aspnet-core-blazor-server-apps"></a>Aplicaciones seguras ASP.NET Core Blazor Server

Por [Javier Calvarro Nelson](https://github.com/javiercn)

Las aplicaciones de Blazor Server adoptan un modelo de procesamiento de datos *con estado,* donde el servidor y el cliente mantienen una relación de larga duración. El estado persistente es mantenido por un [circuito,](xref:blazor/state-management)que puede abarcar conexiones que también son potencialmente longevas.

Cuando un usuario visita un sitio de Blazor Server, el servidor crea un circuito en la memoria del servidor. El circuito indica al explorador qué contenido representar y responde a eventos, como cuando el usuario selecciona un botón en la interfaz de usuario. Para realizar estas acciones, un circuito invoca funciones JavaScript en el explorador del usuario y métodos .NET en el servidor. Esta interacción bidireccional basada en JavaScript se conoce como [interoperabilidad de JavaScript (interoperabilidad JS).](xref:blazor/call-javascript-from-dotnet)

Dado que la interoperabilidad de JS se produce a través de Internet y el cliente utiliza un explorador remoto, las aplicaciones de Blazor Server comparten la mayoría de los problemas de seguridad de las aplicaciones web. En este tema se describen las amenazas comunes a las aplicaciones de Blazor Server y se proporcionan instrucciones sobre la mitigación de amenazas centradas en las aplicaciones orientadas a Internet.

En entornos restringidos, como dentro de redes corporativas o intranets, algunas de las instrucciones de mitigación:

* No se aplica en el entorno restringido.
* No vale la pena el costo de implementar porque el riesgo de seguridad es bajo en un entorno restringido.

## <a name="blazor-server-project-template"></a>Plantilla de proyecto de Blazor Server

La plantilla de proyecto DeBlazor Server se puede configurar para la autenticación cuando se crea el proyecto.

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

Siga las instrucciones de Visual Studio que se indican en el artículo <xref:blazor/get-started> para crear un proyecto de servidor Blazor con un mecanismo de autenticación.

Después de elegir la plantilla **Aplicación de servidor Blazor** en el cuadro de diálogo **Crear una aplicación web ASP.NET Core**, seleccione **Cambiar** en **Autenticación**.

Se abre un cuadro de diálogo para ofrecer el mismo conjunto de mecanismos de autenticación disponibles para otros proyectos ASP.NET Core:

* **Sin autenticación**
* **Cuentas de usuario individuales** &ndash;Las cuentas de usuario se pueden almacenar:
  * Dentro de la aplicación mediante el sistema de [identidad](xref:security/authentication/identity) de ASP.NET Core.
  * Con [Azure AD B2C](xref:security/authentication/azure-ad-b2c).
* **Cuentas de trabajo o escolares**
* **Autenticación de Windows**

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Siga las instrucciones de Visual Studio Code que se indican en el artículo <xref:blazor/get-started> para crear un proyecto de servidor Blazor con un mecanismo de autenticación:

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

En la tabla siguiente se muestran los valores de autenticación permitidos (`{AUTHENTICATION}`).

| Mecanismo de autenticación                                                                 | Valor de `{AUTHENTICATION}` |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| Sin autenticación                                                                        | `None`                   |
| Individual<br>Usuarios almacenados en la aplicación con la identidad de ASP.NET Core.                        | `Individual`             |
| Individual<br>Usuarios almacenados en [Azure AD B2C](xref:security/authentication/azure-ad-b2c). | `IndividualB2C`          |
| Cuentas profesionales o educativas<br>Autenticación organizativa para un solo inquilino.            | `SingleOrg`              |
| Cuentas profesionales o educativas<br>Autenticación organizativa para varios inquilinos.           | `MultiOrg`               |
| Autenticación de Windows                                                                   | `Windows`                |

El comando crea una carpeta con el valor proporcionado para el marcador de posición `{APP NAME}` y usa el nombre de la carpeta como nombre de la aplicación. Para más información, consulte el comando [dotnet new](/dotnet/core/tools/dotnet-new) de la guía de .NET Core.

# <a name="visual-studio-for-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

1. Siga las instrucciones de Visual <xref:blazor/get-started> Studio para Mac en el artículo.

1. En el paso Configurar la nueva aplicación de **servidor de Blazor,** seleccione **Autenticación individual (en la aplicación)** en el menú desplegable **Autenticación.**

1. La aplicación se crea para usuarios individuales almacenados en la aplicación con ASP.NET identidad principal.

# <a name="net-core-cli"></a>[CLI de .NET Core](#tab/netcore-cli/)

Siga las instrucciones de la <xref:blazor/get-started> CLI de .NET Core del artículo para crear un nuevo proyecto de Blazor Server con un mecanismo de autenticación:

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

En la tabla siguiente se muestran los valores de autenticación permitidos (`{AUTHENTICATION}`).

| Mecanismo de autenticación                                                                 | Valor de `{AUTHENTICATION}` |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| Sin autenticación                                                                        | `None`                   |
| Individual<br>Usuarios almacenados en la aplicación con la identidad de ASP.NET Core.                        | `Individual`             |
| Individual<br>Usuarios almacenados en [Azure AD B2C](xref:security/authentication/azure-ad-b2c). | `IndividualB2C`          |
| Cuentas profesionales o educativas<br>Autenticación organizativa para un solo inquilino.            | `SingleOrg`              |
| Cuentas profesionales o educativas<br>Autenticación organizativa para varios inquilinos.           | `MultiOrg`               |
| Autenticación de Windows                                                                   | `Windows`                |

El comando crea una carpeta con el valor proporcionado para el marcador de posición `{APP NAME}` y usa el nombre de la carpeta como nombre de la aplicación. Para más información, consulte el comando [dotnet new](/dotnet/core/tools/dotnet-new) de la guía de .NET Core.

---

## <a name="pass-tokens-to-a-blazor-server-app"></a>Pasar tokens a una aplicación Blazor Server

Autentique la aplicación Blazor Server como lo haría con una aplicación normal de Razor Pages o MVC. Aprovisione y guarde los tokens en la cookie de autenticación. Por ejemplo:

```csharp
using Microsoft.AspNetCore.Authentication.OpenIdConnect;

...

services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
{
    options.ResponseType = "code";
    options.SaveTokens = true;

    options.Scope.Add("offline_access");
    options.Scope.Add("{SCOPE}");
    options.Resource = "{RESOURCE}";
});
```

Para obtener código de `Startup.ConfigureServices` ejemplo, incluido un ejemplo completo, consulte [Pasar tokens a una aplicación Blazor del lado servidor.](https://github.com/javiercn/blazor-server-aad-sample)

Defina una clase para pasar el estado inicial de la aplicación con los tokens de acceso y actualización:

```csharp
public class InitialApplicationState
{
    public string AccessToken { get; set; }
    public string RefreshToken { get; set; }
}
```

Defina un servicio de proveedor de tokens **con ámbito** que se pueda usar dentro de la aplicación Blazor para resolver los tokens desde DI:

```csharp
using System;
using System.Security.Claims;
using System.Threading.Tasks;

public class TokenProvider
{
    public string AccessToken { get; set; }
    public string RefreshToken { get; set; }
}
```

En `Startup.ConfigureServices`, agregue servicios para:

* `IHttpClientFactory`
* `TokenProvider`

```csharp
services.AddHttpClient();
services.AddScoped<TokenProvider>();
```

En el archivo *_Host.cshtml,* `InitialApplicationState` cree e instancia y páselo como parámetro a la aplicación:

```cshtml
@using Microsoft.AspNetCore.Authentication

...

@{
    var tokens = new InitialApplicationState
    {
        AccessToken = await HttpContext.GetTokenAsync("access_token"),
        RefreshToken = await HttpContext.GetTokenAsync("refresh_token")
    };
}

<app>
    <component type="typeof(App)" param-InitialState="tokens" 
        render-mode="ServerPrerendered" />
</app>
```

En `App` el componente (*App.razor*), resuelva el servicio e inicialícelo con los datos del parámetro:

```razor
@inject TokenProvider TokensProvider

...

@code {
    [Parameter]
    public InitialApplicationState InitialState { get; set; }

    protected override Task OnInitializedAsync()
    {
        TokensProvider.AccessToken = InitialState.AccessToken;
        TokensProvider.RefreshToken = InitialState.RefreshToken;

        return base.OnInitializedAsync();
    }
}
```

En el servicio que realiza una solicitud de API segura, inserte el proveedor de tokens y recupere el token para llamar a la API:

```csharp
public class WeatherForecastService
{
    private readonly TokenProvider _store;

    public WeatherForecastService(IHttpClientFactory clientFactory, 
        TokenProvider tokenProvider)
    {
        Client = clientFactory.CreateClient();
        _store = tokenProvider;
    }

    public HttpClient Client { get; }

    public async Task<WeatherForecast[]> GetForecastAsync(DateTime startDate)
    {
        var token = _store.AccessToken;
        var request = new HttpRequestMessage(HttpMethod.Get, 
            "https://localhost:5003/WeatherForecast");
        request.Headers.Add("Authorization", $"Bearer {token}");
        var response = await Client.SendAsync(request);
        response.EnsureSuccessStatusCode();

        return await response.Content.ReadAsAsync<WeatherForecast[]>();
    }
}
```

## <a name="resource-exhaustion"></a>Agotamiento de recursos

El agotamiento de recursos puede producirse cuando un cliente interactúa con el servidor y hace que el servidor consuma recursos excesivos. El consumo excesivo de recursos afecta principalmente a:

* [Cpu](#cpu)
* [Memoria](#memory)
* [Conexiones de cliente](#client-connections)

Los ataques de denegación de servicio (DoS) suelen intentar agotar los recursos de una aplicación o servidor. Sin embargo, el agotamiento de recursos no es necesariamente el resultado de un ataque al sistema. Por ejemplo, los recursos finitos se pueden agotar debido a la alta demanda de los usuarios. DoS se cubre más adelante en la sección [ataques de denegación de servicio (DoS).](#denial-of-service-dos-attacks)

Los recursos externos al marco de Blazor, como bases de datos y identificadores de archivos (utilizados para leer y escribir archivos), también pueden experimentar agotamiento de recursos. Para obtener más información, vea <xref:performance/performance-best-practices>.

### <a name="cpu"></a>CPU

El agotamiento de la CPU puede ocurrir cuando uno o más clientes fuerzan al servidor a realizar un trabajo intensivo de CPU.

Por ejemplo, considere una aplicación Blazor Server que calcula un número de *Fibonnacci.* Un número de Fibonnacci se produce a partir de una secuencia de Fibonnacci, donde cada número de la secuencia es la suma de los dos números anteriores. La cantidad de trabajo necesario para llegar a la respuesta depende de la longitud de la secuencia y del tamaño del valor inicial. Si la aplicación no pone límites a la solicitud de un cliente, los cálculos de uso intensivo de CPU pueden dominar el tiempo de la CPU y disminuir el rendimiento de otras tareas. El consumo excesivo de recursos es un problema de seguridad que afecta a la disponibilidad.

El agotamiento de la CPU es una preocupación para todas las aplicaciones orientadas al público. En las aplicaciones web normales, las solicitudes y conexiones agotan el tiempo de espera como medida de seguridad, pero las aplicaciones de Blazor Server no proporcionan las mismas medidas de seguridad. Las aplicaciones de Blazor Server deben incluir comprobaciones y límites adecuados antes de realizar un trabajo con un uso intensivo de la CPU.

### <a name="memory"></a>Memoria

El agotamiento de la memoria puede producirse cuando uno o más clientes obligan al servidor a consumir una gran cantidad de memoria.

Por ejemplo, considere una aplicación lateral De Blazor-server con un componente que acepte y muestre una lista de elementos. Si la aplicación Blazor no pone límites en el número de elementos permitidos o el número de elementos representados en el cliente, el procesamiento y la representación que consumen mucha memoria pueden dominar la memoria del servidor hasta el punto en que el rendimiento del servidor se ve afectado. El servidor puede bloquearse o ralentizarse hasta el punto de que parece haberse bloqueado.

Considere el siguiente escenario para mantener y mostrar una lista de elementos que pertenecen a un escenario de agotamiento de memoria potencial en el servidor:

* Los elementos `List<MyItem>` de una propiedad o campo utilizan la memoria del servidor. Si la aplicación permite que la lista de elementos crezca sin límites, existe el riesgo de que el servidor se quede sin memoria. Si se quede sin memoria, la sesión actual finaliza (se bloquea) y todas las sesiones simultáneas de esa instancia de servidor reciben una excepción de memoria insuficiente. Para evitar que se produzca este escenario, la aplicación debe usar una estructura de datos que imponga un límite de elementos a los usuarios simultáneos.
* Si no se usa un esquema de paginación para la representación, el servidor usa memoria adicional para los objetos que no están visibles en la interfaz de usuario. Sin un límite en el número de elementos, las demandas de memoria pueden agotar la memoria del servidor disponible. Para evitar este escenario, utilice uno de los siguientes enfoques:
  * Utilice listas paginadas al renderizar.
  * Muestre solo los primeros 100 a 1.000 elementos y requiera que el usuario introduzca criterios de búsqueda para buscar elementos más allá de los elementos mostrados.
  * Para un escenario de representación más avanzado, implemente listas o cuadrículas que admitan *la virtualización.* Mediante la virtualización, las listas solo representan un subconjunto de elementos visibles actualmente para el usuario. Cuando el usuario interactúa con la barra de desplazamiento en la interfaz de usuario, el componente representa solo los elementos necesarios para la visualización. Los elementos que actualmente no son necesarios para la visualización se pueden mantener en almacenamiento secundario, que es el enfoque ideal. Los elementos no mostrados también se pueden mantener en la memoria, lo que es menos ideal.

Las aplicaciones de Blazor Server ofrecen un modelo de programación similar a otros marcos de interfaz de usuario para aplicaciones con estado, como WPF, Windows Forms o Blazor WebAssembly. La principal diferencia es que en varios de los marcos de interfaz de usuario la memoria consumida por la aplicación pertenece al cliente y solo afecta a ese cliente individual. Por ejemplo, una aplicación Blazor WebAssembly se ejecuta completamente en el cliente y solo usa recursos de memoria de cliente. En el escenario De blazor Server, la memoria consumida por la aplicación pertenece al servidor y se comparte entre los clientes de la instancia del servidor.

Las demandas de memoria del lado del servidor son una consideración para todas las aplicaciones de Blazor Server. Sin embargo, la mayoría de las aplicaciones web no tienen estado y la memoria utilizada durante el procesamiento de una solicitud se libera cuando se devuelve la respuesta. Como recomendación general, no permita que los clientes asignen una cantidad de memoria sin enlazar como en cualquier otra aplicación del lado servidor que conserve las conexiones de cliente. La memoria consumida por una aplicación Blazor Server persiste durante más tiempo que una sola solicitud.

> [!NOTE]
> Durante el desarrollo, se puede utilizar un generador de perfiles o un seguimiento capturado para evaluar las demandas de memoria de los clientes. Un generador de perfiles o un seguimiento no capturará la memoria asignada a un cliente específico. Para capturar el uso de memoria de un cliente específico durante el desarrollo, capture un volcado y examine la demanda de memoria de todos los objetos rooteados en el circuito de un usuario.

### <a name="client-connections"></a>Conexiones de cliente

El agotamiento de la conexión puede producirse cuando uno o más clientes abren demasiadas conexiones simultáneas con el servidor, lo que impide que otros clientes establezcan nuevas conexiones.

Los clientes de Blazor establecen una sola conexión por sesión y mantienen la conexión abierta mientras la ventana del explorador esté abierta. Las exigencias en el servidor de mantener todas las conexiones no es específica de las aplicaciones Blazor. Dada la naturaleza persistente de las conexiones y la naturaleza con estado de las aplicaciones de Blazor Server, el agotamiento de la conexión es un mayor riesgo para la disponibilidad de la aplicación.

De forma predeterminada, no hay límite en el número de conexiones por usuario para una aplicación de servidor de Blazor. Si la aplicación requiere un límite de conexión, tome uno o varios de los siguientes enfoques:

* Requiere autenticación, que naturalmente limita la capacidad de los usuarios no autorizados para conectarse a la aplicación. Para que este escenario sea eficaz, se debe impedir que los usuarios aprovisionen nuevos usuarios a voluntad.
* Limite el número de conexiones por usuario. La limitación de conexiones se puede realizar mediante los siguientes enfoques. Tenga cuidado para permitir que los usuarios legítimos accedan a la aplicación (por ejemplo, cuando se establece un límite de conexión basado en la dirección IP del cliente).
  * En el nivel de aplicación:
    * Extensibilidad de enrutamiento de punto final.
    * Requerir autenticación para conectarse a la aplicación y realizar un seguimiento de las sesiones activas por usuario.
    * Rechazar nuevas sesiones al llegar a un límite.
    * Proxy WebSocket se condatará a una aplicación mediante el uso de un proxy, como el [servicio Azure SignalR](/azure/azure-signalr/signalr-overview) que multiplexa las conexiones de los clientes a una aplicación. Esto proporciona una aplicación con mayor capacidad de conexión que un solo cliente puede establecer, lo que impide que un cliente agote las conexiones con el servidor.
  * En el nivel de servidor: use un proxy/puerta de enlace delante de la aplicación. Por ejemplo, [Azure Front Door](/azure/frontdoor/front-door-overview) le permite definir, administrar y supervisar el enrutamiento global del tráfico web a una aplicación.

## <a name="denial-of-service-dos-attacks"></a>Ataques de denegación de servicio (DoS)

Los ataques de denegación de servicio (DoS) implican que un cliente agote uno o varios de sus recursos haciendo que la aplicación no esté disponible. Las aplicaciones de Blazor Server incluyen algunos límites predeterminados y dependen de otros ASP.NET límites Core y SignalR para protegerse contra ataques DoS:

| Límite de aplicaciones de Blazor Server                            | Descripción | Valor predeterminado |
| ------------------------------------------------------- | ----------- | ------- |
| `CircuitOptions.DisconnectedCircuitMaxRetained`         | Número máximo de circuitos desconectados que un servidor determinado tiene en la memoria a la vez. | 100 |
| `CircuitOptions.DisconnectedCircuitRetentionPeriod`     | La cantidad máxima de tiempo que un circuito desconectado se mantiene en la memoria antes de ser derribado. | 3 minutos |
| `CircuitOptions.JSInteropDefaultCallTimeout`            | Cantidad máxima de tiempo que el servidor espera antes de agotar el tiempo de espera de una invocación de función JavaScript asincrónica. | 1 minuto. |
| `CircuitOptions.MaxBufferedUnacknowledgedRenderBatches` | Número máximo de lotes de renderización no reconocidos que el servidor mantiene en memoria por circuito en un momento dado para admitir la reconexión robusta. Después de alcanzar el límite, el servidor deja de producir nuevos lotes de renderización hasta que el cliente haya reconocido uno o más lotes. | 10 |


| Límite de SignalR y ASP.NET Core             | Descripción | Valor predeterminado |
| ------------------------------------------ | ----------- | ------- |
| `CircuitOptions.MaximumReceiveMessageSize` | Tamaño del mensaje para un mensaje individual. | 32 KB |

## <a name="interactions-with-the-browser-client"></a>Interacciones con el navegador (cliente)

Un cliente interactúa con el servidor a través de la distribución de eventos de interoperabilidad JS y la finalización de la representación. La comunicación de interoperabilidad de JS va en ambos sentidos entre JavaScript y .NET:

* Los eventos del explorador se distribuyen desde el cliente al servidor de forma asincrónica.
* El servidor responde de forma asincrónica rerepresentando la interfaz de usuario según sea necesario.

### <a name="javascript-functions-invoked-from-net"></a>Funciones JavaScript invocadas desde .NET

Para llamadas desde métodos .NET a JavaScript:

* Todas las invocaciones tienen un tiempo de espera <xref:System.OperationCanceledException> configurable después del cual fallan, devolviendo un al llamador.
  * Hay un tiempo de espera`CircuitOptions.JSInteropDefaultCallTimeout`predeterminado para las llamadas ( ) de un minuto. Para configurar este <xref:blazor/call-javascript-from-dotnet#harden-js-interop-calls>límite, consulte .
  * Se puede proporcionar un token de cancelación para controlar la cancelación por llamada. Confíe en el tiempo de espera de llamada predeterminado siempre que sea posible y de cualquier llamada al cliente si se proporciona un token de cancelación.
* No se puede confiar en el resultado de una llamada de JavaScript. El Blazor cliente de aplicación que se ejecuta en el explorador busca la función JavaScript que se va a invocar. Se invoca la función y se genera el resultado o un error. Un cliente malintencionado puede intentar:
  * Causa un problema en la aplicación devolviendo un error de la función JavaScript.
  * Inducir un comportamiento no deseado en el servidor devolviendo un resultado inesperado de la función JavaScript.

Tome las siguientes precauciones para protegerse contra los escenarios anteriores:

* Ajuste las llamadas de interoperabilidad de JS dentro de instrucciones [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) para tener en cuenta los errores que pueden producirse durante las invocaciones. Para obtener más información, vea <xref:blazor/handle-errors#javascript-interop>.
* Validar los datos devueltos por invocaciones de interoperabilidad de JS, incluidos los mensajes de error, antes de realizar cualquier acción.

### <a name="net-methods-invoked-from-the-browser"></a>Métodos .NET invocados desde el explorador

No confíe en las llamadas de JavaScript a métodos .NET. Cuando un método .NET se expone a JavaScript, tenga en cuenta cómo se invoca el método .NET:

* Tratar cualquier método .NET expuesto a JavaScript como lo haría con un punto de conexión público a la aplicación.
  * Validar entrada.
    * Asegúrese de que los valores están dentro de los rangos esperados.
    * Asegúrese de que el usuario tiene permiso para realizar la acción solicitada.
  * No asigne una cantidad excesiva de recursos como parte de la invocación del método .NET. Por ejemplo, realice comprobaciones y coloque límites en el uso de CPU y memoria.
  * Tenga en cuenta que los métodos estáticos y de instancia se pueden exponer a clientes JavaScript. Evite compartir el estado entre sesiones a menos que el diseño llame al estado de uso compartido con las restricciones adecuadas.
    * Por ejemplo, `DotNetReference` los métodos expuestos a través de objetos creados originalmente a través de la inserción de dependencias (DI), los objetos deben registrarse como objetos con ámbito. Esto se aplica a Blazor cualquier servicio di que use la aplicación De servidor.
    * Para los métodos estáticos, evite establecer un estado que no se pueda limitar al cliente a menos que la aplicación comparta explícitamente el estado por diseño entre todos los usuarios de una instancia de servidor.
  * Evite pasar datos proporcionados por el usuario en parámetros a llamadas JavaScript. Si es absolutamente necesario pasar datos en parámetros, asegúrese de que el código JavaScript controla el paso de los datos sin introducir vulnerabilidades de secuencias de comandos entre sitios [(XSS).](#cross-site-scripting-xss) Por ejemplo, no escriba datos proporcionados por el usuario en `innerHTML` el modelo de objetos de documento (DOM) estableciendo la propiedad de un elemento. Considere la posibilidad de usar `eval` Content Security Policy [(CSP)](https://developer.mozilla.org/docs/Web/HTTP/CSP) para deshabilitar y otros primitivos de JavaScript no seguros.
* Evite implementar la distribución personalizada de invocaciones de .NET además de la implementación de distribución del marco de trabajo. La exposición de métodos .NET al explorador Blazor es un escenario avanzado, no se recomienda para el desarrollo general.

### <a name="events"></a>Events

Los eventos proporcionan un Blazor punto de entrada a una aplicación de servidor. Las mismas reglas para proteger los puntos de Blazor conexión en aplicaciones web se aplican al control de eventos en las aplicaciones de servidor. Un cliente malintencionado puede enviar los datos que desea enviar como carga útil para un evento.

Por ejemplo:

* Un evento de `<select>` cambio para un podría enviar un valor que no está dentro de las opciones que la aplicación presentó al cliente.
* Un `<input>` podría enviar cualquier dato de texto al servidor, omitiendo la validación del lado cliente.

La aplicación debe validar los datos de cualquier evento que controle la aplicación. El Blazor marco de trabajo forma que los [componentes](xref:blazor/forms-validation) realicen validaciones básicas. Si la aplicación usa componentes de formularios personalizados, se debe escribir código personalizado para validar los datos de eventos según corresponda.

BlazorLos eventos de servidor son asincrónicos, por lo que se pueden distribuir varios eventos al servidor antes de que la aplicación tenga tiempo para reaccionar produciendo un nuevo procesamiento. Esto tiene algunas implicaciones de seguridad a tener en cuenta. Limitar las acciones de cliente en la aplicación debe realizarse dentro de los controladores de eventos y no depender del estado de vista representado actual.

Considere un componente de contador que debe permitir a un usuario incrementar un contador un máximo de tres veces. El botón para incrementar el contador se `count`basa condicionalmente en el valor de:

```razor
<p>Count: @count<p>

@if (count < 3)
{
    <button @onclick="IncrementCount" value="Increment count" />
}

@code 
{
    private int count = 0;

    private void IncrementCount()
    {
        count++;
    }
}
```

Un cliente puede distribuir uno o varios eventos de incremento antes de que el marco de trabajo genere una nueva representación de este componente. El resultado es `count` que el usuario puede incrementarse más de *tres veces* porque la interfaz de usuario no quita el botón lo suficientemente rápido. En el ejemplo siguiente se `count` muestra la forma correcta de alcanzar el límite de tres incrementos:

```razor
<p>Count: @count<p>

@if (count < 3)
{
    <button @onclick="IncrementCount" value="Increment count" />
}

@code 
{
    private int count = 0;

    private void IncrementCount()
    {
        if (count < 3)
        {
            count++;
        }
    }
}
```

Al agregar `if (count < 3) { ... }` la comprobación dentro del `count` controlador, la decisión de incrementar se basa en el estado actual de la aplicación. La decisión no se basa en el estado de la interfaz de usuario como estaba en el ejemplo anterior, que podría estar temporalmente obsoleto.

### <a name="guard-against-multiple-dispatches"></a>Protegerse contra múltiples despachos

Si una devolución de llamada de evento invoca una operación de larga ejecución de forma asincrónica, como la obtención de datos de un servicio externo o una base de datos, considere la posibilidad de usar una protección. La protección puede impedir que el usuario haga cola varias operaciones mientras la operación está en curso con comentarios visuales. El siguiente código `isLoading` `true` de `GetForecastAsync` componente se establece en while obtiene datos del servidor. `isLoading` Mientras `true`está , el botón está deshabilitado en la interfaz de usuario:

```razor
@page "/fetchdata"
@using BlazorServerSample.Data
@inject WeatherForecastService ForecastService

<button disabled="@isLoading" @onclick="UpdateForecasts">Update</button>

@code {
    private bool isLoading;
    private WeatherForecast[] forecasts;

    private async Task UpdateForecasts()
    {
        if (!isLoading)
        {
            isLoading = true;
            forecasts = await ForecastService.GetForecastAsync(DateTime.Now);
            isLoading = false;
        }
    }
}
```

El patrón de protección demostrado en el ejemplo anterior funciona `async` - `await` si la operación en segundo plano se ejecuta de forma asincrónica con el patrón.

### <a name="cancel-early-and-avoid-use-after-dispose"></a>Cancele a tiempo y evite el uso después de desechar

Además de usar un protector como se describe en <xref:System.Threading.CancellationToken> la sección Proteger contra varios [envíos,](#guard-against-multiple-dispatches) considere la posibilidad de usar a para cancelar operaciones de ejecución prolongada cuando se elimina el componente. Este enfoque tiene la ventaja añadida de evitar el *uso después de la eliminación* en los componentes:

```razor
@implements IDisposable

...

@code {
    private readonly CancellationTokenSource TokenSource = 
        new CancellationTokenSource();

    private async Task UpdateForecasts()
    {
        ...

        forecasts = await ForecastService.GetForecastAsync(DateTime.Now, 
            TokenSource.Token);

        if (TokenSource.Token.IsCancellationRequested)
        {
           return;
        }

        ...
    }

    public void Dispose()
    {
        TokenSource.Cancel();
    }
}
```

### <a name="avoid-events-that-produce-large-amounts-of-data"></a>Evite eventos que produzcan grandes cantidades de datos

Algunos eventos DOM, `oninput` `onscroll`como o , pueden producir una gran cantidad de datos. Evite usar estos Blazor eventos en aplicaciones de servidor.

## <a name="additional-security-guidance"></a>Orientación de seguridad adicional

Las instrucciones para proteger ASP.NET aplicaciones Blazor principales se aplican a las aplicaciones de servidor y se tratan en las secciones siguientes:

* [Registro y datos confidenciales](#logging-and-sensitive-data)
* [Proteger la información en tránsito con HTTPS](#protect-information-in-transit-with-https)
* [Scripting entre sitios (XSS)](#cross-site-scripting-xss))
* [Protección entre orígenes](#cross-origin-protection)
* [Click-jacking](#click-jacking)
* [Redirecciones abiertas](#open-redirects)

### <a name="logging-and-sensitive-data"></a>Registro y datos confidenciales

Las interacciones de interoperabilidad de JS entre el <xref:Microsoft.Extensions.Logging.ILogger> cliente y el servidor se registran en los registros del servidor con instancias. Blazorevita el registro de información confidencial, como eventos reales o entradas y salidas de interoperabilidad JS.

Cuando se produce un error en el servidor, el marco de trabajo notifica al cliente y derriba la sesión. De forma predeterminada, el cliente recibe un mensaje de error genérico que se puede ver en las herramientas de desarrollador del explorador.

El error del lado cliente no incluye la pila de llamadas y no proporciona detalles sobre la causa del error, pero los registros del servidor contienen dicha información. Para fines de desarrollo, la información de error confidencial se puede poner a disposición del cliente habilitando errores detallados.

Habilite errores detallados con:

* `CircuitOptions.DetailedErrors`.
* `DetailedErrors`clave de configuración. Por ejemplo, `ASPNETCORE_DETAILEDERRORS` establezca la variable `true`de entorno en un valor de .

> [!WARNING]
> La exposición de información de error a los clientes en Internet es un riesgo de seguridad que siempre debe evitarse.

### <a name="protect-information-in-transit-with-https"></a>Proteger la información en tránsito con HTTPS

BlazorEl SignalR servidor se utiliza para la comunicación entre el cliente y el servidor. BlazorServer normalmente usa SignalR el transporte que negocia, que suele ser WebSockets.

BlazorEl servidor no garantiza la integridad y confidencialidad de los datos enviados entre el servidor y el cliente. Utilice siempre HTTPS.

### <a name="cross-site-scripting-xss"></a>Scripting entre sitios (XSS)

El scripting entre sitios (XSS) permite a una parte no autorizada ejecutar lógica arbitraria en el contexto del explorador. Una aplicación comprometida podría ejecutar código arbitrario en el cliente. La vulnerabilidad podría utilizarse para realizar potencialmente una serie de acciones maliciosas contra el servidor:

* Enviar eventos falsos/no válidos al servidor.
* Finalizaciones de renderización no válidas o con errores de envío.
* Evite enviar las terminaciones de renderización.
* Distribuir llamadas de interoperabilidad de JavaScript a .NET.
* Modifique la respuesta de las llamadas de interoperabilidad de .NET a JavaScript.
* Evite distribuir resultados de interoperabilidad de .NET a JS.

El Blazor marco de trabajo del servidor toma medidas para protegerse contra algunas de las amenazas anteriores:

* Deja de producir nuevas actualizaciones de la interfaz de usuario si el cliente no reconoce lotes de representación. Configurado `CircuitOptions.MaxBufferedUnacknowledgedRenderBatches`con .
* Agota el tiempo de espera de cualquier llamada de .NET a JavaScript después de un minuto sin recibir una respuesta del cliente. Configurado `CircuitOptions.JSInteropDefaultCallTimeout`con .
* Realiza la validación básica en todas las entradas procedentes del navegador durante la interoperabilidad de JS:
  * Las referencias de .NET son válidas y del tipo esperado por el método .NET.
  * Los datos no están mal formados.
  * El número correcto de argumentos para el método está presente en la carga.
  * Los argumentos o el resultado se pueden deserializar correctamente antes de invocar el método.
* Realiza la validación básica en todas las entradas procedentes del explorador de eventos distribuidos:
  * El evento tiene un tipo válido.
  * Los datos del evento se pueden deserializar.
  * Hay un controlador de eventos asociado al evento.

Además de las protecciones que implementa el marco de trabajo, el desarrollador debe codificar la aplicación para protegerse contra las amenazas y tomar las medidas adecuadas:

* Valide siempre los datos al controlar eventos.
* Tome las medidas apropiadas al recibir datos no válidos:
  * Ignore los datos y vuelva. Esto permite que la aplicación continúe procesando las solicitudes.
  * Si la aplicación determina que la entrada es ilegítima y no puede ser producida por el cliente legítimo, inicie una excepción. Lanzar una excepción derriba el circuito y finaliza la sesión.
* No confíe en el mensaje de error proporcionado por las terminaciones de lote de procesamiento incluidas en los registros. El cliente proporciona el error y, por lo general, no se puede confiar en el los que se puede confiar, ya que el cliente podría verse comprometido.
* No confíe en la entrada de las llamadas de interoperabilidad de JS en cualquier dirección entre los métodos JavaScript y .NET.
* La aplicación es responsable de validar que el contenido de los argumentos y los resultados son válidos, incluso si los argumentos o resultados se deserializan correctamente.

Para que exista una vulnerabilidad XSS, la aplicación debe incorporar la entrada del usuario en la página representada. BlazorLos componentes del servidor ejecutan un paso en tiempo de compilación en el que el marcado de un archivo *.razor* se transforma en lógica de procedimiento de C. En tiempo de ejecución, la lógica de C- crea un árbol de *representación* que describe los elementos, el texto y los componentes secundarios. Esto se aplica al DOM del navegador a través de una secuencia de instrucciones de JavaScript (o se serializa en HTML en el caso de preprocesamiento):

* La entrada del usuario representada a `@someStringValue`través de la sintaxis normal de Razor (por ejemplo, ) no expone una vulnerabilidad XSS porque la sintaxis de Razor se agrega al DOM a través de comandos que solo pueden escribir texto. Incluso si el valor incluye marcado HTML, el valor se muestra como texto estático. Al prerenderizar, la salida está codificada en HTML, que también muestra el contenido como texto estático.
* Las etiquetas de script no están permitidas y no deben incluirse en el árbol de procesamiento de componentes de la aplicación. Si se incluye una etiqueta de script en el marcado de un componente, se genera un error en tiempo de compilación.
* Los autores de componentes pueden crear componentes en C- sin usar Razor. El autor del componente es responsable de utilizar las API correctas al emitir la salida. Por ejemplo, `builder.AddContent(0, someUserSuppliedString)` use y no , *ya* `builder.AddMarkupContent(0, someUserSuppliedString)`que este último podría crear una vulnerabilidad XSS.

Como parte de la protección contra ataques XSS, considere la posibilidad de implementar mitigaciones XSS, como la directiva de seguridad de contenido [(CSP).](https://developer.mozilla.org/docs/Web/HTTP/CSP)

Para obtener más información, vea <xref:security/cross-site-scripting>.

### <a name="cross-origin-protection"></a>Protección entre orígenes

Los ataques entre orígenes implican a un cliente de un origen diferente que realiza una acción contra el servidor. La acción maliciosa suele ser una solicitud GET o un formulario POST (Cross-Site Request Forgery, CSRF), pero también es posible abrir un WebSocket malintencionado. BlazorLas aplicaciones de servidor ofrecen [las mismas garantías que cualquier otra SignalR aplicación que utilice el protocolo hub ofrece:](xref:signalr/security)

* BlazorSe puede acceder a las aplicaciones de servidor entre orígenes a menos que se tomen medidas adicionales para evitarlo. Para deshabilitar el acceso entre orígenes, deshabilite CORS en el punto de `DisableCorsAttribute` conexión Blazor agregando el middleware CORS a la canalización y agregando los metadatos del punto de conexión o limite el conjunto de orígenes permitidos [configurando SignalR el uso compartido](xref:signalr/security#cross-origin-resource-sharing)de recursos entre orígenes.
* Si CORS está habilitado, es posible que se necesiten pasos adicionales para proteger la aplicación en función de la configuración de CORS. Si CORS está habilitado globalmente, CORS Blazor se puede `DisableCorsAttribute` deshabilitar para el concentrador `hub.MapBlazorHub()`de servidor agregando los metadatos a los metadatos del punto de conexión después de llamar a .

Para obtener más información, vea <xref:security/anti-request-forgery>.

### <a name="click-jacking"></a>Click-jacking

El click-jacking implica representar `<iframe>` un sitio como un sitio dentro de un origen diferente con el fin de engañar al usuario para que realice acciones en el sitio bajo ataque.

Para proteger una aplicación de `<iframe>`la representación dentro de `X-Frame-Options` una , use la directiva de seguridad de contenido [(CSP)](https://developer.mozilla.org/docs/Web/HTTP/CSP) y el encabezado. Para obtener más información, consulte Documentos web de [MDN: X-Frame-Options](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-Frame-Options).

### <a name="open-redirects"></a>Redirecciones abiertas

Cuando Blazor se inicia una sesión de aplicación de servidor, el servidor realiza la validación básica de las direcciones URL enviadas como parte del inicio de la sesión. El marco de trabajo comprueba que la dirección URL base es un elemento primario de la dirección URL actual antes de establecer el circuito. El marco de trabajo no realiza comprobaciones adicionales.

Cuando un usuario selecciona un vínculo en el cliente, la dirección URL del vínculo se envía al servidor, que determina qué acción realizar. Por ejemplo, la aplicación puede realizar una navegación del lado cliente o indicar al explorador para ir a la nueva ubicación.

Los componentes también pueden desencadenar solicitudes `NavigationManager`de navegación mediante programación mediante el uso de . En tales escenarios, la aplicación puede realizar una navegación del lado cliente o indicar al explorador para ir a la nueva ubicación.

Los componentes deben:

* Evite usar la entrada del usuario como parte de los argumentos de llamada de navegación.
* Validar argumentos para asegurarse de que la aplicación permite el destino.

De lo contrario, un usuario malintencionado puede forzar al navegador a ir a un sitio controlado por el atacante. En este escenario, el atacante engaña a la aplicación para `NavigationManager.Navigate` que use alguna entrada de usuario como parte de la invocación del método.

Este consejo también se aplica al representar vínculos como parte de la aplicación:

* Si es posible, utilice enlaces relativos.
* Valide que los destinos de vínculo absolutos son válidos antes de incluirlos en una página.

Para obtener más información, vea <xref:security/preventing-open-redirects>.

## <a name="authentication-and-authorization"></a>Autenticación y autorización

Para obtener instrucciones sobre autenticación <xref:security/blazor/index>y autorización, consulte .

## <a name="security-checklist"></a>Lista de comprobación de seguridad

La siguiente lista de consideraciones de seguridad no es completa:

* Validar argumentos de eventos.
* Valide las entradas y los resultados de las llamadas de interoperabilidad de JS.
* Evite usar (o validar de antemano) la entrada de usuario para las llamadas de interoperabilidad de .NET a JS.
* Evite que el cliente amese una cantidad de memoria sin enlazar.
  * Datos dentro del componente.
  * `DotNetObject`referencias devueltas al cliente.
* Protéyate de múltiples despachos.
* Cancelar operaciones de ejecución prolongada cuando se elimina el componente.
* Evite eventos que produzcan grandes cantidades de datos.
* Evite usar la entrada del `NavigationManager.Navigate` usuario como parte de las llamadas y valide la entrada del usuario para las direcciones URL con un conjunto de orígenes permitidos primero si es inevitable.
* No tome decisiones de autorización basadas en el estado de la interfaz de usuario, sino solo desde el estado del componente.
* Considere la posibilidad de usar [Content Security Policy (CSP)](https://developer.mozilla.org/docs/Web/HTTP/CSP) para protegerse contra ataques XSS.
* Considere la posibilidad de usar CSP y [X-Frame-Options](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-Frame-Options) para proteger contra el robo de clics.
* Asegúrese de que la configuración de CORS sea Blazor adecuada al habilitar CORS o deshabilitar explícitamente CORS para aplicaciones.
* Pruebe para asegurarse de que Blazor los límites del lado del servidor para la aplicación proporcionan una experiencia de usuario aceptable sin niveles de riesgo inaceptables.
