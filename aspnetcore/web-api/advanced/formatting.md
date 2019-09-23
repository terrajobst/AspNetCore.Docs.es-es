---
title: Aplicación de formato a datos de respuesta en ASP.NET Core Web API
author: ardalis
description: Obtenga información sobre cómo dar formato a datos de respuesta en ASP.NET Core Web API.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 05/29/2019
uid: web-api/advanced/formatting
ms.openlocfilehash: 8bee4efdae5341ddab5bd3aec278ecfef37f0c08
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082355"
---
<!-- DO NOT EDIT BEFORE https://github.com/aspnet/AspNetCore.Docs/pull/12077 MERGES -->
# <a name="format-response-data-in-aspnet-core-web-api"></a><span data-ttu-id="03200-103">Aplicación de formato a datos de respuesta en ASP.NET Core Web API</span><span class="sxs-lookup"><span data-stu-id="03200-103">Format response data in ASP.NET Core Web API</span></span>

<span data-ttu-id="03200-104">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="03200-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="03200-105">ASP.NET Core MVC tiene compatibilidad integrada para dar formato a datos de respuesta, en función de formatos fijos o especificaciones del cliente.</span><span class="sxs-lookup"><span data-stu-id="03200-105">ASP.NET Core MVC has built-in support for formatting response data, using fixed formats or in response to client specifications.</span></span>

<span data-ttu-id="03200-106">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="03200-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="03200-107">Resultados de acción específica de formato</span><span class="sxs-lookup"><span data-stu-id="03200-107">Format-Specific Action Results</span></span>

<span data-ttu-id="03200-108">Algunos tipos de resultado de acción son específicos de un formato determinado, como `JsonResult` y `ContentResult`.</span><span class="sxs-lookup"><span data-stu-id="03200-108">Some action result types are specific to a particular format, such as `JsonResult` and `ContentResult`.</span></span> <span data-ttu-id="03200-109">Las acciones pueden devolver resultados específicos a los que siempre se da formato de una manera determinada.</span><span class="sxs-lookup"><span data-stu-id="03200-109">Actions can return specific results that are always formatted in a particular manner.</span></span> <span data-ttu-id="03200-110">Por ejemplo, si se devuelve un `JsonResult`, se devolverán datos con formato JSON, independientemente de las preferencias del cliente.</span><span class="sxs-lookup"><span data-stu-id="03200-110">For example, returning a `JsonResult` will return JSON-formatted data, regardless of client preferences.</span></span> <span data-ttu-id="03200-111">Del mismo modo, si se devuelve un `ContentResult`, se devolverán datos de cadena con formato de texto sin formato (al igual que si se devuelve simplemente una cadena).</span><span class="sxs-lookup"><span data-stu-id="03200-111">Likewise, returning a `ContentResult` will return plain-text-formatted string data (as will simply returning a string).</span></span>

> [!NOTE]
> <span data-ttu-id="03200-112">No se requieren acciones para devolver un tipo en particular, ya que MVC es compatible con cualquier valor devuelto por un objeto.</span><span class="sxs-lookup"><span data-stu-id="03200-112">An action isn't required to return any particular type; MVC supports any object return value.</span></span> <span data-ttu-id="03200-113">Si una acción devuelve una implementación `IActionResult` y el controlador hereda de `Controller`, los desarrolladores disponen de diversos métodos del asistente que se corresponden con muchas de las opciones.</span><span class="sxs-lookup"><span data-stu-id="03200-113">If an action returns an `IActionResult` implementation and the controller inherits from `Controller`, developers have many helper methods corresponding to many of the choices.</span></span> <span data-ttu-id="03200-114">Los resultados de acciones que devuelven objetos que no son tipos `IActionResult` se serializarán con la implementación `IOutputFormatter` correspondiente.</span><span class="sxs-lookup"><span data-stu-id="03200-114">Results from actions that return objects that are not `IActionResult` types will be serialized using the appropriate `IOutputFormatter` implementation.</span></span>

<span data-ttu-id="03200-115">Para devolver datos en un formato específico desde un controlador que hereda de la clase base `Controller`, use el método del asistente integrado `Json` para devolver JSON y `Content` para texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="03200-115">To return data in a specific format from a controller that inherits from the `Controller` base class, use the built-in helper method `Json` to return JSON and `Content` for plain text.</span></span> <span data-ttu-id="03200-116">El método de acción debe devolver el tipo de resultado específico (por ejemplo, `JsonResult`) o bien `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="03200-116">Your action method should return either the specific result type (for instance, `JsonResult`) or `IActionResult`.</span></span>

<span data-ttu-id="03200-117">Para devolver datos con formato JSON:</span><span class="sxs-lookup"><span data-stu-id="03200-117">Returning JSON-formatted data:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

<span data-ttu-id="03200-118">Respuesta de ejemplo de esta acción:</span><span class="sxs-lookup"><span data-stu-id="03200-118">Sample response from this action:</span></span>

![Pestaña Red de Herramientas de desarrollo de Microsoft Edge en la que se muestra que el tipo de contenido de la respuesta es application/json](formatting/_static/json-response.png)

<span data-ttu-id="03200-120">Tenga en cuenta que el tipo de contenido de la respuesta es `application/json`, y se muestra en la lista de solicitudes de red y en la sección Encabezados de respuesta.</span><span class="sxs-lookup"><span data-stu-id="03200-120">Note that the content type of the response is `application/json`, shown both in the list of network requests and in the Response Headers section.</span></span> <span data-ttu-id="03200-121">Observe también la lista de opciones que aparecen en el explorador (en este caso, Microsoft Edge) en el encabezado Accept de la sección Encabezados de solicitud.</span><span class="sxs-lookup"><span data-stu-id="03200-121">Also note the list of options presented by the browser (in this case, Microsoft Edge) in the Accept header in the Request Headers section.</span></span> <span data-ttu-id="03200-122">La técnica actual omite este encabezado; más adelante se describe su uso.</span><span class="sxs-lookup"><span data-stu-id="03200-122">The current technique is ignoring this header; obeying it is discussed below.</span></span>

<span data-ttu-id="03200-123">Para devolver datos con formato de texto sin formato, use `ContentResult` y el método del asistente `Content`:</span><span class="sxs-lookup"><span data-stu-id="03200-123">To return plain text formatted data, use `ContentResult` and the `Content` helper:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

<span data-ttu-id="03200-124">Una respuesta de esta acción:</span><span class="sxs-lookup"><span data-stu-id="03200-124">A response from this action:</span></span>

![Pestaña Red de Herramientas de desarrollo de Microsoft Edge en la que se muestra que el tipo de contenido de la respuesta es texto/sin formato](formatting/_static/text-response.png)

<span data-ttu-id="03200-126">Observe que en este caso el `Content-Type` devuelto es `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="03200-126">Note in this case the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="03200-127">También puede obtener este mismo comportamiento mediante el uso de un tipo de respuesta de cadena:</span><span class="sxs-lookup"><span data-stu-id="03200-127">You can also achieve this same behavior using just a string response type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> <span data-ttu-id="03200-128">Para las acciones no triviales con varias opciones o tipos de valor devueltos (por ejemplo, códigos de estado HTTP diferentes en función del resultado de las operaciones realizadas), se recomienda el uso de `IActionResult` como tipo de valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="03200-128">For non-trivial actions with multiple return types or options (for example, different HTTP status codes based on the result of operations performed), prefer `IActionResult` as the return type.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="03200-129">Negociación de contenido</span><span class="sxs-lookup"><span data-stu-id="03200-129">Content Negotiation</span></span>

<span data-ttu-id="03200-130">La negociación de contenido (o *conneg*) se produce cuando el cliente especifica un [encabezado Accept](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span><span class="sxs-lookup"><span data-stu-id="03200-130">Content negotiation (*conneg* for short) occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="03200-131">El formato predeterminado que usa ASP.NET Core MVC es JSON.</span><span class="sxs-lookup"><span data-stu-id="03200-131">The default format used by ASP.NET Core MVC is JSON.</span></span> <span data-ttu-id="03200-132">La negociación de contenido se implementa mediante `ObjectResult`.</span><span class="sxs-lookup"><span data-stu-id="03200-132">Content negotiation is implemented by `ObjectResult`.</span></span> <span data-ttu-id="03200-133">También se integra en los resultados de acción específicos del código de estado devueltos por los métodos del asistente (que se basan en `ObjectResult`).</span><span class="sxs-lookup"><span data-stu-id="03200-133">It's also built into the status code specific action results returned from the helper methods (which are all based on `ObjectResult`).</span></span> <span data-ttu-id="03200-134">También puede devolver un tipo de modelo (es decir, una clase que haya definido como tipo de transferencia de datos) y el marco de trabajo lo ajustará automáticamente en un `ObjectResult`.</span><span class="sxs-lookup"><span data-stu-id="03200-134">You can also return a model type (a class you've defined as your data transfer type) and the framework will automatically wrap it in an `ObjectResult` for you.</span></span>

<span data-ttu-id="03200-135">El siguiente método de acción usa los métodos del asistente `Ok` y `NotFound`:</span><span class="sxs-lookup"><span data-stu-id="03200-135">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

<span data-ttu-id="03200-136">Se devolverá una respuesta con formato JSON, a menos que se haya solicitado otro formato y el servidor pueda devolverlo.</span><span class="sxs-lookup"><span data-stu-id="03200-136">A JSON-formatted response will be returned unless another format was requested and the server can return the requested format.</span></span> <span data-ttu-id="03200-137">Puede usar una herramienta como [Fiddler](https://www.telerik.com/fiddler) para crear una solicitud que incluya un encabezado Accept y especifique otro formato.</span><span class="sxs-lookup"><span data-stu-id="03200-137">You can use a tool like [Fiddler](https://www.telerik.com/fiddler) to create a request that includes an Accept header and specify another format.</span></span> <span data-ttu-id="03200-138">En ese caso, si el servidor tiene un *formateador* que puede producir una respuesta en el formato solicitado, el resultado se devolverá en el formato preferido por el cliente.</span><span class="sxs-lookup"><span data-stu-id="03200-138">In that case, if the server has a *formatter* that can produce a response in the requested format, the result will be returned in the client-preferred format.</span></span>

![Consola de Fiddler en la que se muestra una solicitud GET creada manualmente con el valor application/xml para el encabezado Accept](formatting/_static/fiddler-composer.png)

<span data-ttu-id="03200-140">En la captura de pantalla anterior, se ha usado el compositor de Fiddler para generar una solicitud mediante la especificación de `Accept: application/xml`.</span><span class="sxs-lookup"><span data-stu-id="03200-140">In the above screenshot, the Fiddler Composer has been used to generate a request, specifying `Accept: application/xml`.</span></span> <span data-ttu-id="03200-141">ASP.NET Core MVC solo admite JSON de forma predeterminada, por lo que, incluso si se especifica otro formato, el resultado devuelto tendrá formato JSON.</span><span class="sxs-lookup"><span data-stu-id="03200-141">By default, ASP.NET Core MVC only supports JSON, so even when another format is specified, the result returned is still JSON-formatted.</span></span> <span data-ttu-id="03200-142">En la sección siguiente verá cómo agregar otros formateadores.</span><span class="sxs-lookup"><span data-stu-id="03200-142">You'll see how to add additional formatters in the next section.</span></span>

<span data-ttu-id="03200-143">Las acciones del controlador pueden devolver POCO (objetos CLR estándar), en cuyo caso ASP.NET Core MVC crea automáticamente un `ObjectResult` que ajuste el objeto.</span><span class="sxs-lookup"><span data-stu-id="03200-143">Controller actions can return POCOs (Plain Old CLR Objects), in which case ASP.NET Core MVC automatically creates an `ObjectResult` for you that wraps the object.</span></span> <span data-ttu-id="03200-144">El cliente obtendrá el objeto serializado con formato (el formato predeterminado es JSON, pero puede configurar XML u otros formatos).</span><span class="sxs-lookup"><span data-stu-id="03200-144">The client will get the formatted serialized object (JSON format is the default; you can configure XML or other formats).</span></span> <span data-ttu-id="03200-145">Si el objeto que se devuelve es `null`, el marco de trabajo devolverá una respuesta `204 No Content`.</span><span class="sxs-lookup"><span data-stu-id="03200-145">If the object being returned is `null`, then the framework will return a `204 No Content` response.</span></span>

<span data-ttu-id="03200-146">Devolución de un tipo de objeto:</span><span class="sxs-lookup"><span data-stu-id="03200-146">Returning an object type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

<span data-ttu-id="03200-147">En el ejemplo, una solicitud de un alias de autor válido recibirá una respuesta "200 - Correcto" con los datos del autor.</span><span class="sxs-lookup"><span data-stu-id="03200-147">In the sample, a request for a valid author alias will receive a 200 OK response with the author's data.</span></span> <span data-ttu-id="03200-148">Una solicitud de un alias no válido recibirá una respuesta "204 Sin contenido".</span><span class="sxs-lookup"><span data-stu-id="03200-148">A request for an invalid alias will receive a 204 No Content response.</span></span> <span data-ttu-id="03200-149">A continuación se muestran capturas de pantalla en las que aparece la respuesta en formatos XML y JSON.</span><span class="sxs-lookup"><span data-stu-id="03200-149">Screenshots showing the response in XML and JSON formats are shown below.</span></span>

### <a name="content-negotiation-process"></a><span data-ttu-id="03200-150">Proceso de negociación de contenido</span><span class="sxs-lookup"><span data-stu-id="03200-150">Content Negotiation Process</span></span>

<span data-ttu-id="03200-151">La *negociación* de contenido solo se lleva a cabo si en la solicitud aparece un encabezado `Accept`.</span><span class="sxs-lookup"><span data-stu-id="03200-151">Content *negotiation* only takes place if an `Accept` header appears in the request.</span></span> <span data-ttu-id="03200-152">Si una solicitud contiene un encabezado Accept, el marco de trabajo enumerará los tipos de medios en el encabezado Accept en orden de preferencia e intentará encontrar un formateador que pueda generar una respuesta en uno de los formatos especificados por dicho encabezado.</span><span class="sxs-lookup"><span data-stu-id="03200-152">When a request contains an accept header, the framework will enumerate the media types in the accept header in preference order and will try to find a formatter that can produce a response in one of the formats specified by the accept header.</span></span> <span data-ttu-id="03200-153">En caso de que no se encuentre ningún formateador que satisfaga la solicitud del cliente, el marco de trabajo intentará encontrar el primer formateador que pueda generar una respuesta (a menos que el desarrollador haya configurado la opción en `MvcOptions` para devolver "406 - No aceptable").</span><span class="sxs-lookup"><span data-stu-id="03200-153">In case no formatter is found that can satisfy the client's request, the framework will try to find the first formatter that can produce a response (unless the developer has configured the option on `MvcOptions` to return 406 Not Acceptable instead).</span></span> <span data-ttu-id="03200-154">Si la solicitud especifica XML pero no se ha configurado el formateador XML, se usará el formateador JSON.</span><span class="sxs-lookup"><span data-stu-id="03200-154">If the request specifies XML, but the XML formatter has not been configured, then the JSON formatter will be used.</span></span> <span data-ttu-id="03200-155">Por lo general, si no se ha configurado ningún formateador que proporcione el formato solicitado, se usará el primer formateador que pueda dar formato al objeto.</span><span class="sxs-lookup"><span data-stu-id="03200-155">More generally, if no formatter is configured that can provide the requested format, then the first formatter that can format the object is used.</span></span> <span data-ttu-id="03200-156">Si no se especifica ningún encabezado, se usará para serializar la respuesta el primer formateador que pueda controlar el objeto que se va a devolver.</span><span class="sxs-lookup"><span data-stu-id="03200-156">If no header is given, the first formatter that can handle the object to be returned will be used to serialize the response.</span></span> <span data-ttu-id="03200-157">En este caso no se produce ninguna negociación, ya que es el servidor el que determina qué formato usará.</span><span class="sxs-lookup"><span data-stu-id="03200-157">In this case, there isn't any negotiation taking place - the server is determining what format it will use.</span></span>

> [!NOTE]
> <span data-ttu-id="03200-158">Si el encabezado Accept contiene `*/*`, el encabezado se omitirá a menos que `RespectBrowserAcceptHeader` esté establecido en true en `MvcOptions`.</span><span class="sxs-lookup"><span data-stu-id="03200-158">If the Accept header contains `*/*`, the Header will be ignored unless `RespectBrowserAcceptHeader` is set to true on `MvcOptions`.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="03200-159">Exploradores y negociación de contenido</span><span class="sxs-lookup"><span data-stu-id="03200-159">Browsers and Content Negotiation</span></span>

<span data-ttu-id="03200-160">A diferencia de los clientes de API típicos, los exploradores web suelen proporcionar encabezados `Accept` que incluyen una amplia gama de formatos, incluidos caracteres comodín.</span><span class="sxs-lookup"><span data-stu-id="03200-160">Unlike typical API clients, web browsers tend to supply `Accept` headers that include a wide array of formats, including wildcards.</span></span> <span data-ttu-id="03200-161">De forma predeterminada, cuando el marco de trabajo detecta que la solicitud procede de un explorador, omite el encabezado `Accept` y devuelve el contenido en el formato predeterminado configurado de la aplicación (JSON, a menos que se haya configurado otro).</span><span class="sxs-lookup"><span data-stu-id="03200-161">By default, when the framework detects that the request is coming from a browser, it will ignore the `Accept` header and instead return the content in the application's configured default format (JSON unless otherwise configured).</span></span> <span data-ttu-id="03200-162">Esto proporciona una experiencia más coherente al usar exploradores diferentes para el consumo de API.</span><span class="sxs-lookup"><span data-stu-id="03200-162">This provides a more consistent experience when using different browsers to consume APIs.</span></span>

<span data-ttu-id="03200-163">Si prefiere que la aplicación respete los encabezados Accept del explorador, puede incluirlo en la configuración de MVC mediante el establecimiento de `RespectBrowserAcceptHeader` en `true` en el método `ConfigureServices` de *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="03200-163">If you would prefer your application honor browser accept headers, you can configure this as part of MVC's configuration by setting `RespectBrowserAcceptHeader` to `true` in the `ConfigureServices` method in *Startup.cs*.</span></span>

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a><span data-ttu-id="03200-164">Configuración de formateadores</span><span class="sxs-lookup"><span data-stu-id="03200-164">Configuring Formatters</span></span>

<span data-ttu-id="03200-165">Si la aplicación necesita admitir formatos adicionales además del formato JSON predeterminado, puede agregar paquetes NuGet y configurar MVC para que los admita.</span><span class="sxs-lookup"><span data-stu-id="03200-165">If your application needs to support additional formats beyond the default of JSON, you can add NuGet packages and configure MVC to support them.</span></span> <span data-ttu-id="03200-166">Hay formateadores independientes para la entrada y la salida.</span><span class="sxs-lookup"><span data-stu-id="03200-166">There are separate formatters for input and output.</span></span> <span data-ttu-id="03200-167">El [enlace de modelos](xref:mvc/models/model-binding) usa formateadores de entrada, mientras que los formateadores de salida se usan para dar formato a las respuestas.</span><span class="sxs-lookup"><span data-stu-id="03200-167">Input formatters are used by [Model Binding](xref:mvc/models/model-binding); output formatters are used to format responses.</span></span> <span data-ttu-id="03200-168">También puede configurar [formateadores personalizados](xref:web-api/advanced/custom-formatters).</span><span class="sxs-lookup"><span data-stu-id="03200-168">You can also configure [Custom Formatters](xref:web-api/advanced/custom-formatters).</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="configure-systemtextjson-based-formatters"></a><span data-ttu-id="03200-169">Configuración de formateadores basados en System.Text.Json</span><span class="sxs-lookup"><span data-stu-id="03200-169">Configure System.Text.Json-based formatters</span></span>

<span data-ttu-id="03200-170">Las características para los formateadores basados en `System.Text.Json` pueden configurarse mediante `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions`.</span><span class="sxs-lookup"><span data-stu-id="03200-170">Features for the `System.Text.Json`-based formatters can be configured using `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions`.</span></span>

```csharp
services.AddControllers().AddJsonOptions(options =>
{
    // Use the default property (Pascal) casing.
    options.SerializerOptions.PropertyNamingPolicy = null;

    // Configure a custom converter.
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

<span data-ttu-id="03200-171">Las opciones de serialización de salida se pueden configurar para cada acción mediante `JsonResult`.</span><span class="sxs-lookup"><span data-stu-id="03200-171">Output serialization options, on a per-action basis, can be configured using `JsonResult`.</span></span> <span data-ttu-id="03200-172">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="03200-172">For example:</span></span>

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerOptions
    {
        options.WriteIndented = true,
    });
}
```

### <a name="add-newtonsoftjson-based-json-format-support"></a><span data-ttu-id="03200-173">Adición de compatibilidad con el formato JSON basado en Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="03200-173">Add Newtonsoft.Json-based JSON format support</span></span>

<span data-ttu-id="03200-174">Antes de ASP.NET Core 3.0, MVC usa de forma predeterminada formateadores JSON que se implementan mediante el paquete `Newtonsoft.Json`.</span><span class="sxs-lookup"><span data-stu-id="03200-174">Prior to ASP.NET Core 3.0, MVC defaulted to using JSON formatters implemented using the `Newtonsoft.Json` package.</span></span> <span data-ttu-id="03200-175">En ASP.NET Core 3.0 o posterior, los formateadores JSON predeterminados se basan en `System.Text.Json`.</span><span class="sxs-lookup"><span data-stu-id="03200-175">In ASP.NET Core 3.0 or later, the default JSON formatters are based on `System.Text.Json`.</span></span> <span data-ttu-id="03200-176">La compatibilidad con los formateadores basados en `Newtonsoft.Json` y con las características está disponible al instalar el paquete NuGet [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) y configurarlo en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="03200-176">Support for `Newtonsoft.Json`-based formatters and features is available by installing the [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet package and configuring it in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddControllers()
    .AddNewtonsoftJson();
```

<span data-ttu-id="03200-177">Es posible que algunas características no funcionen bien con formateadores basados en `System.Text.Json` y requieren una referencia a los formateadores basados en `Newtonsoft.Json` para la versión de ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="03200-177">Some features may not work well with `System.Text.Json`-based formatters and require a reference to the `Newtonsoft.Json`-based formatters for the ASP.NET Core 3.0 release.</span></span> <span data-ttu-id="03200-178">Siga usando los formateadores basados en `Newtonsoft.Json` si la aplicación ASP.NET Core 3.0 o posterior:</span><span class="sxs-lookup"><span data-stu-id="03200-178">Continue using the `Newtonsoft.Json`-based formatters if your ASP.NET Core 3.0 or later app:</span></span>

* <span data-ttu-id="03200-179">Usa atributos `Newtonsoft.Json` (por ejemplo, `[JsonProperty]` o `[JsonIgnore]`), personaliza la configuración de serialización o se basa en características que `Newtonsoft.Json` proporciona.</span><span class="sxs-lookup"><span data-stu-id="03200-179">Uses `Newtonsoft.Json` attributes (for example, `[JsonProperty]` or `[JsonIgnore]`), customizes the serialization settings, or relies on features that `Newtonsoft.Json` provides.</span></span>
* <span data-ttu-id="03200-180">Configura `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`.</span><span class="sxs-lookup"><span data-stu-id="03200-180">Configures `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`.</span></span> <span data-ttu-id="03200-181">Antes de ASP.NET Core 3.0, `JsonResult.SerializerSettings` acepta una instancia de `JsonSerializerSettings` específica de `Newtonsoft.Json`.</span><span class="sxs-lookup"><span data-stu-id="03200-181">Prior to ASP.NET Core 3.0, `JsonResult.SerializerSettings` accepts an instance of `JsonSerializerSettings` that is specific to `Newtonsoft.Json`.</span></span>
* <span data-ttu-id="03200-182">Genera la documentación de [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>).</span><span class="sxs-lookup"><span data-stu-id="03200-182">Generates [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>) documentation.</span></span>

<span data-ttu-id="03200-183">Las características para los formateadores basados en `Newtonsoft.Json` pueden configurarse mediante `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`:</span><span class="sxs-lookup"><span data-stu-id="03200-183">Features for the `Newtonsoft.Json`-based formatters can be configured using `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`:</span></span>

```csharp
services.AddControllers().AddNewtonsoftJson(options =>
{
    // Use the default property (Pascal) casing
    options.SerializerSettings.ContractResolver = new DefautlContractResolver();

    // Configure a custom converter
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

<span data-ttu-id="03200-184">Las opciones de serialización de salida se pueden configurar para cada acción mediante `JsonResult`.</span><span class="sxs-lookup"><span data-stu-id="03200-184">Output serialization options, on a per-action basis, can be configured using `JsonResult`.</span></span> <span data-ttu-id="03200-185">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="03200-185">For example:</span></span>

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

### <a name="add-xml-format-support"></a><span data-ttu-id="03200-186">Adición de compatibilidad con el formato XML</span><span class="sxs-lookup"><span data-stu-id="03200-186">Add XML format support</span></span>

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="03200-187">Para agregar compatibilidad con el formato XML en ASP.NET Core 2.2 o anterior, instale el paquete NuGet [Microsoft.AspNetCore.Mvc.Formatters.Xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/).</span><span class="sxs-lookup"><span data-stu-id="03200-187">To add XML formatting support in ASP.NET Core 2.2 or earlier, install the [Microsoft.AspNetCore.Mvc.Formatters.Xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) NuGet package.</span></span>

::: moniker-end

<span data-ttu-id="03200-188">Los formateadores XML que se implementan mediante `System.Xml.Serialization.XmlSerializer` pueden configurarse llamando a <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*> en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="03200-188">XML formatters implemented using `System.Xml.Serialization.XmlSerializer` can be configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*> in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

<span data-ttu-id="03200-189">Además, los formateadores XML que se implementan mediante `System.Runtime.Serialization.DataContractSerializer` pueden configurarse llamando a <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlDataContractSerializerFormatters*> en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="03200-189">Alternatively, XML formatters implemented using `System.Runtime.Serialization.DataContractSerializer` can be configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlDataContractSerializerFormatters*> in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .AddXmlDataContractSerializerFormatters();
```

<span data-ttu-id="03200-190">Una vez que se ha agregado compatibilidad con el formato XML, los métodos de controlador deben devolver el formato adecuado en función del encabezado `Accept` de la solicitud, como se muestra en este ejemplo de Fiddler:</span><span class="sxs-lookup"><span data-stu-id="03200-190">Once you've added support for XML formatting, your controller methods should return the appropriate format based on the request's `Accept` header, as this Fiddler example demonstrates:</span></span>

![Consola de Fiddler: en la pestaña Sin formato de la solicitud se ve que el valor del encabezado Accept es application/xml.](formatting/_static/xml-response.png)

<span data-ttu-id="03200-193">En la pestaña Inspectores puede ver que la solicitud GET sin formato se ha realizado con un conjunto de encabezados `Accept: application/xml`.</span><span class="sxs-lookup"><span data-stu-id="03200-193">You can see in the Inspectors tab that the Raw GET request was made with an `Accept: application/xml` header set.</span></span> <span data-ttu-id="03200-194">En el panel de respuesta se muestra el encabezado `Content-Type: application/xml`, y el objeto `Author` se ha serializado a XML.</span><span class="sxs-lookup"><span data-stu-id="03200-194">The response pane shows the `Content-Type: application/xml` header, and the `Author` object has been serialized to XML.</span></span>

<span data-ttu-id="03200-195">Use la pestaña Compositor para modificar la solicitud para especificar `application/json` en el encabezado `Accept`.</span><span class="sxs-lookup"><span data-stu-id="03200-195">Use the Composer tab to modify the request to specify `application/json` in the `Accept` header.</span></span> <span data-ttu-id="03200-196">Ejecute la solicitud y la respuesta se formateará como JSON:</span><span class="sxs-lookup"><span data-stu-id="03200-196">Execute the request, and the response will be formatted as JSON:</span></span>

![Consola de Fiddler: en la pestaña Sin formato de la solicitud se ve que el valor del encabezado Accept es application/json.](formatting/_static/json-response-fiddler.png)

<span data-ttu-id="03200-199">En esta captura de pantalla, puede ver que la solicitud establece un encabezado `Accept: application/json` y la respuesta lo especifica como su `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="03200-199">In this screenshot, you can see the request sets a header of `Accept: application/json` and the response specifies the same as its `Content-Type`.</span></span> <span data-ttu-id="03200-200">El objeto `Author` se muestra en el cuerpo de la respuesta, en formato JSON.</span><span class="sxs-lookup"><span data-stu-id="03200-200">The `Author` object is shown in the body of the response, in JSON format.</span></span>

### <a name="forcing-a-particular-format"></a><span data-ttu-id="03200-201">Forzar un formato determinado</span><span class="sxs-lookup"><span data-stu-id="03200-201">Forcing a Particular Format</span></span>

<span data-ttu-id="03200-202">Si quiere restringir los formatos de respuesta para una acción concreta, aplique el filtro `[Produces]`.</span><span class="sxs-lookup"><span data-stu-id="03200-202">If you would like to restrict the response formats for a specific action you can, you can apply the `[Produces]` filter.</span></span> <span data-ttu-id="03200-203">El filtro `[Produces]` especifica los formatos de respuesta para una acción o controlador específicos.</span><span class="sxs-lookup"><span data-stu-id="03200-203">The `[Produces]` filter specifies the response formats for a specific action (or controller).</span></span> <span data-ttu-id="03200-204">Al igual que la mayoría de los [filtros](xref:mvc/controllers/filters), puede aplicarse a la acción, el controlador o el ámbito global.</span><span class="sxs-lookup"><span data-stu-id="03200-204">Like most [Filters](xref:mvc/controllers/filters), this can be applied at the action, controller, or global scope.</span></span>

```csharp
[Produces("application/json")]
public class AuthorsController
```

<span data-ttu-id="03200-205">El filtro `[Produces]` obligará a que todas las acciones dentro de `AuthorsController` devuelvan respuestas con formato JSON, incluso si se han configurado otros formateadores para la aplicación y si el cliente ha proporcionado un encabezado `Accept` que solicita un formato diferente disponible.</span><span class="sxs-lookup"><span data-stu-id="03200-205">The `[Produces]` filter will force all actions within the `AuthorsController` to return JSON-formatted responses, even if other formatters were configured for the application and the client provided an `Accept` header requesting a different, available format.</span></span> <span data-ttu-id="03200-206">Vea [Filtros](xref:mvc/controllers/filters) para obtener más información, incluida la forma de aplicar filtros globales.</span><span class="sxs-lookup"><span data-stu-id="03200-206">See [Filters](xref:mvc/controllers/filters) to learn more, including how to apply filters globally.</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="03200-207">Formateadores de casos especiales</span><span class="sxs-lookup"><span data-stu-id="03200-207">Special Case Formatters</span></span>

<span data-ttu-id="03200-208">Algunos casos especiales se implementan mediante formateadores integrados.</span><span class="sxs-lookup"><span data-stu-id="03200-208">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="03200-209">De forma predeterminada, los tipos de valor devueltos `string` se formatearán como *texto/sin formato* (*texto/html* si se solicita a través del encabezado `Accept`).</span><span class="sxs-lookup"><span data-stu-id="03200-209">By default, `string` return types will be formatted as *text/plain* (*text/html* if requested via `Accept` header).</span></span> <span data-ttu-id="03200-210">Este comportamiento se puede quitar mediante la eliminación de `TextOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="03200-210">This behavior can be removed by removing the `TextOutputFormatter`.</span></span> <span data-ttu-id="03200-211">Los formateadores deben quitarse del método `Configure` de *Startup.cs* (como se muestra a continuación).</span><span class="sxs-lookup"><span data-stu-id="03200-211">You remove formatters in the `Configure` method in *Startup.cs* (shown below).</span></span> <span data-ttu-id="03200-212">Las acciones que tienen un tipo de valor devuelto de objeto de modelo devolverán una respuesta "204 Sin contenido" al devolver `null`.</span><span class="sxs-lookup"><span data-stu-id="03200-212">Actions that have a model object return type will return a 204 No Content response when returning `null`.</span></span> <span data-ttu-id="03200-213">Este comportamiento se puede quitar mediante la eliminación de `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="03200-213">This behavior can be removed by removing the `HttpNoContentOutputFormatter`.</span></span> <span data-ttu-id="03200-214">El código siguiente quita `TextOutputFormatter` y `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="03200-214">The following code removes the `TextOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

<span data-ttu-id="03200-215">Sin `TextOutputFormatter`, los tipos de valor devueltos `string` devuelven "406 - No aceptable", por ejemplo.</span><span class="sxs-lookup"><span data-stu-id="03200-215">Without the `TextOutputFormatter`, `string` return types return 406 Not Acceptable, for example.</span></span> <span data-ttu-id="03200-216">Tenga en cuenta que si existe un formateador XML, dará formato a los tipos de valor devueltos `string` si se quita `TextOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="03200-216">Note that if an XML formatter exists, it will format `string` return types if the `TextOutputFormatter` is removed.</span></span>

<span data-ttu-id="03200-217">Sin `HttpNoContentOutputFormatter`, se da formato a los objetos nulos mediante el formateador configurado.</span><span class="sxs-lookup"><span data-stu-id="03200-217">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="03200-218">Por ejemplo, el formateador JSON simplemente devolverá una respuesta con un cuerpo `null`, mientras que el formateador XML devolverá un elemento XML vacío con el atributo `xsi:nil="true"` establecido.</span><span class="sxs-lookup"><span data-stu-id="03200-218">For example, the JSON formatter will simply return a response with a body of `null`, while the XML formatter will return an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="03200-219">Asignaciones de direcciones URL de formato de respuesta</span><span class="sxs-lookup"><span data-stu-id="03200-219">Response Format URL Mappings</span></span>

<span data-ttu-id="03200-220">Los clientes pueden solicitar un formato determinado como parte de la dirección URL, como en la cadena de consulta o en una parte de la ruta de acceso, o mediante el uso de una extensión de archivo de formato específico, como .xml o .json.</span><span class="sxs-lookup"><span data-stu-id="03200-220">Clients can request a particular format as part of the URL, such as in the query string or part of the path, or by using a format-specific file extension such as .xml or .json.</span></span> <span data-ttu-id="03200-221">La asignación de la ruta de acceso de la solicitud debe especificarse en la ruta que use la API.</span><span class="sxs-lookup"><span data-stu-id="03200-221">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="03200-222">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="03200-222">For example:</span></span>

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

<span data-ttu-id="03200-223">Esta ruta permitiría especificar el formato solicitado como una extensión de archivo opcional.</span><span class="sxs-lookup"><span data-stu-id="03200-223">This route would allow the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="03200-224">El atributo `[FormatFilter]` comprueba la existencia del valor de formato en `RouteData` y asignará el formato de respuesta al formateador adecuado cuando se cree la respuesta.</span><span class="sxs-lookup"><span data-stu-id="03200-224">The `[FormatFilter]` attribute checks for the existence of the format value in the `RouteData` and will map the response format to the appropriate formatter when the response is created.</span></span>

|           <span data-ttu-id="03200-225">Ruta</span><span class="sxs-lookup"><span data-stu-id="03200-225">Route</span></span>            |             <span data-ttu-id="03200-226">Formateador</span><span class="sxs-lookup"><span data-stu-id="03200-226">Formatter</span></span>              |
|----------------------------|------------------------------------|
|   `/products/GetById/5`    |    <span data-ttu-id="03200-227">Formateador de salida predeterminado</span><span class="sxs-lookup"><span data-stu-id="03200-227">The default output formatter</span></span>    |
| `/products/GetById/5.json` | <span data-ttu-id="03200-228">Formateador JSON (si está configurado)</span><span class="sxs-lookup"><span data-stu-id="03200-228">The JSON formatter (if configured)</span></span> |
| `/products/GetById/5.xml`  | <span data-ttu-id="03200-229">Formateador XML (si está configurado)</span><span class="sxs-lookup"><span data-stu-id="03200-229">The XML formatter (if configured)</span></span>  |
