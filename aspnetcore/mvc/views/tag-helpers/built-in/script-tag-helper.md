---
title: Asistente de etiquetas de script en ASP.NET Core
author: rick-anderson
ms.author: riande
description: Descubra los atributos del asistente de etiquetas de script de ASP.NET Core y el papel que desempeña cada atributo al ampliar el comportamiento de la etiqueta de script de código HTML.
ms.custom: mvc
ms.date: 12/02/2019
uid: mvc/views/tag-helpers/builtin-th/script-tag-helper
ms.openlocfilehash: 8a90eb5a74ff3f8178a47c59ad7ba1b6a389ab87
ms.sourcegitcommit: 3b6b0a54b20dc99b0c8c5978400c60adf431072f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74717382"
---
# <a name="script-tag-helper-in-aspnet-core"></a>Asistente de etiquetas de script en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

El [asistente de etiquetas de script](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) genera un vínculo a un archivo de script principal o de reserva. Normalmente, el archivo de script principal se encuentra en una red [Content Delivery Network](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).

[!INCLUDE[](~/includes/cdn.md)]

El asistente de etiquetas de script permite especificar una red CDN para el archivo de script y una reserva cuando dicha red no está disponible. El asistente de etiquetas de script proporciona la ventaja de rendimiento de una red CDN con la solidez del hospedaje local.

En el marcado de Razor siguiente se muestra un elemento `script` con una reserva:

```HTML
<script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-3.3.1.min.js"
        asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
        asp-fallback-test="window.jQuery"
        crossorigin="anonymous"
        integrity="sha384-tsQFqpEReu7ZLhBV2VZlAu7zcOV+rXbYlF2cqB8txI/8aZajjp4Bqd+V6D5IgvKT">
</script>
```

No use el atributo [defer](https://developer.mozilla.org/docs/Web/HTML/Element/script) del elemento `<script>` para diferir la carga del script de CDN. El Asistente de etiquetas de script representa JavaScript que ejecuta de inmediato la expresión [asp-fallback-test](#asp-fallback-test). Se produce un error en la expresión si se difiere la carga del script de CDN.

## <a name="commonly-used-script-tag-helper-attributes"></a>Atributos del asistente de etiquetas de script utilizados habitualmente

Consulte [Asistente de etiquetas de script](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) para todos los atributos, propiedades y métodos del asistente de etiquetas de script.

### <a name="asp-fallback-test"></a>asp-fallback-test

Método de script definido en el script principal que se va a usar para la prueba de reserva. Para más información, consulte <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper.FallbackTestExpression>.

### <a name="asp-fallback-src"></a>asp-fallback-src

Dirección URL de una etiqueta de script en la que se va a realizar la reserva en caso de error en principal. Para más información, consulte <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper.FallbackSrc>.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
* <xref:mvc/compatibility-version>
