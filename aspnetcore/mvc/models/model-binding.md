---
title: Enlace de modelos en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo funciona el enlace de modelos en ASP.NET Core y cómo personalizar su comportamiento.
ms.assetid: 0be164aa-1d72-4192-bd6b-192c9c301164
ms.author: riande
ms.date: 11/21/2019
uid: mvc/models/model-binding
ms.openlocfilehash: da6cc25e0bbb1b2301529b34eab4c91f9ccb46eb
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/09/2019
ms.locfileid: "74944300"
---
# <a name="model-binding-in-aspnet-core"></a><span data-ttu-id="b6f60-103">Enlace de modelos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b6f60-103">Model Binding in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b6f60-104">En este artículo se explica qué es el enlace de modelos, cómo funciona y cómo personalizar su comportamiento.</span><span class="sxs-lookup"><span data-stu-id="b6f60-104">This article explains what model binding is, how it works, and how to customize its behavior.</span></span>

<span data-ttu-id="b6f60-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="b6f60-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="what-is-model-binding"></a><span data-ttu-id="b6f60-106">Qué es el enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="b6f60-106">What is Model binding</span></span>

<span data-ttu-id="b6f60-107">Los controladores y Razor Pages trabajan con datos procedentes de solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="b6f60-107">Controllers and Razor pages work with data that comes from HTTP requests.</span></span> <span data-ttu-id="b6f60-108">Por ejemplo, los datos de ruta pueden proporcionar una clave de registro y los campos de formulario publicados pueden proporcionar valores para las propiedades del modelo.</span><span class="sxs-lookup"><span data-stu-id="b6f60-108">For example, route data may provide a record key, and posted form fields may provide values for the properties of the model.</span></span> <span data-ttu-id="b6f60-109">La escritura de código para recuperar cada uno de estos valores y convertirlos de cadenas a tipos de .NET sería tediosa y propensa a errores.</span><span class="sxs-lookup"><span data-stu-id="b6f60-109">Writing code to retrieve each of these values and convert them from strings to .NET types would be tedious and error-prone.</span></span> <span data-ttu-id="b6f60-110">El enlace de modelos automatiza este proceso.</span><span class="sxs-lookup"><span data-stu-id="b6f60-110">Model binding automates this process.</span></span> <span data-ttu-id="b6f60-111">El sistema de enlace de modelos:</span><span class="sxs-lookup"><span data-stu-id="b6f60-111">The model binding system:</span></span>

* <span data-ttu-id="b6f60-112">Recupera datos de diversos orígenes, como datos de ruta, campos de formulario y cadenas de consulta.</span><span class="sxs-lookup"><span data-stu-id="b6f60-112">Retrieves data from various sources such as route data, form fields, and query strings.</span></span>
* <span data-ttu-id="b6f60-113">Proporciona los datos a los controladores y Razor Pages en parámetros de método y propiedades públicas.</span><span class="sxs-lookup"><span data-stu-id="b6f60-113">Provides the data to controllers and Razor pages in method parameters and public properties.</span></span>
* <span data-ttu-id="b6f60-114">Convierte datos de cadena en tipos de .NET.</span><span class="sxs-lookup"><span data-stu-id="b6f60-114">Converts string data to .NET types.</span></span>
* <span data-ttu-id="b6f60-115">Actualiza las propiedades de tipos complejos.</span><span class="sxs-lookup"><span data-stu-id="b6f60-115">Updates properties of complex types.</span></span>

## <a name="example"></a><span data-ttu-id="b6f60-116">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="b6f60-116">Example</span></span>

<span data-ttu-id="b6f60-117">Imagine que tiene el siguiente método de acción:</span><span class="sxs-lookup"><span data-stu-id="b6f60-117">Suppose you have the following action method:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Controllers/PetsController.cs?name=snippet_DogsOnly)]

<span data-ttu-id="b6f60-118">Y que la aplicación recibe una solicitud con esta dirección URL:</span><span class="sxs-lookup"><span data-stu-id="b6f60-118">And the app receives a request with this URL:</span></span>

```
http://contoso.com/api/pets/2?DogsOnly=true
```

<span data-ttu-id="b6f60-119">El enlace de modelos realiza los pasos siguientes después de que el sistema de enrutamiento selecciona el método de acción:</span><span class="sxs-lookup"><span data-stu-id="b6f60-119">Model binding goes through the following steps after the routing system selects the action method:</span></span>

* <span data-ttu-id="b6f60-120">Busca el primer parámetro de `GetByID`, un entero denominado `id`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-120">Finds the first parameter of `GetByID`, an integer named `id`.</span></span>
* <span data-ttu-id="b6f60-121">Examina los orígenes disponibles en la solicitud HTTP y busca `id` = "2" en los datos de ruta.</span><span class="sxs-lookup"><span data-stu-id="b6f60-121">Looks through the available sources in the HTTP request and finds `id` = "2" in route data.</span></span>
* <span data-ttu-id="b6f60-122">Convierte la cadena "2" en el entero 2.</span><span class="sxs-lookup"><span data-stu-id="b6f60-122">Converts the string "2" into integer 2.</span></span>
* <span data-ttu-id="b6f60-123">Busca el siguiente parámetro de `GetByID`, un valor booleano denominado `dogsOnly`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-123">Finds the next parameter of `GetByID`, a boolean named `dogsOnly`.</span></span>
* <span data-ttu-id="b6f60-124">Examina los orígenes y busca "DogsOnly=true" en la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="b6f60-124">Looks through the sources and finds "DogsOnly=true" in the query string.</span></span> <span data-ttu-id="b6f60-125">La coincidencia de nombres no distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="b6f60-125">Name matching is not case-sensitive.</span></span>
* <span data-ttu-id="b6f60-126">Convierte la cadena "true" en un valor booleano `true`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-126">Converts the string "true" into boolean `true`.</span></span>

<span data-ttu-id="b6f60-127">Después, el marco llama al método `GetById` y pasa 2 para el parámetro `id` y `true` para el parámetro `dogsOnly`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-127">The framework then calls the `GetById` method, passing in 2 for the `id` parameter, and `true` for the `dogsOnly` parameter.</span></span>

<span data-ttu-id="b6f60-128">En el ejemplo anterior, los destinos de enlace de modelos son parámetros de método que son tipos simples.</span><span class="sxs-lookup"><span data-stu-id="b6f60-128">In the preceding example, the model binding targets are method parameters that are simple types.</span></span> <span data-ttu-id="b6f60-129">Los destinos también pueden ser las propiedades de un tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="b6f60-129">Targets may also be the properties of a complex type.</span></span> <span data-ttu-id="b6f60-130">Después de que cada propiedad se haya enlazado correctamente, se produce la [validación de modelos](xref:mvc/models/validation) para esa propiedad.</span><span class="sxs-lookup"><span data-stu-id="b6f60-130">After each property is successfully bound, [model validation](xref:mvc/models/validation) occurs for that property.</span></span> <span data-ttu-id="b6f60-131">El registro de qué datos se han enlazado al modelo y los errores de enlace o validación se almacenan en [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) o [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span><span class="sxs-lookup"><span data-stu-id="b6f60-131">The record of what data is bound to the model, and any binding or validation errors, is stored in [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) or [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span></span> <span data-ttu-id="b6f60-132">Para averiguar si este proceso se ha realizado correctamente, la aplicación comprueba la marca [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid).</span><span class="sxs-lookup"><span data-stu-id="b6f60-132">To find out if this process was successful, the app checks the [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) flag.</span></span>

## <a name="targets"></a><span data-ttu-id="b6f60-133">Destinos</span><span class="sxs-lookup"><span data-stu-id="b6f60-133">Targets</span></span>

<span data-ttu-id="b6f60-134">El enlace de modelos intenta encontrar valores para los tipos de destinos siguientes:</span><span class="sxs-lookup"><span data-stu-id="b6f60-134">Model binding tries to find values for the following kinds of targets:</span></span>

* <span data-ttu-id="b6f60-135">Parámetros del método de acción de controlador al que se enruta una solicitud.</span><span class="sxs-lookup"><span data-stu-id="b6f60-135">Parameters of the controller action method that a request is routed to.</span></span>
* <span data-ttu-id="b6f60-136">Parámetros del método de controlador de Razor Pages al que se enruta una solicitud.</span><span class="sxs-lookup"><span data-stu-id="b6f60-136">Parameters of the Razor Pages handler method that a request is routed to.</span></span> 
* <span data-ttu-id="b6f60-137">Propiedades públicas de un controlador o una clase `PageModel`, si se especifican mediante atributos.</span><span class="sxs-lookup"><span data-stu-id="b6f60-137">Public properties of a controller or `PageModel` class, if specified by attributes.</span></span>

### <a name="bindproperty-attribute"></a><span data-ttu-id="b6f60-138">Atributo [BindProperty]</span><span class="sxs-lookup"><span data-stu-id="b6f60-138">[BindProperty] attribute</span></span>

<span data-ttu-id="b6f60-139">Se puede aplicar a una propiedad pública de un controlador o una clase `PageModel` para hacer que el enlace de modelos tenga esa propiedad como destino:</span><span class="sxs-lookup"><span data-stu-id="b6f60-139">Can be applied to a public property of a controller or `PageModel` class to cause model binding to target that property:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Edit.cshtml.cs?name=snippet_BindProperty&highlight=3-4)]

### <a name="bindpropertiesattribute"></a><span data-ttu-id="b6f60-140">Atributo [BindProperties]</span><span class="sxs-lookup"><span data-stu-id="b6f60-140">[BindProperties] attribute</span></span>

<span data-ttu-id="b6f60-141">Disponible en ASP.NET 2.1 Core y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="b6f60-141">Available in ASP.NET Core 2.1 and later.</span></span>  <span data-ttu-id="b6f60-142">Se pueden aplicar a un controlador o una clase `PageModel` para indicar al enlace de modelos que seleccione como destino todas las propiedades públicas de la clase:</span><span class="sxs-lookup"><span data-stu-id="b6f60-142">Can be applied to a controller or `PageModel` class to tell model binding to target all public properties of the class:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_BindProperties&highlight=1-2)]

### <a name="model-binding-for-http-get-requests"></a><span data-ttu-id="b6f60-143">Enlace de modelos para solicitudes HTTP GET</span><span class="sxs-lookup"><span data-stu-id="b6f60-143">Model binding for HTTP GET requests</span></span>

<span data-ttu-id="b6f60-144">De forma predeterminada, las propiedades no se enlazan para las solicitudes HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="b6f60-144">By default, properties are not bound for HTTP GET requests.</span></span> <span data-ttu-id="b6f60-145">Normalmente, todo lo que necesita para una solicitud GET es un parámetro de id. de registro.</span><span class="sxs-lookup"><span data-stu-id="b6f60-145">Typically, all you need for a GET request is a record ID parameter.</span></span> <span data-ttu-id="b6f60-146">El id. de registro se usa para buscar el elemento en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b6f60-146">The record ID is used to look up the item in the database.</span></span> <span data-ttu-id="b6f60-147">Por tanto, no es necesario enlazar una propiedad que contiene una instancia del modelo.</span><span class="sxs-lookup"><span data-stu-id="b6f60-147">Therefore, there is no need to bind a property that holds an instance of the model.</span></span> <span data-ttu-id="b6f60-148">En escenarios donde quiera propiedades enlazadas a datos de las solicitudes GET, establezca la propiedad `SupportsGet` en `true`:</span><span class="sxs-lookup"><span data-stu-id="b6f60-148">In scenarios where you do want properties bound to data from GET requests, set the `SupportsGet` property to `true`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_SupportsGet)]

## <a name="sources"></a><span data-ttu-id="b6f60-149">Orígenes</span><span class="sxs-lookup"><span data-stu-id="b6f60-149">Sources</span></span>

<span data-ttu-id="b6f60-150">De forma predeterminada, el enlace de modelos obtiene datos en forma de pares clave-valor de los siguientes orígenes de una solicitud HTTP:</span><span class="sxs-lookup"><span data-stu-id="b6f60-150">By default, model binding gets data in the form of key-value pairs from the following sources in an HTTP request:</span></span>

1. <span data-ttu-id="b6f60-151">Campos de formulario</span><span class="sxs-lookup"><span data-stu-id="b6f60-151">Form fields</span></span>
1. <span data-ttu-id="b6f60-152">El cuerpo de la solicitud (para [controladores que tienen el atributo [ApiController]](xref:web-api/index#binding-source-parameter-inference)).</span><span class="sxs-lookup"><span data-stu-id="b6f60-152">The request body (For [controllers that have the [ApiController] attribute](xref:web-api/index#binding-source-parameter-inference).)</span></span>
1. <span data-ttu-id="b6f60-153">Datos de ruta</span><span class="sxs-lookup"><span data-stu-id="b6f60-153">Route data</span></span>
1. <span data-ttu-id="b6f60-154">Parámetros de cadena de consulta</span><span class="sxs-lookup"><span data-stu-id="b6f60-154">Query string parameters</span></span>
1. <span data-ttu-id="b6f60-155">Archivos cargados</span><span class="sxs-lookup"><span data-stu-id="b6f60-155">Uploaded files</span></span>

<span data-ttu-id="b6f60-156">Para cada parámetro o propiedad de destino, se examinan los orígenes en el orden indicado en la lista anterior.</span><span class="sxs-lookup"><span data-stu-id="b6f60-156">For each target parameter or property, the sources are scanned in the order indicated in the preceding list.</span></span> <span data-ttu-id="b6f60-157">Hay algunas excepciones:</span><span class="sxs-lookup"><span data-stu-id="b6f60-157">There are a few exceptions:</span></span>

* <span data-ttu-id="b6f60-158">Los datos de ruta y los valores de cadena de consulta solo se usan para tipos simples.</span><span class="sxs-lookup"><span data-stu-id="b6f60-158">Route data and query string values are used only for simple types.</span></span>
* <span data-ttu-id="b6f60-159">Los archivos cargados solo se enlazan a tipos de destino que implementan `IFormFile` o `IEnumerable<IFormFile>`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-159">Uploaded files are bound only to target types that implement `IFormFile` or `IEnumerable<IFormFile>`.</span></span>

<span data-ttu-id="b6f60-160">Si el origen predeterminado no es correcto, use uno de los atributos siguientes para especificar el origen:</span><span class="sxs-lookup"><span data-stu-id="b6f60-160">If the default source is not correct, use one of the following attributes to specify the source:</span></span>

* <span data-ttu-id="b6f60-161">[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute): obtiene valores de la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="b6f60-161">[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) - Gets values from the query string.</span></span> 
* <span data-ttu-id="b6f60-162">[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute): obtiene valores de los datos de ruta.</span><span class="sxs-lookup"><span data-stu-id="b6f60-162">[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) - Gets values from route data.</span></span>
* <span data-ttu-id="b6f60-163">[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute): obtiene valores de los campos de formulario publicados.</span><span class="sxs-lookup"><span data-stu-id="b6f60-163">[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) - Gets values from posted form fields.</span></span>
* <span data-ttu-id="b6f60-164">[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute): obtiene valores del cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="b6f60-164">[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) - Gets values from the request body.</span></span>
* <span data-ttu-id="b6f60-165">[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute): obtiene valores de encabezados HTTP.</span><span class="sxs-lookup"><span data-stu-id="b6f60-165">[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) - Gets values from HTTP headers.</span></span>

<span data-ttu-id="b6f60-166">Estos atributos:</span><span class="sxs-lookup"><span data-stu-id="b6f60-166">These attributes:</span></span>

* <span data-ttu-id="b6f60-167">Se agregan de forma individual a las propiedades del modelo (no a la clase de modelo), como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="b6f60-167">Are added to model properties individually (not to the model class), as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/Instructor.cs?name=snippet_FromQuery&highlight=5-6)]

* <span data-ttu-id="b6f60-168">Opcionalmente, acepte un valor de nombre de modelo en el constructor.</span><span class="sxs-lookup"><span data-stu-id="b6f60-168">Optionally accept a model name value in the constructor.</span></span> <span data-ttu-id="b6f60-169">Esta opción se proporciona en caso de que el nombre de la propiedad no coincida con el valor de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="b6f60-169">This option is provided in case the property name doesn't match the value in the request.</span></span> <span data-ttu-id="b6f60-170">Por ejemplo, es posible que el valor de la solicitud sea un encabezado con un guion en el nombre, como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="b6f60-170">For instance, the value in the request might be a header with a hyphen in its name, as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_FromHeader)]

### <a name="frombody-attribute"></a><span data-ttu-id="b6f60-171">Atributo [FromBody]</span><span class="sxs-lookup"><span data-stu-id="b6f60-171">[FromBody] attribute</span></span>

<span data-ttu-id="b6f60-172">Aplique el atributo `[FromBody]` a un parámetro para rellenar sus propiedades desde el cuerpo de una solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="b6f60-172">Apply the `[FromBody]` attribute to a parameter to populate its properties from the body of an HTTP request.</span></span> <span data-ttu-id="b6f60-173">El runtime de ASP.NET Core delega la responsabilidad de leer el cuerpo al formateador de entrada.</span><span class="sxs-lookup"><span data-stu-id="b6f60-173">The ASP.NET Core runtime delegates the responsibility of reading the body to an input formatter.</span></span> <span data-ttu-id="b6f60-174">Los formateadores de entrada se explican [más adelante en este artículo](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="b6f60-174">Input formatters are explained [later in this article](#input-formatters).</span></span>

<span data-ttu-id="b6f60-175">Cuando se aplica `[FromBody]` a un parámetro de tipo complejo, se omiten los atributos de origen de enlace aplicados a sus propiedades.</span><span class="sxs-lookup"><span data-stu-id="b6f60-175">When `[FromBody]` is applied to a complex type parameter, any binding source attributes applied to its properties are ignored.</span></span> <span data-ttu-id="b6f60-176">Por ejemplo, la acción `Create` siguiente especifica que su parámetro `pet` se rellena a partir del cuerpo:</span><span class="sxs-lookup"><span data-stu-id="b6f60-176">For example, the following `Create` action specifies that its `pet` parameter is populated from the body:</span></span>

```csharp
public ActionResult<Pet> Create([FromBody] Pet pet)
```

<span data-ttu-id="b6f60-177">La clase `Pet` especifica que su propiedad `Breed` se rellena a partir de un parámetro de cadena de consulta:</span><span class="sxs-lookup"><span data-stu-id="b6f60-177">The `Pet` class specifies that its `Breed` property is populated from a query string parameter:</span></span>

```csharp
public class Pet
{
    public string Name { get; set; }

    [FromQuery] // Attribute is ignored.
    public string Breed { get; set; }
}
```

<span data-ttu-id="b6f60-178">En el ejemplo anterior:</span><span class="sxs-lookup"><span data-stu-id="b6f60-178">In the preceding example:</span></span>

* <span data-ttu-id="b6f60-179">El atributo `[FromQuery]` se ignora.</span><span class="sxs-lookup"><span data-stu-id="b6f60-179">The `[FromQuery]` attribute is ignored.</span></span>
* <span data-ttu-id="b6f60-180">La propiedad `Breed` no se rellena desde un parámetro de cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="b6f60-180">The `Breed` property is not populated from a query string parameter.</span></span> 

<span data-ttu-id="b6f60-181">Los formateadores de entrada solo leen el cuerpo y no entienden los atributos de origen de enlace.</span><span class="sxs-lookup"><span data-stu-id="b6f60-181">Input formatters read only the body and don't understand binding source attributes.</span></span> <span data-ttu-id="b6f60-182">Si se encuentra un valor adecuado en el cuerpo, ese valor se usa para rellenar la propiedad `Breed`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-182">If a suitable value is found in the body, that value is used to populate the `Breed` property.</span></span>

<span data-ttu-id="b6f60-183">No aplique `[FromBody]` a más de un parámetro por método de acción.</span><span class="sxs-lookup"><span data-stu-id="b6f60-183">Don't apply `[FromBody]` to more than one parameter per action method.</span></span> <span data-ttu-id="b6f60-184">Una vez que un formateador de entrada ha leído la secuencia de solicitudes, deja de estar disponible para una nueva lectura con el fin de enlazar otros parámetros `[FromBody]`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-184">Once the request stream is read by an input formatter, it's no longer available to be read again for binding other `[FromBody]` parameters.</span></span>

### <a name="additional-sources"></a><span data-ttu-id="b6f60-185">Orígenes adicionales</span><span class="sxs-lookup"><span data-stu-id="b6f60-185">Additional sources</span></span>

<span data-ttu-id="b6f60-186">Los *proveedores de valores* proporcionan datos de origen al sistema de enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="b6f60-186">Source data is provided to the model binding system by *value providers*.</span></span> <span data-ttu-id="b6f60-187">Puede escribir y registrar proveedores de valores personalizados que obtienen datos de otros orígenes para el enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="b6f60-187">You can write and register custom value providers that get data for model binding from other sources.</span></span> <span data-ttu-id="b6f60-188">Por ejemplo, es posible que le interesen datos de cookies o del estado de sesión.</span><span class="sxs-lookup"><span data-stu-id="b6f60-188">For example, you might want data from cookies or session state.</span></span> <span data-ttu-id="b6f60-189">Para obtener datos desde un origen nuevo:</span><span class="sxs-lookup"><span data-stu-id="b6f60-189">To get data from a new source:</span></span>

* <span data-ttu-id="b6f60-190">Cree una clase que implemente `IValueProvider`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-190">Create a class that implements `IValueProvider`.</span></span>
* <span data-ttu-id="b6f60-191">Cree una clase que implemente `IValueProviderFactory`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-191">Create a class that implements `IValueProviderFactory`.</span></span>
* <span data-ttu-id="b6f60-192">Registre la clase de generador en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-192">Register the factory class in `Startup.ConfigureServices`.</span></span>

<span data-ttu-id="b6f60-193">En la aplicación de ejemplo se incluye un [proveedor de valores](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProvider.cs) y un [generador](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProviderFactory.cs) que obtiene valores de cookies.</span><span class="sxs-lookup"><span data-stu-id="b6f60-193">The sample app includes a [value provider](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProvider.cs) and [factory](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProviderFactory.cs) example that gets values from cookies.</span></span> <span data-ttu-id="b6f60-194">Este es el código de registro de `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b6f60-194">Here's the registration code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=3)]

<span data-ttu-id="b6f60-195">En el código mostrado, el proveedor de valor personalizado se coloca después de todos los proveedores de valor integrados.</span><span class="sxs-lookup"><span data-stu-id="b6f60-195">The code shown puts the custom value provider after all the built-in value providers.</span></span>  <span data-ttu-id="b6f60-196">Para que sea el primero en la lista, llame a `Insert(0, new CookieValueProviderFactory())` en lugar de a `Add`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-196">To make it the first in the list, call `Insert(0, new CookieValueProviderFactory())` instead of `Add`.</span></span>

## <a name="no-source-for-a-model-property"></a><span data-ttu-id="b6f60-197">No hay origen para una propiedad de modelo</span><span class="sxs-lookup"><span data-stu-id="b6f60-197">No source for a model property</span></span>

<span data-ttu-id="b6f60-198">De forma predeterminada, si no se encuentra ningún valor para una propiedad de modelo no se crea un error de estado del modelo.</span><span class="sxs-lookup"><span data-stu-id="b6f60-198">By default, a model state error isn't created if no value is found for a model property.</span></span> <span data-ttu-id="b6f60-199">La propiedad se establece en NULL o en un valor predeterminado:</span><span class="sxs-lookup"><span data-stu-id="b6f60-199">The property is set to null or a default value:</span></span>

* <span data-ttu-id="b6f60-200">Los tipos simples que aceptan valores NULL se establecen en `null`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-200">Nullable simple types are set to `null`.</span></span>
* <span data-ttu-id="b6f60-201">Los tipos de valor que no aceptan valores NULL se establecen en `default(T)`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-201">Non-nullable value types are set to `default(T)`.</span></span> <span data-ttu-id="b6f60-202">Por ejemplo, un parámetro `int id` se establece en 0.</span><span class="sxs-lookup"><span data-stu-id="b6f60-202">For example, a parameter `int id` is set to 0.</span></span>
* <span data-ttu-id="b6f60-203">Para los tipos complejos, el enlace de modelos crea una instancia mediante el constructor predeterminado, sin establecer propiedades.</span><span class="sxs-lookup"><span data-stu-id="b6f60-203">For complex Types, model binding creates an instance by using the default constructor, without setting properties.</span></span>
* <span data-ttu-id="b6f60-204">Las matrices se establecen en `Array.Empty<T>()`, salvo las matrices `byte[]`, que se establecen en `null`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-204">Arrays are set to `Array.Empty<T>()`, except that `byte[]` arrays are set to `null`.</span></span>

<span data-ttu-id="b6f60-205">Si el estado del modelo se debe invalidar cuando no se encuentra nada en los campos de formulario para una propiedad de modelo, use el atributo [`[BindRequired]`](#bindrequired-attribute).</span><span class="sxs-lookup"><span data-stu-id="b6f60-205">If model state should be invalidated when nothing is found in form fields for a model property, use the [`[BindRequired]`](#bindrequired-attribute) attribute.</span></span>

<span data-ttu-id="b6f60-206">Tenga en cuenta que este comportamiento de `[BindRequired]` se aplica al enlace de modelos desde datos de formulario publicados, no a los datos JSON o XML del cuerpo de una solicitud.</span><span class="sxs-lookup"><span data-stu-id="b6f60-206">Note that this `[BindRequired]` behavior applies to model binding from posted form data, not to JSON or XML data in a request body.</span></span> <span data-ttu-id="b6f60-207">Los datos del cuerpo de la solicitud se controlan mediante [formateadores de entrada](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="b6f60-207">Request body data is handled by [input formatters](#input-formatters).</span></span>

## <a name="type-conversion-errors"></a><span data-ttu-id="b6f60-208">Errores de la conversión de tipos</span><span class="sxs-lookup"><span data-stu-id="b6f60-208">Type conversion errors</span></span>

<span data-ttu-id="b6f60-209">Si se encuentra un origen pero no se puede convertir al tipo de destino, el estado del modelo se marca como no válido.</span><span class="sxs-lookup"><span data-stu-id="b6f60-209">If a source is found but can't be converted into the target type, model state is flagged as invalid.</span></span> <span data-ttu-id="b6f60-210">El parámetro o la propiedad de destino se establece en NULL o en un valor predeterminado, como se ha indicado en la sección anterior.</span><span class="sxs-lookup"><span data-stu-id="b6f60-210">The target parameter or property is set to null or a default value, as noted in the previous section.</span></span>

<span data-ttu-id="b6f60-211">En un controlador de API que tenga el atributo `[ApiController]`, el estado de modelo no válido genera una respuesta HTTP 400 automática.</span><span class="sxs-lookup"><span data-stu-id="b6f60-211">In an API controller that has the `[ApiController]` attribute, invalid model state results in an automatic HTTP 400 response.</span></span>

<span data-ttu-id="b6f60-212">En una página de Razor, se vuelve a mostrar la página con un mensaje de error:</span><span class="sxs-lookup"><span data-stu-id="b6f60-212">In a Razor page, redisplay the page with an error message:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_HandleMBError&highlight=3-6)]

<span data-ttu-id="b6f60-213">La validación del lado cliente detecta la mayoría de los datos incorrectos que en otros casos se enviarían a un formulario de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="b6f60-213">Client-side validation catches most bad data that would otherwise be submitted to a Razor Pages form.</span></span> <span data-ttu-id="b6f60-214">Esta validación dificulta que se pueda desencadenar el código resaltado anterior.</span><span class="sxs-lookup"><span data-stu-id="b6f60-214">This validation makes it hard to trigger the preceding highlighted code.</span></span> <span data-ttu-id="b6f60-215">En la aplicación de ejemplo se incluye un botón **Submit with Invalid Date** (Enviar con fecha no válida) que agrega datos incorrectos al campo **Hire Date** (Fecha de contratación) y envía el formulario.</span><span class="sxs-lookup"><span data-stu-id="b6f60-215">The sample app includes a **Submit with Invalid Date** button that puts bad data in the **Hire Date** field and submits the form.</span></span> <span data-ttu-id="b6f60-216">Este botón muestra cómo funciona el código para volver a mostrar la página cuando se producen errores de conversión de datos.</span><span class="sxs-lookup"><span data-stu-id="b6f60-216">This button shows how the code for redisplaying the page works when data conversion errors occur.</span></span>

<span data-ttu-id="b6f60-217">Cuando el código anterior vuelve a mostrar la página, no se muestra la entrada no válida en el campo de formulario.</span><span class="sxs-lookup"><span data-stu-id="b6f60-217">When the page is redisplayed by the preceding code, the invalid input is not shown in the form field.</span></span> <span data-ttu-id="b6f60-218">El motivo es que la propiedad de modelo se ha establecido en NULL o en un valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="b6f60-218">This is because the model property has been set to null or a default value.</span></span> <span data-ttu-id="b6f60-219">La entrada no válida sí aparece en un mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="b6f60-219">The invalid input does appear in an error message.</span></span> <span data-ttu-id="b6f60-220">Pero si quiere volver a mostrar los datos incorrectos en el campo de formulario, considere la posibilidad de convertir la propiedad de modelo en una cadena y realizar la conversión de datos de forma manual.</span><span class="sxs-lookup"><span data-stu-id="b6f60-220">But if you want to redisplay the bad data in the form field, consider making the model property a string and doing the data conversion manually.</span></span>

<span data-ttu-id="b6f60-221">Se recomienda la misma estrategia si no quiere que los errores de conversión de tipo generen errores de estado de modelo.</span><span class="sxs-lookup"><span data-stu-id="b6f60-221">The same strategy is recommended if you don't want type conversion errors to result in model state errors.</span></span> <span data-ttu-id="b6f60-222">En ese caso, convierta la propiedad de modelo en una cadena.</span><span class="sxs-lookup"><span data-stu-id="b6f60-222">In that case, make the model property a string.</span></span>

## <a name="simple-types"></a><span data-ttu-id="b6f60-223">Tipos simples</span><span class="sxs-lookup"><span data-stu-id="b6f60-223">Simple types</span></span>

<span data-ttu-id="b6f60-224">Los tipos simples a los que el enlazador de modelos puede convertir las cadenas de origen incluyen los siguientes:</span><span class="sxs-lookup"><span data-stu-id="b6f60-224">The simple types that the model binder can convert source strings into include the following:</span></span>

* [<span data-ttu-id="b6f60-225">Boolean</span><span class="sxs-lookup"><span data-stu-id="b6f60-225">Boolean</span></span>](xref:System.ComponentModel.BooleanConverter)
* <span data-ttu-id="b6f60-226">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span><span class="sxs-lookup"><span data-stu-id="b6f60-226">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span></span>
* [<span data-ttu-id="b6f60-227">Char</span><span class="sxs-lookup"><span data-stu-id="b6f60-227">Char</span></span>](xref:System.ComponentModel.CharConverter)
* [<span data-ttu-id="b6f60-228">DateTime</span><span class="sxs-lookup"><span data-stu-id="b6f60-228">DateTime</span></span>](xref:System.ComponentModel.DateTimeConverter)
* [<span data-ttu-id="b6f60-229">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="b6f60-229">DateTimeOffset</span></span>](xref:System.ComponentModel.DateTimeOffsetConverter)
* [<span data-ttu-id="b6f60-230">Decimal</span><span class="sxs-lookup"><span data-stu-id="b6f60-230">Decimal</span></span>](xref:System.ComponentModel.DecimalConverter)
* [<span data-ttu-id="b6f60-231">Double</span><span class="sxs-lookup"><span data-stu-id="b6f60-231">Double</span></span>](xref:System.ComponentModel.DoubleConverter)
* [<span data-ttu-id="b6f60-232">Enum</span><span class="sxs-lookup"><span data-stu-id="b6f60-232">Enum</span></span>](xref:System.ComponentModel.EnumConverter)
* [<span data-ttu-id="b6f60-233">Guid</span><span class="sxs-lookup"><span data-stu-id="b6f60-233">Guid</span></span>](xref:System.ComponentModel.GuidConverter)
* <span data-ttu-id="b6f60-234">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span><span class="sxs-lookup"><span data-stu-id="b6f60-234">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span></span>
* [<span data-ttu-id="b6f60-235">Single</span><span class="sxs-lookup"><span data-stu-id="b6f60-235">Single</span></span>](xref:System.ComponentModel.SingleConverter)
* [<span data-ttu-id="b6f60-236">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="b6f60-236">TimeSpan</span></span>](xref:System.ComponentModel.TimeSpanConverter)
* <span data-ttu-id="b6f60-237">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span><span class="sxs-lookup"><span data-stu-id="b6f60-237">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span></span>
* [<span data-ttu-id="b6f60-238">Uri</span><span class="sxs-lookup"><span data-stu-id="b6f60-238">Uri</span></span>](xref:System.UriTypeConverter)
* [<span data-ttu-id="b6f60-239">Versión</span><span class="sxs-lookup"><span data-stu-id="b6f60-239">Version</span></span>](xref:System.ComponentModel.VersionConverter)

## <a name="complex-types"></a><span data-ttu-id="b6f60-240">Tipos complejos</span><span class="sxs-lookup"><span data-stu-id="b6f60-240">Complex types</span></span>

<span data-ttu-id="b6f60-241">Un tipo complejo debe tener un constructor público predeterminado y propiedades grabables públicas para enlazar.</span><span class="sxs-lookup"><span data-stu-id="b6f60-241">A complex type must have a public default constructor and public writable properties to bind.</span></span> <span data-ttu-id="b6f60-242">Cuando se produce el enlace de modelos, se crea una instancia de la clase con el constructor predeterminado público.</span><span class="sxs-lookup"><span data-stu-id="b6f60-242">When model binding occurs, the class is instantiated using the public default constructor.</span></span> 

<span data-ttu-id="b6f60-243">Para cada propiedad del tipo complejo, el enlace de modelos busca entre los orígenes el patrón de nombre *prefijo.nombre_de_propiedad*.</span><span class="sxs-lookup"><span data-stu-id="b6f60-243">For each property of the complex type, model binding looks through the sources for the name pattern *prefix.property_name*.</span></span> <span data-ttu-id="b6f60-244">Si no se encuentra nada, solo busca *nombre_de_propiedad* sin el prefijo.</span><span class="sxs-lookup"><span data-stu-id="b6f60-244">If nothing is found, it looks for just *property_name* without the prefix.</span></span>

<span data-ttu-id="b6f60-245">Para el enlace a un parámetro, el prefijo es el nombre del parámetro.</span><span class="sxs-lookup"><span data-stu-id="b6f60-245">For binding to a parameter, the prefix is the parameter name.</span></span> <span data-ttu-id="b6f60-246">Para el enlace a una propiedad pública `PageModel`, el prefijo es el nombre de la propiedad pública.</span><span class="sxs-lookup"><span data-stu-id="b6f60-246">For binding to a `PageModel` public property, the prefix is the public property name.</span></span> <span data-ttu-id="b6f60-247">Algunos atributos tienen una propiedad `Prefix` que permite invalidar el uso predeterminado del nombre de parámetro o propiedad.</span><span class="sxs-lookup"><span data-stu-id="b6f60-247">Some attributes have a `Prefix` property that lets you override the default usage of parameter or property name.</span></span>

<span data-ttu-id="b6f60-248">Por ejemplo, imagine que el tipo complejo es la clase `Instructor` siguiente:</span><span class="sxs-lookup"><span data-stu-id="b6f60-248">For example, suppose the complex type is the following `Instructor` class:</span></span>

  ```csharp
  public class Instructor
  {
      public int ID { get; set; }
      public string LastName { get; set; }
      public string FirstName { get; set; }
  }
  ```

### <a name="prefix--parameter-name"></a><span data-ttu-id="b6f60-249">Prefijo = nombre del parámetro</span><span class="sxs-lookup"><span data-stu-id="b6f60-249">Prefix = parameter name</span></span>

<span data-ttu-id="b6f60-250">Si el modelo que se va a enlazar es un parámetro denominado `instructorToUpdate`:</span><span class="sxs-lookup"><span data-stu-id="b6f60-250">If the model to be bound is a parameter named `instructorToUpdate`:</span></span>

```csharp
public IActionResult OnPost(int? id, Instructor instructorToUpdate)
```

<span data-ttu-id="b6f60-251">El enlace de modelos se inicia mediante el examen de los orígenes para la clave `instructorToUpdate.ID`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-251">Model binding starts by looking through the sources for the key `instructorToUpdate.ID`.</span></span> <span data-ttu-id="b6f60-252">Si no se encuentra, busca `ID` sin un prefijo.</span><span class="sxs-lookup"><span data-stu-id="b6f60-252">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="prefix--property-name"></a><span data-ttu-id="b6f60-253">Prefijo = nombre de propiedad</span><span class="sxs-lookup"><span data-stu-id="b6f60-253">Prefix = property name</span></span>

<span data-ttu-id="b6f60-254">Si el modelo que se va a enlazar es una propiedad denominada `Instructor` del controlador o la clase `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="b6f60-254">If the model to be bound is a property named `Instructor` of the controller or `PageModel` class:</span></span>

```csharp
[BindProperty]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="b6f60-255">El enlace de modelos se inicia mediante el examen de los orígenes para la clave `Instructor.ID`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-255">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="b6f60-256">Si no se encuentra, busca `ID` sin un prefijo.</span><span class="sxs-lookup"><span data-stu-id="b6f60-256">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="custom-prefix"></a><span data-ttu-id="b6f60-257">Prefijo personalizado</span><span class="sxs-lookup"><span data-stu-id="b6f60-257">Custom prefix</span></span>

<span data-ttu-id="b6f60-258">Si el modelo que se va a enlazar es un parámetro denominado `instructorToUpdate` y un atributo `Bind`, especifica `Instructor` como el prefijo:</span><span class="sxs-lookup"><span data-stu-id="b6f60-258">If the model to be bound is a parameter named `instructorToUpdate` and a `Bind` attribute specifies `Instructor` as the prefix:</span></span>

```csharp
public IActionResult OnPost(
    int? id, [Bind(Prefix = "Instructor")] Instructor instructorToUpdate)
```

<span data-ttu-id="b6f60-259">El enlace de modelos se inicia mediante el examen de los orígenes para la clave `Instructor.ID`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-259">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="b6f60-260">Si no se encuentra, busca `ID` sin un prefijo.</span><span class="sxs-lookup"><span data-stu-id="b6f60-260">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="attributes-for-complex-type-targets"></a><span data-ttu-id="b6f60-261">Atributos para destinos de tipo complejo</span><span class="sxs-lookup"><span data-stu-id="b6f60-261">Attributes for complex type targets</span></span>

<span data-ttu-id="b6f60-262">Existen varios atributos integrados para controlar el enlace de modelos de tipos complejos:</span><span class="sxs-lookup"><span data-stu-id="b6f60-262">Several built-in attributes are available for controlling model binding of complex types:</span></span>

* `[BindRequired]`
* `[BindNever]`
* `[Bind]`

> [!NOTE]
> <span data-ttu-id="b6f60-263">Estos atributos afectan al enlace de modelos cuando el origen de los valores son datos de formulario publicados.</span><span class="sxs-lookup"><span data-stu-id="b6f60-263">These attributes affect model binding when posted form data is the source of values.</span></span> <span data-ttu-id="b6f60-264">No afectan a los formateadores de entrada, que procesan los cuerpos de la solicitud JSON y XML publicados.</span><span class="sxs-lookup"><span data-stu-id="b6f60-264">They do not affect input formatters, which process posted JSON and XML request bodies.</span></span> <span data-ttu-id="b6f60-265">Los formateadores de entrada se explican [más adelante en este artículo](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="b6f60-265">Input formatters are explained [later in this article](#input-formatters).</span></span>
>
> <span data-ttu-id="b6f60-266">Vea también la explicación del atributo `[Required]` en [Validación de modelos](xref:mvc/models/validation#required-attribute).</span><span class="sxs-lookup"><span data-stu-id="b6f60-266">See also the discussion of the `[Required]` attribute in [Model validation](xref:mvc/models/validation#required-attribute).</span></span>

### <a name="bindrequired-attribute"></a><span data-ttu-id="b6f60-267">Atributo [BindRequired]</span><span class="sxs-lookup"><span data-stu-id="b6f60-267">[BindRequired] attribute</span></span>

<span data-ttu-id="b6f60-268">Solo se puede aplicar a propiedades del modelo, no a parámetros de método.</span><span class="sxs-lookup"><span data-stu-id="b6f60-268">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="b6f60-269">Hace que el enlace de modelos agregue un error de estado de modelo si no se puede realizar el enlace para la propiedad de un modelo.</span><span class="sxs-lookup"><span data-stu-id="b6f60-269">Causes model binding to add a model state error if binding cannot occur for a model's property.</span></span> <span data-ttu-id="b6f60-270">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="b6f60-270">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/InstructorWithCollection.cs?name=snippet_BindRequired&highlight=8-9)]

### <a name="bindnever-attribute"></a><span data-ttu-id="b6f60-271">Atributo [BindNever]</span><span class="sxs-lookup"><span data-stu-id="b6f60-271">[BindNever] attribute</span></span>

<span data-ttu-id="b6f60-272">Solo se puede aplicar a propiedades del modelo, no a parámetros de método.</span><span class="sxs-lookup"><span data-stu-id="b6f60-272">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="b6f60-273">Impide que el enlace de modelos establezca la propiedad de un modelo.</span><span class="sxs-lookup"><span data-stu-id="b6f60-273">Prevents model binding from setting a model's property.</span></span> <span data-ttu-id="b6f60-274">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="b6f60-274">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/InstructorWithDictionary.cs?name=snippet_BindNever&highlight=3-4)]

### <a name="bind-attribute"></a><span data-ttu-id="b6f60-275">Atributo [Bind]</span><span class="sxs-lookup"><span data-stu-id="b6f60-275">[Bind] attribute</span></span>

<span data-ttu-id="b6f60-276">Se puede aplicar a una clase o un parámetro de método.</span><span class="sxs-lookup"><span data-stu-id="b6f60-276">Can be applied to a class or a method parameter.</span></span> <span data-ttu-id="b6f60-277">Especifica qué propiedades de un modelo se deben incluir en el enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="b6f60-277">Specifies which properties of a model should be included in model binding.</span></span>

<span data-ttu-id="b6f60-278">En el ejemplo siguiente, solo se enlazan las propiedades especificadas del modelo `Instructor` cuando se llama a cualquier método de acción o controlador:</span><span class="sxs-lookup"><span data-stu-id="b6f60-278">In the following example, only the specified properties of the `Instructor` model are bound when any handler or action method is called:</span></span>

```csharp
[Bind("LastName,FirstMidName,HireDate")]
public class Instructor
```

<span data-ttu-id="b6f60-279">En el ejemplo siguiente, solo se enlazan las propiedades especificadas del modelo `Instructor` cuando se llama al método `OnPost`:</span><span class="sxs-lookup"><span data-stu-id="b6f60-279">In the following example, only the specified properties of the `Instructor` model are bound when the `OnPost` method is called:</span></span>

```csharp
[HttpPost]
public IActionResult OnPost([Bind("LastName,FirstMidName,HireDate")] Instructor instructor)
```

<span data-ttu-id="b6f60-280">El atributo `[Bind]` se puede usar para protegerse de la publicación excesiva en escenarios de *creación*.</span><span class="sxs-lookup"><span data-stu-id="b6f60-280">The `[Bind]` attribute can be used to protect against overposting in *create* scenarios.</span></span> <span data-ttu-id="b6f60-281">No funciona bien en escenarios de edición porque las propiedades excluidas se establecen en NULL o en un valor predeterminado en lugar de mantenerse sin cambios.</span><span class="sxs-lookup"><span data-stu-id="b6f60-281">It doesn't work well in edit scenarios because excluded properties are set to null or a default value instead of being left unchanged.</span></span> <span data-ttu-id="b6f60-282">Para defenderse de la publicación excesiva, se recomiendan modelos de vista en lugar del atributo `[Bind]`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-282">For defense against overposting, view models are recommended rather than the `[Bind]` attribute.</span></span> <span data-ttu-id="b6f60-283">Para más información, vea [Nota de seguridad sobre la publicación excesiva](xref:data/ef-mvc/crud#security-note-about-overposting).</span><span class="sxs-lookup"><span data-stu-id="b6f60-283">For more information, see [Security note about overposting](xref:data/ef-mvc/crud#security-note-about-overposting).</span></span>

## <a name="collections"></a><span data-ttu-id="b6f60-284">Colecciones</span><span class="sxs-lookup"><span data-stu-id="b6f60-284">Collections</span></span>

<span data-ttu-id="b6f60-285">Para los destinos que son colecciones de tipos simples, el enlace de modelos busca coincidencias con *nombre_de_parámetro* o *nombre_de_propiedad*.</span><span class="sxs-lookup"><span data-stu-id="b6f60-285">For targets that are collections of simple types, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="b6f60-286">Si no se encuentra ninguna coincidencia, busca uno de los formatos admitidos sin el prefijo.</span><span class="sxs-lookup"><span data-stu-id="b6f60-286">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="b6f60-287">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="b6f60-287">For example:</span></span>

* <span data-ttu-id="b6f60-288">Imagine que el parámetro que se va a enlazar es una matriz llamada `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="b6f60-288">Suppose the parameter to be bound is an array named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, int[] selectedCourses)
  ```

* <span data-ttu-id="b6f60-289">Los datos de cadena de consulta o formulario pueden estar en uno de los formatos siguientes:</span><span class="sxs-lookup"><span data-stu-id="b6f60-289">Form or query string data can be in one of the following formats:</span></span>
   
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

* <span data-ttu-id="b6f60-290">El formato siguiente solo se admite en datos de formulario:</span><span class="sxs-lookup"><span data-stu-id="b6f60-290">The following format is supported only in form data:</span></span>

  ```
  selectedCourses[]=1050&selectedCourses[]=2000
  ```

* <span data-ttu-id="b6f60-291">Para todos los formatos de ejemplo anteriores, el enlace de modelos pasa una matriz de dos elementos al parámetro `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="b6f60-291">For all of the preceding example formats, model binding passes an array of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="b6f60-292">selectedCourses[0]=1050</span><span class="sxs-lookup"><span data-stu-id="b6f60-292">selectedCourses[0]=1050</span></span>
  * <span data-ttu-id="b6f60-293">selectedCourses[1]=2000</span><span class="sxs-lookup"><span data-stu-id="b6f60-293">selectedCourses[1]=2000</span></span>

  <span data-ttu-id="b6f60-294">Los formatos de datos que usan números de subíndice (... [0] ... [1] ...) deben asegurarse de que se numeran de forma secuencial a partir de cero.</span><span class="sxs-lookup"><span data-stu-id="b6f60-294">Data formats that use subscript numbers (... [0] ... [1] ...) must ensure that they are numbered sequentially starting at zero.</span></span> <span data-ttu-id="b6f60-295">Si hay algún hueco en la numeración de los subíndices, se omiten todos los elementos que aparecen después del hueco.</span><span class="sxs-lookup"><span data-stu-id="b6f60-295">If there are any gaps in subscript numbering, all items after the gap are ignored.</span></span> <span data-ttu-id="b6f60-296">Por ejemplo, si los subíndices son 0 y 2 en lugar de 0 y 1, se omite el segundo elemento.</span><span class="sxs-lookup"><span data-stu-id="b6f60-296">For example, if the subscripts are 0 and 2 instead of 0 and 1, the second item is ignored.</span></span>

## <a name="dictionaries"></a><span data-ttu-id="b6f60-297">Diccionarios</span><span class="sxs-lookup"><span data-stu-id="b6f60-297">Dictionaries</span></span>

<span data-ttu-id="b6f60-298">Para los destinos `Dictionary`, el enlace de modelos busca coincidencias con *nombre_de_parámetro* o *nombre_de_propiedad*.</span><span class="sxs-lookup"><span data-stu-id="b6f60-298">For `Dictionary` targets, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="b6f60-299">Si no se encuentra ninguna coincidencia, busca uno de los formatos admitidos sin el prefijo.</span><span class="sxs-lookup"><span data-stu-id="b6f60-299">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="b6f60-300">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="b6f60-300">For example:</span></span>

* <span data-ttu-id="b6f60-301">Imagine que el parámetro de destino es un elemento `Dictionary<int, string>` denominado `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="b6f60-301">Suppose the target parameter is a `Dictionary<int, string>` named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, Dictionary<int, string> selectedCourses)
  ```

* <span data-ttu-id="b6f60-302">Los datos de cadena de consulta o de formulario publicados pueden ser similares a uno de los ejemplos siguientes:</span><span class="sxs-lookup"><span data-stu-id="b6f60-302">The posted form or query string data can look like one of the following examples:</span></span>

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

* <span data-ttu-id="b6f60-303">Para todos los formatos de ejemplo anteriores, el enlace de modelos pasa un diccionario de dos elementos al parámetro `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="b6f60-303">For all of the preceding example formats, model binding passes a dictionary of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="b6f60-304">selectedCourses["1050"]="Chemistry"</span><span class="sxs-lookup"><span data-stu-id="b6f60-304">selectedCourses["1050"]="Chemistry"</span></span>
  * <span data-ttu-id="b6f60-305">selectedCourses["2000"]="Economics"</span><span class="sxs-lookup"><span data-stu-id="b6f60-305">selectedCourses["2000"]="Economics"</span></span>

<a name="glob"></a>

## <a name="globalization-behavior-of-model-binding-route-data-and-query-strings"></a><span data-ttu-id="b6f60-306">Comportamiento de globalización del enlace de modelos datos de ruta y cadenas de consulta</span><span class="sxs-lookup"><span data-stu-id="b6f60-306">Globalization behavior of model binding route data and query strings</span></span>

<span data-ttu-id="b6f60-307">El proveedor de valores de ruta de ASP.NET Core y el proveedor de valores de cadena de consulta:</span><span class="sxs-lookup"><span data-stu-id="b6f60-307">The ASP.NET Core route value provider and query string value provider:</span></span>

* <span data-ttu-id="b6f60-308">Tratan los valores como referencia cultural de todos los idiomas.</span><span class="sxs-lookup"><span data-stu-id="b6f60-308">Treat values as invariant culture.</span></span>
* <span data-ttu-id="b6f60-309">Esperan que las direcciones URL sean independientes de la referencia cultural.</span><span class="sxs-lookup"><span data-stu-id="b6f60-309">Expect that URLs are culture-invariant.</span></span>

<span data-ttu-id="b6f60-310">Por el contrario, los valores procedentes de datos de formulario se someten a una conversión que tiene en cuenta la referencia cultural.</span><span class="sxs-lookup"><span data-stu-id="b6f60-310">In contrast, values coming from form data undergo a culture-sensitive conversion.</span></span> <span data-ttu-id="b6f60-311">Esto es así por diseño, para que las direcciones URL se puedan compartir entre configuraciones regionales.</span><span class="sxs-lookup"><span data-stu-id="b6f60-311">This is by design so that URLs are shareable across locales.</span></span>

<span data-ttu-id="b6f60-312">Para que el proveedor de valores de ruta de ASP.NET Core y el proveedor de valores de cadena de consulta se sometan a una conversión dependiente de la referencia cultural:</span><span class="sxs-lookup"><span data-stu-id="b6f60-312">To make the ASP.NET Core route value provider and query string value provider undergo a culture-sensitive conversion:</span></span>

* <span data-ttu-id="b6f60-313">Heredan de <xref:Microsoft.AspNetCore.Mvc.ModelBinding.IValueProviderFactory>.</span><span class="sxs-lookup"><span data-stu-id="b6f60-313">Inherit from <xref:Microsoft.AspNetCore.Mvc.ModelBinding.IValueProviderFactory></span></span>
* <span data-ttu-id="b6f60-314">Copie el código de [QueryStringValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs) o [RouteValueValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/RouteValueProviderFactory.cs)</span><span class="sxs-lookup"><span data-stu-id="b6f60-314">Copy the code from [QueryStringValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs) or [RouteValueValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/RouteValueProviderFactory.cs)</span></span>
* <span data-ttu-id="b6f60-315">Reemplace el [valor de referencia cultural](https://github.com/aspnet/AspNetCore/blob/e625fe29b049c60242e8048b4ea743cca65aa7b5/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs#L30) pasado al constructor de proveedor de valores con [CultureInfo.CurrentCulture](xref:System.Globalization.CultureInfo.CurrentCulture)</span><span class="sxs-lookup"><span data-stu-id="b6f60-315">Replace the [culture value](https://github.com/aspnet/AspNetCore/blob/e625fe29b049c60242e8048b4ea743cca65aa7b5/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs#L30) passed to the value provider constructor with [CultureInfo.CurrentCulture](xref:System.Globalization.CultureInfo.CurrentCulture)</span></span>
* <span data-ttu-id="b6f60-316">Reemplace el generador de proveedores de valor predeterminado en las opciones de MVC por el nuevo:</span><span class="sxs-lookup"><span data-stu-id="b6f60-316">Replace the default value provider factory in MVC options with your new one:</span></span>

[!code-csharp[](model-binding/samples_snapshot/2.x/Startup.cs?name=snippet)]
[!code-csharp[](model-binding/samples_snapshot/2.x/Startup.cs?name=snippet1)]

## <a name="special-data-types"></a><span data-ttu-id="b6f60-317">Tipos de datos especiales</span><span class="sxs-lookup"><span data-stu-id="b6f60-317">Special data types</span></span>

<span data-ttu-id="b6f60-318">Hay algunos tipos de datos especiales que el enlace de modelos puede controlar.</span><span class="sxs-lookup"><span data-stu-id="b6f60-318">There are some special data types that model binding can handle.</span></span>

### <a name="iformfile-and-iformfilecollection"></a><span data-ttu-id="b6f60-319">IFormFile e IFormFileCollection</span><span class="sxs-lookup"><span data-stu-id="b6f60-319">IFormFile and IFormFileCollection</span></span>

<span data-ttu-id="b6f60-320">Un archivo cargado incluido en la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="b6f60-320">An uploaded file included in the HTTP request.</span></span>  <span data-ttu-id="b6f60-321">También se admite `IEnumerable<IFormFile>` para varios archivos.</span><span class="sxs-lookup"><span data-stu-id="b6f60-321">Also supported is `IEnumerable<IFormFile>` for multiple files.</span></span>

### <a name="cancellationtoken"></a><span data-ttu-id="b6f60-322">CancellationToken</span><span class="sxs-lookup"><span data-stu-id="b6f60-322">CancellationToken</span></span>

<span data-ttu-id="b6f60-323">se usa para cancelar la actividad en controladores asincrónicos.</span><span class="sxs-lookup"><span data-stu-id="b6f60-323">Used to cancel activity in asynchronous controllers.</span></span>

### <a name="formcollection"></a><span data-ttu-id="b6f60-324">FormCollection</span><span class="sxs-lookup"><span data-stu-id="b6f60-324">FormCollection</span></span>

<span data-ttu-id="b6f60-325">Se usa para recuperar todos los valores de los datos de formulario publicados.</span><span class="sxs-lookup"><span data-stu-id="b6f60-325">Used to retrieve all the values from posted form data.</span></span>

## <a name="input-formatters"></a><span data-ttu-id="b6f60-326">Formateadores de entrada</span><span class="sxs-lookup"><span data-stu-id="b6f60-326">Input formatters</span></span>

<span data-ttu-id="b6f60-327">Los datos del cuerpo de la solicitud pueden estar en XML, JSON u otro formato.</span><span class="sxs-lookup"><span data-stu-id="b6f60-327">Data in the request body can be in JSON, XML, or some other format.</span></span> <span data-ttu-id="b6f60-328">Para analizar estos datos, el enlace de modelos usa un *formateador de entrada* configurado para controlar un tipo de contenido determinado.</span><span class="sxs-lookup"><span data-stu-id="b6f60-328">To parse this data, model binding uses an *input formatter* that is configured to handle a particular content type.</span></span> <span data-ttu-id="b6f60-329">De forma predeterminada, en ASP.NET Core se incluyen formateadores de entrada basados en JSON para el control de los datos JSON.</span><span class="sxs-lookup"><span data-stu-id="b6f60-329">By default, ASP.NET Core includes JSON based input formatters for handling JSON data.</span></span> <span data-ttu-id="b6f60-330">Puede agregar otros formateadores para otros tipos de contenido.</span><span class="sxs-lookup"><span data-stu-id="b6f60-330">You can add other formatters for other content types.</span></span>

<span data-ttu-id="b6f60-331">ASP.NET Core selecciona los formateadores de entrada en función del atributo [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute).</span><span class="sxs-lookup"><span data-stu-id="b6f60-331">ASP.NET Core selects input formatters based on the [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) attribute.</span></span> <span data-ttu-id="b6f60-332">Si no hay ningún atributo, usa el [encabezado Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span><span class="sxs-lookup"><span data-stu-id="b6f60-332">If no attribute is present, it uses the [Content-Type header](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span></span>

<span data-ttu-id="b6f60-333">Para usar los formateadores de entrada XML integrados:</span><span class="sxs-lookup"><span data-stu-id="b6f60-333">To use the built-in XML input formatters:</span></span>

* <span data-ttu-id="b6f60-334">Instale el paquete NuGet `Microsoft.AspNetCore.Mvc.Formatters.Xml`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-334">Install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

* <span data-ttu-id="b6f60-335">En `Startup.ConfigureServices`, llame a <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> o a <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span><span class="sxs-lookup"><span data-stu-id="b6f60-335">In `Startup.ConfigureServices`, call <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> or <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span></span>

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=9)]

* <span data-ttu-id="b6f60-336">Aplique el atributo `Consumes` a las clases de controlador o los métodos de acción que deben esperar XML en el cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="b6f60-336">Apply the `Consumes` attribute to controller classes or action methods that should expect XML in the request body.</span></span>

  ```csharp
  [HttpPost]
  [Consumes("application/xml")]
  public ActionResult<Pet> Create(Pet pet)
  ```

  <span data-ttu-id="b6f60-337">Para más información, vea [Introducción de la serialización XML](/dotnet/standard/serialization/introducing-xml-serialization).</span><span class="sxs-lookup"><span data-stu-id="b6f60-337">For more information, see [Introducing XML Serialization](/dotnet/standard/serialization/introducing-xml-serialization).</span></span>

## <a name="exclude-specified-types-from-model-binding"></a><span data-ttu-id="b6f60-338">Exclusión de tipos especificados del enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="b6f60-338">Exclude specified types from model binding</span></span>

<span data-ttu-id="b6f60-339">El comportamiento del enlace de modelos y los sistemas de validación se controla mediante [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span><span class="sxs-lookup"><span data-stu-id="b6f60-339">The model binding and validation systems' behavior is driven by [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span></span> <span data-ttu-id="b6f60-340">Puede personalizar `ModelMetadata` mediante la adición de un proveedor de detalles a [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span><span class="sxs-lookup"><span data-stu-id="b6f60-340">You can customize `ModelMetadata` by adding a details provider to [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span></span> <span data-ttu-id="b6f60-341">Los proveedores de detalles integrados están disponibles para deshabilitar el enlace de modelos o la validación para tipos especificados.</span><span class="sxs-lookup"><span data-stu-id="b6f60-341">Built-in details providers are available for disabling model binding or validation for specified types.</span></span>

<span data-ttu-id="b6f60-342">Para deshabilitar el enlace de modelos en todos los modelos de un tipo especificado, agregue una instancia de <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-342">To disable model binding on all models of a specified type, add an <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b6f60-343">Por ejemplo, para deshabilitar el enlace de modelos en todos los modelos del tipo `System.Version`:</span><span class="sxs-lookup"><span data-stu-id="b6f60-343">For example, to disable model binding on all models of type `System.Version`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=4-5)]

<span data-ttu-id="b6f60-344">Para deshabilitar la validación en propiedades de un tipo especificado, agregue una instancia de <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-344">To disable validation on properties of a specified type, add a <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b6f60-345">Por ejemplo, para deshabilitar la validación en las propiedades de tipo `System.Guid`:</span><span class="sxs-lookup"><span data-stu-id="b6f60-345">For example, to disable validation on properties of type `System.Guid`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=6-7)]

## <a name="custom-model-binders"></a><span data-ttu-id="b6f60-346">Enlazadores de modelos personalizados</span><span class="sxs-lookup"><span data-stu-id="b6f60-346">Custom model binders</span></span>

<span data-ttu-id="b6f60-347">Puede ampliar el enlace de modelos si escribe un enlazador de modelos personalizado y usa el atributo `[ModelBinder]` para seleccionarlo para un destino concreto.</span><span class="sxs-lookup"><span data-stu-id="b6f60-347">You can extend model binding by writing a custom model binder and using the `[ModelBinder]` attribute to select it for a given target.</span></span> <span data-ttu-id="b6f60-348">Más información sobre el [enlace de modelos personalizados](xref:mvc/advanced/custom-model-binding).</span><span class="sxs-lookup"><span data-stu-id="b6f60-348">Learn more about [custom model binding](xref:mvc/advanced/custom-model-binding).</span></span>

## <a name="manual-model-binding"></a><span data-ttu-id="b6f60-349">Enlace de modelos manual</span><span class="sxs-lookup"><span data-stu-id="b6f60-349">Manual model binding</span></span>

<span data-ttu-id="b6f60-350">El enlace de modelos se puede invocar de forma manual mediante el método <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>.</span><span class="sxs-lookup"><span data-stu-id="b6f60-350">Model binding can be invoked manually by using the <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> method.</span></span> <span data-ttu-id="b6f60-351">El método se define en las clases `ControllerBase` y `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-351">The method is defined on both `ControllerBase` and `PageModel` classes.</span></span> <span data-ttu-id="b6f60-352">Las sobrecargas de método permiten especificar el prefijo y el proveedor de valores que se van a usar.</span><span class="sxs-lookup"><span data-stu-id="b6f60-352">Method overloads let you specify the prefix and value provider to use.</span></span> <span data-ttu-id="b6f60-353">El método devuelve `false` si se produce un error en el enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="b6f60-353">The method returns `false` if model binding fails.</span></span> <span data-ttu-id="b6f60-354">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="b6f60-354">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/InstructorsWithCollection/Create.cshtml.cs?name=snippet_TryUpdate&highlight=1-4)]

## <a name="fromservices-attribute"></a><span data-ttu-id="b6f60-355">Atributo [FromServices]</span><span class="sxs-lookup"><span data-stu-id="b6f60-355">[FromServices] attribute</span></span>

<span data-ttu-id="b6f60-356">El nombre de este atributo sigue el patrón de los atributos de enlace de modelos que especifican un origen de datos.</span><span class="sxs-lookup"><span data-stu-id="b6f60-356">This attribute's name follows the pattern of model binding attributes that specify a data source.</span></span> <span data-ttu-id="b6f60-357">Pero no se trata de enlazar datos desde un proveedor de valores.</span><span class="sxs-lookup"><span data-stu-id="b6f60-357">But it's not about binding data from a value provider.</span></span> <span data-ttu-id="b6f60-358">Obtiene una instancia de un tipo desde el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b6f60-358">It gets an instance of a type from the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="b6f60-359">Su objetivo es proporcionar una alternativa a la inserción de constructores cuando se necesita un servicio solo si se llama a un método concreto.</span><span class="sxs-lookup"><span data-stu-id="b6f60-359">Its purpose is to provide an alternative to constructor injection for when you need a service only if a particular method is called.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b6f60-360">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="b6f60-360">Additional resources</span></span>

* <xref:mvc/models/validation>
* <xref:mvc/advanced/custom-model-binding>

::: moniker-end
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="b6f60-361">En este artículo se explica qué es el enlace de modelos, cómo funciona y cómo personalizar su comportamiento.</span><span class="sxs-lookup"><span data-stu-id="b6f60-361">This article explains what model binding is, how it works, and how to customize its behavior.</span></span>

<span data-ttu-id="b6f60-362">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="b6f60-362">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="what-is-model-binding"></a><span data-ttu-id="b6f60-363">Qué es el enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="b6f60-363">What is Model binding</span></span>

<span data-ttu-id="b6f60-364">Los controladores y Razor Pages trabajan con datos procedentes de solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="b6f60-364">Controllers and Razor pages work with data that comes from HTTP requests.</span></span> <span data-ttu-id="b6f60-365">Por ejemplo, los datos de ruta pueden proporcionar una clave de registro y los campos de formulario publicados pueden proporcionar valores para las propiedades del modelo.</span><span class="sxs-lookup"><span data-stu-id="b6f60-365">For example, route data may provide a record key, and posted form fields may provide values for the properties of the model.</span></span> <span data-ttu-id="b6f60-366">La escritura de código para recuperar cada uno de estos valores y convertirlos de cadenas a tipos de .NET sería tediosa y propensa a errores.</span><span class="sxs-lookup"><span data-stu-id="b6f60-366">Writing code to retrieve each of these values and convert them from strings to .NET types would be tedious and error-prone.</span></span> <span data-ttu-id="b6f60-367">El enlace de modelos automatiza este proceso.</span><span class="sxs-lookup"><span data-stu-id="b6f60-367">Model binding automates this process.</span></span> <span data-ttu-id="b6f60-368">El sistema de enlace de modelos:</span><span class="sxs-lookup"><span data-stu-id="b6f60-368">The model binding system:</span></span>

* <span data-ttu-id="b6f60-369">Recupera datos de diversos orígenes, como datos de ruta, campos de formulario y cadenas de consulta.</span><span class="sxs-lookup"><span data-stu-id="b6f60-369">Retrieves data from various sources such as route data, form fields, and query strings.</span></span>
* <span data-ttu-id="b6f60-370">Proporciona los datos a los controladores y Razor Pages en parámetros de método y propiedades públicas.</span><span class="sxs-lookup"><span data-stu-id="b6f60-370">Provides the data to controllers and Razor pages in method parameters and public properties.</span></span>
* <span data-ttu-id="b6f60-371">Convierte datos de cadena en tipos de .NET.</span><span class="sxs-lookup"><span data-stu-id="b6f60-371">Converts string data to .NET types.</span></span>
* <span data-ttu-id="b6f60-372">Actualiza las propiedades de tipos complejos.</span><span class="sxs-lookup"><span data-stu-id="b6f60-372">Updates properties of complex types.</span></span>

## <a name="example"></a><span data-ttu-id="b6f60-373">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="b6f60-373">Example</span></span>

<span data-ttu-id="b6f60-374">Imagine que tiene el siguiente método de acción:</span><span class="sxs-lookup"><span data-stu-id="b6f60-374">Suppose you have the following action method:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Controllers/PetsController.cs?name=snippet_DogsOnly)]

<span data-ttu-id="b6f60-375">Y que la aplicación recibe una solicitud con esta dirección URL:</span><span class="sxs-lookup"><span data-stu-id="b6f60-375">And the app receives a request with this URL:</span></span>

```
http://contoso.com/api/pets/2?DogsOnly=true
```

<span data-ttu-id="b6f60-376">El enlace de modelos realiza los pasos siguientes después de que el sistema de enrutamiento selecciona el método de acción:</span><span class="sxs-lookup"><span data-stu-id="b6f60-376">Model binding goes through the following steps after the routing system selects the action method:</span></span>

* <span data-ttu-id="b6f60-377">Busca el primer parámetro de `GetByID`, un entero denominado `id`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-377">Finds the first parameter of `GetByID`, an integer named `id`.</span></span>
* <span data-ttu-id="b6f60-378">Examina los orígenes disponibles en la solicitud HTTP y busca `id` = "2" en los datos de ruta.</span><span class="sxs-lookup"><span data-stu-id="b6f60-378">Looks through the available sources in the HTTP request and finds `id` = "2" in route data.</span></span>
* <span data-ttu-id="b6f60-379">Convierte la cadena "2" en el entero 2.</span><span class="sxs-lookup"><span data-stu-id="b6f60-379">Converts the string "2" into integer 2.</span></span>
* <span data-ttu-id="b6f60-380">Busca el siguiente parámetro de `GetByID`, un valor booleano denominado `dogsOnly`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-380">Finds the next parameter of `GetByID`, a boolean named `dogsOnly`.</span></span>
* <span data-ttu-id="b6f60-381">Examina los orígenes y busca "DogsOnly=true" en la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="b6f60-381">Looks through the sources and finds "DogsOnly=true" in the query string.</span></span> <span data-ttu-id="b6f60-382">La coincidencia de nombres no distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="b6f60-382">Name matching is not case-sensitive.</span></span>
* <span data-ttu-id="b6f60-383">Convierte la cadena "true" en un valor booleano `true`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-383">Converts the string "true" into boolean `true`.</span></span>

<span data-ttu-id="b6f60-384">Después, el marco llama al método `GetById` y pasa 2 para el parámetro `id` y `true` para el parámetro `dogsOnly`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-384">The framework then calls the `GetById` method, passing in 2 for the `id` parameter, and `true` for the `dogsOnly` parameter.</span></span>

<span data-ttu-id="b6f60-385">En el ejemplo anterior, los destinos de enlace de modelos son parámetros de método que son tipos simples.</span><span class="sxs-lookup"><span data-stu-id="b6f60-385">In the preceding example, the model binding targets are method parameters that are simple types.</span></span> <span data-ttu-id="b6f60-386">Los destinos también pueden ser las propiedades de un tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="b6f60-386">Targets may also be the properties of a complex type.</span></span> <span data-ttu-id="b6f60-387">Después de que cada propiedad se haya enlazado correctamente, se produce la [validación de modelos](xref:mvc/models/validation) para esa propiedad.</span><span class="sxs-lookup"><span data-stu-id="b6f60-387">After each property is successfully bound, [model validation](xref:mvc/models/validation) occurs for that property.</span></span> <span data-ttu-id="b6f60-388">El registro de qué datos se han enlazado al modelo y los errores de enlace o validación se almacenan en [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) o [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span><span class="sxs-lookup"><span data-stu-id="b6f60-388">The record of what data is bound to the model, and any binding or validation errors, is stored in [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) or [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span></span> <span data-ttu-id="b6f60-389">Para averiguar si este proceso se ha realizado correctamente, la aplicación comprueba la marca [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid).</span><span class="sxs-lookup"><span data-stu-id="b6f60-389">To find out if this process was successful, the app checks the [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) flag.</span></span>

## <a name="targets"></a><span data-ttu-id="b6f60-390">Destinos</span><span class="sxs-lookup"><span data-stu-id="b6f60-390">Targets</span></span>

<span data-ttu-id="b6f60-391">El enlace de modelos intenta encontrar valores para los tipos de destinos siguientes:</span><span class="sxs-lookup"><span data-stu-id="b6f60-391">Model binding tries to find values for the following kinds of targets:</span></span>

* <span data-ttu-id="b6f60-392">Parámetros del método de acción de controlador al que se enruta una solicitud.</span><span class="sxs-lookup"><span data-stu-id="b6f60-392">Parameters of the controller action method that a request is routed to.</span></span>
* <span data-ttu-id="b6f60-393">Parámetros del método de controlador de Razor Pages al que se enruta una solicitud.</span><span class="sxs-lookup"><span data-stu-id="b6f60-393">Parameters of the Razor Pages handler method that a request is routed to.</span></span> 
* <span data-ttu-id="b6f60-394">Propiedades públicas de un controlador o una clase `PageModel`, si se especifican mediante atributos.</span><span class="sxs-lookup"><span data-stu-id="b6f60-394">Public properties of a controller or `PageModel` class, if specified by attributes.</span></span>

### <a name="bindproperty-attribute"></a><span data-ttu-id="b6f60-395">Atributo [BindProperty]</span><span class="sxs-lookup"><span data-stu-id="b6f60-395">[BindProperty] attribute</span></span>

<span data-ttu-id="b6f60-396">Se puede aplicar a una propiedad pública de un controlador o una clase `PageModel` para hacer que el enlace de modelos tenga esa propiedad como destino:</span><span class="sxs-lookup"><span data-stu-id="b6f60-396">Can be applied to a public property of a controller or `PageModel` class to cause model binding to target that property:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Edit.cshtml.cs?name=snippet_BindProperty&highlight=3-4)]

### <a name="bindpropertiesattribute"></a><span data-ttu-id="b6f60-397">Atributo [BindProperties]</span><span class="sxs-lookup"><span data-stu-id="b6f60-397">[BindProperties] attribute</span></span>

<span data-ttu-id="b6f60-398">Disponible en ASP.NET 2.1 Core y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="b6f60-398">Available in ASP.NET Core 2.1 and later.</span></span>  <span data-ttu-id="b6f60-399">Se pueden aplicar a un controlador o una clase `PageModel` para indicar al enlace de modelos que seleccione como destino todas las propiedades públicas de la clase:</span><span class="sxs-lookup"><span data-stu-id="b6f60-399">Can be applied to a controller or `PageModel` class to tell model binding to target all public properties of the class:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_BindProperties&highlight=1-2)]

### <a name="model-binding-for-http-get-requests"></a><span data-ttu-id="b6f60-400">Enlace de modelos para solicitudes HTTP GET</span><span class="sxs-lookup"><span data-stu-id="b6f60-400">Model binding for HTTP GET requests</span></span>

<span data-ttu-id="b6f60-401">De forma predeterminada, las propiedades no se enlazan para las solicitudes HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="b6f60-401">By default, properties are not bound for HTTP GET requests.</span></span> <span data-ttu-id="b6f60-402">Normalmente, todo lo que necesita para una solicitud GET es un parámetro de id. de registro.</span><span class="sxs-lookup"><span data-stu-id="b6f60-402">Typically, all you need for a GET request is a record ID parameter.</span></span> <span data-ttu-id="b6f60-403">El id. de registro se usa para buscar el elemento en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b6f60-403">The record ID is used to look up the item in the database.</span></span> <span data-ttu-id="b6f60-404">Por tanto, no es necesario enlazar una propiedad que contiene una instancia del modelo.</span><span class="sxs-lookup"><span data-stu-id="b6f60-404">Therefore, there is no need to bind a property that holds an instance of the model.</span></span> <span data-ttu-id="b6f60-405">En escenarios donde quiera propiedades enlazadas a datos de las solicitudes GET, establezca la propiedad `SupportsGet` en `true`:</span><span class="sxs-lookup"><span data-stu-id="b6f60-405">In scenarios where you do want properties bound to data from GET requests, set the `SupportsGet` property to `true`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_SupportsGet)]

## <a name="sources"></a><span data-ttu-id="b6f60-406">Orígenes</span><span class="sxs-lookup"><span data-stu-id="b6f60-406">Sources</span></span>

<span data-ttu-id="b6f60-407">De forma predeterminada, el enlace de modelos obtiene datos en forma de pares clave-valor de los siguientes orígenes de una solicitud HTTP:</span><span class="sxs-lookup"><span data-stu-id="b6f60-407">By default, model binding gets data in the form of key-value pairs from the following sources in an HTTP request:</span></span>

1. <span data-ttu-id="b6f60-408">Campos de formulario</span><span class="sxs-lookup"><span data-stu-id="b6f60-408">Form fields</span></span>
1. <span data-ttu-id="b6f60-409">El cuerpo de la solicitud (para [controladores que tienen el atributo [ApiController]](xref:web-api/index#binding-source-parameter-inference)).</span><span class="sxs-lookup"><span data-stu-id="b6f60-409">The request body (For [controllers that have the [ApiController] attribute](xref:web-api/index#binding-source-parameter-inference).)</span></span>
1. <span data-ttu-id="b6f60-410">Datos de ruta</span><span class="sxs-lookup"><span data-stu-id="b6f60-410">Route data</span></span>
1. <span data-ttu-id="b6f60-411">Parámetros de cadena de consulta</span><span class="sxs-lookup"><span data-stu-id="b6f60-411">Query string parameters</span></span>
1. <span data-ttu-id="b6f60-412">Archivos cargados</span><span class="sxs-lookup"><span data-stu-id="b6f60-412">Uploaded files</span></span>

<span data-ttu-id="b6f60-413">Para cada parámetro o propiedad de destino, se examinan los orígenes en el orden indicado en la lista anterior.</span><span class="sxs-lookup"><span data-stu-id="b6f60-413">For each target parameter or property, the sources are scanned in the order indicated in the preceding list.</span></span> <span data-ttu-id="b6f60-414">Hay algunas excepciones:</span><span class="sxs-lookup"><span data-stu-id="b6f60-414">There are a few exceptions:</span></span>

* <span data-ttu-id="b6f60-415">Los datos de ruta y los valores de cadena de consulta solo se usan para tipos simples.</span><span class="sxs-lookup"><span data-stu-id="b6f60-415">Route data and query string values are used only for simple types.</span></span>
* <span data-ttu-id="b6f60-416">Los archivos cargados solo se enlazan a tipos de destino que implementan `IFormFile` o `IEnumerable<IFormFile>`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-416">Uploaded files are bound only to target types that implement `IFormFile` or `IEnumerable<IFormFile>`.</span></span>

<span data-ttu-id="b6f60-417">Si el origen predeterminado no es correcto, use uno de los atributos siguientes para especificar el origen:</span><span class="sxs-lookup"><span data-stu-id="b6f60-417">If the default source is not correct, use one of the following attributes to specify the source:</span></span>

* <span data-ttu-id="b6f60-418">[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute): obtiene valores de la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="b6f60-418">[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) - Gets values from the query string.</span></span> 
* <span data-ttu-id="b6f60-419">[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute): obtiene valores de los datos de ruta.</span><span class="sxs-lookup"><span data-stu-id="b6f60-419">[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) - Gets values from route data.</span></span>
* <span data-ttu-id="b6f60-420">[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute): obtiene valores de los campos de formulario publicados.</span><span class="sxs-lookup"><span data-stu-id="b6f60-420">[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) - Gets values from posted form fields.</span></span>
* <span data-ttu-id="b6f60-421">[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute): obtiene valores del cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="b6f60-421">[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) - Gets values from the request body.</span></span>
* <span data-ttu-id="b6f60-422">[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute): obtiene valores de encabezados HTTP.</span><span class="sxs-lookup"><span data-stu-id="b6f60-422">[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) - Gets values from HTTP headers.</span></span>

<span data-ttu-id="b6f60-423">Estos atributos:</span><span class="sxs-lookup"><span data-stu-id="b6f60-423">These attributes:</span></span>

* <span data-ttu-id="b6f60-424">Se agregan de forma individual a las propiedades del modelo (no a la clase de modelo), como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="b6f60-424">Are added to model properties individually (not to the model class), as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/Instructor.cs?name=snippet_FromQuery&highlight=5-6)]

* <span data-ttu-id="b6f60-425">Opcionalmente, acepte un valor de nombre de modelo en el constructor.</span><span class="sxs-lookup"><span data-stu-id="b6f60-425">Optionally accept a model name value in the constructor.</span></span> <span data-ttu-id="b6f60-426">Esta opción se proporciona en caso de que el nombre de la propiedad no coincida con el valor de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="b6f60-426">This option is provided in case the property name doesn't match the value in the request.</span></span> <span data-ttu-id="b6f60-427">Por ejemplo, es posible que el valor de la solicitud sea un encabezado con un guion en el nombre, como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="b6f60-427">For instance, the value in the request might be a header with a hyphen in its name, as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_FromHeader)]

### <a name="frombody-attribute"></a><span data-ttu-id="b6f60-428">Atributo [FromBody]</span><span class="sxs-lookup"><span data-stu-id="b6f60-428">[FromBody] attribute</span></span>

<span data-ttu-id="b6f60-429">Aplique el atributo `[FromBody]` a un parámetro para rellenar sus propiedades desde el cuerpo de una solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="b6f60-429">Apply the `[FromBody]` attribute to a parameter to populate its properties from the body of an HTTP request.</span></span> <span data-ttu-id="b6f60-430">El runtime de ASP.NET Core delega la responsabilidad de leer el cuerpo al formateador de entrada.</span><span class="sxs-lookup"><span data-stu-id="b6f60-430">The ASP.NET Core runtime delegates the responsibility of reading the body to an input formatter.</span></span> <span data-ttu-id="b6f60-431">Los formateadores de entrada se explican [más adelante en este artículo](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="b6f60-431">Input formatters are explained [later in this article](#input-formatters).</span></span>

<span data-ttu-id="b6f60-432">Cuando se aplica `[FromBody]` a un parámetro de tipo complejo, se omiten los atributos de origen de enlace aplicados a sus propiedades.</span><span class="sxs-lookup"><span data-stu-id="b6f60-432">When `[FromBody]` is applied to a complex type parameter, any binding source attributes applied to its properties are ignored.</span></span> <span data-ttu-id="b6f60-433">Por ejemplo, la acción `Create` siguiente especifica que su parámetro `pet` se rellena a partir del cuerpo:</span><span class="sxs-lookup"><span data-stu-id="b6f60-433">For example, the following `Create` action specifies that its `pet` parameter is populated from the body:</span></span>

```csharp
public ActionResult<Pet> Create([FromBody] Pet pet)
```

<span data-ttu-id="b6f60-434">La clase `Pet` especifica que su propiedad `Breed` se rellena a partir de un parámetro de cadena de consulta:</span><span class="sxs-lookup"><span data-stu-id="b6f60-434">The `Pet` class specifies that its `Breed` property is populated from a query string parameter:</span></span>

```csharp
public class Pet
{
    public string Name { get; set; }

    [FromQuery] // Attribute is ignored.
    public string Breed { get; set; }
}
```

<span data-ttu-id="b6f60-435">En el ejemplo anterior:</span><span class="sxs-lookup"><span data-stu-id="b6f60-435">In the preceding example:</span></span>

* <span data-ttu-id="b6f60-436">El atributo `[FromQuery]` se ignora.</span><span class="sxs-lookup"><span data-stu-id="b6f60-436">The `[FromQuery]` attribute is ignored.</span></span>
* <span data-ttu-id="b6f60-437">La propiedad `Breed` no se rellena desde un parámetro de cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="b6f60-437">The `Breed` property is not populated from a query string parameter.</span></span> 

<span data-ttu-id="b6f60-438">Los formateadores de entrada solo leen el cuerpo y no entienden los atributos de origen de enlace.</span><span class="sxs-lookup"><span data-stu-id="b6f60-438">Input formatters read only the body and don't understand binding source attributes.</span></span> <span data-ttu-id="b6f60-439">Si se encuentra un valor adecuado en el cuerpo, ese valor se usa para rellenar la propiedad `Breed`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-439">If a suitable value is found in the body, that value is used to populate the `Breed` property.</span></span>

<span data-ttu-id="b6f60-440">No aplique `[FromBody]` a más de un parámetro por método de acción.</span><span class="sxs-lookup"><span data-stu-id="b6f60-440">Don't apply `[FromBody]` to more than one parameter per action method.</span></span> <span data-ttu-id="b6f60-441">Una vez que un formateador de entrada ha leído la secuencia de solicitudes, deja de estar disponible para una nueva lectura con el fin de enlazar otros parámetros `[FromBody]`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-441">Once the request stream is read by an input formatter, it's no longer available to be read again for binding other `[FromBody]` parameters.</span></span>

### <a name="additional-sources"></a><span data-ttu-id="b6f60-442">Orígenes adicionales</span><span class="sxs-lookup"><span data-stu-id="b6f60-442">Additional sources</span></span>

<span data-ttu-id="b6f60-443">Los *proveedores de valores* proporcionan datos de origen al sistema de enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="b6f60-443">Source data is provided to the model binding system by *value providers*.</span></span> <span data-ttu-id="b6f60-444">Puede escribir y registrar proveedores de valores personalizados que obtienen datos de otros orígenes para el enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="b6f60-444">You can write and register custom value providers that get data for model binding from other sources.</span></span> <span data-ttu-id="b6f60-445">Por ejemplo, es posible que le interesen datos de cookies o del estado de sesión.</span><span class="sxs-lookup"><span data-stu-id="b6f60-445">For example, you might want data from cookies or session state.</span></span> <span data-ttu-id="b6f60-446">Para obtener datos desde un origen nuevo:</span><span class="sxs-lookup"><span data-stu-id="b6f60-446">To get data from a new source:</span></span>

* <span data-ttu-id="b6f60-447">Cree una clase que implemente `IValueProvider`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-447">Create a class that implements `IValueProvider`.</span></span>
* <span data-ttu-id="b6f60-448">Cree una clase que implemente `IValueProviderFactory`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-448">Create a class that implements `IValueProviderFactory`.</span></span>
* <span data-ttu-id="b6f60-449">Registre la clase de generador en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-449">Register the factory class in `Startup.ConfigureServices`.</span></span>

<span data-ttu-id="b6f60-450">En la aplicación de ejemplo se incluye un [proveedor de valores](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProvider.cs) y un [generador](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProviderFactory.cs) que obtiene valores de cookies.</span><span class="sxs-lookup"><span data-stu-id="b6f60-450">The sample app includes a [value provider](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProvider.cs) and [factory](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProviderFactory.cs) example that gets values from cookies.</span></span> <span data-ttu-id="b6f60-451">Este es el código de registro de `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b6f60-451">Here's the registration code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=3)]

<span data-ttu-id="b6f60-452">En el código mostrado, el proveedor de valor personalizado se coloca después de todos los proveedores de valor integrados.</span><span class="sxs-lookup"><span data-stu-id="b6f60-452">The code shown puts the custom value provider after all the built-in value providers.</span></span>  <span data-ttu-id="b6f60-453">Para que sea el primero en la lista, llame a `Insert(0, new CookieValueProviderFactory())` en lugar de a `Add`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-453">To make it the first in the list, call `Insert(0, new CookieValueProviderFactory())` instead of `Add`.</span></span>

## <a name="no-source-for-a-model-property"></a><span data-ttu-id="b6f60-454">No hay origen para una propiedad de modelo</span><span class="sxs-lookup"><span data-stu-id="b6f60-454">No source for a model property</span></span>

<span data-ttu-id="b6f60-455">De forma predeterminada, si no se encuentra ningún valor para una propiedad de modelo no se crea un error de estado del modelo.</span><span class="sxs-lookup"><span data-stu-id="b6f60-455">By default, a model state error isn't created if no value is found for a model property.</span></span> <span data-ttu-id="b6f60-456">La propiedad se establece en NULL o en un valor predeterminado:</span><span class="sxs-lookup"><span data-stu-id="b6f60-456">The property is set to null or a default value:</span></span>

* <span data-ttu-id="b6f60-457">Los tipos simples que aceptan valores NULL se establecen en `null`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-457">Nullable simple types are set to `null`.</span></span>
* <span data-ttu-id="b6f60-458">Los tipos de valor que no aceptan valores NULL se establecen en `default(T)`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-458">Non-nullable value types are set to `default(T)`.</span></span> <span data-ttu-id="b6f60-459">Por ejemplo, un parámetro `int id` se establece en 0.</span><span class="sxs-lookup"><span data-stu-id="b6f60-459">For example, a parameter `int id` is set to 0.</span></span>
* <span data-ttu-id="b6f60-460">Para los tipos complejos, el enlace de modelos crea una instancia mediante el constructor predeterminado, sin establecer propiedades.</span><span class="sxs-lookup"><span data-stu-id="b6f60-460">For complex Types, model binding creates an instance by using the default constructor, without setting properties.</span></span>
* <span data-ttu-id="b6f60-461">Las matrices se establecen en `Array.Empty<T>()`, salvo las matrices `byte[]`, que se establecen en `null`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-461">Arrays are set to `Array.Empty<T>()`, except that `byte[]` arrays are set to `null`.</span></span>

<span data-ttu-id="b6f60-462">Si el estado del modelo se debe invalidar cuando no se encuentra nada en los campos de formulario para una propiedad de modelo, use el atributo [`[BindRequired]`](#bindrequired-attribute).</span><span class="sxs-lookup"><span data-stu-id="b6f60-462">If model state should be invalidated when nothing is found in form fields for a model property, use the [`[BindRequired]`](#bindrequired-attribute) attribute.</span></span>

<span data-ttu-id="b6f60-463">Tenga en cuenta que este comportamiento de `[BindRequired]` se aplica al enlace de modelos desde datos de formulario publicados, no a los datos JSON o XML del cuerpo de una solicitud.</span><span class="sxs-lookup"><span data-stu-id="b6f60-463">Note that this `[BindRequired]` behavior applies to model binding from posted form data, not to JSON or XML data in a request body.</span></span> <span data-ttu-id="b6f60-464">Los datos del cuerpo de la solicitud se controlan mediante [formateadores de entrada](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="b6f60-464">Request body data is handled by [input formatters](#input-formatters).</span></span>

## <a name="type-conversion-errors"></a><span data-ttu-id="b6f60-465">Errores de la conversión de tipos</span><span class="sxs-lookup"><span data-stu-id="b6f60-465">Type conversion errors</span></span>

<span data-ttu-id="b6f60-466">Si se encuentra un origen pero no se puede convertir al tipo de destino, el estado del modelo se marca como no válido.</span><span class="sxs-lookup"><span data-stu-id="b6f60-466">If a source is found but can't be converted into the target type, model state is flagged as invalid.</span></span> <span data-ttu-id="b6f60-467">El parámetro o la propiedad de destino se establece en NULL o en un valor predeterminado, como se ha indicado en la sección anterior.</span><span class="sxs-lookup"><span data-stu-id="b6f60-467">The target parameter or property is set to null or a default value, as noted in the previous section.</span></span>

<span data-ttu-id="b6f60-468">En un controlador de API que tenga el atributo `[ApiController]`, el estado de modelo no válido genera una respuesta HTTP 400 automática.</span><span class="sxs-lookup"><span data-stu-id="b6f60-468">In an API controller that has the `[ApiController]` attribute, invalid model state results in an automatic HTTP 400 response.</span></span>

<span data-ttu-id="b6f60-469">En una página de Razor, se vuelve a mostrar la página con un mensaje de error:</span><span class="sxs-lookup"><span data-stu-id="b6f60-469">In a Razor page, redisplay the page with an error message:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_HandleMBError&highlight=3-6)]

<span data-ttu-id="b6f60-470">La validación del lado cliente detecta la mayoría de los datos incorrectos que en otros casos se enviarían a un formulario de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="b6f60-470">Client-side validation catches most bad data that would otherwise be submitted to a Razor Pages form.</span></span> <span data-ttu-id="b6f60-471">Esta validación dificulta que se pueda desencadenar el código resaltado anterior.</span><span class="sxs-lookup"><span data-stu-id="b6f60-471">This validation makes it hard to trigger the preceding highlighted code.</span></span> <span data-ttu-id="b6f60-472">En la aplicación de ejemplo se incluye un botón **Submit with Invalid Date** (Enviar con fecha no válida) que agrega datos incorrectos al campo **Hire Date** (Fecha de contratación) y envía el formulario.</span><span class="sxs-lookup"><span data-stu-id="b6f60-472">The sample app includes a **Submit with Invalid Date** button that puts bad data in the **Hire Date** field and submits the form.</span></span> <span data-ttu-id="b6f60-473">Este botón muestra cómo funciona el código para volver a mostrar la página cuando se producen errores de conversión de datos.</span><span class="sxs-lookup"><span data-stu-id="b6f60-473">This button shows how the code for redisplaying the page works when data conversion errors occur.</span></span>

<span data-ttu-id="b6f60-474">Cuando el código anterior vuelve a mostrar la página, no se muestra la entrada no válida en el campo de formulario.</span><span class="sxs-lookup"><span data-stu-id="b6f60-474">When the page is redisplayed by the preceding code, the invalid input is not shown in the form field.</span></span> <span data-ttu-id="b6f60-475">El motivo es que la propiedad de modelo se ha establecido en NULL o en un valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="b6f60-475">This is because the model property has been set to null or a default value.</span></span> <span data-ttu-id="b6f60-476">La entrada no válida sí aparece en un mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="b6f60-476">The invalid input does appear in an error message.</span></span> <span data-ttu-id="b6f60-477">Pero si quiere volver a mostrar los datos incorrectos en el campo de formulario, considere la posibilidad de convertir la propiedad de modelo en una cadena y realizar la conversión de datos de forma manual.</span><span class="sxs-lookup"><span data-stu-id="b6f60-477">But if you want to redisplay the bad data in the form field, consider making the model property a string and doing the data conversion manually.</span></span>

<span data-ttu-id="b6f60-478">Se recomienda la misma estrategia si no quiere que los errores de conversión de tipo generen errores de estado de modelo.</span><span class="sxs-lookup"><span data-stu-id="b6f60-478">The same strategy is recommended if you don't want type conversion errors to result in model state errors.</span></span> <span data-ttu-id="b6f60-479">En ese caso, convierta la propiedad de modelo en una cadena.</span><span class="sxs-lookup"><span data-stu-id="b6f60-479">In that case, make the model property a string.</span></span>

## <a name="simple-types"></a><span data-ttu-id="b6f60-480">Tipos simples</span><span class="sxs-lookup"><span data-stu-id="b6f60-480">Simple types</span></span>

<span data-ttu-id="b6f60-481">Los tipos simples a los que el enlazador de modelos puede convertir las cadenas de origen incluyen los siguientes:</span><span class="sxs-lookup"><span data-stu-id="b6f60-481">The simple types that the model binder can convert source strings into include the following:</span></span>

* [<span data-ttu-id="b6f60-482">Boolean</span><span class="sxs-lookup"><span data-stu-id="b6f60-482">Boolean</span></span>](xref:System.ComponentModel.BooleanConverter)
* <span data-ttu-id="b6f60-483">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span><span class="sxs-lookup"><span data-stu-id="b6f60-483">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span></span>
* [<span data-ttu-id="b6f60-484">Char</span><span class="sxs-lookup"><span data-stu-id="b6f60-484">Char</span></span>](xref:System.ComponentModel.CharConverter)
* [<span data-ttu-id="b6f60-485">DateTime</span><span class="sxs-lookup"><span data-stu-id="b6f60-485">DateTime</span></span>](xref:System.ComponentModel.DateTimeConverter)
* [<span data-ttu-id="b6f60-486">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="b6f60-486">DateTimeOffset</span></span>](xref:System.ComponentModel.DateTimeOffsetConverter)
* [<span data-ttu-id="b6f60-487">Decimal</span><span class="sxs-lookup"><span data-stu-id="b6f60-487">Decimal</span></span>](xref:System.ComponentModel.DecimalConverter)
* [<span data-ttu-id="b6f60-488">Double</span><span class="sxs-lookup"><span data-stu-id="b6f60-488">Double</span></span>](xref:System.ComponentModel.DoubleConverter)
* [<span data-ttu-id="b6f60-489">Enum</span><span class="sxs-lookup"><span data-stu-id="b6f60-489">Enum</span></span>](xref:System.ComponentModel.EnumConverter)
* [<span data-ttu-id="b6f60-490">Guid</span><span class="sxs-lookup"><span data-stu-id="b6f60-490">Guid</span></span>](xref:System.ComponentModel.GuidConverter)
* <span data-ttu-id="b6f60-491">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span><span class="sxs-lookup"><span data-stu-id="b6f60-491">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span></span>
* [<span data-ttu-id="b6f60-492">Single</span><span class="sxs-lookup"><span data-stu-id="b6f60-492">Single</span></span>](xref:System.ComponentModel.SingleConverter)
* [<span data-ttu-id="b6f60-493">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="b6f60-493">TimeSpan</span></span>](xref:System.ComponentModel.TimeSpanConverter)
* <span data-ttu-id="b6f60-494">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span><span class="sxs-lookup"><span data-stu-id="b6f60-494">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span></span>
* [<span data-ttu-id="b6f60-495">Uri</span><span class="sxs-lookup"><span data-stu-id="b6f60-495">Uri</span></span>](xref:System.UriTypeConverter)
* [<span data-ttu-id="b6f60-496">Versión</span><span class="sxs-lookup"><span data-stu-id="b6f60-496">Version</span></span>](xref:System.ComponentModel.VersionConverter)

## <a name="complex-types"></a><span data-ttu-id="b6f60-497">Tipos complejos</span><span class="sxs-lookup"><span data-stu-id="b6f60-497">Complex types</span></span>

<span data-ttu-id="b6f60-498">Un tipo complejo debe tener un constructor público predeterminado y propiedades grabables públicas para enlazar.</span><span class="sxs-lookup"><span data-stu-id="b6f60-498">A complex type must have a public default constructor and public writable properties to bind.</span></span> <span data-ttu-id="b6f60-499">Cuando se produce el enlace de modelos, se crea una instancia de la clase con el constructor predeterminado público.</span><span class="sxs-lookup"><span data-stu-id="b6f60-499">When model binding occurs, the class is instantiated using the public default constructor.</span></span> 

<span data-ttu-id="b6f60-500">Para cada propiedad del tipo complejo, el enlace de modelos busca entre los orígenes el patrón de nombre *prefijo.nombre_de_propiedad*.</span><span class="sxs-lookup"><span data-stu-id="b6f60-500">For each property of the complex type, model binding looks through the sources for the name pattern *prefix.property_name*.</span></span> <span data-ttu-id="b6f60-501">Si no se encuentra nada, solo busca *nombre_de_propiedad* sin el prefijo.</span><span class="sxs-lookup"><span data-stu-id="b6f60-501">If nothing is found, it looks for just *property_name* without the prefix.</span></span>

<span data-ttu-id="b6f60-502">Para el enlace a un parámetro, el prefijo es el nombre del parámetro.</span><span class="sxs-lookup"><span data-stu-id="b6f60-502">For binding to a parameter, the prefix is the parameter name.</span></span> <span data-ttu-id="b6f60-503">Para el enlace a una propiedad pública `PageModel`, el prefijo es el nombre de la propiedad pública.</span><span class="sxs-lookup"><span data-stu-id="b6f60-503">For binding to a `PageModel` public property, the prefix is the public property name.</span></span> <span data-ttu-id="b6f60-504">Algunos atributos tienen una propiedad `Prefix` que permite invalidar el uso predeterminado del nombre de parámetro o propiedad.</span><span class="sxs-lookup"><span data-stu-id="b6f60-504">Some attributes have a `Prefix` property that lets you override the default usage of parameter or property name.</span></span>

<span data-ttu-id="b6f60-505">Por ejemplo, imagine que el tipo complejo es la clase `Instructor` siguiente:</span><span class="sxs-lookup"><span data-stu-id="b6f60-505">For example, suppose the complex type is the following `Instructor` class:</span></span>

  ```csharp
  public class Instructor
  {
      public int ID { get; set; }
      public string LastName { get; set; }
      public string FirstName { get; set; }
  }
  ```

### <a name="prefix--parameter-name"></a><span data-ttu-id="b6f60-506">Prefijo = nombre del parámetro</span><span class="sxs-lookup"><span data-stu-id="b6f60-506">Prefix = parameter name</span></span>

<span data-ttu-id="b6f60-507">Si el modelo que se va a enlazar es un parámetro denominado `instructorToUpdate`:</span><span class="sxs-lookup"><span data-stu-id="b6f60-507">If the model to be bound is a parameter named `instructorToUpdate`:</span></span>

```csharp
public IActionResult OnPost(int? id, Instructor instructorToUpdate)
```

<span data-ttu-id="b6f60-508">El enlace de modelos se inicia mediante el examen de los orígenes para la clave `instructorToUpdate.ID`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-508">Model binding starts by looking through the sources for the key `instructorToUpdate.ID`.</span></span> <span data-ttu-id="b6f60-509">Si no se encuentra, busca `ID` sin un prefijo.</span><span class="sxs-lookup"><span data-stu-id="b6f60-509">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="prefix--property-name"></a><span data-ttu-id="b6f60-510">Prefijo = nombre de propiedad</span><span class="sxs-lookup"><span data-stu-id="b6f60-510">Prefix = property name</span></span>

<span data-ttu-id="b6f60-511">Si el modelo que se va a enlazar es una propiedad denominada `Instructor` del controlador o la clase `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="b6f60-511">If the model to be bound is a property named `Instructor` of the controller or `PageModel` class:</span></span>

```csharp
[BindProperty]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="b6f60-512">El enlace de modelos se inicia mediante el examen de los orígenes para la clave `Instructor.ID`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-512">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="b6f60-513">Si no se encuentra, busca `ID` sin un prefijo.</span><span class="sxs-lookup"><span data-stu-id="b6f60-513">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="custom-prefix"></a><span data-ttu-id="b6f60-514">Prefijo personalizado</span><span class="sxs-lookup"><span data-stu-id="b6f60-514">Custom prefix</span></span>

<span data-ttu-id="b6f60-515">Si el modelo que se va a enlazar es un parámetro denominado `instructorToUpdate` y un atributo `Bind`, especifica `Instructor` como el prefijo:</span><span class="sxs-lookup"><span data-stu-id="b6f60-515">If the model to be bound is a parameter named `instructorToUpdate` and a `Bind` attribute specifies `Instructor` as the prefix:</span></span>

```csharp
public IActionResult OnPost(
    int? id, [Bind(Prefix = "Instructor")] Instructor instructorToUpdate)
```

<span data-ttu-id="b6f60-516">El enlace de modelos se inicia mediante el examen de los orígenes para la clave `Instructor.ID`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-516">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="b6f60-517">Si no se encuentra, busca `ID` sin un prefijo.</span><span class="sxs-lookup"><span data-stu-id="b6f60-517">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="attributes-for-complex-type-targets"></a><span data-ttu-id="b6f60-518">Atributos para destinos de tipo complejo</span><span class="sxs-lookup"><span data-stu-id="b6f60-518">Attributes for complex type targets</span></span>

<span data-ttu-id="b6f60-519">Existen varios atributos integrados para controlar el enlace de modelos de tipos complejos:</span><span class="sxs-lookup"><span data-stu-id="b6f60-519">Several built-in attributes are available for controlling model binding of complex types:</span></span>

* `[BindRequired]`
* `[BindNever]`
* `[Bind]`

> [!NOTE]
> <span data-ttu-id="b6f60-520">Estos atributos afectan al enlace de modelos cuando el origen de los valores son datos de formulario publicados.</span><span class="sxs-lookup"><span data-stu-id="b6f60-520">These attributes affect model binding when posted form data is the source of values.</span></span> <span data-ttu-id="b6f60-521">No afectan a los formateadores de entrada, que procesan los cuerpos de la solicitud JSON y XML publicados.</span><span class="sxs-lookup"><span data-stu-id="b6f60-521">They do not affect input formatters, which process posted JSON and XML request bodies.</span></span> <span data-ttu-id="b6f60-522">Los formateadores de entrada se explican [más adelante en este artículo](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="b6f60-522">Input formatters are explained [later in this article](#input-formatters).</span></span>
>
> <span data-ttu-id="b6f60-523">Vea también la explicación del atributo `[Required]` en [Validación de modelos](xref:mvc/models/validation#required-attribute).</span><span class="sxs-lookup"><span data-stu-id="b6f60-523">See also the discussion of the `[Required]` attribute in [Model validation](xref:mvc/models/validation#required-attribute).</span></span>

### <a name="bindrequired-attribute"></a><span data-ttu-id="b6f60-524">Atributo [BindRequired]</span><span class="sxs-lookup"><span data-stu-id="b6f60-524">[BindRequired] attribute</span></span>

<span data-ttu-id="b6f60-525">Solo se puede aplicar a propiedades del modelo, no a parámetros de método.</span><span class="sxs-lookup"><span data-stu-id="b6f60-525">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="b6f60-526">Hace que el enlace de modelos agregue un error de estado de modelo si no se puede realizar el enlace para la propiedad de un modelo.</span><span class="sxs-lookup"><span data-stu-id="b6f60-526">Causes model binding to add a model state error if binding cannot occur for a model's property.</span></span> <span data-ttu-id="b6f60-527">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="b6f60-527">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/InstructorWithCollection.cs?name=snippet_BindRequired&highlight=8-9)]

### <a name="bindnever-attribute"></a><span data-ttu-id="b6f60-528">Atributo [BindNever]</span><span class="sxs-lookup"><span data-stu-id="b6f60-528">[BindNever] attribute</span></span>

<span data-ttu-id="b6f60-529">Solo se puede aplicar a propiedades del modelo, no a parámetros de método.</span><span class="sxs-lookup"><span data-stu-id="b6f60-529">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="b6f60-530">Impide que el enlace de modelos establezca la propiedad de un modelo.</span><span class="sxs-lookup"><span data-stu-id="b6f60-530">Prevents model binding from setting a model's property.</span></span> <span data-ttu-id="b6f60-531">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="b6f60-531">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/InstructorWithDictionary.cs?name=snippet_BindNever&highlight=3-4)]

### <a name="bind-attribute"></a><span data-ttu-id="b6f60-532">Atributo [Bind]</span><span class="sxs-lookup"><span data-stu-id="b6f60-532">[Bind] attribute</span></span>

<span data-ttu-id="b6f60-533">Se puede aplicar a una clase o un parámetro de método.</span><span class="sxs-lookup"><span data-stu-id="b6f60-533">Can be applied to a class or a method parameter.</span></span> <span data-ttu-id="b6f60-534">Especifica qué propiedades de un modelo se deben incluir en el enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="b6f60-534">Specifies which properties of a model should be included in model binding.</span></span>

<span data-ttu-id="b6f60-535">En el ejemplo siguiente, solo se enlazan las propiedades especificadas del modelo `Instructor` cuando se llama a cualquier método de acción o controlador:</span><span class="sxs-lookup"><span data-stu-id="b6f60-535">In the following example, only the specified properties of the `Instructor` model are bound when any handler or action method is called:</span></span>

```csharp
[Bind("LastName,FirstMidName,HireDate")]
public class Instructor
```

<span data-ttu-id="b6f60-536">En el ejemplo siguiente, solo se enlazan las propiedades especificadas del modelo `Instructor` cuando se llama al método `OnPost`:</span><span class="sxs-lookup"><span data-stu-id="b6f60-536">In the following example, only the specified properties of the `Instructor` model are bound when the `OnPost` method is called:</span></span>

```csharp
[HttpPost]
public IActionResult OnPost([Bind("LastName,FirstMidName,HireDate")] Instructor instructor)
```

<span data-ttu-id="b6f60-537">El atributo `[Bind]` se puede usar para protegerse de la publicación excesiva en escenarios de *creación*.</span><span class="sxs-lookup"><span data-stu-id="b6f60-537">The `[Bind]` attribute can be used to protect against overposting in *create* scenarios.</span></span> <span data-ttu-id="b6f60-538">No funciona bien en escenarios de edición porque las propiedades excluidas se establecen en NULL o en un valor predeterminado en lugar de mantenerse sin cambios.</span><span class="sxs-lookup"><span data-stu-id="b6f60-538">It doesn't work well in edit scenarios because excluded properties are set to null or a default value instead of being left unchanged.</span></span> <span data-ttu-id="b6f60-539">Para defenderse de la publicación excesiva, se recomiendan modelos de vista en lugar del atributo `[Bind]`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-539">For defense against overposting, view models are recommended rather than the `[Bind]` attribute.</span></span> <span data-ttu-id="b6f60-540">Para más información, vea [Nota de seguridad sobre la publicación excesiva](xref:data/ef-mvc/crud#security-note-about-overposting).</span><span class="sxs-lookup"><span data-stu-id="b6f60-540">For more information, see [Security note about overposting](xref:data/ef-mvc/crud#security-note-about-overposting).</span></span>

## <a name="collections"></a><span data-ttu-id="b6f60-541">Colecciones</span><span class="sxs-lookup"><span data-stu-id="b6f60-541">Collections</span></span>

<span data-ttu-id="b6f60-542">Para los destinos que son colecciones de tipos simples, el enlace de modelos busca coincidencias con *nombre_de_parámetro* o *nombre_de_propiedad*.</span><span class="sxs-lookup"><span data-stu-id="b6f60-542">For targets that are collections of simple types, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="b6f60-543">Si no se encuentra ninguna coincidencia, busca uno de los formatos admitidos sin el prefijo.</span><span class="sxs-lookup"><span data-stu-id="b6f60-543">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="b6f60-544">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="b6f60-544">For example:</span></span>

* <span data-ttu-id="b6f60-545">Imagine que el parámetro que se va a enlazar es una matriz llamada `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="b6f60-545">Suppose the parameter to be bound is an array named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, int[] selectedCourses)
  ```

* <span data-ttu-id="b6f60-546">Los datos de cadena de consulta o formulario pueden estar en uno de los formatos siguientes:</span><span class="sxs-lookup"><span data-stu-id="b6f60-546">Form or query string data can be in one of the following formats:</span></span>
   
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

* <span data-ttu-id="b6f60-547">El formato siguiente solo se admite en datos de formulario:</span><span class="sxs-lookup"><span data-stu-id="b6f60-547">The following format is supported only in form data:</span></span>

  ```
  selectedCourses[]=1050&selectedCourses[]=2000
  ```

* <span data-ttu-id="b6f60-548">Para todos los formatos de ejemplo anteriores, el enlace de modelos pasa una matriz de dos elementos al parámetro `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="b6f60-548">For all of the preceding example formats, model binding passes an array of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="b6f60-549">selectedCourses[0]=1050</span><span class="sxs-lookup"><span data-stu-id="b6f60-549">selectedCourses[0]=1050</span></span>
  * <span data-ttu-id="b6f60-550">selectedCourses[1]=2000</span><span class="sxs-lookup"><span data-stu-id="b6f60-550">selectedCourses[1]=2000</span></span>

  <span data-ttu-id="b6f60-551">Los formatos de datos que usan números de subíndice (... [0] ... [1] ...) deben asegurarse de que se numeran de forma secuencial a partir de cero.</span><span class="sxs-lookup"><span data-stu-id="b6f60-551">Data formats that use subscript numbers (... [0] ... [1] ...) must ensure that they are numbered sequentially starting at zero.</span></span> <span data-ttu-id="b6f60-552">Si hay algún hueco en la numeración de los subíndices, se omiten todos los elementos que aparecen después del hueco.</span><span class="sxs-lookup"><span data-stu-id="b6f60-552">If there are any gaps in subscript numbering, all items after the gap are ignored.</span></span> <span data-ttu-id="b6f60-553">Por ejemplo, si los subíndices son 0 y 2 en lugar de 0 y 1, se omite el segundo elemento.</span><span class="sxs-lookup"><span data-stu-id="b6f60-553">For example, if the subscripts are 0 and 2 instead of 0 and 1, the second item is ignored.</span></span>

## <a name="dictionaries"></a><span data-ttu-id="b6f60-554">Diccionarios</span><span class="sxs-lookup"><span data-stu-id="b6f60-554">Dictionaries</span></span>

<span data-ttu-id="b6f60-555">Para los destinos `Dictionary`, el enlace de modelos busca coincidencias con *nombre_de_parámetro* o *nombre_de_propiedad*.</span><span class="sxs-lookup"><span data-stu-id="b6f60-555">For `Dictionary` targets, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="b6f60-556">Si no se encuentra ninguna coincidencia, busca uno de los formatos admitidos sin el prefijo.</span><span class="sxs-lookup"><span data-stu-id="b6f60-556">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="b6f60-557">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="b6f60-557">For example:</span></span>

* <span data-ttu-id="b6f60-558">Imagine que el parámetro de destino es un elemento `Dictionary<int, string>` denominado `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="b6f60-558">Suppose the target parameter is a `Dictionary<int, string>` named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, Dictionary<int, string> selectedCourses)
  ```

* <span data-ttu-id="b6f60-559">Los datos de cadena de consulta o de formulario publicados pueden ser similares a uno de los ejemplos siguientes:</span><span class="sxs-lookup"><span data-stu-id="b6f60-559">The posted form or query string data can look like one of the following examples:</span></span>

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

* <span data-ttu-id="b6f60-560">Para todos los formatos de ejemplo anteriores, el enlace de modelos pasa un diccionario de dos elementos al parámetro `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="b6f60-560">For all of the preceding example formats, model binding passes a dictionary of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="b6f60-561">selectedCourses["1050"]="Chemistry"</span><span class="sxs-lookup"><span data-stu-id="b6f60-561">selectedCourses["1050"]="Chemistry"</span></span>
  * <span data-ttu-id="b6f60-562">selectedCourses["2000"]="Economics"</span><span class="sxs-lookup"><span data-stu-id="b6f60-562">selectedCourses["2000"]="Economics"</span></span>

<a name="glob"></a>

## <a name="globalization-behavior-of-model-binding-route-data-and-query-strings"></a><span data-ttu-id="b6f60-563">Comportamiento de globalización del enlace de modelos datos de ruta y cadenas de consulta</span><span class="sxs-lookup"><span data-stu-id="b6f60-563">Globalization behavior of model binding route data and query strings</span></span>

<span data-ttu-id="b6f60-564">El proveedor de valores de ruta de ASP.NET Core y el proveedor de valores de cadena de consulta:</span><span class="sxs-lookup"><span data-stu-id="b6f60-564">The ASP.NET Core route value provider and query string value provider:</span></span>

* <span data-ttu-id="b6f60-565">Tratan los valores como referencia cultural de todos los idiomas.</span><span class="sxs-lookup"><span data-stu-id="b6f60-565">Treat values as invariant culture.</span></span>
* <span data-ttu-id="b6f60-566">Esperan que las direcciones URL sean independientes de la referencia cultural.</span><span class="sxs-lookup"><span data-stu-id="b6f60-566">Expect that URLs are culture-invariant.</span></span>

<span data-ttu-id="b6f60-567">Por el contrario, los valores procedentes de datos de formulario se someten a una conversión que tiene en cuenta la referencia cultural.</span><span class="sxs-lookup"><span data-stu-id="b6f60-567">In contrast, values coming from form data undergo a culture-sensitive conversion.</span></span> <span data-ttu-id="b6f60-568">Esto es así por diseño, para que las direcciones URL se puedan compartir entre configuraciones regionales.</span><span class="sxs-lookup"><span data-stu-id="b6f60-568">This is by design so that URLs are shareable across locales.</span></span>

<span data-ttu-id="b6f60-569">Para que el proveedor de valores de ruta de ASP.NET Core y el proveedor de valores de cadena de consulta se sometan a una conversión dependiente de la referencia cultural:</span><span class="sxs-lookup"><span data-stu-id="b6f60-569">To make the ASP.NET Core route value provider and query string value provider undergo a culture-sensitive conversion:</span></span>

* <span data-ttu-id="b6f60-570">Heredan de <xref:Microsoft.AspNetCore.Mvc.ModelBinding.IValueProviderFactory>.</span><span class="sxs-lookup"><span data-stu-id="b6f60-570">Inherit from <xref:Microsoft.AspNetCore.Mvc.ModelBinding.IValueProviderFactory></span></span>
* <span data-ttu-id="b6f60-571">Copie el código de [QueryStringValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs) o [RouteValueValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/RouteValueProviderFactory.cs)</span><span class="sxs-lookup"><span data-stu-id="b6f60-571">Copy the code from [QueryStringValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs) or [RouteValueValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/RouteValueProviderFactory.cs)</span></span>
* <span data-ttu-id="b6f60-572">Reemplace el [valor de referencia cultural](https://github.com/aspnet/AspNetCore/blob/e625fe29b049c60242e8048b4ea743cca65aa7b5/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs#L30) pasado al constructor de proveedor de valores con [CultureInfo.CurrentCulture](xref:System.Globalization.CultureInfo.CurrentCulture)</span><span class="sxs-lookup"><span data-stu-id="b6f60-572">Replace the [culture value](https://github.com/aspnet/AspNetCore/blob/e625fe29b049c60242e8048b4ea743cca65aa7b5/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs#L30) passed to the value provider constructor with [CultureInfo.CurrentCulture](xref:System.Globalization.CultureInfo.CurrentCulture)</span></span>
* <span data-ttu-id="b6f60-573">Reemplace el generador de proveedores de valor predeterminado en las opciones de MVC por el nuevo:</span><span class="sxs-lookup"><span data-stu-id="b6f60-573">Replace the default value provider factory in MVC options with your new one:</span></span>

[!code-csharp[](model-binding/samples_snapshot/2.x/Startup.cs?name=snippet)]
[!code-csharp[](model-binding/samples_snapshot/2.x/Startup.cs?name=snippet1)]

## <a name="special-data-types"></a><span data-ttu-id="b6f60-574">Tipos de datos especiales</span><span class="sxs-lookup"><span data-stu-id="b6f60-574">Special data types</span></span>

<span data-ttu-id="b6f60-575">Hay algunos tipos de datos especiales que el enlace de modelos puede controlar.</span><span class="sxs-lookup"><span data-stu-id="b6f60-575">There are some special data types that model binding can handle.</span></span>

### <a name="iformfile-and-iformfilecollection"></a><span data-ttu-id="b6f60-576">IFormFile e IFormFileCollection</span><span class="sxs-lookup"><span data-stu-id="b6f60-576">IFormFile and IFormFileCollection</span></span>

<span data-ttu-id="b6f60-577">Un archivo cargado incluido en la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="b6f60-577">An uploaded file included in the HTTP request.</span></span>  <span data-ttu-id="b6f60-578">También se admite `IEnumerable<IFormFile>` para varios archivos.</span><span class="sxs-lookup"><span data-stu-id="b6f60-578">Also supported is `IEnumerable<IFormFile>` for multiple files.</span></span>

### <a name="cancellationtoken"></a><span data-ttu-id="b6f60-579">CancellationToken</span><span class="sxs-lookup"><span data-stu-id="b6f60-579">CancellationToken</span></span>

<span data-ttu-id="b6f60-580">se usa para cancelar la actividad en controladores asincrónicos.</span><span class="sxs-lookup"><span data-stu-id="b6f60-580">Used to cancel activity in asynchronous controllers.</span></span>

### <a name="formcollection"></a><span data-ttu-id="b6f60-581">FormCollection</span><span class="sxs-lookup"><span data-stu-id="b6f60-581">FormCollection</span></span>

<span data-ttu-id="b6f60-582">Se usa para recuperar todos los valores de los datos de formulario publicados.</span><span class="sxs-lookup"><span data-stu-id="b6f60-582">Used to retrieve all the values from posted form data.</span></span>

## <a name="input-formatters"></a><span data-ttu-id="b6f60-583">Formateadores de entrada</span><span class="sxs-lookup"><span data-stu-id="b6f60-583">Input formatters</span></span>

<span data-ttu-id="b6f60-584">Los datos del cuerpo de la solicitud pueden estar en XML, JSON u otro formato.</span><span class="sxs-lookup"><span data-stu-id="b6f60-584">Data in the request body can be in JSON, XML, or some other format.</span></span> <span data-ttu-id="b6f60-585">Para analizar estos datos, el enlace de modelos usa un *formateador de entrada* configurado para controlar un tipo de contenido determinado.</span><span class="sxs-lookup"><span data-stu-id="b6f60-585">To parse this data, model binding uses an *input formatter* that is configured to handle a particular content type.</span></span> <span data-ttu-id="b6f60-586">De forma predeterminada, en ASP.NET Core se incluyen formateadores de entrada basados en JSON para el control de los datos JSON.</span><span class="sxs-lookup"><span data-stu-id="b6f60-586">By default, ASP.NET Core includes JSON based input formatters for handling JSON data.</span></span> <span data-ttu-id="b6f60-587">Puede agregar otros formateadores para otros tipos de contenido.</span><span class="sxs-lookup"><span data-stu-id="b6f60-587">You can add other formatters for other content types.</span></span>

<span data-ttu-id="b6f60-588">ASP.NET Core selecciona los formateadores de entrada en función del atributo [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute).</span><span class="sxs-lookup"><span data-stu-id="b6f60-588">ASP.NET Core selects input formatters based on the [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) attribute.</span></span> <span data-ttu-id="b6f60-589">Si no hay ningún atributo, usa el [encabezado Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span><span class="sxs-lookup"><span data-stu-id="b6f60-589">If no attribute is present, it uses the [Content-Type header](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span></span>

<span data-ttu-id="b6f60-590">Para usar los formateadores de entrada XML integrados:</span><span class="sxs-lookup"><span data-stu-id="b6f60-590">To use the built-in XML input formatters:</span></span>

* <span data-ttu-id="b6f60-591">Instale el paquete NuGet `Microsoft.AspNetCore.Mvc.Formatters.Xml`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-591">Install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

* <span data-ttu-id="b6f60-592">En `Startup.ConfigureServices`, llame a <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> o a <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span><span class="sxs-lookup"><span data-stu-id="b6f60-592">In `Startup.ConfigureServices`, call <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> or <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span></span>

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=9)]

* <span data-ttu-id="b6f60-593">Aplique el atributo `Consumes` a las clases de controlador o los métodos de acción que deben esperar XML en el cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="b6f60-593">Apply the `Consumes` attribute to controller classes or action methods that should expect XML in the request body.</span></span>

  ```csharp
  [HttpPost]
  [Consumes("application/xml")]
  public ActionResult<Pet> Create(Pet pet)
  ```

  <span data-ttu-id="b6f60-594">Para más información, vea [Introducción de la serialización XML](/dotnet/standard/serialization/introducing-xml-serialization).</span><span class="sxs-lookup"><span data-stu-id="b6f60-594">For more information, see [Introducing XML Serialization](/dotnet/standard/serialization/introducing-xml-serialization).</span></span>

## <a name="exclude-specified-types-from-model-binding"></a><span data-ttu-id="b6f60-595">Exclusión de tipos especificados del enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="b6f60-595">Exclude specified types from model binding</span></span>

<span data-ttu-id="b6f60-596">El comportamiento del enlace de modelos y los sistemas de validación se controla mediante [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span><span class="sxs-lookup"><span data-stu-id="b6f60-596">The model binding and validation systems' behavior is driven by [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span></span> <span data-ttu-id="b6f60-597">Puede personalizar `ModelMetadata` mediante la adición de un proveedor de detalles a [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span><span class="sxs-lookup"><span data-stu-id="b6f60-597">You can customize `ModelMetadata` by adding a details provider to [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span></span> <span data-ttu-id="b6f60-598">Los proveedores de detalles integrados están disponibles para deshabilitar el enlace de modelos o la validación para tipos especificados.</span><span class="sxs-lookup"><span data-stu-id="b6f60-598">Built-in details providers are available for disabling model binding or validation for specified types.</span></span>

<span data-ttu-id="b6f60-599">Para deshabilitar el enlace de modelos en todos los modelos de un tipo especificado, agregue una instancia de <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-599">To disable model binding on all models of a specified type, add an <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b6f60-600">Por ejemplo, para deshabilitar el enlace de modelos en todos los modelos del tipo `System.Version`:</span><span class="sxs-lookup"><span data-stu-id="b6f60-600">For example, to disable model binding on all models of type `System.Version`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=4-5)]

<span data-ttu-id="b6f60-601">Para deshabilitar la validación en propiedades de un tipo especificado, agregue una instancia de <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-601">To disable validation on properties of a specified type, add a <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b6f60-602">Por ejemplo, para deshabilitar la validación en las propiedades de tipo `System.Guid`:</span><span class="sxs-lookup"><span data-stu-id="b6f60-602">For example, to disable validation on properties of type `System.Guid`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=6-7)]

## <a name="custom-model-binders"></a><span data-ttu-id="b6f60-603">Enlazadores de modelos personalizados</span><span class="sxs-lookup"><span data-stu-id="b6f60-603">Custom model binders</span></span>

<span data-ttu-id="b6f60-604">Puede ampliar el enlace de modelos si escribe un enlazador de modelos personalizado y usa el atributo `[ModelBinder]` para seleccionarlo para un destino concreto.</span><span class="sxs-lookup"><span data-stu-id="b6f60-604">You can extend model binding by writing a custom model binder and using the `[ModelBinder]` attribute to select it for a given target.</span></span> <span data-ttu-id="b6f60-605">Más información sobre el [enlace de modelos personalizados](xref:mvc/advanced/custom-model-binding).</span><span class="sxs-lookup"><span data-stu-id="b6f60-605">Learn more about [custom model binding](xref:mvc/advanced/custom-model-binding).</span></span>

## <a name="manual-model-binding"></a><span data-ttu-id="b6f60-606">Enlace de modelos manual</span><span class="sxs-lookup"><span data-stu-id="b6f60-606">Manual model binding</span></span>

<span data-ttu-id="b6f60-607">El enlace de modelos se puede invocar de forma manual mediante el método <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>.</span><span class="sxs-lookup"><span data-stu-id="b6f60-607">Model binding can be invoked manually by using the <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> method.</span></span> <span data-ttu-id="b6f60-608">El método se define en las clases `ControllerBase` y `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="b6f60-608">The method is defined on both `ControllerBase` and `PageModel` classes.</span></span> <span data-ttu-id="b6f60-609">Las sobrecargas de método permiten especificar el prefijo y el proveedor de valores que se van a usar.</span><span class="sxs-lookup"><span data-stu-id="b6f60-609">Method overloads let you specify the prefix and value provider to use.</span></span> <span data-ttu-id="b6f60-610">El método devuelve `false` si se produce un error en el enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="b6f60-610">The method returns `false` if model binding fails.</span></span> <span data-ttu-id="b6f60-611">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="b6f60-611">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/InstructorsWithCollection/Create.cshtml.cs?name=snippet_TryUpdate&highlight=1-4)]

## <a name="fromservices-attribute"></a><span data-ttu-id="b6f60-612">Atributo [FromServices]</span><span class="sxs-lookup"><span data-stu-id="b6f60-612">[FromServices] attribute</span></span>

<span data-ttu-id="b6f60-613">El nombre de este atributo sigue el patrón de los atributos de enlace de modelos que especifican un origen de datos.</span><span class="sxs-lookup"><span data-stu-id="b6f60-613">This attribute's name follows the pattern of model binding attributes that specify a data source.</span></span> <span data-ttu-id="b6f60-614">Pero no se trata de enlazar datos desde un proveedor de valores.</span><span class="sxs-lookup"><span data-stu-id="b6f60-614">But it's not about binding data from a value provider.</span></span> <span data-ttu-id="b6f60-615">Obtiene una instancia de un tipo desde el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b6f60-615">It gets an instance of a type from the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="b6f60-616">Su objetivo es proporcionar una alternativa a la inserción de constructores cuando se necesita un servicio solo si se llama a un método concreto.</span><span class="sxs-lookup"><span data-stu-id="b6f60-616">Its purpose is to provide an alternative to constructor injection for when you need a service only if a particular method is called.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b6f60-617">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="b6f60-617">Additional resources</span></span>

* <xref:mvc/models/validation>
* <xref:mvc/advanced/custom-model-binding>

::: moniker-end
