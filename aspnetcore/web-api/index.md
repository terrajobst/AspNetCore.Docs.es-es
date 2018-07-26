---
title: Compilación de API web con ASP.NET Core
author: scottaddie
description: Obtenga información sobre las características disponibles para la compilación de una API web en ASP.NET Core y los casos en los que se recomienda usar cada una.
ms.author: scaddie
ms.custom: mvc
ms.date: 07/06/2018
uid: web-api/index
ms.openlocfilehash: ccee4f7bae0abe1b36088d58e5c1e1362d8de9f0
ms.sourcegitcommit: 5338b1ed9e2ef225ab565d6cba072b474fd9324d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/25/2018
ms.locfileid: "39243102"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="6d4e9-103">Compilación de API web con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6d4e9-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="6d4e9-104">Por [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="6d4e9-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="6d4e9-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6d4e9-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="6d4e9-106">En este documento se explica cómo crear una API web en ASP.NET Core y los casos en los que se recomienda usar cada una.</span><span class="sxs-lookup"><span data-stu-id="6d4e9-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="6d4e9-107">Derivación de una clase desde ControllerBase</span><span class="sxs-lookup"><span data-stu-id="6d4e9-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="6d4e9-108">Herede desde la clase [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) en un controlador diseñado para funcionar como API web.</span><span class="sxs-lookup"><span data-stu-id="6d4e9-108">Inherit from the [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="6d4e9-109">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="6d4e9-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="6d4e9-110">La clase `ControllerBase` proporciona acceso a varios métodos y propiedades.</span><span class="sxs-lookup"><span data-stu-id="6d4e9-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="6d4e9-111">En el código anterior, algunos ejemplos incluyen [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) y [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span><span class="sxs-lookup"><span data-stu-id="6d4e9-111">In the preceding code, examples include [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) and [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span></span> <span data-ttu-id="6d4e9-112">Se llama a estos métodos en los métodos de acción para devolver los códigos de estado HTTP 400 y 201, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="6d4e9-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="6d4e9-113">La propiedad [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate), que también se proporciona con `ControllerBase`, se usa para controlar la validación del modelo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="6d4e9-113">The [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="6d4e9-114">Anotación de una clase con ApiControllerAttribute</span><span class="sxs-lookup"><span data-stu-id="6d4e9-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="6d4e9-115">ASP.NET Core 2.1 incorpora el atributo [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) para designar una clase de controlador de API web.</span><span class="sxs-lookup"><span data-stu-id="6d4e9-115">ASP.NET Core 2.1 introduces the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="6d4e9-116">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="6d4e9-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="6d4e9-117">Para usar este atributo, se requiere una versión de compatibilidad de 2.1 o posterior, establecida mediante [SetCompatibilityVersion](/dotnet/api/microsoft.extensions.dependencyinjection.mvccoremvcbuilderextensions.setcompatibilityversion).</span><span class="sxs-lookup"><span data-stu-id="6d4e9-117">A compatibility version of 2.1 or later, set via [SetCompatibilityVersion](/dotnet/api/microsoft.extensions.dependencyinjection.mvccoremvcbuilderextensions.setcompatibilityversion), is required to use this attribute.</span></span> <span data-ttu-id="6d4e9-118">Por ejemplo, el código resaltado en *Startup.ConfigureServices* establece la marca de compatibilidad 2.1:</span><span class="sxs-lookup"><span data-stu-id="6d4e9-118">For example, the highlighted code in *Startup.ConfigureServices* sets the 2.1 compatibility flag:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="6d4e9-119">El atributo `[ApiController]` se suele emparejar con `ControllerBase` para habilitar el comportamiento específico de REST para los controladores.</span><span class="sxs-lookup"><span data-stu-id="6d4e9-119">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="6d4e9-120">`ControllerBase` proporciona acceso a métodos como [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) y [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span><span class="sxs-lookup"><span data-stu-id="6d4e9-120">`ControllerBase` provides access to methods such as [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) and [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span></span>

<span data-ttu-id="6d4e9-121">Otro enfoque consiste en crear una clase de controlador base personalizada anotada con el atributo `[ApiController]`:</span><span class="sxs-lookup"><span data-stu-id="6d4e9-121">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="6d4e9-122">En las secciones siguientes se describen las ventajas de las características que aporta el atributo.</span><span class="sxs-lookup"><span data-stu-id="6d4e9-122">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="6d4e9-123">Respuestas HTTP 400 automáticas</span><span class="sxs-lookup"><span data-stu-id="6d4e9-123">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="6d4e9-124">Los errores de validación desencadenan automáticamente una respuesta HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="6d4e9-124">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="6d4e9-125">El código siguiente deja de ser necesario en las acciones:</span><span class="sxs-lookup"><span data-stu-id="6d4e9-125">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

<span data-ttu-id="6d4e9-126">El comportamiento predeterminado se deshabilita cuando la propiedad [SuppressModelStateInvalidFilter](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressmodelstateinvalidfilter) se establece en `true`.</span><span class="sxs-lookup"><span data-stu-id="6d4e9-126">The default behavior is disabled when the [SuppressModelStateInvalidFilter](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressmodelstateinvalidfilter) property is set to `true`.</span></span> <span data-ttu-id="6d4e9-127">Agregue el siguiente código en *Startup.ConfigureServices* después de `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="6d4e9-127">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="6d4e9-128">Inferencia de parámetro de origen de enlace</span><span class="sxs-lookup"><span data-stu-id="6d4e9-128">Binding source parameter inference</span></span>

<span data-ttu-id="6d4e9-129">Un atributo de origen de enlace define la ubicación del valor del parámetro de una acción.</span><span class="sxs-lookup"><span data-stu-id="6d4e9-129">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="6d4e9-130">Existen los atributos de origen de enlace siguientes:</span><span class="sxs-lookup"><span data-stu-id="6d4e9-130">The following binding source attributes exist:</span></span>

|<span data-ttu-id="6d4e9-131">Atributo</span><span class="sxs-lookup"><span data-stu-id="6d4e9-131">Attribute</span></span>|<span data-ttu-id="6d4e9-132">Origen de enlace</span><span class="sxs-lookup"><span data-stu-id="6d4e9-132">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="6d4e9-133">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span><span class="sxs-lookup"><span data-stu-id="6d4e9-133">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span></span>     | <span data-ttu-id="6d4e9-134">Cuerpo de la solicitud</span><span class="sxs-lookup"><span data-stu-id="6d4e9-134">Request body</span></span> |
|<span data-ttu-id="6d4e9-135">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span><span class="sxs-lookup"><span data-stu-id="6d4e9-135">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span></span>     | <span data-ttu-id="6d4e9-136">Datos del formulario en el cuerpo de la solicitud</span><span class="sxs-lookup"><span data-stu-id="6d4e9-136">Form data in the request body</span></span> |
|<span data-ttu-id="6d4e9-137">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span><span class="sxs-lookup"><span data-stu-id="6d4e9-137">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span></span> | <span data-ttu-id="6d4e9-138">Encabezado de la solicitud</span><span class="sxs-lookup"><span data-stu-id="6d4e9-138">Request header</span></span> |
|<span data-ttu-id="6d4e9-139">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span><span class="sxs-lookup"><span data-stu-id="6d4e9-139">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span></span>   | <span data-ttu-id="6d4e9-140">Parámetro de la cadena de consulta de la solicitud</span><span class="sxs-lookup"><span data-stu-id="6d4e9-140">Request query string parameter</span></span> |
|<span data-ttu-id="6d4e9-141">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span><span class="sxs-lookup"><span data-stu-id="6d4e9-141">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span></span>   | <span data-ttu-id="6d4e9-142">Datos de ruta de la solicitud actual</span><span class="sxs-lookup"><span data-stu-id="6d4e9-142">Route data from the current request</span></span> |
|<span data-ttu-id="6d4e9-143">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="6d4e9-143">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="6d4e9-144">Servicio de solicitud insertado como parámetro de acción</span><span class="sxs-lookup"><span data-stu-id="6d4e9-144">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="6d4e9-145">No use `[FromRoute]` si los valores pueden contener `%2f` (es decir, `/`).</span><span class="sxs-lookup"><span data-stu-id="6d4e9-145">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="6d4e9-146">`%2f` no incluirá el carácter sin escape `/`.</span><span class="sxs-lookup"><span data-stu-id="6d4e9-146">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="6d4e9-147">Use `[FromQuery]` si el valor puede contener `%2f`.</span><span class="sxs-lookup"><span data-stu-id="6d4e9-147">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="6d4e9-148">Sin el `[ApiController]` atributo, los atributos de origen de enlace se definen explícitamente.</span><span class="sxs-lookup"><span data-stu-id="6d4e9-148">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="6d4e9-149">En el ejemplo siguiente, el atributo `[FromQuery]` indica que el valor del parámetro `discontinuedOnly` se proporciona en la cadena de consulta de la dirección URL de la solicitud:</span><span class="sxs-lookup"><span data-stu-id="6d4e9-149">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="6d4e9-150">Las reglas de inferencia se aplican para los orígenes de datos predeterminados de los parámetros de acción.</span><span class="sxs-lookup"><span data-stu-id="6d4e9-150">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="6d4e9-151">Estas reglas configuran los orígenes de enlace que aplicaría manualmente a los parámetros de acción.</span><span class="sxs-lookup"><span data-stu-id="6d4e9-151">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="6d4e9-152">Los atributos de origen de enlace presentan este comportamiento:</span><span class="sxs-lookup"><span data-stu-id="6d4e9-152">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="6d4e9-153">**[FromBody]** se infiere para los parámetros de tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="6d4e9-153">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="6d4e9-154">La excepción a esta regla es cualquier tipo integrado complejo que tenga un significado especial, como [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) o [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span><span class="sxs-lookup"><span data-stu-id="6d4e9-154">An exception to this rule is any complex, built-in type with a special meaning, such as [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) and [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span></span> <span data-ttu-id="6d4e9-155">El código de inferencia del origen de enlace omite esos tipos especiales.</span><span class="sxs-lookup"><span data-stu-id="6d4e9-155">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="6d4e9-156">Cuando una acción contiene más de un parámetro que se especifica explícitamente (a través de `[FromBody]`) o se infiere como enlazado desde el cuerpo de la solicitud, se produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="6d4e9-156">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="6d4e9-157">Por ejemplo, las firmas de acción siguientes provocan una excepción:</span><span class="sxs-lookup"><span data-stu-id="6d4e9-157">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="6d4e9-158">**[FromForm]** se infiere para los parámetros de acción de tipo [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) o [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span><span class="sxs-lookup"><span data-stu-id="6d4e9-158">**[FromForm]** is inferred for action parameters of type [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span></span> <span data-ttu-id="6d4e9-159">No se infiere para los tipos simples o definidos por el usuario.</span><span class="sxs-lookup"><span data-stu-id="6d4e9-159">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="6d4e9-160">**[FromRoute]** se infiere para cualquier nombre de parámetro de acción que coincida con un parámetro de la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="6d4e9-160">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="6d4e9-161">Si varias rutas coinciden con un parámetro de acción, cualquier valor de ruta se considera `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="6d4e9-161">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="6d4e9-162">**[FromQuery]** se infiere para cualquier otro parámetro de acción.</span><span class="sxs-lookup"><span data-stu-id="6d4e9-162">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="6d4e9-163">Las reglas de inferencia predeterminadas se deshabilitan cuando la propiedad [SuppressInferBindingSourcesForParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressinferbindingsourcesforparameters) se establece en `true`.</span><span class="sxs-lookup"><span data-stu-id="6d4e9-163">The default inference rules are disabled when the [SuppressInferBindingSourcesForParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressinferbindingsourcesforparameters) property is set to `true`.</span></span> <span data-ttu-id="6d4e9-164">Agregue el siguiente código en *Startup.ConfigureServices* después de `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="6d4e9-164">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="6d4e9-165">Inferencia de solicitud de varios elementos o datos de formulario</span><span class="sxs-lookup"><span data-stu-id="6d4e9-165">Multipart/form-data request inference</span></span>

<span data-ttu-id="6d4e9-166">Si un parámetro de acción se anota con el atributo [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute), se infiere el tipo de contenido de la solicitud `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="6d4e9-166">When an action parameter is annotated with the [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="6d4e9-167">El comportamiento predeterminado se deshabilita cuando la propiedad [SuppressConsumesConstraintForFormFileParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressconsumesconstraintforformfileparameters) se establece en `true`.</span><span class="sxs-lookup"><span data-stu-id="6d4e9-167">The default behavior is disabled when the [SuppressConsumesConstraintForFormFileParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressconsumesconstraintforformfileparameters) property is set to `true`.</span></span> <span data-ttu-id="6d4e9-168">Agregue el siguiente código en *Startup.ConfigureServices* después de `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="6d4e9-168">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="6d4e9-169">Requisito de enrutamiento mediante atributos</span><span class="sxs-lookup"><span data-stu-id="6d4e9-169">Attribute routing requirement</span></span>

<span data-ttu-id="6d4e9-170">El enrutamiento mediante atributos pasa a ser un requisito.</span><span class="sxs-lookup"><span data-stu-id="6d4e9-170">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="6d4e9-171">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="6d4e9-171">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="6d4e9-172">Las acciones dejan de estar disponibles a través de las [rutas convencionales](xref:mvc/controllers/routing#conventional-routing) definidas en [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) o mediante [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) en *Startup.Configure*.</span><span class="sxs-lookup"><span data-stu-id="6d4e9-172">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) or by [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) in *Startup.Configure*.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="6d4e9-173">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="6d4e9-173">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
