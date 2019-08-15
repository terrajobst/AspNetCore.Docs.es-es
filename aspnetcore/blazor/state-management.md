---
title: Administración de Estados de ASP.NET Core increíblemente
author: guardrex
description: Obtenga información sobre cómo conservar el estado en las aplicaciones de servidor más increíbles.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2019
uid: blazor/state-management
ms.openlocfilehash: af040635302fbf2dae8192dcf37d55bfcfedfcec
ms.sourcegitcommit: f5f0ff65d4e2a961939762fb00e654491a2c772a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/15/2019
ms.locfileid: "69030365"
---
# <a name="aspnet-core-blazor-state-management"></a>Administración de Estados de ASP.NET Core increíblemente

Por [Steve Sanderson](https://github.com/SteveSandersonMS)

El lado servidor es un marco de trabajo de aplicación con estado. La mayoría de las veces, la aplicación mantiene una conexión continua con el servidor. El estado del usuario se guarda en la memoria del servidor en un *circuito*. 

Entre los ejemplos de estado que se mantiene para el circuito de un usuario se incluyen:

* La interfaz de usuario&mdash;representada la jerarquía de instancias de componente y su salida de representación más reciente.
* Los valores de los campos y propiedades de las instancias de componente.
* Datos contenidos en las instancias de servicio de [inserción de dependencias (di)](xref:fundamentals/dependency-injection) que están en el ámbito del circuito.

> [!NOTE]
> En este artículo se trata la persistencia de estado en las aplicaciones de servidor más increíbles. Las aplicaciones de cliente increíbles pueden aprovechar la persistencia de [Estado de cliente en el explorador](#client-side-in-the-browser) , pero requieren soluciones personalizadas o paquetes de terceros más allá del ámbito de este artículo.

## <a name="blazor-circuits"></a>Circuitos increíbles

Si un usuario experimenta una pérdida de conexión de red temporal, más increíble intenta volver a conectar el usuario a su circuito original para que pueda seguir usando la aplicación. Sin embargo, no siempre es posible volver a conectar un usuario a su circuito original en la memoria del servidor:

* El servidor no puede mantener un circuito desconectado para siempre. El servidor debe liberar un circuito desconectado después de un tiempo de espera o cuando el servidor esté bajo presión de memoria.
* En entornos de implementación multiservidor y con equilibrio de carga, las solicitudes de procesamiento de servidor pueden dejar de estar disponibles en un momento dado. Se puede producir un error en los servidores individuales o quitarlos automáticamente cuando ya no sean necesarios para controlar el volumen total de solicitudes. Es posible que el servidor original no esté disponible cuando el usuario intenta volver a conectarse.
* El usuario podría cerrar y volver a abrir el explorador o recargar la página, lo que elimina cualquier Estado que se mantenga en la memoria del explorador. Por ejemplo, los valores establecidos mediante llamadas de interoperabilidad de JavaScript se pierden.

Cuando un usuario no se puede volver a conectar a su circuito original, el usuario recibe un circuito nuevo con un estado vacío. Esto es equivalente a cerrar y volver a abrir una aplicación de escritorio.

## <a name="preserve-state-across-circuits"></a>Conservar el estado entre circuitos

En algunos escenarios, es deseable conservar el estado entre los circuitos. Una aplicación puede conservar datos importantes para un usuario si:

* El servidor Web deja de estar disponible.
* El explorador del usuario se ve obligado a iniciar un nuevo circuito con un nuevo servidor Web.

En general, mantener el estado en los circuitos se aplica a los escenarios en los que los usuarios crean activamente datos, no simplemente leyendo los datos que ya existen.

Para conservar el estado más allá de un solo circuito, *no almacene simplemente los datos en la memoria del servidor*. La aplicación debe conservar los datos en otra ubicación de almacenamiento. La persistencia de estado&mdash;no es automática. debe seguir los pasos al desarrollar la aplicación para implementar la persistencia de datos con estado.

La persistencia de los datos normalmente solo es necesaria para el estado de alto valor que los usuarios han gastado en crear. En los ejemplos siguientes, el estado persistente ahorra tiempo o ayudas en las actividades comerciales:

* Webstep WebForm &ndash; es un proceso que tarda mucho tiempo en volver a escribir los datos de varios pasos completos de un proceso de varios pasos si se pierde su estado. Un usuario pierde el estado en este escenario si se desplaza fuera del formulario de múltiples pasos y vuelve al formulario más adelante.
* Carro &ndash; de la compra se puede mantener cualquier componente comercial importante de una aplicación que represente ingresos potenciales. Un usuario que pierde su estado y, por tanto, su carro de la compra, puede comprar menos productos o servicios cuando vuelvan al sitio más adelante.

Normalmente no es necesario conservar el estado de fácil creación, como el nombre de usuario especificado en un cuadro de diálogo de inicio de sesión que no se ha enviado.

> [!IMPORTANT]
> Una aplicación solo puede conservar el estado de la *aplicación*. Las ius no pueden persistir, como instancias de componente y sus árboles de representación. Los componentes y los árboles de representación no suelen ser serializables. Para conservar algo similar al estado de la interfaz de usuario, como los nodos expandidos de una vista de árbol, la aplicación debe tener código personalizado para modelar el comportamiento como estado de la aplicación serializable.

## <a name="where-to-persist-state"></a>Dónde conservar el estado

Existen tres ubicaciones comunes para el estado persistente en una aplicación del lado servidor increíblemente alta. Cada enfoque es más adecuado para distintos escenarios y tiene advertencias diferentes:

* [Lado servidor en una base de datos](#server-side-in-a-database)
* [URL](#url)
* [Del lado cliente en el explorador](#client-side-in-the-browser)

### <a name="server-side-in-a-database"></a>Lado servidor en una base de datos

Para la persistencia de datos permanente o para cualquier dato que deba abarcar varios usuarios o dispositivos, una base de datos independiente del servidor es casi seguro la mejor opción. Las opciones son:

* SQL Database relacional
* Almacén de clave-valor
* Almacén de blobs
* Almacén de tabla

Una vez guardados los datos en la base de datos, un usuario puede iniciar un nuevo circuito en cualquier momento. Los datos del usuario se conservan y están disponibles en cualquier circuito nuevo.

Para más información sobre las opciones de almacenamiento de datos de Azure, consulte la [documentación de Azure Storage](/azure/storage/) y [las bases](https://azure.microsoft.com/product-categories/databases/)de datos de Azure.

### <a name="url"></a>URL

Para los datos transitorios que representan el estado de navegación, modelo de los datos como parte de la dirección URL. Entre los ejemplos de estado modelado en la dirección URL se incluyen:

* IDENTIFICADOR de una entidad visualizada.
* El número de página actual en una cuadrícula paginada.

Se conserva el contenido de la barra de direcciones del explorador:

* Si el usuario recarga manualmente la página.
* Si el servidor Web deja de&mdash;estar disponible, el usuario se ve obligado a volver a cargar la página para conectarse a un servidor diferente.

Para obtener información sobre cómo definir patrones de `@page` direcciones URL con <xref:blazor/routing>la Directiva, vea.

### <a name="client-side-in-the-browser"></a>Del lado cliente en el explorador

En el caso de los datos transitorios que el usuario está creando activamente, una memoria auxiliar `localStorage` común `sessionStorage` son las colecciones y del explorador. No es necesario que la aplicación administre o borre el estado almacenado si se abandona el circuito, lo que supone una ventaja sobre el almacenamiento del lado servidor.

> [!NOTE]
> En esta sección, "cliente" hace referencia a los escenarios del lado cliente en el explorador, no al [modelo de hospedaje del lado cliente](xref:blazor/hosting-models#client-side). `localStorage`y `sessionStorage` se pueden usar en aplicaciones del lado cliente increíblemente, pero solo escribiendo código personalizado o usando un paquete de terceros.

`localStorage`y `sessionStorage` difieren como se indica a continuación:

* `localStorage`está en el ámbito del explorador del usuario. Si el usuario recarga la página o cierra y vuelve a abrir el explorador, el estado persiste. Si el usuario abre varias pestañas del explorador, el estado se comparte entre las pestañas. Los datos se conservan en `localStorage` hasta que se borran explícitamente.
* `sessionStorage`está en el ámbito de la pestaña del explorador del usuario. Si el usuario vuelve a cargar la pestaña, el estado persiste. Si el usuario cierra la pestaña o el explorador, se pierde el estado. Si el usuario abre varias pestañas del explorador, cada pestaña tendrá su propia versión independiente de los datos.

Por lo `sessionStorage` general, es más seguro usar. `sessionStorage`evita el riesgo de que un usuario abra varias pestañas y encuentre lo siguiente:

* Errores en el almacenamiento de estado en todas las pestañas.
* Comportamiento confuso cuando una pestaña sobrescribe el estado de otras pestañas.

`localStorage`es la mejor opción si la aplicación debe conservar el estado en el cierre y volver a abrir el explorador.

Advertencias sobre el uso del almacenamiento del explorador:

* De forma similar al uso de una base de datos del lado servidor, la carga y el almacenamiento de datos son asíncronos.
* A diferencia de las bases de datos del lado servidor, el almacenamiento no está disponible durante la representación previa porque la página solicitada no existe en el explorador durante la fase de representación previa.
* El almacenamiento de unos pocos kilobytes de datos es razonable para que se conserven para las aplicaciones de servidor increíbles. Más allá de unos pocos kilobytes, debe tener en cuenta las implicaciones de rendimiento porque los datos se cargan y se guardan en la red.
* Los usuarios pueden ver o alterar los datos. ASP.NET Core [protección de datos](xref:security/data-protection/introduction) puede mitigar el riesgo.

## <a name="third-party-browser-storage-solutions"></a>Soluciones de almacenamiento de explorador de terceros

Los paquetes NuGet de terceros proporcionan API para trabajar con `localStorage` y `sessionStorage`.

Merece la pena considerar la elección de un paquete que utiliza de forma transparente la [protección de datos](xref:security/data-protection/introduction)de ASP.net Core. ASP.NET Core protección de datos cifra los datos almacenados y reduce el riesgo potencial de alteración de los datos almacenados. Si los datos serializados con JSON se almacenan en texto sin formato, los usuarios pueden ver los datos mediante las herramientas de desarrollo del explorador y también modificar los datos almacenados. La protección de datos no siempre es un problema, ya que los datos pueden ser bastante triviales. Por ejemplo, la lectura o modificación del color almacenado de un elemento de la interfaz de usuario no supone un riesgo de seguridad significativo para el usuario o la organización. Evite que los usuarios puedan inspeccionar o alterar los *datos confidenciales*.

## <a name="protected-browser-storage-experimental-package"></a>Paquete experimental del almacenamiento de explorador protegido

Un ejemplo de un paquete NuGet que proporciona [protección](xref:security/data-protection/introduction) de datos `localStorage` para `sessionStorage` y es [Microsoft. AspNetCore. ProtectedBrowserStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.ProtectedBrowserStorage).

> [!WARNING]
> `Microsoft.AspNetCore.ProtectedBrowserStorage`es un paquete experimental no compatible que no es adecuado para su uso en producción en este momento.

### <a name="installation"></a>Instalación

Para instalar el `Microsoft.AspNetCore.ProtectedBrowserStorage` paquete:

1. En el proyecto de aplicación del lado servidor, agregue una referencia de paquete a [Microsoft. AspNetCore. ProtectedBrowserStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.ProtectedBrowserStorage).
1. En el código HTML de nivel superior (por ejemplo, en el archivo *pages/_Host. cshtml* en la plantilla de proyecto predeterminada), `<script>` agregue la siguiente etiqueta:

   ```html
   <script src="_content/Microsoft.AspNetCore.ProtectedBrowserStorage/protectedBrowserStorage.js"></script>
   ```

1. En el `Startup.ConfigureServices` método, llame `AddProtectedBrowserStorage` a para `localStorage` agregar `sessionStorage` los servicios de y a la colección de servicios:

   ```csharp
   services.AddProtectedBrowserStorage();
   ```

### <a name="save-and-load-data-within-a-component"></a>Guardar y cargar datos dentro de un componente

En cualquier componente que requiera cargar o guardar datos en el almacenamiento del [@inject](xref:blazor/dependency-injection#request-a-service-in-a-component) explorador, use para insertar una instancia de uno de los elementos siguientes:

* `ProtectedLocalStorage`
* `ProtectedSessionStorage`

La elección depende de la memoria auxiliar que quiera usar. En el ejemplo siguiente, `sessionStorage` se usa:

```cshtml
@using Microsoft.AspNetCore.ProtectedBrowserStorage
@inject ProtectedSessionStorage ProtectedSessionStore
```

La `@using` instrucción se puede colocar en un archivo *_Imports. Razor* en lugar de en el componente. El uso del archivo *_Imports. Razor* hace que el espacio de nombres esté disponible para segmentos mayores de la aplicación o de toda la aplicación.

Para conservar el `currentCount` valor en el `Counter` componente de la plantilla de proyecto, modifique `IncrementCount` el método para `ProtectedSessionStore.SetAsync`usar:

```csharp
private async Task IncrementCount()
{
    currentCount++;
    await ProtectedSessionStore.SetAsync("count", currentCount);
}
```

En aplicaciones más grandes y realistas, el almacenamiento de campos individuales es un escenario improbable. Es más probable que las aplicaciones almacenen objetos de modelo completos que incluyen un estado complejo. `ProtectedSessionStore`Serializa y deserializa automáticamente los datos JSON.

En el ejemplo de código anterior, `currentCount` los datos se almacenan como `sessionStorage['count']` en el explorador del usuario. Los datos no se almacenan en texto sin formato, sino que se protegen mediante la [protección de datos](xref:security/data-protection/introduction)de ASP.net Core. Los datos cifrados pueden verse si `sessionStorage['count']` se evalúa en la consola del desarrollador del explorador.

Para recuperar los `currentCount` datos si el usuario vuelve `Counter` al componente más adelante (incluso si están en un circuito totalmente nuevo), use `ProtectedSessionStore.GetAsync`:

```csharp
protected override async Task OnInitializedAsync()
{
    currentCount = await ProtectedSessionStore.GetAsync<int>("count");
}
```

Si los parámetros del componente incluyen el estado de navegación `ProtectedSessionStore.GetAsync` , llame a y asigne `OnParametersSetAsync`el resultado `OnInitializedAsync`en, no en. `OnInitializedAsync`solo se llama una vez cuando se crea una instancia del componente en primer lugar. `OnInitializedAsync`no se llama de nuevo más tarde si el usuario navega a una dirección URL diferente mientras permanece en la misma página.

> [!WARNING]
> Los ejemplos de esta sección solo funcionan si el servidor no tiene habilitada la representación previa. Con la representación previa habilitada, se genera un error similar al siguiente:
>
> > No se pueden emitir llamadas de interoperabilidad de JavaScript en este momento. Esto se debe a que el componente se está preprocesando.
>
> Deshabilite la representación previa o agregue código adicional para trabajar con la representación previa. Para obtener más información sobre cómo escribir código que funcione con la representación previa, consulte la sección [Handle](#handle-prerendering) prerendering.

### <a name="handle-the-loading-state"></a>Controlar el estado de carga

Dado que el almacenamiento del explorador es asincrónico (al que se accede a través de una conexión de red), siempre hay un período de tiempo antes de que los datos se carguen y estén disponibles para que los use un componente. Para obtener los mejores resultados, procese un mensaje de estado de carga mientras se carga está en curso en lugar de Mostrar datos en blanco o predeterminados.

Un enfoque consiste en realizar un seguimiento de si `null` los datos están (todavía en proceso de carga) o no. En el componente `Counter` predeterminado, el recuento se mantiene en `int`un. Para `currentCount` hacer que acepte valores NULL, agregue un`?`signo de interrogación (`int`) al tipo ():

```csharp
private int? currentCount;
```

En lugar de mostrar de manera incondicional el botón recuento e **incremento** , elija Mostrar estos elementos solo si se cargan los datos:

```cshtml
@if (currentCount.HasValue)
{
    <p>Current count: <strong>@currentCount</strong></p>

    <button @onclick="IncrementCount">Increment</button>
}
else
{
    <p>Loading...</p>
}
```

### <a name="handle-prerendering"></a>Controlar la representación previa

Durante la representación previa:

* No existe una conexión interactiva al explorador del usuario.
* El explorador todavía no tiene una página en la que pueda ejecutar código JavaScript.

`localStorage`o `sessionStorage` no están disponibles durante la representación previa. Si el componente intenta interactuar con el almacenamiento, se genera un error similar a:

> No se pueden emitir llamadas de interoperabilidad de JavaScript en este momento. Esto se debe a que el componente se está preprocesando.

Una manera de resolver el error es deshabilitar la representación previa. Esta suele ser la mejor opción si la aplicación hace un uso intensivo del almacenamiento basado en explorador. La representación previa agrega complejidad y no beneficia a la aplicación porque la aplicación no puede representar un contenido útil `localStorage` hasta `sessionStorage` que no esté disponible.

Para deshabilitar la representación previa:

1. Abra el archivo *pages/_Host. cshtml* y quite la llamada `Html.RenderComponentAsync`a.
1. Abra el `Startup.cs` archivo y reemplace la llamada a `endpoints.MapBlazorHub()` por `endpoints.MapBlazorHub<App>("app")`. `App`es el tipo del componente raíz. `"app"`es un selector de CSS que especifica la ubicación del componente raíz.

La representación previa puede ser útil para otras páginas que no utilizan `localStorage` o `sessionStorage`. Para mantener habilitada la pregeneración, postergue la operación de carga hasta que el explorador esté conectado al circuito. El siguiente es un ejemplo para almacenar un valor de contador:

```cshtml
@using Microsoft.AspNetCore.ProtectedBrowserStorage
@inject ProtectedLocalStorage ProtectedLocalStore
@inject IComponentContext ComponentContext

... rendering code goes here ...

@code {
    private int? currentCount;
    private bool isWaitingForConnection;

    protected override async Task OnInitializedAsync()
    {
        if (ComponentContext.IsConnected)
        {
            // It looks like the app isn't prerendering, so the data can be
            // immediately loaded from browser storage.
            await LoadStateAsync();
        }
        else
        {
            // Prerendering is in progress, so the app defers the load operation
            // until later.
            isWaitingForConnection = true;
        }
    }

    protected override async Task OnAfterRenderAsync()
    {
        // By this stage, the client has connected back to the server, and
        // browser services are available. If the app didn't load the data earlier,
        // the app should do so now and then trigger a new render.
        if (isWaitingForConnection)
        {
            isWaitingForConnection = false;
            await LoadStateAsync();
            StateHasChanged();
        }
    }

    private async Task LoadStateAsync()
    {
        currentCount = await ProtectedLocalStore.GetAsync<int>("prerenderedCount");
    }

    private async Task IncrementCount()
    {
        currentCount++;
        await ProtectedSessionStore.SetAsync("count", currentCount);
    }
}
```

### <a name="factor-out-the-state-preservation-to-a-common-location"></a>Factorizar la preservación de estado en una ubicación común

Si muchos componentes se basan en el almacenamiento basado en explorador, al volver a implementar el código del proveedor de estado muchas veces se crea la duplicación de código. Una opción para evitar la duplicación de código es crear un *componente primario del proveedor de estado* que encapsula la lógica del proveedor de estado. Los componentes secundarios pueden trabajar con datos persistentes sin tener en cuenta el mecanismo de persistencia del estado.

En el siguiente ejemplo de un `CounterStateProvider` componente, se conservan los datos del contador:

```cshtml
@using Microsoft.AspNetCore.ProtectedBrowserStorage
@inject ProtectedSessionStorage ProtectedSessionStore

@if (hasLoaded)
{
    <CascadingValue Value="@this">
        @ChildContent
    </CascadingValue>
}
else
{
    <p>Loading...</p>
}

@code {
    private bool hasLoaded;

    [Parameter]
    public RenderFragment ChildContent { get; set; }

    public int CurrentCount { get; set; }

    protected override async Task OnInitializedAsync()
    {
        CurrentCount = await ProtectedSessionStore.GetAsync<int>("count");
        hasLoaded = true;
    }

    public async Task SaveChangesAsync()
    {
        await ProtectedSessionStore.SetAsync("count", CurrentCount);
    }
}
```

El `CounterStateProvider` componente controla la fase de carga al no representar su contenido secundario hasta que se completa la carga.

Para usar el `CounterStateProvider` componente, ajuste una instancia del componente en torno a cualquier otro componente que requiera acceso al estado del contador. Para hacer que el estado sea accesible a todos los componentes de una aplicación `CounterStateProvider` , ajuste el `Router` componente alrededor `App` del en el componente (*app. Razor*):

```cshtml
<CounterStateProvider>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CounterStateProvider>
```

Los componentes encapsulados reciben y pueden modificar el estado del contador guardado. El componente `Counter` siguiente implementa el patrón:

```cshtml
@page "/counter"

<p>Current count: <strong>@CounterStateProvider.CurrentCount</strong></p>

<button @onclick="IncrementCount">Increment</button>

@code {
    [CascadingParameter]
    private CounterStateProvider CounterStateProvider { get; set; }

    private async Task IncrementCount()
    {
        CounterStateProvider.CurrentCount++;
        await CounterStateProvider.SaveChangesAsync();
    }
}
```

No es necesario que el componente anterior interactúe `ProtectedBrowserStorage`con, ni se trata de una fase de "carga".

Para tratar con la representación previa tal y como se `CounterStateProvider` ha descrito anteriormente, se puede modificar para que todos los componentes que consumen los datos del contador funcionen automáticamente con la representación previa. Para obtener más información, consulte la sección [Handle](#handle-prerendering) prerendering.

En general, se recomienda el patrón de *componente primario del proveedor de estado* :

* Para consumir el estado en muchos otros componentes.
* Si solo hay un objeto de estado de nivel superior para conservar.

Para conservar muchos objetos de estado diferentes y consumir distintos subconjuntos de objetos en distintos lugares, es mejor evitar el control de la carga y el guardado de estado globalmente.
