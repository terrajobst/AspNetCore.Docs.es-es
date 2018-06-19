---
uid: web-api/overview/error-handling/exception-handling
title: Control de excepciones en ASP.NET Web API | Documentos de Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2012
ms.topic: article
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: c65ddcca012840d70ab5a33af92edb30041be971
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
ms.locfileid: "26506964"
---
<a name="exception-handling-in-aspnet-web-api"></a><span data-ttu-id="5d9aa-102">Control de excepciones en ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="5d9aa-102">Exception Handling in ASP.NET Web API</span></span>
====================
<span data-ttu-id="5d9aa-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5d9aa-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="5d9aa-104">Este artículo describe el error y control de excepciones en ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="5d9aa-104">This article describes error and exception handling in ASP.NET Web API.</span></span>

- [<span data-ttu-id="5d9aa-105">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="5d9aa-105">HttpResponseException</span></span>](#httpresponserexception)
- [<span data-ttu-id="5d9aa-106">Filtros de excepciones</span><span class="sxs-lookup"><span data-stu-id="5d9aa-106">Exception Filters</span></span>](#exception_filters)
- [<span data-ttu-id="5d9aa-107">Registro de filtros de excepciones</span><span class="sxs-lookup"><span data-stu-id="5d9aa-107">Registering Exception Filters</span></span>](#registering_exception_filters)
- [<span data-ttu-id="5d9aa-108">HttpError</span><span class="sxs-lookup"><span data-stu-id="5d9aa-108">HttpError</span></span>](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a><span data-ttu-id="5d9aa-109">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="5d9aa-109">HttpResponseException</span></span>

<span data-ttu-id="5d9aa-110">¿Qué ocurre si un controlador de Web API produce una excepción no detectada?</span><span class="sxs-lookup"><span data-stu-id="5d9aa-110">What happens if a Web API controller throws an uncaught exception?</span></span> <span data-ttu-id="5d9aa-111">De forma predeterminada, la mayoría de las excepciones se traduce a una respuesta HTTP con código de estado 500 Error interno del servidor.</span><span class="sxs-lookup"><span data-stu-id="5d9aa-111">By default, most exceptions are translated into an HTTP response with status code 500, Internal Server Error.</span></span>

<span data-ttu-id="5d9aa-112">El **HttpResponseException** tipo es un caso especial.</span><span class="sxs-lookup"><span data-stu-id="5d9aa-112">The **HttpResponseException** type is a special case.</span></span> <span data-ttu-id="5d9aa-113">Esta excepción devuelve cualquier código de estado HTTP que especifica en el constructor de la excepción.</span><span class="sxs-lookup"><span data-stu-id="5d9aa-113">This exception returns any HTTP status code that you specify in the exception constructor.</span></span> <span data-ttu-id="5d9aa-114">Por ejemplo, el método siguiente devuelve 404, no se encuentra, si la *identificador* parámetro no es válido.</span><span class="sxs-lookup"><span data-stu-id="5d9aa-114">For example, the following method returns 404, Not Found, if the *id* parameter is not valid.</span></span>

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

<span data-ttu-id="5d9aa-115">Para tener más control sobre la respuesta, también puede construir el mensaje de respuesta completo e incluir con el **HttpResponseException:**</span><span class="sxs-lookup"><span data-stu-id="5d9aa-115">For more control over the response, you can also construct the entire response message and include it with the **HttpResponseException:**</span></span> 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a><span data-ttu-id="5d9aa-116">Filtros de excepciones</span><span class="sxs-lookup"><span data-stu-id="5d9aa-116">Exception Filters</span></span>

<span data-ttu-id="5d9aa-117">Puede personalizar cómo API Web controla las excepciones mediante la escritura de un *filtro de excepción*.</span><span class="sxs-lookup"><span data-stu-id="5d9aa-117">You can customize how Web API handles exceptions by writing an *exception filter*.</span></span> <span data-ttu-id="5d9aa-118">Un filtro de excepción se ejecuta cuando un método de controlador produce cualquier excepción no controlada que es *no* una **HttpResponseException** excepción.</span><span class="sxs-lookup"><span data-stu-id="5d9aa-118">An exception filter is executed when a controller method throws any unhandled exception that is *not* an **HttpResponseException** exception.</span></span> <span data-ttu-id="5d9aa-119">El **HttpResponseException** tipo es un caso especial, puesto que se ha diseñado específicamente para devolver una respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="5d9aa-119">The **HttpResponseException** type is a special case, because it is designed specifically for returning an HTTP response.</span></span>

<span data-ttu-id="5d9aa-120">Filtros de excepción implementan la **System.Web.Http.Filters.IExceptionFilter** interfaz.</span><span class="sxs-lookup"><span data-stu-id="5d9aa-120">Exception filters implement the **System.Web.Http.Filters.IExceptionFilter** interface.</span></span> <span data-ttu-id="5d9aa-121">La manera más sencilla de crear un filtro de excepción es derivar desde la **System.Web.Http.Filters.ExceptionFilterAttribute** clase e invalidar el **OnException** método.</span><span class="sxs-lookup"><span data-stu-id="5d9aa-121">The simplest way to write an exception filter is to derive from the **System.Web.Http.Filters.ExceptionFilterAttribute** class and override the **OnException** method.</span></span>

> [!NOTE]
> <span data-ttu-id="5d9aa-122">Los filtros de excepción en ASP.NET Web API son similares a las de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="5d9aa-122">Exception filters in ASP.NET Web API are similar to those in ASP.NET MVC.</span></span> <span data-ttu-id="5d9aa-123">Sin embargo, se declaran en un espacio de nombres independiente y una función por separado.</span><span class="sxs-lookup"><span data-stu-id="5d9aa-123">However, they are declared in a separate namespace and function separately.</span></span> <span data-ttu-id="5d9aa-124">En concreto, el **HandleErrorAttribute** clase usada en MVC no controla las excepciones producidas por controladores de API Web.</span><span class="sxs-lookup"><span data-stu-id="5d9aa-124">In particular, the **HandleErrorAttribute** class used in MVC does not handle exceptions thrown by Web API controllers.</span></span>


<span data-ttu-id="5d9aa-125">Este es un filtro que convierte **NotImplementedException** excepciones en código de estado HTTP 501, no se ha implementado de código:</span><span class="sxs-lookup"><span data-stu-id="5d9aa-125">Here is a filter that converts **NotImplementedException** exceptions into HTTP status code 501, Not Implemented:</span></span>

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

<span data-ttu-id="5d9aa-126">El **respuesta** propiedad de la **HttpActionExecutedContext** objeto contiene el mensaje de respuesta HTTP que se enviará al cliente.</span><span class="sxs-lookup"><span data-stu-id="5d9aa-126">The **Response** property of the **HttpActionExecutedContext** object contains the HTTP response message that will be sent to the client.</span></span>

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a><span data-ttu-id="5d9aa-127">Registro de filtros de excepciones</span><span class="sxs-lookup"><span data-stu-id="5d9aa-127">Registering Exception Filters</span></span>

<span data-ttu-id="5d9aa-128">Hay varias formas de registrar un filtro de excepción Web API:</span><span class="sxs-lookup"><span data-stu-id="5d9aa-128">There are several ways to register a Web API exception filter:</span></span>

- <span data-ttu-id="5d9aa-129">Forma acción</span><span class="sxs-lookup"><span data-stu-id="5d9aa-129">By action</span></span>
- <span data-ttu-id="5d9aa-130">Por el controlador</span><span class="sxs-lookup"><span data-stu-id="5d9aa-130">By controller</span></span>
- <span data-ttu-id="5d9aa-131">Global</span><span class="sxs-lookup"><span data-stu-id="5d9aa-131">Globally</span></span>

<span data-ttu-id="5d9aa-132">Para aplicar el filtro a una acción concreta, agregue el filtro como un atributo a la acción:</span><span class="sxs-lookup"><span data-stu-id="5d9aa-132">To apply the filter to a specific action, add the filter as an attribute to the action:</span></span>

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

<span data-ttu-id="5d9aa-133">Para aplicar el filtro a todas las acciones en un controlador, agregue el filtro como un atributo a la clase de controlador:</span><span class="sxs-lookup"><span data-stu-id="5d9aa-133">To apply the filter to all of the actions on a controller, add the filter as an attribute to the controller class:</span></span>

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

<span data-ttu-id="5d9aa-134">Para aplicar el filtro globalmente en todos los controladores de API Web, debe agregar una instancia del filtro para la **GlobalConfiguration.Configuration.Filters** colección.</span><span class="sxs-lookup"><span data-stu-id="5d9aa-134">To apply the filter globally to all Web API controllers, add an instance of the filter to the **GlobalConfiguration.Configuration.Filters** collection.</span></span> <span data-ttu-id="5d9aa-135">Filtros de excepción de esta colección se aplican a cualquier acción de controlador Web API.</span><span class="sxs-lookup"><span data-stu-id="5d9aa-135">Exeption filters in this collection apply to any Web API controller action.</span></span>

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

<span data-ttu-id="5d9aa-136">Si usa la plantilla de proyecto de "Aplicación Web de ASP.NET MVC 4" para crear el proyecto, colocar el código de configuración de la API Web dentro de la `WebApiConfig` (clase), que se encuentra en la aplicación\_carpeta de inicio:</span><span class="sxs-lookup"><span data-stu-id="5d9aa-136">If you use the "ASP.NET MVC 4 Web Application" project template to create your project, put your Web API configuration code inside the `WebApiConfig` class, which is located in the App\_Start folder:</span></span>

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a><span data-ttu-id="5d9aa-137">HttpError</span><span class="sxs-lookup"><span data-stu-id="5d9aa-137">HttpError</span></span>

<span data-ttu-id="5d9aa-138">El **HttpError** objeto proporciona una manera coherente para devolver información de error en el cuerpo de respuesta.</span><span class="sxs-lookup"><span data-stu-id="5d9aa-138">The **HttpError** object provides a consistent way to return error information in the response body.</span></span> <span data-ttu-id="5d9aa-139">En el ejemplo siguiente se muestra cómo devolver el código de estado HTTP 404 (no encontrado) con un **HttpError** en el cuerpo de respuesta.</span><span class="sxs-lookup"><span data-stu-id="5d9aa-139">The following example shows how to return HTTP status code 404 (Not Found) with an **HttpError** in the response body.</span></span>

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

<span data-ttu-id="5d9aa-140">**CreateErrorResponse** se define un método de extensión en la **System.Net.Http.HttpRequestMessageExtensions** clase.</span><span class="sxs-lookup"><span data-stu-id="5d9aa-140">**CreateErrorResponse** is an extension method defined in the **System.Net.Http.HttpRequestMessageExtensions** class.</span></span> <span data-ttu-id="5d9aa-141">Internamente, **CreateErrorResponse** crea un **HttpError** de la instancia y, a continuación, se crea un **HttpResponseMessage** que contiene el **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="5d9aa-141">Internally, **CreateErrorResponse** creates an **HttpError** instance and then creates an **HttpResponseMessage** that contains the **HttpError**.</span></span>

<span data-ttu-id="5d9aa-142">En este ejemplo, si el método se realiza correctamente, devuelve el producto en la respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="5d9aa-142">In this example, if the method is successful, it returns the product in the HTTP response.</span></span> <span data-ttu-id="5d9aa-143">Pero si el producto solicitado no se encuentra, la respuesta HTTP contiene un **HttpError** en el cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="5d9aa-143">But if the requested product is not found, the HTTP response contains an **HttpError** in the request body.</span></span> <span data-ttu-id="5d9aa-144">La respuesta podría ser similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="5d9aa-144">The response might look like the following:</span></span>

[!code-console[Main](exception-handling/samples/sample9.cmd)]

<span data-ttu-id="5d9aa-145">Tenga en cuenta que la **HttpError** se serializa a JSON en este ejemplo.</span><span class="sxs-lookup"><span data-stu-id="5d9aa-145">Notice that the **HttpError** was serialized to JSON in this example.</span></span> <span data-ttu-id="5d9aa-146">Una ventaja de usar **HttpError** es que se lleva a cabo la misma [negociación de contenido](../formats-and-model-binding/content-negotiation.md) y proceso de serialización que cualquier otro modelo fuertemente tipado.</span><span class="sxs-lookup"><span data-stu-id="5d9aa-146">One advantage of using **HttpError** is that it goes through the same [content-negotiation](../formats-and-model-binding/content-negotiation.md) and serialization process as any other strongly-typed model.</span></span>

### <a name="httperror-and-model-validation"></a><span data-ttu-id="5d9aa-147">HttpError y validación del modelo</span><span class="sxs-lookup"><span data-stu-id="5d9aa-147">HttpError and Model Validation</span></span>

<span data-ttu-id="5d9aa-148">Para la validación del modelo, puede pasar el estado del modelo para **CreateErrorResponse**, para incluir los errores de validación en la respuesta:</span><span class="sxs-lookup"><span data-stu-id="5d9aa-148">For model validation, you can pass the model state to **CreateErrorResponse**, to include the validation errors in the response:</span></span>

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

<span data-ttu-id="5d9aa-149">En este ejemplo podría devolver la respuesta siguiente:</span><span class="sxs-lookup"><span data-stu-id="5d9aa-149">This example might return the following response:</span></span>

[!code-console[Main](exception-handling/samples/sample11.cmd)]

<span data-ttu-id="5d9aa-150">Para obtener más información acerca de la validación del modelo, vea [validación del modelo en ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="5d9aa-150">For more information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>

### <a name="using-httperror-with-httpresponseexception"></a><span data-ttu-id="5d9aa-151">Usar HttpError con HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="5d9aa-151">Using HttpError with HttpResponseException</span></span>

<span data-ttu-id="5d9aa-152">Los ejemplos anteriores devuelven un **HttpResponseMessage** mensaje desde la acción de controlador, pero también puede usar **HttpResponseException** para devolver un **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="5d9aa-152">The previous examples return an **HttpResponseMessage** message from the controller action, but you can also use **HttpResponseException** to return an **HttpError**.</span></span> <span data-ttu-id="5d9aa-153">Esto le permite devolver un modelo fuertemente tipadas en el caso de éxito normal, mientras se sigue devolviendo **HttpError** si se produce un error:</span><span class="sxs-lookup"><span data-stu-id="5d9aa-153">This lets you return a strongly-typed model in the normal success case, while still returning **HttpError** if there is an error:</span></span>

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
