---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Enrutamiento y selección de acción en ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 12/14/2018
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: ce54181996376cb5dde3b91c10c16f33b3c6a570
ms.sourcegitcommit: 6548c19f345850ee22b50f7ef9fca732895d9e08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2018
ms.locfileid: "53425177"
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a><span data-ttu-id="93729-102">Enrutamiento y selección de acción en ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="93729-102">Routing and Action Selection in ASP.NET Web API</span></span>
====================
<span data-ttu-id="93729-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="93729-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="93729-104">En este artículo se describe cómo ASP.NET Web API enruta una solicitud HTTP a una acción determinada en un controlador.</span><span class="sxs-lookup"><span data-stu-id="93729-104">This article describes how ASP.NET Web API routes an HTTP request to a particular action on a controller.</span></span>

> [!NOTE]
> <span data-ttu-id="93729-105">Para obtener una visión general del enrutamiento, consulte [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="93729-105">For a high-level overview of routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>


<span data-ttu-id="93729-106">Este artículo examina los detalles del proceso de enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="93729-106">This article looks at the details of the routing process.</span></span> <span data-ttu-id="93729-107">Si crea un proyecto Web API y búsqueda que no hagan algunas solicitudes enruta según que lo previsto, espero que este artículo le ayude.</span><span class="sxs-lookup"><span data-stu-id="93729-107">If you create a Web API project and find that some requests don't get routed the way you expect, hopefully this article will help.</span></span>

<span data-ttu-id="93729-108">Enrutamiento tiene tres fases principales:</span><span class="sxs-lookup"><span data-stu-id="93729-108">Routing has three main phases:</span></span>

1. <span data-ttu-id="93729-109">Coincide con el URI para una plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="93729-109">Matching the URI to a route template.</span></span>
2. <span data-ttu-id="93729-110">Al seleccionar un controlador.</span><span class="sxs-lookup"><span data-stu-id="93729-110">Selecting a controller.</span></span>
3. <span data-ttu-id="93729-111">Seleccione una acción.</span><span class="sxs-lookup"><span data-stu-id="93729-111">Selecting an action.</span></span>

<span data-ttu-id="93729-112">Puede reemplazar algunas partes del proceso con sus propios comportamientos personalizados.</span><span class="sxs-lookup"><span data-stu-id="93729-112">You can replace some parts of the process with your own custom behaviors.</span></span> <span data-ttu-id="93729-113">En este artículo, describiré el comportamiento predeterminado.</span><span class="sxs-lookup"><span data-stu-id="93729-113">In this article, I describe the default behavior.</span></span> <span data-ttu-id="93729-114">Al final, tenga en cuenta los lugares donde puede personalizar el comportamiento.</span><span class="sxs-lookup"><span data-stu-id="93729-114">At the end, I note the places where you can customize the behavior.</span></span>

## <a name="route-templates"></a><span data-ttu-id="93729-115">Plantillas de ruta</span><span class="sxs-lookup"><span data-stu-id="93729-115">Route Templates</span></span>

<span data-ttu-id="93729-116">Una plantilla de ruta es similar a una ruta de acceso URI, pero puede tener valores de marcador de posición que se indica con llaves:</span><span class="sxs-lookup"><span data-stu-id="93729-116">A route template looks similar to a URI path, but it can have placeholder values, indicated with curly braces:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

<span data-ttu-id="93729-117">Cuando se crea una ruta, puede proporcionar valores predeterminados para algunos o todos los marcadores de posición:</span><span class="sxs-lookup"><span data-stu-id="93729-117">When you create a route, you can provide default values for some or all of the placeholders:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

<span data-ttu-id="93729-118">También puede proporcionar restricciones, que limiten cómo un segmento de URI puede coincidir con un marcador de posición:</span><span class="sxs-lookup"><span data-stu-id="93729-118">You can also provide constraints, which restrict how a URI segment can match a placeholder:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

<span data-ttu-id="93729-119">El marco de trabajo intenta hacer coincidir los segmentos en la ruta de acceso URI a la plantilla.</span><span class="sxs-lookup"><span data-stu-id="93729-119">The framework tries to match the segments in the URI path to the template.</span></span> <span data-ttu-id="93729-120">En la plantilla literales deben coincidir exactamente.</span><span class="sxs-lookup"><span data-stu-id="93729-120">Literals in the template must match exactly.</span></span> <span data-ttu-id="93729-121">Un marcador de posición coincide con algún valor, a menos que especifique las restricciones.</span><span class="sxs-lookup"><span data-stu-id="93729-121">A placeholder matches any value, unless you specify constraints.</span></span> <span data-ttu-id="93729-122">El marco de trabajo no coincide con otras partes del URI, como el nombre de host o los parámetros de consulta.</span><span class="sxs-lookup"><span data-stu-id="93729-122">The framework does not match other parts of the URI, such as the host name or the query parameters.</span></span> <span data-ttu-id="93729-123">El marco de trabajo, selecciona la primera ruta en la tabla de ruta que coincida con el identificador URI.</span><span class="sxs-lookup"><span data-stu-id="93729-123">The framework selects the first route in the route table that matches the URI.</span></span>

<span data-ttu-id="93729-124">Hay dos marcadores de posición especiales: "{controller}" y "{action}".</span><span class="sxs-lookup"><span data-stu-id="93729-124">There are two special placeholders: "{controller}" and "{action}".</span></span>

- <span data-ttu-id="93729-125">"{controller}" proporciona el nombre del controlador.</span><span class="sxs-lookup"><span data-stu-id="93729-125">"{controller}" provides the name of the controller.</span></span>
- <span data-ttu-id="93729-126">"{action}" proporciona el nombre de la acción.</span><span class="sxs-lookup"><span data-stu-id="93729-126">"{action}" provides the name of the action.</span></span> <span data-ttu-id="93729-127">En la API Web, la convención habitual es omitir "{action}".</span><span class="sxs-lookup"><span data-stu-id="93729-127">In Web API, the usual convention is to omit "{action}".</span></span>

### <a name="defaults"></a><span data-ttu-id="93729-128">del parte de horas</span><span class="sxs-lookup"><span data-stu-id="93729-128">Defaults</span></span>

<span data-ttu-id="93729-129">Si proporciona valores predeterminados, la ruta se corresponderá con un URI que le falta esos segmentos.</span><span class="sxs-lookup"><span data-stu-id="93729-129">If you provide defaults, the route will match a URI that is missing those segments.</span></span> <span data-ttu-id="93729-130">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="93729-130">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

<span data-ttu-id="93729-131">Los URI `http://localhost/api/products/all` y `http://localhost/api/products` coinciden con la ruta anterior.</span><span class="sxs-lookup"><span data-stu-id="93729-131">The URIs `http://localhost/api/products/all` and `http://localhost/api/products` match the preceding route.</span></span> <span data-ttu-id="93729-132">En el URI de este último, la falta `{category}` segmento se asigna el valor predeterminado `all`.</span><span class="sxs-lookup"><span data-stu-id="93729-132">In the latter URI, the missing `{category}` segment is assigned the default value `all`.</span></span>

### <a name="route-dictionary"></a><span data-ttu-id="93729-133">Diccionario de ruta</span><span class="sxs-lookup"><span data-stu-id="93729-133">Route Dictionary</span></span>

<span data-ttu-id="93729-134">Si el marco de trabajo encuentra a una coincidencia para un URI, crea un diccionario que contiene el valor para cada marcador de posición.</span><span class="sxs-lookup"><span data-stu-id="93729-134">If the framework finds a match for a URI, it creates a dictionary that contains the value for each placeholder.</span></span> <span data-ttu-id="93729-135">Las claves son los nombres de marcador de posición, no se incluye entre llaves.</span><span class="sxs-lookup"><span data-stu-id="93729-135">The keys are the placeholder names, not including the curly braces.</span></span> <span data-ttu-id="93729-136">Los valores se toman de la ruta de acceso URI o de los valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="93729-136">The values are taken from the URI path or from the defaults.</span></span> <span data-ttu-id="93729-137">El diccionario se almacena en el **IHttpRouteData** objeto.</span><span class="sxs-lookup"><span data-stu-id="93729-137">The dictionary is stored in the **IHttpRouteData** object.</span></span>

<span data-ttu-id="93729-138">Durante esta fase de coincidencia de ruta, el especial "{controller}" y los marcadores de posición "{action}" se tratan igual que los otros marcadores de posición.</span><span class="sxs-lookup"><span data-stu-id="93729-138">During this route-matching phase, the special "{controller}" and "{action}" placeholders are treated just like the other placeholders.</span></span> <span data-ttu-id="93729-139">Simplemente se almacenan en el diccionario con los demás valores.</span><span class="sxs-lookup"><span data-stu-id="93729-139">They are simply stored in the dictionary with the other values.</span></span>

<span data-ttu-id="93729-140">Un valor predeterminado puede tener el valor especial **RouteParameter.Optional**.</span><span class="sxs-lookup"><span data-stu-id="93729-140">A default can have the special value **RouteParameter.Optional**.</span></span> <span data-ttu-id="93729-141">Si un marcador de posición se asigna este valor, el valor no se agrega al diccionario de ruta.</span><span class="sxs-lookup"><span data-stu-id="93729-141">If a placeholder gets assigned this value, the value is not added to the route dictionary.</span></span> <span data-ttu-id="93729-142">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="93729-142">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

<span data-ttu-id="93729-143">Para la ruta de acceso URI "api/products", el diccionario de ruta contendrá:</span><span class="sxs-lookup"><span data-stu-id="93729-143">For the URI path "api/products", the route dictionary will contain:</span></span>

- <span data-ttu-id="93729-144">controlador: "productos"</span><span class="sxs-lookup"><span data-stu-id="93729-144">controller: "products"</span></span>
- <span data-ttu-id="93729-145">categoría: "all"</span><span class="sxs-lookup"><span data-stu-id="93729-145">category: "all"</span></span>

<span data-ttu-id="93729-146">Para "api/productos/toys/123", sin embargo, el diccionario de ruta contendrá:</span><span class="sxs-lookup"><span data-stu-id="93729-146">For "api/products/toys/123", however, the route dictionary will contain:</span></span>

- <span data-ttu-id="93729-147">controlador: "productos"</span><span class="sxs-lookup"><span data-stu-id="93729-147">controller: "products"</span></span>
- <span data-ttu-id="93729-148">categoría: "toys"</span><span class="sxs-lookup"><span data-stu-id="93729-148">category: "toys"</span></span>
- <span data-ttu-id="93729-149">Id.: "123"</span><span class="sxs-lookup"><span data-stu-id="93729-149">id: "123"</span></span>

<span data-ttu-id="93729-150">Los valores predeterminados también pueden incluir un valor que no aparece en la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="93729-150">The defaults can also include a value that does not appear anywhere in the route template.</span></span> <span data-ttu-id="93729-151">Si coincide con la ruta, ese valor se almacena en el diccionario.</span><span class="sxs-lookup"><span data-stu-id="93729-151">If the route matches, that value is stored in the dictionary.</span></span> <span data-ttu-id="93729-152">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="93729-152">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

<span data-ttu-id="93729-153">Si la ruta de acceso URI es "api/raíz/8", el diccionario contendrá dos valores:</span><span class="sxs-lookup"><span data-stu-id="93729-153">If the URI path is "api/root/8", the dictionary will contain two values:</span></span>

- <span data-ttu-id="93729-154">controlador: "clientes"</span><span class="sxs-lookup"><span data-stu-id="93729-154">controller: "customers"</span></span>
- <span data-ttu-id="93729-155">Id.: "8"</span><span class="sxs-lookup"><span data-stu-id="93729-155">id: "8"</span></span>

## <a name="selecting-a-controller"></a><span data-ttu-id="93729-156">Al seleccionar un controlador</span><span class="sxs-lookup"><span data-stu-id="93729-156">Selecting a Controller</span></span>

<span data-ttu-id="93729-157">Selección del controlador se controla mediante el **IHttpControllerSelector.SelectController** método.</span><span class="sxs-lookup"><span data-stu-id="93729-157">Controller selection is handled by the **IHttpControllerSelector.SelectController** method.</span></span> <span data-ttu-id="93729-158">Este método toma un **HttpRequestMessage** instancia y devuelve un **HttpControllerDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="93729-158">This method takes an **HttpRequestMessage** instance and returns an **HttpControllerDescriptor**.</span></span> <span data-ttu-id="93729-159">Proporciona la implementación predeterminada del **DefaultHttpControllerSelector** clase.</span><span class="sxs-lookup"><span data-stu-id="93729-159">The default implementation is provided by the **DefaultHttpControllerSelector** class.</span></span> <span data-ttu-id="93729-160">Esta clase utiliza un algoritmo sencillo:</span><span class="sxs-lookup"><span data-stu-id="93729-160">This class uses a straightforward algorithm:</span></span>

1. <span data-ttu-id="93729-161">Busque en el diccionario de ruta para la clave "controller".</span><span class="sxs-lookup"><span data-stu-id="93729-161">Look in the route dictionary for the key "controller".</span></span>
2. <span data-ttu-id="93729-162">Tomamos el valor de esta clave y anexe la cadena "Controller" para obtener el nombre del tipo de controlador.</span><span class="sxs-lookup"><span data-stu-id="93729-162">Take the value for this key and append the string "Controller" to get the controller type name.</span></span>
3. <span data-ttu-id="93729-163">Busque un controlador Web API con este nombre de tipo.</span><span class="sxs-lookup"><span data-stu-id="93729-163">Look for a Web API controller with this type name.</span></span>

<span data-ttu-id="93729-164">Por ejemplo, si el diccionario de ruta contiene el par clave-valor "controlador" = "productos", el tipo de controlador es "ProductsController".</span><span class="sxs-lookup"><span data-stu-id="93729-164">For example, if the route dictionary contains the key-value pair "controller" = "products", then the controller type is "ProductsController".</span></span> <span data-ttu-id="93729-165">Si no hay ningún tipo de coincidencia o varias coincidencias, el marco de trabajo devuelve un error al cliente.</span><span class="sxs-lookup"><span data-stu-id="93729-165">If there is no matching type, or multiple matches, the framework returns an error to the client.</span></span>

<span data-ttu-id="93729-166">Para el paso 3, **DefaultHttpControllerSelector** usa el **IHttpControllerTypeResolver** interfaz para obtener la lista de tipos de controlador de API Web.</span><span class="sxs-lookup"><span data-stu-id="93729-166">For step 3, **DefaultHttpControllerSelector** uses the **IHttpControllerTypeResolver** interface to get the list of Web API controller types.</span></span> <span data-ttu-id="93729-167">La implementación predeterminada de **IHttpControllerTypeResolver** devuelve todas las clases públicas que implementan (a) **IHttpController**, (b) son no abstracta y (c) tiene un nombre que termina en "Controller".</span><span class="sxs-lookup"><span data-stu-id="93729-167">The default implementation of **IHttpControllerTypeResolver** returns all public classes that (a) implement **IHttpController**, (b) are not abstract, and (c) have a name that ends in "Controller".</span></span>

## <a name="action-selection"></a><span data-ttu-id="93729-168">Selección de acción</span><span class="sxs-lookup"><span data-stu-id="93729-168">Action Selection</span></span>

<span data-ttu-id="93729-169">Después de seleccionar el controlador, framework selecciona la acción mediante una llamada a la **IHttpActionSelector.SelectAction** método.</span><span class="sxs-lookup"><span data-stu-id="93729-169">After selecting the controller, the framework selects the action by calling the **IHttpActionSelector.SelectAction** method.</span></span> <span data-ttu-id="93729-170">Este método toma un **HttpControllerContext** y devuelve un **HttpActionDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="93729-170">This method takes an **HttpControllerContext** and returns an **HttpActionDescriptor**.</span></span>

<span data-ttu-id="93729-171">Proporciona la implementación predeterminada del **ApiControllerActionSelector** clase.</span><span class="sxs-lookup"><span data-stu-id="93729-171">The default implementation is provided by the **ApiControllerActionSelector** class.</span></span> <span data-ttu-id="93729-172">Para seleccionar una acción, es similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="93729-172">To select an action, it looks at the following:</span></span>

- <span data-ttu-id="93729-173">El método HTTP de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="93729-173">The HTTP method of the request.</span></span>
- <span data-ttu-id="93729-174">El marcador de posición "{action}" en la plantilla de ruta, si está presente.</span><span class="sxs-lookup"><span data-stu-id="93729-174">The "{action}" placeholder in the route template, if present.</span></span>
- <span data-ttu-id="93729-175">Los parámetros de las acciones en el controlador.</span><span class="sxs-lookup"><span data-stu-id="93729-175">The parameters of the actions on the controller.</span></span>

<span data-ttu-id="93729-176">Antes de ver el algoritmo de selección, es necesario comprender algunas cosas sobre las acciones de controlador.</span><span class="sxs-lookup"><span data-stu-id="93729-176">Before looking at the selection algorithm, we need to understand some things about controller actions.</span></span>

<span data-ttu-id="93729-177">**¿Los métodos en el controlador se consideran "acciones"?**</span><span class="sxs-lookup"><span data-stu-id="93729-177">**Which methods on the controller are considered "actions"?**</span></span> <span data-ttu-id="93729-178">Al seleccionar una acción, el marco de trabajo busca solo en métodos de instancia públicos en el controlador.</span><span class="sxs-lookup"><span data-stu-id="93729-178">When selecting an action, the framework only looks at public instance methods on the controller.</span></span> <span data-ttu-id="93729-179">Además, excluye ["nombre especial"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) métodos (constructores, eventos, las sobrecargas del operador etc.) y métodos heredados de la **ApiController** clase.</span><span class="sxs-lookup"><span data-stu-id="93729-179">Also, it excludes ["special name"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) methods (constructors, events, operator overloads, and so forth), and methods inherited from the **ApiController** class.</span></span>

<span data-ttu-id="93729-180">**Métodos HTTP.**</span><span class="sxs-lookup"><span data-stu-id="93729-180">**HTTP Methods.**</span></span> <span data-ttu-id="93729-181">El marco de trabajo solo elige las acciones que coinciden con el método HTTP de la solicitud, se determina como sigue:</span><span class="sxs-lookup"><span data-stu-id="93729-181">The framework only chooses actions that match the HTTP method of the request, determined as follows:</span></span>

1. <span data-ttu-id="93729-182">Puede especificar el método HTTP con un atributo: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**, **HttpOptions**, **HttpPatch**, **HttpPost**, o **HttpPut**.</span><span class="sxs-lookup"><span data-stu-id="93729-182">You can specify the HTTP method with an attribute: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**, **HttpOptions**, **HttpPatch**, **HttpPost**, or **HttpPut**.</span></span>
2. <span data-ttu-id="93729-183">En caso contrario, si el nombre del método controller comienza con "Get", "Post", "Put", "Delete", "Head", "Opciones" o "Revisión", a continuación, por convención, la acción es compatible con ese método HTTP.</span><span class="sxs-lookup"><span data-stu-id="93729-183">Otherwise, if the name of the controller method starts with "Get", "Post", "Put", "Delete", "Head", "Options", or "Patch", then by convention the action supports that HTTP method.</span></span>
3. <span data-ttu-id="93729-184">Si ninguno de los anteriores, el método admite la publicación.</span><span class="sxs-lookup"><span data-stu-id="93729-184">If none of the above, the method supports POST.</span></span>

<span data-ttu-id="93729-185">**Enlaces de parámetros.**</span><span class="sxs-lookup"><span data-stu-id="93729-185">**Parameter Bindings.**</span></span> <span data-ttu-id="93729-186">Un enlace de parámetros es cómo la API Web crea un valor para un parámetro.</span><span class="sxs-lookup"><span data-stu-id="93729-186">A parameter binding is how Web API creates a value for a parameter.</span></span> <span data-ttu-id="93729-187">Aquí es la regla predeterminada para el enlace de parámetros:</span><span class="sxs-lookup"><span data-stu-id="93729-187">Here is the default rule for parameter binding:</span></span>

- <span data-ttu-id="93729-188">Tipos simples se toman de la dirección URI.</span><span class="sxs-lookup"><span data-stu-id="93729-188">Simple types are taken from the URI.</span></span>
- <span data-ttu-id="93729-189">Tipos complejos se toman del cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="93729-189">Complex types are taken from the request body.</span></span>

<span data-ttu-id="93729-190">Los tipos simples incluyen todos los [tipos primitivos de .NET Framework](https://msdn.microsoft.com/library/system.type.isprimitive), además de **DateTime**, **Decimal**, **Guid**, **cadena** , y **TimeSpan**.</span><span class="sxs-lookup"><span data-stu-id="93729-190">Simple types include all of the [.NET Framework primitive types](https://msdn.microsoft.com/library/system.type.isprimitive), plus **DateTime**, **Decimal**, **Guid**, **String**, and **TimeSpan**.</span></span> <span data-ttu-id="93729-191">Para cada acción, como máximo un parámetro puede leer el cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="93729-191">For each action, at most one parameter can read the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="93729-192">Es posible invalidar las reglas de enlace predeterminada.</span><span class="sxs-lookup"><span data-stu-id="93729-192">It is possible to override the default binding rules.</span></span> <span data-ttu-id="93729-193">Consulte [enlace de parámetros de WebAPI debajo del capó](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span><span class="sxs-lookup"><span data-stu-id="93729-193">See [WebAPI Parameter binding under the hood](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span></span>


<span data-ttu-id="93729-194">Con esa información, aquí es el algoritmo de selección de acción.</span><span class="sxs-lookup"><span data-stu-id="93729-194">With that background, here is the action selection algorithm.</span></span>

1. <span data-ttu-id="93729-195">Crear una lista de todas las acciones en el controlador que coincida con el método de solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="93729-195">Create a list of all actions on the controller that match the HTTP request method.</span></span>
2. <span data-ttu-id="93729-196">Si el diccionario de ruta tiene una entrada de "acción", quite las acciones cuyo nombre no coincide con este valor.</span><span class="sxs-lookup"><span data-stu-id="93729-196">If the route dictionary has an "action" entry, remove actions whose name does not match this value.</span></span>
3. <span data-ttu-id="93729-197">Intenta coincidir con los parámetros de acción al URI, como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="93729-197">Try to match action parameters to the URI, as follows:</span></span> 

    1. <span data-ttu-id="93729-198">Para cada acción, obtenga una lista de los parámetros que son un tipo simple, donde el enlace obtiene el parámetro del URI.</span><span class="sxs-lookup"><span data-stu-id="93729-198">For each action, get a list of the parameters that are a simple type, where the binding gets the parameter from the URI.</span></span> <span data-ttu-id="93729-199">Excluir los parámetros opcionales.</span><span class="sxs-lookup"><span data-stu-id="93729-199">Exclude optional parameters.</span></span>
    2. <span data-ttu-id="93729-200">En esta lista, intenta encontrar a una coincidencia para cada nombre de parámetro, en el diccionario de ruta o en la cadena de consulta URI.</span><span class="sxs-lookup"><span data-stu-id="93729-200">From this list, try to find a match for each parameter name, either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="93729-201">Las coincidencias no distinguen mayúsculas de minúsculas y no dependen de la orden de los parámetros.</span><span class="sxs-lookup"><span data-stu-id="93729-201">Matches are case insensitive and do not depend on the parameter order.</span></span>
    3. <span data-ttu-id="93729-202">Seleccione una acción donde todos los parámetros de la lista tiene una coincidencia en el URI.</span><span class="sxs-lookup"><span data-stu-id="93729-202">Select an action where every parameter in the list has a match in the URI.</span></span>
    4. <span data-ttu-id="93729-203">Si más que una acción cumple estos criterios, elija uno con la mayoría coincidencias de parámetro.</span><span class="sxs-lookup"><span data-stu-id="93729-203">If more that one action meets these criteria, pick the one with the most parameter matches.</span></span>
4. <span data-ttu-id="93729-204">Pasar por alto las acciones con el **[NonAction]** atributo.</span><span class="sxs-lookup"><span data-stu-id="93729-204">Ignore actions with the **[NonAction]** attribute.</span></span>

<span data-ttu-id="93729-205">Paso #3 es probablemente el más confuso.</span><span class="sxs-lookup"><span data-stu-id="93729-205">Step #3 is probably the most confusing.</span></span> <span data-ttu-id="93729-206">La idea básica es que un parámetro puede obtener su valor desde el URI, desde el cuerpo de solicitud o desde un enlace personalizado.</span><span class="sxs-lookup"><span data-stu-id="93729-206">The basic idea is that a parameter can get its value either from the URI, from the request body, or from a custom binding.</span></span> <span data-ttu-id="93729-207">Para los parámetros que proceden de la dirección URI, queremos asegurarnos de que el URI contiene realmente un valor para ese parámetro, en la ruta de acceso (mediante el diccionario de ruta) o en la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="93729-207">For parameters that come from the URI, we want to ensure that the URI actually contains a value for that parameter, either in the path (via the route dictionary) or in the query string.</span></span>

<span data-ttu-id="93729-208">Por ejemplo, considere la siguiente acción:</span><span class="sxs-lookup"><span data-stu-id="93729-208">For example, consider the following action:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

<span data-ttu-id="93729-209">El *id* parámetro se enlaza a la dirección URI.</span><span class="sxs-lookup"><span data-stu-id="93729-209">The *id* parameter binds to the URI.</span></span> <span data-ttu-id="93729-210">Por lo tanto, esta acción solo puede coincidir con un URI que contiene un valor para "id", en el diccionario de ruta o en la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="93729-210">Therefore, this action can only match a URI that contains a value for "id", either in the route dictionary or in the query string.</span></span>

<span data-ttu-id="93729-211">Los parámetros opcionales son una excepción, porque son opcionales.</span><span class="sxs-lookup"><span data-stu-id="93729-211">Optional parameters are an exception, because they are optional.</span></span> <span data-ttu-id="93729-212">Para un parámetro opcional, es correcto si el enlace no puede obtener el valor del URI.</span><span class="sxs-lookup"><span data-stu-id="93729-212">For an optional parameter, it's OK if the binding can't get the value from the URI.</span></span>

<span data-ttu-id="93729-213">Los tipos complejos son una excepción por un motivo distinto.</span><span class="sxs-lookup"><span data-stu-id="93729-213">Complex types are an exception for a different reason.</span></span> <span data-ttu-id="93729-214">Un tipo complejo sólo puede enlazar al identificador URI a través de un enlace personalizado.</span><span class="sxs-lookup"><span data-stu-id="93729-214">A complex type can only bind to the URI through a custom binding.</span></span> <span data-ttu-id="93729-215">Pero en ese caso, no se puede saber el marco de trabajo de antemano si el parámetro se enlazaría a un URI determinado.</span><span class="sxs-lookup"><span data-stu-id="93729-215">But in that case, the framework cannot know in advance whether the parameter would bind to a particular URI.</span></span> <span data-ttu-id="93729-216">Para obtener, deberá invocar el enlace.</span><span class="sxs-lookup"><span data-stu-id="93729-216">To find out, it would need to invoke the binding.</span></span> <span data-ttu-id="93729-217">Es el objetivo del algoritmo de selección seleccionar una acción de la descripción estática, antes de invocar todos los enlaces.</span><span class="sxs-lookup"><span data-stu-id="93729-217">The goal of the selection algorithm is to select an action from the static description, before invoking any bindings.</span></span> <span data-ttu-id="93729-218">Por lo tanto, los tipos complejos se excluyen el algoritmo de coincidencia.</span><span class="sxs-lookup"><span data-stu-id="93729-218">Therefore, complex types are excluded from the matching algorithm.</span></span>

<span data-ttu-id="93729-219">Después de selecciona la acción, se invocan todos los enlaces de parámetro.</span><span class="sxs-lookup"><span data-stu-id="93729-219">After the action is selected, all parameter bindings are invoked.</span></span>

<span data-ttu-id="93729-220">Resumen:</span><span class="sxs-lookup"><span data-stu-id="93729-220">Summary:</span></span>

- <span data-ttu-id="93729-221">La acción debe coincidir con el método HTTP de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="93729-221">The action must match the HTTP method of the request.</span></span>
- <span data-ttu-id="93729-222">El nombre de acción debe coincidir con la entrada de "acción" en el diccionario de ruta, si está presente.</span><span class="sxs-lookup"><span data-stu-id="93729-222">The action name must match the "action" entry in the route dictionary, if present.</span></span>
- <span data-ttu-id="93729-223">Para cada parámetro de la acción, si se toma el parámetro de la dirección URI, a continuación, el nombre del parámetro se debe encontrar en el diccionario de ruta o en la cadena de consulta URI.</span><span class="sxs-lookup"><span data-stu-id="93729-223">For every parameter of the action, if the parameter is taken from the URI, then the parameter name must be found either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="93729-224">(Se excluyen los parámetros opcionales y los parámetros con tipos complejos.)</span><span class="sxs-lookup"><span data-stu-id="93729-224">(Optional parameters and parameters with complex types are excluded.)</span></span>
- <span data-ttu-id="93729-225">Intenta coincidir con el mayor número de parámetros.</span><span class="sxs-lookup"><span data-stu-id="93729-225">Try to match the most number of parameters.</span></span> <span data-ttu-id="93729-226">La mejor coincidencia podría ser un método sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="93729-226">The best match might be a method with no parameters.</span></span>

## <a name="extended-example"></a><span data-ttu-id="93729-227">Ejemplo ampliado</span><span class="sxs-lookup"><span data-stu-id="93729-227">Extended Example</span></span>

<span data-ttu-id="93729-228">Rutas:</span><span class="sxs-lookup"><span data-stu-id="93729-228">Routes:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

<span data-ttu-id="93729-229">Controlador:</span><span class="sxs-lookup"><span data-stu-id="93729-229">Controller:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

<span data-ttu-id="93729-230">Solicitud HTTP:</span><span class="sxs-lookup"><span data-stu-id="93729-230">HTTP request:</span></span>

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a><span data-ttu-id="93729-231">Coincidencia de rutas</span><span class="sxs-lookup"><span data-stu-id="93729-231">Route Matching</span></span>

<span data-ttu-id="93729-232">El URI coincida con la ruta denominada "DefaultApi".</span><span class="sxs-lookup"><span data-stu-id="93729-232">The URI matches the route named "DefaultApi".</span></span> <span data-ttu-id="93729-233">El diccionario de ruta contiene las siguientes entradas:</span><span class="sxs-lookup"><span data-stu-id="93729-233">The route dictionary contains the following entries:</span></span>

- <span data-ttu-id="93729-234">controlador: "productos"</span><span class="sxs-lookup"><span data-stu-id="93729-234">controller: "products"</span></span>
- <span data-ttu-id="93729-235">Id.: "1"</span><span class="sxs-lookup"><span data-stu-id="93729-235">id: "1"</span></span>

<span data-ttu-id="93729-236">El diccionario de ruta no contiene los parámetros de cadena de consulta, "version" y "Detalles", pero estos se seguirá considerando durante la selección de acción.</span><span class="sxs-lookup"><span data-stu-id="93729-236">The route dictionary does not contain the query string parameters, "version" and "details", but these will still be considered during action selection.</span></span>

### <a name="controller-selection"></a><span data-ttu-id="93729-237">Selección de controlador</span><span class="sxs-lookup"><span data-stu-id="93729-237">Controller Selection</span></span>

<span data-ttu-id="93729-238">En la entrada "controller" en el diccionario de ruta, el tipo de controlador es `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="93729-238">From the "controller" entry in the route dictionary, the controller type is `ProductsController`.</span></span>

### <a name="action-selection"></a><span data-ttu-id="93729-239">Selección de acción</span><span class="sxs-lookup"><span data-stu-id="93729-239">Action Selection</span></span>

<span data-ttu-id="93729-240">La solicitud HTTP es una solicitud GET.</span><span class="sxs-lookup"><span data-stu-id="93729-240">The HTTP request is a GET request.</span></span> <span data-ttu-id="93729-241">Las acciones de controlador que se admiten GET son `GetAll`, `GetById`, y `FindProductsByName`.</span><span class="sxs-lookup"><span data-stu-id="93729-241">The controller actions that support GET are `GetAll`, `GetById`, and `FindProductsByName`.</span></span> <span data-ttu-id="93729-242">El diccionario de ruta no contiene una entrada para "action", por lo que no necesitamos para que coincida con el nombre de acción.</span><span class="sxs-lookup"><span data-stu-id="93729-242">The route dictionary does not contain an entry for "action", so we don't need to match the action name.</span></span>

<span data-ttu-id="93729-243">A continuación, intentamos hacer coincidir los nombres de parámetro para las acciones, mirar solo según las acciones GET.</span><span class="sxs-lookup"><span data-stu-id="93729-243">Next, we try to match parameter names for the actions, looking only at the GET actions.</span></span>

| <span data-ttu-id="93729-244">Acción</span><span class="sxs-lookup"><span data-stu-id="93729-244">Action</span></span> | <span data-ttu-id="93729-245">Parámetros de coincidencia</span><span class="sxs-lookup"><span data-stu-id="93729-245">Parameters to Match</span></span> |
| --- | --- |
| `GetAll` | <span data-ttu-id="93729-246">ninguna</span><span class="sxs-lookup"><span data-stu-id="93729-246">none</span></span> |
| `GetById` | <span data-ttu-id="93729-247">"ID".</span><span class="sxs-lookup"><span data-stu-id="93729-247">"id"</span></span> |
| `FindProductsByName` | <span data-ttu-id="93729-248">"name"</span><span class="sxs-lookup"><span data-stu-id="93729-248">"name"</span></span> |

<span data-ttu-id="93729-249">Tenga en cuenta que el *versión* parámetro de `GetById` no se considera, porque es un parámetro opcional.</span><span class="sxs-lookup"><span data-stu-id="93729-249">Notice that the *version* parameter of `GetById` is not considered, because it is an optional parameter.</span></span>

<span data-ttu-id="93729-250">El `GetAll` método coincide con forma trivial.</span><span class="sxs-lookup"><span data-stu-id="93729-250">The `GetAll` method matches trivially.</span></span> <span data-ttu-id="93729-251">El `GetById` método también coincide con, ya que el diccionario de ruta contiene "id".</span><span class="sxs-lookup"><span data-stu-id="93729-251">The `GetById` method also matches, because the route dictionary contains "id".</span></span> <span data-ttu-id="93729-252">El `FindProductsByName` no coincide con el método.</span><span class="sxs-lookup"><span data-stu-id="93729-252">The `FindProductsByName` method does not match.</span></span>

<span data-ttu-id="93729-253">El `GetById` método wins, dado que coincide con un parámetro, frente a sin parámetros para `GetAll`.</span><span class="sxs-lookup"><span data-stu-id="93729-253">The `GetById` method wins, because it matches one parameter, versus no parameters for `GetAll`.</span></span> <span data-ttu-id="93729-254">Se invoca el método con los siguientes valores de parámetro:</span><span class="sxs-lookup"><span data-stu-id="93729-254">The method is invoked with the following parameter values:</span></span>

- <span data-ttu-id="93729-255">*Id. de* = 1</span><span class="sxs-lookup"><span data-stu-id="93729-255">*id* = 1</span></span>
- <span data-ttu-id="93729-256">*versión* = 1.5</span><span class="sxs-lookup"><span data-stu-id="93729-256">*version* = 1.5</span></span>

<span data-ttu-id="93729-257">Tenga en cuenta que aunque *versión* no se ha usado en el algoritmo de selección, el valor del parámetro procede de la cadena de consulta URI.</span><span class="sxs-lookup"><span data-stu-id="93729-257">Notice that even though *version* was not used in the selection algorithm, the value of the parameter comes from the URI query string.</span></span>

## <a name="extension-points"></a><span data-ttu-id="93729-258">Puntos de extensión</span><span class="sxs-lookup"><span data-stu-id="93729-258">Extension Points</span></span>

<span data-ttu-id="93729-259">API Web proporciona puntos de extensión para algunas partes del proceso de enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="93729-259">Web API provides extension points for some parts of the routing process.</span></span>

| <span data-ttu-id="93729-260">Interfaz</span><span class="sxs-lookup"><span data-stu-id="93729-260">Interface</span></span> | <span data-ttu-id="93729-261">Descripción</span><span class="sxs-lookup"><span data-stu-id="93729-261">Description</span></span> |
| --- | --- |
| <span data-ttu-id="93729-262">**IHttpControllerSelector**</span><span class="sxs-lookup"><span data-stu-id="93729-262">**IHttpControllerSelector**</span></span> | <span data-ttu-id="93729-263">Selecciona el controlador.</span><span class="sxs-lookup"><span data-stu-id="93729-263">Selects the controller.</span></span> |
| <span data-ttu-id="93729-264">**IHttpControllerTypeResolver**</span><span class="sxs-lookup"><span data-stu-id="93729-264">**IHttpControllerTypeResolver**</span></span> | <span data-ttu-id="93729-265">Obtiene la lista de tipos de controlador.</span><span class="sxs-lookup"><span data-stu-id="93729-265">Gets the list of controller types.</span></span> <span data-ttu-id="93729-266">El **DefaultHttpControllerSelector** elige el tipo de controlador de esta lista.</span><span class="sxs-lookup"><span data-stu-id="93729-266">The **DefaultHttpControllerSelector** chooses the controller type from this list.</span></span> |
| <span data-ttu-id="93729-267">**IAssembliesResolver**</span><span class="sxs-lookup"><span data-stu-id="93729-267">**IAssembliesResolver**</span></span> | <span data-ttu-id="93729-268">Obtiene la lista de ensamblados de proyecto.</span><span class="sxs-lookup"><span data-stu-id="93729-268">Gets the list of project assemblies.</span></span> <span data-ttu-id="93729-269">El **IHttpControllerTypeResolver** interfaz utiliza esta lista para buscar los tipos de controlador.</span><span class="sxs-lookup"><span data-stu-id="93729-269">The **IHttpControllerTypeResolver** interface uses this list to find the controller types.</span></span> |
| <span data-ttu-id="93729-270">**IHttpControllerActivator**</span><span class="sxs-lookup"><span data-stu-id="93729-270">**IHttpControllerActivator**</span></span> | <span data-ttu-id="93729-271">Crea nuevas instancias de controlador.</span><span class="sxs-lookup"><span data-stu-id="93729-271">Creates new controller instances.</span></span> |
| <span data-ttu-id="93729-272">**IHttpActionSelector**</span><span class="sxs-lookup"><span data-stu-id="93729-272">**IHttpActionSelector**</span></span> | <span data-ttu-id="93729-273">Selecciona la acción.</span><span class="sxs-lookup"><span data-stu-id="93729-273">Selects the action.</span></span> |
| <span data-ttu-id="93729-274">**IHttpActionInvoker**</span><span class="sxs-lookup"><span data-stu-id="93729-274">**IHttpActionInvoker**</span></span> | <span data-ttu-id="93729-275">Invoca la acción.</span><span class="sxs-lookup"><span data-stu-id="93729-275">Invokes the action.</span></span> |

<span data-ttu-id="93729-276">Para proporcionar su propia implementación de cualquiera de estas interfaces, utilice el **servicios** colección en el **HttpConfiguration** objeto:</span><span class="sxs-lookup"><span data-stu-id="93729-276">To provide your own implementation for any of these interfaces, use the **Services** collection on the **HttpConfiguration** object:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
