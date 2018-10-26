---
title: Compilación de API web con ASP.NET Core
author: scottaddie
description: Obtenga información sobre las características disponibles para la compilación de una API web en ASP.NET Core y los casos en los que se recomienda usar cada una.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/15/2018
uid: web-api/index
ms.openlocfilehash: 763b95fb8ed3806bc67b7ad199153ea1027efa57
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090425"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="fc5a1-103">Compilación de API web con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fc5a1-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="fc5a1-104">Por [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="fc5a1-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="fc5a1-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fc5a1-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="fc5a1-106">En este documento se explica cómo crear una API web en ASP.NET Core y los casos en los que se recomienda usar cada una.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="fc5a1-107">Derivación de una clase desde ControllerBase</span><span class="sxs-lookup"><span data-stu-id="fc5a1-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="fc5a1-108">Herede desde la clase <xref:Microsoft.AspNetCore.Mvc.ControllerBase> en un controlador diseñado para funcionar como API web.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-108">Inherit from the <xref:Microsoft.AspNetCore.Mvc.ControllerBase> class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="fc5a1-109">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="fc5a1-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="fc5a1-110">La clase `ControllerBase` proporciona acceso a varios métodos y propiedades.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="fc5a1-111">En el código anterior, algunos ejemplos son <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> y <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-111">In the preceding code, examples include <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span></span> <span data-ttu-id="fc5a1-112">Se llama a estos métodos en los métodos de acción para devolver los códigos de estado HTTP 400 y 201, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="fc5a1-113">La propiedad <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState>, que también se proporciona con `ControllerBase`, se usa para controlar la validación del modelo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-113">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="fc5a1-114">Anotación de una clase con ApiControllerAttribute</span><span class="sxs-lookup"><span data-stu-id="fc5a1-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="fc5a1-115">ASP.NET Core 2.1 incorpora el atributo [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) para designar una clase de controlador de API web.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-115">ASP.NET Core 2.1 introduces the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="fc5a1-116">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="fc5a1-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="fc5a1-117">Para usar este atributo se requiere una versión de compatibilidad de 2.1 o posterior, establecida mediante <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-117">A compatibility version of 2.1 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute.</span></span> <span data-ttu-id="fc5a1-118">Por ejemplo, el código resaltado en *Startup.ConfigureServices* establece la marca de compatibilidad 2.2:</span><span class="sxs-lookup"><span data-stu-id="fc5a1-118">For example, the highlighted code in *Startup.ConfigureServices* sets the 2.2 compatibility flag:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="fc5a1-119">Para obtener más información, vea <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-119">For more information, see <xref:mvc/compatibility-version>.</span></span>

<span data-ttu-id="fc5a1-120">El atributo `[ApiController]` se suele emparejar con `ControllerBase` para habilitar el comportamiento específico de REST para los controladores.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-120">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="fc5a1-121">`ControllerBase` proporciona acceso a métodos como <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> y <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-121">`ControllerBase` provides access to methods such as <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span></span>

<span data-ttu-id="fc5a1-122">Otro enfoque consiste en crear una clase de controlador base personalizada anotada con el atributo `[ApiController]`:</span><span class="sxs-lookup"><span data-stu-id="fc5a1-122">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="fc5a1-123">En las secciones siguientes se describen las ventajas de las características que aporta el atributo.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-123">The following sections describe convenience features added by the attribute.</span></span>

### <a name="problem-details-responses-for-error-status-codes"></a><span data-ttu-id="fc5a1-124">Respuestas de los detalles de problemas para los códigos de estado de error</span><span class="sxs-lookup"><span data-stu-id="fc5a1-124">Problem details responses for error status codes</span></span>

<span data-ttu-id="fc5a1-125">ASP.NET Core 2.1 y las versiones posteriores incluyen [ProblemDetails](xref:Microsoft.AspNetCore.Mvc.ProblemDetails), un tipo basado en la [especificación RFC 7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="fc5a1-125">ASP.NET Core 2.1 and later includes [ProblemDetails](xref:Microsoft.AspNetCore.Mvc.ProblemDetails), a type based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span> <span data-ttu-id="fc5a1-126">El tipo `ProblemDetails` proporciona un formato estandarizado para tratar los detalles de los errores legibles por una máquina en una respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-126">The `ProblemDetails` type provides a standardized format for conveying machine readable details of errors in a HTTP response.</span></span>

<span data-ttu-id="fc5a1-127">En ASP.NET Core 2.2 y las versiones posteriores, MVC transforma los resultados del código de estado de error (código de estado 400 y superiores) en un resultado con `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-127">In ASP.NET Core 2.2 and later, MVC transforms error status code results (status code 400 and higher) to a result with `ProblemDetails`.</span></span> <span data-ttu-id="fc5a1-128">Observe el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="fc5a1-128">Consider the following code:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_ProblemDetails_StatusCode&highlight=4)]

<span data-ttu-id="fc5a1-129">La respuesta HTTP del resultado `NotFound` tiene un código de estado 404 con un cuerpo `ProblemDetails` similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="fc5a1-129">The HTTP response for the `NotFound` result has a 404 status code with a `ProblemDetails` body similar to the following:</span></span>

```js
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

<span data-ttu-id="fc5a1-130">La característica de los detalles del problema requiere una marca de compatibilidad 2.2 o posterior.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-130">The problem details feature requires a compatibility flag of 2.2 or later.</span></span> <span data-ttu-id="fc5a1-131">El comportamiento predeterminado se deshabilita cuando la propiedad [SuppressMapClientErrors](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors> --> se establece en `true`.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-131">The default behavior is disabled when the [SuppressMapClientErrors](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors> --> property is set to `true`.</span></span> <span data-ttu-id="fc5a1-132">El siguiente código de resaltado `Startup.ConfigureServices` deshabilita los detalles del problema:</span><span class="sxs-lookup"><span data-stu-id="fc5a1-132">The following highlighted code from `Startup.ConfigureServices` disables problem details:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=8)]

<span data-ttu-id="fc5a1-133">Use la propiedad [ClientErrorMapping](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping> --> para configurar el contenido de la respuesta `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-133">Use the [ClientErrorMapping](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping> --> property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="fc5a1-134">Por ejemplo, el código siguiente permite actualizar la propiedad `type` para respuestas 404:</span><span class="sxs-lookup"><span data-stu-id="fc5a1-134">For example, the following code updates the `type` property for 404 responses:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=10)]

### <a name="automatic-http-400-responses"></a><span data-ttu-id="fc5a1-135">Respuestas HTTP 400 automáticas</span><span class="sxs-lookup"><span data-stu-id="fc5a1-135">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="fc5a1-136">Los errores de validación desencadenan automáticamente una respuesta HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-136">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="fc5a1-137">El código siguiente deja de ser necesario en las acciones:</span><span class="sxs-lookup"><span data-stu-id="fc5a1-137">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

<span data-ttu-id="fc5a1-138">Use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> para personalizar la salida de la respuesta resultante.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-138">Use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> to customize the output of the the resulting response.</span></span>

<span data-ttu-id="fc5a1-139">El comportamiento predeterminado se deshabilita al establecer la propiedad <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> en `true`.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-139">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property is set to `true`.</span></span> <span data-ttu-id="fc5a1-140">Agregue el siguiente código en *Startup.ConfigureServices* después de `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="fc5a1-140">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

<span data-ttu-id="fc5a1-141">Con una marca de compatibilidad de 2.2 o posterior, el tipo de respuesta predeterminada que se devuelve para las respuestas 400 es un elemento <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-141">With a compatibility flag of 2.2 or later, the default response type returned for 400 responses is a <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="fc5a1-142">Use la propiedad [SuppressUseValidationProblemDetailsForInvalidModelStateResponses](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressUseValidationProblemDetailsForInvalidModelStateResponses> --> para emplear el formato de error de ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-142">Use the Use the [SuppressUseValidationProblemDetailsForInvalidModelStateResponses](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressUseValidationProblemDetailsForInvalidModelStateResponses> --> property to use the ASP.NET Core 2.1 error format.</span></span>

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="fc5a1-143">Inferencia de parámetro de origen de enlace</span><span class="sxs-lookup"><span data-stu-id="fc5a1-143">Binding source parameter inference</span></span>

<span data-ttu-id="fc5a1-144">Un atributo de origen de enlace define la ubicación del valor del parámetro de una acción.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-144">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="fc5a1-145">Existen los atributos de origen de enlace siguientes:</span><span class="sxs-lookup"><span data-stu-id="fc5a1-145">The following binding source attributes exist:</span></span>

|<span data-ttu-id="fc5a1-146">Atributo</span><span class="sxs-lookup"><span data-stu-id="fc5a1-146">Attribute</span></span>|<span data-ttu-id="fc5a1-147">Origen de enlace</span><span class="sxs-lookup"><span data-stu-id="fc5a1-147">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="fc5a1-148">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span><span class="sxs-lookup"><span data-stu-id="fc5a1-148">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span></span>     | <span data-ttu-id="fc5a1-149">Cuerpo de la solicitud</span><span class="sxs-lookup"><span data-stu-id="fc5a1-149">Request body</span></span> |
|<span data-ttu-id="fc5a1-150">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span><span class="sxs-lookup"><span data-stu-id="fc5a1-150">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span></span>     | <span data-ttu-id="fc5a1-151">Datos del formulario en el cuerpo de la solicitud</span><span class="sxs-lookup"><span data-stu-id="fc5a1-151">Form data in the request body</span></span> |
|<span data-ttu-id="fc5a1-152">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span><span class="sxs-lookup"><span data-stu-id="fc5a1-152">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span></span> | <span data-ttu-id="fc5a1-153">Encabezado de la solicitud</span><span class="sxs-lookup"><span data-stu-id="fc5a1-153">Request header</span></span> |
|<span data-ttu-id="fc5a1-154">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span><span class="sxs-lookup"><span data-stu-id="fc5a1-154">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span></span>   | <span data-ttu-id="fc5a1-155">Parámetro de la cadena de consulta de la solicitud</span><span class="sxs-lookup"><span data-stu-id="fc5a1-155">Request query string parameter</span></span> |
|<span data-ttu-id="fc5a1-156">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span><span class="sxs-lookup"><span data-stu-id="fc5a1-156">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span></span>   | <span data-ttu-id="fc5a1-157">Datos de ruta de la solicitud actual</span><span class="sxs-lookup"><span data-stu-id="fc5a1-157">Route data from the current request</span></span> |
|<span data-ttu-id="fc5a1-158">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="fc5a1-158">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="fc5a1-159">Servicio de solicitud insertado como parámetro de acción</span><span class="sxs-lookup"><span data-stu-id="fc5a1-159">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="fc5a1-160">No use `[FromRoute]` si los valores pueden contener `%2f` (es decir, `/`).</span><span class="sxs-lookup"><span data-stu-id="fc5a1-160">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="fc5a1-161">`%2f` no incluirá el carácter sin escape `/`.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-161">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="fc5a1-162">Use `[FromQuery]` si el valor puede contener `%2f`.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-162">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="fc5a1-163">Sin el `[ApiController]` atributo, los atributos de origen de enlace se definen explícitamente.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-163">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="fc5a1-164">En el ejemplo siguiente, el atributo `[FromQuery]` indica que el valor del parámetro `discontinuedOnly` se proporciona en la cadena de consulta de la dirección URL de la solicitud:</span><span class="sxs-lookup"><span data-stu-id="fc5a1-164">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="fc5a1-165">Las reglas de inferencia se aplican para los orígenes de datos predeterminados de los parámetros de acción.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-165">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="fc5a1-166">Estas reglas configuran los orígenes de enlace que aplicaría manualmente a los parámetros de acción.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-166">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="fc5a1-167">Los atributos de origen de enlace presentan este comportamiento:</span><span class="sxs-lookup"><span data-stu-id="fc5a1-167">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="fc5a1-168">**[FromBody]** se infiere para los parámetros de tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-168">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="fc5a1-169">La excepción a esta regla es cualquier tipo integrado complejo que tenga un significado especial, como <xref:Microsoft.AspNetCore.Http.IFormCollection> y <xref:System.Threading.CancellationToken>.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-169">An exception to this rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="fc5a1-170">El código de inferencia del origen de enlace omite esos tipos especiales.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-170">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="fc5a1-171">En el caso de los tipos simples, como `string` y `int`, `[FromBody]` no se infiere.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-171">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="fc5a1-172">Así pues, para los tipos simples, en los casos en los que quiera utilizar dicha funcionalidad, conviene usar el atributo `[FromBody]`.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-172">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is desired.</span></span> <span data-ttu-id="fc5a1-173">Cuando una acción contiene más de un parámetro que se especifica explícitamente (a través de `[FromBody]`) o se infiere como enlazado desde el cuerpo de la solicitud, se produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-173">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="fc5a1-174">Por ejemplo, las firmas de acción siguientes provocan una excepción:</span><span class="sxs-lookup"><span data-stu-id="fc5a1-174">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="fc5a1-175">**[FromForm]** se infiere para los parámetros de acción de tipo <xref:Microsoft.AspNetCore.Http.IFormFile> y <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-175">**[FromForm]** is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="fc5a1-176">No se infiere para los tipos simples o definidos por el usuario.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-176">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="fc5a1-177">**[FromRoute]** se infiere para cualquier nombre de parámetro de acción que coincida con un parámetro de la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-177">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="fc5a1-178">Si varias rutas coinciden con un parámetro de acción, cualquier valor de ruta se considera `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-178">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="fc5a1-179">**[FromQuery]** se infiere para cualquier otro parámetro de acción.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-179">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="fc5a1-180">Las reglas de inferencia predeterminadas se deshabilitan al establecer la propiedad <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> en `true`.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-180">The default inference rules are disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> property is set to `true`.</span></span> <span data-ttu-id="fc5a1-181">Agregue el siguiente código en *Startup.ConfigureServices* después de `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="fc5a1-181">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="fc5a1-182">Inferencia de solicitud de varios elementos o datos de formulario</span><span class="sxs-lookup"><span data-stu-id="fc5a1-182">Multipart/form-data request inference</span></span>

<span data-ttu-id="fc5a1-183">Si un parámetro de acción se anota con el atributo [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute), se infiere el tipo de contenido de la solicitud `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-183">When an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="fc5a1-184">El comportamiento predeterminado se deshabilita al establecer la propiedad <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> en `true`.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-184">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property is set to `true`.</span></span> <span data-ttu-id="fc5a1-185">Agregue el siguiente código en *Startup.ConfigureServices* después de `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="fc5a1-185">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="fc5a1-186">Requisito de enrutamiento mediante atributos</span><span class="sxs-lookup"><span data-stu-id="fc5a1-186">Attribute routing requirement</span></span>

<span data-ttu-id="fc5a1-187">El enrutamiento mediante atributos pasa a ser un requisito.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-187">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="fc5a1-188">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="fc5a1-188">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="fc5a1-189">Las acciones dejan de estar disponibles a través de las [rutas convencionales](xref:mvc/controllers/routing#conventional-routing) definidas en <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> o mediante <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> en *Startup.Configure*.</span><span class="sxs-lookup"><span data-stu-id="fc5a1-189">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in *Startup.Configure*.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="fc5a1-190">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="fc5a1-190">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>