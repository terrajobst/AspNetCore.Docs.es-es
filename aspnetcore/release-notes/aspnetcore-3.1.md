---
title: Novedades de ASP.NET Core 3.1
author: rick-anderson
description: Obtenga información sobre las nuevas características de ASP.NET Core 3.1.
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: aspnetcore-3.1
ms.openlocfilehash: f375022ad3ebdea2990f626320ef295926f88c22
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "78648773"
---
# <a name="whats-new-in-aspnet-core-31"></a>Novedades de ASP.NET Core 3.1

En este artículo se resaltan los cambios más importantes de ASP.NET Core 3.1, con vínculos a la documentación pertinente.

## <a name="partial-class-support-for-razor-components"></a>Compatibilidad de clases parciales con componentes de Razor

Ahora los componentes de Razor se generan como clases parciales. El código de un componente de Razor se puede escribir con un archivo de código subyacente definido como una clase parcial en lugar de definir todo el código del componente en un solo archivo. Para más información, vea [Compatibilidad de clases parciales](xref:blazor/components#partial-class-support).

## <a name="opno-locblazor-component-tag-helper-and-pass-parameters-to-top-level-components"></a>El Asistente de etiquetas de componente de Blazor y transferencia de parámetros a componentes de nivel superior

En Blazor con ASP.NET Core 3.0, los componentes se representaban en páginas y vistas mediante una aplicación auxiliar de HTML (`Html.RenderComponentAsync`). En ASP.NET Core 3.1, un componente se representa desde una página o vista con el nuevo asistente de etiquetas de componente:

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" />
```

La aplicación auxiliar de HTML sigue siendo compatible con ASP.NET Core 3.1, pero se recomienda el asistente de etiquetas de componente.

Ahora, las aplicaciones de Blazor Server pueden pasar parámetros a componentes de nivel superior durante la representación inicial. Anteriormente, solo se podían pasar parámetros a un componente de nivel superior por medio de [RenderMode.Static](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static). Con esta versión, se admiten tanto [RenderMode.Server](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server) como [RenderModel.ServerPrerendered](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered). Los valores de parámetro especificados se serializan como JSON y se incluyen en la respuesta inicial.

Por ejemplo, se puede realizar la representación previa de un componente `Counter` con una cantidad de incremento (`IncrementAmount`):

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

Para más información, vea [Integración de componentes en aplicaciones de Razor Pages y MVC](xref:blazor/integrate-components).

## <a name="support-for-shared-queues-in-httpsys"></a>Compatibilidad con las colas compartidas en HTTP.sys

[HTTP.sys](xref:fundamentals/servers/httpsys) admite la creación de colas de solicitudes anónimas. En ASP.NET Core 3.1, hemos agregado la capacidad de crear o adjuntar contenido a una cola de solicitudes existente de HTTP.sys con nombre. La operación de crear o adjuntar contenido a una cola de solicitudes existente de HTTP.sys con nombre habilita escenarios en los que el proceso del controlador HTTP.Sys propietario de la cola es independiente del proceso del cliente de escucha. Esta independencia permite conservar las conexiones existentes y las solicitudes en cola entre los reinicios del proceso del cliente de escucha:

[!code-csharp[](sample/Program.cs?name=snippet)]

## <a name="breaking-changes-for-samesite-cookies"></a>Cambios importantes en las cookies de SameSite

El comportamiento de las cookies de SameSite ha cambiado para reflejar los próximos cambios del explorador. Esto puede afectar a escenarios de autenticación como AzureAd, OpenIdConnect o WsFederation. Para obtener más información, consulta <xref:security/samesite>.

## <a name="prevent-default-actions-for-events-in-opno-locblazor-apps"></a>Impedir acciones predeterminadas para eventos en aplicaciones Blazor

Use el atributo de directiva `@on{EVENT}:preventDefault` para evitar la acción predeterminada de un evento. En el ejemplo siguiente, se impide la acción predeterminada de mostrar el carácter de la clave en el cuadro de texto:

```razor
<input value="@_count" @onkeypress="KeyHandler" @onkeypress:preventDefault />
```

Para más información, vea [Impedir acciones predeterminadas](xref:blazor/event-handling#prevent-default-actions).

## <a name="stop-event-propagation-in-opno-locblazor-apps"></a>Detener la propagación de eventos en aplicaciones Blazor

Use el atributo de directiva `@on{EVENT}:stopPropagation` para detener la propagación de eventos. En el ejemplo siguiente, al activar la casilla se impide que los eventos de clic del elemento `<div>` secundario se propaguen al elemento `<div>` principal:

```razor
<input @bind="_stopPropagation" type="checkbox" />

<div @onclick="OnSelectParentDiv">
    <div @onclick="OnSelectChildDiv" @onclick:stopPropagation="_stopPropagation">
        ...
    </div>
</div>

@code {
    private bool _stopPropagation = false;
}
```

Para más información, vea [Detener la propagación de eventos](xref:blazor/event-handling#stop-event-propagation).

## <a name="detailed-errors-during-opno-locblazor-app-development"></a>Errores detallados durante el desarrollo de aplicaciones Blazor

Cuando una aplicación Blazor no funciona correctamente durante el desarrollo, recibir información detallada del error de la aplicación ayuda a solucionar el problema. Cuando se produce un error, en las aplicaciones Blazor se muestra una barra dorada en la parte inferior de la pantalla:

* Durante el desarrollo, la barra dorada le dirige a la consola del explorador, donde puede ver la excepción.
* En producción, la barra dorada informa al usuario de que se ha producido un error y recomienda actualizar el explorador.

Para más información, vea [Errores detallados durante el desarrollo](xref:blazor/handle-errors#detailed-errors-during-development).
