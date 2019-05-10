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
# <a name="controller-action-return-types-in-aspnet-core-web-api"></a>Tipos de valor devuelto de acción del controlador de ASP.NET Core Web API

Por [Scott Addie](https://github.com/scottaddie)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

ASP.NET Core ofrece las siguientes opciones relativas a los tipos de valor devuelto de acción del controlador de Web API:

::: moniker range=">= aspnetcore-2.1"

* [Tipo específico](#specific-type)
* [IActionResult](#iactionresult-type)
* [ActionResult\<T>](#actionresultt-type)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* [Tipo específico](#specific-type)
* [IActionResult](#iactionresult-type)

::: moniker-end

En este documento se explica cuándo resulta más adecuado usar cada tipo de valor devuelto.

## <a name="specific-type"></a>Tipo específico

La acción más sencilla devuelve un tipo de datos primitivo o complejo (por ejemplo, `string` o un tipo de objeto personalizado). Consideremos la siguiente acción, que devuelve una colección de objetos `Product` personalizados:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_Get)]

Sin condiciones conocidas de las que haya que protegerse durante la ejecución de la acción, bastaría con que se devolviera un tipo específico. La acción anterior no acepta parámetros, por lo que no se necesita ninguna validación de restricciones de parámetros.

Si existen condiciones conocidas que deban tenerse en cuenta en una acción, se presentan varias rutas de acceso de valor devuelto. En tal caso, lo habitual es mezclar un tipo devuelto [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) con el tipo de valor devuelto primitivo o complejo. Se necesitará [IActionResult](#iactionresult-type) o [ActionResult\<T>](#actionresultt-type) para dar cabida a este tipo de acción.

## <a name="iactionresult-type"></a>Tipo IActionResult

El tipo de valor devuelto [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) resulta adecuado cuando existen varios tipos de valor devuelto [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) posibles en una acción. Los tipos `ActionResult` representan varios códigos de estado HTTP. Algunos de los tipos de valor devuelto que suelen entrar en esta categoría son [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400), [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404) y [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).

Dado que hay varios tipos de valor devuelto y rutas de acceso en la acción, es necesario el uso libre del atributo [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor). Este atributo genera detalles de respuesta más pormenorizados relativos a las páginas de ayuda de API generadas por herramientas como [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger). `[ProducesResponseType]` indica los tipos conocidos y los códigos de estado HTTP que la acción va a devolver.

### <a name="synchronous-action"></a>Acción sincrónica

Veamos la siguiente acción sincrónica, en la que pueden darse dos tipos de valor devuelto posibles:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

En la acción anterior, se devuelve un código de estado 404 cuando el producto representado por `id` no existe en el almacén de datos subyacente. El método del asistente [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) se invoca como una forma abreviada de `return new NotFoundResult();`. Si el producto existe, se devuelve un objeto `Product` que representa la carga con un código de estado 200. El método del asistente [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) se invoca como una forma abreviada de `return new OkObjectResult(product);`.

### <a name="asynchronous-action"></a>Acción asincrónica

Veamos la siguiente acción asincrónica, en la que pueden darse dos tipos de valor devuelto posibles:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

En el código anterior:

* El entorno de ejecución de ASP.NET Core devuelve un código de estado 400 ([BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*)) cuando la descripción del producto contiene "Widget XYZ".
* Al crear un producto, el método [CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) genera un código de estado 201. En esta ruta de acceso de código, se devuelve el objeto `Product`.

Por ejemplo, el siguiente modelo indica que las solicitudes deben incluir las propiedades `Name` y `Description`. Por lo tanto, si no se proporcionan `Name` y `Description` en la solicitud, se producirá un error en la validación de los modelos.

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.DataAccess/Models/Product.cs?name=snippet_ProductClass&highlight=5-6,8-9)]

::: moniker range=">= aspnetcore-2.1"

En ASP.NET Core 2.1 o versiones posteriores, si se aplica el atributo [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute), los errores de validación de los modelos generarán un código de estado 400. Para obtener más información, consulte [Respuestas HTTP 400 automáticas](xref:web-api/index#automatic-http-400-responses).

## <a name="actionresultt-type"></a>Tipo ActionResult\<T>

ASP.NET Core 2.1 incorpora el tipo de valor devuelto [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) para las acciones de controlador de Web API. Permite devolver un tipo que se deriva de [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) o bien un [tipo específico](#specific-type). `ActionResult<T>` reporta las siguientes ventajas con frente al [tipo IActionResult](#iactionresult-type):

* La propiedad `Type` del atributo [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) se puede excluir. Por ejemplo, `[ProducesResponseType(200, Type = typeof(Product))]` se simplifica a `[ProducesResponseType(200)]`. En su lugar, el tipo de valor devuelto esperado de la acción se infiere desde `T` en `ActionResult<T>`.
* Los [operadores de conversión implícitos](/dotnet/csharp/language-reference/keywords/implicit) admiten la conversión tanto de `T` como de `ActionResult` en `ActionResult<T>`. `T` se convierte en [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult), lo que significa que `return new ObjectResult(T);` se ha simplificado para `return T;`.

C# no admite operadores de conversión implícitos en las interfaces. Por consiguiente, la conversión de la interfaz a un tipo concreto es necesaria para usar `ActionResult<T>`. Por ejemplo, el uso de `IEnumerable` en el siguiente ejemplo no funciona:

```csharp
[HttpGet]
public ActionResult<IEnumerable<Product>> Get()
{
    return _repository.GetProducts();
}
```

Una opción para corregir el código anterior es devolver `_repository.GetProducts().ToList();`.

La mayoría de las acciones tiene un tipo de valor devuelto específico. Se pueden producir condiciones inesperadas que durante la ejecución de una acción, en cuyo caso no se devuelve el tipo específico. Por ejemplo, puede suceder que el parámetro de entrada de una acción genere un error de validación de modelos. En tal caso, lo normal es que se devuelva el tipo `ActionResult` correspondiente en lugar del tipo específico.

### <a name="synchronous-action"></a>Acción sincrónica

Veamos una acción sincrónica, en la que pueden darse dos tipos de valor devuelto posibles:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

En el código anterior, se devuelve un código de estado 404 cuando el producto no existe en la base de datos. Si el producto existe, se devuelve el objeto `Product` correspondiente. Antes de ASP.NET Core 2.1, la línea `return product;` habría sido `return Ok(product);`.

> [!TIP]
> A partir de ASP.NET Core 2.1, la inferencia del origen de enlace de los parámetros de la acción está habilitada cuando una clase de controlador incluye el atributo `[ApiController]`. Así, un nombre de parámetro que coincida con un nombre de la plantilla de ruta se enlazará automáticamente usando los datos de ruta de la solicitud. Por lo tanto, el parámetro `id` de la acción anterior no está anotado explícitamente con el atributo [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute).

### <a name="asynchronous-action"></a>Acción asincrónica

Veamos una acción asincrónica, en la que pueden darse dos tipos de valor devuelto posibles:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

En el código anterior:

* El entorno de ejecución de ASP.NET Core devuelve un código de estado 400 ([BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*)) en los casos siguientes:
  * El atributo [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) se ha aplicado y se produce un error de validación de los modelos.
  * La descripción del producto contiene "Widget XYZ".
* Al crear un producto, el método [CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) genera un código de estado 201. En esta ruta de acceso de código, se devuelve el objeto `Product`.

> [!TIP]
> A partir de ASP.NET Core 2.1, la inferencia del origen de enlace de los parámetros de la acción está habilitada cuando una clase de controlador incluye el atributo `[ApiController]`. Los parámetros de tipo complejo se enlazan automáticamente usando el cuerpo de solicitud. Por lo tanto, el parámetro `product` de la acción anterior no está anotado explícitamente con el atributo [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute).

::: moniker-end

## <a name="additional-resources"></a>Recursos adicionales

* <xref:mvc/controllers/actions>
* <xref:mvc/models/validation>
* <xref:tutorials/web-api-help-pages-using-swagger>
