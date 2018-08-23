---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Enrutamiento de ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 02/11/2012
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 458f9a6369fe97bab33d70bf31bd470b1b0e593c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838846"
---
<a name="routing-in-aspnet-web-api"></a><span data-ttu-id="2acc4-102">Enrutamiento de ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="2acc4-102">Routing in ASP.NET Web API</span></span>
====================
<span data-ttu-id="2acc4-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2acc4-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="2acc4-104">En este artículo se describe cómo ASP.NET Web API enruta las solicitudes HTTP a los controladores.</span><span class="sxs-lookup"><span data-stu-id="2acc4-104">This article describes how ASP.NET Web API routes HTTP requests to controllers.</span></span>

> [!NOTE]
> <span data-ttu-id="2acc4-105">Si está familiarizado con ASP.NET MVC, API Web de enrutamiento es muy similar para el enrutamiento de MVC.</span><span class="sxs-lookup"><span data-stu-id="2acc4-105">If you are familiar with ASP.NET MVC, Web API routing is very similar to MVC routing.</span></span> <span data-ttu-id="2acc4-106">La principal diferencia es que API Web usa el método HTTP, no la ruta de acceso URI, para seleccionar la acción.</span><span class="sxs-lookup"><span data-stu-id="2acc4-106">The main difference is that Web API uses the HTTP method, not the URI path, to select the action.</span></span> <span data-ttu-id="2acc4-107">También puede usar el enrutamiento de estilo MVC en Web API.</span><span class="sxs-lookup"><span data-stu-id="2acc4-107">You can also use MVC-style routing in Web API.</span></span> <span data-ttu-id="2acc4-108">En este artículo no supone ningún conocimiento de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="2acc4-108">This article does not assume any knowledge of ASP.NET MVC.</span></span>


## <a name="routing-tables"></a><span data-ttu-id="2acc4-109">Tablas de enrutamiento</span><span class="sxs-lookup"><span data-stu-id="2acc4-109">Routing Tables</span></span>

<span data-ttu-id="2acc4-110">En ASP.NET Web API, un *controlador* es una clase que controla las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="2acc4-110">In ASP.NET Web API, a *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="2acc4-111">Se llama a los métodos públicos del controlador de *métodos de acción* o simplemente *acciones*.</span><span class="sxs-lookup"><span data-stu-id="2acc4-111">The public methods of the controller are called *action methods* or simply *actions*.</span></span> <span data-ttu-id="2acc4-112">Cuando el marco API Web recibe una solicitud, enruta la solicitud a una acción.</span><span class="sxs-lookup"><span data-stu-id="2acc4-112">When the Web API framework receives a request, it routes the request to an action.</span></span>

<span data-ttu-id="2acc4-113">Para determinar qué acción va a invocar, el marco de trabajo usa un *tabla de enrutamiento*.</span><span class="sxs-lookup"><span data-stu-id="2acc4-113">To determine which action to invoke, the framework uses a *routing table*.</span></span> <span data-ttu-id="2acc4-114">La plantilla de proyecto de Visual Studio para Web API crea una ruta predeterminada:</span><span class="sxs-lookup"><span data-stu-id="2acc4-114">The Visual Studio project template for Web API creates a default route:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="2acc4-115">Esta ruta se define en el archivo WebApiConfig.cs, que se coloca en la aplicación\_directorio de inicio:</span><span class="sxs-lookup"><span data-stu-id="2acc4-115">This route is defined in the WebApiConfig.cs file, which is placed in the App\_Start directory:</span></span>

![](routing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="2acc4-116">Para obtener más información sobre la **WebApiConfig** de clases, vea [configuración de ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="2acc4-116">For more information about the **WebApiConfig** class, see [Configuring ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span></span>

<span data-ttu-id="2acc4-117">Si la API Web de autohospedaje, debe establecer la tabla de enrutamiento directamente en el **HttpSelfHostConfiguration** objeto.</span><span class="sxs-lookup"><span data-stu-id="2acc4-117">If you self-host Web API, you must set the routing table directly on the **HttpSelfHostConfiguration** object.</span></span> <span data-ttu-id="2acc4-118">Para obtener más información, consulte [autohospedaje una API Web](../older-versions/self-host-a-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="2acc4-118">For more information, see [Self-Host a Web API](../older-versions/self-host-a-web-api.md).</span></span>

<span data-ttu-id="2acc4-119">Cada entrada en la tabla de enrutamiento contiene un *plantilla de ruta*.</span><span class="sxs-lookup"><span data-stu-id="2acc4-119">Each entry in the routing table contains a *route template*.</span></span> <span data-ttu-id="2acc4-120">La plantilla de ruta predeterminada para la API Web es &quot;api / {controller} / {id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="2acc4-120">The default route template for Web API is &quot;api/{controller}/{id}&quot;.</span></span> <span data-ttu-id="2acc4-121">En esta plantilla, &quot;api&quot; es un segmento de ruta de acceso literal y {controller} y {id} son variables de marcador de posición.</span><span class="sxs-lookup"><span data-stu-id="2acc4-121">In this template, &quot;api&quot; is a literal path segment, and {controller} and {id} are placeholder variables.</span></span>

<span data-ttu-id="2acc4-122">Cuando el marco API Web recibe una solicitud HTTP, intenta coincidir con el identificador URI con una de las plantillas de ruta en la tabla de enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="2acc4-122">When the Web API framework receives an HTTP request, it tries to match the URI against one of the route templates in the routing table.</span></span> <span data-ttu-id="2acc4-123">Si coincide con ninguna ruta, el cliente recibe un error 404.</span><span class="sxs-lookup"><span data-stu-id="2acc4-123">If no route matches, the client receives a 404 error.</span></span> <span data-ttu-id="2acc4-124">Por ejemplo, los siguientes URI coinciden con la ruta predeterminada:</span><span class="sxs-lookup"><span data-stu-id="2acc4-124">For example, the following URIs match the default route:</span></span>

- <span data-ttu-id="2acc4-125">/ api/contacts</span><span class="sxs-lookup"><span data-stu-id="2acc4-125">/api/contacts</span></span>
- <span data-ttu-id="2acc4-126">/API/Contacts/1</span><span class="sxs-lookup"><span data-stu-id="2acc4-126">/api/contacts/1</span></span>
- <span data-ttu-id="2acc4-127">/API/Products/gizmo1</span><span class="sxs-lookup"><span data-stu-id="2acc4-127">/api/products/gizmo1</span></span>

<span data-ttu-id="2acc4-128">Sin embargo, el siguiente URI no coincide, porque carece del &quot;api&quot; segmento:</span><span class="sxs-lookup"><span data-stu-id="2acc4-128">However, the following URI does not match, because it lacks the &quot;api&quot; segment:</span></span>

- <span data-ttu-id="2acc4-129">/ contacts/1</span><span class="sxs-lookup"><span data-stu-id="2acc4-129">/contacts/1</span></span>

> [!NOTE]
> <span data-ttu-id="2acc4-130">La razón para usar "api" en la ruta es evitar colisiones con enrutamiento de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="2acc4-130">The reason for using "api" in the route is to avoid collisions with ASP.NET MVC routing.</span></span> <span data-ttu-id="2acc4-131">De este modo, puede tener &quot;/pone en contacto con&quot; vaya a un controlador MVC y &quot;/api/contacts&quot; vaya a un controlador Web API.</span><span class="sxs-lookup"><span data-stu-id="2acc4-131">That way, you can have &quot;/contacts&quot; go to an MVC controller, and &quot;/api/contacts&quot; go to a Web API controller.</span></span> <span data-ttu-id="2acc4-132">Por supuesto, si no le gusta esta convención, puede cambiar la tabla de rutas predeterminadas.</span><span class="sxs-lookup"><span data-stu-id="2acc4-132">Of course, if you don't like this convention, you can change the default route table.</span></span>

<span data-ttu-id="2acc4-133">Una vez que se encuentra una ruta coincidente, Web API selecciona el controlador y la acción:</span><span class="sxs-lookup"><span data-stu-id="2acc4-133">Once a matching route is found, Web API selects the controller and the action:</span></span>

- <span data-ttu-id="2acc4-134">Para buscar el controlador, Web API agrega &quot;controlador&quot; al valor de la *{controller}* variable.</span><span class="sxs-lookup"><span data-stu-id="2acc4-134">To find the controller, Web API adds &quot;Controller&quot; to the value of the *{controller}* variable.</span></span>
- <span data-ttu-id="2acc4-135">Para encontrar la acción, API Web examina el método HTTP y, a continuación, busca una acción cuyo nombre comienza con ese nombre de método HTTP.</span><span class="sxs-lookup"><span data-stu-id="2acc4-135">To find the action, Web API looks at the HTTP method, and then looks for an action whose name begins with that HTTP method name.</span></span> <span data-ttu-id="2acc4-136">Por ejemplo, con una solicitud GET, Web API es una acción que se inicia con &quot;obtener... &quot;, tales como &quot;GetContact&quot; o &quot;GetAllContacts&quot;.</span><span class="sxs-lookup"><span data-stu-id="2acc4-136">For example, with a GET request, Web API looks for an action that starts with &quot;Get...&quot;, such as &quot;GetContact&quot; or &quot;GetAllContacts&quot;.</span></span> <span data-ttu-id="2acc4-137">Esta convención se aplica solo a GET, POST, PUT y DELETE de métodos.</span><span class="sxs-lookup"><span data-stu-id="2acc4-137">This convention applies only to GET, POST, PUT, and DELETE methods.</span></span> <span data-ttu-id="2acc4-138">Puede habilitar otros métodos HTTP mediante el uso de atributos en el controlador.</span><span class="sxs-lookup"><span data-stu-id="2acc4-138">You can enable other HTTP methods by using attributes on your controller.</span></span> <span data-ttu-id="2acc4-139">Veremos un ejemplo de eso más adelante.</span><span class="sxs-lookup"><span data-stu-id="2acc4-139">We'll see an example of that later.</span></span>
- <span data-ttu-id="2acc4-140">Otras variables de marcador de posición en la plantilla de ruta, como *{id},* se asignan a parámetros de acción.</span><span class="sxs-lookup"><span data-stu-id="2acc4-140">Other placeholder variables in the route template, such as *{id},* are mapped to action parameters.</span></span>

<span data-ttu-id="2acc4-141">Veamos un ejemplo.</span><span class="sxs-lookup"><span data-stu-id="2acc4-141">Let's look at an example.</span></span> <span data-ttu-id="2acc4-142">Supongamos que define el controlador siguiente:</span><span class="sxs-lookup"><span data-stu-id="2acc4-142">Suppose that you define the following controller:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="2acc4-143">Estas son algunas posibles solicitudes HTTP, junto con la acción que se invoca para cada uno:</span><span class="sxs-lookup"><span data-stu-id="2acc4-143">Here are some possible HTTP requests, along with the action that gets invoked for each:</span></span>

| <span data-ttu-id="2acc4-144">Método HTTP</span><span class="sxs-lookup"><span data-stu-id="2acc4-144">HTTP Method</span></span> | <span data-ttu-id="2acc4-145">Ruta de acceso URI</span><span class="sxs-lookup"><span data-stu-id="2acc4-145">URI Path</span></span> | <span data-ttu-id="2acc4-146">Acción</span><span class="sxs-lookup"><span data-stu-id="2acc4-146">Action</span></span> | <span data-ttu-id="2acc4-147">Parámetro</span><span class="sxs-lookup"><span data-stu-id="2acc4-147">Parameter</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="2acc4-148">GET</span><span class="sxs-lookup"><span data-stu-id="2acc4-148">GET</span></span> | <span data-ttu-id="2acc4-149">API/products</span><span class="sxs-lookup"><span data-stu-id="2acc4-149">api/products</span></span> | <span data-ttu-id="2acc4-150">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="2acc4-150">GetAllProducts</span></span> | <span data-ttu-id="2acc4-151">*(ninguno)*</span><span class="sxs-lookup"><span data-stu-id="2acc4-151">*(none)*</span></span> |
| <span data-ttu-id="2acc4-152">GET</span><span class="sxs-lookup"><span data-stu-id="2acc4-152">GET</span></span> | <span data-ttu-id="2acc4-153">productos/API/4</span><span class="sxs-lookup"><span data-stu-id="2acc4-153">api/products/4</span></span> | <span data-ttu-id="2acc4-154">GetProductById</span><span class="sxs-lookup"><span data-stu-id="2acc4-154">GetProductById</span></span> | <span data-ttu-id="2acc4-155">4</span><span class="sxs-lookup"><span data-stu-id="2acc4-155">4</span></span> |
| <span data-ttu-id="2acc4-156">SUPRIMIR</span><span class="sxs-lookup"><span data-stu-id="2acc4-156">DELETE</span></span> | <span data-ttu-id="2acc4-157">productos/API/4</span><span class="sxs-lookup"><span data-stu-id="2acc4-157">api/products/4</span></span> | <span data-ttu-id="2acc4-158">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="2acc4-158">DeleteProduct</span></span> | <span data-ttu-id="2acc4-159">4</span><span class="sxs-lookup"><span data-stu-id="2acc4-159">4</span></span> |
| <span data-ttu-id="2acc4-160">EXPONER</span><span class="sxs-lookup"><span data-stu-id="2acc4-160">POST</span></span> | <span data-ttu-id="2acc4-161">API/products</span><span class="sxs-lookup"><span data-stu-id="2acc4-161">api/products</span></span> | <span data-ttu-id="2acc4-162">*(no coincidencia)*</span><span class="sxs-lookup"><span data-stu-id="2acc4-162">*(no match)*</span></span> |  |

<span data-ttu-id="2acc4-163">Tenga en cuenta que el *{id}* segmento del URI, si está presente, se asigna a la *id* parámetro de la acción.</span><span class="sxs-lookup"><span data-stu-id="2acc4-163">Notice that the *{id}* segment of the URI, if present, is mapped to the *id* parameter of the action.</span></span> <span data-ttu-id="2acc4-164">En este ejemplo, el controlador define dos métodos GET, uno con un *id* parámetro y otra sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="2acc4-164">In this example, the controller defines two GET methods, one with an *id* parameter and one with no parameters.</span></span>

<span data-ttu-id="2acc4-165">Además, tenga en cuenta que la solicitud POST producirá un error, porque el controlador no define un &quot;Post... &quot; método.</span><span class="sxs-lookup"><span data-stu-id="2acc4-165">Also, note that the POST request will fail, because the controller does not define a &quot;Post...&quot; method.</span></span>

## <a name="routing-variations"></a><span data-ttu-id="2acc4-166">Variaciones de enrutamientos</span><span class="sxs-lookup"><span data-stu-id="2acc4-166">Routing Variations</span></span>

<span data-ttu-id="2acc4-167">La sección anterior describe el mecanismo de enrutamiento básico para ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="2acc4-167">The previous section described the basic routing mechanism for ASP.NET Web API.</span></span> <span data-ttu-id="2acc4-168">Esta sección describen algunas variaciones.</span><span class="sxs-lookup"><span data-stu-id="2acc4-168">This section describes some variations.</span></span>

### <a name="http-methods"></a><span data-ttu-id="2acc4-169">Métodos HTTP</span><span class="sxs-lookup"><span data-stu-id="2acc4-169">HTTP Methods</span></span>

<span data-ttu-id="2acc4-170">En lugar de usar la convención de nomenclatura para métodos HTTP, puede especificar explícitamente el método HTTP para una acción decorar el método de acción con el **HttpGet**, **HttpPut**, **HttpPost** , o **HttpDelete** atributo.</span><span class="sxs-lookup"><span data-stu-id="2acc4-170">Instead of using the naming convention for HTTP methods, you can explicitly specify the HTTP method for an action by decorating the action method with the **HttpGet**, **HttpPut**, **HttpPost**, or **HttpDelete** attribute.</span></span>

<span data-ttu-id="2acc4-171">En el ejemplo siguiente, el método FindProduct se asigna a las solicitudes GET:</span><span class="sxs-lookup"><span data-stu-id="2acc4-171">In the following example, the FindProduct method is mapped to GET requests:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="2acc4-172">Para permitir que varios métodos HTTP para una acción, o para permitir que los métodos HTTP que no sean GET, PUT, POST y DELETE, use el **AcceptVerbs** atributo, que toma una lista de métodos HTTP.</span><span class="sxs-lookup"><span data-stu-id="2acc4-172">To allow multiple HTTP methods for an action, or to allow HTTP methods other than GET, PUT, POST, and DELETE, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a><span data-ttu-id="2acc4-173">Enrutamiento por nombre de acción</span><span class="sxs-lookup"><span data-stu-id="2acc4-173">Routing by Action Name</span></span>

<span data-ttu-id="2acc4-174">Con la plantilla de enrutamiento de forma predeterminada, la API Web usa el método HTTP para seleccionar la acción.</span><span class="sxs-lookup"><span data-stu-id="2acc4-174">With the default routing template, Web API uses the HTTP method to select the action.</span></span> <span data-ttu-id="2acc4-175">Sin embargo, también puede crear una ruta donde se incluye el nombre de acción en el URI:</span><span class="sxs-lookup"><span data-stu-id="2acc4-175">However, you can also create a route where the action name is included in the URI:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="2acc4-176">En esta plantilla de ruta, el *{action}* el método de acción en el controlador de los nombres de parámetros.</span><span class="sxs-lookup"><span data-stu-id="2acc4-176">In this route template, the *{action}* parameter names the action method on the controller.</span></span> <span data-ttu-id="2acc4-177">Con este estilo de enrutamiento, usar atributos para especificar los métodos HTTP permitidos.</span><span class="sxs-lookup"><span data-stu-id="2acc4-177">With this style of routing, use attributes to specify the allowed HTTP methods.</span></span> <span data-ttu-id="2acc4-178">Por ejemplo, suponga que el controlador tiene el siguiente método:</span><span class="sxs-lookup"><span data-stu-id="2acc4-178">For example, suppose your controller has the following method:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="2acc4-179">En este caso, podría asignar una solicitud GET para "api/productos/detalles/1" al método detalles.</span><span class="sxs-lookup"><span data-stu-id="2acc4-179">In this case, a GET request for "api/products/details/1" would map to the Details method.</span></span> <span data-ttu-id="2acc4-180">Este estilo de enrutamiento es similar a ASP.NET MVC y puede ser adecuado para una API de estilo RPC.</span><span class="sxs-lookup"><span data-stu-id="2acc4-180">This style of routing is similar to ASP.NET MVC, and may be appropriate for an RPC-style API.</span></span>

<span data-ttu-id="2acc4-181">Puede reemplazar el nombre de acción mediante el **ActionName** atributo.</span><span class="sxs-lookup"><span data-stu-id="2acc4-181">You can override the action name by using the **ActionName** attribute.</span></span> <span data-ttu-id="2acc4-182">En el ejemplo siguiente, hay dos acciones que se asignan a &quot;productos/api/miniatura/*id*. Uno es compatible con GET y el otro admite POST:</span><span class="sxs-lookup"><span data-stu-id="2acc4-182">In the following example, there are two actions that map to &quot;api/products/thumbnail/*id*. One supports GET and the other supports POST:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a><span data-ttu-id="2acc4-183">No acciones</span><span class="sxs-lookup"><span data-stu-id="2acc4-183">Non-Actions</span></span>

<span data-ttu-id="2acc4-184">Para evitar que un método invocando como una acción, use el **NonAction** atributo.</span><span class="sxs-lookup"><span data-stu-id="2acc4-184">To prevent a method from getting invoked as an action, use the **NonAction** attribute.</span></span> <span data-ttu-id="2acc4-185">Esto indica al marco que el método no es una acción, incluso si en caso contrario, coincidiría con las reglas de enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="2acc4-185">This signals to the framework that the method is not an action, even if it would otherwise match the routing rules.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a><span data-ttu-id="2acc4-186">Información adicional</span><span class="sxs-lookup"><span data-stu-id="2acc4-186">Further Reading</span></span>

<span data-ttu-id="2acc4-187">En este tema se proporciona una visión general del enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="2acc4-187">This topic provided a high-level view of routing.</span></span> <span data-ttu-id="2acc4-188">Para obtener más información, consulte [enrutamiento y selección de acción](routing-and-action-selection.md), que describen exactamente cómo el marco de trabajo coincide con un URI a una ruta, selecciona un controlador y, a continuación, selecciona la acción que se invoca.</span><span class="sxs-lookup"><span data-stu-id="2acc4-188">For more detail, see [Routing and Action Selection](routing-and-action-selection.md), which describes exactly how the framework matches a URI to a route, selects a controller, and then selects the action to invoke.</span></span>
