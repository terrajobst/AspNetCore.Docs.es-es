---
title: Enlace de modelos en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo funciona el enlace de modelos en ASP.NET Core y cómo personalizar su comportamiento.
ms.assetid: 0be164aa-1d72-4192-bd6b-192c9c301164
ms.author: riande
ms.date: 11/15/2019
uid: mvc/models/model-binding
ms.openlocfilehash: a025419a5b4d2c2e3e5c5a7850df281ddd3164ea
ms.sourcegitcommit: f91d322f790123d41ec3271fa084ae20ed9f89a6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/18/2019
ms.locfileid: "74155041"
---
# <a name="model-binding-in-aspnet-core"></a><span data-ttu-id="0818d-103">Enlace de modelos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0818d-103">Model Binding in ASP.NET Core</span></span>

<span data-ttu-id="0818d-104">En este artículo se explica qué es el enlace de modelos, cómo funciona y cómo personalizar su comportamiento.</span><span class="sxs-lookup"><span data-stu-id="0818d-104">This article explains what model binding is, how it works, and how to customize its behavior.</span></span>

<span data-ttu-id="0818d-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="0818d-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="what-is-model-binding"></a><span data-ttu-id="0818d-106">Qué es el enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="0818d-106">What is Model binding</span></span>

<span data-ttu-id="0818d-107">Los controladores y Razor Pages trabajan con datos procedentes de solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="0818d-107">Controllers and Razor pages work with data that comes from HTTP requests.</span></span> <span data-ttu-id="0818d-108">Por ejemplo, los datos de ruta pueden proporcionar una clave de registro y los campos de formulario publicados pueden proporcionar valores para las propiedades del modelo.</span><span class="sxs-lookup"><span data-stu-id="0818d-108">For example, route data may provide a record key, and posted form fields may provide values for the properties of the model.</span></span> <span data-ttu-id="0818d-109">La escritura de código para recuperar cada uno de estos valores y convertirlos de cadenas a tipos de .NET sería tediosa y propensa a errores.</span><span class="sxs-lookup"><span data-stu-id="0818d-109">Writing code to retrieve each of these values and convert them from strings to .NET types would be tedious and error-prone.</span></span> <span data-ttu-id="0818d-110">El enlace de modelos automatiza este proceso.</span><span class="sxs-lookup"><span data-stu-id="0818d-110">Model binding automates this process.</span></span> <span data-ttu-id="0818d-111">El sistema de enlace de modelos:</span><span class="sxs-lookup"><span data-stu-id="0818d-111">The model binding system:</span></span>

* <span data-ttu-id="0818d-112">Recupera datos de diversos orígenes, como datos de ruta, campos de formulario y cadenas de consulta.</span><span class="sxs-lookup"><span data-stu-id="0818d-112">Retrieves data from various sources such as route data, form fields, and query strings.</span></span>
* <span data-ttu-id="0818d-113">Proporciona los datos a los controladores y Razor Pages en parámetros de método y propiedades públicas.</span><span class="sxs-lookup"><span data-stu-id="0818d-113">Provides the data to controllers and Razor pages in method parameters and public properties.</span></span>
* <span data-ttu-id="0818d-114">Convierte datos de cadena en tipos de .NET.</span><span class="sxs-lookup"><span data-stu-id="0818d-114">Converts string data to .NET types.</span></span>
* <span data-ttu-id="0818d-115">Actualiza las propiedades de tipos complejos.</span><span class="sxs-lookup"><span data-stu-id="0818d-115">Updates properties of complex types.</span></span>

## <a name="example"></a><span data-ttu-id="0818d-116">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="0818d-116">Example</span></span>

<span data-ttu-id="0818d-117">Imagine que tiene el siguiente método de acción:</span><span class="sxs-lookup"><span data-stu-id="0818d-117">Suppose you have the following action method:</span></span>

[!code-csharp[](model-binding/samples/2.x/Controllers/PetsController.cs?name=snippet_DogsOnly)]

<span data-ttu-id="0818d-118">Y que la aplicación recibe una solicitud con esta dirección URL:</span><span class="sxs-lookup"><span data-stu-id="0818d-118">And the app receives a request with this URL:</span></span>

```
http://contoso.com/api/pets/2?DogsOnly=true
```

<span data-ttu-id="0818d-119">El enlace de modelos realiza los pasos siguientes después de que el sistema de enrutamiento selecciona el método de acción:</span><span class="sxs-lookup"><span data-stu-id="0818d-119">Model binding goes through the following steps after the routing system selects the action method:</span></span>

* <span data-ttu-id="0818d-120">Busca el primer parámetro de `GetByID`, un entero denominado `id`.</span><span class="sxs-lookup"><span data-stu-id="0818d-120">Finds the first parameter of `GetByID`, an integer named `id`.</span></span>
* <span data-ttu-id="0818d-121">Examina los orígenes disponibles en la solicitud HTTP y busca `id` = "2" en los datos de ruta.</span><span class="sxs-lookup"><span data-stu-id="0818d-121">Looks through the available sources in the HTTP request and finds `id` = "2" in route data.</span></span>
* <span data-ttu-id="0818d-122">Convierte la cadena "2" en el entero 2.</span><span class="sxs-lookup"><span data-stu-id="0818d-122">Converts the string "2" into integer 2.</span></span>
* <span data-ttu-id="0818d-123">Busca el siguiente parámetro de `GetByID`, un valor booleano denominado `dogsOnly`.</span><span class="sxs-lookup"><span data-stu-id="0818d-123">Finds the next parameter of `GetByID`, a boolean named `dogsOnly`.</span></span>
* <span data-ttu-id="0818d-124">Examina los orígenes y busca "DogsOnly=true" en la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="0818d-124">Looks through the sources and finds "DogsOnly=true" in the query string.</span></span> <span data-ttu-id="0818d-125">La coincidencia de nombres no distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="0818d-125">Name matching is not case-sensitive.</span></span>
* <span data-ttu-id="0818d-126">Convierte la cadena "true" en un valor booleano `true`.</span><span class="sxs-lookup"><span data-stu-id="0818d-126">Converts the string "true" into boolean `true`.</span></span>

<span data-ttu-id="0818d-127">Después, el marco llama al método `GetById` y pasa 2 para el parámetro `id` y `true` para el parámetro `dogsOnly`.</span><span class="sxs-lookup"><span data-stu-id="0818d-127">The framework then calls the `GetById` method, passing in 2 for the `id` parameter, and `true` for the `dogsOnly` parameter.</span></span>

<span data-ttu-id="0818d-128">En el ejemplo anterior, los destinos de enlace de modelos son parámetros de método que son tipos simples.</span><span class="sxs-lookup"><span data-stu-id="0818d-128">In the preceding example, the model binding targets are method parameters that are simple types.</span></span> <span data-ttu-id="0818d-129">Los destinos también pueden ser las propiedades de un tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="0818d-129">Targets may also be the properties of a complex type.</span></span> <span data-ttu-id="0818d-130">Después de que cada propiedad se haya enlazado correctamente, se produce la [validación de modelos](xref:mvc/models/validation) para esa propiedad.</span><span class="sxs-lookup"><span data-stu-id="0818d-130">After each property is successfully bound, [model validation](xref:mvc/models/validation) occurs for that property.</span></span> <span data-ttu-id="0818d-131">El registro de qué datos se han enlazado al modelo y los errores de enlace o validación se almacenan en [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) o [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span><span class="sxs-lookup"><span data-stu-id="0818d-131">The record of what data is bound to the model, and any binding or validation errors, is stored in [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) or [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span></span> <span data-ttu-id="0818d-132">Para averiguar si este proceso se ha realizado correctamente, la aplicación comprueba la marca [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid).</span><span class="sxs-lookup"><span data-stu-id="0818d-132">To find out if this process was successful, the app checks the [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) flag.</span></span>

## <a name="targets"></a><span data-ttu-id="0818d-133">Destinos</span><span class="sxs-lookup"><span data-stu-id="0818d-133">Targets</span></span>

<span data-ttu-id="0818d-134">El enlace de modelos intenta encontrar valores para los tipos de destinos siguientes:</span><span class="sxs-lookup"><span data-stu-id="0818d-134">Model binding tries to find values for the following kinds of targets:</span></span>

* <span data-ttu-id="0818d-135">Parámetros del método de acción de controlador al que se enruta una solicitud.</span><span class="sxs-lookup"><span data-stu-id="0818d-135">Parameters of the controller action method that a request is routed to.</span></span>
* <span data-ttu-id="0818d-136">Parámetros del método de controlador de Razor Pages al que se enruta una solicitud.</span><span class="sxs-lookup"><span data-stu-id="0818d-136">Parameters of the Razor Pages handler method that a request is routed to.</span></span> 
* <span data-ttu-id="0818d-137">Propiedades públicas de un controlador o una clase `PageModel`, si se especifican mediante atributos.</span><span class="sxs-lookup"><span data-stu-id="0818d-137">Public properties of a controller or `PageModel` class, if specified by attributes.</span></span>

### <a name="bindproperty-attribute"></a><span data-ttu-id="0818d-138">Atributo [BindProperty]</span><span class="sxs-lookup"><span data-stu-id="0818d-138">[BindProperty] attribute</span></span>

<span data-ttu-id="0818d-139">Se puede aplicar a una propiedad pública de un controlador o una clase `PageModel` para hacer que el enlace de modelos tenga esa propiedad como destino:</span><span class="sxs-lookup"><span data-stu-id="0818d-139">Can be applied to a public property of a controller or `PageModel` class to cause model binding to target that property:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Edit.cshtml.cs?name=snippet_BindProperty&highlight=7-8)]

### <a name="bindpropertiesattribute"></a><span data-ttu-id="0818d-140">Atributo [BindProperties]</span><span class="sxs-lookup"><span data-stu-id="0818d-140">[BindProperties] attribute</span></span>

<span data-ttu-id="0818d-141">Disponible en ASP.NET 2.1 Core y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="0818d-141">Available in ASP.NET Core 2.1 and later.</span></span>  <span data-ttu-id="0818d-142">Se pueden aplicar a un controlador o una clase `PageModel` para indicar al enlace de modelos que seleccione como destino todas las propiedades públicas de la clase:</span><span class="sxs-lookup"><span data-stu-id="0818d-142">Can be applied to a controller or `PageModel` class to tell model binding to target all public properties of the class:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Create.cshtml.cs?name=snippet_BindProperties&highlight=1-2)]

### <a name="model-binding-for-http-get-requests"></a><span data-ttu-id="0818d-143">Enlace de modelos para solicitudes HTTP GET</span><span class="sxs-lookup"><span data-stu-id="0818d-143">Model binding for HTTP GET requests</span></span>

<span data-ttu-id="0818d-144">De forma predeterminada, las propiedades no se enlazan para las solicitudes HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="0818d-144">By default, properties are not bound for HTTP GET requests.</span></span> <span data-ttu-id="0818d-145">Normalmente, todo lo que necesita para una solicitud GET es un parámetro de id. de registro.</span><span class="sxs-lookup"><span data-stu-id="0818d-145">Typically, all you need for a GET request is a record ID parameter.</span></span> <span data-ttu-id="0818d-146">El id. de registro se usa para buscar el elemento en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0818d-146">The record ID is used to look up the item in the database.</span></span> <span data-ttu-id="0818d-147">Por tanto, no es necesario enlazar una propiedad que contiene una instancia del modelo.</span><span class="sxs-lookup"><span data-stu-id="0818d-147">Therefore, there is no need to bind a property that holds an instance of the model.</span></span> <span data-ttu-id="0818d-148">En escenarios donde quiera propiedades enlazadas a datos de las solicitudes GET, establezca la propiedad `SupportsGet` en `true`:</span><span class="sxs-lookup"><span data-stu-id="0818d-148">In scenarios where you do want properties bound to data from GET requests, set the `SupportsGet` property to `true`:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Index.cshtml.cs?name=snippet_SupportsGet)]

## <a name="sources"></a><span data-ttu-id="0818d-149">Orígenes</span><span class="sxs-lookup"><span data-stu-id="0818d-149">Sources</span></span>

<span data-ttu-id="0818d-150">De forma predeterminada, el enlace de modelos obtiene datos en forma de pares clave-valor de los siguientes orígenes de una solicitud HTTP:</span><span class="sxs-lookup"><span data-stu-id="0818d-150">By default, model binding gets data in the form of key-value pairs from the following sources in an HTTP request:</span></span>

1. <span data-ttu-id="0818d-151">Campos de formulario</span><span class="sxs-lookup"><span data-stu-id="0818d-151">Form fields</span></span> 
1. <span data-ttu-id="0818d-152">El cuerpo de la solicitud (para [controladores que tienen el atributo [ApiController]](xref:web-api/index#binding-source-parameter-inference)).</span><span class="sxs-lookup"><span data-stu-id="0818d-152">The request body (For [controllers that have the [ApiController] attribute](xref:web-api/index#binding-source-parameter-inference).)</span></span>
1. <span data-ttu-id="0818d-153">Datos de ruta</span><span class="sxs-lookup"><span data-stu-id="0818d-153">Route data</span></span>
1. <span data-ttu-id="0818d-154">Parámetros de cadena de consulta</span><span class="sxs-lookup"><span data-stu-id="0818d-154">Query string parameters</span></span>
1. <span data-ttu-id="0818d-155">Archivos cargados</span><span class="sxs-lookup"><span data-stu-id="0818d-155">Uploaded files</span></span> 

<span data-ttu-id="0818d-156">Para cada parámetro o propiedad de destino, se examinan los orígenes en el orden indicado en esta lista.</span><span class="sxs-lookup"><span data-stu-id="0818d-156">For each target parameter or property, the sources are scanned in the order indicated in this list.</span></span> <span data-ttu-id="0818d-157">Hay algunas excepciones:</span><span class="sxs-lookup"><span data-stu-id="0818d-157">There are a few exceptions:</span></span>

* <span data-ttu-id="0818d-158">Los datos de ruta y los valores de cadena de consulta solo se usan para tipos simples.</span><span class="sxs-lookup"><span data-stu-id="0818d-158">Route data and query string values are used only for simple types.</span></span>
* <span data-ttu-id="0818d-159">Los archivos cargados solo se enlazan a tipos de destino que implementan `IFormFile` o `IEnumerable<IFormFile>`.</span><span class="sxs-lookup"><span data-stu-id="0818d-159">Uploaded files are bound only to target types that implement `IFormFile` or `IEnumerable<IFormFile>`.</span></span>

<span data-ttu-id="0818d-160">Si el comportamiento predeterminado no proporciona los resultados adecuados, puede usar uno de los atributos siguientes para especificar el origen que se va a usar para cualquier destino dado.</span><span class="sxs-lookup"><span data-stu-id="0818d-160">If the default behavior doesn't give the right results, you can use one of the following attributes to specify the source to use for any given target.</span></span> 

* <span data-ttu-id="0818d-161">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute): obtiene valores de la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="0818d-161">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) - Gets values from the query string.</span></span> 
* <span data-ttu-id="0818d-162">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute): obtiene valores de los datos de ruta.</span><span class="sxs-lookup"><span data-stu-id="0818d-162">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) - Gets values from route data.</span></span>
* <span data-ttu-id="0818d-163">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute): obtiene valores de los campos de formulario publicados.</span><span class="sxs-lookup"><span data-stu-id="0818d-163">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) - Gets values from posted form fields.</span></span>
* <span data-ttu-id="0818d-164">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute): obtiene valores del cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="0818d-164">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) - Gets values from the request body.</span></span>
* <span data-ttu-id="0818d-165">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute): obtiene valores de encabezados HTTP.</span><span class="sxs-lookup"><span data-stu-id="0818d-165">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) - Gets values from HTTP headers.</span></span>

<span data-ttu-id="0818d-166">Estos atributos:</span><span class="sxs-lookup"><span data-stu-id="0818d-166">These attributes:</span></span>

* <span data-ttu-id="0818d-167">Se agregan de forma individual a las propiedades del modelo (no a la clase de modelo), como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="0818d-167">Are added to model properties individually (not to the model class), as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/Models/Instructor.cs?name=snippet_FromQuery&highlight=5-6)]

* <span data-ttu-id="0818d-168">Opcionalmente, acepte un valor de nombre de modelo en el constructor.</span><span class="sxs-lookup"><span data-stu-id="0818d-168">Optionally accept a model name value in the constructor.</span></span> <span data-ttu-id="0818d-169">Esta opción se proporciona en caso de que el nombre de la propiedad no coincida con el valor de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="0818d-169">This option is provided in case the property name doesn't match the value in the request.</span></span> <span data-ttu-id="0818d-170">Por ejemplo, es posible que el valor de la solicitud sea un encabezado con un guion en el nombre, como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="0818d-170">For instance, the value in the request might be a header with a hyphen in its name, as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Index.cshtml.cs?name=snippet_FromHeader)]

### <a name="frombody-attribute"></a><span data-ttu-id="0818d-171">Atributo [FromBody]</span><span class="sxs-lookup"><span data-stu-id="0818d-171">[FromBody] attribute</span></span>

<span data-ttu-id="0818d-172">Los datos del cuerpo de la solicitud se analizan mediante formateadores de entrada específicos para el tipo de contenido de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="0818d-172">The request body data is parsed by using input formatters specific to the content type of the request.</span></span> <span data-ttu-id="0818d-173">Los formateadores de entrada se explican [más adelante en este artículo](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="0818d-173">Input formatters are explained [later in this article](#input-formatters).</span></span>

<span data-ttu-id="0818d-174">No aplique `[FromBody]` a más de un parámetro por método de acción.</span><span class="sxs-lookup"><span data-stu-id="0818d-174">Don't apply `[FromBody]` to more than one parameter per action method.</span></span> <span data-ttu-id="0818d-175">El runtime de ASP.NET Core delega la responsabilidad de leer la secuencia de solicitudes al formateador de entrada.</span><span class="sxs-lookup"><span data-stu-id="0818d-175">The ASP.NET Core runtime delegates the responsibility of reading the request stream to the input formatter.</span></span> <span data-ttu-id="0818d-176">Una vez que se ha leído la secuencia de solicitudes, deja de estar disponible para una nueva lectura con el fin de enlazar otros parámetros `[FromBody]`.</span><span class="sxs-lookup"><span data-stu-id="0818d-176">Once the request stream is read, it's no longer available to be read again for binding other `[FromBody]` parameters.</span></span>

### <a name="additional-sources"></a><span data-ttu-id="0818d-177">Orígenes adicionales</span><span class="sxs-lookup"><span data-stu-id="0818d-177">Additional sources</span></span>

<span data-ttu-id="0818d-178">Los *proveedores de valores* proporcionan datos de origen al sistema de enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="0818d-178">Source data is provided to the model binding system by *value providers*.</span></span> <span data-ttu-id="0818d-179">Puede escribir y registrar proveedores de valores personalizados que obtienen datos de otros orígenes para el enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="0818d-179">You can write and register custom value providers that get data for model binding from other sources.</span></span> <span data-ttu-id="0818d-180">Por ejemplo, es posible que le interesen datos de cookies o del estado de sesión.</span><span class="sxs-lookup"><span data-stu-id="0818d-180">For example, you might want data from cookies or session state.</span></span> <span data-ttu-id="0818d-181">Para obtener datos desde un origen nuevo:</span><span class="sxs-lookup"><span data-stu-id="0818d-181">To get data from a new source:</span></span>

* <span data-ttu-id="0818d-182">Cree una clase que implemente `IValueProvider`.</span><span class="sxs-lookup"><span data-stu-id="0818d-182">Create a class that implements `IValueProvider`.</span></span>
* <span data-ttu-id="0818d-183">Cree una clase que implemente `IValueProviderFactory`.</span><span class="sxs-lookup"><span data-stu-id="0818d-183">Create a class that implements `IValueProviderFactory`.</span></span>
* <span data-ttu-id="0818d-184">Registre la clase de generador en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0818d-184">Register the factory class in `Startup.ConfigureServices`.</span></span>

<span data-ttu-id="0818d-185">En la aplicación de ejemplo se incluye un [proveedor de valores](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProvider.cs) y un [generador](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProviderFactory.cs) que obtiene valores de cookies.</span><span class="sxs-lookup"><span data-stu-id="0818d-185">The sample app includes a [value provider](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProvider.cs) and [factory](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProviderFactory.cs) example that gets values from cookies.</span></span> <span data-ttu-id="0818d-186">Este es el código de registro de `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0818d-186">Here's the registration code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=3)]

<span data-ttu-id="0818d-187">En el código mostrado, el proveedor de valor personalizado se coloca después de todos los proveedores de valor integrados.</span><span class="sxs-lookup"><span data-stu-id="0818d-187">The code shown puts the custom value provider after all the built-in value providers.</span></span>  <span data-ttu-id="0818d-188">Para que sea el primero en la lista, llame a `Insert(0, new CookieValueProviderFactory())` en lugar de a `Add`.</span><span class="sxs-lookup"><span data-stu-id="0818d-188">To make it the first in the list, call `Insert(0, new CookieValueProviderFactory())` instead of `Add`.</span></span>

## <a name="no-source-for-a-model-property"></a><span data-ttu-id="0818d-189">No hay origen para una propiedad de modelo</span><span class="sxs-lookup"><span data-stu-id="0818d-189">No source for a model property</span></span>

<span data-ttu-id="0818d-190">De forma predeterminada, si no se encuentra ningún valor para una propiedad de modelo no se crea un error de estado del modelo.</span><span class="sxs-lookup"><span data-stu-id="0818d-190">By default, a model state error isn't created if no value is found for a model property.</span></span> <span data-ttu-id="0818d-191">La propiedad se establece en NULL o en un valor predeterminado:</span><span class="sxs-lookup"><span data-stu-id="0818d-191">The property is set to null or a default value:</span></span>

* <span data-ttu-id="0818d-192">Los tipos simples que aceptan valores NULL se establecen en `null`.</span><span class="sxs-lookup"><span data-stu-id="0818d-192">Nullable simple types are set to `null`.</span></span>
* <span data-ttu-id="0818d-193">Los tipos de valor que no aceptan valores NULL se establecen en `default(T)`.</span><span class="sxs-lookup"><span data-stu-id="0818d-193">Non-nullable value types are set to `default(T)`.</span></span> <span data-ttu-id="0818d-194">Por ejemplo, un parámetro `int id` se establece en 0.</span><span class="sxs-lookup"><span data-stu-id="0818d-194">For example, a parameter `int id` is set to 0.</span></span>
* <span data-ttu-id="0818d-195">Para los tipos complejos, el enlace de modelos crea una instancia mediante el constructor predeterminado, sin establecer propiedades.</span><span class="sxs-lookup"><span data-stu-id="0818d-195">For complex Types, model binding creates an instance by using the default constructor, without setting properties.</span></span>
* <span data-ttu-id="0818d-196">Las matrices se establecen en `Array.Empty<T>()`, salvo las matrices `byte[]`, que se establecen en `null`.</span><span class="sxs-lookup"><span data-stu-id="0818d-196">Arrays are set to `Array.Empty<T>()`, except that `byte[]` arrays are set to `null`.</span></span>

<span data-ttu-id="0818d-197">Si el estado del modelo se debe invalidar cuando no se encuentra nada en los campos de formulario para una propiedad de modelo, use el [atributo [BindRequired]](#bindrequired-attribute).</span><span class="sxs-lookup"><span data-stu-id="0818d-197">If model state should be invalidated when nothing is found in form fields for a model property, use the [[BindRequired] attribute](#bindrequired-attribute).</span></span>

<span data-ttu-id="0818d-198">Tenga en cuenta que este comportamiento de `[BindRequired]` se aplica al enlace de modelos desde datos de formulario publicados, no a los datos JSON o XML del cuerpo de una solicitud.</span><span class="sxs-lookup"><span data-stu-id="0818d-198">Note that this `[BindRequired]` behavior applies to model binding from posted form data, not to JSON or XML data in a request body.</span></span> <span data-ttu-id="0818d-199">Los datos del cuerpo de la solicitud se controlan mediante [formateadores de entrada](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="0818d-199">Request body data is handled by [input formatters](#input-formatters).</span></span>

## <a name="type-conversion-errors"></a><span data-ttu-id="0818d-200">Errores de la conversión de tipos</span><span class="sxs-lookup"><span data-stu-id="0818d-200">Type conversion errors</span></span>

<span data-ttu-id="0818d-201">Si se encuentra un origen pero no se puede convertir al tipo de destino, el estado del modelo se marca como no válido.</span><span class="sxs-lookup"><span data-stu-id="0818d-201">If a source is found but can't be converted into the target type, model state is flagged as invalid.</span></span> <span data-ttu-id="0818d-202">El parámetro o la propiedad de destino se establece en NULL o en un valor predeterminado, como se ha indicado en la sección anterior.</span><span class="sxs-lookup"><span data-stu-id="0818d-202">The target parameter or property is set to null or a default value, as noted in the previous section.</span></span>

<span data-ttu-id="0818d-203">En un controlador de API que tenga el atributo `[ApiController]`, el estado de modelo no válido genera una respuesta HTTP 400 automática.</span><span class="sxs-lookup"><span data-stu-id="0818d-203">In an API controller that has the `[ApiController]` attribute, invalid model state results in an automatic HTTP 400 response.</span></span>

<span data-ttu-id="0818d-204">En una página de Razor, se vuelve a mostrar la página con un mensaje de error:</span><span class="sxs-lookup"><span data-stu-id="0818d-204">In a Razor page, redisplay the page with an error message:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Create.cshtml.cs?name=snippet_HandleMBError&highlight=3-6)]

<span data-ttu-id="0818d-205">La validación del lado cliente detecta la mayoría de los datos incorrectos que en otros casos se enviarían a un formulario de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0818d-205">Client-side validation catches most bad data that would otherwise be submitted to a Razor Pages form.</span></span> <span data-ttu-id="0818d-206">Esta validación dificulta que se pueda desencadenar el código resaltado anterior.</span><span class="sxs-lookup"><span data-stu-id="0818d-206">This validation makes it hard to trigger the preceding highlighted code.</span></span> <span data-ttu-id="0818d-207">En la aplicación de ejemplo se incluye un botón **Submit with Invalid Date** (Enviar con fecha no válida) que agrega datos incorrectos al campo **Hire Date** (Fecha de contratación) y envía el formulario.</span><span class="sxs-lookup"><span data-stu-id="0818d-207">The sample app includes a **Submit with Invalid Date** button that puts bad data in the **Hire Date** field and submits the form.</span></span> <span data-ttu-id="0818d-208">Este botón muestra cómo funciona el código para volver a mostrar la página cuando se producen errores de conversión de datos.</span><span class="sxs-lookup"><span data-stu-id="0818d-208">This button shows how the code for redisplaying the page works when data conversion errors occur.</span></span>

<span data-ttu-id="0818d-209">Cuando el código anterior vuelve a mostrar la página, no se muestra la entrada no válida en el campo de formulario.</span><span class="sxs-lookup"><span data-stu-id="0818d-209">When the page is redisplayed by the preceding code, the invalid input is not shown in the form field.</span></span> <span data-ttu-id="0818d-210">El motivo es que la propiedad de modelo se ha establecido en NULL o en un valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="0818d-210">This is because the model property has been set to null or a default value.</span></span> <span data-ttu-id="0818d-211">La entrada no válida sí aparece en un mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="0818d-211">The invalid input does appear in an error message.</span></span> <span data-ttu-id="0818d-212">Pero si quiere volver a mostrar los datos incorrectos en el campo de formulario, considere la posibilidad de convertir la propiedad de modelo en una cadena y realizar la conversión de datos de forma manual.</span><span class="sxs-lookup"><span data-stu-id="0818d-212">But if you want to redisplay the bad data in the form field, consider making the model property a string and doing the data conversion manually.</span></span>

<span data-ttu-id="0818d-213">Se recomienda la misma estrategia si no quiere que los errores de conversión de tipo generen errores de estado de modelo.</span><span class="sxs-lookup"><span data-stu-id="0818d-213">The same strategy is recommended if you don't want type conversion errors to result in model state errors.</span></span> <span data-ttu-id="0818d-214">En ese caso, convierta la propiedad de modelo en una cadena.</span><span class="sxs-lookup"><span data-stu-id="0818d-214">In that case, make the model property a string.</span></span>

## <a name="simple-types"></a><span data-ttu-id="0818d-215">Tipos simples</span><span class="sxs-lookup"><span data-stu-id="0818d-215">Simple types</span></span>

<span data-ttu-id="0818d-216">Los tipos simples a los que el enlazador de modelos puede convertir las cadenas de origen incluyen los siguientes:</span><span class="sxs-lookup"><span data-stu-id="0818d-216">The simple types that the model binder can convert source strings into include the following:</span></span>

* [<span data-ttu-id="0818d-217">Boolean</span><span class="sxs-lookup"><span data-stu-id="0818d-217">Boolean</span></span>](xref:System.ComponentModel.BooleanConverter)
* <span data-ttu-id="0818d-218">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span><span class="sxs-lookup"><span data-stu-id="0818d-218">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span></span>
* [<span data-ttu-id="0818d-219">Char</span><span class="sxs-lookup"><span data-stu-id="0818d-219">Char</span></span>](xref:System.ComponentModel.CharConverter)
* [<span data-ttu-id="0818d-220">DateTime</span><span class="sxs-lookup"><span data-stu-id="0818d-220">DateTime</span></span>](xref:System.ComponentModel.DateTimeConverter)
* [<span data-ttu-id="0818d-221">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="0818d-221">DateTimeOffset</span></span>](xref:System.ComponentModel.DateTimeOffsetConverter)
* [<span data-ttu-id="0818d-222">Decimal</span><span class="sxs-lookup"><span data-stu-id="0818d-222">Decimal</span></span>](xref:System.ComponentModel.DecimalConverter)
* [<span data-ttu-id="0818d-223">Double</span><span class="sxs-lookup"><span data-stu-id="0818d-223">Double</span></span>](xref:System.ComponentModel.DoubleConverter)
* [<span data-ttu-id="0818d-224">Enum</span><span class="sxs-lookup"><span data-stu-id="0818d-224">Enum</span></span>](xref:System.ComponentModel.EnumConverter)
* [<span data-ttu-id="0818d-225">Guid</span><span class="sxs-lookup"><span data-stu-id="0818d-225">Guid</span></span>](xref:System.ComponentModel.GuidConverter)
* <span data-ttu-id="0818d-226">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span><span class="sxs-lookup"><span data-stu-id="0818d-226">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span></span>
* [<span data-ttu-id="0818d-227">Single</span><span class="sxs-lookup"><span data-stu-id="0818d-227">Single</span></span>](xref:System.ComponentModel.SingleConverter)
* [<span data-ttu-id="0818d-228">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="0818d-228">TimeSpan</span></span>](xref:System.ComponentModel.TimeSpanConverter)
* <span data-ttu-id="0818d-229">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span><span class="sxs-lookup"><span data-stu-id="0818d-229">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span></span>
* [<span data-ttu-id="0818d-230">Uri</span><span class="sxs-lookup"><span data-stu-id="0818d-230">Uri</span></span>](xref:System.UriTypeConverter)
* [<span data-ttu-id="0818d-231">Versión</span><span class="sxs-lookup"><span data-stu-id="0818d-231">Version</span></span>](xref:System.ComponentModel.VersionConverter)

## <a name="complex-types"></a><span data-ttu-id="0818d-232">Tipos complejos</span><span class="sxs-lookup"><span data-stu-id="0818d-232">Complex types</span></span>

<span data-ttu-id="0818d-233">Un tipo complejo debe tener un constructor público predeterminado y propiedades grabables públicas para enlazar.</span><span class="sxs-lookup"><span data-stu-id="0818d-233">A complex type must have a public default constructor and public writable properties to bind.</span></span> <span data-ttu-id="0818d-234">Cuando se produce el enlace de modelos, se crea una instancia de la clase con el constructor predeterminado público.</span><span class="sxs-lookup"><span data-stu-id="0818d-234">When model binding occurs, the class is instantiated using the public default constructor.</span></span> 

<span data-ttu-id="0818d-235">Para cada propiedad del tipo complejo, el enlace de modelos busca entre los orígenes el patrón de nombre *prefijo.nombre_de_propiedad*.</span><span class="sxs-lookup"><span data-stu-id="0818d-235">For each property of the complex type, model binding looks through the sources for the name pattern *prefix.property_name*.</span></span> <span data-ttu-id="0818d-236">Si no se encuentra nada, solo busca *nombre_de_propiedad* sin el prefijo.</span><span class="sxs-lookup"><span data-stu-id="0818d-236">If nothing is found, it looks for just *property_name* without the prefix.</span></span>

<span data-ttu-id="0818d-237">Para el enlace a un parámetro, el prefijo es el nombre del parámetro.</span><span class="sxs-lookup"><span data-stu-id="0818d-237">For binding to a parameter, the prefix is the parameter name.</span></span> <span data-ttu-id="0818d-238">Para el enlace a una propiedad pública `PageModel`, el prefijo es el nombre de la propiedad pública.</span><span class="sxs-lookup"><span data-stu-id="0818d-238">For binding to a `PageModel` public property, the prefix is the public property name.</span></span> <span data-ttu-id="0818d-239">Algunos atributos tienen una propiedad `Prefix` que permite invalidar el uso predeterminado del nombre de parámetro o propiedad.</span><span class="sxs-lookup"><span data-stu-id="0818d-239">Some attributes have a `Prefix` property that lets you override the default usage of parameter or property name.</span></span>

<span data-ttu-id="0818d-240">Por ejemplo, imagine que el tipo complejo es la clase `Instructor` siguiente:</span><span class="sxs-lookup"><span data-stu-id="0818d-240">For example, suppose the complex type is the following `Instructor` class:</span></span>

  ```csharp
  public class Instructor
  {
      public int ID { get; set; }
      public string LastName { get; set; }
      public string FirstName { get; set; }
  }
  ```

### <a name="prefix--parameter-name"></a><span data-ttu-id="0818d-241">Prefijo = nombre del parámetro</span><span class="sxs-lookup"><span data-stu-id="0818d-241">Prefix = parameter name</span></span>

<span data-ttu-id="0818d-242">Si el modelo que se va a enlazar es un parámetro denominado `instructorToUpdate`:</span><span class="sxs-lookup"><span data-stu-id="0818d-242">If the model to be bound is a parameter named `instructorToUpdate`:</span></span>

```csharp
public IActionResult OnPost(int? id, Instructor instructorToUpdate)
```

<span data-ttu-id="0818d-243">El enlace de modelos se inicia mediante el examen de los orígenes para la clave `instructorToUpdate.ID`.</span><span class="sxs-lookup"><span data-stu-id="0818d-243">Model binding starts by looking through the sources for the key `instructorToUpdate.ID`.</span></span> <span data-ttu-id="0818d-244">Si no se encuentra, busca `ID` sin un prefijo.</span><span class="sxs-lookup"><span data-stu-id="0818d-244">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="prefix--property-name"></a><span data-ttu-id="0818d-245">Prefijo = nombre de propiedad</span><span class="sxs-lookup"><span data-stu-id="0818d-245">Prefix = property name</span></span>

<span data-ttu-id="0818d-246">Si el modelo que se va a enlazar es una propiedad denominada `Instructor` del controlador o la clase `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="0818d-246">If the model to be bound is a property named `Instructor` of the controller or `PageModel` class:</span></span>

```csharp
[BindProperty]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="0818d-247">El enlace de modelos se inicia mediante el examen de los orígenes para la clave `Instructor.ID`.</span><span class="sxs-lookup"><span data-stu-id="0818d-247">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="0818d-248">Si no se encuentra, busca `ID` sin un prefijo.</span><span class="sxs-lookup"><span data-stu-id="0818d-248">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="custom-prefix"></a><span data-ttu-id="0818d-249">Prefijo personalizado</span><span class="sxs-lookup"><span data-stu-id="0818d-249">Custom prefix</span></span>

<span data-ttu-id="0818d-250">Si el modelo que se va a enlazar es un parámetro denominado `instructorToUpdate` y un atributo `Bind`, especifica `Instructor` como el prefijo:</span><span class="sxs-lookup"><span data-stu-id="0818d-250">If the model to be bound is a parameter named `instructorToUpdate` and a `Bind` attribute specifies `Instructor` as the prefix:</span></span>

```csharp
public IActionResult OnPost(
    int? id, [Bind(Prefix = "Instructor")] Instructor instructorToUpdate)
```

<span data-ttu-id="0818d-251">El enlace de modelos se inicia mediante el examen de los orígenes para la clave `Instructor.ID`.</span><span class="sxs-lookup"><span data-stu-id="0818d-251">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="0818d-252">Si no se encuentra, busca `ID` sin un prefijo.</span><span class="sxs-lookup"><span data-stu-id="0818d-252">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="attributes-for-complex-type-targets"></a><span data-ttu-id="0818d-253">Atributos para destinos de tipo complejo</span><span class="sxs-lookup"><span data-stu-id="0818d-253">Attributes for complex type targets</span></span>

<span data-ttu-id="0818d-254">Existen varios atributos integrados para controlar el enlace de modelos de tipos complejos:</span><span class="sxs-lookup"><span data-stu-id="0818d-254">Several built-in attributes are available for controlling model binding of complex types:</span></span>

* `[BindRequired]`
* `[BindNever]`
* `[Bind]`

> [!NOTE]
> <span data-ttu-id="0818d-255">Estos atributos afectan al enlace de modelos cuando el origen de los valores son datos de formulario publicados.</span><span class="sxs-lookup"><span data-stu-id="0818d-255">These attributes affect model binding when posted form data is the source of values.</span></span> <span data-ttu-id="0818d-256">No afectan a los formateadores de entrada, que procesan los cuerpos de la solicitud JSON y XML publicados.</span><span class="sxs-lookup"><span data-stu-id="0818d-256">They do not affect input formatters, which process posted JSON and XML request bodies.</span></span> <span data-ttu-id="0818d-257">Los formateadores de entrada se explican [más adelante en este artículo](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="0818d-257">Input formatters are explained [later in this article](#input-formatters).</span></span>
>
> <span data-ttu-id="0818d-258">Vea también la explicación del atributo `[Required]` en [Validación de modelos](xref:mvc/models/validation#required-attribute).</span><span class="sxs-lookup"><span data-stu-id="0818d-258">See also the discussion of the `[Required]` attribute in [Model validation](xref:mvc/models/validation#required-attribute).</span></span>

### <a name="bindrequired-attribute"></a><span data-ttu-id="0818d-259">Atributo [BindRequired]</span><span class="sxs-lookup"><span data-stu-id="0818d-259">[BindRequired] attribute</span></span>

<span data-ttu-id="0818d-260">Solo se puede aplicar a propiedades del modelo, no a parámetros de método.</span><span class="sxs-lookup"><span data-stu-id="0818d-260">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="0818d-261">Hace que el enlace de modelos agregue un error de estado de modelo si no se puede realizar el enlace para la propiedad de un modelo.</span><span class="sxs-lookup"><span data-stu-id="0818d-261">Causes model binding to add a model state error if binding cannot occur for a model's property.</span></span> <span data-ttu-id="0818d-262">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0818d-262">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/Models/InstructorWithCollection.cs?name=snippet_BindRequired&highlight=8-9)]

### <a name="bindnever-attribute"></a><span data-ttu-id="0818d-263">Atributo [BindNever]</span><span class="sxs-lookup"><span data-stu-id="0818d-263">[BindNever] attribute</span></span>

<span data-ttu-id="0818d-264">Solo se puede aplicar a propiedades del modelo, no a parámetros de método.</span><span class="sxs-lookup"><span data-stu-id="0818d-264">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="0818d-265">Impide que el enlace de modelos establezca la propiedad de un modelo.</span><span class="sxs-lookup"><span data-stu-id="0818d-265">Prevents model binding from setting a model's property.</span></span> <span data-ttu-id="0818d-266">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0818d-266">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/Models/InstructorWithDictionary.cs?name=snippet_BindNever&highlight=3-4)]

### <a name="bind-attribute"></a><span data-ttu-id="0818d-267">Atributo [Bind]</span><span class="sxs-lookup"><span data-stu-id="0818d-267">[Bind] attribute</span></span>

<span data-ttu-id="0818d-268">Se puede aplicar a una clase o un parámetro de método.</span><span class="sxs-lookup"><span data-stu-id="0818d-268">Can be applied to a class or a method parameter.</span></span> <span data-ttu-id="0818d-269">Especifica qué propiedades de un modelo se deben incluir en el enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="0818d-269">Specifies which properties of a model should be included in model binding.</span></span>

<span data-ttu-id="0818d-270">En el ejemplo siguiente, solo se enlazan las propiedades especificadas del modelo `Instructor` cuando se llama a cualquier método de acción o controlador:</span><span class="sxs-lookup"><span data-stu-id="0818d-270">In the following example, only the specified properties of the `Instructor` model are bound when any handler or action method is called:</span></span>

```csharp
[Bind("LastName,FirstMidName,HireDate")]
public class Instructor
```

<span data-ttu-id="0818d-271">En el ejemplo siguiente, solo se enlazan las propiedades especificadas del modelo `Instructor` cuando se llama al método `OnPost`:</span><span class="sxs-lookup"><span data-stu-id="0818d-271">In the following example, only the specified properties of the `Instructor` model are bound when the `OnPost` method is called:</span></span>

```csharp
[HttpPost]
public IActionResult OnPost([Bind("LastName,FirstMidName,HireDate")] Instructor instructor)
```

<span data-ttu-id="0818d-272">El atributo `[Bind]` se puede usar para protegerse de la publicación excesiva en escenarios de *creación*.</span><span class="sxs-lookup"><span data-stu-id="0818d-272">The `[Bind]` attribute can be used to protect against overposting in *create* scenarios.</span></span> <span data-ttu-id="0818d-273">No funciona bien en escenarios de edición porque las propiedades excluidas se establecen en NULL o en un valor predeterminado en lugar de mantenerse sin cambios.</span><span class="sxs-lookup"><span data-stu-id="0818d-273">It doesn't work well in edit scenarios because excluded properties are set to null or a default value instead of being left unchanged.</span></span> <span data-ttu-id="0818d-274">Para defenderse de la publicación excesiva, se recomiendan modelos de vista en lugar del atributo `[Bind]`.</span><span class="sxs-lookup"><span data-stu-id="0818d-274">For defense against overposting, view models are recommended rather than the `[Bind]` attribute.</span></span> <span data-ttu-id="0818d-275">Para más información, vea [Nota de seguridad sobre la publicación excesiva](xref:data/ef-mvc/crud#security-note-about-overposting).</span><span class="sxs-lookup"><span data-stu-id="0818d-275">For more information, see [Security note about overposting](xref:data/ef-mvc/crud#security-note-about-overposting).</span></span>

## <a name="collections"></a><span data-ttu-id="0818d-276">Colecciones</span><span class="sxs-lookup"><span data-stu-id="0818d-276">Collections</span></span>

<span data-ttu-id="0818d-277">Para los destinos que son colecciones de tipos simples, el enlace de modelos busca coincidencias con *nombre_de_parámetro* o *nombre_de_propiedad*.</span><span class="sxs-lookup"><span data-stu-id="0818d-277">For targets that are collections of simple types, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="0818d-278">Si no se encuentra ninguna coincidencia, busca uno de los formatos admitidos sin el prefijo.</span><span class="sxs-lookup"><span data-stu-id="0818d-278">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="0818d-279">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0818d-279">For example:</span></span>

* <span data-ttu-id="0818d-280">Imagine que el parámetro que se va a enlazar es una matriz llamada `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="0818d-280">Suppose the parameter to be bound is an array named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, int[] selectedCourses)
  ```

* <span data-ttu-id="0818d-281">Los datos de cadena de consulta o formulario pueden estar en uno de los formatos siguientes:</span><span class="sxs-lookup"><span data-stu-id="0818d-281">Form or query string data can be in one of the following formats:</span></span>
   
  ```
  selectedCourses=1050&selectedCourses=2000 
  ```

  ```
  selectedCourses[0]=1050&selectedCourses[1]=2000
  ```

  ```
  [0]=1050&[1]=2000
  ```

  ```
  selectedCourses[a]=1050&selectedCourses[b]=2000&selectedCourses.index=a&selectedCourses.index=b
  ```

  ```
  [a]=1050&[b]=2000&index=a&index=b
  ```

* <span data-ttu-id="0818d-282">El formato siguiente solo se admite en datos de formulario:</span><span class="sxs-lookup"><span data-stu-id="0818d-282">The following format is supported only in form data:</span></span>

  ```
  selectedCourses[]=1050&selectedCourses[]=2000
  ```

* <span data-ttu-id="0818d-283">Para todos los formatos de ejemplo anteriores, el enlace de modelos pasa una matriz de dos elementos al parámetro `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="0818d-283">For all of the preceding example formats, model binding passes an array of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="0818d-284">selectedCourses[0]=1050</span><span class="sxs-lookup"><span data-stu-id="0818d-284">selectedCourses[0]=1050</span></span>
  * <span data-ttu-id="0818d-285">selectedCourses[1]=2000</span><span class="sxs-lookup"><span data-stu-id="0818d-285">selectedCourses[1]=2000</span></span>

  <span data-ttu-id="0818d-286">Los formatos de datos que usan números de subíndice (... [0] ... [1] ...) deben asegurarse de que se numeran de forma secuencial a partir de cero.</span><span class="sxs-lookup"><span data-stu-id="0818d-286">Data formats that use subscript numbers (... [0] ... [1] ...) must ensure that they are numbered sequentially starting at zero.</span></span> <span data-ttu-id="0818d-287">Si hay algún hueco en la numeración de los subíndices, se omiten todos los elementos que aparecen después del hueco.</span><span class="sxs-lookup"><span data-stu-id="0818d-287">If there are any gaps in subscript numbering, all items after the gap are ignored.</span></span> <span data-ttu-id="0818d-288">Por ejemplo, si los subíndices son 0 y 2 en lugar de 0 y 1, se omite el segundo elemento.</span><span class="sxs-lookup"><span data-stu-id="0818d-288">For example, if the subscripts are 0 and 2 instead of 0 and 1, the second item is ignored.</span></span>

## <a name="dictionaries"></a><span data-ttu-id="0818d-289">Diccionarios</span><span class="sxs-lookup"><span data-stu-id="0818d-289">Dictionaries</span></span>

<span data-ttu-id="0818d-290">Para los destinos `Dictionary`, el enlace de modelos busca coincidencias con *nombre_de_parámetro* o *nombre_de_propiedad*.</span><span class="sxs-lookup"><span data-stu-id="0818d-290">For `Dictionary` targets, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="0818d-291">Si no se encuentra ninguna coincidencia, busca uno de los formatos admitidos sin el prefijo.</span><span class="sxs-lookup"><span data-stu-id="0818d-291">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="0818d-292">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0818d-292">For example:</span></span>

* <span data-ttu-id="0818d-293">Imagine que el parámetro de destino es un elemento `Dictionary<int, string>` denominado `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="0818d-293">Suppose the target parameter is a `Dictionary<int, string>` named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, Dictionary<int, string> selectedCourses)
  ```

* <span data-ttu-id="0818d-294">Los datos de cadena de consulta o de formulario publicados pueden ser similares a uno de los ejemplos siguientes:</span><span class="sxs-lookup"><span data-stu-id="0818d-294">The posted form or query string data can look like one of the following examples:</span></span>

  ```
  selectedCourses[1050]=Chemistry&selectedCourses[2000]=Economics
  ```

  ```
  [1050]=Chemistry&selectedCourses[2000]=Economics
  ```

  ```
  selectedCourses[0].Key=1050&selectedCourses[0].Value=Chemistry&
  selectedCourses[1].Key=2000&selectedCourses[1].Value=Economics
  ```

  ```
  [0].Key=1050&[0].Value=Chemistry&[1].Key=2000&[1].Value=Economics
  ```

* <span data-ttu-id="0818d-295">Para todos los formatos de ejemplo anteriores, el enlace de modelos pasa un diccionario de dos elementos al parámetro `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="0818d-295">For all of the preceding example formats, model binding passes a dictionary of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="0818d-296">selectedCourses["1050"]="Chemistry"</span><span class="sxs-lookup"><span data-stu-id="0818d-296">selectedCourses["1050"]="Chemistry"</span></span>
  * <span data-ttu-id="0818d-297">selectedCourses["2000"]="Economics"</span><span class="sxs-lookup"><span data-stu-id="0818d-297">selectedCourses["2000"]="Economics"</span></span>

## <a name="special-data-types"></a><span data-ttu-id="0818d-298">Tipos de datos especiales</span><span class="sxs-lookup"><span data-stu-id="0818d-298">Special data types</span></span>

<span data-ttu-id="0818d-299">Hay algunos tipos de datos especiales que el enlace de modelos puede controlar.</span><span class="sxs-lookup"><span data-stu-id="0818d-299">There are some special data types that model binding can handle.</span></span>

### <a name="iformfile-and-iformfilecollection"></a><span data-ttu-id="0818d-300">IFormFile e IFormFileCollection</span><span class="sxs-lookup"><span data-stu-id="0818d-300">IFormFile and IFormFileCollection</span></span>

<span data-ttu-id="0818d-301">Un archivo cargado incluido en la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="0818d-301">An uploaded file included in the HTTP request.</span></span>  <span data-ttu-id="0818d-302">También se admite `IEnumerable<IFormFile>` para varios archivos.</span><span class="sxs-lookup"><span data-stu-id="0818d-302">Also supported is `IEnumerable<IFormFile>` for multiple files.</span></span>

### <a name="cancellationtoken"></a><span data-ttu-id="0818d-303">CancellationToken</span><span class="sxs-lookup"><span data-stu-id="0818d-303">CancellationToken</span></span>

<span data-ttu-id="0818d-304">se usa para cancelar la actividad en controladores asincrónicos.</span><span class="sxs-lookup"><span data-stu-id="0818d-304">Used to cancel activity in asynchronous controllers.</span></span>

### <a name="formcollection"></a><span data-ttu-id="0818d-305">FormCollection</span><span class="sxs-lookup"><span data-stu-id="0818d-305">FormCollection</span></span>

<span data-ttu-id="0818d-306">Se usa para recuperar todos los valores de los datos de formulario publicados.</span><span class="sxs-lookup"><span data-stu-id="0818d-306">Used to retrieve all the values from posted form data.</span></span>

## <a name="input-formatters"></a><span data-ttu-id="0818d-307">Formateadores de entrada</span><span class="sxs-lookup"><span data-stu-id="0818d-307">Input formatters</span></span>

<span data-ttu-id="0818d-308">Los datos del cuerpo de la solicitud pueden estar en XML, JSON u otro formato.</span><span class="sxs-lookup"><span data-stu-id="0818d-308">Data in the request body can be in JSON, XML, or some other format.</span></span> <span data-ttu-id="0818d-309">Para analizar estos datos, el enlace de modelos usa un *formateador de entrada* configurado para controlar un tipo de contenido determinado.</span><span class="sxs-lookup"><span data-stu-id="0818d-309">To parse this data, model binding uses an *input formatter* that is configured to handle a particular content type.</span></span> <span data-ttu-id="0818d-310">De forma predeterminada, en ASP.NET Core se incluyen formateadores de entrada basados en JSON para el control de los datos JSON.</span><span class="sxs-lookup"><span data-stu-id="0818d-310">By default, ASP.NET Core includes JSON based input formatters for handling JSON data.</span></span> <span data-ttu-id="0818d-311">Puede agregar otros formateadores para otros tipos de contenido.</span><span class="sxs-lookup"><span data-stu-id="0818d-311">You can add other formatters for other content types.</span></span>

<span data-ttu-id="0818d-312">ASP.NET Core selecciona los formateadores de entrada en función del atributo [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute).</span><span class="sxs-lookup"><span data-stu-id="0818d-312">ASP.NET Core selects input formatters based on the [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) attribute.</span></span> <span data-ttu-id="0818d-313">Si no hay ningún atributo, usa el [encabezado Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span><span class="sxs-lookup"><span data-stu-id="0818d-313">If no attribute is present, it uses the [Content-Type header](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span></span>

<span data-ttu-id="0818d-314">Para usar los formateadores de entrada XML integrados:</span><span class="sxs-lookup"><span data-stu-id="0818d-314">To use the built-in XML input formatters:</span></span>

* <span data-ttu-id="0818d-315">Instale el paquete NuGet `Microsoft.AspNetCore.Mvc.Formatters.Xml`.</span><span class="sxs-lookup"><span data-stu-id="0818d-315">Install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

* <span data-ttu-id="0818d-316">En `Startup.ConfigureServices`, llame a <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> o a <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span><span class="sxs-lookup"><span data-stu-id="0818d-316">In `Startup.ConfigureServices`, call <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> or <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span></span>

  [!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=9)]

* <span data-ttu-id="0818d-317">Aplique el atributo `Consumes` a las clases de controlador o los métodos de acción que deben esperar XML en el cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="0818d-317">Apply the `Consumes` attribute to controller classes or action methods that should expect XML in the request body.</span></span>

  ```csharp
  [HttpPost]
  [Consumes("application/xml")]
  public ActionResult<Pet> Create(Pet pet)
  ```

  <span data-ttu-id="0818d-318">Para más información, vea [Introducción de la serialización XML](/dotnet/standard/serialization/introducing-xml-serialization).</span><span class="sxs-lookup"><span data-stu-id="0818d-318">For more information, see [Introducing XML Serialization](/dotnet/standard/serialization/introducing-xml-serialization).</span></span>

## <a name="exclude-specified-types-from-model-binding"></a><span data-ttu-id="0818d-319">Exclusión de tipos especificados del enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="0818d-319">Exclude specified types from model binding</span></span>

<span data-ttu-id="0818d-320">El comportamiento del enlace de modelos y los sistemas de validación se controla mediante [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span><span class="sxs-lookup"><span data-stu-id="0818d-320">The model binding and validation systems' behavior is driven by [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span></span> <span data-ttu-id="0818d-321">Puede personalizar `ModelMetadata` mediante la adición de un proveedor de detalles a [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span><span class="sxs-lookup"><span data-stu-id="0818d-321">You can customize `ModelMetadata` by adding a details provider to [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span></span> <span data-ttu-id="0818d-322">Los proveedores de detalles integrados están disponibles para deshabilitar el enlace de modelos o la validación para tipos especificados.</span><span class="sxs-lookup"><span data-stu-id="0818d-322">Built-in details providers are available for disabling model binding or validation for specified types.</span></span>

<span data-ttu-id="0818d-323">Para deshabilitar el enlace de modelos en todos los modelos de un tipo especificado, agregue una instancia de <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0818d-323">To disable model binding on all models of a specified type, add an <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="0818d-324">Por ejemplo, para deshabilitar el enlace de modelos en todos los modelos del tipo `System.Version`:</span><span class="sxs-lookup"><span data-stu-id="0818d-324">For example, to disable model binding on all models of type `System.Version`:</span></span>

[!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=4-5)]

<span data-ttu-id="0818d-325">Para deshabilitar la validación en propiedades de un tipo especificado, agregue una instancia de <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0818d-325">To disable validation on properties of a specified type, add a <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="0818d-326">Por ejemplo, para deshabilitar la validación en las propiedades de tipo `System.Guid`:</span><span class="sxs-lookup"><span data-stu-id="0818d-326">For example, to disable validation on properties of type `System.Guid`:</span></span>

[!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=6-7)]

## <a name="custom-model-binders"></a><span data-ttu-id="0818d-327">Enlazadores de modelos personalizados</span><span class="sxs-lookup"><span data-stu-id="0818d-327">Custom model binders</span></span>

<span data-ttu-id="0818d-328">Puede ampliar el enlace de modelos si escribe un enlazador de modelos personalizado y usa el atributo `[ModelBinder]` para seleccionarlo para un destino concreto.</span><span class="sxs-lookup"><span data-stu-id="0818d-328">You can extend model binding by writing a custom model binder and using the `[ModelBinder]` attribute to select it for a given target.</span></span> <span data-ttu-id="0818d-329">Más información sobre el [enlace de modelos personalizados](xref:mvc/advanced/custom-model-binding).</span><span class="sxs-lookup"><span data-stu-id="0818d-329">Learn more about [custom model binding](xref:mvc/advanced/custom-model-binding).</span></span>

## <a name="manual-model-binding"></a><span data-ttu-id="0818d-330">Enlace de modelos manual</span><span class="sxs-lookup"><span data-stu-id="0818d-330">Manual model binding</span></span>

<span data-ttu-id="0818d-331">El enlace de modelos se puede invocar de forma manual mediante el método <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>.</span><span class="sxs-lookup"><span data-stu-id="0818d-331">Model binding can be invoked manually by using the <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> method.</span></span> <span data-ttu-id="0818d-332">El método se define en las clases `ControllerBase` y `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="0818d-332">The method is defined on both `ControllerBase` and `PageModel` classes.</span></span> <span data-ttu-id="0818d-333">Las sobrecargas de método permiten especificar el prefijo y el proveedor de valores que se van a usar.</span><span class="sxs-lookup"><span data-stu-id="0818d-333">Method overloads let you specify the prefix and value provider to use.</span></span> <span data-ttu-id="0818d-334">El método devuelve `false` si se produce un error en el enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="0818d-334">The method returns `false` if model binding fails.</span></span> <span data-ttu-id="0818d-335">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0818d-335">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/InstructorsWithCollection/Create.cshtml.cs?name=snippet_TryUpdate&highlight=1-4)]

## <a name="fromservices-attribute"></a><span data-ttu-id="0818d-336">Atributo [FromServices]</span><span class="sxs-lookup"><span data-stu-id="0818d-336">[FromServices] attribute</span></span>

<span data-ttu-id="0818d-337">El nombre de este atributo sigue el patrón de los atributos de enlace de modelos que especifican un origen de datos.</span><span class="sxs-lookup"><span data-stu-id="0818d-337">This attribute's name follows the pattern of model binding attributes that specify a data source.</span></span> <span data-ttu-id="0818d-338">Pero no se trata de enlazar datos desde un proveedor de valores.</span><span class="sxs-lookup"><span data-stu-id="0818d-338">But it's not about binding data from a value provider.</span></span> <span data-ttu-id="0818d-339">Obtiene una instancia de un tipo desde el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="0818d-339">It gets an instance of a type from the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="0818d-340">Su objetivo es proporcionar una alternativa a la inserción de constructores cuando se necesita un servicio solo si se llama a un método concreto.</span><span class="sxs-lookup"><span data-stu-id="0818d-340">Its purpose is to provide an alternative to constructor injection for when you need a service only if a particular method is called.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0818d-341">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="0818d-341">Additional resources</span></span>

* <xref:mvc/models/validation>
* <xref:mvc/advanced/custom-model-binding>
