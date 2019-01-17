---
title: Compilación de API web con ASP.NET Core
author: scottaddie
description: Obtenga información sobre las características disponibles para la compilación de una API web en ASP.NET Core y los casos en los que se recomienda usar cada una.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/11/2019
uid: web-api/index
ms.openlocfilehash: 8ba20c51f38a43adca4133a402c6d741379a4c54
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341633"
---
# <a name="build-web-apis-with-aspnet-core"></a>Compilación de API web con ASP.NET Core

Por [Scott Addie](https://github.com/scottaddie)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

En este documento se explica cómo crear una API web en ASP.NET Core y los casos en los que se recomienda usar cada una.

## <a name="derive-class-from-controllerbase"></a>Derivación de una clase desde ControllerBase

Herede desde la clase <xref:Microsoft.AspNetCore.Mvc.ControllerBase> en un controlador diseñado para funcionar como API web. Por ejemplo:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

La clase `ControllerBase` proporciona acceso a varios métodos y propiedades. En el código anterior, algunos ejemplos son <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> y <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>. Se llama a estos métodos en los métodos de acción para devolver los códigos de estado HTTP 400 y 201, respectivamente. La propiedad <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState>, que también se proporciona con `ControllerBase`, se usa para controlar la validación del modelo de solicitud.

::: moniker range=">= aspnetcore-2.1"

## <a name="annotation-with-apicontroller-attribute"></a>Anotación con el atributo ApiController

ASP.NET Core 2.1 incorpora el atributo [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) para designar una clase de controlador de API web. Por ejemplo:

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

Para usar este atributo a nivel de controlador, se requiere una versión de compatibilidad de 2.1 o posterior, establecida mediante <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>. Por ejemplo, el código resaltado en `Startup.ConfigureServices` establece la marca de compatibilidad 2.1:

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

Para obtener más información, vea <xref:mvc/compatibility-version>.

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

En ASP.NET Core 2.2 o versiones posteriores, el atributo `[ApiController]` se puede aplicar a un ensamblado. En este contexto, la anotación aplica el comportamiento de API web a todos los controladores del ensamblado. Tenga en cuenta que no hay ninguna manera de excluir controladores específicos. Le recomendamos que aplique atributos de nivel de ensamblado a la clase `Startup`:

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ApiControllerAttributeOnAssembly&highlight=1)]

Para usar este atributo a nivel de ensamblado, se requiere una versión de compatibilidad de 2.2 o posterior, establecida mediante <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

El atributo `[ApiController]` se suele emparejar con `ControllerBase` para habilitar el comportamiento específico de REST para los controladores. `ControllerBase` proporciona acceso a métodos como <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> y <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.

Otro enfoque consiste en crear una clase de controlador base personalizada anotada con el atributo `[ApiController]`:

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

En las secciones siguientes se describen las ventajas de las características que aporta el atributo.

### <a name="automatic-http-400-responses"></a>Respuestas HTTP 400 automáticas

Los errores de validación de modelo desencadenan automáticamente una respuesta HTTP 400. En consecuencia, el código siguiente deja de ser necesario en las acciones:

[!code-csharp[](define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

Use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> para personalizar la salida de la respuesta resultante.

Deshabilitar el comportamiento predeterminado es una opción que resulta útil cuando una acción se puede recuperar de un error de validación de modelos. El comportamiento predeterminado se deshabilita al establecer la propiedad <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> en `true`. Agregue el código siguiente en `Startup.ConfigureServices` tras `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=7)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Con una marca de compatibilidad de 2.2 o posterior, el tipo de respuesta predeterminada para las respuestas HTTP 400 es <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>. El tipo `ValidationProblemDetails` cumple los requisitos de la [especificación RFC 7807](https://tools.ietf.org/html/rfc7807). Configure la propiedad `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` como `true` para que devuelva el formato de error de ASP.NET Core 2.1 de <xref:Microsoft.AspNetCore.Mvc.SerializableError> en su lugar. Agregue el siguiente código en `Startup.ConfigureServices`:

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2)
    .ConfigureApiBehaviorOptions(options =>
    {
        options
          .SuppressUseValidationProblemDetailsForInvalidModelStateResponses = true;
    });
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="binding-source-parameter-inference"></a>Inferencia de parámetro de origen de enlace

Un atributo de origen de enlace define la ubicación del valor del parámetro de una acción. Existen los atributos de origen de enlace siguientes:

|Atributo|Origen de enlace |
|---------|---------|
|**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**     | Cuerpo de la solicitud |
|**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**     | Datos del formulario en el cuerpo de la solicitud |
|**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)** | Encabezado de la solicitud |
|**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**   | Parámetro de la cadena de consulta de la solicitud |
|**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**   | Datos de ruta de la solicitud actual |
|**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)** | Servicio de solicitud insertado como parámetro de acción |

> [!WARNING]
> No use `[FromRoute]` si los valores pueden contener `%2f` (es decir, `/`). `%2f` no incluirá el carácter sin escape `/`. Use `[FromQuery]` si el valor puede contener `%2f`.

Sin el `[ApiController]` atributo, los atributos de origen de enlace se definen explícitamente. Sin `[ApiController]` u otros atributos de origen de enlace, como `[FromQuery]`, el entorno de tiempo de ejecución de ASP.NET Core intenta usar el enlazador de modelos de objeto complejo. El enlazador de modelos de objeto complejo extrae los datos de los proveedores de valor (que tienen un orden definido). Por ejemplo, siempre se opta por recibir "body model binder".

En el ejemplo siguiente, el atributo `[FromQuery]` indica que el valor del parámetro `discontinuedOnly` se proporciona en la cadena de consulta de la dirección URL de la solicitud:

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

Las reglas de inferencia se aplican para los orígenes de datos predeterminados de los parámetros de acción. Estas reglas configuran los orígenes de enlace que aplicaría manualmente a los parámetros de acción. Los atributos de origen de enlace presentan este comportamiento:

* **[FromBody]** se infiere para los parámetros de tipo complejo. La excepción a esta regla es cualquier tipo integrado complejo que tenga un significado especial, como <xref:Microsoft.AspNetCore.Http.IFormCollection> y <xref:System.Threading.CancellationToken>. El código de inferencia del origen de enlace omite esos tipos especiales. En el caso de los tipos simples, como `string` y `int`, `[FromBody]` no se infiere. Así pues, para los tipos simples, en los casos en los que quiera utilizar dicha funcionalidad, conviene usar el atributo `[FromBody]`. Cuando una acción contiene más de un parámetro que se especifica explícitamente (a través de `[FromBody]`) o se infiere como enlazado desde el cuerpo de la solicitud, se produce una excepción. Por ejemplo, las firmas de acción siguientes provocan una excepción:

    [!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

    > [!NOTE]
    > En ASP.NET Core 2.1, los parámetros de tipo de colección, como listas y matrices, se deducen incorrectamente como [[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute). [[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) debe usarse para estos parámetros si van a enlazarse desde el cuerpo de solicitud. Este comportamiento se ha corregido en ASP.NET Core 2.2 o posterior, donde se deducen los parámetros de tipo de colección que se enlazan desde el cuerpo de forma predeterminada.

* **[FromForm]** se infiere para los parámetros de acción de tipo <xref:Microsoft.AspNetCore.Http.IFormFile> y <xref:Microsoft.AspNetCore.Http.IFormFileCollection>. No se infiere para los tipos simples o definidos por el usuario.
* **[FromRoute]** se infiere para cualquier nombre de parámetro de acción que coincida con un parámetro de la plantilla de ruta. Si varias rutas coinciden con un parámetro de acción, cualquier valor de ruta se considera `[FromRoute]`.
* **[FromQuery]** se infiere para cualquier otro parámetro de acción.

Las reglas de inferencia predeterminadas se deshabilitan al establecer la propiedad <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> en `true`. Agregue el código siguiente en `Startup.ConfigureServices` tras `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=6)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="multipartform-data-request-inference"></a>Inferencia de solicitud de varios elementos o datos de formulario

Si un parámetro de acción se anota con el atributo [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute), se infiere el tipo de contenido de la solicitud `multipart/form-data`.

El comportamiento predeterminado se deshabilita al establecer la propiedad <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> en `true`.

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Agregue el siguiente código en `Startup.ConfigureServices`:

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

Agregue el código siguiente en `Startup.ConfigureServices` tras `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="attribute-routing-requirement"></a>Requisito de enrutamiento mediante atributos

El enrutamiento mediante atributos pasa a ser un requisito. Por ejemplo:

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

Las acciones dejan de estar disponibles a través de las [rutas convencionales](xref:mvc/controllers/routing#conventional-routing) definidas en <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> o mediante <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> en `Startup.Configure`.

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="problem-details-responses-for-error-status-codes"></a>Respuestas de los detalles de problemas para los códigos de estado de error

En ASP.NET Core 2.2 o versiones posteriores, MVC transforma un resultado de error (un resultado con el código de estado 400 o superior) en un resultado con <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>. `ProblemDetails` es:

* Un tipo basado en la [especificación RFC 7807](https://tools.ietf.org/html/rfc7807).
* Un formato estandarizado usado para especificar detalles de error de lectura mecánica en una respuesta HTTP.

Observe el código siguiente en una acción de controlador:

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Controllers/ProductsController.cs?name=snippet_ProblemDetailsStatusCode)]

La respuesta HTTP para `NotFound` tiene un código de estado 404 con un cuerpo `ProblemDetails`. Por ejemplo:

```json
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

La característica de los detalles del problema requiere una marca de compatibilidad 2.2 o posterior. El comportamiento predeterminado se deshabilita al establecer la propiedad `SuppressMapClientErrors` en `true`. Agregue el siguiente código en `Startup.ConfigureServices`:

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8)]

Use la propiedad `ClientErrorMapping` para configurar el contenido de la respuesta `ProblemDetails`. Por ejemplo, el código siguiente permite actualizar la propiedad `type` para respuestas 404:

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=10-11)]

::: moniker-end

## <a name="additional-resources"></a>Recursos adicionales

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
****
