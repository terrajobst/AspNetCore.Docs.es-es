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
# <a name="create-web-apis-with-aspnet-core"></a><span data-ttu-id="c612c-103">Creación de API web con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c612c-103">Create web APIs with ASP.NET Core</span></span>

<span data-ttu-id="c612c-104">Por [Scott Addie](https://github.com/scottaddie) y [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="c612c-104">By [Scott Addie](https://github.com/scottaddie) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="c612c-105">ASP.NET Core admite la creación de servicios RESTful, lo que también se conoce como API web, mediante C#.</span><span class="sxs-lookup"><span data-stu-id="c612c-105">ASP.NET Core supports creating RESTful services, also known as web APIs, using C#.</span></span> <span data-ttu-id="c612c-106">Para gestionar las solicitudes, una API web usa controladores.</span><span class="sxs-lookup"><span data-stu-id="c612c-106">To handle requests, a web API uses controllers.</span></span> <span data-ttu-id="c612c-107">Los *controladores* de una API web son clases que se derivan de `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="c612c-107">*Controllers* in a web API are classes that derive from `ControllerBase`.</span></span> <span data-ttu-id="c612c-108">En este artículo se muestra cómo usar controladores para gestionar las solicitudes de API.</span><span class="sxs-lookup"><span data-stu-id="c612c-108">This article shows how to use controllers for handling API requests.</span></span>

<span data-ttu-id="c612c-109">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples).</span><span class="sxs-lookup"><span data-stu-id="c612c-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples).</span></span> <span data-ttu-id="c612c-110">([Método de descarga](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="c612c-110">([How to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="controllerbase-class"></a><span data-ttu-id="c612c-111">Clase ControllerBase</span><span class="sxs-lookup"><span data-stu-id="c612c-111">ControllerBase class</span></span>

<span data-ttu-id="c612c-112">Una API web tiene una o varias clases de controlador que se derivan de <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="c612c-112">A web API has one or more controller classes that derive from <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="c612c-113">Por ejemplo, la plantilla de proyecto de API web crea un controlador de valores:</span><span class="sxs-lookup"><span data-stu-id="c612c-113">For example, the web API project template creates a Values controller:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=3)]

<span data-ttu-id="c612c-114">No cree un controlador de API web mediante la derivación de la clase base <xref:Microsoft.AspNetCore.Mvc.Controller>.</span><span class="sxs-lookup"><span data-stu-id="c612c-114">Don't create a web API controller by deriving from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class.</span></span> <span data-ttu-id="c612c-115">`Controller` se deriva de `ControllerBase` y agrega compatibilidad con vistas, por lo que sirve para gestionar páginas web, no solicitudes de API web.</span><span class="sxs-lookup"><span data-stu-id="c612c-115">`Controller` derives from `ControllerBase` and adds support for views, so it's for handling web pages, not web API requests.</span></span>  <span data-ttu-id="c612c-116">Hay una excepción a esta regla: si tiene pensado usar el mismo controlador tanto para vistas como para API, debe derivarlo de `Controller`.</span><span class="sxs-lookup"><span data-stu-id="c612c-116">There's an exception to this rule: if you plan to use the same controller for both views and APIs, derive it from `Controller`.</span></span>

<span data-ttu-id="c612c-117">La clase `ControllerBase` ofrece muchas propiedades y métodos que son útiles para gestionar solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="c612c-117">The `ControllerBase` class provides many properties and methods that are useful for handling HTTP requests.</span></span> <span data-ttu-id="c612c-118">Por ejemplo, `ControllerBase.CreatedAtAction` devuelve un código de estado 201:</span><span class="sxs-lookup"><span data-stu-id="c612c-118">For example, `ControllerBase.CreatedAtAction` returns a 201 status code:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=8-9)]

 <span data-ttu-id="c612c-119">A continuación se muestran algunos ejemplos más de métodos que proporciona `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="c612c-119">Here are some more examples of methods that `ControllerBase` provides.</span></span>

|<span data-ttu-id="c612c-120">Método</span><span class="sxs-lookup"><span data-stu-id="c612c-120">Method</span></span>  |<span data-ttu-id="c612c-121">Notas</span><span class="sxs-lookup"><span data-stu-id="c612c-121">Notes</span></span>  |
|---------|---------|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*>| <span data-ttu-id="c612c-122">Devuelve el código de estado 400.</span><span class="sxs-lookup"><span data-stu-id="c612c-122">Returns 400 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> |<span data-ttu-id="c612c-123">Devuelve el código de estado 404.</span><span class="sxs-lookup"><span data-stu-id="c612c-123">Returns 404 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.PhysicalFile*>|<span data-ttu-id="c612c-124">Devuelve un archivo.</span><span class="sxs-lookup"><span data-stu-id="c612c-124">Returns a file.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>|<span data-ttu-id="c612c-125">Invoca el [enlace de modelo](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="c612c-125">Invokes [model binding](xref:mvc/models/model-binding).</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryValidateModel*>|<span data-ttu-id="c612c-126">Invoca la [validación de modelos](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="c612c-126">Invokes [model validation](xref:mvc/models/validation).</span></span>|

<span data-ttu-id="c612c-127">Para ver una lista de todos los métodos y propiedades disponibles, consulte <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="c612c-127">For a list of all available methods and properties, see <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span>

## <a name="attributes"></a><span data-ttu-id="c612c-128">Atributos</span><span class="sxs-lookup"><span data-stu-id="c612c-128">Attributes</span></span>

<span data-ttu-id="c612c-129">El espacio de nombres <xref:Microsoft.AspNetCore.Mvc> proporciona atributos que se pueden usar para configurar el comportamiento de los controladores API web y los métodos de acción.</span><span class="sxs-lookup"><span data-stu-id="c612c-129">The <xref:Microsoft.AspNetCore.Mvc> namespace provides attributes that can be used to configure the behavior of web API controllers and action methods.</span></span> <span data-ttu-id="c612c-130">En el ejemplo siguiente se usan atributos para especificar el método HTTP aceptado y los códigos de estado devueltos:</span><span class="sxs-lookup"><span data-stu-id="c612c-130">The following example uses attributes to specify the HTTP method accepted and the status codes returned:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=1-3)]

<span data-ttu-id="c612c-131">Estos son algunos ejemplos más de atributos que están disponibles.</span><span class="sxs-lookup"><span data-stu-id="c612c-131">Here are some more examples of attributes that are available.</span></span>

|<span data-ttu-id="c612c-132">Atributo</span><span class="sxs-lookup"><span data-stu-id="c612c-132">Attribute</span></span>|<span data-ttu-id="c612c-133">Notas</span><span class="sxs-lookup"><span data-stu-id="c612c-133">Notes</span></span>|
|---------|-----|
|<span data-ttu-id="c612c-134">[[Route]](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)</span><span class="sxs-lookup"><span data-stu-id="c612c-134">[[Route]](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)</span></span>      |<span data-ttu-id="c612c-135">Especifica el patrón de dirección URL de un controlador o una acción.</span><span class="sxs-lookup"><span data-stu-id="c612c-135">Specifies URL pattern for a controller or action.</span></span>|
|<span data-ttu-id="c612c-136">[[Bind]](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)</span><span class="sxs-lookup"><span data-stu-id="c612c-136">[[Bind]](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)</span></span>        |<span data-ttu-id="c612c-137">Especifica el prefijo y las propiedades que se incluirán en el enlace de modelo.</span><span class="sxs-lookup"><span data-stu-id="c612c-137">Specifies prefix and properties to include for model binding.</span></span>|
|<span data-ttu-id="c612c-138">[[HttpGet]](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)</span><span class="sxs-lookup"><span data-stu-id="c612c-138">[[HttpGet]](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)</span></span>  |<span data-ttu-id="c612c-139">Identifica una acción que admite el método HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="c612c-139">Identifies an action that supports the HTTP GET method.</span></span>|
|<span data-ttu-id="c612c-140">[[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)</span><span class="sxs-lookup"><span data-stu-id="c612c-140">[[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)</span></span>|<span data-ttu-id="c612c-141">Especifica los tipos de datos que acepta una acción.</span><span class="sxs-lookup"><span data-stu-id="c612c-141">Specifies data types that an action accepts.</span></span>|
|<span data-ttu-id="c612c-142">[[Produces]](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)</span><span class="sxs-lookup"><span data-stu-id="c612c-142">[[Produces]](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)</span></span>|<span data-ttu-id="c612c-143">Especifica los tipos de datos que devuelve una acción.</span><span class="sxs-lookup"><span data-stu-id="c612c-143">Specifies data types that an action returns.</span></span>|

<span data-ttu-id="c612c-144">Para ver una lista que incluye los atributos disponibles, consulte el espacio de nombres <xref:Microsoft.AspNetCore.Mvc>.</span><span class="sxs-lookup"><span data-stu-id="c612c-144">For a list that includes the available attributes, see the <xref:Microsoft.AspNetCore.Mvc> namespace.</span></span>

## <a name="apicontroller-attribute"></a><span data-ttu-id="c612c-145">Atributo ApiController</span><span class="sxs-lookup"><span data-stu-id="c612c-145">ApiController attribute</span></span>

<span data-ttu-id="c612c-146">El atributo [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) puede aplicarse a una clase de controlador para permitir comportamientos específicos de API:</span><span class="sxs-lookup"><span data-stu-id="c612c-146">The [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute can be applied to a controller class to enable API-specific behaviors:</span></span>

* [<span data-ttu-id="c612c-147">Requisito de enrutamiento mediante atributos</span><span class="sxs-lookup"><span data-stu-id="c612c-147">Attribute routing requirement</span></span>](#attribute-routing-requirement)
* [<span data-ttu-id="c612c-148">Respuestas HTTP 400 automáticas</span><span class="sxs-lookup"><span data-stu-id="c612c-148">Automatic HTTP 400 responses</span></span>](#automatic-http-400-responses)
* [<span data-ttu-id="c612c-149">Inferencia de parámetro de origen de enlace</span><span class="sxs-lookup"><span data-stu-id="c612c-149">Binding source parameter inference</span></span>](#binding-source-parameter-inference)
* [<span data-ttu-id="c612c-150">Inferencia de solicitud de varios elementos o datos de formulario</span><span class="sxs-lookup"><span data-stu-id="c612c-150">Multipart/form-data request inference</span></span>](#multipartform-data-request-inference)
* [<span data-ttu-id="c612c-151">Detalles de problemas de los códigos de estado de error</span><span class="sxs-lookup"><span data-stu-id="c612c-151">Problem details for error status codes</span></span>](#problem-details-for-error-status-codes)

<span data-ttu-id="c612c-152">Estas características requieren una [versión de compatibilidad](<xref:mvc/compatibility-version>) de 2.1 o posterior.</span><span class="sxs-lookup"><span data-stu-id="c612c-152">These features require a [compatibility version](<xref:mvc/compatibility-version>) of 2.1 or later.</span></span>

### <a name="apicontroller-on-specific-controllers"></a><span data-ttu-id="c612c-153">ApiController en controladores específicos</span><span class="sxs-lookup"><span data-stu-id="c612c-153">ApiController on specific controllers</span></span>

<span data-ttu-id="c612c-154">El atributo `[ApiController]` puede aplicarse a controladores específicos, como se muestra en el siguiente ejemplo de la plantilla de proyecto:</span><span class="sxs-lookup"><span data-stu-id="c612c-154">The `[ApiController]` attribute can be applied to specific controllers, as in the following example from the project template:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=2)]

### <a name="apicontroller-on-multiple-controllers"></a><span data-ttu-id="c612c-155">ApiController en varios controladores</span><span class="sxs-lookup"><span data-stu-id="c612c-155">ApiController on multiple controllers</span></span>

<span data-ttu-id="c612c-156">Una estrategia para el uso del atributo en más de un controlador consiste en crear una clase personalizada de controlador base anotada con el atributo `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="c612c-156">One approach to using the attribute on more than one controller is to create a custom base controller class annotated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="c612c-157">Este es un ejemplo que muestra una clase base personalizada y un controlador que se deriva de ella:</span><span class="sxs-lookup"><span data-stu-id="c612c-157">Here's an example showing a custom base class and a controller that derives from it:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/MyControllerBase.cs?name=snippet_MyControllerBase)]

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_Inherit)]

### <a name="apicontroller-on-an-assembly"></a><span data-ttu-id="c612c-158">ApiController en un ensamblado</span><span class="sxs-lookup"><span data-stu-id="c612c-158">ApiController on an assembly</span></span>

<span data-ttu-id="c612c-159">Si la [versión de compatibilidad](<xref:mvc/compatibility-version>) está establecida en 2.2 o posterior, el atributo `[ApiController]` se puede aplicar a un ensamblado.</span><span class="sxs-lookup"><span data-stu-id="c612c-159">If [compatibility version](<xref:mvc/compatibility-version>) is set to 2.2 or later, the `[ApiController]` attribute can be applied to an assembly.</span></span> <span data-ttu-id="c612c-160">En este contexto, la anotación aplica el comportamiento de API web a todos los controladores del ensamblado.</span><span class="sxs-lookup"><span data-stu-id="c612c-160">Annotation in this manner applies web API behavior to all controllers in the assembly.</span></span> <span data-ttu-id="c612c-161">No hay ninguna manera de excluir controladores específicos.</span><span class="sxs-lookup"><span data-stu-id="c612c-161">There's no way to opt out for individual controllers.</span></span> <span data-ttu-id="c612c-162">Aplique el atributo de nivel de ensamblado a la clase `Startup`, tal y como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c612c-162">Apply the assembly-level attribute to the `Startup` class as shown in the following example:</span></span>

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

## <a name="attribute-routing-requirement"></a><span data-ttu-id="c612c-163">Requisito de enrutamiento mediante atributos</span><span class="sxs-lookup"><span data-stu-id="c612c-163">Attribute routing requirement</span></span>

<span data-ttu-id="c612c-164">El atributo `ApiController` convierte el enrutamiento de atributos en un requisito.</span><span class="sxs-lookup"><span data-stu-id="c612c-164">The `ApiController` attribute makes attribute routing a requirement.</span></span> <span data-ttu-id="c612c-165">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="c612c-165">For example:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=1)]

<span data-ttu-id="c612c-166">Las acciones no son accesibles mediante [rutas convencionales](xref:mvc/controllers/routing#conventional-routing) definidas por <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> o <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="c612c-166">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.</span></span>

## <a name="automatic-http-400-responses"></a><span data-ttu-id="c612c-167">Respuestas HTTP 400 automáticas</span><span class="sxs-lookup"><span data-stu-id="c612c-167">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="c612c-168">El atributo `ApiController` hace que los errores de validación de un modelo desencadenen automáticamente una respuesta HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="c612c-168">The `ApiController` attribute makes model validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="c612c-169">Por lo tanto, el siguiente código no es necesario en un método de acción:</span><span class="sxs-lookup"><span data-stu-id="c612c-169">Consequently, the following code is unnecessary in an action method:</span></span>

```csharp
if (!ModelState.IsValid)
{
    return BadRequest(ModelState);
}
```

### <a name="default-badrequest-response"></a><span data-ttu-id="c612c-170">Respuesta BadRequest predeterminada</span><span class="sxs-lookup"><span data-stu-id="c612c-170">Default BadRequest response</span></span> 

<span data-ttu-id="c612c-171">Con una versión de compatibilidad de 2.2 o posterior, el tipo de respuesta predeterminado para las respuestas HTTP 400 es <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="c612c-171">With a compatibility version of 2.2 or later, the default response type for HTTP 400 responses is <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="c612c-172">El tipo `ValidationProblemDetails` cumple los requisitos de la [especificación RFC 7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="c612c-172">The `ValidationProblemDetails` type complies with the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span>

<span data-ttu-id="c612c-173">Para cambiar la respuesta predeterminada por <xref:Microsoft.AspNetCore.Mvc.SerializableError>, establezca la propiedad `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` como `true` en `Startup.ConfigureServices`, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c612c-173">To change the default response to <xref:Microsoft.AspNetCore.Mvc.SerializableError>, set the `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` property to `true` in `Startup.ConfigureServices`, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,9)]

### <a name="customize-badrequest-response"></a><span data-ttu-id="c612c-174">Respuesta BadRequest personalizada</span><span class="sxs-lookup"><span data-stu-id="c612c-174">Customize BadRequest response</span></span>

<span data-ttu-id="c612c-175">Para personalizar la respuesta que es consecuencia de un error de validación, use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory>.</span><span class="sxs-lookup"><span data-stu-id="c612c-175">To customize the response that results from a validation error, use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory>.</span></span> <span data-ttu-id="c612c-176">Agregue el código resaltado siguiente después de `services.AddMvc().SetCompatibilityVersion`:</span><span class="sxs-lookup"><span data-stu-id="c612c-176">Add the following highlighted code after `services.AddMvc().SetCompatibilityVersion`:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureBadRequestResponse&highlight=3-20)]

### <a name="log-automatic-400-responses"></a><span data-ttu-id="c612c-177">Registro de respuestas 400 automáticas</span><span class="sxs-lookup"><span data-stu-id="c612c-177">Log automatic 400 responses</span></span>

<span data-ttu-id="c612c-178">Consulte [cómo registrar respuestas 400 automáticas sobre errores de validación de modelos (aspnet/AspNetCore.Docs #12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157).</span><span class="sxs-lookup"><span data-stu-id="c612c-178">See [How to log automatic 400 responses on model validation errors (aspnet/AspNetCore.Docs #12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157).</span></span>

### <a name="disable-automatic-400"></a><span data-ttu-id="c612c-179">Deshabilitación de respuestas 400 automáticas</span><span class="sxs-lookup"><span data-stu-id="c612c-179">Disable automatic 400</span></span>

<span data-ttu-id="c612c-180">Para deshabilitar el comportamiento 400 automático, establezca la propiedad <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> en `true`.</span><span class="sxs-lookup"><span data-stu-id="c612c-180">To disable the automatic 400 behavior, set the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property to `true`.</span></span> <span data-ttu-id="c612c-181">Agregue el código resaltado siguiente en `Startup.ConfigureServices` después de `services.AddMvc().SetCompatibilityVersion`:</span><span class="sxs-lookup"><span data-stu-id="c612c-181">Add the following highlighted code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion`:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,7)]

## <a name="binding-source-parameter-inference"></a><span data-ttu-id="c612c-182">Inferencia de parámetro de origen de enlace</span><span class="sxs-lookup"><span data-stu-id="c612c-182">Binding source parameter inference</span></span>

<span data-ttu-id="c612c-183">Un atributo de origen de enlace define la ubicación del valor del parámetro de una acción.</span><span class="sxs-lookup"><span data-stu-id="c612c-183">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="c612c-184">Existen los atributos de origen de enlace siguientes:</span><span class="sxs-lookup"><span data-stu-id="c612c-184">The following binding source attributes exist:</span></span>

|<span data-ttu-id="c612c-185">Atributo</span><span class="sxs-lookup"><span data-stu-id="c612c-185">Attribute</span></span>|<span data-ttu-id="c612c-186">Origen de enlace</span><span class="sxs-lookup"><span data-stu-id="c612c-186">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="c612c-187">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)</span><span class="sxs-lookup"><span data-stu-id="c612c-187">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)</span></span>     | <span data-ttu-id="c612c-188">Cuerpo de la solicitud</span><span class="sxs-lookup"><span data-stu-id="c612c-188">Request body</span></span> |
|<span data-ttu-id="c612c-189">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)</span><span class="sxs-lookup"><span data-stu-id="c612c-189">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)</span></span>     | <span data-ttu-id="c612c-190">Datos del formulario en el cuerpo de la solicitud</span><span class="sxs-lookup"><span data-stu-id="c612c-190">Form data in the request body</span></span> |
|<span data-ttu-id="c612c-191">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)</span><span class="sxs-lookup"><span data-stu-id="c612c-191">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)</span></span> | <span data-ttu-id="c612c-192">Encabezado de la solicitud</span><span class="sxs-lookup"><span data-stu-id="c612c-192">Request header</span></span> |
|<span data-ttu-id="c612c-193">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)</span><span class="sxs-lookup"><span data-stu-id="c612c-193">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)</span></span>   | <span data-ttu-id="c612c-194">Parámetro de la cadena de consulta de la solicitud</span><span class="sxs-lookup"><span data-stu-id="c612c-194">Request query string parameter</span></span> |
|<span data-ttu-id="c612c-195">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)</span><span class="sxs-lookup"><span data-stu-id="c612c-195">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)</span></span>   | <span data-ttu-id="c612c-196">Datos de ruta de la solicitud actual</span><span class="sxs-lookup"><span data-stu-id="c612c-196">Route data from the current request</span></span> |
|<span data-ttu-id="c612c-197">[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)</span><span class="sxs-lookup"><span data-stu-id="c612c-197">[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)</span></span> | <span data-ttu-id="c612c-198">Servicio de solicitud insertado como parámetro de acción</span><span class="sxs-lookup"><span data-stu-id="c612c-198">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="c612c-199">No use `[FromRoute]` si los valores pueden contener `%2f` (es decir, `/`).</span><span class="sxs-lookup"><span data-stu-id="c612c-199">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="c612c-200">`%2f` no incluirá el carácter sin escape `/`.</span><span class="sxs-lookup"><span data-stu-id="c612c-200">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="c612c-201">Use `[FromQuery]` si el valor puede contener `%2f`.</span><span class="sxs-lookup"><span data-stu-id="c612c-201">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="c612c-202">Sin el atributo `[ApiController]` o los atributos de origen de enlace, como `[FromQuery]`, el entorno de tiempo de ejecución de ASP.NET Core intenta usar el enlazador de modelos de objetos complejos.</span><span class="sxs-lookup"><span data-stu-id="c612c-202">Without the `[ApiController]` attribute or binding source attributes like `[FromQuery]`, the ASP.NET Core runtime attempts to use the complex object model binder.</span></span> <span data-ttu-id="c612c-203">El enlazador de modelos de objetos complejos extrae los datos de los proveedores de valor en un orden definido.</span><span class="sxs-lookup"><span data-stu-id="c612c-203">The complex object model binder pulls data from value providers in a defined order.</span></span>

<span data-ttu-id="c612c-204">En el ejemplo siguiente, el atributo `[FromQuery]` indica que el valor del parámetro `discontinuedOnly` se proporciona en la cadena de consulta de la dirección URL de la solicitud:</span><span class="sxs-lookup"><span data-stu-id="c612c-204">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="c612c-205">El atributo `[ApiController]` aplica reglas de inferencia a los orígenes de datos predeterminados de los parámetros de acción.</span><span class="sxs-lookup"><span data-stu-id="c612c-205">The `[ApiController]` attribute applies inference rules for the default data sources of action parameters.</span></span> <span data-ttu-id="c612c-206">Estas reglas aplican atributos a los parámetros de acción, lo que ahorra la necesidad de identificar los orígenes de enlace manualmente.</span><span class="sxs-lookup"><span data-stu-id="c612c-206">These rules save you from having to identify binding sources manually by applying attributes to the action parameters.</span></span> <span data-ttu-id="c612c-207">Las reglas de inferencia de orígenes de enlace se comportan de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="c612c-207">The binding source inference rules behave as follows:</span></span>

* <span data-ttu-id="c612c-208">`[FromBody]` se infiere para parámetros de tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="c612c-208">`[FromBody]` is inferred for complex type parameters.</span></span> <span data-ttu-id="c612c-209">La excepción a `[FromBody]` esta regla es cualquier tipo integrado complejo que tenga un significado especial, como <xref:Microsoft.AspNetCore.Http.IFormCollection> y <xref:System.Threading.CancellationToken>.</span><span class="sxs-lookup"><span data-stu-id="c612c-209">An exception to the `[FromBody]` inference rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="c612c-210">El código de inferencia del origen de enlace omite esos tipos especiales.</span><span class="sxs-lookup"><span data-stu-id="c612c-210">The binding source inference code ignores those special types.</span></span> 
* <span data-ttu-id="c612c-211">`[FromForm]` se infiere para los parámetros de acción de tipo <xref:Microsoft.AspNetCore.Http.IFormFile> y <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span><span class="sxs-lookup"><span data-stu-id="c612c-211">`[FromForm]` is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="c612c-212">No se infiere para los tipos simples o definidos por el usuario.</span><span class="sxs-lookup"><span data-stu-id="c612c-212">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="c612c-213">`[FromRoute]` se infiere para cualquier nombre de parámetro de acción que coincida con un parámetro de la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="c612c-213">`[FromRoute]` is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="c612c-214">Si varias rutas coinciden con un parámetro de acción, cualquier valor de ruta se considera `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="c612c-214">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="c612c-215">`[FromQuery]` se infiere para cualquier otro parámetro de acción.</span><span class="sxs-lookup"><span data-stu-id="c612c-215">`[FromQuery]` is inferred for any other action parameters.</span></span>

### <a name="frombody-inference-notes"></a><span data-ttu-id="c612c-216">Notas de la inferencia de FromBody</span><span class="sxs-lookup"><span data-stu-id="c612c-216">FromBody inference notes</span></span>

<span data-ttu-id="c612c-217">En el caso de los tipos simples, como `string` y `int`, `[FromBody]` no se infiere.</span><span class="sxs-lookup"><span data-stu-id="c612c-217">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="c612c-218">Así pues, para los tipos simples, en los casos en los que quiera utilizar dicha funcionalidad, conviene usar el atributo `[FromBody]`.</span><span class="sxs-lookup"><span data-stu-id="c612c-218">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is needed.</span></span>

<span data-ttu-id="c612c-219">Cuando una acción tiene más de un parámetro enlazado desde el cuerpo de solicitud, se produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="c612c-219">When an action has more than one parameter bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="c612c-220">Por ejemplo, todas las firmas de acción siguientes provocan una excepción:</span><span class="sxs-lookup"><span data-stu-id="c612c-220">For example, all of the following action method signatures cause an exception:</span></span>

* <span data-ttu-id="c612c-221">`[FromBody]` se infiere en ambos porque son tipos complejos.</span><span class="sxs-lookup"><span data-stu-id="c612c-221">`[FromBody]` inferred on both because they're complex types.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action1(Product product, Order order)
  ```

* <span data-ttu-id="c612c-222">El atributo `[FromBody]` en uno se infiere en el otro porque es un tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="c612c-222">`[FromBody]` attribute on one, inferred on the other because it's a complex type.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action2(Product product, [FromBody] Order order)
  ```

* <span data-ttu-id="c612c-223">El atributo `[FromBody]` se infiere en ambos.</span><span class="sxs-lookup"><span data-stu-id="c612c-223">`[FromBody]` attribute on both.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action3([FromBody] Product product, [FromBody] Order order)
  ```

> [!NOTE]
> <span data-ttu-id="c612c-224">En ASP.NET Core 2.1, los parámetros de tipo de colección, como listas y matrices, se infieren incorrectamente como `[FromQuery]`.</span><span class="sxs-lookup"><span data-stu-id="c612c-224">In ASP.NET Core 2.1, collection type parameters such as lists and arrays are incorrectly inferred as `[FromQuery]`.</span></span> <span data-ttu-id="c612c-225">El atributo `[FromBody]` debe usarse con estos parámetros si van a enlazarse desde el cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="c612c-225">The `[FromBody]` attribute should be used for these parameters if they are to be bound from the request body.</span></span> <span data-ttu-id="c612c-226">Este comportamiento se ha corregido en ASP.NET Core 2.2 o posterior, donde se infieren los parámetros de tipo de colección para enlazarse desde el cuerpo de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="c612c-226">This behavior is corrected in ASP.NET Core 2.2 or later, where collection type parameters are inferred to be bound from the body by default.</span></span>

### <a name="disable-inference-rules"></a><span data-ttu-id="c612c-227">Deshabilitación de las reglas de inferencia</span><span class="sxs-lookup"><span data-stu-id="c612c-227">Disable inference rules</span></span>

<span data-ttu-id="c612c-228">Para deshabilitar la inferencia del origen de enlace, establezca <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> en `true`.</span><span class="sxs-lookup"><span data-stu-id="c612c-228">To disable binding source inference, set <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> to `true`.</span></span> <span data-ttu-id="c612c-229">Agregue el código siguiente en `Startup.ConfigureServices` tras `services.AddMvc().SetCompatibilityVersion`:</span><span class="sxs-lookup"><span data-stu-id="c612c-229">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion`:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,6)]

## <a name="multipartform-data-request-inference"></a><span data-ttu-id="c612c-230">Inferencia de solicitud de varios elementos o datos de formulario</span><span class="sxs-lookup"><span data-stu-id="c612c-230">Multipart/form-data request inference</span></span>

<span data-ttu-id="c612c-231">El atributo `[ApiController]` aplica una regla de inferencia cuando se anota un parámetro de acción con el atributo [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute): se infiere el tipo de contenido de solicitud `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="c612c-231">The `[ApiController]` attribute applies an inference rule when an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute: the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="c612c-232">Para deshabilitar el comportamiento predeterminado, establezca <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> como `true` en `Startup.ConfigureServices`, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c612c-232">To disable the default behavior, set <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters>  to `true` in `Startup.ConfigureServices`, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,5)]

## <a name="problem-details-for-error-status-codes"></a><span data-ttu-id="c612c-233">Detalles de problemas de los códigos de estado de error</span><span class="sxs-lookup"><span data-stu-id="c612c-233">Problem details for error status codes</span></span>

<span data-ttu-id="c612c-234">Cuando la versión de compatibilidad es 2.2 o posterior, MVC transforma un resultado de error (un resultado con el código de estado 400 o superior) en un resultado con <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="c612c-234">When the compatibility version is 2.2 or later, MVC transforms an error result (a result with status code 400 or higher) to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span> <span data-ttu-id="c612c-235">El tipo `ProblemDetails` se basa en la [especificación RFC 7807](https://tools.ietf.org/html/rfc7807) para proporcionar detalles de error de lectura mecánica en una respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="c612c-235">The `ProblemDetails` type is based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807) for providing machine-readable error details in an HTTP response.</span></span>

<span data-ttu-id="c612c-236">Observe el código siguiente en una acción de controlador:</span><span class="sxs-lookup"><span data-stu-id="c612c-236">Consider the following code in a controller action:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_ProblemDetailsStatusCode)]

<span data-ttu-id="c612c-237">La respuesta HTTP para `NotFound` tiene un código de estado 404 con un cuerpo `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="c612c-237">The HTTP response for `NotFound` has a 404 status code with a `ProblemDetails` body.</span></span> <span data-ttu-id="c612c-238">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="c612c-238">For example:</span></span>

```json
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

### <a name="customize-problemdetails-response"></a><span data-ttu-id="c612c-239">Respuesta ProblemDetails personalizada</span><span class="sxs-lookup"><span data-stu-id="c612c-239">Customize ProblemDetails response</span></span>

<span data-ttu-id="c612c-240">Use la propiedad `ClientErrorMapping` para configurar el contenido de la respuesta `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="c612c-240">Use the `ClientErrorMapping` property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="c612c-241">Por ejemplo, el código siguiente permite actualizar la propiedad `type` para respuestas 404:</span><span class="sxs-lookup"><span data-stu-id="c612c-241">For example, the following code updates the `type` property for 404 responses:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=10-11)]

### <a name="disable-problemdetails-response"></a><span data-ttu-id="c612c-242">Deshabilitación de la respuesta ProblemDetails</span><span class="sxs-lookup"><span data-stu-id="c612c-242">Disable ProblemDetails response</span></span>

<span data-ttu-id="c612c-243">La creación automática de `ProblemDetails` está deshabilitada cuando la propiedad `SuppressMapClientErrors` está establecida en `true`.</span><span class="sxs-lookup"><span data-stu-id="c612c-243">The automatic creation of `ProblemDetails` is disabled when the `SuppressMapClientErrors` property is set to `true`.</span></span> <span data-ttu-id="c612c-244">Agregue el siguiente código en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c612c-244">Add the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,8)]

## <a name="additional-resources"></a><span data-ttu-id="c612c-245">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="c612c-245">Additional resources</span></span> 

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
