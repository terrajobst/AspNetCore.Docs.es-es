---
title: Creación de API web con ASP.NET Core
author: scottaddie
description: Conozca los aspectos básicos de la creación de una API web en ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 05/07/2019
uid: web-api/index
ms.openlocfilehash: 593fd33babc81cddfc4db2150a37e5ec3bc1a0be
ms.sourcegitcommit: a3926eae3f687013027a2828830c12a89add701f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65450839"
---
# <a name="create-web-apis-with-aspnet-core"></a>Creación de API web con ASP.NET Core

Por [Scott Addie](https://github.com/scottaddie) y [Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core admite la creación de servicios RESTful, lo que también se conoce como API web, mediante C#. Para gestionar las solicitudes, una API web usa controladores. Los *controladores* de una API web son clases que se derivan de `ControllerBase`. En este artículo se muestra cómo usar controladores para gestionar las solicitudes de API.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples). ([Método de descarga](xref:index#how-to-download-a-sample)).

## <a name="controllerbase-class"></a>Clase ControllerBase

Una API web tiene una o varias clases de controlador que se derivan de <xref:Microsoft.AspNetCore.Mvc.ControllerBase>. Por ejemplo, la plantilla de proyecto de API web crea un controlador de valores:

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=3)]

No cree un controlador de API web mediante la derivación de la clase base <xref:Microsoft.AspNetCore.Mvc.Controller>. `Controller` se deriva de `ControllerBase` y agrega compatibilidad con vistas, por lo que sirve para gestionar páginas web, no solicitudes de API web.  Hay una excepción a esta regla: si tiene pensado usar el mismo controlador tanto para vistas como para API, debe derivarlo de `Controller`.

La clase `ControllerBase` ofrece muchas propiedades y métodos que son útiles para gestionar solicitudes HTTP. Por ejemplo, `ControllerBase.CreatedAtAction` devuelve un código de estado 201:

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=8-9)]

 A continuación se muestran algunos ejemplos más de métodos que proporciona `ControllerBase`.

|Método  |Notas  |
|---------|---------|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*>| Devuelve el código de estado 400.|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> |Devuelve el código de estado 404.|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.PhysicalFile*>|Devuelve un archivo.|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>|Invoca el [enlace de modelo](xref:mvc/models/model-binding).|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryValidateModel*>|Invoca la [validación de modelos](xref:mvc/models/validation).|

Para ver una lista de todos los métodos y propiedades disponibles, consulte <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.

## <a name="attributes"></a>Atributos

El espacio de nombres <xref:Microsoft.AspNetCore.Mvc> proporciona atributos que se pueden usar para configurar el comportamiento de los controladores API web y los métodos de acción. En el ejemplo siguiente se usan atributos para especificar el método HTTP aceptado y los códigos de estado devueltos:

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=1-3)]

Estos son algunos ejemplos más de atributos que están disponibles.

|Atributo|Notas|
|---------|-----|
|[[Route]](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)      |Especifica el patrón de dirección URL de un controlador o una acción.|
|[[Bind]](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)        |Especifica el prefijo y las propiedades que se incluirán en el enlace de modelo.|
|[[HttpGet]](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)  |Identifica una acción que admite el método HTTP GET.|
|[[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)|Especifica los tipos de datos que acepta una acción.|
|[[Produces]](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)|Especifica los tipos de datos que devuelve una acción.|

Para ver una lista que incluye los atributos disponibles, consulte el espacio de nombres <xref:Microsoft.AspNetCore.Mvc>.

## <a name="apicontroller-attribute"></a>Atributo ApiController

El atributo [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) puede aplicarse a una clase de controlador para permitir comportamientos específicos de API:

* [Requisito de enrutamiento mediante atributos](#attribute-routing-requirement)
* [Respuestas HTTP 400 automáticas](#automatic-http-400-responses)
* [Inferencia de parámetro de origen de enlace](#binding-source-parameter-inference)
* [Inferencia de solicitud de varios elementos o datos de formulario](#multipartform-data-request-inference)
* [Detalles de problemas de los códigos de estado de error](#problem-details-for-error-status-codes)

Estas características requieren una [versión de compatibilidad](<xref:mvc/compatibility-version>) de 2.1 o posterior.

### <a name="apicontroller-on-specific-controllers"></a>ApiController en controladores específicos

El atributo `[ApiController]` puede aplicarse a controladores específicos, como se muestra en el siguiente ejemplo de la plantilla de proyecto:

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=2)]

### <a name="apicontroller-on-multiple-controllers"></a>ApiController en varios controladores

Una estrategia para el uso del atributo en más de un controlador consiste en crear una clase personalizada de controlador base anotada con el atributo `[ApiController]`. Este es un ejemplo que muestra una clase base personalizada y un controlador que se deriva de ella:

[!code-csharp[](index/samples/2.x/Controllers/MyControllerBase.cs?name=snippet_MyControllerBase)]

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_Inherit)]

### <a name="apicontroller-on-an-assembly"></a>ApiController en un ensamblado

Si la [versión de compatibilidad](<xref:mvc/compatibility-version>) está establecida en 2.2 o posterior, el atributo `[ApiController]` se puede aplicar a un ensamblado. En este contexto, la anotación aplica el comportamiento de API web a todos los controladores del ensamblado. No hay ninguna manera de excluir controladores específicos. Aplique el atributo de nivel de ensamblado a la clase `Startup`, tal y como se muestra en el ejemplo siguiente:

```csharp
[assembly: ApiController]
namespace WebApiSample
{
    public class Startup
    {
        ...
    }
}
```

## <a name="attribute-routing-requirement"></a>Requisito de enrutamiento mediante atributos

El atributo `ApiController` convierte el enrutamiento de atributos en un requisito. Por ejemplo:

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=1)]

Las acciones no son accesibles mediante [rutas convencionales](xref:mvc/controllers/routing#conventional-routing) definidas por <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> o <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> en `Startup.Configure`.

## <a name="automatic-http-400-responses"></a>Respuestas HTTP 400 automáticas

El atributo `ApiController` hace que los errores de validación de un modelo desencadenen automáticamente una respuesta HTTP 400. Por lo tanto, el siguiente código no es necesario en un método de acción:

```csharp
if (!ModelState.IsValid)
{
    return BadRequest(ModelState);
}
```

### <a name="default-badrequest-response"></a>Respuesta BadRequest predeterminada 

Con una versión de compatibilidad de 2.2 o posterior, el tipo de respuesta predeterminado para las respuestas HTTP 400 es <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>. El tipo `ValidationProblemDetails` cumple los requisitos de la [especificación RFC 7807](https://tools.ietf.org/html/rfc7807).

Para cambiar la respuesta predeterminada por <xref:Microsoft.AspNetCore.Mvc.SerializableError>, establezca la propiedad `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` como `true` en `Startup.ConfigureServices`, como se muestra en el ejemplo siguiente:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,9)]

### <a name="customize-badrequest-response"></a>Respuesta BadRequest personalizada

Para personalizar la respuesta que es consecuencia de un error de validación, use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory>. Agregue el código resaltado siguiente después de `services.AddMvc().SetCompatibilityVersion`:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureBadRequestResponse&highlight=3-20)]

### <a name="log-automatic-400-responses"></a>Registro de respuestas 400 automáticas

Consulte [cómo registrar respuestas 400 automáticas sobre errores de validación de modelos (aspnet/AspNetCore.Docs #12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157).

### <a name="disable-automatic-400"></a>Deshabilitación de respuestas 400 automáticas

Para deshabilitar el comportamiento 400 automático, establezca la propiedad <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> en `true`. Agregue el código resaltado siguiente en `Startup.ConfigureServices` después de `services.AddMvc().SetCompatibilityVersion`:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,7)]

## <a name="binding-source-parameter-inference"></a>Inferencia de parámetro de origen de enlace

Un atributo de origen de enlace define la ubicación del valor del parámetro de una acción. Existen los atributos de origen de enlace siguientes:

|Atributo|Origen de enlace |
|---------|---------|
|[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)     | Cuerpo de la solicitud |
|[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)     | Datos del formulario en el cuerpo de la solicitud |
|[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) | Encabezado de la solicitud |
|[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)   | Parámetro de la cadena de consulta de la solicitud |
|[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)   | Datos de ruta de la solicitud actual |
|[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices) | Servicio de solicitud insertado como parámetro de acción |

> [!WARNING]
> No use `[FromRoute]` si los valores pueden contener `%2f` (es decir, `/`). `%2f` no incluirá el carácter sin escape `/`. Use `[FromQuery]` si el valor puede contener `%2f`.

Sin el atributo `[ApiController]` o los atributos de origen de enlace, como `[FromQuery]`, el entorno de tiempo de ejecución de ASP.NET Core intenta usar el enlazador de modelos de objetos complejos. El enlazador de modelos de objetos complejos extrae los datos de los proveedores de valor en un orden definido.

En el ejemplo siguiente, el atributo `[FromQuery]` indica que el valor del parámetro `discontinuedOnly` se proporciona en la cadena de consulta de la dirección URL de la solicitud:

[!code-csharp[](index/samples/2.x/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

El atributo `[ApiController]` aplica reglas de inferencia a los orígenes de datos predeterminados de los parámetros de acción. Estas reglas aplican atributos a los parámetros de acción, lo que ahorra la necesidad de identificar los orígenes de enlace manualmente. Las reglas de inferencia de orígenes de enlace se comportan de la manera siguiente:

* `[FromBody]` se infiere para parámetros de tipo complejo. La excepción a `[FromBody]` esta regla es cualquier tipo integrado complejo que tenga un significado especial, como <xref:Microsoft.AspNetCore.Http.IFormCollection> y <xref:System.Threading.CancellationToken>. El código de inferencia del origen de enlace omite esos tipos especiales. 
* `[FromForm]` se infiere para los parámetros de acción de tipo <xref:Microsoft.AspNetCore.Http.IFormFile> y <xref:Microsoft.AspNetCore.Http.IFormFileCollection>. No se infiere para los tipos simples o definidos por el usuario.
* `[FromRoute]` se infiere para cualquier nombre de parámetro de acción que coincida con un parámetro de la plantilla de ruta. Si varias rutas coinciden con un parámetro de acción, cualquier valor de ruta se considera `[FromRoute]`.
* `[FromQuery]` se infiere para cualquier otro parámetro de acción.

### <a name="frombody-inference-notes"></a>Notas de la inferencia de FromBody

En el caso de los tipos simples, como `string` y `int`, `[FromBody]` no se infiere. Así pues, para los tipos simples, en los casos en los que quiera utilizar dicha funcionalidad, conviene usar el atributo `[FromBody]`.

Cuando una acción tiene más de un parámetro enlazado desde el cuerpo de solicitud, se produce una excepción. Por ejemplo, todas las firmas de acción siguientes provocan una excepción:

* `[FromBody]` se infiere en ambos porque son tipos complejos.

  ```csharp
  [HttpPost]
  public IActionResult Action1(Product product, Order order)
  ```

* El atributo `[FromBody]` en uno se infiere en el otro porque es un tipo complejo.

  ```csharp
  [HttpPost]
  public IActionResult Action2(Product product, [FromBody] Order order)
  ```

* El atributo `[FromBody]` se infiere en ambos.

  ```csharp
  [HttpPost]
  public IActionResult Action3([FromBody] Product product, [FromBody] Order order)
  ```

> [!NOTE]
> En ASP.NET Core 2.1, los parámetros de tipo de colección, como listas y matrices, se infieren incorrectamente como `[FromQuery]`. El atributo `[FromBody]` debe usarse con estos parámetros si van a enlazarse desde el cuerpo de solicitud. Este comportamiento se ha corregido en ASP.NET Core 2.2 o posterior, donde se infieren los parámetros de tipo de colección para enlazarse desde el cuerpo de forma predeterminada.

### <a name="disable-inference-rules"></a>Deshabilitación de las reglas de inferencia

Para deshabilitar la inferencia del origen de enlace, establezca <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> en `true`. Agregue el código siguiente en `Startup.ConfigureServices` tras `services.AddMvc().SetCompatibilityVersion`:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,6)]

## <a name="multipartform-data-request-inference"></a>Inferencia de solicitud de varios elementos o datos de formulario

El atributo `[ApiController]` aplica una regla de inferencia cuando se anota un parámetro de acción con el atributo [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute): se infiere el tipo de contenido de solicitud `multipart/form-data`.

Para deshabilitar el comportamiento predeterminado, establezca <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> como `true` en `Startup.ConfigureServices`, como se muestra en el ejemplo siguiente:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,5)]

## <a name="problem-details-for-error-status-codes"></a>Detalles de problemas de los códigos de estado de error

Cuando la versión de compatibilidad es 2.2 o posterior, MVC transforma un resultado de error (un resultado con el código de estado 400 o superior) en un resultado con <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>. El tipo `ProblemDetails` se basa en la [especificación RFC 7807](https://tools.ietf.org/html/rfc7807) para proporcionar detalles de error de lectura mecánica en una respuesta HTTP.

Observe el código siguiente en una acción de controlador:

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_ProblemDetailsStatusCode)]

La respuesta HTTP para `NotFound` tiene un código de estado 404 con un cuerpo `ProblemDetails`. Por ejemplo:

```json
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

### <a name="customize-problemdetails-response"></a>Respuesta ProblemDetails personalizada

Use la propiedad `ClientErrorMapping` para configurar el contenido de la respuesta `ProblemDetails`. Por ejemplo, el código siguiente permite actualizar la propiedad `type` para respuestas 404:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=10-11)]

### <a name="disable-problemdetails-response"></a>Deshabilitación de la respuesta ProblemDetails

La creación automática de `ProblemDetails` está deshabilitada cuando la propiedad `SuppressMapClientErrors` está establecida en `true`. Agregue el siguiente código en `Startup.ConfigureServices`:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,8)]

## <a name="additional-resources"></a>Recursos adicionales 

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
