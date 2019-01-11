---
title: Uso de convenciones de API web
author: pranavkm
description: Obtenga más información sobre las convenciones de API web en ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: scaddie
ms.custom: mvc
ms.date: 12/13/2018
uid: web-api/advanced/conventions
ms.openlocfilehash: 481e3810f1e1aca40e0ee1ce3da6c67dc9d841f4
ms.sourcegitcommit: 6548c19f345850ee22b50f7ef9fca732895d9e08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2018
ms.locfileid: "53425112"
---
# <a name="use-web-api-conventions"></a><span data-ttu-id="58d40-103">Uso de convenciones de API web</span><span class="sxs-lookup"><span data-stu-id="58d40-103">Use web API conventions</span></span>

<span data-ttu-id="58d40-104">Por [Pranav Krishnamoorthy](https://github.com/pranavkm) y [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="58d40-104">By [Pranav Krishnamoorthy](https://github.com/pranavkm) and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="58d40-105">ASP.NET Core 2.2 (y versiones posteriores) incluye una forma de extraer la [documentación de API](xref:tutorials/web-api-help-pages-using-swagger) común y aplicarla a varias acciones o varios controladores, e incluso a todos los controladores dentro de un ensamblado.</span><span class="sxs-lookup"><span data-stu-id="58d40-105">ASP.NET Core 2.2 and later includes a way to extract common [API documentation](xref:tutorials/web-api-help-pages-using-swagger) and apply it to multiple actions, controllers, or all controllers within an assembly.</span></span> <span data-ttu-id="58d40-106">Las convenciones de API web son un sustituto para complementar acciones individuales con [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute).</span><span class="sxs-lookup"><span data-stu-id="58d40-106">Web API conventions are a substitute for decorating individual actions with [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute).</span></span>

<span data-ttu-id="58d40-107">Una convención permite lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="58d40-107">A convention allows you to:</span></span>

* <span data-ttu-id="58d40-108">Definir los tipos de valor devuelto más comunes y los códigos de estado devueltos a partir de un tipo específico de acción.</span><span class="sxs-lookup"><span data-stu-id="58d40-108">Define the most common return types and status codes returned from a specific type of action.</span></span>
* <span data-ttu-id="58d40-109">Identificar las acciones que no siguen el estándar definido.</span><span class="sxs-lookup"><span data-stu-id="58d40-109">Identify actions that deviate from the defined standard.</span></span>

<span data-ttu-id="58d40-110">ASP.NET Core MVC 2.2 (y versiones posteriores) incluye un conjunto de convenciones predeterminadas en `Microsoft.AspNetCore.Mvc.DefaultApiConventions`.</span><span class="sxs-lookup"><span data-stu-id="58d40-110">ASP.NET Core MVC 2.2 and later includes a set of default conventions in `Microsoft.AspNetCore.Mvc.DefaultApiConventions`.</span></span> <span data-ttu-id="58d40-111">Las convenciones se basan en el controlador (*ValuesController.cs*) proporcionado en la plantilla de proyecto de la **API** de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="58d40-111">The conventions are based on the controller (*ValuesController.cs*) provided in the ASP.NET Core **API** project template.</span></span> <span data-ttu-id="58d40-112">Si sus acciones siguen los patrones de la plantilla, debería poder usar las convenciones predeterminadas correctamente.</span><span class="sxs-lookup"><span data-stu-id="58d40-112">If your actions follow the patterns in the template, you should be successful using the default conventions.</span></span> <span data-ttu-id="58d40-113">Si las convenciones predeterminadas no satisfacen sus necesidades, consulte [Creación de convenciones de API web](#create-web-api-conventions).</span><span class="sxs-lookup"><span data-stu-id="58d40-113">If the default conventions don't meet your needs, see [Create web API conventions](#create-web-api-conventions).</span></span>

<span data-ttu-id="58d40-114">En tiempo de ejecución, <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> entiende las convenciones.</span><span class="sxs-lookup"><span data-stu-id="58d40-114">At runtime, <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> understands conventions.</span></span> <span data-ttu-id="58d40-115">`ApiExplorer` es la abstracción de MVC para comunicarse con los generadores de documento de [OpenAPI](https://www.openapis.org/), conocido también como Swagger.</span><span class="sxs-lookup"><span data-stu-id="58d40-115">`ApiExplorer` is MVC's abstraction to communicate with [OpenAPI](https://www.openapis.org/) (also known as Swagger) document generators.</span></span> <span data-ttu-id="58d40-116">Los atributos de la convención aplicada se asocian a una acción y se incluyen en la documentación de OpenAPI de la acción.</span><span class="sxs-lookup"><span data-stu-id="58d40-116">Attributes from the applied convention are associated with an action and are included in the action's OpenAPI documentation.</span></span> <span data-ttu-id="58d40-117">Los [analizadores de API](xref:web-api/advanced/analyzers) también comprenden las convenciones.</span><span class="sxs-lookup"><span data-stu-id="58d40-117">[API analyzers](xref:web-api/advanced/analyzers) also understand conventions.</span></span> <span data-ttu-id="58d40-118">Si la acción no es convencional (por ejemplo, devuelve un código de estado no documentado en la convención aplicada), recibirá una advertencia en la que se le animará a documentar el código de estado.</span><span class="sxs-lookup"><span data-stu-id="58d40-118">If your action is unconventional (for example, it returns a status code that isn't documented by the applied convention), a warning encourages you to document the status code.</span></span>

<span data-ttu-id="58d40-119">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="58d40-119">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="apply-web-api-conventions"></a><span data-ttu-id="58d40-120">Aplicación de convenciones de API web</span><span class="sxs-lookup"><span data-stu-id="58d40-120">Apply web API conventions</span></span>

<span data-ttu-id="58d40-121">Las convenciones no se crean, y es posible que cada acción esté asociada a una única convención.</span><span class="sxs-lookup"><span data-stu-id="58d40-121">Conventions don't compose; each action may be associated with exactly one convention.</span></span> <span data-ttu-id="58d40-122">Las convenciones más específicas tienen prioridad sobre las menos específicas.</span><span class="sxs-lookup"><span data-stu-id="58d40-122">More specific conventions take precedence over less specific conventions.</span></span> <span data-ttu-id="58d40-123">La selección es no determinista cuando se aplican dos o más convenciones de la misma prioridad a una acción.</span><span class="sxs-lookup"><span data-stu-id="58d40-123">The selection is non-deterministic when two or more conventions of the same priority apply to an action.</span></span> <span data-ttu-id="58d40-124">Las siguientes opciones existen para aplicar una convención a una acción, de la más específica a la menos específica:</span><span class="sxs-lookup"><span data-stu-id="58d40-124">The following options exist to apply a convention to an action, from the most specific to the least specific:</span></span>

1. <span data-ttu-id="58d40-125">`Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute`: se aplica a las acciones individuales y especifica el tipo de convención y el método de la convención que se aplica.</span><span class="sxs-lookup"><span data-stu-id="58d40-125">`Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; Applies to individual actions and specifies the convention type and the convention method that applies.</span></span>

    <span data-ttu-id="58d40-126">En el ejemplo siguiente, el método de convención `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` del tipo de convención predeterminada se aplica a la acción `Update`:</span><span class="sxs-lookup"><span data-stu-id="58d40-126">In the following example, the default convention type's `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` convention method is applied to the `Update` action:</span></span>

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=snippet_ApiConventionMethod&highlight=3)]

    <span data-ttu-id="58d40-127">El método de convención `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` aplica los siguientes atributos a la acción:</span><span class="sxs-lookup"><span data-stu-id="58d40-127">The `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` convention method applies the following attributes to the action:</span></span>

    ```csharp
    [ProducesDefaultResponseType]
    [ProducesResponseType(204)]
    [ProducesResponseType(404)]
    [ProducesResponseType(400)]
    ```

1. <span data-ttu-id="58d40-128">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` aplicado a un controlador: se aplica el tipo de convención especificado a todas las acciones del controlador.</span><span class="sxs-lookup"><span data-stu-id="58d40-128">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applied to a controller &mdash; Applies the specified convention type to all actions on the controller.</span></span> <span data-ttu-id="58d40-129">Un método de convención se representa con sugerencias que determinan las acciones a las que este se aplica.</span><span class="sxs-lookup"><span data-stu-id="58d40-129">A convention method is decorated with hints that determine the actions to which the convention method applies.</span></span> <span data-ttu-id="58d40-130">Para obtener más información sobre las sugerencias, consulte [Creación de convenciones de API web](#create-web-api-conventions).</span><span class="sxs-lookup"><span data-stu-id="58d40-130">For more information on hints, see [Create web API conventions](#create-web-api-conventions)).</span></span>

    <span data-ttu-id="58d40-131">En el ejemplo siguiente, el conjunto predeterminado de convenciones se aplica a todas las acciones de *ContactsConventionController*:</span><span class="sxs-lookup"><span data-stu-id="58d40-131">In the following example, the default set of conventions is applied to all actions in *ContactsConventionController*:</span></span>

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=snippet_ApiConventionTypeAttribute&highlight=2)]

1. <span data-ttu-id="58d40-132">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` aplicado a un ensamblado: se aplica el tipo de convención especificado a todos los controladores del ensamblado actual.</span><span class="sxs-lookup"><span data-stu-id="58d40-132">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applied to an assembly &mdash; Applies the specified convention type to all controllers in the current assembly.</span></span> <span data-ttu-id="58d40-133">Se recomienda aplicar atributos de nivel de ensamblado a la clase `Startup`.</span><span class="sxs-lookup"><span data-stu-id="58d40-133">As a recommendation, apply assembly-level attributes to the `Startup` class.</span></span>

    <span data-ttu-id="58d40-134">En el ejemplo siguiente, el conjunto predeterminado de convenciones se aplica a todos los controladores del ensamblado:</span><span class="sxs-lookup"><span data-stu-id="58d40-134">In the following example, the default set of conventions is applied to all controllers in the assembly:</span></span>

    [!code-csharp[](conventions/sample/Startup.cs?name=snippet_ApiConventionTypeAttribute&highlight=1)]

## <a name="create-web-api-conventions"></a><span data-ttu-id="58d40-135">Creación de convenciones de API web</span><span class="sxs-lookup"><span data-stu-id="58d40-135">Create web API conventions</span></span>

<span data-ttu-id="58d40-136">Si las convenciones de API predeterminadas no satisfacen sus necesidades, cree sus propias convenciones.</span><span class="sxs-lookup"><span data-stu-id="58d40-136">If the default API conventions don't meet your needs, create your own conventions.</span></span> <span data-ttu-id="58d40-137">Una convención es:</span><span class="sxs-lookup"><span data-stu-id="58d40-137">A convention is:</span></span>

* <span data-ttu-id="58d40-138">Un tipo estático con métodos.</span><span class="sxs-lookup"><span data-stu-id="58d40-138">A static type with methods.</span></span>
* <span data-ttu-id="58d40-139">Capaz de definir [tipos de respuesta](#response-types) y [requisitos de nomenclatura](#naming-requirements) en acciones.</span><span class="sxs-lookup"><span data-stu-id="58d40-139">Capable of defining [response types](#response-types) and [naming requirements](#naming-requirements) on actions.</span></span>

### <a name="response-types"></a><span data-ttu-id="58d40-140">Tipos de respuesta</span><span class="sxs-lookup"><span data-stu-id="58d40-140">Response types</span></span>

<span data-ttu-id="58d40-141">Estos métodos se anotan con atributos `[ProducesResponseType]` o `[ProducesDefaultResponseType]`.</span><span class="sxs-lookup"><span data-stu-id="58d40-141">These methods are annotated with `[ProducesResponseType]` or `[ProducesDefaultResponseType]` attributes.</span></span> <span data-ttu-id="58d40-142">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="58d40-142">For example:</span></span>

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

<span data-ttu-id="58d40-143">Si no se presentan atributos de metadatos más específicos, la aplicación de esta convención a un ensamblado requiere lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="58d40-143">If more specific metadata attributes are absent, applying this convention to an assembly enforces that:</span></span>

* <span data-ttu-id="58d40-144">El método de convención debe aplicarse a cualquier acción denominada `Find`.</span><span class="sxs-lookup"><span data-stu-id="58d40-144">The convention method applies to any action named `Find`.</span></span>
* <span data-ttu-id="58d40-145">Un parámetro denominado `id` debe estar presente en la acción `Find`.</span><span class="sxs-lookup"><span data-stu-id="58d40-145">A parameter named `id` is present on the `Find` action.</span></span>

### <a name="naming-requirements"></a><span data-ttu-id="58d40-146">Requisitos de nomenclatura</span><span class="sxs-lookup"><span data-stu-id="58d40-146">Naming requirements</span></span>

<span data-ttu-id="58d40-147">Los atributos `[ApiConventionNameMatch]` y `[ApiConventionTypeMatch]` pueden aplicarse al método de convención que determina las acciones a las que se aplican.</span><span class="sxs-lookup"><span data-stu-id="58d40-147">The `[ApiConventionNameMatch]` and `[ApiConventionTypeMatch]` attributes can be applied to the convention method that determines the actions to which they apply.</span></span> <span data-ttu-id="58d40-148">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="58d40-148">For example:</span></span>

```csharp
[ProducesResponseType(200)]
[ProducesResponseType(404)]
[ApiConventionNameMatch(ApiConventionNameMatchBehavior.Prefix)]
public static void Find(
    [ApiConventionNameMatch(ApiConventionNameMatchBehavior.Suffix)]
    int id)
{ }
```

<span data-ttu-id="58d40-149">En el ejemplo anterior:</span><span class="sxs-lookup"><span data-stu-id="58d40-149">In the preceding example:</span></span>

* <span data-ttu-id="58d40-150">La opción `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` aplicada al método indica que la convención coincide con cualquier acción que vaya precedida de "Búsqueda".</span><span class="sxs-lookup"><span data-stu-id="58d40-150">The `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` option applied to the method indicates that the convention matches any action prefixed with "Find".</span></span> <span data-ttu-id="58d40-151">Entre los ejemplos de acciones coincidentes se incluyen `Find`, `FindPet` y `FindById`.</span><span class="sxs-lookup"><span data-stu-id="58d40-151">Examples of matching actions include `Find`, `FindPet`, and `FindById`.</span></span>
* <span data-ttu-id="58d40-152">El valor `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` aplicado al parámetro indica que la convención coincide con los métodos que tengan exactamente un parámetro que termine con el identificador del sufijo.</span><span class="sxs-lookup"><span data-stu-id="58d40-152">The `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` applied to the parameter indicates that the convention matches methods with exactly one parameter ending in the suffix identifier.</span></span> <span data-ttu-id="58d40-153">Entre los ejemplos se incluyen parámetros tales como `id` o `petId`.</span><span class="sxs-lookup"><span data-stu-id="58d40-153">Examples include parameters such as `id` or `petId`.</span></span> <span data-ttu-id="58d40-154">`ApiConventionTypeMatch` puede aplicarse de forma similar a los tipos para restringir el tipo de parámetro.</span><span class="sxs-lookup"><span data-stu-id="58d40-154">`ApiConventionTypeMatch` can be similarly applied to types to constrain the parameter type.</span></span> <span data-ttu-id="58d40-155">Un argumento `params[]` indica los parámetros restantes que no tienen que coincidir explícitamente.</span><span class="sxs-lookup"><span data-stu-id="58d40-155">A `params[]` argument indicates remaining parameters that don't need to be explicitly matched.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="58d40-156">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="58d40-156">Additional resources</span></span>

* <xref:web-api/advanced/analyzers>
* <xref:tutorials/web-api-help-pages-using-swagger>
