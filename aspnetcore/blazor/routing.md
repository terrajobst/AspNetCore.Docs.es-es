---
title: ASP.NET Core el enrutamiento más brillante
author: guardrex
description: Aprenda a enrutar las solicitudes en aplicaciones y el componente NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/23/2019
uid: blazor/routing
ms.openlocfilehash: ae3d7ab01185dd6f2e8e0f59b78c2e693fe464b0
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310342"
---
# <a name="aspnet-core-blazor-routing"></a>ASP.NET Core el enrutamiento más brillante

Por [Luke Latham](https://github.com/guardrex)

Obtenga información acerca de cómo enrutar las solicitudes `NavLink` y cómo usar el componente para crear vínculos de navegación en aplicaciones increíbles.

## <a name="aspnet-core-endpoint-routing-integration"></a>Integración del enrutamiento de puntos de conexión de ASP.NET Core

El lado servidor increíblemente integrado se integra en [ASP.net Core enrutamiento de puntos de conexión](xref:fundamentals/routing). Una aplicación ASP.net Core está configurada para aceptar conexiones entrantes para `MapBlazorHub` componentes `Startup.Configure`interactivos con en:

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

## <a name="route-templates"></a>Plantillas de ruta

El `Router` componente habilita el enrutamiento y se proporciona una plantilla de ruta a cada componente accesible. El `Router` componente aparece en el archivo *app. Razor* :

En una aplicación del lado servidor o en el lado cliente:

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

Cuando se compila un archivo *. Razor* con una `@page` Directiva, se proporciona la <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> clase generada que especifica la plantilla de ruta. En tiempo de ejecución, el enrutador busca clases `RouteAttribute` de componentes con y representa el componente con una plantilla de ruta que coincide con la dirección URL solicitada.

Se pueden aplicar varias plantillas de ruta a un componente. El siguiente componente responde a las solicitudes de `/BlazorRoute` y `/DifferentBlazorRoute`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> Para que las direcciones URL se resuelvan correctamente, la `<base>` aplicación debe incluir una etiqueta en su archivo *wwwroot/index.html* (lado cliente) o en el archivo *pages/_Host. cshtml* (servidor increíble) con la ruta de acceso base de `href` la aplicación especificada en el atributo ( `<base href="/">`). Para obtener más información, consulta <xref:host-and-deploy/blazor/client-side#app-base-path>.

## <a name="provide-custom-content-when-content-isnt-found"></a>Proporcionar contenido personalizado cuando no se encuentra el contenido

El `Router` componente permite que la aplicación especifique el contenido personalizado si no se encuentra el contenido para la ruta solicitada.

En el archivo *app. Razor* , establezca el contenido personalizado en `<NotFound>` el parámetro de `Router` plantilla del componente:

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

El contenido de `<NotFound>` puede incluir elementos arbitrarios, como otros componentes interactivos.

## <a name="route-parameters"></a>Parámetros de ruta

El enrutador usa parámetros de ruta para rellenar los parámetros de componente correspondientes con el mismo nombre (no distingue mayúsculas de minúsculas):

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

Los parámetros opcionales no se admiten para las aplicaciones increíbles en ASP.NET Core vista previa de 3,0. En `@page` el ejemplo anterior se aplican dos directivas. El primero permite la navegación al componente sin un parámetro. La segunda `@page` Directiva toma el `{text}` parámetro Route y asigna el valor a la `Text` propiedad.

## <a name="route-constraints"></a>Restricciones de ruta

Una restricción de ruta aplica la coincidencia de tipos en un segmento de ruta a un componente.

En el ejemplo siguiente, la ruta al `Users` componente solo coincide con si:

* Hay `Id` un segmento de ruta en la dirección URL de la solicitud.
* El `Id` segmento es un entero (`int`).

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

Están disponibles las restricciones de ruta que se muestran en la tabla siguiente. Para obtener más información sobre las restricciones de ruta que coinciden con la referencia cultural de todos los idiomas, vea la advertencia que se encuentra debajo de la tabla.

| Restricción | Ejemplo           | Coincidencias de ejemplo                                                                  | Invariable<br>culture<br>coincidencia |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | `true`, `FALSE`                                                                  | Sin                               |
| `datetime` | `{dob:datetime}`  | `2016-12-31`, `2016-12-31 7:32pm`                                                | Sí                              |
| `decimal`  | `{price:decimal}` | `49.99`, `-1,000.01`                                                             | Sí                              |
| `double`   | `{weight:double}` | `1.234`, `-1,001.01e8`                                                           | Sí                              |
| `float`    | `{weight:float}`  | `1.234`, `-1,001.01e8`                                                           | Sí                              |
| `guid`     | `{id:guid}`       | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Sin                               |
| `int`      | `{id:int}`        | `123456789`, `-123456789`                                                        | Sí                              |
| `long`     | `{ticks:long}`    | `123456789`, `-123456789`                                                        | Sí                              |

> [!WARNING]
> Las restricciones de ruta que comprueban la dirección URL y que se convierten en un tipo CLR (como `int` o `DateTime`) usan siempre la referencia cultural invariable. Estas restricciones dan por supuesto que la dirección URL no es localizable.

### <a name="routing-with-urls-that-contain-dots"></a>Enrutamiento con direcciones URL que contienen puntos

En las aplicaciones de servidor increíbles, la ruta predeterminada en *_Host. cshtml* es `/` (`@page "/"`). Una dirección URL de solicitud que contiene un`.`punto () no coincide con la ruta predeterminada porque la dirección URL parece solicitar un archivo. Una aplicación increíblemente alta devuelve una respuesta *404 no encontrada* para un archivo estático que no existe. Para usar rutas que contienen un punto, configure *_Host. cshtml* con la siguiente plantilla de ruta:

```cshtml
@page "/{**path}"
```

La `"/{**path}"` plantilla incluye:

* Sintaxis *catch-all de* doble asterisco (`**`) para capturar la ruta de acceso en varios límites de carpeta sin codificar barras`/`diagonales ().
* Nombre `path` del parámetro de ruta.

Para obtener más información, consulta <xref:fundamentals/routing>.

## <a name="navlink-component"></a>Componente NavLink

Use un `NavLink` componente en lugar de los elementos de hipervínculo HTML (`<a>`) al crear vínculos de navegación. Un `NavLink` componente se comporta como un `<a>` elemento, salvo que alterna una `active` clase CSS en función de si `href` coincide con la dirección URL actual. La `active` clase ayuda a un usuario a entender qué página es la página activa entre los vínculos de navegación mostrados.

El componente `NavMenu` siguiente crea una barra de navegación de [bootstrap](https://getbootstrap.com/docs/) que muestra cómo `NavLink` utilizar los componentes de:

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

Hay dos `NavLinkMatch` opciones que puede asignar `Match` al atributo del `<NavLink>` elemento:

* `NavLinkMatch.All`&ndash; El`NavLink` está activo cuando coincide con toda la dirección URL actual.
* `NavLinkMatch.Prefix`(*valor predeterminado*) &ndash; El`NavLink` está activo cuando coincide con cualquier prefijo de la dirección URL actual.

En el ejemplo anterior, el inicio `NavLink` `href=""` coincide con la dirección URL de inicio y `active` solo recibe la clase CSS en la dirección URL de la ruta de acceso `https://localhost:5001/`base predeterminada de la aplicación (por ejemplo,). El segundo `NavLink` recibe la `active` clase cuando el usuario visita cualquier dirección URL con `MyComponent` un prefijo (por `https://localhost:5001/MyComponent` ejemplo `https://localhost:5001/MyComponent/AnotherSegment`, y).

Los `NavLink` atributos de componente adicionales se pasan a la etiqueta delimitadora representada. En el ejemplo siguiente, el `NavLink` componente incluye el `target` atributo:

```cshtml
<NavLink href="my-page" target="_blank">My page</NavLink>
```

Se representa el siguiente marcado HTML:

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a>Aplicaciones auxiliares de estado de navegación y URI

Use `Microsoft.AspNetCore.Components.NavigationManager` para trabajar con los URI y la C# navegación en el código. `NavigationManager`proporciona el evento y los métodos que se muestran en la tabla siguiente.

| Member | DESCRIPCIÓN |
| ------ | ----------- |
| `Uri` | Obtiene el URI absoluto actual. |
| `BaseUri` | Obtiene el URI base (con una barra diagonal final) que se puede anteponer a las rutas de acceso del URI relativo para generar un URI absoluto. Normalmente, `BaseUri` se corresponde con `href` el atributo en el elemento `<base>` del documento en *wwwroot/index.html* (cliente de increíble parte) o *pages/_Host. cshtml* (servidor más increíble). |
| `NavigateTo` | Navega al URI especificado. Si `forceLoad` es `true`:<ul><li>Se omite el enrutamiento del lado cliente.</li><li>El explorador se ve obligado a cargar la nueva página del servidor, tanto si el identificador URI está controlado normalmente por el enrutador del lado cliente como si no.</li></ul> |
| `LocationChanged` | Evento que se desencadena cuando ha cambiado la ubicación de navegación. |
| `ToAbsoluteUri` | Convierte un URI relativo en un URI absoluto. |
| `ToBaseRelativePath` | Dado un URI base (por ejemplo, un URI previamente devuelto `GetBaseUri`por), convierte un URI absoluto en un URI relativo al prefijo de URI base. |

El siguiente componente navega al componente de `Counter` la aplicación cuando se selecciona el botón:

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

