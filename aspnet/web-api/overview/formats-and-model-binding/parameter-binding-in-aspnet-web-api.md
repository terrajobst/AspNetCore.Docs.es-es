---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Parámetro de enlace en ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/11/2013
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 97785db962691c25bac03f904924321af2f6b500
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810087"
---
<a name="parameter-binding-in-aspnet-web-api"></a><span data-ttu-id="747fd-102">Parámetro de enlace en ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="747fd-102">Parameter Binding in ASP.NET Web API</span></span>
====================
<span data-ttu-id="747fd-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="747fd-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="747fd-104">Cuando la API Web llama a un método en un controlador, se deben establecer valores para los parámetros, un proceso denominado *enlace*.</span><span class="sxs-lookup"><span data-stu-id="747fd-104">When Web API calls a method on a controller, it must set values for the parameters, a process called *binding*.</span></span> <span data-ttu-id="747fd-105">En este artículo se describe cómo Web API enlaza los parámetros y cómo puede personalizar el proceso de enlace.</span><span class="sxs-lookup"><span data-stu-id="747fd-105">This article describes how Web API binds parameters, and how you can customize the binding process.</span></span>

<span data-ttu-id="747fd-106">De forma predeterminada, Web API usa las siguientes reglas para enlazar parámetros:</span><span class="sxs-lookup"><span data-stu-id="747fd-106">By default, Web API uses the following rules to bind parameters:</span></span>

- <span data-ttu-id="747fd-107">Si el parámetro es un tipo "simple", API Web intenta obtener el valor del URI.</span><span class="sxs-lookup"><span data-stu-id="747fd-107">If the parameter is a "simple" type, Web API tries to get the value from the URI.</span></span> <span data-ttu-id="747fd-108">Los tipos simples incluyen .NET [tipos primitivos](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **doble**, y así sucesivamente), además de **TimeSpan**, **DateTime**, **Guid**, **decimal**, y **cadena**, *plus* cualquier tipo con un convertidor de tipos puede convertir una cadena.</span><span class="sxs-lookup"><span data-stu-id="747fd-108">Simple types include the .NET [primitive types](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, and so forth), plus **TimeSpan**, **DateTime**, **Guid**, **decimal**, and **string**, *plus* any type with a type converter that can convert from a string.</span></span> <span data-ttu-id="747fd-109">(Más información acerca de los convertidores de tipos más adelante).</span><span class="sxs-lookup"><span data-stu-id="747fd-109">(More about type converters later.)</span></span>
- <span data-ttu-id="747fd-110">Para tipos complejos, API Web intenta leer el valor del cuerpo del mensaje, pero utiliza un [formateador de tipo de medio](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="747fd-110">For complex types, Web API tries to read the value from the message body, using a [media-type formatter](media-formatters.md).</span></span>

<span data-ttu-id="747fd-111">Por ejemplo, este es un método de controlador Web API típico:</span><span class="sxs-lookup"><span data-stu-id="747fd-111">For example, here is a typical Web API controller method:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="747fd-112">El *id* parámetro es un &quot;simple&quot; escriba, por lo que la API Web intenta obtener el valor URI de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="747fd-112">The *id* parameter is a &quot;simple&quot; type, so Web API tries to get the value from the request URI.</span></span> <span data-ttu-id="747fd-113">El *elemento* parámetro es un tipo complejo, por lo que la API Web utiliza un formateador de tipo de medio para leer el valor desde el cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="747fd-113">The *item* parameter is a complex type, so Web API uses a media-type formatter to read the value from the request body.</span></span>

<span data-ttu-id="747fd-114">Para obtener un valor de identificador URI, Web API se busca en los datos de ruta y la cadena de consulta URI.</span><span class="sxs-lookup"><span data-stu-id="747fd-114">To get a value from the URI, Web API looks in the route data and the URI query string.</span></span> <span data-ttu-id="747fd-115">Los datos de ruta se rellenan cuando el sistema de enrutamiento analiza el identificador URI y coincide con una ruta.</span><span class="sxs-lookup"><span data-stu-id="747fd-115">The route data is populated when the routing system parses the URI and matches it to a route.</span></span> <span data-ttu-id="747fd-116">Para obtener más información, consulte [enrutamiento y selección de acción](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="747fd-116">For more information, see [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span>

<span data-ttu-id="747fd-117">En el resto de este artículo, le mostraré cómo puede personalizar el proceso de enlace de modelo.</span><span class="sxs-lookup"><span data-stu-id="747fd-117">In the rest of this article, I'll show how you can customize the model binding process.</span></span> <span data-ttu-id="747fd-118">Para los tipos complejos, sin embargo, considere el uso de formateadores de tipo multimedia siempre que sea posible.</span><span class="sxs-lookup"><span data-stu-id="747fd-118">For complex types, however, consider using media-type formatters whenever possible.</span></span> <span data-ttu-id="747fd-119">Un principio clave de HTTP es que los recursos se envían en el cuerpo del mensaje utilizando la negociación de contenido para especificar la representación del recurso.</span><span class="sxs-lookup"><span data-stu-id="747fd-119">A key principle of HTTP is that resources are sent in the message body, using content negotiation to specify the representation of the resource.</span></span> <span data-ttu-id="747fd-120">Formateadores de tipo de medio se diseñaron exactamente para este propósito.</span><span class="sxs-lookup"><span data-stu-id="747fd-120">Media-type formatters were designed for exactly this purpose.</span></span>

## <a name="using-fromuri"></a><span data-ttu-id="747fd-121">Uso de [FromUri]</span><span class="sxs-lookup"><span data-stu-id="747fd-121">Using [FromUri]</span></span>

<span data-ttu-id="747fd-122">Para forzar a Web API para leer un tipo complejo del URI, agregue el **[FromUri]** al parámetro.</span><span class="sxs-lookup"><span data-stu-id="747fd-122">To force Web API to read a complex type from the URI, add the **[FromUri]** attribute to the parameter.</span></span> <span data-ttu-id="747fd-123">En el ejemplo siguiente se define un `GeoPoint` tipo, junto con un método de controlador que obtiene el `GeoPoint` desde el URI.</span><span class="sxs-lookup"><span data-stu-id="747fd-123">The following example defines a `GeoPoint` type, along with a controller method that gets the `GeoPoint` from the URI.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="747fd-124">El cliente puede colocar los valores de latitud y longitud en la cadena de consulta y Web API se usarán para construir un `GeoPoint`.</span><span class="sxs-lookup"><span data-stu-id="747fd-124">The client can put the Latitude and Longitude values in the query string and Web API will use them to construct a `GeoPoint`.</span></span> <span data-ttu-id="747fd-125">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="747fd-125">For example:</span></span>

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a><span data-ttu-id="747fd-126">Uso de [FromBody]</span><span class="sxs-lookup"><span data-stu-id="747fd-126">Using [FromBody]</span></span>

<span data-ttu-id="747fd-127">Para forzar a Web API para leer un tipo simple desde el cuerpo de solicitud, agregue el **[FromBody]** al parámetro:</span><span class="sxs-lookup"><span data-stu-id="747fd-127">To force Web API to read a simple type from the request body, add the **[FromBody]** attribute to the parameter:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="747fd-128">En este ejemplo, API Web usará un formateador de tipo de medio para leer el valor de *nombre* desde el cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="747fd-128">In this example, Web API will use a media-type formatter to read the value of *name* from the request body.</span></span> <span data-ttu-id="747fd-129">Este es un ejemplo de solicitud de cliente.</span><span class="sxs-lookup"><span data-stu-id="747fd-129">Here is an example client request.</span></span>

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

<span data-ttu-id="747fd-130">Cuando un parámetro tiene [FromBody], API Web usa el encabezado Content-Type para seleccionar a un formateador.</span><span class="sxs-lookup"><span data-stu-id="747fd-130">When a parameter has [FromBody], Web API uses the Content-Type header to select a formatter.</span></span> <span data-ttu-id="747fd-131">En este ejemplo, el tipo de contenido es &quot;application/json&quot; y el cuerpo de solicitud es una cadena JSON sin formato (no un objeto JSON).</span><span class="sxs-lookup"><span data-stu-id="747fd-131">In this example, the content type is &quot;application/json&quot; and the request body is a raw JSON string (not a JSON object).</span></span>

<span data-ttu-id="747fd-132">A lo sumo un parámetro puede leer desde el cuerpo del mensaje.</span><span class="sxs-lookup"><span data-stu-id="747fd-132">At most one parameter is allowed to read from the message body.</span></span> <span data-ttu-id="747fd-133">Por lo que esto no funcionará:</span><span class="sxs-lookup"><span data-stu-id="747fd-133">So this will not work:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="747fd-134">La razón de esta regla es que el cuerpo de solicitud podría almacenarse en una secuencia sin búfer que solo se puede leer una vez.</span><span class="sxs-lookup"><span data-stu-id="747fd-134">The reason for this rule is that the request body might be stored in a non-buffered stream that can only be read once.</span></span>

## <a name="type-converters"></a><span data-ttu-id="747fd-135">Convertidores de tipos</span><span class="sxs-lookup"><span data-stu-id="747fd-135">Type Converters</span></span>

<span data-ttu-id="747fd-136">Puede realizar Web API tratar una clase como un tipo simple (para que Web API intentará enlazar desde el URI) mediante la creación de un **TypeConverter** y proporcionando una conversión de cadena.</span><span class="sxs-lookup"><span data-stu-id="747fd-136">You can make Web API treat a class as a simple type (so that Web API will try to bind it from the URI) by creating a **TypeConverter** and providing a string conversion.</span></span>

<span data-ttu-id="747fd-137">El código siguiente muestra un `GeoPoint` clase que representa un punto geográfico, más una **TypeConverter** que convierte las cadenas a `GeoPoint` instancias.</span><span class="sxs-lookup"><span data-stu-id="747fd-137">The following code shows a `GeoPoint` class that represents a geographical point, plus a **TypeConverter** that converts from strings to `GeoPoint` instances.</span></span> <span data-ttu-id="747fd-138">El `GeoPoint` clase se decora con un **[TypeConverter]** atributo para especificar el convertidor de tipos.</span><span class="sxs-lookup"><span data-stu-id="747fd-138">The `GeoPoint` class is decorated with a **[TypeConverter]** attribute to specify the type converter.</span></span> <span data-ttu-id="747fd-139">(En este ejemplo se inspiró en la entrada de blog de Mike Stall [cómo enlazar a objetos personalizados en las firmas de acción de MVC y WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span><span class="sxs-lookup"><span data-stu-id="747fd-139">(This example was inspired by Mike Stall's blog post [How to bind to custom objects in action signatures in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="747fd-140">Ahora se tratará la API Web `GeoPoint` como un tipo simple, lo que significa que intentará enlazar `GeoPoint` parámetros desde el URI.</span><span class="sxs-lookup"><span data-stu-id="747fd-140">Now Web API will treat `GeoPoint` as a simple type, meaning it will try to bind `GeoPoint` parameters from the URI.</span></span> <span data-ttu-id="747fd-141">No es necesario incluir **[FromUri]** en el parámetro.</span><span class="sxs-lookup"><span data-stu-id="747fd-141">You don't need to include **[FromUri]** on the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="747fd-142">El cliente puede invocar el método con un URI como este:</span><span class="sxs-lookup"><span data-stu-id="747fd-142">The client can invoke the method with a URI like this:</span></span>

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a><span data-ttu-id="747fd-143">Enlazadores modelo</span><span class="sxs-lookup"><span data-stu-id="747fd-143">Model Binders</span></span>

<span data-ttu-id="747fd-144">Es una opción más flexible que un convertidor de tipos crear un enlazador de modelos personalizado.</span><span class="sxs-lookup"><span data-stu-id="747fd-144">A more flexible option than a type converter is to create a custom model binder.</span></span> <span data-ttu-id="747fd-145">Con un enlazador de modelos, tendrá acceso a cosas como la solicitud HTTP, la descripción de la acción y los valores sin procesar de los datos de ruta.</span><span class="sxs-lookup"><span data-stu-id="747fd-145">With a model binder, you have access to things like the HTTP request, the action description, and the raw values from the route data.</span></span>

<span data-ttu-id="747fd-146">Para crear un enlazador de modelos, implemente el **IModelBinder** interfaz.</span><span class="sxs-lookup"><span data-stu-id="747fd-146">To create a model binder, implement the **IModelBinder** interface.</span></span> <span data-ttu-id="747fd-147">Esta interfaz define un método único, **BindModel**:</span><span class="sxs-lookup"><span data-stu-id="747fd-147">This interface defines a single method, **BindModel**:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

<span data-ttu-id="747fd-148">Este es un enlazador de modelos para `GeoPoint` objetos.</span><span class="sxs-lookup"><span data-stu-id="747fd-148">Here is a model binder for `GeoPoint` objects.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="747fd-149">Un enlazador de modelos obtiene los valores de entrada sin procesar de un *proveedor de valores*.</span><span class="sxs-lookup"><span data-stu-id="747fd-149">A model binder gets raw input values from a *value provider*.</span></span> <span data-ttu-id="747fd-150">Este diseño separa dos funciones distintas:</span><span class="sxs-lookup"><span data-stu-id="747fd-150">This design separates two distinct functions:</span></span>

- <span data-ttu-id="747fd-151">El proveedor de valores recibe la solicitud HTTP y rellena un diccionario de pares clave-valor.</span><span class="sxs-lookup"><span data-stu-id="747fd-151">The value provider takes the HTTP request and populates a dictionary of key-value pairs.</span></span>
- <span data-ttu-id="747fd-152">El enlazador de modelos usa este diccionario para rellenar el modelo.</span><span class="sxs-lookup"><span data-stu-id="747fd-152">The model binder uses this dictionary to populate the model.</span></span>

<span data-ttu-id="747fd-153">El proveedor de valor predeterminado en la API Web obtiene valores de los datos de ruta y la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="747fd-153">The default value provider in Web API gets values from the route data and the query string.</span></span> <span data-ttu-id="747fd-154">Por ejemplo, si el URI es `http://localhost/api/values/1?location=48,-122`, el proveedor de valores crea los siguientes pares clave-valor:</span><span class="sxs-lookup"><span data-stu-id="747fd-154">For example, if the URI is `http://localhost/api/values/1?location=48,-122`, the value provider creates the following key-value pairs:</span></span>

- <span data-ttu-id="747fd-155">Id. = &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="747fd-155">id = &quot;1&quot;</span></span>
- <span data-ttu-id="747fd-156">ubicación = &quot;48,122&quot;</span><span class="sxs-lookup"><span data-stu-id="747fd-156">location = &quot;48,122&quot;</span></span>

<span data-ttu-id="747fd-157">(Supongo que la plantilla de ruta predeterminada, que es &quot;api / {controller} / {id}&quot;.)</span><span class="sxs-lookup"><span data-stu-id="747fd-157">(I'm assuming the default route template, which is &quot;api/{controller}/{id}&quot;.)</span></span>

<span data-ttu-id="747fd-158">El nombre del parámetro que se va a enlazar se almacena en el **ModelBindingContext.ModelName** propiedad.</span><span class="sxs-lookup"><span data-stu-id="747fd-158">The name of the parameter to bind is stored in the **ModelBindingContext.ModelName** property.</span></span> <span data-ttu-id="747fd-159">El enlazador de modelos busca una clave con este valor en el diccionario.</span><span class="sxs-lookup"><span data-stu-id="747fd-159">The model binder looks for a key with this value in the dictionary.</span></span> <span data-ttu-id="747fd-160">Si el valor existe y se puede convertir en un `GeoPoint`, el enlazador de modelos asigna el valor enlazado a la **ModelBindingContext.Model** propiedad.</span><span class="sxs-lookup"><span data-stu-id="747fd-160">If the value exists and can be converted into a `GeoPoint`, the model binder assigns the bound value to the **ModelBindingContext.Model** property.</span></span>

<span data-ttu-id="747fd-161">Tenga en cuenta que el enlazador de modelos no se limita a una conversión de tipo simple.</span><span class="sxs-lookup"><span data-stu-id="747fd-161">Notice that the model binder is not limited to a simple type conversion.</span></span> <span data-ttu-id="747fd-162">En este ejemplo, el enlazador de modelos busca primero en una tabla de ubicaciones conocidas y, si se produce un error, utiliza la conversión de tipo.</span><span class="sxs-lookup"><span data-stu-id="747fd-162">In this example, the model binder first looks in a table of known locations, and if that fails, it uses type conversion.</span></span>

<span data-ttu-id="747fd-163">**Establecer el enlazador de modelos**</span><span class="sxs-lookup"><span data-stu-id="747fd-163">**Setting the Model Binder**</span></span>

<span data-ttu-id="747fd-164">Hay varias maneras de establecer un enlazador de modelos.</span><span class="sxs-lookup"><span data-stu-id="747fd-164">There are several ways to set a model binder.</span></span> <span data-ttu-id="747fd-165">En primer lugar, puede agregar un **[ModelBinder]** al parámetro.</span><span class="sxs-lookup"><span data-stu-id="747fd-165">First, you can add a **[ModelBinder]** attribute to the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

<span data-ttu-id="747fd-166">También puede agregar un **[ModelBinder]** al tipo de atributo.</span><span class="sxs-lookup"><span data-stu-id="747fd-166">You can also add a **[ModelBinder]** attribute to the type.</span></span> <span data-ttu-id="747fd-167">API Web usará el enlazador de modelos especificado para todos los parámetros de ese tipo.</span><span class="sxs-lookup"><span data-stu-id="747fd-167">Web API will use the specified model binder for all parameters of that type.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="747fd-168">Por último, puede agregar un proveedor de enlazador de modelos para la **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="747fd-168">Finally, you can add a model-binder provider to the **HttpConfiguration**.</span></span> <span data-ttu-id="747fd-169">Un proveedor de enlazadores de modelos es simplemente una clase de generador que crea un enlazador de modelos.</span><span class="sxs-lookup"><span data-stu-id="747fd-169">A model-binder provider is simply a factory class that creates a model binder.</span></span> <span data-ttu-id="747fd-170">Puede crear un proveedor mediante la derivación de la [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) clase.</span><span class="sxs-lookup"><span data-stu-id="747fd-170">You can create a provider by deriving from the [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) class.</span></span> <span data-ttu-id="747fd-171">Sin embargo, si su enlazador de modelos administra un solo tipo, resulta más fácil de usar integrado **SimpleModelBinderProvider**, que está diseñado para este propósito.</span><span class="sxs-lookup"><span data-stu-id="747fd-171">However, if your model binder handles a single type, it's easier to use the built-in **SimpleModelBinderProvider**, which is designed for this purpose.</span></span> <span data-ttu-id="747fd-172">El código siguiente muestra cómo hacerlo.</span><span class="sxs-lookup"><span data-stu-id="747fd-172">The following code shows how to do this.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

<span data-ttu-id="747fd-173">Con un proveedor de enlace de modelos, deberá agregar el **[ModelBinder]** al parámetro, para indicar a la API Web que debe usar un enlazador de modelos y no un formateador de tipo de medio.</span><span class="sxs-lookup"><span data-stu-id="747fd-173">With a model-binding provider, you still need to add the **[ModelBinder]** attribute to the parameter, to tell Web API that it should use a model binder and not a media-type formatter.</span></span> <span data-ttu-id="747fd-174">Pero ahora no es necesario especificar el tipo de enlazador de modelos en el atributo:</span><span class="sxs-lookup"><span data-stu-id="747fd-174">But now you don't need to specify the type of model binder in the attribute:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a><span data-ttu-id="747fd-175">Proveedores de valor</span><span class="sxs-lookup"><span data-stu-id="747fd-175">Value Providers</span></span>

<span data-ttu-id="747fd-176">Mencioné que un enlazador de modelos obtiene los valores de un proveedor de valores.</span><span class="sxs-lookup"><span data-stu-id="747fd-176">I mentioned that a model binder gets values from a value provider.</span></span> <span data-ttu-id="747fd-177">Para escribir un proveedor de valores personalizados, implementar la **IValueProvider** interfaz.</span><span class="sxs-lookup"><span data-stu-id="747fd-177">To write a custom value provider, implement the **IValueProvider** interface.</span></span> <span data-ttu-id="747fd-178">Este es un ejemplo que extrae los valores de las cookies en la solicitud:</span><span class="sxs-lookup"><span data-stu-id="747fd-178">Here is an example that pulls values from the cookies in the request:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

<span data-ttu-id="747fd-179">También deberá crear una fábrica de proveedor de valor mediante la derivación desde el **ValueProviderFactory** clase.</span><span class="sxs-lookup"><span data-stu-id="747fd-179">You also need to create a value provider factory by deriving from the **ValueProviderFactory** class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

<span data-ttu-id="747fd-180">Agregar el generador de proveedores de valor para el **HttpConfiguration** como sigue.</span><span class="sxs-lookup"><span data-stu-id="747fd-180">Add the value provider factory to the **HttpConfiguration** as follows.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

<span data-ttu-id="747fd-181">API Web compone de todos los proveedores de valor, por lo que cuando llama un enlazador de modelos **ValueProvider.GetValue**, el enlazador de modelos recibe el valor desde el primer proveedor de valor que es capaz de producirlo.</span><span class="sxs-lookup"><span data-stu-id="747fd-181">Web API composes all of the value providers, so when a model binder calls **ValueProvider.GetValue**, the model binder receives the value from the first value provider that is able to produce it.</span></span>

<span data-ttu-id="747fd-182">Como alternativa, puede establecer el generador de proveedores de valor en el nivel de parámetro mediante el **ValueProvider** atributo, como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="747fd-182">Alternatively, you can set the value provider factory at the parameter level by using the **ValueProvider** attribute, as follows:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

<span data-ttu-id="747fd-183">Esto indica a Web API para usar el enlace de modelos con el generador de proveedores de valor especificado y no se debe utilizar cualquiera de los otros proveedores de valor registrados.</span><span class="sxs-lookup"><span data-stu-id="747fd-183">This tells Web API to use model binding with the specified value provider factory, and not to use any of the other registered value providers.</span></span>

## <a name="httpparameterbinding"></a><span data-ttu-id="747fd-184">HttpParameterBinding</span><span class="sxs-lookup"><span data-stu-id="747fd-184">HttpParameterBinding</span></span>

<span data-ttu-id="747fd-185">Los enlazadores de modelos son una instancia específica de un mecanismo más general.</span><span class="sxs-lookup"><span data-stu-id="747fd-185">Model binders are a specific instance of a more general mechanism.</span></span> <span data-ttu-id="747fd-186">Si observa la **[ModelBinder]** atributo, verá que se deriva de la clase abstracta **ParameterBindingAttribute** clase.</span><span class="sxs-lookup"><span data-stu-id="747fd-186">If you look at the **[ModelBinder]** attribute, you will see that it derives from the abstract **ParameterBindingAttribute** class.</span></span> <span data-ttu-id="747fd-187">Esta clase define un método único, **GetBinding**, que devuelve un **HttpParameterBinding** objeto:</span><span class="sxs-lookup"><span data-stu-id="747fd-187">This class defines a single method, **GetBinding**, which returns an **HttpParameterBinding** object:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

<span data-ttu-id="747fd-188">Un **HttpParameterBinding** es responsable de enlazar un parámetro a un valor.</span><span class="sxs-lookup"><span data-stu-id="747fd-188">An **HttpParameterBinding** is responsible for binding a parameter to a value.</span></span> <span data-ttu-id="747fd-189">En el caso de **[ModelBinder]**, devuelve el atributo una **HttpParameterBinding** implementación que utiliza un **IModelBinder** para realizar el enlace real.</span><span class="sxs-lookup"><span data-stu-id="747fd-189">In the case of **[ModelBinder]**, the attribute returns an **HttpParameterBinding** implementation that uses an **IModelBinder** to perform the actual binding.</span></span> <span data-ttu-id="747fd-190">También puede implementar su propio **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="747fd-190">You can also implement your own **HttpParameterBinding**.</span></span>

<span data-ttu-id="747fd-191">Por ejemplo, suponga que desea obtener ETags desde `if-match` y `if-none-match` encabezados en la solicitud.</span><span class="sxs-lookup"><span data-stu-id="747fd-191">For example, suppose you want to get ETags from `if-match` and `if-none-match` headers in the request.</span></span> <span data-ttu-id="747fd-192">Comenzaremos por definir una clase para representar valores de ETag.</span><span class="sxs-lookup"><span data-stu-id="747fd-192">We'll start by defining a class to represent ETags.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

<span data-ttu-id="747fd-193">También definiremos una enumeración para indicar si se debe obtener el valor ETag de la `if-match` encabezado o el `if-none-match` encabezado.</span><span class="sxs-lookup"><span data-stu-id="747fd-193">We'll also define an enumeration to indicate whether to get the ETag from the `if-match` header or the `if-none-match` header.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

<span data-ttu-id="747fd-194">Este es un **HttpParameterBinding** que obtiene el valor ETag del encabezado deseado y lo enlaza a un parámetro de tipo ETag:</span><span class="sxs-lookup"><span data-stu-id="747fd-194">Here is an **HttpParameterBinding** that gets the ETag from the desired header and binds it to a parameter of type ETag:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

<span data-ttu-id="747fd-195">El **ExecuteBindingAsync** método realiza el enlace.</span><span class="sxs-lookup"><span data-stu-id="747fd-195">The **ExecuteBindingAsync** method does the binding.</span></span> <span data-ttu-id="747fd-196">Dentro de este método, agregue el valor del parámetro enlazado a la **ActionArgument** diccionario en el **HttpActionContext**.</span><span class="sxs-lookup"><span data-stu-id="747fd-196">Within this method, add the bound parameter value to the **ActionArgument** dictionary in the **HttpActionContext**.</span></span>

> [!NOTE]
> <span data-ttu-id="747fd-197">Si su **ExecuteBindingAsync** método lee el cuerpo del mensaje de solicitud, reemplace el **WillReadBody** propiedad devuelva true.</span><span class="sxs-lookup"><span data-stu-id="747fd-197">If your **ExecuteBindingAsync** method reads the body of the request message, override the **WillReadBody** property to return true.</span></span> <span data-ttu-id="747fd-198">El cuerpo de solicitud podría ser una secuencia sin almacenamiento en búfer que solo se puede leer una vez, por lo que la API Web se aplica una regla que a lo sumo un enlace puede leer el cuerpo del mensaje.</span><span class="sxs-lookup"><span data-stu-id="747fd-198">The request body might be an unbuffered stream that can only be read once, so Web API enforces a rule that at most one binding can read the message body.</span></span>


<span data-ttu-id="747fd-199">Para aplicar un personalizado **HttpParameterBinding**, puede definir un atributo que se deriva de **ParameterBindingAttribute**.</span><span class="sxs-lookup"><span data-stu-id="747fd-199">To apply a custom **HttpParameterBinding**, you can define an attribute that derives from **ParameterBindingAttribute**.</span></span> <span data-ttu-id="747fd-200">Para `ETagParameterBinding`, definiremos dos atributos, uno para `if-match` encabezados y otra para `if-none-match` encabezados.</span><span class="sxs-lookup"><span data-stu-id="747fd-200">For `ETagParameterBinding`, we'll define two attributes, one for `if-match` headers and one for `if-none-match` headers.</span></span> <span data-ttu-id="747fd-201">Ambos derivan de una clase base abstracta.</span><span class="sxs-lookup"><span data-stu-id="747fd-201">Both derive from an abstract base class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

<span data-ttu-id="747fd-202">Este es un método de controlador que utiliza el `[IfNoneMatch]` atributo.</span><span class="sxs-lookup"><span data-stu-id="747fd-202">Here is a controller method that uses the `[IfNoneMatch]` attribute.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

<span data-ttu-id="747fd-203">Además **ParameterBindingAttribute**, hay otro enlace para agregar un personalizado **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="747fd-203">Besides **ParameterBindingAttribute**, there is another hook for adding a custom **HttpParameterBinding**.</span></span> <span data-ttu-id="747fd-204">En el **HttpConfiguration** objeto, el **ParameterBindingRules** propiedad es una colección de funciones anomymous de tipo (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**).</span><span class="sxs-lookup"><span data-stu-id="747fd-204">On the **HttpConfiguration** object, the **ParameterBindingRules** property is a collection of anomymous functions of type (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**).</span></span> <span data-ttu-id="747fd-205">Por ejemplo, podría agregar una regla que utiliza cualquier otro parámetro ETag en un método GET `ETagParameterBinding` con `if-none-match`:</span><span class="sxs-lookup"><span data-stu-id="747fd-205">For example, you could add a rule that any ETag parameter on a GET method uses `ETagParameterBinding` with `if-none-match`:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

<span data-ttu-id="747fd-206">La función debe devolver `null` para los parámetros donde el enlace no es aplicable.</span><span class="sxs-lookup"><span data-stu-id="747fd-206">The function should return `null` for parameters where the binding is not applicable.</span></span>

## <a name="iactionvaluebinder"></a><span data-ttu-id="747fd-207">IActionValueBinder</span><span class="sxs-lookup"><span data-stu-id="747fd-207">IActionValueBinder</span></span>

<span data-ttu-id="747fd-208">Todo el proceso de enlace de parámetros se controla mediante un servicio conectable, **IActionValueBinder**.</span><span class="sxs-lookup"><span data-stu-id="747fd-208">The entire parameter-binding process is controlled by a pluggable service, **IActionValueBinder**.</span></span> <span data-ttu-id="747fd-209">La implementación predeterminada de **IActionValueBinder** hace lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="747fd-209">The default implementation of **IActionValueBinder** does the following:</span></span>

1. <span data-ttu-id="747fd-210">Busque un **ParameterBindingAttribute** en el parámetro.</span><span class="sxs-lookup"><span data-stu-id="747fd-210">Look for a **ParameterBindingAttribute** on the parameter.</span></span> <span data-ttu-id="747fd-211">Esto incluye **[FromBody]**, **[FromUri]**, y **[ModelBinder]**, o los atributos personalizados.</span><span class="sxs-lookup"><span data-stu-id="747fd-211">This includes **[FromBody]**, **[FromUri]**, and **[ModelBinder]**, or custom attributes.</span></span>
2. <span data-ttu-id="747fd-212">En caso contrario, busque en **HttpConfiguration.ParameterBindingRules** para una función que devuelve un valor no null **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="747fd-212">Otherwise, look in **HttpConfiguration.ParameterBindingRules** for a function that returns a non-null **HttpParameterBinding**.</span></span>
3. <span data-ttu-id="747fd-213">En caso contrario, utilice las reglas predeterminadas que he descrito anteriormente.</span><span class="sxs-lookup"><span data-stu-id="747fd-213">Otherwise, use the default rules that I described previously.</span></span> 

    - <span data-ttu-id="747fd-214">Si el tipo de parámetro es "sencillo" o tiene un convertidor de tipos, enlazar desde el URI.</span><span class="sxs-lookup"><span data-stu-id="747fd-214">If the parameter type is "simple"or has a type converter, bind from the URI.</span></span> <span data-ttu-id="747fd-215">Esto equivale a poner el **[FromUri]** atributo en el parámetro.</span><span class="sxs-lookup"><span data-stu-id="747fd-215">This is equivalent to putting the **[FromUri]** attribute on the parameter.</span></span>
    - <span data-ttu-id="747fd-216">En caso contrario, intenta leer el parámetro del cuerpo del mensaje.</span><span class="sxs-lookup"><span data-stu-id="747fd-216">Otherwise, try to read the parameter from the message body.</span></span> <span data-ttu-id="747fd-217">Esto equivale a poner **[FromBody]** en el parámetro.</span><span class="sxs-lookup"><span data-stu-id="747fd-217">This is equivalent to putting **[FromBody]** on the parameter.</span></span>

<span data-ttu-id="747fd-218">Si deseara, podría reemplazar toda la **IActionValueBinder** servicio con una implementación personalizada.</span><span class="sxs-lookup"><span data-stu-id="747fd-218">If you wanted, you could replace the entire **IActionValueBinder** service with a custom implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="747fd-219">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="747fd-219">Additional Resources</span></span>

[<span data-ttu-id="747fd-220">Ejemplo de enlace de parámetros personalizados</span><span class="sxs-lookup"><span data-stu-id="747fd-220">Custom Parameter Binding Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

<span data-ttu-id="747fd-221">Mike Stall escribió una buena serie de entradas de blog sobre el enlace de parámetro de API Web:</span><span class="sxs-lookup"><span data-stu-id="747fd-221">Mike Stall wrote a good series of blog posts about Web API parameter binding:</span></span>

- [<span data-ttu-id="747fd-222">¿Cómo de API Web de enlace de parámetros</span><span class="sxs-lookup"><span data-stu-id="747fd-222">How Web API does Parameter Binding</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [<span data-ttu-id="747fd-223">Enlace de parámetro de estilo MVC para API Web</span><span class="sxs-lookup"><span data-stu-id="747fd-223">MVC Style parameter binding for Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [<span data-ttu-id="747fd-224">Cómo enlazar a objetos personalizados en las firmas de acción de MVC o Web API</span><span class="sxs-lookup"><span data-stu-id="747fd-224">How to bind to custom objects in action signatures in MVC/Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [<span data-ttu-id="747fd-225">Cómo crear un proveedor de valor personalizado en Web API</span><span class="sxs-lookup"><span data-stu-id="747fd-225">How to create a custom value provider in Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [<span data-ttu-id="747fd-226">Enlace de parámetros de la API Web en segundo plano</span><span class="sxs-lookup"><span data-stu-id="747fd-226">Web API Parameter binding under the hood</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
