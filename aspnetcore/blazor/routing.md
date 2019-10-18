---
title: ASP.NET Core el enrutamiento más brillante
author: guardrex
description: Aprenda a enrutar las solicitudes en aplicaciones y el componente NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: blazor/routing
ms.openlocfilehash: d9f81c8aa2cf07f8bfaede65efcb7328088f55b9
ms.sourcegitcommit: ce2bfb01f2cc7dd83f8a97da0689d232c71bcdc4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2019
ms.locfileid: "72531138"
---
# <a name="aspnet-core-blazor-routing"></a>ASP.NET Core el enrutamiento más brillante

Por [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Obtenga información acerca de cómo enrutar las solicitudes y cómo usar el componente `NavLink` para crear vínculos de navegación en aplicaciones increíbles.

## <a name="aspnet-core-endpoint-routing-integration"></a>Integración del enrutamiento de puntos de conexión de ASP.NET Core

El servidor más rápido está integrado en [ASP.net Core enrutamiento de puntos de conexión](xref:fundamentals/routing). Una aplicación ASP.NET Core está configurada para aceptar conexiones entrantes para componentes interactivos con `MapBlazorHub` en `Startup.Configure`:

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

La configuración más habitual consiste en enrutar todas las solicitudes a una página de Razor, que actúa como el host para la parte del lado servidor de la aplicación de servidor de la extraordinaria. Por Convención, la página *host* se denomina normalmente *_Host. cshtml*. La ruta especificada en el archivo host se denomina *ruta de reserva* porque funciona con una prioridad baja en la coincidencia de rutas. La ruta de reserva se considera cuando otras rutas no coinciden. Esto permite que la aplicación use otros controladores y páginas sin interferir con la aplicación de servidor increíblemente.

## <a name="route-templates"></a>Plantillas de ruta

El componente `Router` permite el enrutamiento a cada componente con una ruta especificada. El componente `Router` aparece en el archivo *app. Razor* :

```cshtml
<Router AppAssembly="typeof(Startup).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <p>Sorry, there's nothing at this address.</p>
    </NotFound>
</Router>
```

Cuando se compila un archivo *. Razor* con una directiva `@page`, se proporciona a la clase generada un <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> que especifica la plantilla de ruta.

En tiempo de ejecución, el componente `RouteView`:

* Recibe el `RouteData` del `Router` junto con los parámetros deseados.
* Representa el componente especificado con su diseño (o un diseño predeterminado opcional) mediante los parámetros especificados.

Opcionalmente, puede especificar un parámetro `DefaultLayout` con una clase de diseño que se usará para los componentes que no especifican un diseño. Las plantillas increíblemente predeterminadas especifican el componente `MainLayout`. *MainLayout. Razor* está en la carpeta *compartida* del proyecto de plantilla. Para obtener más información sobre los diseños, vea <xref:blazor/layouts>.

Se pueden aplicar varias plantillas de ruta a un componente. El siguiente componente responde a las solicitudes de `/BlazorRoute` y `/DifferentBlazorRoute`:

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> Para que las direcciones URL se resuelvan correctamente, la aplicación debe incluir una `<base>` etiqueta en su archivo *wwwroot/index.html* (webassembly) o *pages/_Host. cshtml* (servidor increíblemente) con la ruta de acceso base de la aplicación especificada en el atributo `href` (`<base href="/">`). Para obtener más información, vea <xref:host-and-deploy/blazor/index#app-base-path>.

## <a name="provide-custom-content-when-content-isnt-found"></a>Proporcionar contenido personalizado cuando no se encuentra el contenido

El componente `Router` permite a la aplicación especificar el contenido personalizado si no se encuentra el contenido para la ruta solicitada.

En el archivo *app. Razor* , establezca el contenido personalizado en el parámetro de plantilla `NotFound` del componente `Router`:

```cshtml
<Router AppAssembly="typeof(Startup).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <h1>Sorry</h1>
        <p>Sorry, there's nothing at this address.</p> b
    </NotFound>
</Router>
```

El contenido de las etiquetas `<NotFound>` puede incluir elementos arbitrarios, como otros componentes interactivos. Para aplicar un diseño predeterminado al contenido `NotFound`, consulte <xref:blazor/layouts>.

## <a name="route-to-components-from-multiple-assemblies"></a>Enrutar a componentes de varios ensamblados

Use el parámetro `AdditionalAssemblies` para especificar ensamblados adicionales para que el componente `Router` tenga en cuenta al buscar componentes enrutables. Los ensamblados especificados se consideran además del ensamblado especificado por el @no__t -0. En el ejemplo siguiente, `Component1` es un componente enrutable definido en una biblioteca de clases a la que se hace referencia. El siguiente ejemplo `AdditionalAssemblies` da como resultado la compatibilidad con el enrutamiento de `Component1`:

```cshtml
<Router
    AppAssembly="typeof(Program).Assembly"
    AdditionalAssemblies="new[] { typeof(Component1).Assembly }">
    ...
</Router>
```

## <a name="route-parameters"></a>Parámetros de ruta

El enrutador usa parámetros de ruta para rellenar los parámetros de componente correspondientes con el mismo nombre (no distingue mayúsculas de minúsculas):

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

Los parámetros opcionales no se admiten para las aplicaciones increíbles en ASP.NET Core 3,0. En el ejemplo anterior se aplican dos directivas `@page`. El primero permite la navegación al componente sin un parámetro. La segunda Directiva `@page` toma el parámetro de ruta `{text}` y asigna el valor a la propiedad `Text`.

## <a name="route-constraints"></a>Restricciones de ruta

Una restricción de ruta aplica la coincidencia de tipos en un segmento de ruta a un componente.

En el ejemplo siguiente, la ruta al componente `Users` solo coincide con si:

* Existe un segmento de ruta `Id` en la dirección URL de la solicitud.
* El segmento `Id` es un entero (`int`).

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

Están disponibles las restricciones de ruta que se muestran en la tabla siguiente. Para obtener más información sobre las restricciones de ruta que coinciden con la referencia cultural de todos los idiomas, vea la advertencia que se encuentra debajo de la tabla.

| Restricción | Ejemplo           | Coincidencias de ejemplo                                                                  | Invariable<br>referencia cultural<br>coincidencia |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | `true`, `FALSE`                                                                  | No                               |
| `datetime` | `{dob:datetime}`  | `2016-12-31`, `2016-12-31 7:32pm`                                                | Sí                              |
| `decimal`  | `{price:decimal}` | `49.99`, `-1,000.01`                                                             | Sí                              |
| `double`   | `{weight:double}` | `1.234`, `-1,001.01e8`                                                           | Sí                              |
| `float`    | `{weight:float}`  | `1.234`, `-1,001.01e8`                                                           | Sí                              |
| `guid`     | `{id:guid}`       | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | No                               |
| `int`      | `{id:int}`        | `123456789`, `-123456789`                                                        | Sí                              |
| `long`     | `{ticks:long}`    | `123456789`, `-123456789`                                                        | Sí                              |

> [!WARNING]
> Las restricciones de ruta que comprueban la dirección URL y que se convierten en un tipo CLR (como `int` o `DateTime`) usan siempre la referencia cultural invariable. Estas restricciones dan por supuesto que la dirección URL no es localizable.

### <a name="routing-with-urls-that-contain-dots"></a>Enrutamiento con direcciones URL que contienen puntos

En las aplicaciones de servidor increíbles, la ruta predeterminada en *_Host. cshtml* es `/` (`@page "/"`). La ruta predeterminada no coincide con la dirección URL de la solicitud que contiene un punto (`.`) porque la dirección URL parece solicitar un archivo. Una aplicación increíblemente alta devuelve una respuesta *404 no encontrada* para un archivo estático que no existe. Para usar rutas que contienen un punto, configure *_Host. cshtml* con la siguiente plantilla de ruta:

```cshtml
@page "/{**path}"
```

La plantilla `"/{**path}"` incluye:

* Sintaxis *catch-all de* doble asterisco (`**`) para capturar la ruta de acceso en varios límites de carpeta sin codificar barras diagonales (`/`).
* Un nombre de parámetro de ruta `path`.

Para obtener más información, vea <xref:fundamentals/routing>.

## <a name="navlink-component"></a>Componente NavLink

Use un componente `NavLink` en lugar de los elementos de hipervínculo HTML (`<a>`) al crear vínculos de navegación. Un componente `NavLink` se comporta como un elemento `<a>`, salvo que alterna una clase CSS @no__t 2 en función de si su `href` coincide con la dirección URL actual. La clase `active` ayuda a los usuarios a entender qué página es la página activa entre los vínculos de navegación mostrados.

El siguiente componente `NavMenu` crea una barra de navegación de [arranque](https://getbootstrap.com/docs/) que muestra cómo usar los componentes de `NavLink`:

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

Hay dos opciones `NavLinkMatch` que puede asignar al atributo `Match` del elemento `<NavLink>`:

* `NavLinkMatch.All` &ndash; el @no__t 2 está activo cuando coincide con toda la dirección URL actual.
* `NavLinkMatch.Prefix` (*valor predeterminado*) &ndash; el `NavLink` está activo cuando coincide con cualquier prefijo de la dirección URL actual.

En el ejemplo anterior, el @no__t Home-0 `href=""` coincide con la dirección URL de inicio y solo recibe la clase CSS @no__t 2 en la dirección URL de la ruta de acceso base predeterminada de la aplicación (por ejemplo, `https://localhost:5001/`). El segundo `NavLink` recibe la clase `active` cuando el usuario visita cualquier dirección URL con un prefijo @no__t 2 (por ejemplo, `https://localhost:5001/MyComponent` y `https://localhost:5001/MyComponent/AnotherSegment`).

Los atributos adicionales del componente `NavLink` se pasan a la etiqueta delimitadora representada. En el ejemplo siguiente, el componente `NavLink` incluye el atributo `target`:

```cshtml
<NavLink href="my-page" target="_blank">My page</NavLink>
```

Se representa el siguiente marcado HTML:

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a>Aplicaciones auxiliares de estado de navegación y URI

Use `Microsoft.AspNetCore.Components.NavigationManager` para trabajar con los URI y la C# navegación en el código. `NavigationManager` proporciona el evento y los métodos que se muestran en la tabla siguiente.

| Miembro | Descripción |
| ------ | ----------- |
| `Uri` | Obtiene el URI absoluto actual. |
| `BaseUri` | Obtiene el URI base (con una barra diagonal final) que se puede anteponer a las rutas de acceso del URI relativo para generar un URI absoluto. Normalmente, `BaseUri` se corresponde con el atributo `href` en el elemento `<base>` del documento en *wwwroot/index.html* (webassembly) o *pages/_Host. cshtml* (servidor increíble). |
| `NavigateTo` | Navega al URI especificado. Si `forceLoad` es `true`:<ul><li>Se omite el enrutamiento del lado cliente.</li><li>El explorador se ve obligado a cargar la nueva página del servidor, tanto si el identificador URI está controlado normalmente por el enrutador del lado cliente como si no.</li></ul> |
| `LocationChanged` | Evento que se desencadena cuando ha cambiado la ubicación de navegación. |
| `ToAbsoluteUri` | Convierte un URI relativo en un URI absoluto. |
| `ToBaseRelativePath` | Dado un URI base (por ejemplo, un URI previamente devuelto por `GetBaseUri`), convierte un URI absoluto en un URI relativo al prefijo de URI base. |

El siguiente componente navega al componente `Counter` de la aplicación cuando se selecciona el botón:

```cshtml
@page "/navigate"
@inject NavigationManager NavigationManager

<h1>Navigate in Code Example</h1>

<button class="btn btn-primary" @onclick="NavigateToCounterComponent">
    Navigate to the Counter component
</button>

@code {
    private void NavigateToCounterComponent()
    {
        NavigationManager.NavigateTo("counter");
    }
}
```
