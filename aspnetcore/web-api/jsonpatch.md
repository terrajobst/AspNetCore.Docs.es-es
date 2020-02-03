---
title: JsonPatch en la API web de ASP.NET Core
author: rick-anderson
description: Aprenda a administrar solicitudes JSON Patch en una API web ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/01/2019
uid: web-api/jsonpatch
ms.openlocfilehash: e57556e4b3fba55c6c187092593ffab4e31ee2d9
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2020
ms.locfileid: "76727108"
---
# <a name="jsonpatch-in-aspnet-core-web-api"></a><span data-ttu-id="4e50f-103">JsonPatch en la API web de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4e50f-103">JsonPatch in ASP.NET Core web API</span></span>

<span data-ttu-id="4e50f-104">Por [Tom Dykstra](https://github.com/tdykstra) y [Kirk Larkin](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="4e50f-104">By [Tom Dykstra](https://github.com/tdykstra) and [Kirk Larkin](https://github.com/serpent5)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="4e50f-105">En este artículo se explica cómo administrar solicitudes JSON Patch en una API web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4e50f-105">This article explains how to handle JSON Patch requests in an ASP.NET Core web API.</span></span>

## <a name="package-installation"></a><span data-ttu-id="4e50f-106">Instalación del paquete</span><span class="sxs-lookup"><span data-stu-id="4e50f-106">Package installation</span></span>

<span data-ttu-id="4e50f-107">La compatibilidad con JsonPatch se habilita mediante el uso del paquete `Microsoft.AspNetCore.Mvc.NewtonsoftJson`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-107">Support for JsonPatch is enabled using the `Microsoft.AspNetCore.Mvc.NewtonsoftJson` package.</span></span> <span data-ttu-id="4e50f-108">Para habilitar esta característica, las aplicaciones deben realizar lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="4e50f-108">To enable this feature, apps must:</span></span>

* <span data-ttu-id="4e50f-109">Instalar el paquete NuGet [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/).</span><span class="sxs-lookup"><span data-stu-id="4e50f-109">Install the [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet package.</span></span>
* <span data-ttu-id="4e50f-110">Actualice el método `Startup.ConfigureServices` del proyecto para incluir una llamada a `AddNewtonsoftJson`:</span><span class="sxs-lookup"><span data-stu-id="4e50f-110">Update the project's `Startup.ConfigureServices` method to include a call to `AddNewtonsoftJson`:</span></span>

  ```csharp
  services
      .AddControllersWithViews()
      .AddNewtonsoftJson();
  ```

<span data-ttu-id="4e50f-111">`AddNewtonsoftJson` es compatible con los métodos de registro del servicio MVC:</span><span class="sxs-lookup"><span data-stu-id="4e50f-111">`AddNewtonsoftJson` is compatible with the MVC service registration methods:</span></span>

  * `AddRazorPages`
  * `AddControllersWithViews`
  * `AddControllers`

## <a name="jsonpatch-addnewtonsoftjson-and-systemtextjson"></a><span data-ttu-id="4e50f-112">JsonPatch, AddNewtonsoftJson y System.Text.Json</span><span class="sxs-lookup"><span data-stu-id="4e50f-112">JsonPatch, AddNewtonsoftJson, and System.Text.Json</span></span>
  
<span data-ttu-id="4e50f-113">`AddNewtonsoftJson` reemplaza los formateadores de entrada y salida basados en `System.Text.Json` que se usan para dar formato a **todo** el contenido JSON.</span><span class="sxs-lookup"><span data-stu-id="4e50f-113">`AddNewtonsoftJson` replaces the `System.Text.Json` based input and output formatters used for formatting **all** JSON content.</span></span> <span data-ttu-id="4e50f-114">Para agregar compatibilidad con `JsonPatch` mediante `Newtonsoft.Json`, sin cambiar los otros formateadores, actualice el `Startup.ConfigureServices` del proyecto como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="4e50f-114">To add support for `JsonPatch` using `Newtonsoft.Json`, while leaving the other formatters unchanged, update the project's `Startup.ConfigureServices` as follows:</span></span>

[!code-csharp[](jsonpatch/samples/3.0/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="4e50f-115">El código anterior requiere una referencia a [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson) y las siguientes instrucciones using:</span><span class="sxs-lookup"><span data-stu-id="4e50f-115">The preceding code requires a reference to [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson) and the following using statements:</span></span>

[!code-csharp[](jsonpatch/samples/3.0/WebApp1/Startup.cs?name=snippet1)]

## <a name="patch-http-request-method"></a><span data-ttu-id="4e50f-116">Método de solicitud HTTP PATCH</span><span class="sxs-lookup"><span data-stu-id="4e50f-116">PATCH HTTP request method</span></span>

<span data-ttu-id="4e50f-117">Los métodos PUT y [PATCH](https://tools.ietf.org/html/rfc5789) se usan para actualizar un recurso existente.</span><span class="sxs-lookup"><span data-stu-id="4e50f-117">The PUT and [PATCH](https://tools.ietf.org/html/rfc5789) methods are used to update an existing resource.</span></span> <span data-ttu-id="4e50f-118">La diferencia entre ellos es que PUT reemplaza el recurso entero, mientras que PATCH especifica únicamente los cambios.</span><span class="sxs-lookup"><span data-stu-id="4e50f-118">The difference between them is that PUT replaces the entire resource, while PATCH specifies only the changes.</span></span>

## <a name="json-patch"></a><span data-ttu-id="4e50f-119">JSON Patch</span><span class="sxs-lookup"><span data-stu-id="4e50f-119">JSON Patch</span></span>

<span data-ttu-id="4e50f-120">[JSON Patch](https://tools.ietf.org/html/rfc6902) es un formato para especificar las actualizaciones que se aplicarán a un recurso.</span><span class="sxs-lookup"><span data-stu-id="4e50f-120">[JSON Patch](https://tools.ietf.org/html/rfc6902) is a format for specifying updates to be applied to a resource.</span></span> <span data-ttu-id="4e50f-121">Un documento JSON Patch tiene una matriz de *operaciones*.</span><span class="sxs-lookup"><span data-stu-id="4e50f-121">A JSON Patch document has an array of *operations*.</span></span> <span data-ttu-id="4e50f-122">Cada operación identifica un determinado tipo de cambio, como agregar un elemento de matriz o reemplazar un valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="4e50f-122">Each operation identifies a particular type of change, such as add an array element or replace a property value.</span></span>

<span data-ttu-id="4e50f-123">Por ejemplo, los documentos JSON siguientes representan un recurso, un documento de revisión JSON para el recurso y el resultado de aplicar las operaciones de revisión.</span><span class="sxs-lookup"><span data-stu-id="4e50f-123">For example, the following JSON documents represent a resource, a JSON patch document for the resource, and the result of applying the patch operations.</span></span>

### <a name="resource-example"></a><span data-ttu-id="4e50f-124">Ejemplo de recursos</span><span class="sxs-lookup"><span data-stu-id="4e50f-124">Resource example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/customer.json)]

### <a name="json-patch-example"></a><span data-ttu-id="4e50f-125">Ejemplo de revisión de JSON</span><span class="sxs-lookup"><span data-stu-id="4e50f-125">JSON patch example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

<span data-ttu-id="4e50f-126">En el código JSON anterior:</span><span class="sxs-lookup"><span data-stu-id="4e50f-126">In the preceding JSON:</span></span>

* <span data-ttu-id="4e50f-127">La propiedad `op` indica el tipo de operación.</span><span class="sxs-lookup"><span data-stu-id="4e50f-127">The `op` property indicates the type of operation.</span></span>
* <span data-ttu-id="4e50f-128">La propiedad `path` indica el elemento que se va a actualizar.</span><span class="sxs-lookup"><span data-stu-id="4e50f-128">The `path` property indicates the element to update.</span></span>
* <span data-ttu-id="4e50f-129">La propiedad `value` proporciona el nuevo valor.</span><span class="sxs-lookup"><span data-stu-id="4e50f-129">The `value` property provides the new value.</span></span>

### <a name="resource-after-patch"></a><span data-ttu-id="4e50f-130">Recurso después de la revisión</span><span class="sxs-lookup"><span data-stu-id="4e50f-130">Resource after patch</span></span>

<span data-ttu-id="4e50f-131">Este es el recurso después de aplicar el documento JSON Patch anterior:</span><span class="sxs-lookup"><span data-stu-id="4e50f-131">Here's the resource after applying the preceding JSON Patch document:</span></span>

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

<span data-ttu-id="4e50f-132">Los cambios realizados mediante la aplicación de un documento JSON Patch a un recurso son atómicos: si alguna operación de la lista produce un error, no se aplica ninguna operación de la lista.</span><span class="sxs-lookup"><span data-stu-id="4e50f-132">The changes made by applying a JSON Patch document to a resource are atomic: if any operation in the list fails, no operation in the list is applied.</span></span>

## <a name="path-syntax"></a><span data-ttu-id="4e50f-133">Sintaxis de path</span><span class="sxs-lookup"><span data-stu-id="4e50f-133">Path syntax</span></span>

<span data-ttu-id="4e50f-134">La propiedad [path](https://tools.ietf.org/html/rfc6901) de un objeto de operación tiene barras inversas entre niveles.</span><span class="sxs-lookup"><span data-stu-id="4e50f-134">The [path](https://tools.ietf.org/html/rfc6901) property of an operation object has slashes between levels.</span></span> <span data-ttu-id="4e50f-135">Por ejemplo: `"/address/zipCode"`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-135">For example, `"/address/zipCode"`.</span></span>

<span data-ttu-id="4e50f-136">Para especificar elementos de matriz se usan índices de base cero.</span><span class="sxs-lookup"><span data-stu-id="4e50f-136">Zero-based indexes are used to specify array elements.</span></span> <span data-ttu-id="4e50f-137">El primer elemento de la matriz `addresses` estaría en `/addresses/0`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-137">The first element of the `addresses` array would be at `/addresses/0`.</span></span> <span data-ttu-id="4e50f-138">Para usar `add` al final de una matriz, use un guion (-) en lugar de un número de índice: `/addresses/-`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-138">To `add` to the end of an array, use a hyphen (-) rather than an index number: `/addresses/-`.</span></span>

### <a name="operations"></a><span data-ttu-id="4e50f-139">Operaciones</span><span class="sxs-lookup"><span data-stu-id="4e50f-139">Operations</span></span>

<span data-ttu-id="4e50f-140">En la siguiente tabla se muestran las operaciones admitidas, como se ha definido en la [especificación de JSON Patch](https://tools.ietf.org/html/rfc6902):</span><span class="sxs-lookup"><span data-stu-id="4e50f-140">The following table shows supported operations as defined in the [JSON Patch specification](https://tools.ietf.org/html/rfc6902):</span></span>

|<span data-ttu-id="4e50f-141">Operación</span><span class="sxs-lookup"><span data-stu-id="4e50f-141">Operation</span></span>  | <span data-ttu-id="4e50f-142">Notas</span><span class="sxs-lookup"><span data-stu-id="4e50f-142">Notes</span></span> |
|-----------|--------------------------------|
| `add`     | <span data-ttu-id="4e50f-143">Agrega un elemento de propiedad o matriz.</span><span class="sxs-lookup"><span data-stu-id="4e50f-143">Add a property or array element.</span></span> <span data-ttu-id="4e50f-144">Para la propiedad existente: establece el valor.</span><span class="sxs-lookup"><span data-stu-id="4e50f-144">For existing property: set value.</span></span>|
| `remove`  | <span data-ttu-id="4e50f-145">Quita un elemento de propiedad o matriz.</span><span class="sxs-lookup"><span data-stu-id="4e50f-145">Remove a property or array element.</span></span> |
| `replace` | <span data-ttu-id="4e50f-146">Lo mismo que `remove` seguido de `add` en la misma ubicación.</span><span class="sxs-lookup"><span data-stu-id="4e50f-146">Same as `remove` followed by `add` at same location.</span></span> |
| `move`    | <span data-ttu-id="4e50f-147">Lo mismo que `remove` desde el origen seguido de `add` al destino mediante el valor del origen.</span><span class="sxs-lookup"><span data-stu-id="4e50f-147">Same as `remove` from source followed by `add` to destination using value from source.</span></span> |
| `copy`    | <span data-ttu-id="4e50f-148">Lo mismo que `add` al destino mediante el valor del origen.</span><span class="sxs-lookup"><span data-stu-id="4e50f-148">Same as `add` to destination using value from source.</span></span> |
| `test`    | <span data-ttu-id="4e50f-149">Devuelve el código de estado correcto si el valor en `path` = al `value` proporcionado.</span><span class="sxs-lookup"><span data-stu-id="4e50f-149">Return success status code if value at `path` = provided `value`.</span></span>|

## <a name="jsonpatch-in-aspnet-core"></a><span data-ttu-id="4e50f-150">JsonPatch en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4e50f-150">JsonPatch in ASP.NET Core</span></span>

<span data-ttu-id="4e50f-151">La implementación de ASP.NET Core de JSON Patch se proporciona en el paquete NuGet [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/).</span><span class="sxs-lookup"><span data-stu-id="4e50f-151">The ASP.NET Core implementation of JSON Patch is provided in the [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) NuGet package.</span></span>

## <a name="action-method-code"></a><span data-ttu-id="4e50f-152">Código del método de acción</span><span class="sxs-lookup"><span data-stu-id="4e50f-152">Action method code</span></span>

<span data-ttu-id="4e50f-153">En un controlador de API, un método de acción para JSON Patch:</span><span class="sxs-lookup"><span data-stu-id="4e50f-153">In an API controller, an action method for JSON Patch:</span></span>

* <span data-ttu-id="4e50f-154">Se anota con el atributo `HttpPatch`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-154">Is annotated with the `HttpPatch` attribute.</span></span>
* <span data-ttu-id="4e50f-155">Acepta `JsonPatchDocument<T>`, normalmente con `[FromBody]`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-155">Accepts a `JsonPatchDocument<T>`, typically with `[FromBody]`.</span></span>
* <span data-ttu-id="4e50f-156">Llama a `ApplyTo` en el documento de revisión para aplicar los cambios.</span><span class="sxs-lookup"><span data-stu-id="4e50f-156">Calls `ApplyTo` on the patch document to apply the changes.</span></span>

<span data-ttu-id="4e50f-157">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="4e50f-157">Here's an example:</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_PatchAction&highlight=1,3,9)]

<span data-ttu-id="4e50f-158">Este código de la aplicación de ejemplo funciona con el siguiente modelo `Customer`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-158">This code from the sample app works with the following `Customer` model.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Models/Customer.cs?name=snippet_Customer)]

[!code-csharp[](jsonpatch/samples/2.2/Models/Order.cs?name=snippet_Order)]

<span data-ttu-id="4e50f-159">El método de acción de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="4e50f-159">The sample action method:</span></span>

* <span data-ttu-id="4e50f-160">Construye un objeto `Customer`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-160">Constructs a `Customer`.</span></span>
* <span data-ttu-id="4e50f-161">Aplica la revisión.</span><span class="sxs-lookup"><span data-stu-id="4e50f-161">Applies the patch.</span></span>
* <span data-ttu-id="4e50f-162">Devuelve el resultado en el cuerpo de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="4e50f-162">Returns the result in the body of the response.</span></span>

 <span data-ttu-id="4e50f-163">En una aplicación real, el código recuperaría los datos de un almacén como una base de datos y actualizaría la base de datos después de aplicar la revisión.</span><span class="sxs-lookup"><span data-stu-id="4e50f-163">In a real app, the code would retrieve the data from a store such as a database and update the database after applying the patch.</span></span>

### <a name="model-state"></a><span data-ttu-id="4e50f-164">Estado del modelo</span><span class="sxs-lookup"><span data-stu-id="4e50f-164">Model state</span></span>

<span data-ttu-id="4e50f-165">En el ejemplo anterior del método de acción, se llama a una sobrecarga de `ApplyTo` que toma el estado del modelo como uno de sus parámetros.</span><span class="sxs-lookup"><span data-stu-id="4e50f-165">The preceding action method example calls an overload of `ApplyTo` that takes model state as one of its parameters.</span></span> <span data-ttu-id="4e50f-166">Con esta opción, puede obtener mensajes de error en las respuestas.</span><span class="sxs-lookup"><span data-stu-id="4e50f-166">With this option, you can get error messages in responses.</span></span> <span data-ttu-id="4e50f-167">En el ejemplo siguiente se muestra el cuerpo de una respuesta 400 Solicitud incorrecta de una operación `test`:</span><span class="sxs-lookup"><span data-stu-id="4e50f-167">The following example shows the body of a 400 Bad Request response for a `test` operation:</span></span>

```json
{
    "Customer": [
        "The current value 'John' at path 'customerName' is not equal to the test value 'Nancy'."
    ]
}
```

### <a name="dynamic-objects"></a><span data-ttu-id="4e50f-168">Objetos dinámicos</span><span class="sxs-lookup"><span data-stu-id="4e50f-168">Dynamic objects</span></span>

<span data-ttu-id="4e50f-169">En el ejemplo siguiente de método de acción se muestra cómo aplicar una revisión a un objeto dinámico.</span><span class="sxs-lookup"><span data-stu-id="4e50f-169">The following action method example shows how to apply a patch to a dynamic object.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_Dynamic)]

## <a name="the-add-operation"></a><span data-ttu-id="4e50f-170">La operación add</span><span class="sxs-lookup"><span data-stu-id="4e50f-170">The add operation</span></span>

* <span data-ttu-id="4e50f-171">Si `path` apunta a un elemento de matriz: inserta un nuevo elemento delante del especificado por `path`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-171">If `path` points to an array element: inserts new element before the one specified by `path`.</span></span>
* <span data-ttu-id="4e50f-172">Si `path` apunta a una propiedad: establece el valor de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="4e50f-172">If `path` points to a property: sets the property value.</span></span>
* <span data-ttu-id="4e50f-173">Si `path` apunta a una ubicación que no existe:</span><span class="sxs-lookup"><span data-stu-id="4e50f-173">If `path` points to a nonexistent location:</span></span>
  * <span data-ttu-id="4e50f-174">Si el recurso para revisar es un objeto dinámico: agrega una propiedad.</span><span class="sxs-lookup"><span data-stu-id="4e50f-174">If the resource to patch is a dynamic object: adds a property.</span></span>
  * <span data-ttu-id="4e50f-175">Si el recurso para revisar es un objeto estático: la solicitud produce un error.</span><span class="sxs-lookup"><span data-stu-id="4e50f-175">If the resource to patch is a static object: the request fails.</span></span>

<span data-ttu-id="4e50f-176">El siguiente documento de revisión de ejemplo establece el valor de `CustomerName` y agrega un objeto `Order` al final de la matriz `Orders`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-176">The following sample patch document sets the value of `CustomerName` and adds an `Order` object to the end of the `Orders` array.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

## <a name="the-remove-operation"></a><span data-ttu-id="4e50f-177">La operación remove</span><span class="sxs-lookup"><span data-stu-id="4e50f-177">The remove operation</span></span>

* <span data-ttu-id="4e50f-178">Si `path` apunta a un elemento de matriz: quita el elemento.</span><span class="sxs-lookup"><span data-stu-id="4e50f-178">If `path` points to an array element: removes the element.</span></span>
* <span data-ttu-id="4e50f-179">Si `path` apunta a una propiedad:</span><span class="sxs-lookup"><span data-stu-id="4e50f-179">If `path` points to a property:</span></span>
  * <span data-ttu-id="4e50f-180">Si el recurso para revisar es un objeto dinámico: quita la propiedad.</span><span class="sxs-lookup"><span data-stu-id="4e50f-180">If resource to patch is a dynamic object: removes the property.</span></span>
  * <span data-ttu-id="4e50f-181">Si el recurso para revisar es un objeto estático:</span><span class="sxs-lookup"><span data-stu-id="4e50f-181">If resource to patch is a static object:</span></span>
    * <span data-ttu-id="4e50f-182">Si la propiedad acepta valores NULL: la establece en null.</span><span class="sxs-lookup"><span data-stu-id="4e50f-182">If the property is nullable: sets it to null.</span></span>
    * <span data-ttu-id="4e50f-183">Si la propiedad es distinta de null, la establece en `default<T>`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-183">If the property is non-nullable, sets it to `default<T>`.</span></span>

<span data-ttu-id="4e50f-184">En el siguiente documento de revisión de ejemplo, se establece `CustomerName` en null y se elimina `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-184">The following sample patch document sets `CustomerName` to null and deletes `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/remove.json)]

## <a name="the-replace-operation"></a><span data-ttu-id="4e50f-185">La operación replace</span><span class="sxs-lookup"><span data-stu-id="4e50f-185">The replace operation</span></span>

<span data-ttu-id="4e50f-186">Esta operación es funcionalmente igual que `remove` seguida de `add`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-186">This operation is functionally the same as a `remove` followed by an `add`.</span></span>

<span data-ttu-id="4e50f-187">En el siguiente documento de revisión de ejemplo se establece el valor de `CustomerName` y se reemplaza `Orders[0]` por un nuevo objeto `Order`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-187">The following sample patch document sets the value of `CustomerName` and replaces `Orders[0]`with a new `Order` object.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/replace.json)]

## <a name="the-move-operation"></a><span data-ttu-id="4e50f-188">La operación move</span><span class="sxs-lookup"><span data-stu-id="4e50f-188">The move operation</span></span>

* <span data-ttu-id="4e50f-189">Si `path` apunta a un elemento de matriz: copia el elemento `from` en la ubicación del elemento `path` y, luego, ejecuta una operación `remove` en el elemento `from`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-189">If `path` points to an array element: copies `from` element to location of `path` element, then runs a `remove` operation on the `from` element.</span></span>
* <span data-ttu-id="4e50f-190">Si `path` apunta a una propiedad: copia el valor de la propiedad `from` en la propiedad `path` y, luego, ejecuta la operación `remove` en la propiedad `from`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-190">If `path` points to a property: copies value of `from` property to `path` property, then runs a `remove` operation on the `from` property.</span></span>
* <span data-ttu-id="4e50f-191">Si `path` apunta a una propiedad que no existe:</span><span class="sxs-lookup"><span data-stu-id="4e50f-191">If `path` points to a nonexistent property:</span></span>
  * <span data-ttu-id="4e50f-192">Si el recurso para revisar es un objeto estático: la solicitud produce un error.</span><span class="sxs-lookup"><span data-stu-id="4e50f-192">If the resource to patch is a static object: the request fails.</span></span>
  * <span data-ttu-id="4e50f-193">Si el recurso para revisar es un objeto dinámico: copia la propiedad `from` en la ubicación indicada por `path` y, luego, ejecuta una operación `remove` en la propiedad `from`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-193">If the resource to patch is a dynamic object: copies `from` property to location indicated by `path`, then runs a `remove` operation on the `from` property.</span></span>

<span data-ttu-id="4e50f-194">En el siguiente documento de revisión de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="4e50f-194">The following sample patch document:</span></span>

* <span data-ttu-id="4e50f-195">Se copia el valor de `Orders[0].OrderName` en `CustomerName`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-195">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="4e50f-196">Se establece `Orders[0].OrderName` en null.</span><span class="sxs-lookup"><span data-stu-id="4e50f-196">Sets `Orders[0].OrderName` to null.</span></span>
* <span data-ttu-id="4e50f-197">Se mueve `Orders[1]` delante de `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-197">Moves `Orders[1]` to before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/move.json)]

## <a name="the-copy-operation"></a><span data-ttu-id="4e50f-198">La operación copy</span><span class="sxs-lookup"><span data-stu-id="4e50f-198">The copy operation</span></span>

<span data-ttu-id="4e50f-199">Esta operación es funcionalmente igual que la operación `move` sin el paso `remove` final.</span><span class="sxs-lookup"><span data-stu-id="4e50f-199">This operation is functionally the same as a `move` operation without the final `remove` step.</span></span>

<span data-ttu-id="4e50f-200">En el siguiente documento de revisión de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="4e50f-200">The following sample patch document:</span></span>

* <span data-ttu-id="4e50f-201">Se copia el valor de `Orders[0].OrderName` en `CustomerName`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-201">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="4e50f-202">Se inserta una copia de `Orders[1]` delante de `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-202">Inserts a copy of `Orders[1]` before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/copy.json)]

## <a name="the-test-operation"></a><span data-ttu-id="4e50f-203">La operación test</span><span class="sxs-lookup"><span data-stu-id="4e50f-203">The test operation</span></span>

<span data-ttu-id="4e50f-204">Si el valor de la ubicación indicada por `path` es diferente del valor proporcionado en `value`, la solicitud produce un error.</span><span class="sxs-lookup"><span data-stu-id="4e50f-204">If the value at the location indicated by `path` is different from the value provided in `value`, the request fails.</span></span> <span data-ttu-id="4e50f-205">En ese caso, la solicitud PATCH entera produce un error incluso si todas las demás operaciones del documento de revisión se realizan correctamente.</span><span class="sxs-lookup"><span data-stu-id="4e50f-205">In that case, the whole PATCH request fails even if all other operations in the patch document would otherwise succeed.</span></span>

<span data-ttu-id="4e50f-206">La operación `test` se usa habitualmente para impedir una actualización cuando hay un conflicto de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="4e50f-206">The `test` operation is commonly used to prevent an update when there's a concurrency conflict.</span></span>

<span data-ttu-id="4e50f-207">El siguiente documento de revisión de ejemplo no tiene ningún efecto si el valor inicial de `CustomerName` es "John", porque la prueba produce un error:</span><span class="sxs-lookup"><span data-stu-id="4e50f-207">The following sample patch document has no effect if the initial value of `CustomerName` is "John", because the test fails:</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/test-fail.json)]

## <a name="get-the-code"></a><span data-ttu-id="4e50f-208">Obtención del código</span><span class="sxs-lookup"><span data-stu-id="4e50f-208">Get the code</span></span>

<span data-ttu-id="4e50f-209">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span><span class="sxs-lookup"><span data-stu-id="4e50f-209">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span></span> <span data-ttu-id="4e50f-210">([Método de descarga](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="4e50f-210">([How to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="4e50f-211">Para probar el ejemplo, ejecute la aplicación y envíe solicitudes HTTP con la configuración siguiente:</span><span class="sxs-lookup"><span data-stu-id="4e50f-211">To test the sample, run the app and send HTTP requests with the following settings:</span></span>

* <span data-ttu-id="4e50f-212">DIRECCIÓN URL: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span><span class="sxs-lookup"><span data-stu-id="4e50f-212">URL: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span></span>
* <span data-ttu-id="4e50f-213">Método HTTP: `PATCH`</span><span class="sxs-lookup"><span data-stu-id="4e50f-213">HTTP method: `PATCH`</span></span>
* <span data-ttu-id="4e50f-214">Encabezado: `Content-Type: application/json-patch+json`</span><span class="sxs-lookup"><span data-stu-id="4e50f-214">Header: `Content-Type: application/json-patch+json`</span></span>
* <span data-ttu-id="4e50f-215">Cuerpo: Copie y pegue uno de los ejemplos de documento de revisión de JSON de la carpeta del proyecto *JSON*.</span><span class="sxs-lookup"><span data-stu-id="4e50f-215">Body: Copy and paste one of the JSON patch document samples from the *JSON* project folder.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4e50f-216">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="4e50f-216">Additional resources</span></span>

* [<span data-ttu-id="4e50f-217">Especificación del método PATCH de IETF RFC 5789</span><span class="sxs-lookup"><span data-stu-id="4e50f-217">IETF RFC 5789 PATCH method specification</span></span>](https://tools.ietf.org/html/rfc5789)
* [<span data-ttu-id="4e50f-218">Especificación del método JSON PATCH de IETF RFC 6902</span><span class="sxs-lookup"><span data-stu-id="4e50f-218">IETF RFC 6902 JSON Patch specification</span></span>](https://tools.ietf.org/html/rfc6902)
* [<span data-ttu-id="4e50f-219">Especificación del formato de ruta de acceso JSON Patch de IETF RFC 6901</span><span class="sxs-lookup"><span data-stu-id="4e50f-219">IETF RFC 6901 JSON Patch path format spec</span></span>](https://tools.ietf.org/html/rfc6901)
* <span data-ttu-id="4e50f-220">[Documentación de JSON Patch](https://jsonpatch.com/).</span><span class="sxs-lookup"><span data-stu-id="4e50f-220">[JSON Patch documentation](https://jsonpatch.com/).</span></span> <span data-ttu-id="4e50f-221">Incluye vínculos a recursos para crear documentos JSON Patch.</span><span class="sxs-lookup"><span data-stu-id="4e50f-221">Includes links to resources for creating JSON Patch documents.</span></span>
* [<span data-ttu-id="4e50f-222">Código fuente de JSON Patch para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4e50f-222">ASP.NET Core JSON Patch source code</span></span>](https://github.com/dotnet/AspNetCore/tree/master/src/Features/JsonPatch/src)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="4e50f-223">En este artículo se explica cómo administrar solicitudes JSON Patch en una API web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4e50f-223">This article explains how to handle JSON Patch requests in an ASP.NET Core web API.</span></span>

## <a name="patch-http-request-method"></a><span data-ttu-id="4e50f-224">Método de solicitud HTTP PATCH</span><span class="sxs-lookup"><span data-stu-id="4e50f-224">PATCH HTTP request method</span></span>

<span data-ttu-id="4e50f-225">Los métodos PUT y [PATCH](https://tools.ietf.org/html/rfc5789) se usan para actualizar un recurso existente.</span><span class="sxs-lookup"><span data-stu-id="4e50f-225">The PUT and [PATCH](https://tools.ietf.org/html/rfc5789) methods are used to update an existing resource.</span></span> <span data-ttu-id="4e50f-226">La diferencia entre ellos es que PUT reemplaza el recurso entero, mientras que PATCH especifica únicamente los cambios.</span><span class="sxs-lookup"><span data-stu-id="4e50f-226">The difference between them is that PUT replaces the entire resource, while PATCH specifies only the changes.</span></span>

## <a name="json-patch"></a><span data-ttu-id="4e50f-227">JSON Patch</span><span class="sxs-lookup"><span data-stu-id="4e50f-227">JSON Patch</span></span>

<span data-ttu-id="4e50f-228">[JSON Patch](https://tools.ietf.org/html/rfc6902) es un formato para especificar las actualizaciones que se aplicarán a un recurso.</span><span class="sxs-lookup"><span data-stu-id="4e50f-228">[JSON Patch](https://tools.ietf.org/html/rfc6902) is a format for specifying updates to be applied to a resource.</span></span> <span data-ttu-id="4e50f-229">Un documento JSON Patch tiene una matriz de *operaciones*.</span><span class="sxs-lookup"><span data-stu-id="4e50f-229">A JSON Patch document has an array of *operations*.</span></span> <span data-ttu-id="4e50f-230">Cada operación identifica un determinado tipo de cambio, como agregar un elemento de matriz o reemplazar un valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="4e50f-230">Each operation identifies a particular type of change, such as add an array element or replace a property value.</span></span>

<span data-ttu-id="4e50f-231">Por ejemplo, los documentos JSON siguientes representan un recurso, un documento de revisión JSON para el recurso y el resultado de aplicar las operaciones de revisión.</span><span class="sxs-lookup"><span data-stu-id="4e50f-231">For example, the following JSON documents represent a resource, a JSON patch document for the resource, and the result of applying the patch operations.</span></span>

### <a name="resource-example"></a><span data-ttu-id="4e50f-232">Ejemplo de recursos</span><span class="sxs-lookup"><span data-stu-id="4e50f-232">Resource example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/customer.json)]

### <a name="json-patch-example"></a><span data-ttu-id="4e50f-233">Ejemplo de revisión de JSON</span><span class="sxs-lookup"><span data-stu-id="4e50f-233">JSON patch example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

<span data-ttu-id="4e50f-234">En el código JSON anterior:</span><span class="sxs-lookup"><span data-stu-id="4e50f-234">In the preceding JSON:</span></span>

* <span data-ttu-id="4e50f-235">La propiedad `op` indica el tipo de operación.</span><span class="sxs-lookup"><span data-stu-id="4e50f-235">The `op` property indicates the type of operation.</span></span>
* <span data-ttu-id="4e50f-236">La propiedad `path` indica el elemento que se va a actualizar.</span><span class="sxs-lookup"><span data-stu-id="4e50f-236">The `path` property indicates the element to update.</span></span>
* <span data-ttu-id="4e50f-237">La propiedad `value` proporciona el nuevo valor.</span><span class="sxs-lookup"><span data-stu-id="4e50f-237">The `value` property provides the new value.</span></span>

### <a name="resource-after-patch"></a><span data-ttu-id="4e50f-238">Recurso después de la revisión</span><span class="sxs-lookup"><span data-stu-id="4e50f-238">Resource after patch</span></span>

<span data-ttu-id="4e50f-239">Este es el recurso después de aplicar el documento JSON Patch anterior:</span><span class="sxs-lookup"><span data-stu-id="4e50f-239">Here's the resource after applying the preceding JSON Patch document:</span></span>

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

<span data-ttu-id="4e50f-240">Los cambios realizados mediante la aplicación de un documento JSON Patch a un recurso son atómicos: si alguna operación de la lista produce un error, no se aplica ninguna operación de la lista.</span><span class="sxs-lookup"><span data-stu-id="4e50f-240">The changes made by applying a JSON Patch document to a resource are atomic: if any operation in the list fails, no operation in the list is applied.</span></span>

## <a name="path-syntax"></a><span data-ttu-id="4e50f-241">Sintaxis de path</span><span class="sxs-lookup"><span data-stu-id="4e50f-241">Path syntax</span></span>

<span data-ttu-id="4e50f-242">La propiedad [path](https://tools.ietf.org/html/rfc6901) de un objeto de operación tiene barras inversas entre niveles.</span><span class="sxs-lookup"><span data-stu-id="4e50f-242">The [path](https://tools.ietf.org/html/rfc6901) property of an operation object has slashes between levels.</span></span> <span data-ttu-id="4e50f-243">Por ejemplo: `"/address/zipCode"`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-243">For example, `"/address/zipCode"`.</span></span>

<span data-ttu-id="4e50f-244">Para especificar elementos de matriz se usan índices de base cero.</span><span class="sxs-lookup"><span data-stu-id="4e50f-244">Zero-based indexes are used to specify array elements.</span></span> <span data-ttu-id="4e50f-245">El primer elemento de la matriz `addresses` estaría en `/addresses/0`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-245">The first element of the `addresses` array would be at `/addresses/0`.</span></span> <span data-ttu-id="4e50f-246">Para usar `add` al final de una matriz, use un guion (-) en lugar de un número de índice: `/addresses/-`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-246">To `add` to the end of an array, use a hyphen (-) rather than an index number: `/addresses/-`.</span></span>

### <a name="operations"></a><span data-ttu-id="4e50f-247">Operaciones</span><span class="sxs-lookup"><span data-stu-id="4e50f-247">Operations</span></span>

<span data-ttu-id="4e50f-248">En la siguiente tabla se muestran las operaciones admitidas, como se ha definido en la [especificación de JSON Patch](https://tools.ietf.org/html/rfc6902):</span><span class="sxs-lookup"><span data-stu-id="4e50f-248">The following table shows supported operations as defined in the [JSON Patch specification](https://tools.ietf.org/html/rfc6902):</span></span>

|<span data-ttu-id="4e50f-249">Operación</span><span class="sxs-lookup"><span data-stu-id="4e50f-249">Operation</span></span>  | <span data-ttu-id="4e50f-250">Notas</span><span class="sxs-lookup"><span data-stu-id="4e50f-250">Notes</span></span> |
|-----------|--------------------------------|
| `add`     | <span data-ttu-id="4e50f-251">Agrega un elemento de propiedad o matriz.</span><span class="sxs-lookup"><span data-stu-id="4e50f-251">Add a property or array element.</span></span> <span data-ttu-id="4e50f-252">Para la propiedad existente: establece el valor.</span><span class="sxs-lookup"><span data-stu-id="4e50f-252">For existing property: set value.</span></span>|
| `remove`  | <span data-ttu-id="4e50f-253">Quita un elemento de propiedad o matriz.</span><span class="sxs-lookup"><span data-stu-id="4e50f-253">Remove a property or array element.</span></span> |
| `replace` | <span data-ttu-id="4e50f-254">Lo mismo que `remove` seguido de `add` en la misma ubicación.</span><span class="sxs-lookup"><span data-stu-id="4e50f-254">Same as `remove` followed by `add` at same location.</span></span> |
| `move`    | <span data-ttu-id="4e50f-255">Lo mismo que `remove` desde el origen seguido de `add` al destino mediante el valor del origen.</span><span class="sxs-lookup"><span data-stu-id="4e50f-255">Same as `remove` from source followed by `add` to destination using value from source.</span></span> |
| `copy`    | <span data-ttu-id="4e50f-256">Lo mismo que `add` al destino mediante el valor del origen.</span><span class="sxs-lookup"><span data-stu-id="4e50f-256">Same as `add` to destination using value from source.</span></span> |
| `test`    | <span data-ttu-id="4e50f-257">Devuelve el código de estado correcto si el valor en `path` = al `value` proporcionado.</span><span class="sxs-lookup"><span data-stu-id="4e50f-257">Return success status code if value at `path` = provided `value`.</span></span>|

## <a name="jsonpatch-in-aspnet-core"></a><span data-ttu-id="4e50f-258">JsonPatch en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4e50f-258">JsonPatch in ASP.NET Core</span></span>

<span data-ttu-id="4e50f-259">La implementación de ASP.NET Core de JSON Patch se proporciona en el paquete NuGet [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/).</span><span class="sxs-lookup"><span data-stu-id="4e50f-259">The ASP.NET Core implementation of JSON Patch is provided in the [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) NuGet package.</span></span> <span data-ttu-id="4e50f-260">El paquete se incluye en el metapaquete [Microsoft.AspnetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="4e50f-260">The package is included in the [Microsoft.AspnetCore.App](xref:fundamentals/metapackage-app) metapackage.</span></span>

## <a name="action-method-code"></a><span data-ttu-id="4e50f-261">Código del método de acción</span><span class="sxs-lookup"><span data-stu-id="4e50f-261">Action method code</span></span>

<span data-ttu-id="4e50f-262">En un controlador de API, un método de acción para JSON Patch:</span><span class="sxs-lookup"><span data-stu-id="4e50f-262">In an API controller, an action method for JSON Patch:</span></span>

* <span data-ttu-id="4e50f-263">Se anota con el atributo `HttpPatch`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-263">Is annotated with the `HttpPatch` attribute.</span></span>
* <span data-ttu-id="4e50f-264">Acepta `JsonPatchDocument<T>`, normalmente con `[FromBody]`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-264">Accepts a `JsonPatchDocument<T>`, typically with `[FromBody]`.</span></span>
* <span data-ttu-id="4e50f-265">Llama a `ApplyTo` en el documento de revisión para aplicar los cambios.</span><span class="sxs-lookup"><span data-stu-id="4e50f-265">Calls `ApplyTo` on the patch document to apply the changes.</span></span>

<span data-ttu-id="4e50f-266">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="4e50f-266">Here's an example:</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_PatchAction&highlight=1,3,9)]

<span data-ttu-id="4e50f-267">Este código de la aplicación de ejemplo funciona con el siguiente modelo `Customer`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-267">This code from the sample app works with the following `Customer` model.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Models/Customer.cs?name=snippet_Customer)]

[!code-csharp[](jsonpatch/samples/2.2/Models/Order.cs?name=snippet_Order)]

<span data-ttu-id="4e50f-268">El método de acción de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="4e50f-268">The sample action method:</span></span>

* <span data-ttu-id="4e50f-269">Construye un objeto `Customer`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-269">Constructs a `Customer`.</span></span>
* <span data-ttu-id="4e50f-270">Aplica la revisión.</span><span class="sxs-lookup"><span data-stu-id="4e50f-270">Applies the patch.</span></span>
* <span data-ttu-id="4e50f-271">Devuelve el resultado en el cuerpo de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="4e50f-271">Returns the result in the body of the response.</span></span>

 <span data-ttu-id="4e50f-272">En una aplicación real, el código recuperaría los datos de un almacén como una base de datos y actualizaría la base de datos después de aplicar la revisión.</span><span class="sxs-lookup"><span data-stu-id="4e50f-272">In a real app, the code would retrieve the data from a store such as a database and update the database after applying the patch.</span></span>

### <a name="model-state"></a><span data-ttu-id="4e50f-273">Estado del modelo</span><span class="sxs-lookup"><span data-stu-id="4e50f-273">Model state</span></span>

<span data-ttu-id="4e50f-274">En el ejemplo anterior del método de acción, se llama a una sobrecarga de `ApplyTo` que toma el estado del modelo como uno de sus parámetros.</span><span class="sxs-lookup"><span data-stu-id="4e50f-274">The preceding action method example calls an overload of `ApplyTo` that takes model state as one of its parameters.</span></span> <span data-ttu-id="4e50f-275">Con esta opción, puede obtener mensajes de error en las respuestas.</span><span class="sxs-lookup"><span data-stu-id="4e50f-275">With this option, you can get error messages in responses.</span></span> <span data-ttu-id="4e50f-276">En el ejemplo siguiente se muestra el cuerpo de una respuesta 400 Solicitud incorrecta de una operación `test`:</span><span class="sxs-lookup"><span data-stu-id="4e50f-276">The following example shows the body of a 400 Bad Request response for a `test` operation:</span></span>

```json
{
    "Customer": [
        "The current value 'John' at path 'customerName' is not equal to the test value 'Nancy'."
    ]
}
```

### <a name="dynamic-objects"></a><span data-ttu-id="4e50f-277">Objetos dinámicos</span><span class="sxs-lookup"><span data-stu-id="4e50f-277">Dynamic objects</span></span>

<span data-ttu-id="4e50f-278">En el ejemplo siguiente de método de acción se muestra cómo aplicar una revisión a un objeto dinámico.</span><span class="sxs-lookup"><span data-stu-id="4e50f-278">The following action method example shows how to apply a patch to a dynamic object.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_Dynamic)]

## <a name="the-add-operation"></a><span data-ttu-id="4e50f-279">La operación add</span><span class="sxs-lookup"><span data-stu-id="4e50f-279">The add operation</span></span>

* <span data-ttu-id="4e50f-280">Si `path` apunta a un elemento de matriz: inserta un nuevo elemento delante del especificado por `path`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-280">If `path` points to an array element: inserts new element before the one specified by `path`.</span></span>
* <span data-ttu-id="4e50f-281">Si `path` apunta a una propiedad: establece el valor de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="4e50f-281">If `path` points to a property: sets the property value.</span></span>
* <span data-ttu-id="4e50f-282">Si `path` apunta a una ubicación que no existe:</span><span class="sxs-lookup"><span data-stu-id="4e50f-282">If `path` points to a nonexistent location:</span></span>
  * <span data-ttu-id="4e50f-283">Si el recurso para revisar es un objeto dinámico: agrega una propiedad.</span><span class="sxs-lookup"><span data-stu-id="4e50f-283">If the resource to patch is a dynamic object: adds a property.</span></span>
  * <span data-ttu-id="4e50f-284">Si el recurso para revisar es un objeto estático: la solicitud produce un error.</span><span class="sxs-lookup"><span data-stu-id="4e50f-284">If the resource to patch is a static object: the request fails.</span></span>

<span data-ttu-id="4e50f-285">El siguiente documento de revisión de ejemplo establece el valor de `CustomerName` y agrega un objeto `Order` al final de la matriz `Orders`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-285">The following sample patch document sets the value of `CustomerName` and adds an `Order` object to the end of the `Orders` array.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

## <a name="the-remove-operation"></a><span data-ttu-id="4e50f-286">La operación remove</span><span class="sxs-lookup"><span data-stu-id="4e50f-286">The remove operation</span></span>

* <span data-ttu-id="4e50f-287">Si `path` apunta a un elemento de matriz: quita el elemento.</span><span class="sxs-lookup"><span data-stu-id="4e50f-287">If `path` points to an array element: removes the element.</span></span>
* <span data-ttu-id="4e50f-288">Si `path` apunta a una propiedad:</span><span class="sxs-lookup"><span data-stu-id="4e50f-288">If `path` points to a property:</span></span>
  * <span data-ttu-id="4e50f-289">Si el recurso para revisar es un objeto dinámico: quita la propiedad.</span><span class="sxs-lookup"><span data-stu-id="4e50f-289">If resource to patch is a dynamic object: removes the property.</span></span>
  * <span data-ttu-id="4e50f-290">Si el recurso para revisar es un objeto estático:</span><span class="sxs-lookup"><span data-stu-id="4e50f-290">If resource to patch is a static object:</span></span>
    * <span data-ttu-id="4e50f-291">Si la propiedad acepta valores NULL: la establece en null.</span><span class="sxs-lookup"><span data-stu-id="4e50f-291">If the property is nullable: sets it to null.</span></span>
    * <span data-ttu-id="4e50f-292">Si la propiedad es distinta de null, la establece en `default<T>`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-292">If the property is non-nullable, sets it to `default<T>`.</span></span>

<span data-ttu-id="4e50f-293">En el siguiente documento de revisión de ejemplo, se establece `CustomerName` en null y se elimina `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-293">The following sample patch document sets `CustomerName` to null and deletes `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/remove.json)]

## <a name="the-replace-operation"></a><span data-ttu-id="4e50f-294">La operación replace</span><span class="sxs-lookup"><span data-stu-id="4e50f-294">The replace operation</span></span>

<span data-ttu-id="4e50f-295">Esta operación es funcionalmente igual que `remove` seguida de `add`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-295">This operation is functionally the same as a `remove` followed by an `add`.</span></span>

<span data-ttu-id="4e50f-296">En el siguiente documento de revisión de ejemplo se establece el valor de `CustomerName` y se reemplaza `Orders[0]` por un nuevo objeto `Order`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-296">The following sample patch document sets the value of `CustomerName` and replaces `Orders[0]`with a new `Order` object.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/replace.json)]

## <a name="the-move-operation"></a><span data-ttu-id="4e50f-297">La operación move</span><span class="sxs-lookup"><span data-stu-id="4e50f-297">The move operation</span></span>

* <span data-ttu-id="4e50f-298">Si `path` apunta a un elemento de matriz: copia el elemento `from` en la ubicación del elemento `path` y, luego, ejecuta una operación `remove` en el elemento `from`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-298">If `path` points to an array element: copies `from` element to location of `path` element, then runs a `remove` operation on the `from` element.</span></span>
* <span data-ttu-id="4e50f-299">Si `path` apunta a una propiedad: copia el valor de la propiedad `from` en la propiedad `path` y, luego, ejecuta la operación `remove` en la propiedad `from`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-299">If `path` points to a property: copies value of `from` property to `path` property, then runs a `remove` operation on the `from` property.</span></span>
* <span data-ttu-id="4e50f-300">Si `path` apunta a una propiedad que no existe:</span><span class="sxs-lookup"><span data-stu-id="4e50f-300">If `path` points to a nonexistent property:</span></span>
  * <span data-ttu-id="4e50f-301">Si el recurso para revisar es un objeto estático: la solicitud produce un error.</span><span class="sxs-lookup"><span data-stu-id="4e50f-301">If the resource to patch is a static object: the request fails.</span></span>
  * <span data-ttu-id="4e50f-302">Si el recurso para revisar es un objeto dinámico: copia la propiedad `from` en la ubicación indicada por `path` y, luego, ejecuta una operación `remove` en la propiedad `from`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-302">If the resource to patch is a dynamic object: copies `from` property to location indicated by `path`, then runs a `remove` operation on the `from` property.</span></span>

<span data-ttu-id="4e50f-303">En el siguiente documento de revisión de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="4e50f-303">The following sample patch document:</span></span>

* <span data-ttu-id="4e50f-304">Se copia el valor de `Orders[0].OrderName` en `CustomerName`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-304">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="4e50f-305">Se establece `Orders[0].OrderName` en null.</span><span class="sxs-lookup"><span data-stu-id="4e50f-305">Sets `Orders[0].OrderName` to null.</span></span>
* <span data-ttu-id="4e50f-306">Se mueve `Orders[1]` delante de `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-306">Moves `Orders[1]` to before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/move.json)]

## <a name="the-copy-operation"></a><span data-ttu-id="4e50f-307">La operación copy</span><span class="sxs-lookup"><span data-stu-id="4e50f-307">The copy operation</span></span>

<span data-ttu-id="4e50f-308">Esta operación es funcionalmente igual que la operación `move` sin el paso `remove` final.</span><span class="sxs-lookup"><span data-stu-id="4e50f-308">This operation is functionally the same as a `move` operation without the final `remove` step.</span></span>

<span data-ttu-id="4e50f-309">En el siguiente documento de revisión de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="4e50f-309">The following sample patch document:</span></span>

* <span data-ttu-id="4e50f-310">Se copia el valor de `Orders[0].OrderName` en `CustomerName`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-310">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="4e50f-311">Se inserta una copia de `Orders[1]` delante de `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="4e50f-311">Inserts a copy of `Orders[1]` before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/copy.json)]

## <a name="the-test-operation"></a><span data-ttu-id="4e50f-312">La operación test</span><span class="sxs-lookup"><span data-stu-id="4e50f-312">The test operation</span></span>

<span data-ttu-id="4e50f-313">Si el valor de la ubicación indicada por `path` es diferente del valor proporcionado en `value`, la solicitud produce un error.</span><span class="sxs-lookup"><span data-stu-id="4e50f-313">If the value at the location indicated by `path` is different from the value provided in `value`, the request fails.</span></span> <span data-ttu-id="4e50f-314">En ese caso, la solicitud PATCH entera produce un error incluso si todas las demás operaciones del documento de revisión se realizan correctamente.</span><span class="sxs-lookup"><span data-stu-id="4e50f-314">In that case, the whole PATCH request fails even if all other operations in the patch document would otherwise succeed.</span></span>

<span data-ttu-id="4e50f-315">La operación `test` se usa habitualmente para impedir una actualización cuando hay un conflicto de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="4e50f-315">The `test` operation is commonly used to prevent an update when there's a concurrency conflict.</span></span>

<span data-ttu-id="4e50f-316">El siguiente documento de revisión de ejemplo no tiene ningún efecto si el valor inicial de `CustomerName` es "John", porque la prueba produce un error:</span><span class="sxs-lookup"><span data-stu-id="4e50f-316">The following sample patch document has no effect if the initial value of `CustomerName` is "John", because the test fails:</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/test-fail.json)]

## <a name="get-the-code"></a><span data-ttu-id="4e50f-317">Obtención del código</span><span class="sxs-lookup"><span data-stu-id="4e50f-317">Get the code</span></span>

<span data-ttu-id="4e50f-318">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span><span class="sxs-lookup"><span data-stu-id="4e50f-318">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span></span> <span data-ttu-id="4e50f-319">([Método de descarga](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="4e50f-319">([How to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="4e50f-320">Para probar el ejemplo, ejecute la aplicación y envíe solicitudes HTTP con la configuración siguiente:</span><span class="sxs-lookup"><span data-stu-id="4e50f-320">To test the sample, run the app and send HTTP requests with the following settings:</span></span>

* <span data-ttu-id="4e50f-321">DIRECCIÓN URL: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span><span class="sxs-lookup"><span data-stu-id="4e50f-321">URL: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span></span>
* <span data-ttu-id="4e50f-322">Método HTTP: `PATCH`</span><span class="sxs-lookup"><span data-stu-id="4e50f-322">HTTP method: `PATCH`</span></span>
* <span data-ttu-id="4e50f-323">Encabezado: `Content-Type: application/json-patch+json`</span><span class="sxs-lookup"><span data-stu-id="4e50f-323">Header: `Content-Type: application/json-patch+json`</span></span>
* <span data-ttu-id="4e50f-324">Cuerpo: Copie y pegue uno de los ejemplos de documento de revisión de JSON de la carpeta del proyecto *JSON*.</span><span class="sxs-lookup"><span data-stu-id="4e50f-324">Body: Copy and paste one of the JSON patch document samples from the *JSON* project folder.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4e50f-325">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="4e50f-325">Additional resources</span></span>

* [<span data-ttu-id="4e50f-326">Especificación del método PATCH de IETF RFC 5789</span><span class="sxs-lookup"><span data-stu-id="4e50f-326">IETF RFC 5789 PATCH method specification</span></span>](https://tools.ietf.org/html/rfc5789)
* [<span data-ttu-id="4e50f-327">Especificación del método JSON PATCH de IETF RFC 6902</span><span class="sxs-lookup"><span data-stu-id="4e50f-327">IETF RFC 6902 JSON Patch specification</span></span>](https://tools.ietf.org/html/rfc6902)
* [<span data-ttu-id="4e50f-328">Especificación del formato de ruta de acceso JSON Patch de IETF RFC 6901</span><span class="sxs-lookup"><span data-stu-id="4e50f-328">IETF RFC 6901 JSON Patch path format spec</span></span>](https://tools.ietf.org/html/rfc6901)
* <span data-ttu-id="4e50f-329">[Documentación de JSON Patch](https://jsonpatch.com/).</span><span class="sxs-lookup"><span data-stu-id="4e50f-329">[JSON Patch documentation](https://jsonpatch.com/).</span></span> <span data-ttu-id="4e50f-330">Incluye vínculos a recursos para crear documentos JSON Patch.</span><span class="sxs-lookup"><span data-stu-id="4e50f-330">Includes links to resources for creating JSON Patch documents.</span></span>
* [<span data-ttu-id="4e50f-331">Código fuente de JSON Patch para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4e50f-331">ASP.NET Core JSON Patch source code</span></span>](https://github.com/dotnet/AspNetCore/tree/master/src/Features/JsonPatch/src)

::: moniker-end
