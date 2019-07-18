---
title: JsonPatch en la API web de ASP.NET Core
author: tdykstra
description: Aprenda a administrar solicitudes JSON Patch en una API web ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/24/2019
uid: web-api/jsonpatch
ms.openlocfilehash: 97264903d85dbb397e85fdbf7b070e2aaae74bc8
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815550"
---
# <a name="jsonpatch-in-aspnet-core-web-api"></a><span data-ttu-id="04ec3-103">JsonPatch en la API web de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="04ec3-103">JsonPatch in ASP.NET Core web API</span></span>

<span data-ttu-id="04ec3-104">Por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="04ec3-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="04ec3-105">En este artículo se explica cómo administrar solicitudes JSON Patch en una API web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="04ec3-105">This article explains how to handle JSON Patch requests in an ASP.NET Core web API.</span></span>

## <a name="patch-http-request-method"></a><span data-ttu-id="04ec3-106">Método de solicitud HTTP PATCH</span><span class="sxs-lookup"><span data-stu-id="04ec3-106">PATCH HTTP request method</span></span>

<span data-ttu-id="04ec3-107">Los métodos PUT y [PATCH](https://tools.ietf.org/html/rfc5789) se usan para actualizar un recurso existente.</span><span class="sxs-lookup"><span data-stu-id="04ec3-107">The PUT and [PATCH](https://tools.ietf.org/html/rfc5789) methods are used to update an existing resource.</span></span> <span data-ttu-id="04ec3-108">La diferencia entre ellos es que PUT reemplaza el recurso entero, mientras que PATCH especifica únicamente los cambios.</span><span class="sxs-lookup"><span data-stu-id="04ec3-108">The difference between them is that PUT replaces the entire resource, while PATCH specifies only the changes.</span></span>

## <a name="json-patch"></a><span data-ttu-id="04ec3-109">JSON Patch</span><span class="sxs-lookup"><span data-stu-id="04ec3-109">JSON Patch</span></span>

<span data-ttu-id="04ec3-110">[JSON Patch](https://tools.ietf.org/html/rfc6902) es un formato para especificar las actualizaciones que se aplicarán a un recurso.</span><span class="sxs-lookup"><span data-stu-id="04ec3-110">[JSON Patch](https://tools.ietf.org/html/rfc6902) is a format for specifying updates to be applied to a resource.</span></span> <span data-ttu-id="04ec3-111">Un documento JSON Patch tiene una matriz de *operaciones*.</span><span class="sxs-lookup"><span data-stu-id="04ec3-111">A JSON Patch document has an array of *operations*.</span></span> <span data-ttu-id="04ec3-112">Cada operación identifica un determinado tipo de cambio, como agregar un elemento de matriz o reemplazar un valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="04ec3-112">Each operation identifies a particular type of change, such as add an array element or replace a property value.</span></span>

<span data-ttu-id="04ec3-113">Por ejemplo, los documentos JSON siguientes representan un recurso, un documento de revisión JSON para el recurso y el resultado de aplicar las operaciones de revisión.</span><span class="sxs-lookup"><span data-stu-id="04ec3-113">For example, the following JSON documents represent a resource, a JSON patch document for the resource, and the result of applying the patch operations.</span></span>

### <a name="resource-example"></a><span data-ttu-id="04ec3-114">Ejemplo de recursos</span><span class="sxs-lookup"><span data-stu-id="04ec3-114">Resource example</span></span>

[!code-csharp[](jsonpatch/samples/2.2/JSON/customer.json)]

### <a name="json-patch-example"></a><span data-ttu-id="04ec3-115">Ejemplo de revisión de JSON</span><span class="sxs-lookup"><span data-stu-id="04ec3-115">JSON patch example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

<span data-ttu-id="04ec3-116">En el código JSON anterior:</span><span class="sxs-lookup"><span data-stu-id="04ec3-116">In the preceding JSON:</span></span>

* <span data-ttu-id="04ec3-117">La propiedad `op` indica el tipo de operación.</span><span class="sxs-lookup"><span data-stu-id="04ec3-117">The `op` property indicates the type of operation.</span></span>
* <span data-ttu-id="04ec3-118">La propiedad `path` indica el elemento que se va a actualizar.</span><span class="sxs-lookup"><span data-stu-id="04ec3-118">The `path` property indicates the element to update.</span></span>
* <span data-ttu-id="04ec3-119">La propiedad `value` proporciona el nuevo valor.</span><span class="sxs-lookup"><span data-stu-id="04ec3-119">The `value` property provides the new value.</span></span>

### <a name="resource-after-patch"></a><span data-ttu-id="04ec3-120">Recurso después de la revisión</span><span class="sxs-lookup"><span data-stu-id="04ec3-120">Resource after patch</span></span>

<span data-ttu-id="04ec3-121">Este es el recurso después de aplicar el documento JSON Patch anterior:</span><span class="sxs-lookup"><span data-stu-id="04ec3-121">Here's the resource after applying the preceding JSON Patch document:</span></span>

```json
{
  "customerName": "Barry",
  "orders": [
    {
      "orderName": "Order0",
      "orderType": null
    },
    {
      "orderName": "Order1",
      "orderType": null
    },
    {
      "orderName": "Order2",
      "orderType": null
    }
  ]
}
```

<span data-ttu-id="04ec3-122">Los cambios realizados mediante la aplicación de un documento JSON Patch a un recurso son atómicos: si alguna operación de la lista produce un error, no se aplica ninguna operación de la lista.</span><span class="sxs-lookup"><span data-stu-id="04ec3-122">The changes made by applying a JSON Patch document to a resource are atomic: if any operation in the list fails, no operation in the list is applied.</span></span>

## <a name="path-syntax"></a><span data-ttu-id="04ec3-123">Sintaxis de path</span><span class="sxs-lookup"><span data-stu-id="04ec3-123">Path syntax</span></span>

<span data-ttu-id="04ec3-124">La propiedad [path](https://tools.ietf.org/html/rfc6901) de un objeto de operación tiene barras inversas entre niveles.</span><span class="sxs-lookup"><span data-stu-id="04ec3-124">The [path](https://tools.ietf.org/html/rfc6901) property of an operation object has slashes between levels.</span></span> <span data-ttu-id="04ec3-125">Por ejemplo, `"/address/zipCode"`.</span><span class="sxs-lookup"><span data-stu-id="04ec3-125">For example, `"/address/zipCode"`.</span></span>

<span data-ttu-id="04ec3-126">Para especificar elementos de matriz se usan índices de base cero.</span><span class="sxs-lookup"><span data-stu-id="04ec3-126">Zero-based indexes are used to specify array elements.</span></span> <span data-ttu-id="04ec3-127">El primer elemento de la matriz `addresses` estaría en `/addresses/0`.</span><span class="sxs-lookup"><span data-stu-id="04ec3-127">The first element of the `addresses` array would be at `/addresses/0`.</span></span> <span data-ttu-id="04ec3-128">Para usar `add` al final de una matriz, use un guion (-) en lugar de un número de índice: `/addresses/-`.</span><span class="sxs-lookup"><span data-stu-id="04ec3-128">To `add` to the end of an array, use a hyphen (-) rather than an index number: `/addresses/-`.</span></span>

### <a name="operations"></a><span data-ttu-id="04ec3-129">Operaciones</span><span class="sxs-lookup"><span data-stu-id="04ec3-129">Operations</span></span>

<span data-ttu-id="04ec3-130">En la siguiente tabla se muestran las operaciones admitidas, como se ha definido en la [especificación de JSON Patch](https://tools.ietf.org/html/rfc6902):</span><span class="sxs-lookup"><span data-stu-id="04ec3-130">The following table shows supported operations as defined in the [JSON Patch specification](https://tools.ietf.org/html/rfc6902):</span></span>

|<span data-ttu-id="04ec3-131">Operación</span><span class="sxs-lookup"><span data-stu-id="04ec3-131">Operation</span></span>  | <span data-ttu-id="04ec3-132">Notas</span><span class="sxs-lookup"><span data-stu-id="04ec3-132">Notes</span></span> |
|-----------|--------------------------------|
| `add`     | <span data-ttu-id="04ec3-133">Agrega un elemento de propiedad o matriz.</span><span class="sxs-lookup"><span data-stu-id="04ec3-133">Add a property or array element.</span></span> <span data-ttu-id="04ec3-134">Para la propiedad existente: establece el valor.</span><span class="sxs-lookup"><span data-stu-id="04ec3-134">For existing property: set value.</span></span>|
| `remove`  | <span data-ttu-id="04ec3-135">Quita un elemento de propiedad o matriz.</span><span class="sxs-lookup"><span data-stu-id="04ec3-135">Remove a property or array element.</span></span> |
| `replace` | <span data-ttu-id="04ec3-136">Lo mismo que `remove` seguido de `add` en la misma ubicación.</span><span class="sxs-lookup"><span data-stu-id="04ec3-136">Same as `remove` followed by `add` at same location.</span></span> |
| `move`    | <span data-ttu-id="04ec3-137">Lo mismo que `remove` desde el origen seguido de `add` al destino mediante el valor del origen.</span><span class="sxs-lookup"><span data-stu-id="04ec3-137">Same as `remove` from source followed by `add` to destination using value from source.</span></span> |
| `copy`    | <span data-ttu-id="04ec3-138">Lo mismo que `add` al destino mediante el valor del origen.</span><span class="sxs-lookup"><span data-stu-id="04ec3-138">Same as `add` to destination using value from source.</span></span> |
| `test`    | <span data-ttu-id="04ec3-139">Devuelve el código de estado correcto si el valor en `path` = al `value` proporcionado.</span><span class="sxs-lookup"><span data-stu-id="04ec3-139">Return success status code if value at `path` = provided `value`.</span></span>|

## <a name="jsonpatch-in-aspnet-core"></a><span data-ttu-id="04ec3-140">JsonPatch en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="04ec3-140">JsonPatch in ASP.NET Core</span></span>

<span data-ttu-id="04ec3-141">La implementación de ASP.NET Core de JSON Patch se proporciona en el paquete NuGet [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/).</span><span class="sxs-lookup"><span data-stu-id="04ec3-141">The ASP.NET Core implementation of JSON Patch is provided in the [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) NuGet package.</span></span> <span data-ttu-id="04ec3-142">El paquete se incluye en el metapaquete [Microsoft.AspnetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="04ec3-142">The package is included in the [Microsoft.AspnetCore.App](xref:fundamentals/metapackage-app) metapackage.</span></span>

## <a name="action-method-code"></a><span data-ttu-id="04ec3-143">Código del método de acción</span><span class="sxs-lookup"><span data-stu-id="04ec3-143">Action method code</span></span>

<span data-ttu-id="04ec3-144">En un controlador de API, un método de acción para JSON Patch:</span><span class="sxs-lookup"><span data-stu-id="04ec3-144">In an API controller, an action method for JSON Patch:</span></span>

* <span data-ttu-id="04ec3-145">Se anota con el atributo `HttpPatch`.</span><span class="sxs-lookup"><span data-stu-id="04ec3-145">Is annotated with the `HttpPatch` attribute.</span></span>
* <span data-ttu-id="04ec3-146">Acepta `JsonPatchDocument<T>`, normalmente con [FromBody].</span><span class="sxs-lookup"><span data-stu-id="04ec3-146">Accepts a `JsonPatchDocument<T>`, typically with [FromBody].</span></span>
* <span data-ttu-id="04ec3-147">Llama a `ApplyTo` en el documento de revisión para aplicar los cambios.</span><span class="sxs-lookup"><span data-stu-id="04ec3-147">Calls `ApplyTo` on the patch document to apply the changes.</span></span>

<span data-ttu-id="04ec3-148">Este es un ejemplo:</span><span class="sxs-lookup"><span data-stu-id="04ec3-148">Here's an example:</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_PatchAction&highlight=1,3,9)]

<span data-ttu-id="04ec3-149">Este código de la aplicación de ejemplo funciona con el siguiente modelo `Customer`.</span><span class="sxs-lookup"><span data-stu-id="04ec3-149">This code from the sample app works with the following `Customer` model.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Models/Customer.cs?name=snippet_Customer)]

[!code-csharp[](jsonpatch/samples/2.2/Models/Order.cs?name=snippet_Order)]

<span data-ttu-id="04ec3-150">El método de acción de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="04ec3-150">The sample action method:</span></span>

* <span data-ttu-id="04ec3-151">Construye un objeto `Customer`.</span><span class="sxs-lookup"><span data-stu-id="04ec3-151">Constructs a `Customer`.</span></span>
* <span data-ttu-id="04ec3-152">Aplica la revisión.</span><span class="sxs-lookup"><span data-stu-id="04ec3-152">Applies the patch.</span></span>
* <span data-ttu-id="04ec3-153">Devuelve el resultado en el cuerpo de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="04ec3-153">Returns the result in the body of the response.</span></span>

 <span data-ttu-id="04ec3-154">En una aplicación real, el código recuperaría los datos de un almacén como una base de datos y actualizaría la base de datos después de aplicar la revisión.</span><span class="sxs-lookup"><span data-stu-id="04ec3-154">In a real app, the code would retrieve the data from a store such as a database and update the database after applying the patch.</span></span>

### <a name="model-state"></a><span data-ttu-id="04ec3-155">Estado del modelo</span><span class="sxs-lookup"><span data-stu-id="04ec3-155">Model state</span></span>

<span data-ttu-id="04ec3-156">En el ejemplo anterior del método de acción, se llama a una sobrecarga de `ApplyTo` que toma el estado del modelo como uno de sus parámetros.</span><span class="sxs-lookup"><span data-stu-id="04ec3-156">The preceding action method example calls an overload of `ApplyTo` that takes model state as one of its parameters.</span></span> <span data-ttu-id="04ec3-157">Con esta opción, puede obtener mensajes de error en las respuestas.</span><span class="sxs-lookup"><span data-stu-id="04ec3-157">With this option, you can get error messages in responses.</span></span> <span data-ttu-id="04ec3-158">En el ejemplo siguiente se muestra el cuerpo de una respuesta 400 Solicitud incorrecta de una operación `test`:</span><span class="sxs-lookup"><span data-stu-id="04ec3-158">The following example shows the body of a 400 Bad Request response for a `test` operation:</span></span>

```json
{
    "Customer": [
        "The current value 'John' at path 'customerName' is not equal to the test value 'Nancy'."
    ]
}
```

### <a name="dynamic-objects"></a><span data-ttu-id="04ec3-159">Objetos dinámicos</span><span class="sxs-lookup"><span data-stu-id="04ec3-159">Dynamic objects</span></span>

<span data-ttu-id="04ec3-160">En el ejemplo siguiente de método de acción se muestra cómo aplicar una revisión a un objeto dinámico.</span><span class="sxs-lookup"><span data-stu-id="04ec3-160">The following action method example shows how to apply a patch to a dynamic object.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_Dynamic)]

## <a name="the-add-operation"></a><span data-ttu-id="04ec3-161">La operación add</span><span class="sxs-lookup"><span data-stu-id="04ec3-161">The add operation</span></span>

* <span data-ttu-id="04ec3-162">Si `path` apunta a un elemento de matriz: inserta un nuevo elemento delante del especificado por `path`.</span><span class="sxs-lookup"><span data-stu-id="04ec3-162">If `path` points to an array element: inserts new element before the one specified by `path`.</span></span>
* <span data-ttu-id="04ec3-163">Si `path` apunta a una propiedad: establece el valor de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="04ec3-163">If `path` points to a property: sets the property value.</span></span>
* <span data-ttu-id="04ec3-164">Si `path` apunta a una ubicación que no existe:</span><span class="sxs-lookup"><span data-stu-id="04ec3-164">If `path` points to a nonexistent location:</span></span>
  * <span data-ttu-id="04ec3-165">Si el recurso para revisar es un objeto dinámico: agrega una propiedad.</span><span class="sxs-lookup"><span data-stu-id="04ec3-165">If the resource to patch is a dynamic object: adds a property.</span></span>
  * <span data-ttu-id="04ec3-166">Si el recurso para revisar es un objeto estático: la solicitud produce un error.</span><span class="sxs-lookup"><span data-stu-id="04ec3-166">If the resource to patch is a static object: the request fails.</span></span>

<span data-ttu-id="04ec3-167">El siguiente documento de revisión de ejemplo establece el valor de `CustomerName` y agrega un objeto `Order` al final de la matriz `Orders`.</span><span class="sxs-lookup"><span data-stu-id="04ec3-167">The following sample patch document sets the value of `CustomerName` and adds an `Order` object to the end of the `Orders` array.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

## <a name="the-remove-operation"></a><span data-ttu-id="04ec3-168">La operación remove</span><span class="sxs-lookup"><span data-stu-id="04ec3-168">The remove operation</span></span>

* <span data-ttu-id="04ec3-169">Si `path` apunta a un elemento de matriz: quita el elemento.</span><span class="sxs-lookup"><span data-stu-id="04ec3-169">If `path` points to an array element: removes the element.</span></span>
* <span data-ttu-id="04ec3-170">Si `path` apunta a una propiedad:</span><span class="sxs-lookup"><span data-stu-id="04ec3-170">If `path` points to a property:</span></span>
  * <span data-ttu-id="04ec3-171">Si el recurso para revisar es un objeto dinámico: quita la propiedad.</span><span class="sxs-lookup"><span data-stu-id="04ec3-171">If resource to patch is a dynamic object: removes the property.</span></span>
  * <span data-ttu-id="04ec3-172">Si el recurso para revisar es un objeto estático:</span><span class="sxs-lookup"><span data-stu-id="04ec3-172">If resource to patch is a static object:</span></span> 
    * <span data-ttu-id="04ec3-173">Si la propiedad acepta valores NULL: la establece en null.</span><span class="sxs-lookup"><span data-stu-id="04ec3-173">If the property is nullable: sets it to null.</span></span>
    * <span data-ttu-id="04ec3-174">Si la propiedad es distinta de null, la establece en `default<T>`.</span><span class="sxs-lookup"><span data-stu-id="04ec3-174">If the property is non-nullable, sets it to `default<T>`.</span></span>

<span data-ttu-id="04ec3-175">En el siguiente documento de revisión de ejemplo, se establece `CustomerName` en null y se elimina `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="04ec3-175">The following sample patch document sets `CustomerName` to null and deletes `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/remove.json)]

## <a name="the-replace-operation"></a><span data-ttu-id="04ec3-176">La operación replace</span><span class="sxs-lookup"><span data-stu-id="04ec3-176">The replace operation</span></span>

<span data-ttu-id="04ec3-177">Esta operación es funcionalmente igual que `remove` seguida de `add`.</span><span class="sxs-lookup"><span data-stu-id="04ec3-177">This operation is functionally the same as a `remove` followed by an `add`.</span></span>

<span data-ttu-id="04ec3-178">En el siguiente documento de revisión de ejemplo se establece el valor de `CustomerName` y se reemplaza `Orders[0]` por un nuevo objeto `Order`.</span><span class="sxs-lookup"><span data-stu-id="04ec3-178">The following sample patch document sets the value of `CustomerName` and replaces `Orders[0]`with a new `Order` object.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/replace.json)]

## <a name="the-move-operation"></a><span data-ttu-id="04ec3-179">La operación move</span><span class="sxs-lookup"><span data-stu-id="04ec3-179">The move operation</span></span>

* <span data-ttu-id="04ec3-180">Si `path` apunta a un elemento de matriz: copia el elemento `from` en la ubicación del elemento `path` y, luego, ejecuta una operación `remove` en el elemento `from`.</span><span class="sxs-lookup"><span data-stu-id="04ec3-180">If `path` points to an array element: copies `from` element to location of `path` element, then runs a `remove` operation on the `from` element.</span></span>
* <span data-ttu-id="04ec3-181">Si `path` apunta a una propiedad: copia el valor de la propiedad `from` en la propiedad `path` y, luego, ejecuta la operación `remove` en la propiedad `from`.</span><span class="sxs-lookup"><span data-stu-id="04ec3-181">If `path` points to a property: copies value of `from` property to `path` property, then runs a `remove` operation on the `from` property.</span></span>
* <span data-ttu-id="04ec3-182">Si `path` apunta a una propiedad que no existe:</span><span class="sxs-lookup"><span data-stu-id="04ec3-182">If `path` points to a nonexistent property:</span></span>
  * <span data-ttu-id="04ec3-183">Si el recurso para revisar es un objeto estático: la solicitud produce un error.</span><span class="sxs-lookup"><span data-stu-id="04ec3-183">If the resource to patch is a static object: the request fails.</span></span>
  * <span data-ttu-id="04ec3-184">Si el recurso para revisar es un objeto dinámico: copia la propiedad `from` en la ubicación indicada por `path` y, luego, ejecuta una operación `remove` en la propiedad `from`.</span><span class="sxs-lookup"><span data-stu-id="04ec3-184">If the resource to patch is a dynamic object: copies `from` property to location indicated by `path`, then runs a `remove` operation on the `from` property.</span></span>

<span data-ttu-id="04ec3-185">En el siguiente documento de revisión de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="04ec3-185">The following sample patch document:</span></span>

* <span data-ttu-id="04ec3-186">Se copia el valor de `Orders[0].OrderName` en `CustomerName`.</span><span class="sxs-lookup"><span data-stu-id="04ec3-186">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="04ec3-187">Se establece `Orders[0].OrderName` en null.</span><span class="sxs-lookup"><span data-stu-id="04ec3-187">Sets `Orders[0].OrderName` to null.</span></span>
* <span data-ttu-id="04ec3-188">Se mueve `Orders[1]` delante de `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="04ec3-188">Moves `Orders[1]` to before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/move.json)]

## <a name="the-copy-operation"></a><span data-ttu-id="04ec3-189">La operación copy</span><span class="sxs-lookup"><span data-stu-id="04ec3-189">The copy operation</span></span>

<span data-ttu-id="04ec3-190">Esta operación es funcionalmente igual que la operación `move` sin el paso `remove` final.</span><span class="sxs-lookup"><span data-stu-id="04ec3-190">This operation is functionally the same as a `move` operation without the final `remove` step.</span></span>

<span data-ttu-id="04ec3-191">En el siguiente documento de revisión de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="04ec3-191">The following sample patch document:</span></span>

* <span data-ttu-id="04ec3-192">Se copia el valor de `Orders[0].OrderName` en `CustomerName`.</span><span class="sxs-lookup"><span data-stu-id="04ec3-192">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="04ec3-193">Se inserta una copia de `Orders[1]` delante de `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="04ec3-193">Inserts a copy of `Orders[1]` before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/copy.json)]

## <a name="the-test-operation"></a><span data-ttu-id="04ec3-194">La operación test</span><span class="sxs-lookup"><span data-stu-id="04ec3-194">The test operation</span></span>

<span data-ttu-id="04ec3-195">Si el valor de la ubicación indicada por `path` es diferente del valor proporcionado en `value`, la solicitud produce un error.</span><span class="sxs-lookup"><span data-stu-id="04ec3-195">If the value at the location indicated by `path` is different from the value provided in `value`, the request fails.</span></span> <span data-ttu-id="04ec3-196">En ese caso, la solicitud PATCH entera produce un error incluso si todas las demás operaciones del documento de revisión se realizan correctamente.</span><span class="sxs-lookup"><span data-stu-id="04ec3-196">In that case, the whole PATCH request fails even if all other operations in the patch document would otherwise succeed.</span></span>

<span data-ttu-id="04ec3-197">La operación `test` se usa habitualmente para impedir una actualización cuando hay un conflicto de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="04ec3-197">The `test` operation is commonly used to prevent an update when there's a concurrency conflict.</span></span>

<span data-ttu-id="04ec3-198">El siguiente documento de revisión de ejemplo no tiene ningún efecto si el valor inicial de `CustomerName` es "John", porque la prueba produce un error:</span><span class="sxs-lookup"><span data-stu-id="04ec3-198">The following sample patch document has no effect if the initial value of `CustomerName` is "John", because the test fails:</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/test-fail.json)]

## <a name="get-the-code"></a><span data-ttu-id="04ec3-199">Obtención del código</span><span class="sxs-lookup"><span data-stu-id="04ec3-199">Get the code</span></span>

<span data-ttu-id="04ec3-200">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span><span class="sxs-lookup"><span data-stu-id="04ec3-200">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span></span> <span data-ttu-id="04ec3-201">([Método de descarga](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="04ec3-201">([How to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="04ec3-202">Para probar el ejemplo, ejecute la aplicación y envíe solicitudes HTTP con la configuración siguiente:</span><span class="sxs-lookup"><span data-stu-id="04ec3-202">To test the sample, run the app and send HTTP requests with the following settings:</span></span>

* <span data-ttu-id="04ec3-203">DIRECCIÓN URL: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span><span class="sxs-lookup"><span data-stu-id="04ec3-203">URL: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span></span>
* <span data-ttu-id="04ec3-204">Método HTTP: `PATCH`</span><span class="sxs-lookup"><span data-stu-id="04ec3-204">HTTP method: `PATCH`</span></span>
* <span data-ttu-id="04ec3-205">Encabezado: `Content-Type: application/json-patch+json`</span><span class="sxs-lookup"><span data-stu-id="04ec3-205">Header: `Content-Type: application/json-patch+json`</span></span>
* <span data-ttu-id="04ec3-206">Cuerpo: Copie y pegue uno de los ejemplos de documento de revisión de JSON de la carpeta del proyecto *JSON*.</span><span class="sxs-lookup"><span data-stu-id="04ec3-206">Body: Copy and paste one of the JSON patch document samples from the *JSON* project folder.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="04ec3-207">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="04ec3-207">Additional resources</span></span>

* [<span data-ttu-id="04ec3-208">Especificación del método PATCH de IETF RFC 5789</span><span class="sxs-lookup"><span data-stu-id="04ec3-208">IETF RFC 5789 PATCH method specification</span></span>](https://tools.ietf.org/html/rfc5789)
* [<span data-ttu-id="04ec3-209">Especificación del método JSON PATCH de IETF RFC 6902</span><span class="sxs-lookup"><span data-stu-id="04ec3-209">IETF RFC 6902 JSON Patch specification</span></span>](https://tools.ietf.org/html/rfc6902)
* [<span data-ttu-id="04ec3-210">Especificación del formato de ruta de acceso JSON Patch de IETF RFC 6901</span><span class="sxs-lookup"><span data-stu-id="04ec3-210">IETF RFC 6901 JSON Patch path format spec</span></span>](https://tools.ietf.org/html/rfc6901)
* <span data-ttu-id="04ec3-211">[Documentación de JSON Patch](https://jsonpatch.com/).</span><span class="sxs-lookup"><span data-stu-id="04ec3-211">[JSON Patch documentation](https://jsonpatch.com/).</span></span> <span data-ttu-id="04ec3-212">Incluye vínculos a recursos para crear documentos JSON Patch.</span><span class="sxs-lookup"><span data-stu-id="04ec3-212">Includes links to resources for creating JSON Patch documents.</span></span>
* [<span data-ttu-id="04ec3-213">Código fuente de JSON Patch para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="04ec3-213">ASP.NET Core JSON Patch source code</span></span>](https://github.com/aspnet/AspNetCore/tree/master/src/Features/JsonPatch/src)
