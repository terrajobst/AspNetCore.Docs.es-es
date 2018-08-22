---
uid: web-api/overview/error-handling/exception-handling
title: Control de excepciones en ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 03/12/2012
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: 62e6187cd82252e7d30f21e03cc4d08418fa39ee
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838746"
---
<a name="exception-handling-in-aspnet-web-api"></a><span data-ttu-id="42cb9-102">Control de excepciones en ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="42cb9-102">Exception Handling in ASP.NET Web API</span></span>
====================
<span data-ttu-id="42cb9-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="42cb9-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="42cb9-104">En este artículo describe el error y control de excepciones en ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="42cb9-104">This article describes error and exception handling in ASP.NET Web API.</span></span>

- [<span data-ttu-id="42cb9-105">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="42cb9-105">HttpResponseException</span></span>](#httpresponserexception)
- [<span data-ttu-id="42cb9-106">Filtros de excepciones</span><span class="sxs-lookup"><span data-stu-id="42cb9-106">Exception Filters</span></span>](#exception_filters)
- [<span data-ttu-id="42cb9-107">Registrar los filtros de excepciones</span><span class="sxs-lookup"><span data-stu-id="42cb9-107">Registering Exception Filters</span></span>](#registering_exception_filters)
- [<span data-ttu-id="42cb9-108">HttpError</span><span class="sxs-lookup"><span data-stu-id="42cb9-108">HttpError</span></span>](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a><span data-ttu-id="42cb9-109">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="42cb9-109">HttpResponseException</span></span>

<span data-ttu-id="42cb9-110">¿Qué ocurre si un controlador de API Web inicia una excepción no detectada?</span><span class="sxs-lookup"><span data-stu-id="42cb9-110">What happens if a Web API controller throws an uncaught exception?</span></span> <span data-ttu-id="42cb9-111">De forma predeterminada, la mayoría de las excepciones se traduce en una respuesta HTTP con código de estado 500 Error interno del servidor.</span><span class="sxs-lookup"><span data-stu-id="42cb9-111">By default, most exceptions are translated into an HTTP response with status code 500, Internal Server Error.</span></span>

<span data-ttu-id="42cb9-112">El **HttpResponseException** tipo es un caso especial.</span><span class="sxs-lookup"><span data-stu-id="42cb9-112">The **HttpResponseException** type is a special case.</span></span> <span data-ttu-id="42cb9-113">Esta excepción, devuelve cualquier código de estado HTTP que especifique en el constructor de la excepción.</span><span class="sxs-lookup"><span data-stu-id="42cb9-113">This exception returns any HTTP status code that you specify in the exception constructor.</span></span> <span data-ttu-id="42cb9-114">Por ejemplo, devuelve 404, no se encuentra en el siguiente método si la *id* parámetro no es válido.</span><span class="sxs-lookup"><span data-stu-id="42cb9-114">For example, the following method returns 404, Not Found, if the *id* parameter is not valid.</span></span>

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

<span data-ttu-id="42cb9-115">Para obtener más control sobre la respuesta, también puede construir el mensaje de respuesta completo e incluir con la **HttpResponseException:**</span><span class="sxs-lookup"><span data-stu-id="42cb9-115">For more control over the response, you can also construct the entire response message and include it with the **HttpResponseException:**</span></span> 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a><span data-ttu-id="42cb9-116">Filtros de excepciones</span><span class="sxs-lookup"><span data-stu-id="42cb9-116">Exception Filters</span></span>

<span data-ttu-id="42cb9-117">Puede personalizar cómo la API Web controla las excepciones escribiendo un *filtro de excepción*.</span><span class="sxs-lookup"><span data-stu-id="42cb9-117">You can customize how Web API handles exceptions by writing an *exception filter*.</span></span> <span data-ttu-id="42cb9-118">Un filtro de excepciones se ejecuta cuando un método de controlador produce cualquier excepción no controlada que es *no* una **HttpResponseException** excepción.</span><span class="sxs-lookup"><span data-stu-id="42cb9-118">An exception filter is executed when a controller method throws any unhandled exception that is *not* an **HttpResponseException** exception.</span></span> <span data-ttu-id="42cb9-119">El **HttpResponseException** tipo es un caso especial, porque se ha diseñado específicamente para devolver una respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="42cb9-119">The **HttpResponseException** type is a special case, because it is designed specifically for returning an HTTP response.</span></span>

<span data-ttu-id="42cb9-120">Filtros de excepción implementan la **System.Web.Http.Filters.IExceptionFilter** interfaz.</span><span class="sxs-lookup"><span data-stu-id="42cb9-120">Exception filters implement the **System.Web.Http.Filters.IExceptionFilter** interface.</span></span> <span data-ttu-id="42cb9-121">La manera más sencilla de escribir un filtro de excepción es derivar de la **System.Web.Http.Filters.ExceptionFilterAttribute** clase e invalidar la **OnException** método.</span><span class="sxs-lookup"><span data-stu-id="42cb9-121">The simplest way to write an exception filter is to derive from the **System.Web.Http.Filters.ExceptionFilterAttribute** class and override the **OnException** method.</span></span>

> [!NOTE]
> <span data-ttu-id="42cb9-122">Los filtros de excepciones en ASP.NET Web API son similares a los de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="42cb9-122">Exception filters in ASP.NET Web API are similar to those in ASP.NET MVC.</span></span> <span data-ttu-id="42cb9-123">Sin embargo, se declaran en un espacio de nombres independiente y una función por separado.</span><span class="sxs-lookup"><span data-stu-id="42cb9-123">However, they are declared in a separate namespace and function separately.</span></span> <span data-ttu-id="42cb9-124">En concreto, el **HandleErrorAttribute** clase usada en MVC no controla las excepciones producidas por controladores de API Web.</span><span class="sxs-lookup"><span data-stu-id="42cb9-124">In particular, the **HandleErrorAttribute** class used in MVC does not handle exceptions thrown by Web API controllers.</span></span>


<span data-ttu-id="42cb9-125">Este es un filtro que convierte **NotImplementedException** excepciones en código de estado HTTP 501, no se ha implementado el código:</span><span class="sxs-lookup"><span data-stu-id="42cb9-125">Here is a filter that converts **NotImplementedException** exceptions into HTTP status code 501, Not Implemented:</span></span>

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

<span data-ttu-id="42cb9-126">El **respuesta** propiedad de la **HttpActionExecutedContext** objeto contiene el mensaje de respuesta HTTP que se enviará al cliente.</span><span class="sxs-lookup"><span data-stu-id="42cb9-126">The **Response** property of the **HttpActionExecutedContext** object contains the HTTP response message that will be sent to the client.</span></span>

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a><span data-ttu-id="42cb9-127">Registrar los filtros de excepciones</span><span class="sxs-lookup"><span data-stu-id="42cb9-127">Registering Exception Filters</span></span>

<span data-ttu-id="42cb9-128">Hay varias formas de registrar un filtro de excepción API Web:</span><span class="sxs-lookup"><span data-stu-id="42cb9-128">There are several ways to register a Web API exception filter:</span></span>

- <span data-ttu-id="42cb9-129">Por acción</span><span class="sxs-lookup"><span data-stu-id="42cb9-129">By action</span></span>
- <span data-ttu-id="42cb9-130">Por el controlador</span><span class="sxs-lookup"><span data-stu-id="42cb9-130">By controller</span></span>
- <span data-ttu-id="42cb9-131">Globalmente</span><span class="sxs-lookup"><span data-stu-id="42cb9-131">Globally</span></span>

<span data-ttu-id="42cb9-132">Para aplicar el filtro a una acción específica, agregue el filtro como un atributo a la acción:</span><span class="sxs-lookup"><span data-stu-id="42cb9-132">To apply the filter to a specific action, add the filter as an attribute to the action:</span></span>

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

<span data-ttu-id="42cb9-133">Para aplicar el filtro a todas las acciones en un controlador, agregue el filtro como un atributo a la clase de controlador:</span><span class="sxs-lookup"><span data-stu-id="42cb9-133">To apply the filter to all of the actions on a controller, add the filter as an attribute to the controller class:</span></span>

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

<span data-ttu-id="42cb9-134">Para aplicar el filtro globalmente a todos los controladores Web API, agregue una instancia del filtro para el **GlobalConfiguration.Configuration.Filters** colección.</span><span class="sxs-lookup"><span data-stu-id="42cb9-134">To apply the filter globally to all Web API controllers, add an instance of the filter to the **GlobalConfiguration.Configuration.Filters** collection.</span></span> <span data-ttu-id="42cb9-135">Filtros de excepción de esta colección se aplican a cualquier acción de controlador Web API.</span><span class="sxs-lookup"><span data-stu-id="42cb9-135">Exeption filters in this collection apply to any Web API controller action.</span></span>

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

<span data-ttu-id="42cb9-136">Si usa la plantilla de proyecto "Aplicación Web de ASP.NET MVC 4" para crear el proyecto, coloque el código de configuración de Web API dentro de la `WebApiConfig` (clase), que se encuentra en la aplicación\_carpeta de inicio:</span><span class="sxs-lookup"><span data-stu-id="42cb9-136">If you use the "ASP.NET MVC 4 Web Application" project template to create your project, put your Web API configuration code inside the `WebApiConfig` class, which is located in the App\_Start folder:</span></span>

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a><span data-ttu-id="42cb9-137">HttpError</span><span class="sxs-lookup"><span data-stu-id="42cb9-137">HttpError</span></span>

<span data-ttu-id="42cb9-138">El **HttpError** objeto proporciona una manera coherente para devolver información de error en el cuerpo de respuesta.</span><span class="sxs-lookup"><span data-stu-id="42cb9-138">The **HttpError** object provides a consistent way to return error information in the response body.</span></span> <span data-ttu-id="42cb9-139">El ejemplo siguiente muestra cómo devolver el código de estado HTTP 404 (no encontrado) con un **HttpError** en el cuerpo de respuesta.</span><span class="sxs-lookup"><span data-stu-id="42cb9-139">The following example shows how to return HTTP status code 404 (Not Found) with an **HttpError** in the response body.</span></span>

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

<span data-ttu-id="42cb9-140">**Createerrorresponse, tal y** se define un método de extensión en el **System.Net.Http.HttpRequestMessageExtensions** clase.</span><span class="sxs-lookup"><span data-stu-id="42cb9-140">**CreateErrorResponse** is an extension method defined in the **System.Net.Http.HttpRequestMessageExtensions** class.</span></span> <span data-ttu-id="42cb9-141">Internamente, **createerrorresponse, tal y** crea un **HttpError** de instancia y, a continuación, se crea un **HttpResponseMessage** que contiene el **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="42cb9-141">Internally, **CreateErrorResponse** creates an **HttpError** instance and then creates an **HttpResponseMessage** that contains the **HttpError**.</span></span>

<span data-ttu-id="42cb9-142">En este ejemplo, si el método se realiza correctamente, devuelve el producto en la respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="42cb9-142">In this example, if the method is successful, it returns the product in the HTTP response.</span></span> <span data-ttu-id="42cb9-143">Pero si no se encuentra el producto solicitado, la respuesta HTTP contiene un **HttpError** en el cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="42cb9-143">But if the requested product is not found, the HTTP response contains an **HttpError** in the request body.</span></span> <span data-ttu-id="42cb9-144">La respuesta podría ser similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="42cb9-144">The response might look like the following:</span></span>

[!code-console[Main](exception-handling/samples/sample9.cmd)]

<span data-ttu-id="42cb9-145">Tenga en cuenta que el **HttpError** se serializa en JSON en este ejemplo.</span><span class="sxs-lookup"><span data-stu-id="42cb9-145">Notice that the **HttpError** was serialized to JSON in this example.</span></span> <span data-ttu-id="42cb9-146">Una ventaja de usar **HttpError** es que va a través de la misma [negociación de contenido](../formats-and-model-binding/content-negotiation.md) y proceso de serialización como cualquier otro modelo fuertemente tipado.</span><span class="sxs-lookup"><span data-stu-id="42cb9-146">One advantage of using **HttpError** is that it goes through the same [content-negotiation](../formats-and-model-binding/content-negotiation.md) and serialization process as any other strongly-typed model.</span></span>

### <a name="httperror-and-model-validation"></a><span data-ttu-id="42cb9-147">HttpError y validación del modelo</span><span class="sxs-lookup"><span data-stu-id="42cb9-147">HttpError and Model Validation</span></span>

<span data-ttu-id="42cb9-148">Para la validación del modelo, puede pasar el estado del modelo para **createerrorresponse, tal y**, para incluir los errores de validación en la respuesta:</span><span class="sxs-lookup"><span data-stu-id="42cb9-148">For model validation, you can pass the model state to **CreateErrorResponse**, to include the validation errors in the response:</span></span>

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

<span data-ttu-id="42cb9-149">En este ejemplo podría devolver la respuesta siguiente:</span><span class="sxs-lookup"><span data-stu-id="42cb9-149">This example might return the following response:</span></span>

[!code-console[Main](exception-handling/samples/sample11.cmd)]

<span data-ttu-id="42cb9-150">Para obtener más información acerca de la validación del modelo, vea [validación de modelos en ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="42cb9-150">For more information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>

### <a name="using-httperror-with-httpresponseexception"></a><span data-ttu-id="42cb9-151">Uso de HttpError con HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="42cb9-151">Using HttpError with HttpResponseException</span></span>

<span data-ttu-id="42cb9-152">Los ejemplos anteriores devuelven un **HttpResponseMessage** mensaje de la acción de controlador, aunque también puede usar **HttpResponseException** para devolver un **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="42cb9-152">The previous examples return an **HttpResponseMessage** message from the controller action, but you can also use **HttpResponseException** to return an **HttpError**.</span></span> <span data-ttu-id="42cb9-153">Esto le permite devolver un modelo fuertemente tipado en el caso de éxito normal, mientras sigue devolviendo **HttpError** si se produce un error:</span><span class="sxs-lookup"><span data-stu-id="42cb9-153">This lets you return a strongly-typed model in the normal success case, while still returning **HttpError** if there is an error:</span></span>

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
