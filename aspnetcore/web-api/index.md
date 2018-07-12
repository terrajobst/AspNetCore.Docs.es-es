---
title: Compilación de API web con ASP.NET Core
author: scottaddie
description: Obtenga información sobre las características disponibles para la compilación de una API web en ASP.NET Core y los casos en los que se recomienda usar cada una.
ms.author: scaddie
ms.custom: mvc
ms.date: 07/06/2018
uid: web-api/index
ms.openlocfilehash: 0ff0bbc629930666d46247d6c1257fac8bfaf7c2
ms.sourcegitcommit: 260abb706ed17f07a53288d8a0c3e69fc13e7468
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2018
ms.locfileid: "38966814"
---
# <a name="build-web-apis-with-aspnet-core"></a>Compilación de API web con ASP.NET Core

Por [Scott Addie](https://github.com/scottaddie)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

En este documento se explica cómo crear una API web en ASP.NET Core y los casos en los que se recomienda usar cada una.

## <a name="derive-class-from-controllerbase"></a>Derivación de una clase desde ControllerBase

Herede desde la clase [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) en un controlador diseñado para funcionar como API web. Por ejemplo:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

La clase `ControllerBase` proporciona acceso a varios métodos y propiedades. En el código anterior, algunos ejemplos incluyen [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) y [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction). Se llama a estos métodos en los métodos de acción para devolver los códigos de estado HTTP 400 y 201, respectivamente. La propiedad [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate), que también se proporciona con `ControllerBase`, se usa para controlar la validación del modelo de solicitud.

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a>Anotación de una clase con ApiControllerAttribute

ASP.NET Core 2.1 incorpora el atributo [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) para designar una clase de controlador de API web. Por ejemplo:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

Para usar este atributo, se requiere una versión de compatibilidad de 2.1 o posterior, establecida mediante [SetCompatibilityVersion](/dotnet/api/microsoft.extensions.dependencyinjection.mvccoremvcbuilderextensions.setcompatibilityversion). Por ejemplo, el código resaltado en *Startup.ConfigureServices* establece la marca de compatibilidad 2.1:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

El atributo `[ApiController]` se suele emparejar con `ControllerBase` para habilitar el comportamiento específico de REST para los controladores. `ControllerBase` proporciona acceso a métodos como [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) y [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).

Otro enfoque consiste en crear una clase de controlador base personalizada anotada con el atributo `[ApiController]`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

En las secciones siguientes se describen las ventajas de las características que aporta el atributo.

### <a name="automatic-http-400-responses"></a>Respuestas HTTP 400 automáticas

Los errores de validación desencadenan automáticamente una respuesta HTTP 400. El código siguiente deja de ser necesario en las acciones:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]

El comportamiento predeterminado se deshabilita cuando la propiedad [SuppressModelStateInvalidFilter](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressmodelstateinvalidfilter) se establece en `true`. Agregue el siguiente código en *Startup.ConfigureServices* después de `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a>Inferencia de parámetro de origen de enlace

Un atributo de origen de enlace define la ubicación del valor del parámetro de una acción. Existen los atributos de origen de enlace siguientes:

|Atributo|Origen de enlace |
|---------|---------|
|**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**     | Cuerpo de la solicitud |
|**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**     | Datos del formulario en el cuerpo de la solicitud |
|**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)** | Encabezado de la solicitud |
|**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**   | Parámetro de la cadena de consulta de la solicitud |
|**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**   | Datos de ruta de la solicitud actual |
|**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)** | Servicio de solicitud insertado como parámetro de acción |

> [!WARNING]
> No use `[FromRoute]` si los valores pueden contener `%2f` (es decir, `/`). `%2f` no incluirá el carácter sin escape `/`. Use `[FromQuery]` si el valor puede contener `%2f`.

Sin el `[ApiController]` atributo, los atributos de origen de enlace se definen explícitamente. En el ejemplo siguiente, el atributo `[FromQuery]` indica que el valor del parámetro `discontinuedOnly` se proporciona en la cadena de consulta de la dirección URL de la solicitud:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

Las reglas de inferencia se aplican para los orígenes de datos predeterminados de los parámetros de acción. Estas reglas configuran los orígenes de enlace que aplicaría manualmente a los parámetros de acción. Los atributos de origen de enlace presentan este comportamiento:

* **[FromBody]** se infiere para los parámetros de tipo complejo. La excepción a esta regla es cualquier tipo integrado complejo que tenga un significado especial, como [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) o [CancellationToken](/dotnet/api/system.threading.cancellationtoken). El código de inferencia del origen de enlace omite esos tipos especiales. Cuando una acción contiene más de un parámetro que se especifica explícitamente (a través de `[FromBody]`) o se infiere como enlazado desde el cuerpo de la solicitud, se produce una excepción. Por ejemplo, las firmas de acción siguientes provocan una excepción:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* **[FromForm]** se infiere para los parámetros de acción de tipo [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) o [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection). No se infiere para los tipos simples o definidos por el usuario.
* **[FromRoute]** se infiere para cualquier nombre de parámetro de acción que coincida con un parámetro de la plantilla de ruta. Si varias rutas coinciden con un parámetro de acción, cualquier valor de ruta se considera `[FromRoute]`.
* **[FromQuery]** se infiere para cualquier otro parámetro de acción.

Las reglas de inferencia predeterminadas se deshabilitan cuando la propiedad [SuppressInferBindingSourcesForParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressinferbindingsourcesforparameters) se establece en `true`. Agregue el siguiente código en *Startup.ConfigureServices* después de `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a>Inferencia de solicitud de varios elementos o datos de formulario

Si un parámetro de acción se anota con el atributo [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute), se infiere el tipo de contenido de la solicitud `multipart/form-data`.

El comportamiento predeterminado se deshabilita cuando la propiedad [SuppressConsumesConstraintForFormFileParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressconsumesconstraintforformfileparameters) se establece en `true`. Agregue el siguiente código en *Startup.ConfigureServices* después de `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a>Requisito de enrutamiento mediante atributos

El enrutamiento mediante atributos pasa a ser un requisito. Por ejemplo:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

Las acciones dejan de estar disponibles a través de las [rutas convencionales](xref:mvc/controllers/routing#conventional-routing) definidas en [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) o mediante [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) en *Startup.Configure*.

::: moniker-end

## <a name="additional-resources"></a>Recursos adicionales

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
