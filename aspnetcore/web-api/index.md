---
title: Creación de API web con ASP.NET Core
author: scottaddie
description: Conozca los aspectos básicos de la creación de una API web en ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 11/22/2019
uid: web-api/index
ms.openlocfilehash: 5ef8b4d012f4ed90339ffea191612e4dc365d958
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880526"
---
# <a name="create-web-apis-with-aspnet-core"></a><span data-ttu-id="16f1c-103">Creación de API web con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="16f1c-103">Create web APIs with ASP.NET Core</span></span>

<span data-ttu-id="16f1c-104">Por [Scott Addie](https://github.com/scottaddie) y [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="16f1c-104">By [Scott Addie](https://github.com/scottaddie) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="16f1c-105">ASP.NET Core admite la creación de servicios RESTful, lo que también se conoce como API web, mediante C#.</span><span class="sxs-lookup"><span data-stu-id="16f1c-105">ASP.NET Core supports creating RESTful services, also known as web APIs, using C#.</span></span> <span data-ttu-id="16f1c-106">Para gestionar las solicitudes, una API web usa controladores.</span><span class="sxs-lookup"><span data-stu-id="16f1c-106">To handle requests, a web API uses controllers.</span></span> <span data-ttu-id="16f1c-107">Los *controladores* de una API web son clases que se derivan de `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="16f1c-107">*Controllers* in a web API are classes that derive from `ControllerBase`.</span></span> <span data-ttu-id="16f1c-108">En este artículo se muestra cómo usar controladores para gestionar las solicitudes de API web.</span><span class="sxs-lookup"><span data-stu-id="16f1c-108">This article shows how to use controllers for handling web API requests.</span></span>

<span data-ttu-id="16f1c-109">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples).</span><span class="sxs-lookup"><span data-stu-id="16f1c-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples).</span></span> <span data-ttu-id="16f1c-110">([Método de descarga](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="16f1c-110">([How to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="controllerbase-class"></a><span data-ttu-id="16f1c-111">Clase ControllerBase</span><span class="sxs-lookup"><span data-stu-id="16f1c-111">ControllerBase class</span></span>

<span data-ttu-id="16f1c-112">Una API web consta de una o varias clases de controlador que se derivan de <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="16f1c-112">A web API consists of one or more controller classes that derive from <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="16f1c-113">La plantilla de proyecto de API web proporciona un controlador de inicio:</span><span class="sxs-lookup"><span data-stu-id="16f1c-113">The web API project template provides a starter controller:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=3)]

::: moniker-end

<span data-ttu-id="16f1c-114">No cree un controlador de API web mediante la derivación de la clase <xref:Microsoft.AspNetCore.Mvc.Controller>.</span><span class="sxs-lookup"><span data-stu-id="16f1c-114">Don't create a web API controller by deriving from the <xref:Microsoft.AspNetCore.Mvc.Controller> class.</span></span> <span data-ttu-id="16f1c-115">`Controller` se deriva de `ControllerBase` y agrega compatibilidad con vistas, por lo que sirve para gestionar páginas web, no solicitudes de API web.</span><span class="sxs-lookup"><span data-stu-id="16f1c-115">`Controller` derives from `ControllerBase` and adds support for views, so it's for handling web pages, not web API requests.</span></span> <span data-ttu-id="16f1c-116">Hay una excepción a esta regla: si tiene pensado usar el mismo controlador tanto para las vistas como para las API web, debe derivarlo de `Controller`.</span><span class="sxs-lookup"><span data-stu-id="16f1c-116">There's an exception to this rule: if you plan to use the same controller for both views and web APIs, derive it from `Controller`.</span></span>

<span data-ttu-id="16f1c-117">La clase `ControllerBase` ofrece muchas propiedades y métodos que son útiles para gestionar solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="16f1c-117">The `ControllerBase` class provides many properties and methods that are useful for handling HTTP requests.</span></span> <span data-ttu-id="16f1c-118">Por ejemplo, `ControllerBase.CreatedAtAction` devuelve un código de estado 201:</span><span class="sxs-lookup"><span data-stu-id="16f1c-118">For example, `ControllerBase.CreatedAtAction` returns a 201 status code:</span></span>

[!code-csharp[](index/samples/2.x/2.2/Controllers/PetsController.cs?name=snippet_400And201&highlight=10)]

<span data-ttu-id="16f1c-119">A continuación se muestran algunos ejemplos más de métodos que proporciona `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="16f1c-119">Here are some more examples of methods that `ControllerBase` provides.</span></span>

|<span data-ttu-id="16f1c-120">Método</span><span class="sxs-lookup"><span data-stu-id="16f1c-120">Method</span></span>   |<span data-ttu-id="16f1c-121">Notas</span><span class="sxs-lookup"><span data-stu-id="16f1c-121">Notes</span></span>    |
|---------|---------|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest%2A>| <span data-ttu-id="16f1c-122">Devuelve el código de estado 400.</span><span class="sxs-lookup"><span data-stu-id="16f1c-122">Returns 400 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound%2A>|<span data-ttu-id="16f1c-123">Devuelve el código de estado 404.</span><span class="sxs-lookup"><span data-stu-id="16f1c-123">Returns 404 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.PhysicalFile%2A>|<span data-ttu-id="16f1c-124">Devuelve un archivo.</span><span class="sxs-lookup"><span data-stu-id="16f1c-124">Returns a file.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync%2A>|<span data-ttu-id="16f1c-125">Invoca el [enlace de modelo](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="16f1c-125">Invokes [model binding](xref:mvc/models/model-binding).</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryValidateModel%2A>|<span data-ttu-id="16f1c-126">Invoca la [validación de modelos](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="16f1c-126">Invokes [model validation](xref:mvc/models/validation).</span></span>|

<span data-ttu-id="16f1c-127">Para ver una lista de todos los métodos y propiedades disponibles, consulte <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="16f1c-127">For a list of all available methods and properties, see <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span>

## <a name="attributes"></a><span data-ttu-id="16f1c-128">Atributos</span><span class="sxs-lookup"><span data-stu-id="16f1c-128">Attributes</span></span>

<span data-ttu-id="16f1c-129">El espacio de nombres <xref:Microsoft.AspNetCore.Mvc> proporciona atributos que se pueden usar para configurar el comportamiento de los controladores API web y los métodos de acción.</span><span class="sxs-lookup"><span data-stu-id="16f1c-129">The <xref:Microsoft.AspNetCore.Mvc> namespace provides attributes that can be used to configure the behavior of web API controllers and action methods.</span></span> <span data-ttu-id="16f1c-130">En el ejemplo siguiente se usan atributos para especificar el verbo de acción HTTP admitido y cualquier código de estado HTTP conocido que se pueda devolver:</span><span class="sxs-lookup"><span data-stu-id="16f1c-130">The following example uses attributes to specify the supported HTTP action verb and any known HTTP status codes that could be returned:</span></span>

[!code-csharp[](index/samples/2.x/2.2/Controllers/PetsController.cs?name=snippet_400And201&highlight=1-3)]

<span data-ttu-id="16f1c-131">Estos son algunos ejemplos más de atributos que están disponibles.</span><span class="sxs-lookup"><span data-stu-id="16f1c-131">Here are some more examples of attributes that are available.</span></span>

|<span data-ttu-id="16f1c-132">Atributo</span><span class="sxs-lookup"><span data-stu-id="16f1c-132">Attribute</span></span>|<span data-ttu-id="16f1c-133">Notas</span><span class="sxs-lookup"><span data-stu-id="16f1c-133">Notes</span></span>|
|---------|-----|
|[`[Route]`](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)      |<span data-ttu-id="16f1c-134">Especifica el patrón de dirección URL de un controlador o una acción.</span><span class="sxs-lookup"><span data-stu-id="16f1c-134">Specifies URL pattern for a controller or action.</span></span>|
|[`[Bind]`](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)        |<span data-ttu-id="16f1c-135">Especifica el prefijo y las propiedades que se incluirán en el enlace de modelo.</span><span class="sxs-lookup"><span data-stu-id="16f1c-135">Specifies prefix and properties to include for model binding.</span></span>|
|[`[HttpGet]`](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)  |<span data-ttu-id="16f1c-136">Identifica una acción que admite el verbo de acción GET HTTP.</span><span class="sxs-lookup"><span data-stu-id="16f1c-136">Identifies an action that supports the HTTP GET action verb.</span></span>|
|[`[Consumes]`](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)|<span data-ttu-id="16f1c-137">Especifica los tipos de datos que acepta una acción.</span><span class="sxs-lookup"><span data-stu-id="16f1c-137">Specifies data types that an action accepts.</span></span>|
|[`[Produces]`](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)|<span data-ttu-id="16f1c-138">Especifica los tipos de datos que devuelve una acción.</span><span class="sxs-lookup"><span data-stu-id="16f1c-138">Specifies data types that an action returns.</span></span>|

<span data-ttu-id="16f1c-139">Para ver una lista que incluye los atributos disponibles, consulte el espacio de nombres <xref:Microsoft.AspNetCore.Mvc>.</span><span class="sxs-lookup"><span data-stu-id="16f1c-139">For a list that includes the available attributes, see the <xref:Microsoft.AspNetCore.Mvc> namespace.</span></span>

## <a name="apicontroller-attribute"></a><span data-ttu-id="16f1c-140">Atributo ApiController</span><span class="sxs-lookup"><span data-stu-id="16f1c-140">ApiController attribute</span></span>

<span data-ttu-id="16f1c-141">El atributo [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) se puede aplicar a una clase de controlador para permitir los siguientes comportamientos específicos de la API:</span><span class="sxs-lookup"><span data-stu-id="16f1c-141">The [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute can be applied to a controller class to enable the following opinionated, API-specific behaviors:</span></span>

* [<span data-ttu-id="16f1c-142">Requisito de enrutamiento mediante atributos</span><span class="sxs-lookup"><span data-stu-id="16f1c-142">Attribute routing requirement</span></span>](#attribute-routing-requirement)
* [<span data-ttu-id="16f1c-143">Respuestas HTTP 400 automáticas</span><span class="sxs-lookup"><span data-stu-id="16f1c-143">Automatic HTTP 400 responses</span></span>](#automatic-http-400-responses)
* [<span data-ttu-id="16f1c-144">Inferencia de parámetro de origen de enlace</span><span class="sxs-lookup"><span data-stu-id="16f1c-144">Binding source parameter inference</span></span>](#binding-source-parameter-inference)
* [<span data-ttu-id="16f1c-145">Inferencia de solicitud de varios elementos o datos de formulario</span><span class="sxs-lookup"><span data-stu-id="16f1c-145">Multipart/form-data request inference</span></span>](#multipartform-data-request-inference)
* [<span data-ttu-id="16f1c-146">Detalles de problemas de los códigos de estado de error</span><span class="sxs-lookup"><span data-stu-id="16f1c-146">Problem details for error status codes</span></span>](#problem-details-for-error-status-codes)

<span data-ttu-id="16f1c-147">Estas características requieren una [versión de compatibilidad](xref:mvc/compatibility-version) de 2.1 o posterior.</span><span class="sxs-lookup"><span data-stu-id="16f1c-147">These features require a [compatibility version](xref:mvc/compatibility-version) of 2.1 or later.</span></span>

### <a name="attribute-on-specific-controllers"></a><span data-ttu-id="16f1c-148">Atributo en controladores específicos</span><span class="sxs-lookup"><span data-stu-id="16f1c-148">Attribute on specific controllers</span></span>

<span data-ttu-id="16f1c-149">El atributo `[ApiController]` puede aplicarse a controladores específicos, como se muestra en el siguiente ejemplo de la plantilla de proyecto:</span><span class="sxs-lookup"><span data-stu-id="16f1c-149">The `[ApiController]` attribute can be applied to specific controllers, as in the following example from the project template:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=2])]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=2)]

::: moniker-end

### <a name="attribute-on-multiple-controllers"></a><span data-ttu-id="16f1c-150">Atributo en varios controladores</span><span class="sxs-lookup"><span data-stu-id="16f1c-150">Attribute on multiple controllers</span></span>

<span data-ttu-id="16f1c-151">Una estrategia para el uso del atributo en más de un controlador consiste en crear una clase personalizada de controlador base anotada con el atributo `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="16f1c-151">One approach to using the attribute on more than one controller is to create a custom base controller class annotated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="16f1c-152">En el siguiente ejemplo se muestra una clase base personalizada y un controlador que se deriva de ella:</span><span class="sxs-lookup"><span data-stu-id="16f1c-152">The following example shows a custom base class and a controller that derives from it:</span></span>

[!code-csharp[](index/samples/2.x/2.2/Controllers/MyControllerBase.cs?name=snippet_MyControllerBase)]

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/PetsController.cs?name=snippet_Inherit)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Controllers/PetsController.cs?name=snippet_Inherit)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="attribute-on-an-assembly"></a><span data-ttu-id="16f1c-153">Atributo en un ensamblado</span><span class="sxs-lookup"><span data-stu-id="16f1c-153">Attribute on an assembly</span></span>

<span data-ttu-id="16f1c-154">Si la [versión de compatibilidad](xref:mvc/compatibility-version) está establecida en 2.2 o posterior, el atributo `[ApiController]` se puede aplicar a un ensamblado.</span><span class="sxs-lookup"><span data-stu-id="16f1c-154">If [compatibility version](xref:mvc/compatibility-version) is set to 2.2 or later, the `[ApiController]` attribute can be applied to an assembly.</span></span> <span data-ttu-id="16f1c-155">En este contexto, la anotación aplica el comportamiento de API web a todos los controladores del ensamblado.</span><span class="sxs-lookup"><span data-stu-id="16f1c-155">Annotation in this manner applies web API behavior to all controllers in the assembly.</span></span> <span data-ttu-id="16f1c-156">No hay ninguna manera de excluir controladores específicos.</span><span class="sxs-lookup"><span data-stu-id="16f1c-156">There's no way to opt out for individual controllers.</span></span> <span data-ttu-id="16f1c-157">Aplique el atributo de nivel de ensamblado a la declaración del espacio de nombres de alrededor de la clase `Startup`:</span><span class="sxs-lookup"><span data-stu-id="16f1c-157">Apply the assembly-level attribute to the namespace declaration surrounding the `Startup` class:</span></span>

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

::: moniker-end

## <a name="attribute-routing-requirement"></a><span data-ttu-id="16f1c-158">Requisito de enrutamiento mediante atributos</span><span class="sxs-lookup"><span data-stu-id="16f1c-158">Attribute routing requirement</span></span>

<span data-ttu-id="16f1c-159">El atributo `[ApiController]` convierte el enrutamiento de atributos en un requisito.</span><span class="sxs-lookup"><span data-stu-id="16f1c-159">The `[ApiController]` attribute makes attribute routing a requirement.</span></span> <span data-ttu-id="16f1c-160">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="16f1c-160">For example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="16f1c-161">No es posible acceder a las acciones mediante [rutas convencionales](xref:mvc/controllers/routing#conventional-routing) que hayan definido `UseEndpoints`, <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc%2A> o <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute%2A> en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="16f1c-161">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined by `UseEndpoints`, <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc%2A>, or <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute%2A> in `Startup.Configure`.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="16f1c-162">Las acciones no son accesibles mediante [rutas convencionales](xref:mvc/controllers/routing#conventional-routing) definidas por <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc%2A> o <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute%2A> en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="16f1c-162">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc%2A> or <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute%2A> in `Startup.Configure`.</span></span>

::: moniker-end

## <a name="automatic-http-400-responses"></a><span data-ttu-id="16f1c-163">Respuestas HTTP 400 automáticas</span><span class="sxs-lookup"><span data-stu-id="16f1c-163">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="16f1c-164">El atributo `[ApiController]` hace que los errores de validación de un modelo desencadenen automáticamente una respuesta HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="16f1c-164">The `[ApiController]` attribute makes model validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="16f1c-165">Por lo tanto, el siguiente código no es necesario en un método de acción:</span><span class="sxs-lookup"><span data-stu-id="16f1c-165">Consequently, the following code is unnecessary in an action method:</span></span>

```csharp
if (!ModelState.IsValid)
{
    return BadRequest(ModelState);
}
```

<span data-ttu-id="16f1c-166">ASP.NET Core MVC usa el filtro de acciones <xref:Microsoft.AspNetCore.Mvc.Infrastructure.ModelStateInvalidFilter> para realizar la comprobación anterior.</span><span class="sxs-lookup"><span data-stu-id="16f1c-166">ASP.NET Core MVC uses the <xref:Microsoft.AspNetCore.Mvc.Infrastructure.ModelStateInvalidFilter> action filter to do the preceding check.</span></span>

### <a name="default-badrequest-response"></a><span data-ttu-id="16f1c-167">Respuesta BadRequest predeterminada</span><span class="sxs-lookup"><span data-stu-id="16f1c-167">Default BadRequest response</span></span>

<span data-ttu-id="16f1c-168">Con una versión de compatibilidad de 2.1, el tipo de respuesta predeterminado para una respuesta HTTP 400 es <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span><span class="sxs-lookup"><span data-stu-id="16f1c-168">With a compatibility version of 2.1, the default response type for an HTTP 400 response is <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span></span> <span data-ttu-id="16f1c-169">El siguiente cuerpo de solicitud es un ejemplo del tipo serializado:</span><span class="sxs-lookup"><span data-stu-id="16f1c-169">The following request body is an example of the serialized type:</span></span>

```json
{
  "": [
    "A non-empty request body is required."
  ]
}
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="16f1c-170">Con una versión de compatibilidad de 2.2 o posterior, el tipo de respuesta predeterminado para una respuesta HTTP 400 es <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="16f1c-170">With a compatibility version of 2.2 or later, the default response type for an HTTP 400 response is <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="16f1c-171">El siguiente cuerpo de solicitud es un ejemplo del tipo serializado:</span><span class="sxs-lookup"><span data-stu-id="16f1c-171">The following request body is an example of the serialized type:</span></span>

```json
{
  "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",
  "title": "One or more validation errors occurred.",
  "status": 400,
  "traceId": "|7fb5e16a-4c8f23bbfc974667.",
  "errors": {
    "": [
      "A non-empty request body is required."
    ]
  }
}
```

<span data-ttu-id="16f1c-172">Tipo `ValidationProblemDetails`:</span><span class="sxs-lookup"><span data-stu-id="16f1c-172">The `ValidationProblemDetails` type:</span></span>

* <span data-ttu-id="16f1c-173">Proporciona un formato de lectura mecánica para especificar errores en las respuestas de la API web.</span><span class="sxs-lookup"><span data-stu-id="16f1c-173">Provides a machine-readable format for specifying errors in web API responses.</span></span>
* <span data-ttu-id="16f1c-174">Cumple los requisitos de la [especificación RFC 7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="16f1c-174">Complies with the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span>

::: moniker-end

### <a name="log-automatic-400-responses"></a><span data-ttu-id="16f1c-175">Registro de respuestas 400 automáticas</span><span class="sxs-lookup"><span data-stu-id="16f1c-175">Log automatic 400 responses</span></span>

<span data-ttu-id="16f1c-176">Consulte [cómo registrar respuestas 400 automáticas sobre errores de validación de modelos (aspnet/AspNetCore.Docs #12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157).</span><span class="sxs-lookup"><span data-stu-id="16f1c-176">See [How to log automatic 400 responses on model validation errors (aspnet/AspNetCore.Docs #12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157).</span></span>

### <a name="disable-automatic-400-response"></a><span data-ttu-id="16f1c-177">Deshabilitación de las respuestas 400 automáticas</span><span class="sxs-lookup"><span data-stu-id="16f1c-177">Disable automatic 400 response</span></span>

<span data-ttu-id="16f1c-178">Para deshabilitar el comportamiento 400 automático, establezca la propiedad <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> en `true`.</span><span class="sxs-lookup"><span data-stu-id="16f1c-178">To disable the automatic 400 behavior, set the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property to `true`.</span></span> <span data-ttu-id="16f1c-179">Agregue el código resaltado siguiente en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="16f1c-179">Add the following highlighted code in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,6)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,7)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](index/samples/2.x/2.1/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=1,5)]

::: moniker-end

## <a name="binding-source-parameter-inference"></a><span data-ttu-id="16f1c-180">Inferencia de parámetro de origen de enlace</span><span class="sxs-lookup"><span data-stu-id="16f1c-180">Binding source parameter inference</span></span>

<span data-ttu-id="16f1c-181">Un atributo de origen de enlace define la ubicación del valor del parámetro de una acción.</span><span class="sxs-lookup"><span data-stu-id="16f1c-181">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="16f1c-182">Existen los atributos de origen de enlace siguientes:</span><span class="sxs-lookup"><span data-stu-id="16f1c-182">The following binding source attributes exist:</span></span>

|<span data-ttu-id="16f1c-183">Atributo</span><span class="sxs-lookup"><span data-stu-id="16f1c-183">Attribute</span></span>|<span data-ttu-id="16f1c-184">Origen de enlace</span><span class="sxs-lookup"><span data-stu-id="16f1c-184">Binding source</span></span> |
|---------|---------|
|[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)     | <span data-ttu-id="16f1c-185">Cuerpo de la solicitud</span><span class="sxs-lookup"><span data-stu-id="16f1c-185">Request body</span></span> |
|[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)     | <span data-ttu-id="16f1c-186">Datos del formulario en el cuerpo de la solicitud</span><span class="sxs-lookup"><span data-stu-id="16f1c-186">Form data in the request body</span></span> |
|[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) | <span data-ttu-id="16f1c-187">Encabezado de la solicitud</span><span class="sxs-lookup"><span data-stu-id="16f1c-187">Request header</span></span> |
|[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)   | <span data-ttu-id="16f1c-188">Parámetro de la cadena de consulta de la solicitud</span><span class="sxs-lookup"><span data-stu-id="16f1c-188">Request query string parameter</span></span> |
|[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)   | <span data-ttu-id="16f1c-189">Datos de ruta de la solicitud actual</span><span class="sxs-lookup"><span data-stu-id="16f1c-189">Route data from the current request</span></span> |
|[`[FromServices]`](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices) | <span data-ttu-id="16f1c-190">Servicio de solicitud insertado como parámetro de acción</span><span class="sxs-lookup"><span data-stu-id="16f1c-190">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="16f1c-191">No use `[FromRoute]` si los valores pueden contener `%2f` (es decir, `/`).</span><span class="sxs-lookup"><span data-stu-id="16f1c-191">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="16f1c-192">`%2f` no incluirá el carácter sin escape `/`.</span><span class="sxs-lookup"><span data-stu-id="16f1c-192">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="16f1c-193">Use `[FromQuery]` si el valor puede contener `%2f`.</span><span class="sxs-lookup"><span data-stu-id="16f1c-193">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="16f1c-194">Sin el atributo `[ApiController]` o los atributos de origen de enlace, como `[FromQuery]`, el entorno de tiempo de ejecución de ASP.NET Core intenta usar el enlazador de modelos de objetos complejos.</span><span class="sxs-lookup"><span data-stu-id="16f1c-194">Without the `[ApiController]` attribute or binding source attributes like `[FromQuery]`, the ASP.NET Core runtime attempts to use the complex object model binder.</span></span> <span data-ttu-id="16f1c-195">El enlazador de modelos de objetos complejos extrae los datos de los proveedores de valor en un orden definido.</span><span class="sxs-lookup"><span data-stu-id="16f1c-195">The complex object model binder pulls data from value providers in a defined order.</span></span>

<span data-ttu-id="16f1c-196">En el ejemplo siguiente, el atributo `[FromQuery]` indica que el valor del parámetro `discontinuedOnly` se proporciona en la cadena de consulta de la dirección URL de la solicitud:</span><span class="sxs-lookup"><span data-stu-id="16f1c-196">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](index/samples/2.x/2.2/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="16f1c-197">El atributo `[ApiController]` aplica reglas de inferencia a los orígenes de datos predeterminados de los parámetros de acción.</span><span class="sxs-lookup"><span data-stu-id="16f1c-197">The `[ApiController]` attribute applies inference rules for the default data sources of action parameters.</span></span> <span data-ttu-id="16f1c-198">Estas reglas aplican atributos a los parámetros de acción, lo que ahorra la necesidad de identificar los orígenes de enlace manualmente.</span><span class="sxs-lookup"><span data-stu-id="16f1c-198">These rules save you from having to identify binding sources manually by applying attributes to the action parameters.</span></span> <span data-ttu-id="16f1c-199">Las reglas de inferencia de orígenes de enlace se comportan de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="16f1c-199">The binding source inference rules behave as follows:</span></span>

* <span data-ttu-id="16f1c-200">`[FromBody]` se infiere para parámetros de tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="16f1c-200">`[FromBody]` is inferred for complex type parameters.</span></span> <span data-ttu-id="16f1c-201">La excepción a `[FromBody]` esta regla es cualquier tipo integrado complejo que tenga un significado especial, como <xref:Microsoft.AspNetCore.Http.IFormCollection> y <xref:System.Threading.CancellationToken>.</span><span class="sxs-lookup"><span data-stu-id="16f1c-201">An exception to the `[FromBody]` inference rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="16f1c-202">El código de inferencia del origen de enlace omite esos tipos especiales.</span><span class="sxs-lookup"><span data-stu-id="16f1c-202">The binding source inference code ignores those special types.</span></span>
* <span data-ttu-id="16f1c-203">`[FromForm]` se infiere para los parámetros de acción de tipo <xref:Microsoft.AspNetCore.Http.IFormFile> y <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span><span class="sxs-lookup"><span data-stu-id="16f1c-203">`[FromForm]` is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="16f1c-204">No se infiere para los tipos simples o definidos por el usuario.</span><span class="sxs-lookup"><span data-stu-id="16f1c-204">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="16f1c-205">`[FromRoute]` se infiere para cualquier nombre de parámetro de acción que coincida con un parámetro de la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="16f1c-205">`[FromRoute]` is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="16f1c-206">Si varias rutas coinciden con un parámetro de acción, cualquier valor de ruta se considera `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="16f1c-206">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="16f1c-207">`[FromQuery]` se infiere para cualquier otro parámetro de acción.</span><span class="sxs-lookup"><span data-stu-id="16f1c-207">`[FromQuery]` is inferred for any other action parameters.</span></span>

### <a name="frombody-inference-notes"></a><span data-ttu-id="16f1c-208">Notas de la inferencia de FromBody</span><span class="sxs-lookup"><span data-stu-id="16f1c-208">FromBody inference notes</span></span>

<span data-ttu-id="16f1c-209">En el caso de los tipos simples, como `string` y `int`, `[FromBody]` no se infiere.</span><span class="sxs-lookup"><span data-stu-id="16f1c-209">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="16f1c-210">Así pues, para los tipos simples, en los casos en los que quiera utilizar dicha funcionalidad, conviene usar el atributo `[FromBody]`.</span><span class="sxs-lookup"><span data-stu-id="16f1c-210">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is needed.</span></span>

<span data-ttu-id="16f1c-211">Cuando una acción tiene más de un parámetro enlazado desde el cuerpo de solicitud, se produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="16f1c-211">When an action has more than one parameter bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="16f1c-212">Por ejemplo, todas las firmas de acción siguientes provocan una excepción:</span><span class="sxs-lookup"><span data-stu-id="16f1c-212">For example, all of the following action method signatures cause an exception:</span></span>

* <span data-ttu-id="16f1c-213">`[FromBody]` se infiere en ambos porque son tipos complejos.</span><span class="sxs-lookup"><span data-stu-id="16f1c-213">`[FromBody]` inferred on both because they're complex types.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action1(Product product, Order order)
  ```

* <span data-ttu-id="16f1c-214">El atributo `[FromBody]` en uno se infiere en el otro porque es un tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="16f1c-214">`[FromBody]` attribute on one, inferred on the other because it's a complex type.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action2(Product product, [FromBody] Order order)
  ```

* <span data-ttu-id="16f1c-215">El atributo `[FromBody]` se infiere en ambos.</span><span class="sxs-lookup"><span data-stu-id="16f1c-215">`[FromBody]` attribute on both.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action3([FromBody] Product product, [FromBody] Order order)
  ```

::: moniker range="= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="16f1c-216">En ASP.NET Core 2.1, los parámetros de tipo de colección, como listas y matrices, se infieren incorrectamente como `[FromQuery]`.</span><span class="sxs-lookup"><span data-stu-id="16f1c-216">In ASP.NET Core 2.1, collection type parameters such as lists and arrays are incorrectly inferred as `[FromQuery]`.</span></span> <span data-ttu-id="16f1c-217">El atributo `[FromBody]` debe usarse con estos parámetros si van a enlazarse desde el cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="16f1c-217">The `[FromBody]` attribute should be used for these parameters if they are to be bound from the request body.</span></span> <span data-ttu-id="16f1c-218">Este comportamiento se ha corregido en ASP.NET Core 2.2 o posterior, donde se infieren los parámetros de tipo de colección para enlazarse desde el cuerpo de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="16f1c-218">This behavior is corrected in ASP.NET Core 2.2 or later, where collection type parameters are inferred to be bound from the body by default.</span></span>

::: moniker-end

### <a name="disable-inference-rules"></a><span data-ttu-id="16f1c-219">Deshabilitación de las reglas de inferencia</span><span class="sxs-lookup"><span data-stu-id="16f1c-219">Disable inference rules</span></span>

<span data-ttu-id="16f1c-220">Para deshabilitar la inferencia del origen de enlace, establezca <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> en `true`.</span><span class="sxs-lookup"><span data-stu-id="16f1c-220">To disable binding source inference, set <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> to `true`.</span></span> <span data-ttu-id="16f1c-221">Agregue el siguiente código en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="16f1c-221">Add the following code in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,5)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,6)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](index/samples/2.x/2.1/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=1,4)]

::: moniker-end

## <a name="multipartform-data-request-inference"></a><span data-ttu-id="16f1c-222">Inferencia de solicitud de varios elementos o datos de formulario</span><span class="sxs-lookup"><span data-stu-id="16f1c-222">Multipart/form-data request inference</span></span>

<span data-ttu-id="16f1c-223">El atributo `[ApiController]` aplica una regla de inferencia cuando se anota un parámetro de acción con el atributo [`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute).</span><span class="sxs-lookup"><span data-stu-id="16f1c-223">The `[ApiController]` attribute applies an inference rule when an action parameter is annotated with the [`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute.</span></span> <span data-ttu-id="16f1c-224">Se infiere el tipo de contenido de la solicitud `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="16f1c-224">The `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="16f1c-225">Para deshabilitar el comportamiento predeterminado, establezca la propiedad <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> en `true` en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="16f1c-225">To disable the default behavior, set the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property to `true` in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,4)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,5)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](index/samples/2.x/2.1/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=1,3)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="problem-details-for-error-status-codes"></a><span data-ttu-id="16f1c-226">Detalles de problemas de los códigos de estado de error</span><span class="sxs-lookup"><span data-stu-id="16f1c-226">Problem details for error status codes</span></span>

<span data-ttu-id="16f1c-227">Cuando la versión de compatibilidad es 2.2 o posterior, MVC transforma un resultado de error (un resultado con el código de estado 400 o superior) en un resultado con <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="16f1c-227">When the compatibility version is 2.2 or later, MVC transforms an error result (a result with status code 400 or higher) to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span> <span data-ttu-id="16f1c-228">El tipo `ProblemDetails` se basa en la [especificación RFC 7807](https://tools.ietf.org/html/rfc7807) para proporcionar detalles de error de lectura mecánica en una respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="16f1c-228">The `ProblemDetails` type is based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807) for providing machine-readable error details in an HTTP response.</span></span>

<span data-ttu-id="16f1c-229">Observe el código siguiente en una acción de controlador:</span><span class="sxs-lookup"><span data-stu-id="16f1c-229">Consider the following code in a controller action:</span></span>

[!code-csharp[](index/samples/2.x/2.2/Controllers/PetsController.cs?name=snippet_ProblemDetailsStatusCode)]

<span data-ttu-id="16f1c-230">El método `NotFound` genera un código de estado HTTP 404 con un cuerpo `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="16f1c-230">The `NotFound` method produces an HTTP 404 status code with a `ProblemDetails` body.</span></span> <span data-ttu-id="16f1c-231">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="16f1c-231">For example:</span></span>

```json
{
  type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
  title: "Not Found",
  status: 404,
  traceId: "0HLHLV31KRN83:00000001"
}
```

### <a name="disable-problemdetails-response"></a><span data-ttu-id="16f1c-232">Deshabilitación de la respuesta ProblemDetails</span><span class="sxs-lookup"><span data-stu-id="16f1c-232">Disable ProblemDetails response</span></span>

<span data-ttu-id="16f1c-233">La creación automática de una instancia de `ProblemDetails` está deshabilitada cuando la propiedad <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors%2A> está establecida en `true`.</span><span class="sxs-lookup"><span data-stu-id="16f1c-233">The automatic creation of a `ProblemDetails` instance is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors%2A> property is set to `true`.</span></span> <span data-ttu-id="16f1c-234">Agregue el siguiente código en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="16f1c-234">Add the following code in `Startup.ConfigureServices`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,7)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,8)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="16f1c-235">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="16f1c-235">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/handle-errors>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
