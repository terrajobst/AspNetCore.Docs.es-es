---
title: Aplicación de formato a datos de respuesta en ASP.NET Core Web API
author: ardalis
description: Obtenga información sobre cómo dar formato a datos de respuesta en ASP.NET Core Web API.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 8/22/2019
uid: web-api/advanced/formatting
ms.openlocfilehash: e503df3d81efbb2800503c0cb4ff5ae093b6e1ac
ms.sourcegitcommit: 023495344053dc59115c80538f0ece935e7490a2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2019
ms.locfileid: "71592349"
---
# <a name="format-response-data-in-aspnet-core-web-api"></a><span data-ttu-id="da086-103">Aplicación de formato a datos de respuesta en ASP.NET Core Web API</span><span class="sxs-lookup"><span data-stu-id="da086-103">Format response data in ASP.NET Core Web API</span></span>

<span data-ttu-id="da086-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="da086-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="da086-105">ASP.NET Core MVC tiene compatibilidad para formatear datos de respuesta.</span><span class="sxs-lookup"><span data-stu-id="da086-105">ASP.NET Core MVC has support for formatting response data.</span></span> <span data-ttu-id="da086-106">Se pueden formatear los datos de respuesta con formatos específicos o en respuesta al formato solicitado por el cliente.</span><span class="sxs-lookup"><span data-stu-id="da086-106">Response data can be formatted using specific formats or in response to client requested format.</span></span>

<span data-ttu-id="da086-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="da086-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="da086-108">Resultados de acción específicos del formato</span><span class="sxs-lookup"><span data-stu-id="da086-108">Format-specific Action Results</span></span>

<span data-ttu-id="da086-109">Algunos tipos de resultado de acción son específicos de un formato determinado, como <xref:Microsoft.AspNetCore.Mvc.JsonResult> y <xref:Microsoft.AspNetCore.Mvc.ContentResult>.</span><span class="sxs-lookup"><span data-stu-id="da086-109">Some action result types are specific to a particular format, such as <xref:Microsoft.AspNetCore.Mvc.JsonResult> and <xref:Microsoft.AspNetCore.Mvc.ContentResult>.</span></span> <span data-ttu-id="da086-110">Las acciones pueden devolver resultados con un formato determinado, independientemente de las preferencias del cliente.</span><span class="sxs-lookup"><span data-stu-id="da086-110">Actions can return results that are formatted in a particular format, regardless of client preferences.</span></span> <span data-ttu-id="da086-111">Por ejemplo, la devolución de `JsonResult` devuelve datos con formato JSON.</span><span class="sxs-lookup"><span data-stu-id="da086-111">For example, returning `JsonResult` returns JSON-formatted data.</span></span> <span data-ttu-id="da086-112">Al devolver `ContentResult` o una cadena se devuelven datos de cadena con formato de texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="da086-112">Returning `ContentResult` or a string returns plain-text-formatted string data.</span></span>

<span data-ttu-id="da086-113">No se requiere una acción para devolver ningún tipo específico.</span><span class="sxs-lookup"><span data-stu-id="da086-113">An action isn't required to return any specific type.</span></span> <span data-ttu-id="da086-114">ASP.NET Core admite cualquier valor devuelto de objeto.</span><span class="sxs-lookup"><span data-stu-id="da086-114">ASP.NET Core supports any object return value.</span></span>  <span data-ttu-id="da086-115">Los resultados de acciones que devuelven objetos que no son tipos <xref:Microsoft.AspNetCore.Mvc.IActionResult> se serializan con la implementación <xref:Microsoft.AspNetCore.Mvc.Formatters.IOutputFormatter> correspondiente.</span><span class="sxs-lookup"><span data-stu-id="da086-115">Results from actions that return objects that are not <xref:Microsoft.AspNetCore.Mvc.IActionResult> types are serialized using the appropriate <xref:Microsoft.AspNetCore.Mvc.Formatters.IOutputFormatter> implementation.</span></span> <span data-ttu-id="da086-116">Para más información, consulte <xref:web-api/action-return-types>.</span><span class="sxs-lookup"><span data-stu-id="da086-116">For more information, see <xref:web-api/action-return-types>.</span></span>

<span data-ttu-id="da086-117">El método auxiliar integrado <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*> devuelve datos con formato JSON: [!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_get)]</span><span class="sxs-lookup"><span data-stu-id="da086-117">The built-in helper method <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*> returns JSON-formatted data: [!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_get)]</span></span>

<span data-ttu-id="da086-118">La descarga de ejemplo devuelve la lista de autores.</span><span class="sxs-lookup"><span data-stu-id="da086-118">The sample download returns the list of authors.</span></span> <span data-ttu-id="da086-119">Con las herramientas para desarrolladores del explorador F12 o [Postman](https://www.getpostman.com/tools) con el código anterior:</span><span class="sxs-lookup"><span data-stu-id="da086-119">Using the F12 browser developer tools or [Postman](https://www.getpostman.com/tools) with the previous code:</span></span>

* <span data-ttu-id="da086-120">Se muestra el encabezado de respuesta que contiene **content-type:** `application/json; charset=utf-8`.</span><span class="sxs-lookup"><span data-stu-id="da086-120">The response header containing **content-type:** `application/json; charset=utf-8` is displayed.</span></span>
* <span data-ttu-id="da086-121">Se muestran los encabezados de solicitud.</span><span class="sxs-lookup"><span data-stu-id="da086-121">The request headers are displayed.</span></span> <span data-ttu-id="da086-122">Por ejemplo, el encabezado `Accept`.</span><span class="sxs-lookup"><span data-stu-id="da086-122">For example, the `Accept` header.</span></span> <span data-ttu-id="da086-123">El código anterior omite el encabezado `Accept`.</span><span class="sxs-lookup"><span data-stu-id="da086-123">The `Accept` header is ignored by the preceding code.</span></span>

<span data-ttu-id="da086-124">Para devolver datos con formato de texto sin formato, use <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> y el método del asistente <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content>:</span><span class="sxs-lookup"><span data-stu-id="da086-124">To return plain text formatted data, use <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> and the <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> helper:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_about)]

<span data-ttu-id="da086-125">En el código anterior, el `Content-Type` devuelto es `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="da086-125">In the preceding code, the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="da086-126">Al devolver una cadena se proporciona `Content-Type` de `text/plain`:</span><span class="sxs-lookup"><span data-stu-id="da086-126">Returning a string delivers `Content-Type` of `text/plain`:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_string)]

<span data-ttu-id="da086-127">Para las acciones con varios tipos de valor devuelto, devuelva `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="da086-127">For actions with multiple return types, return `IActionResult`.</span></span> <span data-ttu-id="da086-128">Por ejemplo, la devolución de códigos de estado HTTP diferentes en función del resultado de las operaciones realizadas.</span><span class="sxs-lookup"><span data-stu-id="da086-128">For example, returning different HTTP status codes based on the result of operations performed.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="da086-129">Negociación de contenido</span><span class="sxs-lookup"><span data-stu-id="da086-129">Content negotiation</span></span>

<span data-ttu-id="da086-130">La negociación de contenido se produce cuando el cliente especifica un [encabezado Accept](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span><span class="sxs-lookup"><span data-stu-id="da086-130">Content negotiation occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="da086-131">El formato predeterminado que ASP.NET Core usa es [JSON](https://json.org/).</span><span class="sxs-lookup"><span data-stu-id="da086-131">The default format used by ASP.NET Core is [JSON](https://json.org/).</span></span> <span data-ttu-id="da086-132">La negociación de contenido:</span><span class="sxs-lookup"><span data-stu-id="da086-132">Content negotiation is:</span></span>

* <span data-ttu-id="da086-133">La implementa <xref:Microsoft.AspNetCore.Mvc.ObjectResult>.</span><span class="sxs-lookup"><span data-stu-id="da086-133">Implemented by <xref:Microsoft.AspNetCore.Mvc.ObjectResult>.</span></span>
* <span data-ttu-id="da086-134">Se integra en los resultados de acción específicos del código de estado devueltos por los métodos auxiliares.</span><span class="sxs-lookup"><span data-stu-id="da086-134">Built into the status code-specific action results returned from the helper methods.</span></span> <span data-ttu-id="da086-135">Los métodos auxiliares de los resultados de acción se basan en `ObjectResult`.</span><span class="sxs-lookup"><span data-stu-id="da086-135">The action results helper methods are based on `ObjectResult`.</span></span>

<span data-ttu-id="da086-136">Cuando se devuelve un tipo de modelo, el tipo de valor devuelto es `ObjectResult`.</span><span class="sxs-lookup"><span data-stu-id="da086-136">When a model type is returned,  the return type is `ObjectResult`.</span></span>

<span data-ttu-id="da086-137">El siguiente método de acción usa los métodos del asistente `Ok` y `NotFound`:</span><span class="sxs-lookup"><span data-stu-id="da086-137">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_search)]

<span data-ttu-id="da086-138">Se devuelve una respuesta con formato JSON, a menos que se haya solicitado otro formato y el servidor pueda devolverlo.</span><span class="sxs-lookup"><span data-stu-id="da086-138">A JSON-formatted response is returned unless another format was requested and the server can return the requested format.</span></span> <span data-ttu-id="da086-139">Las herramientas como [Fiddler](https://www.telerik.com/fiddler) o [Postman](https://www.getpostman.com/tools) pueden establecer el encabezado `Accept` para especificar el formato de devolución.</span><span class="sxs-lookup"><span data-stu-id="da086-139">Tools such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/tools) can set the `Accept` header to specify the return format.</span></span> <span data-ttu-id="da086-140">Cuando `Accept` contiene un tipo que el servidor admite, se devuelve ese tipo.</span><span class="sxs-lookup"><span data-stu-id="da086-140">When the `Accept` contains a type the server supports, that type is returned.</span></span>

<span data-ttu-id="da086-141">De forma predeterminada, ASP.NET Core solo admite JSON.</span><span class="sxs-lookup"><span data-stu-id="da086-141">By default, ASP.NET Core only supports JSON.</span></span> <span data-ttu-id="da086-142">En el caso de las aplicaciones que no cambian el valor predeterminado, las respuestas con formato JSON siempre se devuelven independientemente de la solicitud del cliente.</span><span class="sxs-lookup"><span data-stu-id="da086-142">For apps not changing the default, JSON-formatted responses are alway returned regardless of the client request.</span></span> <span data-ttu-id="da086-143">En la sección siguiente se muestra cómo agregar otros formateadores.</span><span class="sxs-lookup"><span data-stu-id="da086-143">The next section shows how to add additional formatters.</span></span>

<span data-ttu-id="da086-144">Las acciones del controlador pueden devolver POCO (objetos CLR antiguos sin formato).</span><span class="sxs-lookup"><span data-stu-id="da086-144">Controller actions can return POCOs (Plain Old CLR Objects).</span></span> <span data-ttu-id="da086-145">Cuando se devuelve un objeto POCO, el tiempo de ejecución crea automáticamente un `ObjectResult` que encapsula al objeto.</span><span class="sxs-lookup"><span data-stu-id="da086-145">When a POCO is returned, the runtime automatically creates an `ObjectResult` that wraps the object.</span></span> <span data-ttu-id="da086-146">El cliente obtiene el objeto serializado con formato.</span><span class="sxs-lookup"><span data-stu-id="da086-146">The client gets the formatted serialized object.</span></span> <span data-ttu-id="da086-147">Si el objeto que se va a devolver es `null`, se devuelve una respuesta `204 No Content`.</span><span class="sxs-lookup"><span data-stu-id="da086-147">If the object being returned is `null`, a `204 No Content` response is returned.</span></span>

<span data-ttu-id="da086-148">Devolución de un tipo de objeto:</span><span class="sxs-lookup"><span data-stu-id="da086-148">Returning an object type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_alias)]

<span data-ttu-id="da086-149">En el código anterior, una solicitud de un alias de autor válido devuelve una respuesta `200 OK` con los datos del autor.</span><span class="sxs-lookup"><span data-stu-id="da086-149">In the preceding code, a request for a valid author alias returns a `200 OK` response with the author's data.</span></span> <span data-ttu-id="da086-150">Una solicitud de un alias no válido devuelve una respuesta `204 No Content`.</span><span class="sxs-lookup"><span data-stu-id="da086-150">A request for an invalid alias returns a `204 No Content` response.</span></span>

### <a name="the-accept-header"></a><span data-ttu-id="da086-151">El encabezado Accept</span><span class="sxs-lookup"><span data-stu-id="da086-151">The Accept header</span></span>

<span data-ttu-id="da086-152">La *negociación* de contenido se lleva a cabo cuando en la solicitud aparece un encabezado `Accept`.</span><span class="sxs-lookup"><span data-stu-id="da086-152">Content *negotiation* takes place when an `Accept` header appears in the request.</span></span> <span data-ttu-id="da086-153">Cuando una solicitud contiene un encabezado Accept, ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="da086-153">When a request contains an accept header, ASP.NET Core:</span></span>

* <span data-ttu-id="da086-154">Enumera los tipos de medios del encabezado Accept en orden de preferencia.</span><span class="sxs-lookup"><span data-stu-id="da086-154">Enumerates the media types in the accept header in preference order.</span></span>
* <span data-ttu-id="da086-155">Intenta encontrar un formateador que pueda generar una respuesta en uno de los formatos especificados.</span><span class="sxs-lookup"><span data-stu-id="da086-155">Tries to find a formatter that can produce a response in one of the formats specified.</span></span>

<span data-ttu-id="da086-156">Si no se encuentra ningún formateador que pueda satisfacer la solicitud del cliente, ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="da086-156">If no formatter is found that can satisfy the client's request, ASP.NET Core:</span></span>

* <span data-ttu-id="da086-157">Devuelve `406 Not Acceptable` si se ha establecido <xref:Microsoft.AspNetCore.Mvc.MvcOptions>, o bien</span><span class="sxs-lookup"><span data-stu-id="da086-157">Returns `406 Not Acceptable` if <xref:Microsoft.AspNetCore.Mvc.MvcOptions> has been set, or -</span></span>
* <span data-ttu-id="da086-158">Intenta encontrar el primer formateador que puede generar una respuesta.</span><span class="sxs-lookup"><span data-stu-id="da086-158">Tries to find the first formatter that can produce a response.</span></span>

<span data-ttu-id="da086-159">Si no se ha configurado ningún formateador para el formato solicitado, se usará el primer formateador que pueda dar formato al objeto.</span><span class="sxs-lookup"><span data-stu-id="da086-159">If no formatter is configured for the requested format, the first formatter that can format the object is used.</span></span> <span data-ttu-id="da086-160">Si no aparece ningún encabezado `Accept` en la solicitud:</span><span class="sxs-lookup"><span data-stu-id="da086-160">If no `Accept` header appears in the request:</span></span>

* <span data-ttu-id="da086-161">El primer formateador que puede controlar el objeto se usa para serializar la respuesta.</span><span class="sxs-lookup"><span data-stu-id="da086-161">The first formatter that can handle the object is used to serialize the response.</span></span>
* <span data-ttu-id="da086-162">No tiene lugar ninguna negociación.</span><span class="sxs-lookup"><span data-stu-id="da086-162">There isn't any negotiation taking place.</span></span> <span data-ttu-id="da086-163">El servidor está determinando el formato que se va a devolver.</span><span class="sxs-lookup"><span data-stu-id="da086-163">The server is determining what format to return.</span></span>

<span data-ttu-id="da086-164">Si el encabezado Accept contiene `*/*`, el encabezado se omite a menos que `RespectBrowserAcceptHeader` esté establecido en true en <xref:Microsoft.AspNetCore.Mvc.MvcOptions>.</span><span class="sxs-lookup"><span data-stu-id="da086-164">If the Accept header contains `*/*`, the Header is ignored unless `RespectBrowserAcceptHeader` is set to true on <xref:Microsoft.AspNetCore.Mvc.MvcOptions>.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="da086-165">Exploradores y negociación de contenido</span><span class="sxs-lookup"><span data-stu-id="da086-165">Browsers and content negotiation</span></span>

<span data-ttu-id="da086-166">A diferencia de los clientes de API típicos, los exploradores web proporcionan encabezados `Accept`.</span><span class="sxs-lookup"><span data-stu-id="da086-166">Unlike typical API clients, web browsers supply `Accept` headers.</span></span> <span data-ttu-id="da086-167">El explorador web especifica muchos formatos, incluidos los comodines.</span><span class="sxs-lookup"><span data-stu-id="da086-167">Web browser specify many formats, including wildcards.</span></span> <span data-ttu-id="da086-168">De forma predeterminada, cuando el marco detecta que la solicitud procede de un explorador:</span><span class="sxs-lookup"><span data-stu-id="da086-168">By default, when the framework detects that the request is coming from a browser:</span></span>

* <span data-ttu-id="da086-169">El encabezado `Accept` se omite.</span><span class="sxs-lookup"><span data-stu-id="da086-169">The `Accept` header is ignored.</span></span>
* <span data-ttu-id="da086-170">El contenido se devuelve en JSON, a menos que se configure de otra manera.</span><span class="sxs-lookup"><span data-stu-id="da086-170">The content is returned in JSON, unless otherwise configured.</span></span>

<span data-ttu-id="da086-171">Esto proporciona una experiencia más coherente entre los exploradores al consumir las API.</span><span class="sxs-lookup"><span data-stu-id="da086-171">This provides a more consistent experience across browsers when consuming APIs.</span></span>

<span data-ttu-id="da086-172">Para configurar una aplicación para que respete los encabezados de aceptación del explorador, establezca <xref:Microsoft.AspNetCore.Mvc.MvcOptions.RespectBrowserAcceptHeader> en `true`:</span><span class="sxs-lookup"><span data-stu-id="da086-172">To configure an app to honor browser accept headers, set <xref:Microsoft.AspNetCore.Mvc.MvcOptions.RespectBrowserAcceptHeader> to `true`:</span></span>

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end

### <a name="configure-formatters"></a><span data-ttu-id="da086-173">Configuración de formateadores</span><span class="sxs-lookup"><span data-stu-id="da086-173">Configure formatters</span></span>

<span data-ttu-id="da086-174">Las aplicaciones que necesitan admitir formatos adicionales pueden agregar los paquetes NuGet adecuados y configurar la compatibilidad.</span><span class="sxs-lookup"><span data-stu-id="da086-174">Apps that need to support additional formats can add the appropriate NuGet packages and configure support.</span></span> <span data-ttu-id="da086-175">Hay formateadores independientes para la entrada y la salida.</span><span class="sxs-lookup"><span data-stu-id="da086-175">There are separate formatters for input and output.</span></span> <span data-ttu-id="da086-176">El [enlace de modelos](xref:mvc/models/model-binding) usa formateadores de entrada.</span><span class="sxs-lookup"><span data-stu-id="da086-176">Input formatters are used by [Model Binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="da086-177">Los formateadores de salida se usan para dar formato a las respuestas.</span><span class="sxs-lookup"><span data-stu-id="da086-177">Output formatters are used to format responses.</span></span> <span data-ttu-id="da086-178">Para obtener información sobre la creación de un formateador personalizado, vea [Formateadores personalizados](xref:web-api/advanced/custom-formatters).</span><span class="sxs-lookup"><span data-stu-id="da086-178">For information on creating a custom formatter, see [Custom Formatters](xref:web-api/advanced/custom-formatters).</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="add-xml-format-support"></a><span data-ttu-id="da086-179">Adición de compatibilidad con el formato XML</span><span class="sxs-lookup"><span data-stu-id="da086-179">Add XML format support</span></span>

<span data-ttu-id="da086-180">Los formateadores XML que se implementan mediante <xref:System.Xml.Serialization.XmlSerializer> se configuran llamando a <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:</span><span class="sxs-lookup"><span data-stu-id="da086-180">XML formatters implemented using <xref:System.Xml.Serialization.XmlSerializer> are configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:</span></span>

[!code-csharp[](./formatting/3.0sample/Startup.cs?name=snippet)]

<span data-ttu-id="da086-181">El código anterior serializa los resultados mediante `XmlSerializer`.</span><span class="sxs-lookup"><span data-stu-id="da086-181">The preceding code serializes results using `XmlSerializer`.</span></span>

<span data-ttu-id="da086-182">Cuando se usa el código anterior, los métodos de controlador deben devolver el formato adecuado en función del encabezado `Accept` de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="da086-182">When using the preceding code, controller methods should return the appropriate format based on the request's `Accept` header.</span></span>

### <a name="configure-systemtextjson-based-formatters"></a><span data-ttu-id="da086-183">Configuración de formateadores basados en System.Text.Json</span><span class="sxs-lookup"><span data-stu-id="da086-183">Configure System.Text.Json-based formatters</span></span>

<span data-ttu-id="da086-184">Las características para los formateadores basados en `System.Text.Json` pueden configurarse mediante `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions`.</span><span class="sxs-lookup"><span data-stu-id="da086-184">Features for the `System.Text.Json`-based formatters can be configured using `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions`.</span></span>

```csharp
services.AddControllers().AddJsonOptions(options =>
{
    // Use the default property (Pascal) casing.
    options.SerializerOptions.PropertyNamingPolicy = null;

    // Configure a custom converter.
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

<span data-ttu-id="da086-185">Las opciones de serialización de salida se pueden configurar para cada acción mediante `JsonResult`.</span><span class="sxs-lookup"><span data-stu-id="da086-185">Output serialization options, on a per-action basis, can be configured using `JsonResult`.</span></span> <span data-ttu-id="da086-186">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="da086-186">For example:</span></span>

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerOptions
    {
        options.WriteIndented = true,
    });
}
```

### <a name="add-newtonsoftjson-based-json-format-support"></a><span data-ttu-id="da086-187">Adición de compatibilidad con el formato JSON basado en Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="da086-187">Add Newtonsoft.Json-based JSON format support</span></span>

<span data-ttu-id="da086-188">Antes de ASP.NET Core 3.0, los formateadores JSON usados de forma predeterminada son los implementados mediante el paquete `Newtonsoft.Json`.</span><span class="sxs-lookup"><span data-stu-id="da086-188">Prior to ASP.NET Core 3.0, the default used JSON formatters implemented using the `Newtonsoft.Json` package.</span></span> <span data-ttu-id="da086-189">En ASP.NET Core 3.0 o posterior, los formateadores JSON predeterminados se basan en `System.Text.Json`.</span><span class="sxs-lookup"><span data-stu-id="da086-189">In ASP.NET Core 3.0 or later, the default JSON formatters are based on `System.Text.Json`.</span></span> <span data-ttu-id="da086-190">La compatibilidad con las características y los formateadores basados en `Newtonsoft.Json` está disponible al instalar el paquete NuGet [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) y configurarlo en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="da086-190">Support for `Newtonsoft.Json` based formatters and features is available by installing the [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet package and configuring it in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](./formatting/3.0sample/StartupNewtonsoftJson.cs?name=snippet)]

<span data-ttu-id="da086-191">Es posible que algunas características no funcionen bien con formateadores basados en `System.Text.Json` y requieren una referencia a los formateadores basados en `Newtonsoft.Json`.</span><span class="sxs-lookup"><span data-stu-id="da086-191">Some features may not work well with `System.Text.Json`-based formatters and require a reference to the `Newtonsoft.Json`-based formatters.</span></span> <span data-ttu-id="da086-192">Siga usando los formateadores basados en `Newtonsoft.Json` si la aplicación:</span><span class="sxs-lookup"><span data-stu-id="da086-192">Continue using the `Newtonsoft.Json`-based formatters if the app:</span></span>

* <span data-ttu-id="da086-193">Usa atributos `Newtonsoft.Json`.</span><span class="sxs-lookup"><span data-stu-id="da086-193">Uses `Newtonsoft.Json` attributes.</span></span> <span data-ttu-id="da086-194">Por ejemplo: `[JsonProperty]` o `[JsonIgnore]`.</span><span class="sxs-lookup"><span data-stu-id="da086-194">For example, `[JsonProperty]` or `[JsonIgnore]`.</span></span>
* <span data-ttu-id="da086-195">Proporciona la configuración de la serialización.</span><span class="sxs-lookup"><span data-stu-id="da086-195">Customizes the serialization settings.</span></span>
* <span data-ttu-id="da086-196">Se basa en las características que `Newtonsoft.Json` proporciona.</span><span class="sxs-lookup"><span data-stu-id="da086-196">Relies on features that `Newtonsoft.Json` provides.</span></span>
* <span data-ttu-id="da086-197">Configura `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`.</span><span class="sxs-lookup"><span data-stu-id="da086-197">Configures `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`.</span></span> <span data-ttu-id="da086-198">Antes de ASP.NET Core 3.0, `JsonResult.SerializerSettings` acepta una instancia de `JsonSerializerSettings` específica de `Newtonsoft.Json`.</span><span class="sxs-lookup"><span data-stu-id="da086-198">Prior to ASP.NET Core 3.0, `JsonResult.SerializerSettings` accepts an instance of `JsonSerializerSettings` that is specific to `Newtonsoft.Json`.</span></span>
* <span data-ttu-id="da086-199">Genera la documentación de [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>).</span><span class="sxs-lookup"><span data-stu-id="da086-199">Generates [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>) documentation.</span></span>

<span data-ttu-id="da086-200">Las características para los formateadores basados en `Newtonsoft.Json` pueden configurarse mediante `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`:</span><span class="sxs-lookup"><span data-stu-id="da086-200">Features for the `Newtonsoft.Json`-based formatters can be configured using `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`:</span></span>

```csharp
services.AddControllers().AddNewtonsoftJson(options =>
{
    // Use the default property (Pascal) casing
    options.SerializerSettings.ContractResolver = new DefaultContractResolver();

    // Configure a custom converter
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

<span data-ttu-id="da086-201">Las opciones de serialización de salida se pueden configurar para cada acción mediante `JsonResult`.</span><span class="sxs-lookup"><span data-stu-id="da086-201">Output serialization options, on a per-action basis, can be configured using `JsonResult`.</span></span> <span data-ttu-id="da086-202">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="da086-202">For example:</span></span>

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerSettings
    {
        options.Formatting = Formatting.Indented,
    });
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

### <a name="add-xml-format-support"></a><span data-ttu-id="da086-203">Adición de compatibilidad con el formato XML</span><span class="sxs-lookup"><span data-stu-id="da086-203">Add XML format support</span></span>

<span data-ttu-id="da086-204">El formato XML requiere el paquete NuGet [Microsoft.AspNetCore.Mvc.Formatters.Xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/).</span><span class="sxs-lookup"><span data-stu-id="da086-204">XML formatting requires the [Microsoft.AspNetCore.Mvc.Formatters.Xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) NuGet package.</span></span>

<span data-ttu-id="da086-205">Los formateadores XML que se implementan mediante <xref:System.Xml.Serialization.XmlSerializer> se configuran llamando a <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:</span><span class="sxs-lookup"><span data-stu-id="da086-205">XML formatters implemented using <xref:System.Xml.Serialization.XmlSerializer> are configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:</span></span>

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet)]

<span data-ttu-id="da086-206">El código anterior serializa los resultados mediante `XmlSerializer`.</span><span class="sxs-lookup"><span data-stu-id="da086-206">The preceding code serializes results using `XmlSerializer`.</span></span>

<span data-ttu-id="da086-207">Cuando se usa el código anterior, los métodos de controlador deben devolver el formato adecuado en función del encabezado `Accept` de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="da086-207">When using the preceding code, controller methods should return the appropriate format based on the request's `Accept` header.</span></span>

::: moniker-end

### <a name="specify-a-format"></a><span data-ttu-id="da086-208">Especificación de un formato</span><span class="sxs-lookup"><span data-stu-id="da086-208">Specify a format</span></span>

<span data-ttu-id="da086-209">Para restringir los formatos de respuesta, aplique el filtro [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute).</span><span class="sxs-lookup"><span data-stu-id="da086-209">To restrict the response formats, apply the [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filter.</span></span> <span data-ttu-id="da086-210">Al igual que la mayoría de los [filtros](xref:mvc/controllers/filters), `[Produces]` puede aplicarse a la acción, el controlador o el ámbito global:</span><span class="sxs-lookup"><span data-stu-id="da086-210">Like most [Filters](xref:mvc/controllers/filters), `[Produces]` can be applied at the action, controller, or global scope:</span></span>

[!code-csharp[](./formatting/3.0sample/Controllers/WeatherForecastController.cs?name=snippet)]

<span data-ttu-id="da086-211">El filtro [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) anterior:</span><span class="sxs-lookup"><span data-stu-id="da086-211">The preceding [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filter:</span></span>

* <span data-ttu-id="da086-212">Obliga a que todas las acciones dentro del controlador devuelvan respuestas con formato JSON.</span><span class="sxs-lookup"><span data-stu-id="da086-212">Forces all actions within the controller to return JSON-formatted responses.</span></span>
* <span data-ttu-id="da086-213">Si se configuran otros formateadores y el cliente especifica un formato diferente, se devuelve JSON.</span><span class="sxs-lookup"><span data-stu-id="da086-213">If other formatters are configured and the client specifies a different format, JSON is returned.</span></span>

<span data-ttu-id="da086-214">Para más información, consulte [Filtros](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="da086-214">For more information, see [Filters](xref:mvc/controllers/filters).</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="da086-215">Formateadores de casos especiales</span><span class="sxs-lookup"><span data-stu-id="da086-215">Special case formatters</span></span>

<span data-ttu-id="da086-216">Algunos casos especiales se implementan mediante formateadores integrados.</span><span class="sxs-lookup"><span data-stu-id="da086-216">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="da086-217">De forma predeterminada, los tipos de valor devueltos `string` se formatean como *texto/sin formato* (*texto/html* si se solicita a través del encabezado `Accept`).</span><span class="sxs-lookup"><span data-stu-id="da086-217">By default, `string` return types are formatted as *text/plain* (*text/html* if requested via the `Accept` header).</span></span> <span data-ttu-id="da086-218">Este comportamiento se puede quitar mediante la eliminación de <xref:Microsoft.AspNetCore.Mvc.Formatters.TextOutputFormatter>.</span><span class="sxs-lookup"><span data-stu-id="da086-218">This behavior can be deleted by removing the <xref:Microsoft.AspNetCore.Mvc.Formatters.TextOutputFormatter>.</span></span> <span data-ttu-id="da086-219">Los formateadores se quitan en el método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="da086-219">Formatters are removed in the `Configure` method.</span></span> <span data-ttu-id="da086-220">Las acciones que tienen un tipo de valor devuelto de objeto de modelo devuelven `204 No Content` al devolver `null`.</span><span class="sxs-lookup"><span data-stu-id="da086-220">Actions that have a model object return type return `204 No Content` when returning `null`.</span></span> <span data-ttu-id="da086-221">Este comportamiento se puede quitar mediante la eliminación de <xref:Microsoft.AspNetCore.Mvc.Formatters.HttpNoContentOutputFormatter>.</span><span class="sxs-lookup"><span data-stu-id="da086-221">This behavior can be deleted by removing the <xref:Microsoft.AspNetCore.Mvc.Formatters.HttpNoContentOutputFormatter>.</span></span> <span data-ttu-id="da086-222">El código siguiente quita `TextOutputFormatter` y `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="da086-222">The following code removes the `TextOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupTextOutputFormatter.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupTextOutputFormatter.cs?name=snippet)]
::: moniker-end

<span data-ttu-id="da086-223">Sin `TextOutputFormatter`, los tipos de valor devuelto `string` devuelven `406 Not Acceptable`.</span><span class="sxs-lookup"><span data-stu-id="da086-223">Without the `TextOutputFormatter`, `string` return types return `406 Not Acceptable`.</span></span> <span data-ttu-id="da086-224">Si existe un formateador XML, formatea los tipos de valor devueltos `string` si `TextOutputFormatter` se quita.</span><span class="sxs-lookup"><span data-stu-id="da086-224">If an XML formatter exists, it formats `string` return types if the `TextOutputFormatter` is removed.</span></span>

<span data-ttu-id="da086-225">Sin `HttpNoContentOutputFormatter`, se da formato a los objetos nulos mediante el formateador configurado.</span><span class="sxs-lookup"><span data-stu-id="da086-225">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="da086-226">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="da086-226">For example:</span></span>

* <span data-ttu-id="da086-227">El formateador JSON devuelve una respuesta con un cuerpo de `null`.</span><span class="sxs-lookup"><span data-stu-id="da086-227">The JSON formatter returns a response with a body of `null`.</span></span>
* <span data-ttu-id="da086-228">El formateador XML devuelve un elemento XML vacío con el atributo `xsi:nil="true"` establecido.</span><span class="sxs-lookup"><span data-stu-id="da086-228">The XML formatter returns an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="da086-229">Asignaciones de direcciones URL de formato de respuesta</span><span class="sxs-lookup"><span data-stu-id="da086-229">Response format URL mappings</span></span>

<span data-ttu-id="da086-230">Los clientes pueden solicitar un formato determinado como parte de la dirección URL, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="da086-230">Clients can request a particular format as part of the URL, for example:</span></span>

* <span data-ttu-id="da086-231">En la cadena de consulta o en una parte de la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="da086-231">In the query string or part of the path.</span></span>
* <span data-ttu-id="da086-232">Mediante el uso de una extensión de archivo específica del formato como .xml o .json.</span><span class="sxs-lookup"><span data-stu-id="da086-232">By using a format-specific file extension such as .xml or .json.</span></span>

<span data-ttu-id="da086-233">La asignación de la ruta de acceso de la solicitud debe especificarse en la ruta que use la API.</span><span class="sxs-lookup"><span data-stu-id="da086-233">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="da086-234">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="da086-234">For example:</span></span>

[!code-csharp[](./formatting/sample/Controllers/ProductsController.cs?name=snippet)]

<span data-ttu-id="da086-235">Esta ruta anterior permite especificar el formato solicitado como una extensión de archivo opcional.</span><span class="sxs-lookup"><span data-stu-id="da086-235">The preceding route allows the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="da086-236">El atributo [`[FormatFilter]`](xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute) comprueba la existencia del valor de formato en `RouteData` y asigna el formato de respuesta al formateador adecuado cuando se crea la respuesta.</span><span class="sxs-lookup"><span data-stu-id="da086-236">The [`[FormatFilter]`](xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute) attribute checks for the existence of the format value in the `RouteData` and maps the response format to the appropriate formatter when the response is created.</span></span>

|           <span data-ttu-id="da086-237">Ruta</span><span class="sxs-lookup"><span data-stu-id="da086-237">Route</span></span>        |             <span data-ttu-id="da086-238">Formateador</span><span class="sxs-lookup"><span data-stu-id="da086-238">Formatter</span></span>              |
|------------------------|------------------------------------|
|   `/api/products/5`    |    <span data-ttu-id="da086-239">Formateador de salida predeterminado</span><span class="sxs-lookup"><span data-stu-id="da086-239">The default output formatter</span></span>    |
| `/api/products/5.json` | <span data-ttu-id="da086-240">Formateador JSON (si está configurado)</span><span class="sxs-lookup"><span data-stu-id="da086-240">The JSON formatter (if configured)</span></span> |
| `/api/products/5.xml`  | <span data-ttu-id="da086-241">Formateador XML (si está configurado)</span><span class="sxs-lookup"><span data-stu-id="da086-241">The XML formatter (if configured)</span></span>  |
