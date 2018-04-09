---
uid: mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
title: Información general de controlador de MVC de ASP.NET (C#) | Documentos de Microsoft
author: StephenWalther
description: En este tutorial, Stephen Walther presenta controladores MVC de ASP.NET. Obtenga información acerca de cómo crear nuevos controladores y devolver tipos diferentes de res acción...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: b985c49a-3668-455c-a366-f85f6bc64b12
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 95e7c555a52c8c3b765a6fffab15276491cf5714
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-mvc-controller-overview-c"></a><span data-ttu-id="0aa51-104">Información general de controlador de MVC de ASP.NET (C#)</span><span class="sxs-lookup"><span data-stu-id="0aa51-104">ASP.NET MVC Controller Overview (C#)</span></span>
====================
<span data-ttu-id="0aa51-105">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="0aa51-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="0aa51-106">En este tutorial, Stephen Walther presenta controladores MVC de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0aa51-106">In this tutorial, Stephen Walther introduces you to ASP.NET MVC controllers.</span></span> <span data-ttu-id="0aa51-107">Obtenga información acerca de cómo crear nuevos controladores y devolver distintos tipos de resultados de la acción.</span><span class="sxs-lookup"><span data-stu-id="0aa51-107">You learn how to create new controllers and return different types of action results.</span></span>


<span data-ttu-id="0aa51-108">Este tutorial explora el tema de controladores, las acciones de controlador y resultados de la acción de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="0aa51-108">This tutorial explores the topic of ASP.NET MVC controllers, controller actions, and action results.</span></span> <span data-ttu-id="0aa51-109">Después de completar este tutorial, comprenderá cómo se usan los controladores para controlar la manera en que un visitante interactúa con un sitio Web de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="0aa51-109">After you complete this tutorial, you will understand how controllers are used to control the way a visitor interacts with an ASP.NET MVC website.</span></span>

## <a name="understanding-controllers"></a><span data-ttu-id="0aa51-110">Descripción de controladores</span><span class="sxs-lookup"><span data-stu-id="0aa51-110">Understanding Controllers</span></span>

<span data-ttu-id="0aa51-111">Controladores MVC están responsables de responder a las solicitudes realizadas en un sitio Web de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="0aa51-111">MVC controllers are responsible for responding to requests made against an ASP.NET MVC website.</span></span> <span data-ttu-id="0aa51-112">Cada solicitud del explorador se asigna a un controlador determinado.</span><span class="sxs-lookup"><span data-stu-id="0aa51-112">Each browser request is mapped to a particular controller.</span></span> <span data-ttu-id="0aa51-113">Por ejemplo, imagine que escriba la dirección URL siguiente en la barra de direcciones del explorador:</span><span class="sxs-lookup"><span data-stu-id="0aa51-113">For example, imagine that you enter the following URL into the address bar of your browser:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="0aa51-114">En este caso, se invoca un controlador denominado ProductController.</span><span class="sxs-lookup"><span data-stu-id="0aa51-114">In this case, a controller named ProductController is invoked.</span></span> <span data-ttu-id="0aa51-115">El ProductController es responsable de generar la respuesta a la solicitud del explorador.</span><span class="sxs-lookup"><span data-stu-id="0aa51-115">The ProductController is responsible for generating the response to the browser request.</span></span> <span data-ttu-id="0aa51-116">Por ejemplo, el controlador puede devolver una vista determinada al explorador o el controlador podría redireccionar al usuario a otro controlador.</span><span class="sxs-lookup"><span data-stu-id="0aa51-116">For example, the controller might return a particular view back to the browser or the controller might redirect the user to another controller.</span></span>

<span data-ttu-id="0aa51-117">Lista 1 contiene un controlador simple denominado ProductController.</span><span class="sxs-lookup"><span data-stu-id="0aa51-117">Listing 1 contains a simple controller named ProductController.</span></span>

<span data-ttu-id="0aa51-118">**Listado de 1 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="0aa51-118">**Listing1 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample1.cs)]

<span data-ttu-id="0aa51-119">Como puede ver en el listado 1, un controlador es solo una clase (una clase .NET de Visual Basic o C#).</span><span class="sxs-lookup"><span data-stu-id="0aa51-119">As you can see from Listing 1, a controller is just a class (a Visual Basic .NET or C# class).</span></span> <span data-ttu-id="0aa51-120">Un controlador es una clase que deriva de la clase System.Web.Mvc.Controller base.</span><span class="sxs-lookup"><span data-stu-id="0aa51-120">A controller is a class that derives from the base System.Web.Mvc.Controller class.</span></span> <span data-ttu-id="0aa51-121">Puesto que un controlador se hereda de esta clase base, un controlador hereda varios métodos útiles de forma gratuita (se explican estos métodos en un momento).</span><span class="sxs-lookup"><span data-stu-id="0aa51-121">Because a controller inherits from this base class, a controller inherits several useful methods for free (We discuss these methods in a moment).</span></span>

## <a name="understanding-controller-actions"></a><span data-ttu-id="0aa51-122">Descripción de las acciones de controlador</span><span class="sxs-lookup"><span data-stu-id="0aa51-122">Understanding Controller Actions</span></span>

<span data-ttu-id="0aa51-123">Un controlador expone las acciones de controlador.</span><span class="sxs-lookup"><span data-stu-id="0aa51-123">A controller exposes controller actions.</span></span> <span data-ttu-id="0aa51-124">Una acción es un método en un controlador que se invoca cuando escriba una dirección URL determinada en la barra de direcciones del explorador.</span><span class="sxs-lookup"><span data-stu-id="0aa51-124">An action is a method on a controller that gets called when you enter a particular URL in your browser address bar.</span></span> <span data-ttu-id="0aa51-125">Por ejemplo, imagine que se realiza una solicitud para la dirección URL siguiente:</span><span class="sxs-lookup"><span data-stu-id="0aa51-125">For example, imagine that you make a request for the following URL:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="0aa51-126">En este caso, se llama al método de Index() en la clase ProductController.</span><span class="sxs-lookup"><span data-stu-id="0aa51-126">In this case, the Index() method is called on the ProductController class.</span></span> <span data-ttu-id="0aa51-127">El método Index() es un ejemplo de una acción del controlador.</span><span class="sxs-lookup"><span data-stu-id="0aa51-127">The Index() method is an example of a controller action.</span></span>

<span data-ttu-id="0aa51-128">Una acción de controlador debe ser un método público de una clase de controlador.</span><span class="sxs-lookup"><span data-stu-id="0aa51-128">A controller action must be a public method of a controller class.</span></span> <span data-ttu-id="0aa51-129">Métodos de C#, de forma predeterminada, son métodos privados.</span><span class="sxs-lookup"><span data-stu-id="0aa51-129">C# methods, by default, are private methods.</span></span> <span data-ttu-id="0aa51-130">Tenga en cuenta que cualquier método público que se agrega a una clase de controlador se expone como una acción de controlador automáticamente (debe ser cuidado con esto ya que una acción de controlador se puede invocar cualquier usuario en el universo escribiendo la dirección URL correcta en la barra de direcciones del explorador).</span><span class="sxs-lookup"><span data-stu-id="0aa51-130">Realize that any public method that you add to a controller class is exposed as a controller action automatically (You must be careful about this since a controller action can be invoked by anyone in the universe simply by typing the right URL into a browser address bar).</span></span>

<span data-ttu-id="0aa51-131">Hay algunos requisitos adicionales que deben cumplirse mediante una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="0aa51-131">There are some additional requirements that must be satisfied by a controller action.</span></span> <span data-ttu-id="0aa51-132">No se puede sobrecargar un método que se utiliza como una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="0aa51-132">A method used as a controller action cannot be overloaded.</span></span> <span data-ttu-id="0aa51-133">Además, una acción de controlador no puede ser un método estático.</span><span class="sxs-lookup"><span data-stu-id="0aa51-133">Furthermore, a controller action cannot be a static method.</span></span> <span data-ttu-id="0aa51-134">Aparte de eso, puede usar cualquier método como una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="0aa51-134">Other than that, you can use just about any method as a controller action.</span></span>

## <a name="understanding-action-results"></a><span data-ttu-id="0aa51-135">Descripción de los resultados de acción</span><span class="sxs-lookup"><span data-stu-id="0aa51-135">Understanding Action Results</span></span>

<span data-ttu-id="0aa51-136">Lo que se denomina devuelve una acción de controlador un *resultado de la acción*.</span><span class="sxs-lookup"><span data-stu-id="0aa51-136">A controller action returns something called an *action result*.</span></span> <span data-ttu-id="0aa51-137">Un resultado de acción es lo que devuelve una acción de controlador en respuesta a una solicitud del explorador.</span><span class="sxs-lookup"><span data-stu-id="0aa51-137">An action result is what a controller action returns in response to a browser request.</span></span>

<span data-ttu-id="0aa51-138">El marco de MVC de ASP.NET admite varios tipos de resultados de acción incluidas:</span><span class="sxs-lookup"><span data-stu-id="0aa51-138">The ASP.NET MVC framework supports several types of action results including:</span></span>

1. <span data-ttu-id="0aa51-139">ViewResult - representa HTML y marcado.</span><span class="sxs-lookup"><span data-stu-id="0aa51-139">ViewResult - Represents HTML and markup.</span></span>
2. <span data-ttu-id="0aa51-140">EmptyResult - no representa ningún resultado.</span><span class="sxs-lookup"><span data-stu-id="0aa51-140">EmptyResult - Represents no result.</span></span>
3. <span data-ttu-id="0aa51-141">RedirectResult - representa una redirección a una nueva dirección URL.</span><span class="sxs-lookup"><span data-stu-id="0aa51-141">RedirectResult - Represents a redirection to a new URL.</span></span>
4. <span data-ttu-id="0aa51-142">JsonResult - representa un resultado de notación de objetos JavaScript que puede usarse en una aplicación de AJAX.</span><span class="sxs-lookup"><span data-stu-id="0aa51-142">JsonResult - Represents a JavaScript Object Notation result that can be used in an AJAX application.</span></span>
5. <span data-ttu-id="0aa51-143">JavaScriptResult - representa una secuencia de comandos de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0aa51-143">JavaScriptResult - Represents a JavaScript script.</span></span>
6. <span data-ttu-id="0aa51-144">ContentResult - representa un resultado de texto.</span><span class="sxs-lookup"><span data-stu-id="0aa51-144">ContentResult - Represents a text result.</span></span>
7. <span data-ttu-id="0aa51-145">FileContentResult - representa un archivo descargable (con el contenido binario).</span><span class="sxs-lookup"><span data-stu-id="0aa51-145">FileContentResult - Represents a downloadable file (with the binary content).</span></span>
8. <span data-ttu-id="0aa51-146">FilePathResult - representa un archivo descargable (con una ruta de acceso).</span><span class="sxs-lookup"><span data-stu-id="0aa51-146">FilePathResult - Represents a downloadable file (with a path).</span></span>
9. <span data-ttu-id="0aa51-147">FileStreamResult - representa un archivo descargable (con una secuencia de archivo).</span><span class="sxs-lookup"><span data-stu-id="0aa51-147">FileStreamResult - Represents a downloadable file (with a file stream).</span></span>

<span data-ttu-id="0aa51-148">Todos estos resultados de acción de heredan de la clase ActionResult base.</span><span class="sxs-lookup"><span data-stu-id="0aa51-148">All of these action results inherit from the base ActionResult class.</span></span>

<span data-ttu-id="0aa51-149">En la mayoría de los casos, una acción de controlador devuelve un ViewResult.</span><span class="sxs-lookup"><span data-stu-id="0aa51-149">In most cases, a controller action returns a ViewResult.</span></span> <span data-ttu-id="0aa51-150">Por ejemplo, la acción de controlador de índice en el listado 2 devuelve un ViewResult.</span><span class="sxs-lookup"><span data-stu-id="0aa51-150">For example, the Index controller action in Listing 2 returns a ViewResult.</span></span>

<span data-ttu-id="0aa51-151">**La lista 2 - Controllers\BookController.cs**</span><span class="sxs-lookup"><span data-stu-id="0aa51-151">**Listing 2 - Controllers\BookController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample2.cs)]

<span data-ttu-id="0aa51-152">Cuando una acción devuelve un ViewResult, HTML se devuelve al explorador.</span><span class="sxs-lookup"><span data-stu-id="0aa51-152">When an action returns a ViewResult, HTML is returned to the browser.</span></span> <span data-ttu-id="0aa51-153">El método Index() en el listado 2 devuelve una vista con el nombre de índice en el explorador.</span><span class="sxs-lookup"><span data-stu-id="0aa51-153">The Index() method in Listing 2 returns a view named Index to the browser.</span></span>

<span data-ttu-id="0aa51-154">Tenga en cuenta que la acción de Index() en el listado 2 no devuelve un ViewResult().</span><span class="sxs-lookup"><span data-stu-id="0aa51-154">Notice that the Index() action in Listing 2 does not return a ViewResult().</span></span> <span data-ttu-id="0aa51-155">En su lugar, se llama al método de View() de la clase base del controlador.</span><span class="sxs-lookup"><span data-stu-id="0aa51-155">Instead, the View() method of the Controller base class is called.</span></span> <span data-ttu-id="0aa51-156">Normalmente, no se devuelve directamente un resultado de acción.</span><span class="sxs-lookup"><span data-stu-id="0aa51-156">Normally, you do not return an action result directly.</span></span> <span data-ttu-id="0aa51-157">En su lugar, se llame a uno de los siguientes métodos de la clase base del controlador:</span><span class="sxs-lookup"><span data-stu-id="0aa51-157">Instead, you call one of the following methods of the Controller base class:</span></span>

1. <span data-ttu-id="0aa51-158">Ver - devuelve un resultado de acción ViewResult.</span><span class="sxs-lookup"><span data-stu-id="0aa51-158">View - Returns a ViewResult action result.</span></span>
2. <span data-ttu-id="0aa51-159">Redirigir: devuelve un resultado de acción RedirectResult.</span><span class="sxs-lookup"><span data-stu-id="0aa51-159">Redirect - Returns a RedirectResult action result.</span></span>
3. <span data-ttu-id="0aa51-160">RedirectToAction - devuelve un resultado de acción de RedirectToRouteResult.</span><span class="sxs-lookup"><span data-stu-id="0aa51-160">RedirectToAction - Returns a RedirectToRouteResult action result.</span></span>
4. <span data-ttu-id="0aa51-161">RedirectToRoute - devuelve un resultado de acción de RedirectToRouteResult.</span><span class="sxs-lookup"><span data-stu-id="0aa51-161">RedirectToRoute - Returns a RedirectToRouteResult action result.</span></span>
5. <span data-ttu-id="0aa51-162">JSON - devuelve un resultado de acción de JsonResult.</span><span class="sxs-lookup"><span data-stu-id="0aa51-162">Json - Returns a JsonResult action result.</span></span>
6. <span data-ttu-id="0aa51-163">JavaScriptResult - devuelve un JavaScriptResult.</span><span class="sxs-lookup"><span data-stu-id="0aa51-163">JavaScriptResult - Returns a JavaScriptResult.</span></span>
7. <span data-ttu-id="0aa51-164">Content - devuelve un resultado de acción ContentResult.</span><span class="sxs-lookup"><span data-stu-id="0aa51-164">Content - Returns a ContentResult action result.</span></span>
8. <span data-ttu-id="0aa51-165">Archivo - devuelve un FileContentResult, FilePathResult o FileStreamResult dependiendo de los parámetros pasado al método.</span><span class="sxs-lookup"><span data-stu-id="0aa51-165">File - Returns a FileContentResult, FilePathResult, or FileStreamResult depending on the parameters passed to the method.</span></span>

<span data-ttu-id="0aa51-166">Por lo tanto, si desea obtener una vista en el explorador, llama al método View().</span><span class="sxs-lookup"><span data-stu-id="0aa51-166">So, if you want to return a View to the browser, you call the View() method.</span></span> <span data-ttu-id="0aa51-167">Si desea redirigir al usuario de la acción de un controlador, se llame al método RedirectToAction().</span><span class="sxs-lookup"><span data-stu-id="0aa51-167">If you want to redirect the user from one controller action to another, you call the RedirectToAction() method.</span></span> <span data-ttu-id="0aa51-168">Por ejemplo, la acción Details() en la lista 3 muestra una vista o redirige al usuario a la acción de Index() dependiendo de si el parámetro Id tiene un valor.</span><span class="sxs-lookup"><span data-stu-id="0aa51-168">For example, the Details() action in Listing 3 either displays a view or redirects the user to the Index() action depending on whether the Id parameter has a value.</span></span>

<span data-ttu-id="0aa51-169">**El listado 3 - CustomerController.cs**</span><span class="sxs-lookup"><span data-stu-id="0aa51-169">**Listing 3 - CustomerController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample3.cs)]

<span data-ttu-id="0aa51-170">El resultado de acción ContentResult es especial.</span><span class="sxs-lookup"><span data-stu-id="0aa51-170">The ContentResult action result is special.</span></span> <span data-ttu-id="0aa51-171">Puede usar el resultado de acción ContentResult para devolver un resultado de acción como texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="0aa51-171">You can use the ContentResult action result to return an action result as plain text.</span></span> <span data-ttu-id="0aa51-172">Por ejemplo, el método Index() en el listado 4 devuelve un mensaje como texto sin formato y no como HTML.</span><span class="sxs-lookup"><span data-stu-id="0aa51-172">For example, the Index() method in Listing 4 returns a message as plain text and not as HTML.</span></span>

<span data-ttu-id="0aa51-173">**Listado 4 - Controllers\StatusController.cs**</span><span class="sxs-lookup"><span data-stu-id="0aa51-173">**Listing 4 - Controllers\StatusController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample4.cs)]

<span data-ttu-id="0aa51-174">Cuando se invoca la acción StatusController.Index(), no se devuelve una vista.</span><span class="sxs-lookup"><span data-stu-id="0aa51-174">When the StatusController.Index() action is invoked, a view is not returned.</span></span> <span data-ttu-id="0aa51-175">En su lugar, el texto sin formato "¡Hello World!"</span><span class="sxs-lookup"><span data-stu-id="0aa51-175">Instead, the raw text "Hello World!"</span></span> <span data-ttu-id="0aa51-176">se devuelve al explorador.</span><span class="sxs-lookup"><span data-stu-id="0aa51-176">is returned to the browser.</span></span>

<span data-ttu-id="0aa51-177">Si una acción de controlador devuelve un resultado que es no es un resultado de acción: por ejemplo, una fecha o un entero -, a continuación, el resultado se encapsula en un ContentResult automáticamente.</span><span class="sxs-lookup"><span data-stu-id="0aa51-177">If a controller action returns a result that is not an action result - for example, a date or an integer - then the result is wrapped in a ContentResult automatically.</span></span> <span data-ttu-id="0aa51-178">Por ejemplo, cuando se invoca la acción de Index() de la WorkController en el listado 5, la fecha se devuelve como un ContentResult automáticamente.</span><span class="sxs-lookup"><span data-stu-id="0aa51-178">For example, when the Index() action of the WorkController in Listing 5 is invoked, the date is returned as a ContentResult automatically.</span></span>

<span data-ttu-id="0aa51-179">**Listado 5 - WorkController.cs**</span><span class="sxs-lookup"><span data-stu-id="0aa51-179">**Listing 5 - WorkController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample5.cs)]

<span data-ttu-id="0aa51-180">La acción de Index() en el listado 5 devuelve un objeto DateTime.</span><span class="sxs-lookup"><span data-stu-id="0aa51-180">The Index() action in Listing 5 returns a DateTime object.</span></span> <span data-ttu-id="0aa51-181">El marco de MVC de ASP.NET convierte el objeto de fecha y hora en una cadena y ajusta el valor de fecha y hora en un ContentResult automáticamente.</span><span class="sxs-lookup"><span data-stu-id="0aa51-181">The ASP.NET MVC framework converts the DateTime object to a string and wraps the DateTime value in a ContentResult automatically.</span></span> <span data-ttu-id="0aa51-182">El explorador recibe la fecha y hora como texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="0aa51-182">The browser receives the date and time as plain text.</span></span>

## <a name="summary"></a><span data-ttu-id="0aa51-183">Resumen</span><span class="sxs-lookup"><span data-stu-id="0aa51-183">Summary</span></span>

<span data-ttu-id="0aa51-184">El objetivo de este tutorial era para presentarle los conceptos de controladores, las acciones de controlador y resultados de la acción de controlador de MVC de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0aa51-184">The purpose of this tutorial was to introduce you to the concepts of ASP.NET MVC controllers, controller actions, and controller action results.</span></span> <span data-ttu-id="0aa51-185">En la primera sección, aprendió a agregar nuevos controladores a un proyecto de MVC de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0aa51-185">In the first section, you learned how to add new controllers to an ASP.NET MVC project.</span></span> <span data-ttu-id="0aa51-186">A continuación, ha aprendido cómo públicos métodos de un controlador se exponen al universo como acciones del controlador.</span><span class="sxs-lookup"><span data-stu-id="0aa51-186">Next, you learned how public methods of a controller are exposed to the universe as controller actions.</span></span> <span data-ttu-id="0aa51-187">Por último, analizamos los diferentes tipos de resultados de la acción que se pueden devolver desde una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="0aa51-187">Finally, we discussed the different types of action results that can be returned from a controller action.</span></span> <span data-ttu-id="0aa51-188">En concreto, se explicó cómo devolver un ViewResult, RedirectToActionResult y ContentResult desde una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="0aa51-188">In particular, we discussed how to return a ViewResult, RedirectToActionResult, and ContentResult from a controller action.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0aa51-189">[Anterior](creating-an-action-vb.md)
> [Siguiente](creating-custom-routes-cs.md)</span><span class="sxs-lookup"><span data-stu-id="0aa51-189">[Previous](creating-an-action-vb.md)
[Next](creating-custom-routes-cs.md)</span></span>
