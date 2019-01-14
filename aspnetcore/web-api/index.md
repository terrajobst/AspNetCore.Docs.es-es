---
title: Compilación de API web con ASP.NET Core
author: scottaddie
description: Obtenga información sobre las características disponibles para la compilación de una API web en ASP.NET Core y los casos en los que se recomienda usar cada una.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/11/2019
uid: web-api/index
ms.openlocfilehash: a826bdecdd3a25eb23597123166695c169ba4229
ms.sourcegitcommit: ec71fd5a988f927ae301813aae5ff764feb3bb6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2019
ms.locfileid: "54249443"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="b7d93-103">Compilación de API web con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b7d93-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="b7d93-104">Por [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="b7d93-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="b7d93-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b7d93-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="b7d93-106">En este documento se explica cómo crear una API web en ASP.NET Core y los casos en los que se recomienda usar cada una.</span><span class="sxs-lookup"><span data-stu-id="b7d93-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="b7d93-107">Derivación de una clase desde ControllerBase</span><span class="sxs-lookup"><span data-stu-id="b7d93-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="b7d93-108">Herede desde la clase <xref:Microsoft.AspNetCore.Mvc.ControllerBase> en un controlador diseñado para funcionar como API web.</span><span class="sxs-lookup"><span data-stu-id="b7d93-108">Inherit from the <xref:Microsoft.AspNetCore.Mvc.ControllerBase> class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="b7d93-109">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="b7d93-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="b7d93-110">La clase `ControllerBase` proporciona acceso a varios métodos y propiedades.</span><span class="sxs-lookup"><span data-stu-id="b7d93-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="b7d93-111">En el código anterior, algunos ejemplos son <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> y <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span><span class="sxs-lookup"><span data-stu-id="b7d93-111">In the preceding code, examples include <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span></span> <span data-ttu-id="b7d93-112">Se llama a estos métodos en los métodos de acción para devolver los códigos de estado HTTP 400 y 201, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="b7d93-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="b7d93-113">La propiedad <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState>, que también se proporciona con `ControllerBase`, se usa para controlar la validación del modelo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="b7d93-113">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotation-with-apicontroller-attribute"></a><span data-ttu-id="b7d93-114">Anotación con el atributo ApiController</span><span class="sxs-lookup"><span data-stu-id="b7d93-114">Annotation with ApiController attribute</span></span>

<span data-ttu-id="b7d93-115">ASP.NET Core 2.1 incorpora el atributo [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) para designar una clase de controlador de API web.</span><span class="sxs-lookup"><span data-stu-id="b7d93-115">ASP.NET Core 2.1 introduces the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="b7d93-116">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="b7d93-116">For example:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="b7d93-117">Para usar este atributo a nivel de controlador, se requiere una versión de compatibilidad de 2.1 o posterior, establecida mediante <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>.</span><span class="sxs-lookup"><span data-stu-id="b7d93-117">A compatibility version of 2.1 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute at the controller level.</span></span> <span data-ttu-id="b7d93-118">Por ejemplo, el código resaltado en `Startup.ConfigureServices` establece la marca de compatibilidad 2.1:</span><span class="sxs-lookup"><span data-stu-id="b7d93-118">For example, the highlighted code in `Startup.ConfigureServices` sets the 2.1 compatibility flag:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="b7d93-119">Para obtener más información, vea <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="b7d93-119">For more information, see <xref:mvc/compatibility-version>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b7d93-120">En ASP.NET Core 2.2 o versiones posteriores, el atributo `[ApiController]` se puede aplicar a un ensamblado.</span><span class="sxs-lookup"><span data-stu-id="b7d93-120">In ASP.NET Core 2.2 or later, the `[ApiController]` attribute can be applied to an assembly.</span></span> <span data-ttu-id="b7d93-121">En este contexto, la anotación aplica el comportamiento de API web a todos los controladores del ensamblado.</span><span class="sxs-lookup"><span data-stu-id="b7d93-121">Annotation in this manner applies web API behavior to all controllers in the assembly.</span></span> <span data-ttu-id="b7d93-122">Tenga en cuenta que no hay ninguna manera de excluir controladores específicos.</span><span class="sxs-lookup"><span data-stu-id="b7d93-122">Beware that there's no way to opt out for individual controllers.</span></span> <span data-ttu-id="b7d93-123">Le recomendamos que aplique atributos de nivel de ensamblado a la clase `Startup`:</span><span class="sxs-lookup"><span data-stu-id="b7d93-123">As a recommendation, assembly-level attributes should be applied to the `Startup` class:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ApiControllerAttributeOnAssembly&highlight=1)]

<span data-ttu-id="b7d93-124">Para usar este atributo a nivel de ensamblado, se requiere una versión de compatibilidad de 2.2 o posterior, establecida mediante <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>.</span><span class="sxs-lookup"><span data-stu-id="b7d93-124">A compatibility version of 2.2 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute at the assembly level.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="b7d93-125">El atributo `[ApiController]` se suele emparejar con `ControllerBase` para habilitar el comportamiento específico de REST para los controladores.</span><span class="sxs-lookup"><span data-stu-id="b7d93-125">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="b7d93-126">`ControllerBase` proporciona acceso a métodos como <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> y <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span><span class="sxs-lookup"><span data-stu-id="b7d93-126">`ControllerBase` provides access to methods such as <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span></span>

<span data-ttu-id="b7d93-127">Otro enfoque consiste en crear una clase de controlador base personalizada anotada con el atributo `[ApiController]`:</span><span class="sxs-lookup"><span data-stu-id="b7d93-127">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="b7d93-128">En las secciones siguientes se describen las ventajas de las características que aporta el atributo.</span><span class="sxs-lookup"><span data-stu-id="b7d93-128">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="b7d93-129">Respuestas HTTP 400 automáticas</span><span class="sxs-lookup"><span data-stu-id="b7d93-129">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="b7d93-130">Los errores de validación de modelo desencadenan automáticamente una respuesta HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="b7d93-130">Model validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="b7d93-131">En consecuencia, el código siguiente deja de ser necesario en las acciones:</span><span class="sxs-lookup"><span data-stu-id="b7d93-131">Consequently, the following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

<span data-ttu-id="b7d93-132">Use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> para personalizar la salida de la respuesta resultante.</span><span class="sxs-lookup"><span data-stu-id="b7d93-132">Use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> to customize the output of the resulting response.</span></span>

<span data-ttu-id="b7d93-133">Deshabilitar el comportamiento predeterminado es una opción que resulta útil cuando una acción se puede recuperar de un error de validación de modelos.</span><span class="sxs-lookup"><span data-stu-id="b7d93-133">Disabling the default behavior is useful when your action can recover from a model validation error.</span></span> <span data-ttu-id="b7d93-134">El comportamiento predeterminado se deshabilita al establecer la propiedad <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> en `true`.</span><span class="sxs-lookup"><span data-stu-id="b7d93-134">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property is set to `true`.</span></span> <span data-ttu-id="b7d93-135">Agregue el código siguiente en `Startup.ConfigureServices` tras `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span><span class="sxs-lookup"><span data-stu-id="b7d93-135">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=7)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b7d93-136">Con una marca de compatibilidad de 2.2 o posterior, el tipo de respuesta predeterminada para las respuestas HTTP 400 es <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="b7d93-136">With a compatibility flag of 2.2 or later, the default response type for HTTP 400 responses is <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="b7d93-137">El tipo `ValidationProblemDetails` cumple los requisitos de la [especificación RFC 7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="b7d93-137">The `ValidationProblemDetails` type complies with the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span> <span data-ttu-id="b7d93-138">Configure la propiedad `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` como `true` para que devuelva el formato de error de ASP.NET Core 2.1 de <xref:Microsoft.AspNetCore.Mvc.SerializableError> en su lugar.</span><span class="sxs-lookup"><span data-stu-id="b7d93-138">Set the `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` property to `true` to instead return the ASP.NET Core 2.1 error format of <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span></span> <span data-ttu-id="b7d93-139">Agregue el siguiente código en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b7d93-139">Add the following code in `Startup.ConfigureServices`:</span></span>

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

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="b7d93-140">Inferencia de parámetro de origen de enlace</span><span class="sxs-lookup"><span data-stu-id="b7d93-140">Binding source parameter inference</span></span>

<span data-ttu-id="b7d93-141">Un atributo de origen de enlace define la ubicación del valor del parámetro de una acción.</span><span class="sxs-lookup"><span data-stu-id="b7d93-141">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="b7d93-142">Existen los atributos de origen de enlace siguientes:</span><span class="sxs-lookup"><span data-stu-id="b7d93-142">The following binding source attributes exist:</span></span>

|<span data-ttu-id="b7d93-143">Atributo</span><span class="sxs-lookup"><span data-stu-id="b7d93-143">Attribute</span></span>|<span data-ttu-id="b7d93-144">Origen de enlace</span><span class="sxs-lookup"><span data-stu-id="b7d93-144">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="b7d93-145">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span><span class="sxs-lookup"><span data-stu-id="b7d93-145">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span></span>     | <span data-ttu-id="b7d93-146">Cuerpo de la solicitud</span><span class="sxs-lookup"><span data-stu-id="b7d93-146">Request body</span></span> |
|<span data-ttu-id="b7d93-147">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span><span class="sxs-lookup"><span data-stu-id="b7d93-147">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span></span>     | <span data-ttu-id="b7d93-148">Datos del formulario en el cuerpo de la solicitud</span><span class="sxs-lookup"><span data-stu-id="b7d93-148">Form data in the request body</span></span> |
|<span data-ttu-id="b7d93-149">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span><span class="sxs-lookup"><span data-stu-id="b7d93-149">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span></span> | <span data-ttu-id="b7d93-150">Encabezado de la solicitud</span><span class="sxs-lookup"><span data-stu-id="b7d93-150">Request header</span></span> |
|<span data-ttu-id="b7d93-151">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span><span class="sxs-lookup"><span data-stu-id="b7d93-151">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span></span>   | <span data-ttu-id="b7d93-152">Parámetro de la cadena de consulta de la solicitud</span><span class="sxs-lookup"><span data-stu-id="b7d93-152">Request query string parameter</span></span> |
|<span data-ttu-id="b7d93-153">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span><span class="sxs-lookup"><span data-stu-id="b7d93-153">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span></span>   | <span data-ttu-id="b7d93-154">Datos de ruta de la solicitud actual</span><span class="sxs-lookup"><span data-stu-id="b7d93-154">Route data from the current request</span></span> |
|<span data-ttu-id="b7d93-155">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="b7d93-155">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="b7d93-156">Servicio de solicitud insertado como parámetro de acción</span><span class="sxs-lookup"><span data-stu-id="b7d93-156">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="b7d93-157">No use `[FromRoute]` si los valores pueden contener `%2f` (es decir, `/`).</span><span class="sxs-lookup"><span data-stu-id="b7d93-157">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="b7d93-158">`%2f` no incluirá el carácter sin escape `/`.</span><span class="sxs-lookup"><span data-stu-id="b7d93-158">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="b7d93-159">Use `[FromQuery]` si el valor puede contener `%2f`.</span><span class="sxs-lookup"><span data-stu-id="b7d93-159">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="b7d93-160">Sin el `[ApiController]` atributo, los atributos de origen de enlace se definen explícitamente.</span><span class="sxs-lookup"><span data-stu-id="b7d93-160">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="b7d93-161">En el ejemplo siguiente, el atributo `[FromQuery]` indica que el valor del parámetro `discontinuedOnly` se proporciona en la cadena de consulta de la dirección URL de la solicitud:</span><span class="sxs-lookup"><span data-stu-id="b7d93-161">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="b7d93-162">Las reglas de inferencia se aplican para los orígenes de datos predeterminados de los parámetros de acción.</span><span class="sxs-lookup"><span data-stu-id="b7d93-162">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="b7d93-163">Estas reglas configuran los orígenes de enlace que aplicaría manualmente a los parámetros de acción.</span><span class="sxs-lookup"><span data-stu-id="b7d93-163">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="b7d93-164">Los atributos de origen de enlace presentan este comportamiento:</span><span class="sxs-lookup"><span data-stu-id="b7d93-164">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="b7d93-165">**[FromBody]** se infiere para los parámetros de tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="b7d93-165">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="b7d93-166">La excepción a esta regla es cualquier tipo integrado complejo que tenga un significado especial, como <xref:Microsoft.AspNetCore.Http.IFormCollection> y <xref:System.Threading.CancellationToken>.</span><span class="sxs-lookup"><span data-stu-id="b7d93-166">An exception to this rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="b7d93-167">El código de inferencia del origen de enlace omite esos tipos especiales.</span><span class="sxs-lookup"><span data-stu-id="b7d93-167">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="b7d93-168">En el caso de los tipos simples, como `string` y `int`, `[FromBody]` no se infiere.</span><span class="sxs-lookup"><span data-stu-id="b7d93-168">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="b7d93-169">Así pues, para los tipos simples, en los casos en los que quiera utilizar dicha funcionalidad, conviene usar el atributo `[FromBody]`.</span><span class="sxs-lookup"><span data-stu-id="b7d93-169">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is needed.</span></span> <span data-ttu-id="b7d93-170">Cuando una acción contiene más de un parámetro que se especifica explícitamente (a través de `[FromBody]`) o se infiere como enlazado desde el cuerpo de la solicitud, se produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="b7d93-170">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="b7d93-171">Por ejemplo, las firmas de acción siguientes provocan una excepción:</span><span class="sxs-lookup"><span data-stu-id="b7d93-171">For example, the following action signatures cause an exception:</span></span>

    [!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

    > [!NOTE]
    > <span data-ttu-id="b7d93-172">En ASP.NET Core 2.1, los parámetros de tipo de colección, como listas y matrices, se deducen incorrectamente como [[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute).</span><span class="sxs-lookup"><span data-stu-id="b7d93-172">In ASP.NET Core 2.1, collection type parameters such as lists and arrays are incorrectly inferred as [[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute).</span></span> <span data-ttu-id="b7d93-173">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) debe usarse para estos parámetros si van a enlazarse desde el cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="b7d93-173">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) should be used for these parameters if they are to be bound from the request body.</span></span> <span data-ttu-id="b7d93-174">Este comportamiento se ha corregido en ASP.NET Core 2.2 o posterior, donde se deducen los parámetros de tipo de colección que se enlazan desde el cuerpo de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="b7d93-174">This behavior is fixed in ASP.NET Core 2.2 or later, where collection type parameters are inferred to be bound from the body by default.</span></span>

* <span data-ttu-id="b7d93-175">**[FromForm]** se infiere para los parámetros de acción de tipo <xref:Microsoft.AspNetCore.Http.IFormFile> y <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span><span class="sxs-lookup"><span data-stu-id="b7d93-175">**[FromForm]** is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="b7d93-176">No se infiere para los tipos simples o definidos por el usuario.</span><span class="sxs-lookup"><span data-stu-id="b7d93-176">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="b7d93-177">**[FromRoute]** se infiere para cualquier nombre de parámetro de acción que coincida con un parámetro de la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="b7d93-177">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="b7d93-178">Si varias rutas coinciden con un parámetro de acción, cualquier valor de ruta se considera `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="b7d93-178">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="b7d93-179">**[FromQuery]** se infiere para cualquier otro parámetro de acción.</span><span class="sxs-lookup"><span data-stu-id="b7d93-179">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="b7d93-180">Las reglas de inferencia predeterminadas se deshabilitan al establecer la propiedad <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> en `true`.</span><span class="sxs-lookup"><span data-stu-id="b7d93-180">The default inference rules are disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> property is set to `true`.</span></span> <span data-ttu-id="b7d93-181">Agregue el código siguiente en `Startup.ConfigureServices` tras `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span><span class="sxs-lookup"><span data-stu-id="b7d93-181">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=6)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="b7d93-182">Inferencia de solicitud de varios elementos o datos de formulario</span><span class="sxs-lookup"><span data-stu-id="b7d93-182">Multipart/form-data request inference</span></span>

<span data-ttu-id="b7d93-183">Si un parámetro de acción se anota con el atributo [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute), se infiere el tipo de contenido de la solicitud `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="b7d93-183">When an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="b7d93-184">El comportamiento predeterminado se deshabilita al establecer la propiedad <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> en `true`.</span><span class="sxs-lookup"><span data-stu-id="b7d93-184">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property is set to `true`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b7d93-185">Agregue el siguiente código en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b7d93-185">Add the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="b7d93-186">Agregue el código siguiente en `Startup.ConfigureServices` tras `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="b7d93-186">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="attribute-routing-requirement"></a><span data-ttu-id="b7d93-187">Requisito de enrutamiento mediante atributos</span><span class="sxs-lookup"><span data-stu-id="b7d93-187">Attribute routing requirement</span></span>

<span data-ttu-id="b7d93-188">El enrutamiento mediante atributos pasa a ser un requisito.</span><span class="sxs-lookup"><span data-stu-id="b7d93-188">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="b7d93-189">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="b7d93-189">For example:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="b7d93-190">Las acciones dejan de estar disponibles a través de las [rutas convencionales](xref:mvc/controllers/routing#conventional-routing) definidas en <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> o mediante <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="b7d93-190">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="problem-details-responses-for-error-status-codes"></a><span data-ttu-id="b7d93-191">Respuestas de los detalles de problemas para los códigos de estado de error</span><span class="sxs-lookup"><span data-stu-id="b7d93-191">Problem details responses for error status codes</span></span>

<span data-ttu-id="b7d93-192">En ASP.NET Core 2.2 o versiones posteriores, MVC transforma un resultado de error (un resultado con el código de estado 400 o superior) en un resultado con <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="b7d93-192">In ASP.NET Core 2.2 or later, MVC transforms an error result (a result with status code 400 or higher) to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span> <span data-ttu-id="b7d93-193">`ProblemDetails` es:</span><span class="sxs-lookup"><span data-stu-id="b7d93-193">`ProblemDetails` is:</span></span>

* <span data-ttu-id="b7d93-194">Un tipo basado en la [especificación RFC 7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="b7d93-194">A type based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span>
* <span data-ttu-id="b7d93-195">Un formato estandarizado usado para especificar detalles de error de lectura mecánica en una respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="b7d93-195">A standardized format for specifying machine-readable error details in an HTTP response.</span></span>

<span data-ttu-id="b7d93-196">Observe el código siguiente en una acción de controlador:</span><span class="sxs-lookup"><span data-stu-id="b7d93-196">Consider the following code in a controller action:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Controllers/ProductsController.cs?name=snippet_ProblemDetailsStatusCode)]

<span data-ttu-id="b7d93-197">La respuesta HTTP para `NotFound` tiene un código de estado 404 con un cuerpo `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="b7d93-197">The HTTP response for `NotFound` has a 404 status code with a `ProblemDetails` body.</span></span> <span data-ttu-id="b7d93-198">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="b7d93-198">For example:</span></span>

```json
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

<span data-ttu-id="b7d93-199">La característica de los detalles del problema requiere una marca de compatibilidad 2.2 o posterior.</span><span class="sxs-lookup"><span data-stu-id="b7d93-199">The problem details feature requires a compatibility flag of 2.2 or later.</span></span> <span data-ttu-id="b7d93-200">El comportamiento predeterminado se deshabilita al establecer la propiedad `SuppressMapClientErrors` en `true`.</span><span class="sxs-lookup"><span data-stu-id="b7d93-200">The default behavior is disabled when the `SuppressMapClientErrors` property is set to `true`.</span></span> <span data-ttu-id="b7d93-201">Agregue el siguiente código en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b7d93-201">Add the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8)]

<span data-ttu-id="b7d93-202">Use la propiedad `ClientErrorMapping` para configurar el contenido de la respuesta `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="b7d93-202">Use the `ClientErrorMapping` property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="b7d93-203">Por ejemplo, el código siguiente permite actualizar la propiedad `type` para respuestas 404:</span><span class="sxs-lookup"><span data-stu-id="b7d93-203">For example, the following code updates the `type` property for 404 responses:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=10-11)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="b7d93-204">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="b7d93-204">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
