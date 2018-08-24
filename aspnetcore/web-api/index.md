---
title: Compilación de API web con ASP.NET Core
author: scottaddie
description: Obtenga información sobre las características disponibles para la compilación de una API web en ASP.NET Core y los casos en los que se recomienda usar cada una.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/15/2018
uid: web-api/index
ms.openlocfilehash: d410f28ff7fda3bf33f73c06b3e626dfd4ee7dd8
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2018
ms.locfileid: "41822145"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="3b83c-103">Compilación de API web con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3b83c-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="3b83c-104">Por [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="3b83c-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="3b83c-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3b83c-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="3b83c-106">En este documento se explica cómo crear una API web en ASP.NET Core y los casos en los que se recomienda usar cada una.</span><span class="sxs-lookup"><span data-stu-id="3b83c-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="3b83c-107">Derivación de una clase desde ControllerBase</span><span class="sxs-lookup"><span data-stu-id="3b83c-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="3b83c-108">Herede desde la clase <xref:Microsoft.AspNetCore.Mvc.ControllerBase> en un controlador diseñado para funcionar como API web.</span><span class="sxs-lookup"><span data-stu-id="3b83c-108">Inherit from the <xref:Microsoft.AspNetCore.Mvc.ControllerBase> class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="3b83c-109">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="3b83c-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="3b83c-110">La clase `ControllerBase` proporciona acceso a varios métodos y propiedades.</span><span class="sxs-lookup"><span data-stu-id="3b83c-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="3b83c-111">En el código anterior, algunos ejemplos son <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> y <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span><span class="sxs-lookup"><span data-stu-id="3b83c-111">In the preceding code, examples include <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span></span> <span data-ttu-id="3b83c-112">Se llama a estos métodos en los métodos de acción para devolver los códigos de estado HTTP 400 y 201, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="3b83c-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="3b83c-113">La propiedad <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState>, que también se proporciona con `ControllerBase`, se usa para controlar la validación del modelo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="3b83c-113">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="3b83c-114">Anotación de una clase con ApiControllerAttribute</span><span class="sxs-lookup"><span data-stu-id="3b83c-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="3b83c-115">ASP.NET Core 2.1 incorpora el atributo [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) para designar una clase de controlador de API web.</span><span class="sxs-lookup"><span data-stu-id="3b83c-115">ASP.NET Core 2.1 introduces the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="3b83c-116">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="3b83c-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="3b83c-117">Para usar este atributo se requiere una versión de compatibilidad de 2.1 o posterior, establecida mediante <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>.</span><span class="sxs-lookup"><span data-stu-id="3b83c-117">A compatibility version of 2.1 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute.</span></span> <span data-ttu-id="3b83c-118">Por ejemplo, el código resaltado en *Startup.ConfigureServices* establece la marca de compatibilidad 2.1:</span><span class="sxs-lookup"><span data-stu-id="3b83c-118">For example, the highlighted code in *Startup.ConfigureServices* sets the 2.1 compatibility flag:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="3b83c-119">Para obtener más información, vea <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="3b83c-119">For more information, see <xref:mvc/compatibility-version>.</span></span>

<span data-ttu-id="3b83c-120">El atributo `[ApiController]` se suele emparejar con `ControllerBase` para habilitar el comportamiento específico de REST para los controladores.</span><span class="sxs-lookup"><span data-stu-id="3b83c-120">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="3b83c-121">`ControllerBase` proporciona acceso a métodos como <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> y <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span><span class="sxs-lookup"><span data-stu-id="3b83c-121">`ControllerBase` provides access to methods such as <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span></span>

<span data-ttu-id="3b83c-122">Otro enfoque consiste en crear una clase de controlador base personalizada anotada con el atributo `[ApiController]`:</span><span class="sxs-lookup"><span data-stu-id="3b83c-122">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="3b83c-123">En las secciones siguientes se describen las ventajas de las características que aporta el atributo.</span><span class="sxs-lookup"><span data-stu-id="3b83c-123">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="3b83c-124">Respuestas HTTP 400 automáticas</span><span class="sxs-lookup"><span data-stu-id="3b83c-124">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="3b83c-125">Los errores de validación desencadenan automáticamente una respuesta HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="3b83c-125">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="3b83c-126">El código siguiente deja de ser necesario en las acciones:</span><span class="sxs-lookup"><span data-stu-id="3b83c-126">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

<span data-ttu-id="3b83c-127">El comportamiento predeterminado se deshabilita al establecer la propiedad <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> en `true`.</span><span class="sxs-lookup"><span data-stu-id="3b83c-127">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property is set to `true`.</span></span> <span data-ttu-id="3b83c-128">Agregue el siguiente código en *Startup.ConfigureServices* después de `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="3b83c-128">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="3b83c-129">Inferencia de parámetro de origen de enlace</span><span class="sxs-lookup"><span data-stu-id="3b83c-129">Binding source parameter inference</span></span>

<span data-ttu-id="3b83c-130">Un atributo de origen de enlace define la ubicación del valor del parámetro de una acción.</span><span class="sxs-lookup"><span data-stu-id="3b83c-130">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="3b83c-131">Existen los atributos de origen de enlace siguientes:</span><span class="sxs-lookup"><span data-stu-id="3b83c-131">The following binding source attributes exist:</span></span>

|<span data-ttu-id="3b83c-132">Atributo</span><span class="sxs-lookup"><span data-stu-id="3b83c-132">Attribute</span></span>|<span data-ttu-id="3b83c-133">Origen de enlace</span><span class="sxs-lookup"><span data-stu-id="3b83c-133">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="3b83c-134">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span><span class="sxs-lookup"><span data-stu-id="3b83c-134">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span></span>     | <span data-ttu-id="3b83c-135">Cuerpo de la solicitud</span><span class="sxs-lookup"><span data-stu-id="3b83c-135">Request body</span></span> |
|<span data-ttu-id="3b83c-136">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span><span class="sxs-lookup"><span data-stu-id="3b83c-136">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span></span>     | <span data-ttu-id="3b83c-137">Datos del formulario en el cuerpo de la solicitud</span><span class="sxs-lookup"><span data-stu-id="3b83c-137">Form data in the request body</span></span> |
|<span data-ttu-id="3b83c-138">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span><span class="sxs-lookup"><span data-stu-id="3b83c-138">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span></span> | <span data-ttu-id="3b83c-139">Encabezado de la solicitud</span><span class="sxs-lookup"><span data-stu-id="3b83c-139">Request header</span></span> |
|<span data-ttu-id="3b83c-140">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span><span class="sxs-lookup"><span data-stu-id="3b83c-140">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span></span>   | <span data-ttu-id="3b83c-141">Parámetro de la cadena de consulta de la solicitud</span><span class="sxs-lookup"><span data-stu-id="3b83c-141">Request query string parameter</span></span> |
|<span data-ttu-id="3b83c-142">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span><span class="sxs-lookup"><span data-stu-id="3b83c-142">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span></span>   | <span data-ttu-id="3b83c-143">Datos de ruta de la solicitud actual</span><span class="sxs-lookup"><span data-stu-id="3b83c-143">Route data from the current request</span></span> |
|<span data-ttu-id="3b83c-144">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="3b83c-144">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="3b83c-145">Servicio de solicitud insertado como parámetro de acción</span><span class="sxs-lookup"><span data-stu-id="3b83c-145">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="3b83c-146">No use `[FromRoute]` si los valores pueden contener `%2f` (es decir, `/`).</span><span class="sxs-lookup"><span data-stu-id="3b83c-146">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="3b83c-147">`%2f` no incluirá el carácter sin escape `/`.</span><span class="sxs-lookup"><span data-stu-id="3b83c-147">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="3b83c-148">Use `[FromQuery]` si el valor puede contener `%2f`.</span><span class="sxs-lookup"><span data-stu-id="3b83c-148">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="3b83c-149">Sin el `[ApiController]` atributo, los atributos de origen de enlace se definen explícitamente.</span><span class="sxs-lookup"><span data-stu-id="3b83c-149">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="3b83c-150">En el ejemplo siguiente, el atributo `[FromQuery]` indica que el valor del parámetro `discontinuedOnly` se proporciona en la cadena de consulta de la dirección URL de la solicitud:</span><span class="sxs-lookup"><span data-stu-id="3b83c-150">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="3b83c-151">Las reglas de inferencia se aplican para los orígenes de datos predeterminados de los parámetros de acción.</span><span class="sxs-lookup"><span data-stu-id="3b83c-151">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="3b83c-152">Estas reglas configuran los orígenes de enlace que aplicaría manualmente a los parámetros de acción.</span><span class="sxs-lookup"><span data-stu-id="3b83c-152">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="3b83c-153">Los atributos de origen de enlace presentan este comportamiento:</span><span class="sxs-lookup"><span data-stu-id="3b83c-153">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="3b83c-154">**[FromBody]** se infiere para los parámetros de tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="3b83c-154">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="3b83c-155">La excepción a esta regla es cualquier tipo integrado complejo que tenga un significado especial, como <xref:Microsoft.AspNetCore.Http.IFormCollection> y <xref:System.Threading.CancellationToken>.</span><span class="sxs-lookup"><span data-stu-id="3b83c-155">An exception to this rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="3b83c-156">El código de inferencia del origen de enlace omite esos tipos especiales.</span><span class="sxs-lookup"><span data-stu-id="3b83c-156">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="3b83c-157">En el caso de los tipos simples, como `string` y `int`, `[FromBody]` no se infiere.</span><span class="sxs-lookup"><span data-stu-id="3b83c-157">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="3b83c-158">Así pues, para los tipos simples, en los casos en los que quiera utilizar dicha funcionalidad, conviene usar el atributo `[FromBody]`.</span><span class="sxs-lookup"><span data-stu-id="3b83c-158">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is desired.</span></span> <span data-ttu-id="3b83c-159">Cuando una acción contiene más de un parámetro que se especifica explícitamente (a través de `[FromBody]`) o se infiere como enlazado desde el cuerpo de la solicitud, se produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="3b83c-159">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="3b83c-160">Por ejemplo, las firmas de acción siguientes provocan una excepción:</span><span class="sxs-lookup"><span data-stu-id="3b83c-160">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="3b83c-161">**[FromForm]** se infiere para los parámetros de acción de tipo <xref:Microsoft.AspNetCore.Http.IFormFile> y <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span><span class="sxs-lookup"><span data-stu-id="3b83c-161">**[FromForm]** is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="3b83c-162">No se infiere para los tipos simples o definidos por el usuario.</span><span class="sxs-lookup"><span data-stu-id="3b83c-162">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="3b83c-163">**[FromRoute]** se infiere para cualquier nombre de parámetro de acción que coincida con un parámetro de la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="3b83c-163">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="3b83c-164">Si varias rutas coinciden con un parámetro de acción, cualquier valor de ruta se considera `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="3b83c-164">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="3b83c-165">**[FromQuery]** se infiere para cualquier otro parámetro de acción.</span><span class="sxs-lookup"><span data-stu-id="3b83c-165">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="3b83c-166">Las reglas de inferencia predeterminadas se deshabilitan al establecer la propiedad <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> en `true`.</span><span class="sxs-lookup"><span data-stu-id="3b83c-166">The default inference rules are disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> property is set to `true`.</span></span> <span data-ttu-id="3b83c-167">Agregue el siguiente código en *Startup.ConfigureServices* después de `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="3b83c-167">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="3b83c-168">Inferencia de solicitud de varios elementos o datos de formulario</span><span class="sxs-lookup"><span data-stu-id="3b83c-168">Multipart/form-data request inference</span></span>

<span data-ttu-id="3b83c-169">Si un parámetro de acción se anota con el atributo [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute), se infiere el tipo de contenido de la solicitud `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="3b83c-169">When an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="3b83c-170">El comportamiento predeterminado se deshabilita al establecer la propiedad <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> en `true`.</span><span class="sxs-lookup"><span data-stu-id="3b83c-170">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property is set to `true`.</span></span> <span data-ttu-id="3b83c-171">Agregue el siguiente código en *Startup.ConfigureServices* después de `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="3b83c-171">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="3b83c-172">Requisito de enrutamiento mediante atributos</span><span class="sxs-lookup"><span data-stu-id="3b83c-172">Attribute routing requirement</span></span>

<span data-ttu-id="3b83c-173">El enrutamiento mediante atributos pasa a ser un requisito.</span><span class="sxs-lookup"><span data-stu-id="3b83c-173">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="3b83c-174">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="3b83c-174">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="3b83c-175">Las acciones dejan de estar disponibles a través de las [rutas convencionales](xref:mvc/controllers/routing#conventional-routing) definidas en <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> o mediante <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> en *Startup.Configure*.</span><span class="sxs-lookup"><span data-stu-id="3b83c-175">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in *Startup.Configure*.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="3b83c-176">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="3b83c-176">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
