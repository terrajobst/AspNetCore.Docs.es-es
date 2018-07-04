---
title: Compilación de API web con ASP.NET Core
author: scottaddie
description: Obtenga información sobre las características disponibles para la compilación de una API web en ASP.NET Core y los casos en los que se recomienda usar cada una.
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
uid: web-api/index
ms.openlocfilehash: ab672667d1ca349d80c4ca80f8d1f32f4871c7e6
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/04/2018
ms.locfileid: "36274971"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="74b3b-103">Compilación de API web con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="74b3b-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="74b3b-104">Por [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="74b3b-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="74b3b-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="74b3b-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="74b3b-106">En este documento se explica cómo crear una API web en ASP.NET Core y los casos en los que se recomienda usar cada una.</span><span class="sxs-lookup"><span data-stu-id="74b3b-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="74b3b-107">Derivación de una clase desde ControllerBase</span><span class="sxs-lookup"><span data-stu-id="74b3b-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="74b3b-108">Herede desde la clase [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) en un controlador diseñado para funcionar como API web.</span><span class="sxs-lookup"><span data-stu-id="74b3b-108">Inherit from the [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="74b3b-109">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="74b3b-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]
::: moniker-end

<span data-ttu-id="74b3b-110">La clase `ControllerBase` proporciona acceso a varios métodos y propiedades.</span><span class="sxs-lookup"><span data-stu-id="74b3b-110">The `ControllerBase` class provides access to numerous properties and methods.</span></span> <span data-ttu-id="74b3b-111">En el ejemplo anterior, algunos métodos de este tipo incluyen [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) y [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span><span class="sxs-lookup"><span data-stu-id="74b3b-111">In the preceding example, some such methods include [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) and [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span></span> <span data-ttu-id="74b3b-112">Estos métodos se invocan en los métodos de acción para devolver los códigos de estado HTTP 400 y 201, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="74b3b-112">These methods are invoked within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="74b3b-113">La propiedad [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate), que también se proporciona con `ControllerBase`, se usa para realizar la validación del modelo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="74b3b-113">The [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) property, also provided by `ControllerBase`, is accessed to perform request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"
## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="74b3b-114">Anotación de una clase con ApiControllerAttribute</span><span class="sxs-lookup"><span data-stu-id="74b3b-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="74b3b-115">ASP.NET Core 2.1 incorpora el atributo [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) para designar una clase de controlador de API web.</span><span class="sxs-lookup"><span data-stu-id="74b3b-115">ASP.NET Core 2.1 introduces the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="74b3b-116">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="74b3b-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="74b3b-117">Este atributo se suele emparejar con `ControllerBase` para acceder a propiedades y métodos útiles.</span><span class="sxs-lookup"><span data-stu-id="74b3b-117">This attribute is commonly coupled with `ControllerBase` to gain access to useful methods and properties.</span></span> <span data-ttu-id="74b3b-118">`ControllerBase` proporciona acceso a métodos como [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) y [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span><span class="sxs-lookup"><span data-stu-id="74b3b-118">`ControllerBase` provides access to methods such as [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) and [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span></span>

<span data-ttu-id="74b3b-119">Otro enfoque consiste en crear una clase de controlador base personalizada anotada con el atributo `[ApiController]`:</span><span class="sxs-lookup"><span data-stu-id="74b3b-119">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="74b3b-120">En las secciones siguientes se describen las ventajas de las características que aporta el atributo.</span><span class="sxs-lookup"><span data-stu-id="74b3b-120">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="74b3b-121">Respuestas HTTP 400 automáticas</span><span class="sxs-lookup"><span data-stu-id="74b3b-121">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="74b3b-122">Los errores de validación desencadenan automáticamente una respuesta HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="74b3b-122">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="74b3b-123">El código siguiente deja de ser necesario en las acciones:</span><span class="sxs-lookup"><span data-stu-id="74b3b-123">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]

<span data-ttu-id="74b3b-124">Este comportamiento predeterminado se deshabilita con siguiente código en *Startup.ConfigureServices*:</span><span class="sxs-lookup"><span data-stu-id="74b3b-124">This default behavior is disabled with the following code in *Startup.ConfigureServices*:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="74b3b-125">Inferencia de parámetro de origen de enlace</span><span class="sxs-lookup"><span data-stu-id="74b3b-125">Binding source parameter inference</span></span>

<span data-ttu-id="74b3b-126">Un atributo de origen de enlace define la ubicación del valor del parámetro de una acción.</span><span class="sxs-lookup"><span data-stu-id="74b3b-126">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="74b3b-127">Existen los atributos de origen de enlace siguientes:</span><span class="sxs-lookup"><span data-stu-id="74b3b-127">The following binding source attributes exist:</span></span>

|<span data-ttu-id="74b3b-128">Atributo</span><span class="sxs-lookup"><span data-stu-id="74b3b-128">Attribute</span></span>|<span data-ttu-id="74b3b-129">Origen de enlace</span><span class="sxs-lookup"><span data-stu-id="74b3b-129">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="74b3b-130">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span><span class="sxs-lookup"><span data-stu-id="74b3b-130">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span></span>     | <span data-ttu-id="74b3b-131">Cuerpo de la solicitud</span><span class="sxs-lookup"><span data-stu-id="74b3b-131">Request body</span></span> |
|<span data-ttu-id="74b3b-132">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span><span class="sxs-lookup"><span data-stu-id="74b3b-132">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span></span>     | <span data-ttu-id="74b3b-133">Datos del formulario en el cuerpo de la solicitud</span><span class="sxs-lookup"><span data-stu-id="74b3b-133">Form data in the request body</span></span> |
|<span data-ttu-id="74b3b-134">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span><span class="sxs-lookup"><span data-stu-id="74b3b-134">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span></span> | <span data-ttu-id="74b3b-135">Encabezado de la solicitud</span><span class="sxs-lookup"><span data-stu-id="74b3b-135">Request header</span></span> |
|<span data-ttu-id="74b3b-136">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span><span class="sxs-lookup"><span data-stu-id="74b3b-136">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span></span>   | <span data-ttu-id="74b3b-137">Parámetro de la cadena de consulta de la solicitud</span><span class="sxs-lookup"><span data-stu-id="74b3b-137">Request query string parameter</span></span> |
|<span data-ttu-id="74b3b-138">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span><span class="sxs-lookup"><span data-stu-id="74b3b-138">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span></span>   | <span data-ttu-id="74b3b-139">Datos de ruta de la solicitud actual</span><span class="sxs-lookup"><span data-stu-id="74b3b-139">Route data from the current request</span></span> |
|<span data-ttu-id="74b3b-140">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="74b3b-140">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="74b3b-141">Servicio de solicitud insertado como parámetro de acción</span><span class="sxs-lookup"><span data-stu-id="74b3b-141">The request service injected as an action parameter</span></span> |

> [!NOTE]
> <span data-ttu-id="74b3b-142">**No** use `[FromRoute]` si los valores pueden contener `%2f` (es decir, `/`) porque `%2f` no incluirá el carácter sin escape `/`.</span><span class="sxs-lookup"><span data-stu-id="74b3b-142">Do **not** use `[FromRoute]` when values might contain `%2f` (that is `/`) because `%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="74b3b-143">Use `[FromQuery]` si el valor puede contener `%2f`.</span><span class="sxs-lookup"><span data-stu-id="74b3b-143">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="74b3b-144">Sin el `[ApiController]` atributo, los atributos de origen de enlace se definen explícitamente.</span><span class="sxs-lookup"><span data-stu-id="74b3b-144">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="74b3b-145">En el ejemplo siguiente, el atributo `[FromQuery]` indica que el valor del parámetro `discontinuedOnly` se proporciona en la cadena de consulta de la dirección URL de la solicitud:</span><span class="sxs-lookup"><span data-stu-id="74b3b-145">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=2)]

<span data-ttu-id="74b3b-146">Las reglas de inferencia se aplican para los orígenes de datos predeterminados de los parámetros de acción.</span><span class="sxs-lookup"><span data-stu-id="74b3b-146">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="74b3b-147">Estas reglas configuran los orígenes de enlace que aplicaría manualmente a los parámetros de acción.</span><span class="sxs-lookup"><span data-stu-id="74b3b-147">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="74b3b-148">Los atributos de origen de enlace presentan este comportamiento:</span><span class="sxs-lookup"><span data-stu-id="74b3b-148">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="74b3b-149">**[FromBody]** se infiere para los parámetros de tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="74b3b-149">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="74b3b-150">La excepción a esta regla es cualquier tipo integrado complejo que tenga un significado especial, como [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) o [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span><span class="sxs-lookup"><span data-stu-id="74b3b-150">An exception to this rule is any complex, built-in type with a special meaning, such as [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) and [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span></span> <span data-ttu-id="74b3b-151">El código de inferencia del origen de enlace omite esos tipos especiales.</span><span class="sxs-lookup"><span data-stu-id="74b3b-151">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="74b3b-152">Cuando una acción contiene más de un parámetro que se especifica explícitamente (a través de `[FromBody]`) o se infiere como enlazado desde el cuerpo de la solicitud, se produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="74b3b-152">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="74b3b-153">Por ejemplo, las firmas de acción siguientes provocan una excepción:</span><span class="sxs-lookup"><span data-stu-id="74b3b-153">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="74b3b-154">**[FromForm]** se infiere para los parámetros de acción de tipo [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) o [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span><span class="sxs-lookup"><span data-stu-id="74b3b-154">**[FromForm]** is inferred for action parameters of type [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span></span> <span data-ttu-id="74b3b-155">No se infiere para los tipos simples o definidos por el usuario.</span><span class="sxs-lookup"><span data-stu-id="74b3b-155">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="74b3b-156">**[FromRoute]** se infiere para cualquier nombre de parámetro de acción que coincida con un parámetro de la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="74b3b-156">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="74b3b-157">Si varias rutas coinciden con un parámetro de acción, cualquier valor de ruta se considera `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="74b3b-157">When multiple routes match an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="74b3b-158">**[FromQuery]** se infiere para cualquier otro parámetro de acción.</span><span class="sxs-lookup"><span data-stu-id="74b3b-158">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="74b3b-159">Las reglas de inferencia predeterminadas se deshabilitan con el código siguiente en *Startup.ConfigureServices*:</span><span class="sxs-lookup"><span data-stu-id="74b3b-159">The default inference rules are disabled with the following code in *Startup.ConfigureServices*:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="74b3b-160">Inferencia de solicitud de varios elementos o datos de formulario</span><span class="sxs-lookup"><span data-stu-id="74b3b-160">Multipart/form-data request inference</span></span>

<span data-ttu-id="74b3b-161">Si un parámetro de acción se anota con el atributo [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute), se infiere el tipo de contenido de la solicitud `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="74b3b-161">When an action parameter is annotated with the [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="74b3b-162">El comportamiento predeterminado se deshabilita con el siguiente código en *Startup.ConfigureServices*:</span><span class="sxs-lookup"><span data-stu-id="74b3b-162">The default behavior is disabled with the following code in *Startup.ConfigureServices*:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="74b3b-163">Requisito de enrutamiento mediante atributos</span><span class="sxs-lookup"><span data-stu-id="74b3b-163">Attribute routing requirement</span></span>

<span data-ttu-id="74b3b-164">El enrutamiento mediante atributos pasa a ser un requisito.</span><span class="sxs-lookup"><span data-stu-id="74b3b-164">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="74b3b-165">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="74b3b-165">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="74b3b-166">Las acciones dejan de estar disponibles a través de las [rutas convencionales](xref:mvc/controllers/routing#conventional-routing) definidas en [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) o mediante [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) en *Startup.Configure*.</span><span class="sxs-lookup"><span data-stu-id="74b3b-166">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) or by [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) in *Startup.Configure*.</span></span>
::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="74b3b-167">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="74b3b-167">Additional resources</span></span>

* [<span data-ttu-id="74b3b-168">Tipos de valor devuelto de acción del controlador</span><span class="sxs-lookup"><span data-stu-id="74b3b-168">Controller action return types</span></span>](xref:web-api/action-return-types)
* [<span data-ttu-id="74b3b-169">Formateadores personalizados</span><span class="sxs-lookup"><span data-stu-id="74b3b-169">Custom formatters</span></span>](xref:web-api/advanced/custom-formatters)
* [<span data-ttu-id="74b3b-170">Aplicación de formato a datos de respuesta</span><span class="sxs-lookup"><span data-stu-id="74b3b-170">Format response data</span></span>](xref:web-api/advanced/formatting)
* [<span data-ttu-id="74b3b-171">Páginas de ayuda mediante Swagger</span><span class="sxs-lookup"><span data-stu-id="74b3b-171">Help pages using Swagger</span></span>](xref:tutorials/web-api-help-pages-using-swagger)
* [<span data-ttu-id="74b3b-172">Enrutamiento a acciones del controlador</span><span class="sxs-lookup"><span data-stu-id="74b3b-172">Routing to controller actions</span></span>](xref:mvc/controllers/routing)
