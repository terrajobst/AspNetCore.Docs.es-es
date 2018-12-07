---
title: Uso de convenciones de API web
author: pranavkm
description: Obtenga más información sobre las convenciones de API web en ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: pranavkm
ms.custom: mvc
ms.date: 11/13/2018
uid: web-api/advanced/conventions
ms.openlocfilehash: ede9a46c160cf6a49aa93da710af0bf0b8f59acc
ms.sourcegitcommit: c4572be5ebb301013a5698caf9b5572b76cb2e34
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "52710080"
---
# <a name="use-web-api-conventions"></a>Uso de convenciones de API web

ASP.NET Core 2.2 presenta una forma de extraer [documentación de API](xref:tutorials/web-api-help-pages-using-swagger) común y aplicarla a varias acciones o controladores, e incluso todos los controladores dentro de un ensamblado. Las convenciones de API web son un sustituto para complementar acciones individuales con [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute). Permiten definir los códigos de estado y tipos de valor devueltos "convencionales" más comunes que se devuelven de la acción con una forma de seleccionar el método de la convención que se aplica a una acción.

De forma predeterminada, ASP.NET Core MVC 2.2 incluye un conjunto de convenciones predeterminadas, `Microsoft.AspNetCore.Mvc.DefaultApiConventions`. Las convenciones se basan en el controlador al que ASP.NET Core aplica la técnica scaffolding. Si sus acciones siguen el patrón que genera el scaffolding, debería poder usar las convenciones predeterminadas correctamente.

En tiempo de ejecución, <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> entiende las convenciones. `ApiExplorer` es la abstracción de MVC para comunicarse con los generadores de documento de Open API. Los atributos de la convención aplicada se asocian a una acción y se incluyen en la documentación de Swagger de la acción. Los analizadores de API también comprenden las convenciones. Si la acción es poco convencional (por ejemplo, devuelve un código de estado no documentado en la convención aplicada), se genera una advertencia, lo cual supone un incentivo para que usted la documente.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="apply-web-api-conventions"></a>Aplicación de convenciones de API web

Hay tres formas de aplicar una convención. Las convenciones no se crean, y cada acción puede estar asociada a exactamente una convención. Las convenciones más específicas, que se detallan a continuación, tienen prioridad sobre las menos específicas. La selección es no determinista cuando se aplican dos o más convenciones de la misma prioridad a una acción. Las siguientes opciones existen para aplicar una convención a una acción, de la más específica a la menos específica:

1. `Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute`: se aplica a las acciones individuales y especifica el tipo de convención y el método de la convención que se aplica. En el ejemplo siguiente, el método de convención `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` se aplica a la acción `Update`:

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventionmethod&highlight=2-3)]

1. `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` aplicado a un controlador: se aplica el tipo de la convención a todas las acciones del controlador. Los métodos de convención se complementan con sugerencias que determinan las acciones a las que deben aplicarse (detalles como parte de la creación de convenciones). Por ejemplo:

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventiontypeattribute)]

1. `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` aplicado a un ensamblado: se aplica el tipo de convención para todos los controladores del ensamblado actual. Por ejemplo:

    [!code-csharp[](conventions/sample/Startup.cs?name=apiconventiontypeattribute)]

## <a name="create-web-api-conventions"></a>Creación de convenciones de API web

Una convención es un tipo estático con métodos. Estos métodos se anotan con atributos `[ProducesResponseType]` o `[ProducesDefaultResponseType]`.

```csharp
public static class MyAppConventions
{
    [ProducesResponseType(200)]
    [ProducesResponseType(404)]
    public static void Find(int id)
    {
    }
}
```

El resultado de aplicar esta convención a un ensamblado es que el método de convención se aplica a cualquier acción que tenga el nombre `Find` y un parámetro `id`, siempre que no tengan otros atributos de metadatos más específicos.

Además de `[ProducesResponseType]` y `[ProducesDefaultResponseType]`, `[ApiConventionNameMatch]` y `[ApiConventionTypeMatch]` se pueden aplicar al método de convención que determina los métodos a los que se aplican. Por ejemplo:

```csharp
[ProducesResponseType(200)]
[ProducesResponseType(404)]
[ApiConventionNameMatch(ApiConventionNameMatchBehavior.Prefix)]
public static void Find(
    [ApiConventionNameMatch(ApiConventionNameMatchBehavior.Suffix)]
    int id)
{ }
```

* La opción `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` aplicada al método indica que la convención puede coincidir con cualquier acción, siempre que esta vaya precedida de "Búsqueda". Esto incluye métodos como `Find`, `FindPet` y `FindById`.
* El valor `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` aplicado al parámetro indica que la convención puede coincidir con los métodos que tengan exactamente un parámetro que termine con el identificador del sufijo. Esto incluye parámetros como `id` y `petId`. `ApiConventionTypeMatch` puede aplicarse de forma similar a los tipos para restringir el tipo del parámetro. Un argumento `params[]` puede usarse para indicar los parámetros restantes que no es necesario que coincidan de forma explícita.
