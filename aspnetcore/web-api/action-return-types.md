---
title: Tipos de valor devuelto de acción del controlador de ASP.NET Core Web API
author: scottaddie
description: Obtenga información sobre los distintos tipos de valor devuelto de método de acción del controlador de ASP.NET Core Web API.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/04/2019
uid: web-api/action-return-types
ms.openlocfilehash: 180d76c2c2e53dbf64b8fcc5cdc6d2b6f4dab6eb
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64887120"
---
# <a name="controller-action-return-types-in-aspnet-core-web-api"></a><span data-ttu-id="44f82-103">Tipos de valor devuelto de acción del controlador de ASP.NET Core Web API</span><span class="sxs-lookup"><span data-stu-id="44f82-103">Controller action return types in ASP.NET Core Web API</span></span>

<span data-ttu-id="44f82-104">Por [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="44f82-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="44f82-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="44f82-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="44f82-106">ASP.NET Core ofrece las siguientes opciones relativas a los tipos de valor devuelto de acción del controlador de Web API:</span><span class="sxs-lookup"><span data-stu-id="44f82-106">ASP.NET Core offers the following options for Web API controller action return types:</span></span>

::: moniker range=">= aspnetcore-2.1"

* [<span data-ttu-id="44f82-107">Tipo específico</span><span class="sxs-lookup"><span data-stu-id="44f82-107">Specific type</span></span>](#specific-type)
* [<span data-ttu-id="44f82-108">IActionResult</span><span class="sxs-lookup"><span data-stu-id="44f82-108">IActionResult</span></span>](#iactionresult-type)
* [<span data-ttu-id="44f82-109">ActionResult\<T></span><span class="sxs-lookup"><span data-stu-id="44f82-109">ActionResult\<T></span></span>](#actionresultt-type)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* [<span data-ttu-id="44f82-110">Tipo específico</span><span class="sxs-lookup"><span data-stu-id="44f82-110">Specific type</span></span>](#specific-type)
* [<span data-ttu-id="44f82-111">IActionResult</span><span class="sxs-lookup"><span data-stu-id="44f82-111">IActionResult</span></span>](#iactionresult-type)

::: moniker-end

<span data-ttu-id="44f82-112">En este documento se explica cuándo resulta más adecuado usar cada tipo de valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="44f82-112">This document explains when it's most appropriate to use each return type.</span></span>

## <a name="specific-type"></a><span data-ttu-id="44f82-113">Tipo específico</span><span class="sxs-lookup"><span data-stu-id="44f82-113">Specific type</span></span>

<span data-ttu-id="44f82-114">La acción más sencilla devuelve un tipo de datos primitivo o complejo (por ejemplo, `string` o un tipo de objeto personalizado).</span><span class="sxs-lookup"><span data-stu-id="44f82-114">The simplest action returns a primitive or complex data type (for example, `string` or a custom object type).</span></span> <span data-ttu-id="44f82-115">Consideremos la siguiente acción, que devuelve una colección de objetos `Product` personalizados:</span><span class="sxs-lookup"><span data-stu-id="44f82-115">Consider the following action, which returns a collection of custom `Product` objects:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_Get)]

<span data-ttu-id="44f82-116">Sin condiciones conocidas de las que haya que protegerse durante la ejecución de la acción, bastaría con que se devolviera un tipo específico.</span><span class="sxs-lookup"><span data-stu-id="44f82-116">Without known conditions to safeguard against during action execution, returning a specific type could suffice.</span></span> <span data-ttu-id="44f82-117">La acción anterior no acepta parámetros, por lo que no se necesita ninguna validación de restricciones de parámetros.</span><span class="sxs-lookup"><span data-stu-id="44f82-117">The preceding action accepts no parameters, so parameter constraints validation isn't needed.</span></span>

<span data-ttu-id="44f82-118">Si existen condiciones conocidas que deban tenerse en cuenta en una acción, se presentan varias rutas de acceso de valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="44f82-118">When known conditions need to be accounted for in an action, multiple return paths are introduced.</span></span> <span data-ttu-id="44f82-119">En tal caso, lo habitual es mezclar un tipo devuelto [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) con el tipo de valor devuelto primitivo o complejo.</span><span class="sxs-lookup"><span data-stu-id="44f82-119">In such a case, it's common to mix an [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) return type with the primitive or complex return type.</span></span> <span data-ttu-id="44f82-120">Se necesitará [IActionResult](#iactionresult-type) o [ActionResult\<T>](#actionresultt-type) para dar cabida a este tipo de acción.</span><span class="sxs-lookup"><span data-stu-id="44f82-120">Either [IActionResult](#iactionresult-type) or [ActionResult\<T>](#actionresultt-type) are necessary to accommodate this type of action.</span></span>

## <a name="iactionresult-type"></a><span data-ttu-id="44f82-121">Tipo IActionResult</span><span class="sxs-lookup"><span data-stu-id="44f82-121">IActionResult type</span></span>

<span data-ttu-id="44f82-122">El tipo de valor devuelto [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) resulta adecuado cuando existen varios tipos de valor devuelto [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) posibles en una acción.</span><span class="sxs-lookup"><span data-stu-id="44f82-122">The [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) return type is appropriate when multiple [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) return types are possible in an action.</span></span> <span data-ttu-id="44f82-123">Los tipos `ActionResult` representan varios códigos de estado HTTP.</span><span class="sxs-lookup"><span data-stu-id="44f82-123">The `ActionResult` types represent various HTTP status codes.</span></span> <span data-ttu-id="44f82-124">Algunos de los tipos de valor devuelto que suelen entrar en esta categoría son [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400), [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404) y [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).</span><span class="sxs-lookup"><span data-stu-id="44f82-124">Some common return types falling into this category are [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400), [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404), and [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).</span></span>

<span data-ttu-id="44f82-125">Dado que hay varios tipos de valor devuelto y rutas de acceso en la acción, es necesario el uso libre del atributo [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor).</span><span class="sxs-lookup"><span data-stu-id="44f82-125">Because there are multiple return types and paths in the action, liberal use of the [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) attribute is necessary.</span></span> <span data-ttu-id="44f82-126">Este atributo genera detalles de respuesta más pormenorizados relativos a las páginas de ayuda de API generadas por herramientas como [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger).</span><span class="sxs-lookup"><span data-stu-id="44f82-126">This attribute produces more descriptive response details for API help pages generated by tools like [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger).</span></span> <span data-ttu-id="44f82-127">`[ProducesResponseType]` indica los tipos conocidos y los códigos de estado HTTP que la acción va a devolver.</span><span class="sxs-lookup"><span data-stu-id="44f82-127">`[ProducesResponseType]` indicates the known types and HTTP status codes to be returned by the action.</span></span>

### <a name="synchronous-action"></a><span data-ttu-id="44f82-128">Acción sincrónica</span><span class="sxs-lookup"><span data-stu-id="44f82-128">Synchronous action</span></span>

<span data-ttu-id="44f82-129">Veamos la siguiente acción sincrónica, en la que pueden darse dos tipos de valor devuelto posibles:</span><span class="sxs-lookup"><span data-stu-id="44f82-129">Consider the following synchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

<span data-ttu-id="44f82-130">En la acción anterior, se devuelve un código de estado 404 cuando el producto representado por `id` no existe en el almacén de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="44f82-130">In the preceding action, a 404 status code is returned when the product represented by `id` doesn't exist in the underlying data store.</span></span> <span data-ttu-id="44f82-131">El método del asistente [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) se invoca como una forma abreviada de `return new NotFoundResult();`.</span><span class="sxs-lookup"><span data-stu-id="44f82-131">The [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) helper method is invoked as a shortcut to `return new NotFoundResult();`.</span></span> <span data-ttu-id="44f82-132">Si el producto existe, se devuelve un objeto `Product` que representa la carga con un código de estado 200.</span><span class="sxs-lookup"><span data-stu-id="44f82-132">If the product does exist, a `Product` object representing the payload is returned with a 200 status code.</span></span> <span data-ttu-id="44f82-133">El método del asistente [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) se invoca como una forma abreviada de `return new OkObjectResult(product);`.</span><span class="sxs-lookup"><span data-stu-id="44f82-133">The [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) helper method is invoked as the shorthand form of `return new OkObjectResult(product);`.</span></span>

### <a name="asynchronous-action"></a><span data-ttu-id="44f82-134">Acción asincrónica</span><span class="sxs-lookup"><span data-stu-id="44f82-134">Asynchronous action</span></span>

<span data-ttu-id="44f82-135">Veamos la siguiente acción asincrónica, en la que pueden darse dos tipos de valor devuelto posibles:</span><span class="sxs-lookup"><span data-stu-id="44f82-135">Consider the following asynchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

<span data-ttu-id="44f82-136">En el código anterior:</span><span class="sxs-lookup"><span data-stu-id="44f82-136">In the preceding code:</span></span>

* <span data-ttu-id="44f82-137">El entorno de ejecución de ASP.NET Core devuelve un código de estado 400 ([BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*)) cuando la descripción del producto contiene "Widget XYZ".</span><span class="sxs-lookup"><span data-stu-id="44f82-137">A 400 status code ([BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*)) is returned by the ASP.NET Core runtime when the product description contains "XYZ Widget".</span></span>
* <span data-ttu-id="44f82-138">Al crear un producto, el método [CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) genera un código de estado 201.</span><span class="sxs-lookup"><span data-stu-id="44f82-138">A 201 status code is generated by the [CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) method when a product is created.</span></span> <span data-ttu-id="44f82-139">En esta ruta de acceso de código, se devuelve el objeto `Product`.</span><span class="sxs-lookup"><span data-stu-id="44f82-139">In this code path, the `Product` object is returned.</span></span>

<span data-ttu-id="44f82-140">Por ejemplo, el siguiente modelo indica que las solicitudes deben incluir las propiedades `Name` y `Description`.</span><span class="sxs-lookup"><span data-stu-id="44f82-140">For example, the following model indicates that requests must include the `Name` and `Description` properties.</span></span> <span data-ttu-id="44f82-141">Por lo tanto, si no se proporcionan `Name` y `Description` en la solicitud, se producirá un error en la validación de los modelos.</span><span class="sxs-lookup"><span data-stu-id="44f82-141">Therefore, failure to provide `Name` and `Description` in the request causes model validation to fail.</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.DataAccess/Models/Product.cs?name=snippet_ProductClass&highlight=5-6,8-9)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="44f82-142">En ASP.NET Core 2.1 o versiones posteriores, si se aplica el atributo [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute), los errores de validación de los modelos generarán un código de estado 400.</span><span class="sxs-lookup"><span data-stu-id="44f82-142">If the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute in ASP.NET Core 2.1 or later  is applied, model validation errors result in a 400 status code.</span></span> <span data-ttu-id="44f82-143">Para obtener más información, consulte [Respuestas HTTP 400 automáticas](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="44f82-143">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

## <a name="actionresultt-type"></a><span data-ttu-id="44f82-144">Tipo ActionResult\<T></span><span class="sxs-lookup"><span data-stu-id="44f82-144">ActionResult\<T> type</span></span>

<span data-ttu-id="44f82-145">ASP.NET Core 2.1 incorpora el tipo de valor devuelto [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) para las acciones de controlador de Web API.</span><span class="sxs-lookup"><span data-stu-id="44f82-145">ASP.NET Core 2.1 introduces the [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) return type for Web API controller actions.</span></span> <span data-ttu-id="44f82-146">Permite devolver un tipo que se deriva de [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) o bien un [tipo específico](#specific-type).</span><span class="sxs-lookup"><span data-stu-id="44f82-146">It enables you to return a type deriving from [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) or return a [specific type](#specific-type).</span></span> <span data-ttu-id="44f82-147">`ActionResult<T>` reporta las siguientes ventajas con frente al [tipo IActionResult](#iactionresult-type):</span><span class="sxs-lookup"><span data-stu-id="44f82-147">`ActionResult<T>` offers the following benefits over the [IActionResult type](#iactionresult-type):</span></span>

* <span data-ttu-id="44f82-148">La propiedad `Type` del atributo [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) se puede excluir.</span><span class="sxs-lookup"><span data-stu-id="44f82-148">The [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) attribute's `Type` property can be excluded.</span></span> <span data-ttu-id="44f82-149">Por ejemplo, `[ProducesResponseType(200, Type = typeof(Product))]` se simplifica a `[ProducesResponseType(200)]`.</span><span class="sxs-lookup"><span data-stu-id="44f82-149">For example, `[ProducesResponseType(200, Type = typeof(Product))]` is simplified to `[ProducesResponseType(200)]`.</span></span> <span data-ttu-id="44f82-150">En su lugar, el tipo de valor devuelto esperado de la acción se infiere desde `T` en `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="44f82-150">The action's expected return type is instead inferred from the `T` in `ActionResult<T>`.</span></span>
* <span data-ttu-id="44f82-151">Los [operadores de conversión implícitos](/dotnet/csharp/language-reference/keywords/implicit) admiten la conversión tanto de `T` como de `ActionResult` en `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="44f82-151">[Implicit cast operators](/dotnet/csharp/language-reference/keywords/implicit) support the conversion of both `T` and `ActionResult` to `ActionResult<T>`.</span></span> <span data-ttu-id="44f82-152">`T` se convierte en [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult), lo que significa que `return new ObjectResult(T);` se ha simplificado para `return T;`.</span><span class="sxs-lookup"><span data-stu-id="44f82-152">`T` converts to [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult), which means `return new ObjectResult(T);` is simplified to `return T;`.</span></span>

<span data-ttu-id="44f82-153">C# no admite operadores de conversión implícitos en las interfaces.</span><span class="sxs-lookup"><span data-stu-id="44f82-153">C# doesn't support implicit cast operators on interfaces.</span></span> <span data-ttu-id="44f82-154">Por consiguiente, la conversión de la interfaz a un tipo concreto es necesaria para usar `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="44f82-154">Consequently, conversion of the interface to a concrete type is necessary to use `ActionResult<T>`.</span></span> <span data-ttu-id="44f82-155">Por ejemplo, el uso de `IEnumerable` en el siguiente ejemplo no funciona:</span><span class="sxs-lookup"><span data-stu-id="44f82-155">For example, use of `IEnumerable` in the following example doesn't work:</span></span>

```csharp
[HttpGet]
public ActionResult<IEnumerable<Product>> Get()
{
    return _repository.GetProducts();
}
```

<span data-ttu-id="44f82-156">Una opción para corregir el código anterior es devolver `_repository.GetProducts().ToList();`.</span><span class="sxs-lookup"><span data-stu-id="44f82-156">One option to fix the preceding code is to return `_repository.GetProducts().ToList();`.</span></span>

<span data-ttu-id="44f82-157">La mayoría de las acciones tiene un tipo de valor devuelto específico.</span><span class="sxs-lookup"><span data-stu-id="44f82-157">Most actions have a specific return type.</span></span> <span data-ttu-id="44f82-158">Se pueden producir condiciones inesperadas que durante la ejecución de una acción, en cuyo caso no se devuelve el tipo específico.</span><span class="sxs-lookup"><span data-stu-id="44f82-158">Unexpected conditions can occur during action execution, in which case the specific type isn't returned.</span></span> <span data-ttu-id="44f82-159">Por ejemplo, puede suceder que el parámetro de entrada de una acción genere un error de validación de modelos.</span><span class="sxs-lookup"><span data-stu-id="44f82-159">For example, an action's input parameter may fail model validation.</span></span> <span data-ttu-id="44f82-160">En tal caso, lo normal es que se devuelva el tipo `ActionResult` correspondiente en lugar del tipo específico.</span><span class="sxs-lookup"><span data-stu-id="44f82-160">In such a case, it's common to return the appropriate `ActionResult` type instead of the specific type.</span></span>

### <a name="synchronous-action"></a><span data-ttu-id="44f82-161">Acción sincrónica</span><span class="sxs-lookup"><span data-stu-id="44f82-161">Synchronous action</span></span>

<span data-ttu-id="44f82-162">Veamos una acción sincrónica, en la que pueden darse dos tipos de valor devuelto posibles:</span><span class="sxs-lookup"><span data-stu-id="44f82-162">Consider a synchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

<span data-ttu-id="44f82-163">En el código anterior, se devuelve un código de estado 404 cuando el producto no existe en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="44f82-163">In the preceding code, a 404 status code is returned when the product doesn't exist in the database.</span></span> <span data-ttu-id="44f82-164">Si el producto existe, se devuelve el objeto `Product` correspondiente.</span><span class="sxs-lookup"><span data-stu-id="44f82-164">If the product does exist, the corresponding `Product` object is returned.</span></span> <span data-ttu-id="44f82-165">Antes de ASP.NET Core 2.1, la línea `return product;` habría sido `return Ok(product);`.</span><span class="sxs-lookup"><span data-stu-id="44f82-165">Before ASP.NET Core 2.1, the `return product;` line would have been `return Ok(product);`.</span></span>

> [!TIP]
> <span data-ttu-id="44f82-166">A partir de ASP.NET Core 2.1, la inferencia del origen de enlace de los parámetros de la acción está habilitada cuando una clase de controlador incluye el atributo `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="44f82-166">As of ASP.NET Core 2.1, action parameter binding source inference is enabled when a controller class is decorated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="44f82-167">Así, un nombre de parámetro que coincida con un nombre de la plantilla de ruta se enlazará automáticamente usando los datos de ruta de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="44f82-167">A parameter name matching a name in the route template is automatically bound using the request route data.</span></span> <span data-ttu-id="44f82-168">Por lo tanto, el parámetro `id` de la acción anterior no está anotado explícitamente con el atributo [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute).</span><span class="sxs-lookup"><span data-stu-id="44f82-168">Consequently, the preceding action's `id` parameter isn't explicitly annotated with the [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute) attribute.</span></span>

### <a name="asynchronous-action"></a><span data-ttu-id="44f82-169">Acción asincrónica</span><span class="sxs-lookup"><span data-stu-id="44f82-169">Asynchronous action</span></span>

<span data-ttu-id="44f82-170">Veamos una acción asincrónica, en la que pueden darse dos tipos de valor devuelto posibles:</span><span class="sxs-lookup"><span data-stu-id="44f82-170">Consider an asynchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

<span data-ttu-id="44f82-171">En el código anterior:</span><span class="sxs-lookup"><span data-stu-id="44f82-171">In the preceding code:</span></span>

* <span data-ttu-id="44f82-172">El entorno de ejecución de ASP.NET Core devuelve un código de estado 400 ([BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*)) en los casos siguientes:</span><span class="sxs-lookup"><span data-stu-id="44f82-172">A 400 status code ([BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*)) is returned by the ASP.NET Core runtime when:</span></span>
  * <span data-ttu-id="44f82-173">El atributo [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) se ha aplicado y se produce un error de validación de los modelos.</span><span class="sxs-lookup"><span data-stu-id="44f82-173">The [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute has been applied and model validation fails.</span></span>
  * <span data-ttu-id="44f82-174">La descripción del producto contiene "Widget XYZ".</span><span class="sxs-lookup"><span data-stu-id="44f82-174">The product description contains "XYZ Widget".</span></span>
* <span data-ttu-id="44f82-175">Al crear un producto, el método [CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) genera un código de estado 201.</span><span class="sxs-lookup"><span data-stu-id="44f82-175">A 201 status code is generated by the [CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) method when a product is created.</span></span> <span data-ttu-id="44f82-176">En esta ruta de acceso de código, se devuelve el objeto `Product`.</span><span class="sxs-lookup"><span data-stu-id="44f82-176">In this code path, the `Product` object is returned.</span></span>

> [!TIP]
> <span data-ttu-id="44f82-177">A partir de ASP.NET Core 2.1, la inferencia del origen de enlace de los parámetros de la acción está habilitada cuando una clase de controlador incluye el atributo `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="44f82-177">As of ASP.NET Core 2.1, action parameter binding source inference is enabled when a controller class is decorated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="44f82-178">Los parámetros de tipo complejo se enlazan automáticamente usando el cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="44f82-178">Complex type parameters are automatically bound using the request body.</span></span> <span data-ttu-id="44f82-179">Por lo tanto, el parámetro `product` de la acción anterior no está anotado explícitamente con el atributo [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute).</span><span class="sxs-lookup"><span data-stu-id="44f82-179">Consequently, the preceding action's `product` parameter isn't explicitly annotated with the [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="44f82-180">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="44f82-180">Additional resources</span></span>

* <xref:mvc/controllers/actions>
* <xref:mvc/models/validation>
* <xref:tutorials/web-api-help-pages-using-swagger>
