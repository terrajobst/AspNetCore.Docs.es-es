---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Parámetro de enlace en ASP.NET Web API | Documentos de Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2013
ms.topic: article
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 5aa532137436922519c86246ebfa834910ac0d86
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="parameter-binding-in-aspnet-web-api"></a><span data-ttu-id="b379b-102">Parámetro de enlace en ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="b379b-102">Parameter Binding in ASP.NET Web API</span></span>
====================
<span data-ttu-id="b379b-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b379b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="b379b-104">Al API Web llama a un método en un controlador, se deben establecer valores para los parámetros, un proceso denominado *enlace*.</span><span class="sxs-lookup"><span data-stu-id="b379b-104">When Web API calls a method on a controller, it must set values for the parameters, a process called *binding*.</span></span> <span data-ttu-id="b379b-105">Este artículo describe cómo API Web enlaza los parámetros y cómo puede personalizar el proceso de enlace.</span><span class="sxs-lookup"><span data-stu-id="b379b-105">This article describes how Web API binds parameters, and how you can customize the binding process.</span></span>

<span data-ttu-id="b379b-106">De forma predeterminada, API Web utiliza las siguientes reglas para enlazar parámetros:</span><span class="sxs-lookup"><span data-stu-id="b379b-106">By default, Web API uses the following rules to bind parameters:</span></span>

- <span data-ttu-id="b379b-107">Si el parámetro es un tipo "simple", API Web intenta obtener el valor del identificador URI.</span><span class="sxs-lookup"><span data-stu-id="b379b-107">If the parameter is a "simple" type, Web API tries to get the value from the URI.</span></span> <span data-ttu-id="b379b-108">Los tipos simples incluyen .NET [tipos primitivos](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **doble**, etc.), además de **TimeSpan**, **DateTime**, **Guid**, **decimal**, y **cadena**, *más* cualquier tipo con un convertidor de tipos que puede convertir una cadena.</span><span class="sxs-lookup"><span data-stu-id="b379b-108">Simple types include the .NET [primitive types](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, and so forth), plus **TimeSpan**, **DateTime**, **Guid**, **decimal**, and **string**, *plus* any type with a type converter that can convert from a string.</span></span> <span data-ttu-id="b379b-109">(Más información acerca de los convertidores de tipos más adelante).</span><span class="sxs-lookup"><span data-stu-id="b379b-109">(More about type converters later.)</span></span>
- <span data-ttu-id="b379b-110">Para los tipos complejos, API Web intenta leer el valor del cuerpo del mensaje, pero utiliza un [formateador de tipo de medio](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="b379b-110">For complex types, Web API tries to read the value from the message body, using a [media-type formatter](media-formatters.md).</span></span>

<span data-ttu-id="b379b-111">Por ejemplo, este es un método de controlador de API Web típico:</span><span class="sxs-lookup"><span data-stu-id="b379b-111">For example, here is a typical Web API controller method:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="b379b-112">El *identificador* parámetro es un &quot;simple&quot; escriba, por lo que la API Web intenta obtener el valor de URI de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="b379b-112">The *id* parameter is a &quot;simple&quot; type, so Web API tries to get the value from the request URI.</span></span> <span data-ttu-id="b379b-113">El *elemento* parámetro es un tipo complejo, por lo que la API Web utiliza un formateador de tipo de medio para leer el valor desde el cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="b379b-113">The *item* parameter is a complex type, so Web API uses a media-type formatter to read the value from the request body.</span></span>

<span data-ttu-id="b379b-114">Para obtener un valor del identificador URI, API Web busca en los datos de ruta y la cadena de consulta URI.</span><span class="sxs-lookup"><span data-stu-id="b379b-114">To get a value from the URI, Web API looks in the route data and the URI query string.</span></span> <span data-ttu-id="b379b-115">Los datos de ruta se rellenan cuando el sistema de enrutamiento analiza el identificador URI y coincide con una ruta.</span><span class="sxs-lookup"><span data-stu-id="b379b-115">The route data is populated when the routing system parses the URI and matches it to a route.</span></span> <span data-ttu-id="b379b-116">Para obtener más información, consulte [enrutamiento y selección de acción](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="b379b-116">For more information, see [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span>

<span data-ttu-id="b379b-117">En el resto de este artículo, voy a explicar cómo personalizar el proceso de enlace de modelo.</span><span class="sxs-lookup"><span data-stu-id="b379b-117">In the rest of this article, I'll show how you can customize the model binding process.</span></span> <span data-ttu-id="b379b-118">Para los tipos complejos, sin embargo, considere el uso de formateadores de tipo de medio siempre que sea posible.</span><span class="sxs-lookup"><span data-stu-id="b379b-118">For complex types, however, consider using media-type formatters whenever possible.</span></span> <span data-ttu-id="b379b-119">Un principio clave de HTTP es que los recursos se envían en el cuerpo del mensaje, utilizando la negociación de contenido para especificar la representación del recurso.</span><span class="sxs-lookup"><span data-stu-id="b379b-119">A key principle of HTTP is that resources are sent in the message body, using content negotiation to specify the representation of the resource.</span></span> <span data-ttu-id="b379b-120">Formateadores de tipo de medio se diseñaron exactamente para este propósito.</span><span class="sxs-lookup"><span data-stu-id="b379b-120">Media-type formatters were designed for exactly this purpose.</span></span>

## <a name="using-fromuri"></a><span data-ttu-id="b379b-121">Uso de [FromUri]</span><span class="sxs-lookup"><span data-stu-id="b379b-121">Using [FromUri]</span></span>

<span data-ttu-id="b379b-122">Para forzar la API Web para leer un tipo complejo del URI, agregue el **[FromUri]** para el parámetro de atributo.</span><span class="sxs-lookup"><span data-stu-id="b379b-122">To force Web API to read a complex type from the URI, add the **[FromUri]** attribute to the parameter.</span></span> <span data-ttu-id="b379b-123">En el ejemplo siguiente se define un `GeoPoint` tipo, junto con un método de controlador que obtiene el `GeoPoint` del URI.</span><span class="sxs-lookup"><span data-stu-id="b379b-123">The following example defines a `GeoPoint` type, along with a controller method that gets the `GeoPoint` from the URI.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="b379b-124">El cliente puede colocar los valores de latitud y longitud de la cadena de consulta y API Web usará para construir un `GeoPoint`.</span><span class="sxs-lookup"><span data-stu-id="b379b-124">The client can put the Latitude and Longitude values in the query string and Web API will use them to construct a `GeoPoint`.</span></span> <span data-ttu-id="b379b-125">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="b379b-125">For example:</span></span>

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a><span data-ttu-id="b379b-126">Uso de [FromBody]</span><span class="sxs-lookup"><span data-stu-id="b379b-126">Using [FromBody]</span></span>

<span data-ttu-id="b379b-127">Para forzar la API Web para leer un tipo simple desde el cuerpo de solicitud, agregue el **[FromBody]** para el parámetro de atributo:</span><span class="sxs-lookup"><span data-stu-id="b379b-127">To force Web API to read a simple type from the request body, add the **[FromBody]** attribute to the parameter:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="b379b-128">En este ejemplo, API Web usará un formateador de tipo de medio para leer el valor de *nombre* desde el cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="b379b-128">In this example, Web API will use a media-type formatter to read the value of *name* from the request body.</span></span> <span data-ttu-id="b379b-129">Esta es una solicitud de cliente de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="b379b-129">Here is an example client request.</span></span>

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

<span data-ttu-id="b379b-130">Cuando un parámetro tiene [FromBody], API Web utiliza el encabezado Content-Type para seleccionar a un formateador.</span><span class="sxs-lookup"><span data-stu-id="b379b-130">When a parameter has [FromBody], Web API uses the Content-Type header to select a formatter.</span></span> <span data-ttu-id="b379b-131">En este ejemplo, el tipo de contenido es &quot;application/json&quot; y el cuerpo de solicitud es una cadena JSON sin formato (no un objeto JSON).</span><span class="sxs-lookup"><span data-stu-id="b379b-131">In this example, the content type is &quot;application/json&quot; and the request body is a raw JSON string (not a JSON object).</span></span>

<span data-ttu-id="b379b-132">A lo sumo un parámetro tiene permiso para leer desde el cuerpo del mensaje.</span><span class="sxs-lookup"><span data-stu-id="b379b-132">At most one parameter is allowed to read from the message body.</span></span> <span data-ttu-id="b379b-133">Por lo que este procedimiento no funcionará:</span><span class="sxs-lookup"><span data-stu-id="b379b-133">So this will not work:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="b379b-134">La razón de esta regla es que el cuerpo de la solicitud podría almacenarse en una secuencia no almacenado en búfer que solo se puede leer una vez.</span><span class="sxs-lookup"><span data-stu-id="b379b-134">The reason for this rule is that the request body might be stored in a non-buffered stream that can only be read once.</span></span>

## <a name="type-converters"></a><span data-ttu-id="b379b-135">Convertidores de tipos</span><span class="sxs-lookup"><span data-stu-id="b379b-135">Type Converters</span></span>

<span data-ttu-id="b379b-136">Puede hacer API Web de tratar una clase como un tipo simple (de modo que las API Web intentará enlazar del URI) mediante la creación de un **TypeConverter** y proporcionando una conversión de cadenas.</span><span class="sxs-lookup"><span data-stu-id="b379b-136">You can make Web API treat a class as a simple type (so that Web API will try to bind it from the URI) by creating a **TypeConverter** and providing a string conversion.</span></span>

<span data-ttu-id="b379b-137">El código siguiente muestra un `GeoPoint` clase que representa un punto geográfico, más una **TypeConverter** que convierte de cadenas para `GeoPoint` instancias.</span><span class="sxs-lookup"><span data-stu-id="b379b-137">The following code shows a `GeoPoint` class that represents a geographical point, plus a **TypeConverter** that converts from strings to `GeoPoint` instances.</span></span> <span data-ttu-id="b379b-138">El `GeoPoint` clase se decora con un **[TypeConverter]** atributo para especificar el convertidor de tipos.</span><span class="sxs-lookup"><span data-stu-id="b379b-138">The `GeoPoint` class is decorated with a **[TypeConverter]** attribute to specify the type converter.</span></span> <span data-ttu-id="b379b-139">(En este ejemplo está inspirado por entrada de blog de Mike Stall [cómo enlazar a objetos personalizados en firmas de acción de MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span><span class="sxs-lookup"><span data-stu-id="b379b-139">(This example was inspired by Mike Stall's blog post [How to bind to custom objects in action signatures in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="b379b-140">Ahora se tratará la API Web `GeoPoint` como un tipo simple, lo que significa que intentará enlazar `GeoPoint` parámetros del URI.</span><span class="sxs-lookup"><span data-stu-id="b379b-140">Now Web API will treat `GeoPoint` as a simple type, meaning it will try to bind `GeoPoint` parameters from the URI.</span></span> <span data-ttu-id="b379b-141">No es necesario incluir **[FromUri]** en el parámetro.</span><span class="sxs-lookup"><span data-stu-id="b379b-141">You don't need to include **[FromUri]** on the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="b379b-142">El cliente puede invocar el método con un URI similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="b379b-142">The client can invoke the method with a URI like this:</span></span>

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a><span data-ttu-id="b379b-143">Enlazadores modelo</span><span class="sxs-lookup"><span data-stu-id="b379b-143">Model Binders</span></span>

<span data-ttu-id="b379b-144">Es una opción más flexible que un convertidor de tipos crear un enlazador de modelos personalizado.</span><span class="sxs-lookup"><span data-stu-id="b379b-144">A more flexible option than a type converter is to create a custom model binder.</span></span> <span data-ttu-id="b379b-145">Con un enlazador de modelos, se tiene acceso a elementos como la solicitud HTTP, la descripción de la acción y los valores sin formato de los datos de ruta.</span><span class="sxs-lookup"><span data-stu-id="b379b-145">With a model binder, you have access to things like the HTTP request, the action description, and the raw values from the route data.</span></span>

<span data-ttu-id="b379b-146">Para crear un enlazador de modelos, implemente el **IModelBinder** interfaz.</span><span class="sxs-lookup"><span data-stu-id="b379b-146">To create a model binder, implement the **IModelBinder** interface.</span></span> <span data-ttu-id="b379b-147">Esta interfaz define un método único, **BindModel**:</span><span class="sxs-lookup"><span data-stu-id="b379b-147">This interface defines a single method, **BindModel**:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

<span data-ttu-id="b379b-148">Este es un enlazador de modelos para `GeoPoint` objetos.</span><span class="sxs-lookup"><span data-stu-id="b379b-148">Here is a model binder for `GeoPoint` objects.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="b379b-149">Un enlazador de modelos obtiene los valores de entrada sin formato de un *proveedor de valores*.</span><span class="sxs-lookup"><span data-stu-id="b379b-149">A model binder gets raw input values from a *value provider*.</span></span> <span data-ttu-id="b379b-150">Este diseño separa dos funciones distintas:</span><span class="sxs-lookup"><span data-stu-id="b379b-150">This design separates two distinct functions:</span></span>

- <span data-ttu-id="b379b-151">El proveedor de valores recibe la solicitud HTTP y rellena un diccionario de pares clave-valor.</span><span class="sxs-lookup"><span data-stu-id="b379b-151">The value provider takes the HTTP request and populates a dictionary of key-value pairs.</span></span>
- <span data-ttu-id="b379b-152">El enlazador de modelos utiliza este diccionario para rellenar el modelo.</span><span class="sxs-lookup"><span data-stu-id="b379b-152">The model binder uses this dictionary to populate the model.</span></span>

<span data-ttu-id="b379b-153">El proveedor de valor predeterminado en la API de Web obtiene valores de los datos de ruta y la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="b379b-153">The default value provider in Web API gets values from the route data and the query string.</span></span> <span data-ttu-id="b379b-154">Por ejemplo, si el identificador URI es `http://localhost/api/values/1?location=48,-122`, el proveedor de valores crea los siguientes pares de clave y valor:</span><span class="sxs-lookup"><span data-stu-id="b379b-154">For example, if the URI is `http://localhost/api/values/1?location=48,-122`, the value provider creates the following key-value pairs:</span></span>

- <span data-ttu-id="b379b-155">id = &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="b379b-155">id = &quot;1&quot;</span></span>
- <span data-ttu-id="b379b-156">ubicación = &quot;48,122&quot;</span><span class="sxs-lookup"><span data-stu-id="b379b-156">location = &quot;48,122&quot;</span></span>

<span data-ttu-id="b379b-157">(Da por supuesto que la plantilla de ruta predeterminada, que es &quot;api / {controller} / {id}&quot;.)</span><span class="sxs-lookup"><span data-stu-id="b379b-157">(I'm assuming the default route template, which is &quot;api/{controller}/{id}&quot;.)</span></span>

<span data-ttu-id="b379b-158">El nombre del parámetro que se va a enlazar se almacena en la **ModelBindingContext.ModelName** propiedad.</span><span class="sxs-lookup"><span data-stu-id="b379b-158">The name of the parameter to bind is stored in the **ModelBindingContext.ModelName** property.</span></span> <span data-ttu-id="b379b-159">Busca el enlazador de modelos para una clave con este valor en el diccionario.</span><span class="sxs-lookup"><span data-stu-id="b379b-159">The model binder looks for a key with this value in the dictionary.</span></span> <span data-ttu-id="b379b-160">Si el valor existe y se puede convertir en un `GeoPoint`, el enlazador de modelos asigna el valor enlazado a la **ModelBindingContext.Model** propiedad.</span><span class="sxs-lookup"><span data-stu-id="b379b-160">If the value exists and can be converted into a `GeoPoint`, the model binder assigns the bound value to the **ModelBindingContext.Model** property.</span></span>

<span data-ttu-id="b379b-161">Tenga en cuenta que el enlazador de modelos no se limita a una conversión de tipo simple.</span><span class="sxs-lookup"><span data-stu-id="b379b-161">Notice that the model binder is not limited to a simple type conversion.</span></span> <span data-ttu-id="b379b-162">En este ejemplo, el enlazador de modelos en primer lugar busca en una tabla de ubicaciones conocidas y, si se produce un error, usa la conversión de tipos.</span><span class="sxs-lookup"><span data-stu-id="b379b-162">In this example, the model binder first looks in a table of known locations, and if that fails, it uses type conversion.</span></span>

<span data-ttu-id="b379b-163">**Establecer el enlazador de modelos**</span><span class="sxs-lookup"><span data-stu-id="b379b-163">**Setting the Model Binder**</span></span>

<span data-ttu-id="b379b-164">Hay varias maneras de establecer un enlazador de modelos.</span><span class="sxs-lookup"><span data-stu-id="b379b-164">There are several ways to set a model binder.</span></span> <span data-ttu-id="b379b-165">En primer lugar, puede agregar un **[ModelBinder]** para el parámetro de atributo.</span><span class="sxs-lookup"><span data-stu-id="b379b-165">First, you can add a **[ModelBinder]** attribute to the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

<span data-ttu-id="b379b-166">También puede agregar un **[ModelBinder]** para el tipo de atributo.</span><span class="sxs-lookup"><span data-stu-id="b379b-166">You can also add a **[ModelBinder]** attribute to the type.</span></span> <span data-ttu-id="b379b-167">API Web usará el enlazador de modelos especificado para todos los parámetros de ese tipo.</span><span class="sxs-lookup"><span data-stu-id="b379b-167">Web API will use the specified model binder for all parameters of that type.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="b379b-168">Por último, puede agregar un proveedor de enlazador de modelos para la **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="b379b-168">Finally, you can add a model-binder provider to the **HttpConfiguration**.</span></span> <span data-ttu-id="b379b-169">Un proveedor de enlazadores de modelos es simplemente una clase de generador que crea un enlazador de modelos.</span><span class="sxs-lookup"><span data-stu-id="b379b-169">A model-binder provider is simply a factory class that creates a model binder.</span></span> <span data-ttu-id="b379b-170">Puede crear un proveedor derivando de la [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) clase.</span><span class="sxs-lookup"><span data-stu-id="b379b-170">You can create a provider by deriving from the [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) class.</span></span> <span data-ttu-id="b379b-171">Sin embargo, si el enlazador de modelos encarga de un tipo único, es más fácil de usar la integrada **SimpleModelBinderProvider**, que está diseñado para este propósito.</span><span class="sxs-lookup"><span data-stu-id="b379b-171">However, if your model binder handles a single type, it's easier to use the built-in **SimpleModelBinderProvider**, which is designed for this purpose.</span></span> <span data-ttu-id="b379b-172">El código siguiente muestra cómo hacerlo.</span><span class="sxs-lookup"><span data-stu-id="b379b-172">The following code shows how to do this.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

<span data-ttu-id="b379b-173">Con un proveedor de enlace de modelos, deberá agregar el **[ModelBinder]** de atributo para el parámetro, para indicar a API Web que debe usar un enlazador de modelos y no un formateador de tipo de medio.</span><span class="sxs-lookup"><span data-stu-id="b379b-173">With a model-binding provider, you still need to add the **[ModelBinder]** attribute to the parameter, to tell Web API that it should use a model binder and not a media-type formatter.</span></span> <span data-ttu-id="b379b-174">Pero ahora no es necesario especificar el tipo de enlazador de modelos en el atributo:</span><span class="sxs-lookup"><span data-stu-id="b379b-174">But now you don't need to specify the type of model binder in the attribute:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a><span data-ttu-id="b379b-175">Proveedores de valores</span><span class="sxs-lookup"><span data-stu-id="b379b-175">Value Providers</span></span>

<span data-ttu-id="b379b-176">He mencionado que un enlazador de modelos obtiene valores de un proveedor de valores.</span><span class="sxs-lookup"><span data-stu-id="b379b-176">I mentioned that a model binder gets values from a value provider.</span></span> <span data-ttu-id="b379b-177">Para escribir un proveedor de valores personalizados, implementar la **IValueProvider** interfaz.</span><span class="sxs-lookup"><span data-stu-id="b379b-177">To write a custom value provider, implement the **IValueProvider** interface.</span></span> <span data-ttu-id="b379b-178">Este es un ejemplo que extrae los valores de las cookies en la solicitud:</span><span class="sxs-lookup"><span data-stu-id="b379b-178">Here is an example that pulls values from the cookies in the request:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

<span data-ttu-id="b379b-179">También debe crear un generador de proveedores de valor derivando de la **ValueProviderFactory** clase.</span><span class="sxs-lookup"><span data-stu-id="b379b-179">You also need to create a value provider factory by deriving from the **ValueProviderFactory** class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

<span data-ttu-id="b379b-180">Agregar el generador de proveedores de valor para el **HttpConfiguration** como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="b379b-180">Add the value provider factory to the **HttpConfiguration** as follows.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

<span data-ttu-id="b379b-181">API Web compone de todos los proveedores de valor, de modo que cuando se llama un enlazador de modelos **ValueProvider.GetValue**, el enlazador de modelos recibe el valor desde el primer proveedor de valor que es capaz de crearla.</span><span class="sxs-lookup"><span data-stu-id="b379b-181">Web API composes all of the value providers, so when a model binder calls **ValueProvider.GetValue**, the model binder receives the value from the first value provider that is able to produce it.</span></span>

<span data-ttu-id="b379b-182">Como alternativa, puede establecer el generador de proveedores de valor en el nivel de parámetro mediante el **ValueProvider** atributo, como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="b379b-182">Alternatively, you can set the value provider factory at the parameter level by using the **ValueProvider** attribute, as follows:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

<span data-ttu-id="b379b-183">Esto indica a la API de Web para usar el enlace de modelos con el generador de proveedores de valor especificado y no se utiliza cualquiera de los otros proveedores de valores registrados.</span><span class="sxs-lookup"><span data-stu-id="b379b-183">This tells Web API to use model binding with the specified value provider factory, and not to use any of the other registered value providers.</span></span>

## <a name="httpparameterbinding"></a><span data-ttu-id="b379b-184">HttpParameterBinding</span><span class="sxs-lookup"><span data-stu-id="b379b-184">HttpParameterBinding</span></span>

<span data-ttu-id="b379b-185">Enlazadores de modelos son una instancia específica de un mecanismo más general.</span><span class="sxs-lookup"><span data-stu-id="b379b-185">Model binders are a specific instance of a more general mechanism.</span></span> <span data-ttu-id="b379b-186">Si observa la **[ModelBinder]** atributo, verá que se deriva de la clase abstracta **ParameterBindingAttribute** clase.</span><span class="sxs-lookup"><span data-stu-id="b379b-186">If you look at the **[ModelBinder]** attribute, you will see that it derives from the abstract **ParameterBindingAttribute** class.</span></span> <span data-ttu-id="b379b-187">Esta clase define un método único, **GetBinding**, que devuelve un **HttpParameterBinding** objeto:</span><span class="sxs-lookup"><span data-stu-id="b379b-187">This class defines a single method, **GetBinding**, which returns an **HttpParameterBinding** object:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

<span data-ttu-id="b379b-188">Un **HttpParameterBinding** es responsable de enlazar un parámetro a un valor.</span><span class="sxs-lookup"><span data-stu-id="b379b-188">An **HttpParameterBinding** is responsible for binding a parameter to a value.</span></span> <span data-ttu-id="b379b-189">En el caso de **[ModelBinder]**, el atributo devuelve un **HttpParameterBinding** implementación que usa un **IModelBinder** para realizar el enlace real.</span><span class="sxs-lookup"><span data-stu-id="b379b-189">In the case of **[ModelBinder]**, the attribute returns an **HttpParameterBinding** implementation that uses an **IModelBinder** to perform the actual binding.</span></span> <span data-ttu-id="b379b-190">También puede implementar su propio **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="b379b-190">You can also implement your own **HttpParameterBinding**.</span></span>

<span data-ttu-id="b379b-191">Por ejemplo, suponga que desea obtener ETags de `if-match` y `if-none-match` encabezados en la solicitud.</span><span class="sxs-lookup"><span data-stu-id="b379b-191">For example, suppose you want to get ETags from `if-match` and `if-none-match` headers in the request.</span></span> <span data-ttu-id="b379b-192">Comenzaremos definiendo una clase para representar valores de ETag.</span><span class="sxs-lookup"><span data-stu-id="b379b-192">We'll start by defining a class to represent ETags.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

<span data-ttu-id="b379b-193">También se podrá definir una enumeración para indicar si se va a obtener el valor ETag de la `if-match` encabezado o el `if-none-match` encabezado.</span><span class="sxs-lookup"><span data-stu-id="b379b-193">We'll also define an enumeration to indicate whether to get the ETag from the `if-match` header or the `if-none-match` header.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

<span data-ttu-id="b379b-194">Este es un **HttpParameterBinding** que obtiene el valor de ETag desde el encabezado deseado y la enlaza con un parámetro de tipo ETag:</span><span class="sxs-lookup"><span data-stu-id="b379b-194">Here is an **HttpParameterBinding** that gets the ETag from the desired header and binds it to a parameter of type ETag:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

<span data-ttu-id="b379b-195">El **ExecuteBindingAsync** método realiza el enlace.</span><span class="sxs-lookup"><span data-stu-id="b379b-195">The **ExecuteBindingAsync** method does the binding.</span></span> <span data-ttu-id="b379b-196">Dentro de este método, agregue el valor del parámetro enlazado a la **ActionArgument** diccionario en la **HttpActionContext**.</span><span class="sxs-lookup"><span data-stu-id="b379b-196">Within this method, add the bound parameter value to the **ActionArgument** dictionary in the **HttpActionContext**.</span></span>

> [!NOTE]
> <span data-ttu-id="b379b-197">Si su **ExecuteBindingAsync** método lee el cuerpo del mensaje de solicitud, invalide el **WillReadBody** propiedad devuelva true.</span><span class="sxs-lookup"><span data-stu-id="b379b-197">If your **ExecuteBindingAsync** method reads the body of the request message, override the **WillReadBody** property to return true.</span></span> <span data-ttu-id="b379b-198">El cuerpo de la solicitud podría ser una secuencia no almacenado en búfer que solo se puede leer una vez, por lo que la API Web se aplica una regla que a lo sumo un enlace puede leer el cuerpo del mensaje.</span><span class="sxs-lookup"><span data-stu-id="b379b-198">The request body might be an unbuffered stream that can only be read once, so Web API enforces a rule that at most one binding can read the message body.</span></span>


<span data-ttu-id="b379b-199">Para aplicar un personalizado **HttpParameterBinding**, puede definir un atributo que se deriva de **ParameterBindingAttribute**.</span><span class="sxs-lookup"><span data-stu-id="b379b-199">To apply a custom **HttpParameterBinding**, you can define an attribute that derives from **ParameterBindingAttribute**.</span></span> <span data-ttu-id="b379b-200">Para `ETagParameterBinding`, se definirá dos atributos, uno para `if-match` encabezados y otro para `if-none-match` encabezados.</span><span class="sxs-lookup"><span data-stu-id="b379b-200">For `ETagParameterBinding`, we'll define two attributes, one for `if-match` headers and one for `if-none-match` headers.</span></span> <span data-ttu-id="b379b-201">Se derivan de una clase base abstracta.</span><span class="sxs-lookup"><span data-stu-id="b379b-201">Both derive from an abstract base class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

<span data-ttu-id="b379b-202">Éste es un método de controlador que usa el `[IfNoneMatch]` atributo.</span><span class="sxs-lookup"><span data-stu-id="b379b-202">Here is a controller method that uses the `[IfNoneMatch]` attribute.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

<span data-ttu-id="b379b-203">Además **ParameterBindingAttribute**, hay otro enlace para agregar un personalizado **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="b379b-203">Besides **ParameterBindingAttribute**, there is another hook for adding a custom **HttpParameterBinding**.</span></span> <span data-ttu-id="b379b-204">En el **HttpConfiguration** objeto, el **ParameterBindingRules** propiedad es una colección de las funciones de anomymous del tipo (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**).</span><span class="sxs-lookup"><span data-stu-id="b379b-204">On the **HttpConfiguration** object, the **ParameterBindingRules** property is a collection of anomymous functions of type (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**).</span></span> <span data-ttu-id="b379b-205">Por ejemplo, podría agregar una regla que utilice cualquier parámetro de ETag en un método GET `ETagParameterBinding` con `if-none-match`:</span><span class="sxs-lookup"><span data-stu-id="b379b-205">For example, you could add a rule that any ETag parameter on a GET method uses `ETagParameterBinding` with `if-none-match`:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

<span data-ttu-id="b379b-206">La función debe devolver `null` para los parámetros que el enlace no es aplicable.</span><span class="sxs-lookup"><span data-stu-id="b379b-206">The function should return `null` for parameters where the binding is not applicable.</span></span>

## <a name="iactionvaluebinder"></a><span data-ttu-id="b379b-207">IActionValueBinder</span><span class="sxs-lookup"><span data-stu-id="b379b-207">IActionValueBinder</span></span>

<span data-ttu-id="b379b-208">Todo el proceso de enlace de parámetros se controla mediante un servicio conectable, **IActionValueBinder**.</span><span class="sxs-lookup"><span data-stu-id="b379b-208">The entire parameter-binding process is controlled by a pluggable service, **IActionValueBinder**.</span></span> <span data-ttu-id="b379b-209">La implementación predeterminada de **IActionValueBinder** hace lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="b379b-209">The default implementation of **IActionValueBinder** does the following:</span></span>

1. <span data-ttu-id="b379b-210">Busque un **ParameterBindingAttribute** en el parámetro.</span><span class="sxs-lookup"><span data-stu-id="b379b-210">Look for a **ParameterBindingAttribute** on the parameter.</span></span> <span data-ttu-id="b379b-211">Esto incluye **[FromBody]**, **[FromUri]**, y **[ModelBinder]**, o atributos personalizados.</span><span class="sxs-lookup"><span data-stu-id="b379b-211">This includes **[FromBody]**, **[FromUri]**, and **[ModelBinder]**, or custom attributes.</span></span>
2. <span data-ttu-id="b379b-212">En caso contrario, busque en **HttpConfiguration.ParameterBindingRules** para una función que devuelve un valor no nulo **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="b379b-212">Otherwise, look in **HttpConfiguration.ParameterBindingRules** for a function that returns a non-null **HttpParameterBinding**.</span></span>
3. <span data-ttu-id="b379b-213">De lo contrario, utilice las reglas predeterminadas que describieron anteriormente.</span><span class="sxs-lookup"><span data-stu-id="b379b-213">Otherwise, use the default rules that I described previously.</span></span> 

    - <span data-ttu-id="b379b-214">Si el tipo de parámetro es "simple" o tiene un convertidor de tipos, enlazar del URI.</span><span class="sxs-lookup"><span data-stu-id="b379b-214">If the parameter type is "simple"or has a type converter, bind from the URI.</span></span> <span data-ttu-id="b379b-215">Esto es equivalente al obtenido al colocar el **[FromUri]** atributo en el parámetro.</span><span class="sxs-lookup"><span data-stu-id="b379b-215">This is equivalent to putting the **[FromUri]** attribute on the parameter.</span></span>
    - <span data-ttu-id="b379b-216">En caso contrario, vuelva a intentar leer el parámetro del cuerpo del mensaje.</span><span class="sxs-lookup"><span data-stu-id="b379b-216">Otherwise, try to read the parameter from the message body.</span></span> <span data-ttu-id="b379b-217">Esto es equivalente a colocar **[FromBody]** en el parámetro.</span><span class="sxs-lookup"><span data-stu-id="b379b-217">This is equivalent to putting **[FromBody]** on the parameter.</span></span>

<span data-ttu-id="b379b-218">Si lo desea, puede reemplazar toda la **IActionValueBinder** servicio con una implementación personalizada.</span><span class="sxs-lookup"><span data-stu-id="b379b-218">If you wanted, you could replace the entire **IActionValueBinder** service with a custom implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b379b-219">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="b379b-219">Additional Resources</span></span>

[<span data-ttu-id="b379b-220">Ejemplo de enlace de parámetros personalizado</span><span class="sxs-lookup"><span data-stu-id="b379b-220">Custom Parameter Binding Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

<span data-ttu-id="b379b-221">Mike Stall escribió una buena serie de entradas de blog sobre el enlace de parámetro de API Web:</span><span class="sxs-lookup"><span data-stu-id="b379b-221">Mike Stall wrote a good series of blog posts about Web API parameter binding:</span></span>

- [<span data-ttu-id="b379b-222">¿Cómo de API Web de enlace de parámetros</span><span class="sxs-lookup"><span data-stu-id="b379b-222">How Web API does Parameter Binding</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [<span data-ttu-id="b379b-223">Enlace de parámetro de estilo MVC para API Web</span><span class="sxs-lookup"><span data-stu-id="b379b-223">MVC Style parameter binding for Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [<span data-ttu-id="b379b-224">Cómo enlazar a objetos personalizados en firmas de acción de MVC o Web API</span><span class="sxs-lookup"><span data-stu-id="b379b-224">How to bind to custom objects in action signatures in MVC/Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [<span data-ttu-id="b379b-225">Cómo crear un proveedor de valor personalizado en Web API</span><span class="sxs-lookup"><span data-stu-id="b379b-225">How to create a custom value provider in Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [<span data-ttu-id="b379b-226">Enlace de parámetros de la API Web en segundo plano</span><span class="sxs-lookup"><span data-stu-id="b379b-226">Web API Parameter binding under the hood</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
