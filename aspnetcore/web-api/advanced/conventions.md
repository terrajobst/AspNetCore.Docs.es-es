---
title: Uso de convenciones de API web
author: pranavkm
description: Obtenga más información sobre las convenciones de API web en ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: pranavkm
ms.custom: mvc
ms.date: 11/13/2018
uid: web-api/advanced/conventions
ms.openlocfilehash: 023b8d09511aa42966e2a7d1c85e407bb6e79b0f
ms.sourcegitcommit: f202864efca81a72ea7120c0692940c40d9d0630
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "51635406"
---
# <a name="use-web-api-conventions"></a><span data-ttu-id="276db-103">Uso de convenciones de API web</span><span class="sxs-lookup"><span data-stu-id="276db-103">Use web API conventions</span></span>

<span data-ttu-id="276db-104">ASP.NET Core 2.2 presenta una forma de extraer [documentación de API](xref:tutorials/web-api-help-pages-using-swagger) común y aplicarla a varias acciones o controladores, e incluso todos los controladores dentro de un ensamblado.</span><span class="sxs-lookup"><span data-stu-id="276db-104">ASP.NET Core 2.2 introduces a way to extract common [API documentation](xref:tutorials/web-api-help-pages-using-swagger) and apply it to multiple actions, controllers, or all controllers within an assembly.</span></span> <span data-ttu-id="276db-105">Las convenciones de API web son un sustituto para complementar acciones individuales con [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute).</span><span class="sxs-lookup"><span data-stu-id="276db-105">Web API conventions are a substitute for decorating individual actions with [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute).</span></span> <span data-ttu-id="276db-106">Permiten definir los códigos de estado y tipos de valor devueltos "convencionales" más comunes que se devuelven de la acción con una forma de seleccionar el método de la convención que se aplica a una acción.</span><span class="sxs-lookup"><span data-stu-id="276db-106">It allows you to define the most common "conventional" return types and status codes that you return from your action with a way to select the convention method that applies to an action.</span></span>

<span data-ttu-id="276db-107">De forma predeterminada, ASP.NET Core MVC 2.2 incluye un conjunto de convenciones predeterminadas, `Microsoft.AspNetCore.Mvc.DefaultApiConventions`.</span><span class="sxs-lookup"><span data-stu-id="276db-107">By default, ASP.NET Core MVC 2.2 ships with a set of default conventions, `Microsoft.AspNetCore.Mvc.DefaultApiConventions`.</span></span> <span data-ttu-id="276db-108">Las convenciones se basan en el controlador al que ASP.NET Core aplica la técnica scaffolding.</span><span class="sxs-lookup"><span data-stu-id="276db-108">The conventions are based on the controller that ASP.NET Core scaffolds.</span></span> <span data-ttu-id="276db-109">Si sus acciones siguen el patrón que genera el scaffolding, debería poder usar las convenciones predeterminadas correctamente.</span><span class="sxs-lookup"><span data-stu-id="276db-109">If your actions follow the pattern that scaffolding produces, you should be successful using the default conventions.</span></span>

<span data-ttu-id="276db-110">En tiempo de ejecución, <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> entiende las convenciones.</span><span class="sxs-lookup"><span data-stu-id="276db-110">At runtime, <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> understands conventions.</span></span> <span data-ttu-id="276db-111">`ApiExplorer` es la abstracción de MVC para comunicarse con los generadores de documento de Open API.</span><span class="sxs-lookup"><span data-stu-id="276db-111">`ApiExplorer` is MVC's abstraction to communicate with Open API document generators.</span></span> <span data-ttu-id="276db-112">Los atributos de la convención aplicada se asocian a una acción y se incluyen en la documentación de Swagger de la acción.</span><span class="sxs-lookup"><span data-stu-id="276db-112">Attributes from the applied convention get associated with an action and are included in the action's Swagger documentation.</span></span> <span data-ttu-id="276db-113">Los analizadores de API también comprenden las convenciones.</span><span class="sxs-lookup"><span data-stu-id="276db-113">API analyzers also understand conventions.</span></span> <span data-ttu-id="276db-114">Si la acción es poco convencional (por ejemplo, devuelve un código de estado no documentado en la convención aplicada), se genera una advertencia, lo cual supone un incentivo para que usted la documente.</span><span class="sxs-lookup"><span data-stu-id="276db-114">If your action is unconventional (for example, it returns a status code that isn't documented by the applied convention), it produces a warning, encouraging you to document it.</span></span>

<span data-ttu-id="276db-115">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="276db-115">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="apply-web-api-conventions"></a><span data-ttu-id="276db-116">Aplicación de convenciones de API web</span><span class="sxs-lookup"><span data-stu-id="276db-116">Apply web API conventions</span></span>

<span data-ttu-id="276db-117">Hay tres formas de aplicar una convención.</span><span class="sxs-lookup"><span data-stu-id="276db-117">There are three ways to apply a convention.</span></span> <span data-ttu-id="276db-118">Las convenciones no se crean, y cada acción puede estar asociada a exactamente una convención.</span><span class="sxs-lookup"><span data-stu-id="276db-118">Conventions don't compose, each action may be associated with exactly one convention.</span></span> <span data-ttu-id="276db-119">Las convenciones más específicas, que se detallan a continuación, tienen prioridad sobre las menos específicas.</span><span class="sxs-lookup"><span data-stu-id="276db-119">More specific conventions (detailed below) take precedence over less specific ones.</span></span> <span data-ttu-id="276db-120">La selección es no determinista cuando se aplican dos o más convenciones de la misma prioridad a una acción.</span><span class="sxs-lookup"><span data-stu-id="276db-120">The selection is non-deterministic when two or more conventions of the same priority apply to an action.</span></span> <span data-ttu-id="276db-121">Las siguientes opciones existen para aplicar una convención a una acción, de la más específica a la menos específica:</span><span class="sxs-lookup"><span data-stu-id="276db-121">The following options exist to apply a convention to an action, from the most specific to the least specific:</span></span>

1. <span data-ttu-id="276db-122">`Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute`: se aplica a las acciones individuales y especifica el tipo de convención y el método de la convención que se aplica.</span><span class="sxs-lookup"><span data-stu-id="276db-122">`Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; Applies to individual actions and specifies the convention type and the convention method that applies.</span></span> <span data-ttu-id="276db-123">En el ejemplo siguiente, el método de convención `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` se aplica a la acción `Update`:</span><span class="sxs-lookup"><span data-stu-id="276db-123">In the following sample, the convention method `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` is applied to the `Update` action:</span></span>

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventionmethod&highlight=2-3)]

1. <span data-ttu-id="276db-124">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` aplicado a un controlador: se aplica el tipo de la convención a todas las acciones del controlador.</span><span class="sxs-lookup"><span data-stu-id="276db-124">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applied to a controller &mdash; Applies the convention type to all actions on the controller.</span></span> <span data-ttu-id="276db-125">Los métodos de convención se complementan con sugerencias que determinan las acciones a las que deben aplicarse (detalles como parte de la creación de convenciones).</span><span class="sxs-lookup"><span data-stu-id="276db-125">Convention methods are decorated with hints that determine which actions it would apply to (details as part of authoring conventions).</span></span> <span data-ttu-id="276db-126">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="276db-126">For example:</span></span>

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventiontypeattribute)]

1. <span data-ttu-id="276db-127">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` aplicado a un ensamblado: se aplica el tipo de convención para todos los controladores del ensamblado actual.</span><span class="sxs-lookup"><span data-stu-id="276db-127">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applied to an assembly &mdash; Applies the convention type to all controllers in the current assembly.</span></span> <span data-ttu-id="276db-128">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="276db-128">For example:</span></span>

    [!code-csharp[](conventions/sample/Startup.cs?name=apiconventiontypeattribute)]

## <a name="create-web-api-conventions"></a><span data-ttu-id="276db-129">Creación de convenciones de API web</span><span class="sxs-lookup"><span data-stu-id="276db-129">Create web API conventions</span></span>

<span data-ttu-id="276db-130">Una convención es un tipo estático con métodos.</span><span class="sxs-lookup"><span data-stu-id="276db-130">A convention is a static type with methods.</span></span> <span data-ttu-id="276db-131">Estos métodos se anotan con atributos `[ProducesResponseType]` o `[ProducesDefaultResponseType]`.</span><span class="sxs-lookup"><span data-stu-id="276db-131">These methods are annotated with `[ProducesResponseType]` or `[ProducesDefaultResponseType]` attributes.</span></span>

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

<span data-ttu-id="276db-132">El resultado de aplicar esta convención a un ensamblado es que el método de convención se aplica a cualquier acción que tenga el nombre `Find` y un parámetro `id`, siempre que no tengan otros atributos de metadatos más específicos.</span><span class="sxs-lookup"><span data-stu-id="276db-132">Applying this convention to an assembly results in the convention method applying to any action with the name `Find` and having exactly one parameter named `id`, as long as they don't have other more specific metadata attributes.</span></span>

<span data-ttu-id="276db-133">Además de `[ProducesResponseType]` y `[ProducesDefaultResponseType]`, `[ApiConventionNameMatch]` y `[ApiConventionTypeMatch]` se pueden aplicar al método de convención que determina los métodos a los que se aplican.</span><span class="sxs-lookup"><span data-stu-id="276db-133">In addition to `[ProducesResponseType]` and `[ProducesDefaultResponseType]`, `[ApiConventionNameMatch]` and `[ApiConventionTypeMatch]` can be applied to the convention method that determines the methods they apply to.</span></span> <span data-ttu-id="276db-134">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="276db-134">For example:</span></span>

```csharp
[ProducesResponseType(200)]
[ProducesResponseType(404)]
[ApiConventionNameMatch(ApiConventionNameMatchBehavior.Prefix)]
public static void Find(
    [ApiConventionNameMatch(ApiConventionNameMatchBehavior.Suffix)]
    int id)
{ }
```

* <span data-ttu-id="276db-135">La opción `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` aplicada al método indica que la convención puede coincidir con cualquier acción, siempre que esta vaya precedida de "Búsqueda".</span><span class="sxs-lookup"><span data-stu-id="276db-135">The `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` option applied to the method, indicates that the convention can match any action as long as it's prefixed with "Find".</span></span> <span data-ttu-id="276db-136">Esto incluye métodos como `Find`, `FindPet` y `FindById`.</span><span class="sxs-lookup"><span data-stu-id="276db-136">This includes methods such as `Find`, `FindPet`, or `FindById`.</span></span>
* <span data-ttu-id="276db-137">El valor `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` aplicado al parámetro indica que la convención puede coincidir con los métodos que tengan exactamente un parámetro que termine con el identificador del sufijo.</span><span class="sxs-lookup"><span data-stu-id="276db-137">The `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` applied to the parameter indicates that the convention can match methods with exactly one parameter ending in the suffix identifier.</span></span> <span data-ttu-id="276db-138">Esto incluye parámetros como `id` y `petId`.</span><span class="sxs-lookup"><span data-stu-id="276db-138">This includes parameters such as `id` or `petId`.</span></span> <span data-ttu-id="276db-139">`ApiConventionTypeMatch` puede aplicarse de forma similar a los tipos para restringir el tipo del parámetro.</span><span class="sxs-lookup"><span data-stu-id="276db-139">`ApiConventionTypeMatch` can be similarly applied to types to constrain the type of the parameter.</span></span> <span data-ttu-id="276db-140">Un argumento `params[]` puede usarse para indicar los parámetros restantes que no es necesario que coincidan de forma explícita.</span><span class="sxs-lookup"><span data-stu-id="276db-140">A `params[]` argument can be used to indicate remaining parameters that don't need to be explicitly matched.</span></span>
