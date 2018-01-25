---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Atributo de enrutamiento en ASP.NET Web API 2 | Documentos de Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 173add73a150d3e13ae243d6548463da912dadee
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="16ff6-102">Ruta de atributo en ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="16ff6-102">Attribute Routing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="16ff6-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="16ff6-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="16ff6-104">*Enrutamiento* es la API Web coincide con un URI para una acción.</span><span class="sxs-lookup"><span data-stu-id="16ff6-104">*Routing* is how Web API matches a URI to an action.</span></span> <span data-ttu-id="16ff6-105">Web API 2 es compatible con un nuevo tipo de enrutamiento, denominado *atributo enrutamiento*.</span><span class="sxs-lookup"><span data-stu-id="16ff6-105">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="16ff6-106">Como su nombre indica, enrutamiento de atributo usa atributos para definir las rutas.</span><span class="sxs-lookup"><span data-stu-id="16ff6-106">As the name implies, attribute routing uses attributes to define routes.</span></span> <span data-ttu-id="16ff6-107">Ruta de atributo proporciona mayor control sobre los URI de la API web.</span><span class="sxs-lookup"><span data-stu-id="16ff6-107">Attribute routing gives you more control over the URIs in your web API.</span></span> <span data-ttu-id="16ff6-108">Por ejemplo, puede crear fácilmente los identificadores URI que describen las jerarquías de recursos.</span><span class="sxs-lookup"><span data-stu-id="16ff6-108">For example, you can easily create URIs that describe hierarchies of resources.</span></span>

<span data-ttu-id="16ff6-109">El estilo anterior de enrutamiento, denominado basada en convenciones de enrutamiento, sigue siendo totalmente compatible.</span><span class="sxs-lookup"><span data-stu-id="16ff6-109">The earlier style of routing, called convention-based routing, is still fully supported.</span></span> <span data-ttu-id="16ff6-110">De hecho, puede combinar ambas técnicas en el mismo proyecto.</span><span class="sxs-lookup"><span data-stu-id="16ff6-110">In fact, you can combine both techniques in the same project.</span></span>

<span data-ttu-id="16ff6-111">En este tema se muestra cómo habilitar el enrutamiento de atributo y describe las distintas opciones para el enrutamiento de atributo.</span><span class="sxs-lookup"><span data-stu-id="16ff6-111">This topic shows how to enable attribute routing and describes the various options for attribute routing.</span></span> <span data-ttu-id="16ff6-112">Para ver un tutorial to-end que utiliza el enrutamiento de atributo, vea [crear una API de REST con el atributo de enrutamiento de Web API 2](create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="16ff6-112">For an end-to-end tutorial that uses attribute routing, see [Create a REST API with Attribute Routing in Web API 2](create-a-rest-api-with-attribute-routing.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="16ff6-113">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="16ff6-113">Prerequisites</span></span>

<span data-ttu-id="16ff6-114">[Visual Studio de 2017](https://www.visualstudio.com/vs/) Community, Professional o Enterprise Edition</span><span class="sxs-lookup"><span data-stu-id="16ff6-114">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional, or Enterprise Edition</span></span>

<span data-ttu-id="16ff6-115">Como alternativa, use el Administrador de paquetes de NuGet para instalar los paquetes necesarios.</span><span class="sxs-lookup"><span data-stu-id="16ff6-115">Alternatively, use NuGet Package Manager to install the necessary packages.</span></span> <span data-ttu-id="16ff6-116">Desde el **herramientas** menú en Visual Studio, seleccione **Administrador de paquetes de biblioteca**, a continuación, seleccione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="16ff6-116">From the **Tools** menu in Visual Studio, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="16ff6-117">Escriba el siguiente comando en la ventana de la consola de administrador de paquetes:</span><span class="sxs-lookup"><span data-stu-id="16ff6-117">Enter the following command in the Package Manager Console window:</span></span>

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a><span data-ttu-id="16ff6-118">¿Por qué atributo enrutamiento?</span><span class="sxs-lookup"><span data-stu-id="16ff6-118">Why Attribute Routing?</span></span>

<span data-ttu-id="16ff6-119">La primera versión de API Web que usan *basada en convenciones* enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="16ff6-119">The first release of Web API used *convention-based* routing.</span></span> <span data-ttu-id="16ff6-120">En ese tipo de enrutamiento, puede definir uno o más plantillas de ruta, que básicamente son cadenas de parámetros.</span><span class="sxs-lookup"><span data-stu-id="16ff6-120">In that type of routing, you define one or more route templates, which are basically parameterized strings.</span></span> <span data-ttu-id="16ff6-121">Cuando el marco de trabajo recibe una solicitud, coincide con el URI en la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="16ff6-121">When the framework receives a request, it matches the URI against the route template.</span></span> <span data-ttu-id="16ff6-122">(Para obtener más información sobre el enrutamiento basado en convenciones, vea [enrutamiento de ASP.NET Web API](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="16ff6-122">(For more information about convention-based routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>

<span data-ttu-id="16ff6-123">Una ventaja de enrutamiento basado en la convención es que las plantillas se definen en un único lugar y las reglas de enrutamiento se aplican de forma coherente en todos los controladores.</span><span class="sxs-lookup"><span data-stu-id="16ff6-123">One advantage of convention-based routing is that templates are defined in a single place, and the routing rules are applied consistently across all controllers.</span></span> <span data-ttu-id="16ff6-124">Por desgracia, enrutamiento basado en la convención hace más difícil admitir determinados patrones URI que son comunes en las API de REST.</span><span class="sxs-lookup"><span data-stu-id="16ff6-124">Unfortunately, convention-based routing makes it hard to support certain URI patterns that are common in RESTful APIs.</span></span> <span data-ttu-id="16ff6-125">Por ejemplo, los recursos a menudo contienen recursos secundarios: los clientes tienen pedidos, las películas tienen actores, tienen los autores de libros y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="16ff6-125">For example, resources often contain child resources: Customers have orders, movies have actors, books have authors, and so forth.</span></span> <span data-ttu-id="16ff6-126">Es natural para crear a URI que refleja estas relaciones:</span><span class="sxs-lookup"><span data-stu-id="16ff6-126">It's natural to create URIs that reflect these relations:</span></span>

`/customers/1/orders`

<span data-ttu-id="16ff6-127">Este tipo de URI es difícil crear mediante el enrutamiento basado en la convención.</span><span class="sxs-lookup"><span data-stu-id="16ff6-127">This type of URI is difficult to create using convention-based routing.</span></span> <span data-ttu-id="16ff6-128">Aunque es posible, los resultados no admiten una ampliación también si tiene muchos controladores o tipos de recursos.</span><span class="sxs-lookup"><span data-stu-id="16ff6-128">Although it can be done, the results don't scale well if you have many controllers or resource types.</span></span>

<span data-ttu-id="16ff6-129">Con el enrutamiento de atributo, es trivial para definir una ruta para este URI.</span><span class="sxs-lookup"><span data-stu-id="16ff6-129">With attribute routing, it's trivial to define a route for this URI.</span></span> <span data-ttu-id="16ff6-130">Simplemente agregue un atributo a la acción de controlador:</span><span class="sxs-lookup"><span data-stu-id="16ff6-130">You simply add an attribute to the controller action:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

<span data-ttu-id="16ff6-131">Estos son algunos otros patrones de ese atributo enrutamiento hace fácil.</span><span class="sxs-lookup"><span data-stu-id="16ff6-131">Here are some other patterns that attribute routing makes easy.</span></span>

<span data-ttu-id="16ff6-132">**Control de versiones de API**</span><span class="sxs-lookup"><span data-stu-id="16ff6-132">**API versioning**</span></span>

<span data-ttu-id="16ff6-133">En este ejemplo, "/ api/v1/products" sería enrutado a un controlador diferente que "/ v2/api/productos".</span><span class="sxs-lookup"><span data-stu-id="16ff6-133">In this example, "/api/v1/products" would be routed to a different controller than "/api/v2/products".</span></span>

`/api/v1/products`  
`/api/v2/products`

<span data-ttu-id="16ff6-134">**Segmentos URI sobrecargados**</span><span class="sxs-lookup"><span data-stu-id="16ff6-134">**Overloaded URI segments**</span></span>

<span data-ttu-id="16ff6-135">En este ejemplo, "1" es un número de pedido, pero "pendiente" se asigna a una colección.</span><span class="sxs-lookup"><span data-stu-id="16ff6-135">In this example, "1" is an order number, but "pending" maps to a collection.</span></span>

`/orders/1`  
`/orders/pending`

<span data-ttu-id="16ff6-136">**Varios tipos de parámetro**</span><span class="sxs-lookup"><span data-stu-id="16ff6-136">**Multiple parameter types**</span></span>

<span data-ttu-id="16ff6-137">En este ejemplo, "1" es un número de pedido, pero "2013/06/16" especifica una fecha.</span><span class="sxs-lookup"><span data-stu-id="16ff6-137">In this example, "1" is an order number, but "2013/06/16" specifies a date.</span></span>

`/orders/1`  
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a><span data-ttu-id="16ff6-138">Habilitar el enrutamiento de atributo</span><span class="sxs-lookup"><span data-stu-id="16ff6-138">Enabling Attribute Routing</span></span>

<span data-ttu-id="16ff6-139">Para habilitar el enrutamiento de atributo, llame a **MapHttpAttributeRoutes** durante la configuración.</span><span class="sxs-lookup"><span data-stu-id="16ff6-139">To enable attribute routing, call **MapHttpAttributeRoutes** during configuration.</span></span> <span data-ttu-id="16ff6-140">Este método de extensión se define en el **System.Web.Http.HttpConfigurationExtensions** clase.</span><span class="sxs-lookup"><span data-stu-id="16ff6-140">This extension method is defined in the **System.Web.Http.HttpConfigurationExtensions** class.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

<span data-ttu-id="16ff6-141">Ruta de atributo se puede combinar con [basada en convenciones](routing-in-aspnet-web-api.md) enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="16ff6-141">Attribute routing can be combined with [convention-based](routing-in-aspnet-web-api.md) routing.</span></span> <span data-ttu-id="16ff6-142">Para definir las rutas basada en convenciones, llame a la **MapHttpRoute** método.</span><span class="sxs-lookup"><span data-stu-id="16ff6-142">To define convention-based routes, call the **MapHttpRoute** method.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

<span data-ttu-id="16ff6-143">Para obtener más información sobre la configuración de Web API, consulte [configurar ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="16ff6-143">For more information about configuring Web API, see [Configuring ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span></span>

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a><span data-ttu-id="16ff6-144">Nota: La migración desde la API Web 1</span><span class="sxs-lookup"><span data-stu-id="16ff6-144">Note: Migrating From Web API 1</span></span>

<span data-ttu-id="16ff6-145">Antes de la API Web 2, las plantillas de proyecto de Web API generan código similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="16ff6-145">Prior to Web API 2, the Web API project templates generated code like this:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

<span data-ttu-id="16ff6-146">Si está habilitado el enrutamiento de atributo, este código producirá una excepción.</span><span class="sxs-lookup"><span data-stu-id="16ff6-146">If attribute routing is enabled, this code will throw an exception.</span></span> <span data-ttu-id="16ff6-147">Si actualiza un proyecto de API Web existente para usar el enrutamiento de atributo, asegúrese de actualizar este código de configuración al siguiente:</span><span class="sxs-lookup"><span data-stu-id="16ff6-147">If you upgrade an existing Web API project to use attribute routing, make sure to update this configuration code to the following:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> <span data-ttu-id="16ff6-148">Para obtener más información, consulte [configuración de Web API con hospedaje de ASP.NET](../advanced/configuring-aspnet-web-api.md#webhost).</span><span class="sxs-lookup"><span data-stu-id="16ff6-148">For more information, see [Configuring Web API with ASP.NET Hosting](../advanced/configuring-aspnet-web-api.md#webhost).</span></span>


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a><span data-ttu-id="16ff6-149">Agregar atributos de ruta</span><span class="sxs-lookup"><span data-stu-id="16ff6-149">Adding Route Attributes</span></span>

<span data-ttu-id="16ff6-150">Este es un ejemplo de una ruta que se definen mediante un atributo:</span><span class="sxs-lookup"><span data-stu-id="16ff6-150">Here is an example of a route defined using an attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

<span data-ttu-id="16ff6-151">La cadena &quot;clientes / {customerId} / ordena&quot; es la plantilla URI para la ruta.</span><span class="sxs-lookup"><span data-stu-id="16ff6-151">The string &quot;customers/{customerId}/orders&quot; is the URI template for the route.</span></span> <span data-ttu-id="16ff6-152">API Web intenta hacer coincidir el URI de solicitud a la plantilla.</span><span class="sxs-lookup"><span data-stu-id="16ff6-152">Web API tries to match the request URI to the template.</span></span> <span data-ttu-id="16ff6-153">En este ejemplo, "customers" y "orders" son literales segmentos y "{customerId}" es un parámetro de la variable.</span><span class="sxs-lookup"><span data-stu-id="16ff6-153">In this example, "customers" and "orders" are literal segments, and "{customerId}" is a variable parameter.</span></span> <span data-ttu-id="16ff6-154">Los URI siguientes coincidiría con esta plantilla:</span><span class="sxs-lookup"><span data-stu-id="16ff6-154">The following URIs would match this template:</span></span>

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

<span data-ttu-id="16ff6-155">Puede restringir la búsqueda de coincidencias utilizando [restricciones](#constraints), que se describen más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="16ff6-155">You can restrict the matching by using [constraints](#constraints), described later in this topic.</span></span>

<span data-ttu-id="16ff6-156">Tenga en cuenta que la &quot;{customerId}&quot; parámetro en la plantilla de ruta coincide con el nombre de la *customerId* parámetro del método.</span><span class="sxs-lookup"><span data-stu-id="16ff6-156">Notice that the &quot;{customerId}&quot; parameter in the route template matches the name of the *customerId* parameter in the method.</span></span> <span data-ttu-id="16ff6-157">Cuando la API Web, se invoca la acción de controlador, intenta enlazar los parámetros de ruta.</span><span class="sxs-lookup"><span data-stu-id="16ff6-157">When Web API invokes the controller action, it tries to bind the route parameters.</span></span> <span data-ttu-id="16ff6-158">Por ejemplo, si el identificador URI es `http://example.com/customers/1/orders`, API Web intenta enlazar el valor "1" para el *customerId* parámetro en la acción.</span><span class="sxs-lookup"><span data-stu-id="16ff6-158">For example, if the URI is `http://example.com/customers/1/orders`, Web API tries to bind the value "1" to the *customerId* parameter in the action.</span></span>

<span data-ttu-id="16ff6-159">Una plantilla URI puede tener varios parámetros:</span><span class="sxs-lookup"><span data-stu-id="16ff6-159">A URI template can have several parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

<span data-ttu-id="16ff6-160">Los métodos de controlador que no tiene un atributo de ruta usan enrutamiento basado en la convención.</span><span class="sxs-lookup"><span data-stu-id="16ff6-160">Any controller methods that do not have a route attribute use convention-based routing.</span></span> <span data-ttu-id="16ff6-161">De este modo, puede combinar ambos tipos de enrutamiento en el mismo proyecto.</span><span class="sxs-lookup"><span data-stu-id="16ff6-161">That way, you can combine both types of routing in the same project.</span></span>

## <a name="http-methods"></a><span data-ttu-id="16ff6-162">Métodos HTTP</span><span class="sxs-lookup"><span data-stu-id="16ff6-162">HTTP Methods</span></span>

<span data-ttu-id="16ff6-163">API Web también selecciona acciones basadas en el método HTTP de la solicitud (GET, POST, etcetera).</span><span class="sxs-lookup"><span data-stu-id="16ff6-163">Web API also selects actions based on the HTTP method of the request (GET, POST, etc).</span></span> <span data-ttu-id="16ff6-164">De forma predeterminada, la API Web busca una coincidencia entre mayúsculas y minúsculas con el inicio del nombre del método de controlador.</span><span class="sxs-lookup"><span data-stu-id="16ff6-164">By default, Web API looks for a case-insensitive match with the start of the controller method name.</span></span> <span data-ttu-id="16ff6-165">Por ejemplo, un método de controlador denominado `PutCustomers` coincide con una solicitud PUT de HTTP.</span><span class="sxs-lookup"><span data-stu-id="16ff6-165">For example, a controller method named `PutCustomers` matches an HTTP PUT request.</span></span>

<span data-ttu-id="16ff6-166">Puede invalidar esta convención decorando el método con cualquiera de los siguientes atributos:</span><span class="sxs-lookup"><span data-stu-id="16ff6-166">You can override this convention by decorating the method with any the following attributes:</span></span>

- <span data-ttu-id="16ff6-167">**[HttpDelete]**</span><span class="sxs-lookup"><span data-stu-id="16ff6-167">**[HttpDelete]**</span></span>
- <span data-ttu-id="16ff6-168">**[HttpGet]**</span><span class="sxs-lookup"><span data-stu-id="16ff6-168">**[HttpGet]**</span></span>
- <span data-ttu-id="16ff6-169">**[HttpHead]**</span><span class="sxs-lookup"><span data-stu-id="16ff6-169">**[HttpHead]**</span></span>
- <span data-ttu-id="16ff6-170">**[HttpOptions]**</span><span class="sxs-lookup"><span data-stu-id="16ff6-170">**[HttpOptions]**</span></span>
- <span data-ttu-id="16ff6-171">**[HttpPatch]**</span><span class="sxs-lookup"><span data-stu-id="16ff6-171">**[HttpPatch]**</span></span>
- <span data-ttu-id="16ff6-172">**[HttpPost]**</span><span class="sxs-lookup"><span data-stu-id="16ff6-172">**[HttpPost]**</span></span>
- <span data-ttu-id="16ff6-173">**[HttpPut]**</span><span class="sxs-lookup"><span data-stu-id="16ff6-173">**[HttpPut]**</span></span>

<span data-ttu-id="16ff6-174">En el ejemplo siguiente se asigna el método CreateBook a solicitudes HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="16ff6-174">The following example maps the CreateBook method to HTTP POST requests.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

<span data-ttu-id="16ff6-175">Para todos los demás métodos HTTP, incluidos los métodos no estándares, utilicen la **AcceptVerbs** atributo, que toma una lista de métodos HTTP.</span><span class="sxs-lookup"><span data-stu-id="16ff6-175">For all other HTTP methods, including non-standard methods, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a><span data-ttu-id="16ff6-176">Prefijos de rutas</span><span class="sxs-lookup"><span data-stu-id="16ff6-176">Route Prefixes</span></span>

<span data-ttu-id="16ff6-177">A menudo, las rutas en un controlador todas comienzan por el mismo prefijo.</span><span class="sxs-lookup"><span data-stu-id="16ff6-177">Often, the routes in a controller all start with the same prefix.</span></span> <span data-ttu-id="16ff6-178">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="16ff6-178">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

<span data-ttu-id="16ff6-179">Puede establecer un prefijo común para un controlador de todo mediante el **[RoutePrefix]** atributo:</span><span class="sxs-lookup"><span data-stu-id="16ff6-179">You can set a common prefix for an entire controller by using the **[RoutePrefix]** attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

<span data-ttu-id="16ff6-180">Usar una tilde (~) en el atributo de método para invalidar el prefijo de ruta:</span><span class="sxs-lookup"><span data-stu-id="16ff6-180">Use a tilde (~) on the method attribute to override the route prefix:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

<span data-ttu-id="16ff6-181">El prefijo de ruta puede incluir parámetros:</span><span class="sxs-lookup"><span data-stu-id="16ff6-181">The route prefix can include parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a><span data-ttu-id="16ff6-182">Restricciones de la ruta</span><span class="sxs-lookup"><span data-stu-id="16ff6-182">Route Constraints</span></span>

<span data-ttu-id="16ff6-183">Restricciones de la ruta le permiten restringir cómo coinciden los parámetros en la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="16ff6-183">Route constraints let you restrict how the parameters in the route template are matched.</span></span> <span data-ttu-id="16ff6-184">La sintaxis general es &quot;{parámetro: restricción}&quot;.</span><span class="sxs-lookup"><span data-stu-id="16ff6-184">The general syntax is &quot;{parameter:constraint}&quot;.</span></span> <span data-ttu-id="16ff6-185">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="16ff6-185">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

<span data-ttu-id="16ff6-186">En este caso, la primera ruta sólo se puede seleccionar si el &quot;identificador&quot; segmento del URI es un entero.</span><span class="sxs-lookup"><span data-stu-id="16ff6-186">Here, the first route will only be selected if the &quot;id&quot; segment of the URI is an integer.</span></span> <span data-ttu-id="16ff6-187">En caso contrario, se elegirá la segunda ruta.</span><span class="sxs-lookup"><span data-stu-id="16ff6-187">Otherwise, the second route will be chosen.</span></span>

<span data-ttu-id="16ff6-188">En la tabla siguiente enumera las restricciones que se admiten.</span><span class="sxs-lookup"><span data-stu-id="16ff6-188">The following table lists the constraints that are supported.</span></span>

| <span data-ttu-id="16ff6-189">Restricción</span><span class="sxs-lookup"><span data-stu-id="16ff6-189">Constraint</span></span> | <span data-ttu-id="16ff6-190">Descripción</span><span class="sxs-lookup"><span data-stu-id="16ff6-190">Description</span></span> | <span data-ttu-id="16ff6-191">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="16ff6-191">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="16ff6-192">Alfa</span><span class="sxs-lookup"><span data-stu-id="16ff6-192">alpha</span></span> | <span data-ttu-id="16ff6-193">Coincidencias en mayúsculas o minúsculas caracteres del alfabeto latino (a – z, a Z)</span><span class="sxs-lookup"><span data-stu-id="16ff6-193">Matches uppercase or lowercase Latin alphabet characters (a-z, A-Z)</span></span> | <span data-ttu-id="16ff6-194">{x:alpha}</span><span class="sxs-lookup"><span data-stu-id="16ff6-194">{x:alpha}</span></span> |
| <span data-ttu-id="16ff6-195">bool</span><span class="sxs-lookup"><span data-stu-id="16ff6-195">bool</span></span> | <span data-ttu-id="16ff6-196">Coincide con un valor booleano.</span><span class="sxs-lookup"><span data-stu-id="16ff6-196">Matches a Boolean value.</span></span> | <span data-ttu-id="16ff6-197">{x:bool}</span><span class="sxs-lookup"><span data-stu-id="16ff6-197">{x:bool}</span></span> |
| <span data-ttu-id="16ff6-198">datetime</span><span class="sxs-lookup"><span data-stu-id="16ff6-198">datetime</span></span> | <span data-ttu-id="16ff6-199">Coincide con un **DateTime** valor.</span><span class="sxs-lookup"><span data-stu-id="16ff6-199">Matches a **DateTime** value.</span></span> | <span data-ttu-id="16ff6-200">{x:datetime}</span><span class="sxs-lookup"><span data-stu-id="16ff6-200">{x:datetime}</span></span> |
| <span data-ttu-id="16ff6-201">decimal</span><span class="sxs-lookup"><span data-stu-id="16ff6-201">decimal</span></span> | <span data-ttu-id="16ff6-202">Coincide con un valor decimal.</span><span class="sxs-lookup"><span data-stu-id="16ff6-202">Matches a decimal value.</span></span> | <span data-ttu-id="16ff6-203">{x:decimal}</span><span class="sxs-lookup"><span data-stu-id="16ff6-203">{x:decimal}</span></span> |
| <span data-ttu-id="16ff6-204">double</span><span class="sxs-lookup"><span data-stu-id="16ff6-204">double</span></span> | <span data-ttu-id="16ff6-205">Coincide con un valor de punto flotante de 64 bits.</span><span class="sxs-lookup"><span data-stu-id="16ff6-205">Matches a 64-bit floating-point value.</span></span> | <span data-ttu-id="16ff6-206">{x:double}</span><span class="sxs-lookup"><span data-stu-id="16ff6-206">{x:double}</span></span> |
| <span data-ttu-id="16ff6-207">float</span><span class="sxs-lookup"><span data-stu-id="16ff6-207">float</span></span> | <span data-ttu-id="16ff6-208">Coincide con un valor de punto flotante de 32 bits.</span><span class="sxs-lookup"><span data-stu-id="16ff6-208">Matches a 32-bit floating-point value.</span></span> | <span data-ttu-id="16ff6-209">{x:float}</span><span class="sxs-lookup"><span data-stu-id="16ff6-209">{x:float}</span></span> |
| <span data-ttu-id="16ff6-210">guid</span><span class="sxs-lookup"><span data-stu-id="16ff6-210">guid</span></span> | <span data-ttu-id="16ff6-211">Coincide con un valor GUID.</span><span class="sxs-lookup"><span data-stu-id="16ff6-211">Matches a GUID value.</span></span> | <span data-ttu-id="16ff6-212">{x:guid}</span><span class="sxs-lookup"><span data-stu-id="16ff6-212">{x:guid}</span></span> |
| <span data-ttu-id="16ff6-213">int</span><span class="sxs-lookup"><span data-stu-id="16ff6-213">int</span></span> | <span data-ttu-id="16ff6-214">Coincide con un valor entero de 32 bits.</span><span class="sxs-lookup"><span data-stu-id="16ff6-214">Matches a 32-bit integer value.</span></span> | <span data-ttu-id="16ff6-215">{x: int}</span><span class="sxs-lookup"><span data-stu-id="16ff6-215">{x:int}</span></span> |
| <span data-ttu-id="16ff6-216">longitud</span><span class="sxs-lookup"><span data-stu-id="16ff6-216">length</span></span> | <span data-ttu-id="16ff6-217">Coincide con una cadena con la longitud especificada o dentro de un intervalo especificado de longitudes.</span><span class="sxs-lookup"><span data-stu-id="16ff6-217">Matches a string with the specified length or within a specified range of lengths.</span></span> | <span data-ttu-id="16ff6-218">{x: length(6)} {x: length(1,20)}</span><span class="sxs-lookup"><span data-stu-id="16ff6-218">{x:length(6)} {x:length(1,20)}</span></span> |
| <span data-ttu-id="16ff6-219">long</span><span class="sxs-lookup"><span data-stu-id="16ff6-219">long</span></span> | <span data-ttu-id="16ff6-220">Coincide con un valor entero de 64 bits.</span><span class="sxs-lookup"><span data-stu-id="16ff6-220">Matches a 64-bit integer value.</span></span> | <span data-ttu-id="16ff6-221">{x:long}</span><span class="sxs-lookup"><span data-stu-id="16ff6-221">{x:long}</span></span> |
| <span data-ttu-id="16ff6-222">max</span><span class="sxs-lookup"><span data-stu-id="16ff6-222">max</span></span> | <span data-ttu-id="16ff6-223">Devuelve un entero con un valor máximo.</span><span class="sxs-lookup"><span data-stu-id="16ff6-223">Matches an integer with a maximum value.</span></span> | <span data-ttu-id="16ff6-224">{x:max(10)}</span><span class="sxs-lookup"><span data-stu-id="16ff6-224">{x:max(10)}</span></span> |
| <span data-ttu-id="16ff6-225">MaxLength</span><span class="sxs-lookup"><span data-stu-id="16ff6-225">maxlength</span></span> | <span data-ttu-id="16ff6-226">Coincide con una cadena con una longitud máxima.</span><span class="sxs-lookup"><span data-stu-id="16ff6-226">Matches a string with a maximum length.</span></span> | <span data-ttu-id="16ff6-227">{x:maxlength(10)}</span><span class="sxs-lookup"><span data-stu-id="16ff6-227">{x:maxlength(10)}</span></span> |
| <span data-ttu-id="16ff6-228">min</span><span class="sxs-lookup"><span data-stu-id="16ff6-228">min</span></span> | <span data-ttu-id="16ff6-229">Devuelve un entero con un valor mínimo.</span><span class="sxs-lookup"><span data-stu-id="16ff6-229">Matches an integer with a minimum value.</span></span> | <span data-ttu-id="16ff6-230">{x:min(10)}</span><span class="sxs-lookup"><span data-stu-id="16ff6-230">{x:min(10)}</span></span> |
| <span data-ttu-id="16ff6-231">minLength</span><span class="sxs-lookup"><span data-stu-id="16ff6-231">minlength</span></span> | <span data-ttu-id="16ff6-232">Coincide con una cadena con una longitud mínima.</span><span class="sxs-lookup"><span data-stu-id="16ff6-232">Matches a string with a minimum length.</span></span> | <span data-ttu-id="16ff6-233">{x:minlength(10)}</span><span class="sxs-lookup"><span data-stu-id="16ff6-233">{x:minlength(10)}</span></span> |
| <span data-ttu-id="16ff6-234">range</span><span class="sxs-lookup"><span data-stu-id="16ff6-234">range</span></span> | <span data-ttu-id="16ff6-235">Devuelve un número entero dentro de un intervalo de valores.</span><span class="sxs-lookup"><span data-stu-id="16ff6-235">Matches an integer within a range of values.</span></span> | <span data-ttu-id="16ff6-236">{x: range(10,50)}</span><span class="sxs-lookup"><span data-stu-id="16ff6-236">{x:range(10,50)}</span></span> |
| <span data-ttu-id="16ff6-237">regex</span><span class="sxs-lookup"><span data-stu-id="16ff6-237">regex</span></span> | <span data-ttu-id="16ff6-238">Coincide con una expresión regular.</span><span class="sxs-lookup"><span data-stu-id="16ff6-238">Matches a regular expression.</span></span> | <span data-ttu-id="16ff6-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span><span class="sxs-lookup"><span data-stu-id="16ff6-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span></span> |

<span data-ttu-id="16ff6-240">Observe que algunas de las restricciones, como &quot;min&quot;, toman argumentos entre paréntesis.</span><span class="sxs-lookup"><span data-stu-id="16ff6-240">Notice that some of the constraints, such as &quot;min&quot;, take arguments in parentheses.</span></span> <span data-ttu-id="16ff6-241">Puede aplicar varias restricciones a un parámetro, separado por un coma.</span><span class="sxs-lookup"><span data-stu-id="16ff6-241">You can apply multiple constraints to a parameter, separated by a colon.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a><span data-ttu-id="16ff6-242">Restricciones de la ruta personalizada</span><span class="sxs-lookup"><span data-stu-id="16ff6-242">Custom Route Constraints</span></span>

<span data-ttu-id="16ff6-243">Puede crear restricciones de la ruta personalizada mediante la implementación de la **IHttpRouteConstraint** interfaz.</span><span class="sxs-lookup"><span data-stu-id="16ff6-243">You can create custom route constraints by implementing the **IHttpRouteConstraint** interface.</span></span> <span data-ttu-id="16ff6-244">Por ejemplo, la restricción siguiente restringe un parámetro a un valor entero distinto de cero.</span><span class="sxs-lookup"><span data-stu-id="16ff6-244">For example, the following constraint restricts a parameter to a non-zero integer value.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

<span data-ttu-id="16ff6-245">El código siguiente muestra cómo registrar la restricción:</span><span class="sxs-lookup"><span data-stu-id="16ff6-245">The following code shows how to register the constraint:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

<span data-ttu-id="16ff6-246">Ahora puede aplicar la restricción en sus rutas:</span><span class="sxs-lookup"><span data-stu-id="16ff6-246">Now you can apply the constraint in your routes:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

<span data-ttu-id="16ff6-247">También puede reemplazar toda la **DefaultInlineConstraintResolver** clase implementando la **IInlineConstraintResolver** interfaz.</span><span class="sxs-lookup"><span data-stu-id="16ff6-247">You can also replace the entire **DefaultInlineConstraintResolver** class by implementing the **IInlineConstraintResolver** interface.</span></span> <span data-ttu-id="16ff6-248">Si lo hace, reemplazará todas las restricciones integradas, a menos que la implementación de **IInlineConstraintResolver** agrega específicamente.</span><span class="sxs-lookup"><span data-stu-id="16ff6-248">Doing so will replace all of the built-in constraints, unless your implementation of **IInlineConstraintResolver** specifically adds them.</span></span>

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a><span data-ttu-id="16ff6-249">Parámetros de URI opcional y los valores predeterminados</span><span class="sxs-lookup"><span data-stu-id="16ff6-249">Optional URI Parameters and Default Values</span></span>

<span data-ttu-id="16ff6-250">Puede hacer que un parámetro URI opcional mediante la adición de un signo de interrogación para el parámetro de ruta.</span><span class="sxs-lookup"><span data-stu-id="16ff6-250">You can make a URI parameter optional by adding a question mark to the route parameter.</span></span> <span data-ttu-id="16ff6-251">Si un parámetro de ruta es opcional, debe definir un valor predeterminado para el parámetro de método.</span><span class="sxs-lookup"><span data-stu-id="16ff6-251">If a route parameter is optional, you must define a default value for the method parameter.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

<span data-ttu-id="16ff6-252">En este ejemplo, `/api/books/locale/1033` y `/api/books/locale` devuelven el mismo recurso.</span><span class="sxs-lookup"><span data-stu-id="16ff6-252">In this example, `/api/books/locale/1033` and `/api/books/locale` return the same resource.</span></span>

<span data-ttu-id="16ff6-253">Como alternativa, puede especificar un valor predeterminado dentro de la plantilla de ruta, como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="16ff6-253">Alternatively, you can specify a default value inside the route template, as follows:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

<span data-ttu-id="16ff6-254">Esto es prácticamente el mismo que el ejemplo anterior, pero hay una ligera diferencia de comportamiento cuando se aplica el valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="16ff6-254">This is almost the same as the previous example, but there is a slight difference of behavior when the default value is applied.</span></span>

- <span data-ttu-id="16ff6-255">En el primer ejemplo ("{lcid?}"), el valor predeterminado de 1033 se asigna directamente al parámetro de método, por lo que el parámetro tendrán este valor exacto.</span><span class="sxs-lookup"><span data-stu-id="16ff6-255">In the first example ("{lcid?}"), the default value of 1033 is assigned directly to the method parameter, so the parameter will have this exact value.</span></span>
- <span data-ttu-id="16ff6-256">En el segundo ejemplo ("{lcid = 1033}"), el valor predeterminado de "1033" lleva a cabo el proceso de enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="16ff6-256">In the second example ("{lcid=1033}"), the default value of "1033" goes through the model-binding process.</span></span> <span data-ttu-id="16ff6-257">El enlazador de modelos predeterminado convertirá "1033" en el valor numérico 1033.</span><span class="sxs-lookup"><span data-stu-id="16ff6-257">The default model-binder will convert "1033" to the numeric value 1033.</span></span> <span data-ttu-id="16ff6-258">Sin embargo, puede conectar un enlazador de modelos personalizado, que puede hacer algo diferente.</span><span class="sxs-lookup"><span data-stu-id="16ff6-258">However, you could plug in a custom model binder, which might do something different.</span></span>

<span data-ttu-id="16ff6-259">(En la mayoría de los casos, a menos que tenga los enlazadores de modelos personalizados en la canalización, los dos formularios será equivalente.)</span><span class="sxs-lookup"><span data-stu-id="16ff6-259">(In most cases, unless you have custom model binders in your pipeline, the two forms will be equivalent.)</span></span>

<a id="route-names"></a>
## <a name="route-names"></a><span data-ttu-id="16ff6-260">Nombres de ruta</span><span class="sxs-lookup"><span data-stu-id="16ff6-260">Route Names</span></span>

<span data-ttu-id="16ff6-261">En la API de Web, cada ruta tiene un nombre.</span><span class="sxs-lookup"><span data-stu-id="16ff6-261">In Web API, every route has a name.</span></span> <span data-ttu-id="16ff6-262">Los nombres de ruta son útiles para generar vínculos, por lo que puede incluir un vínculo en una respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="16ff6-262">Route names are useful for generating links, so that you can include a link in an HTTP response.</span></span>

<span data-ttu-id="16ff6-263">Para especificar el nombre de ruta, establezca la **nombre** propiedad del atributo.</span><span class="sxs-lookup"><span data-stu-id="16ff6-263">To specify the route name, set the **Name** property on the attribute.</span></span> <span data-ttu-id="16ff6-264">En el ejemplo siguiente se muestra cómo establecer el nombre de ruta y también cómo usar el nombre de ruta cuando se genera un vínculo.</span><span class="sxs-lookup"><span data-stu-id="16ff6-264">The following example shows how to set the route name, and also how to use the route name when generating a link.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a><span data-ttu-id="16ff6-265">Orden de la ruta</span><span class="sxs-lookup"><span data-stu-id="16ff6-265">Route Order</span></span>

<span data-ttu-id="16ff6-266">Cuando el marco de trabajo intenta hacer coincidir un URI con una ruta, evalúa las rutas en un orden concreto.</span><span class="sxs-lookup"><span data-stu-id="16ff6-266">When the framework tries to match a URI with a route, it evaluates the routes in a particular order.</span></span> <span data-ttu-id="16ff6-267">Para especificar el orden, establezca el **RouteOrder** propiedad en el atributo de ruta.</span><span class="sxs-lookup"><span data-stu-id="16ff6-267">To specify the order, set the **RouteOrder** property on the route attribute.</span></span> <span data-ttu-id="16ff6-268">Los valores más bajos se evalúan primero.</span><span class="sxs-lookup"><span data-stu-id="16ff6-268">Lower values are evaluated first.</span></span> <span data-ttu-id="16ff6-269">El valor de orden predeterminado es cero.</span><span class="sxs-lookup"><span data-stu-id="16ff6-269">The default order value is zero.</span></span>

<span data-ttu-id="16ff6-270">Aquí es cómo se determina el orden total:</span><span class="sxs-lookup"><span data-stu-id="16ff6-270">Here is how the total ordering is determined:</span></span>

1. <span data-ttu-id="16ff6-271">Comparar el **RouteOrder** propiedad del atributo de ruta.</span><span class="sxs-lookup"><span data-stu-id="16ff6-271">Compare the **RouteOrder** property of the route attribute.</span></span>
2. <span data-ttu-id="16ff6-272">Mire cada segmento URI de la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="16ff6-272">Look at each URI segment in the route template.</span></span> <span data-ttu-id="16ff6-273">Para cada segmento, ordenar como sigue:</span><span class="sxs-lookup"><span data-stu-id="16ff6-273">For each segment, order as follows:</span></span> 

    1. <span data-ttu-id="16ff6-274">Segmentos de literales.</span><span class="sxs-lookup"><span data-stu-id="16ff6-274">Literal segments.</span></span>
    2. <span data-ttu-id="16ff6-275">Parámetros de ruta con restricciones.</span><span class="sxs-lookup"><span data-stu-id="16ff6-275">Route parameters with constraints.</span></span>
    3. <span data-ttu-id="16ff6-276">Parámetros de ruta sin restricciones.</span><span class="sxs-lookup"><span data-stu-id="16ff6-276">Route parameters without constraints.</span></span>
    4. <span data-ttu-id="16ff6-277">Segmentos de parámetro de carácter comodín con restricciones.</span><span class="sxs-lookup"><span data-stu-id="16ff6-277">Wildcard parameter segments with constraints.</span></span>
    5. <span data-ttu-id="16ff6-278">Segmentos de parámetro de carácter comodín sin restricciones.</span><span class="sxs-lookup"><span data-stu-id="16ff6-278">Wildcard parameter segments without constraints.</span></span>
3. <span data-ttu-id="16ff6-279">En el caso de empate, las rutas se ordenan por una comparación de cadenas ordinales entre mayúsculas y minúsculas ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) de la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="16ff6-279">In the case of a tie, routes are ordered by a case-insensitive ordinal string comparison ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) of the route template.</span></span>

<span data-ttu-id="16ff6-280">A continuación se muestra un ejemplo.</span><span class="sxs-lookup"><span data-stu-id="16ff6-280">Here is an example.</span></span> <span data-ttu-id="16ff6-281">Supongamos que define el siguiente controlador:</span><span class="sxs-lookup"><span data-stu-id="16ff6-281">Suppose you define the following controller:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

<span data-ttu-id="16ff6-282">Estas rutas se ordenan como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="16ff6-282">These routes are ordered as follows.</span></span>

1. <span data-ttu-id="16ff6-283">orders/details</span><span class="sxs-lookup"><span data-stu-id="16ff6-283">orders/details</span></span>
2. <span data-ttu-id="16ff6-284">pedidos / {id}</span><span class="sxs-lookup"><span data-stu-id="16ff6-284">orders/{id}</span></span>
3. <span data-ttu-id="16ff6-285">orders/{customerName}</span><span class="sxs-lookup"><span data-stu-id="16ff6-285">orders/{customerName}</span></span>
4. <span data-ttu-id="16ff6-286">orders/{\*date}</span><span class="sxs-lookup"><span data-stu-id="16ff6-286">orders/{\*date}</span></span>
5. <span data-ttu-id="16ff6-287">pedidos / pendiente</span><span class="sxs-lookup"><span data-stu-id="16ff6-287">orders/pending</span></span>

<span data-ttu-id="16ff6-288">Observe que "Detalles" es un segmento literal y aparece antes que "{id}", pero "pendiente" aparece última porque la **RouteOrder** propiedad es 1.</span><span class="sxs-lookup"><span data-stu-id="16ff6-288">Notice that "details" is a literal segment and appears before "{id}", but "pending" appears last because the **RouteOrder** property is 1.</span></span> <span data-ttu-id="16ff6-289">(En este ejemplo se supone que hay es ningún cliente denominada "Detalles" o "pendiente".</span><span class="sxs-lookup"><span data-stu-id="16ff6-289">(This example assumes there are no customers named "details" or "pending".</span></span> <span data-ttu-id="16ff6-290">En general, intente evitar las rutas ambiguas.</span><span class="sxs-lookup"><span data-stu-id="16ff6-290">In general, try to avoid ambiguous routes.</span></span> <span data-ttu-id="16ff6-291">En este ejemplo, una plantilla de ruta mejor para `GetByCustomer` es "clientes / {NombreCliente}")</span><span class="sxs-lookup"><span data-stu-id="16ff6-291">In this example, a better route template for `GetByCustomer` is "customers/{customerName}" )</span></span>
