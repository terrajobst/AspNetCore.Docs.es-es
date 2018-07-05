---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
title: Información general sobre el controlador de MVC de ASP.NET (VB) | Microsoft Docs
author: StephenWalther
description: En este tutorial, Stephen Walther presenta los controladores ASP.NET MVC. Aprenda a crear nuevos controladores y devolver tipos diferentes de res de acción...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: 94c3e5d9-a904-445e-a34e-d92fd1ca108a
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: d1fa2e71ae37d9123b489511c57ae0f79c0802ff
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371783"
---
<a name="aspnet-mvc-controller-overview-vb"></a><span data-ttu-id="efa52-104">Información general sobre el controlador de MVC de ASP.NET (VB)</span><span class="sxs-lookup"><span data-stu-id="efa52-104">ASP.NET MVC Controller Overview (VB)</span></span>
====================
<span data-ttu-id="efa52-105">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="efa52-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="efa52-106">En este tutorial, Stephen Walther presenta los controladores ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="efa52-106">In this tutorial, Stephen Walther introduces you to ASP.NET MVC controllers.</span></span> <span data-ttu-id="efa52-107">Aprenda a crear nuevos controladores y devolver tipos diferentes de los resultados de acción.</span><span class="sxs-lookup"><span data-stu-id="efa52-107">You learn how to create new controllers and return different types of action results.</span></span>


<span data-ttu-id="efa52-108">Este tutorial exploran el tema de los controladores de ASP.NET MVC, las acciones de controlador y los resultados de acción.</span><span class="sxs-lookup"><span data-stu-id="efa52-108">This tutorial explores the topic of ASP.NET MVC controllers, controller actions, and action results.</span></span> <span data-ttu-id="efa52-109">Después de completar este tutorial, comprenderá cómo se usan los controladores para controlar la manera en que un visitante interactúa con un sitio Web de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="efa52-109">After you complete this tutorial, you will understand how controllers are used to control the way a visitor interacts with an ASP.NET MVC website.</span></span>

## <a name="understanding-controllers"></a><span data-ttu-id="efa52-110">Descripción de los controladores</span><span class="sxs-lookup"><span data-stu-id="efa52-110">Understanding Controllers</span></span>

<span data-ttu-id="efa52-111">Los controladores MVC están responsables de responder a las solicitudes realizadas en un sitio Web de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="efa52-111">MVC controllers are responsible for responding to requests made against an ASP.NET MVC website.</span></span> <span data-ttu-id="efa52-112">Cada solicitud del explorador se asigna a un controlador determinado.</span><span class="sxs-lookup"><span data-stu-id="efa52-112">Each browser request is mapped to a particular controller.</span></span> <span data-ttu-id="efa52-113">Por ejemplo, imagine que escriba la dirección URL siguiente en la barra de direcciones del explorador:</span><span class="sxs-lookup"><span data-stu-id="efa52-113">For example, imagine that you enter the following URL into the address bar of your browser:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="efa52-114">En este caso, se invoca un controlador llamado ProductController.</span><span class="sxs-lookup"><span data-stu-id="efa52-114">In this case, a controller named ProductController is invoked.</span></span> <span data-ttu-id="efa52-115">ProductController es responsable de generar la respuesta a la solicitud del explorador.</span><span class="sxs-lookup"><span data-stu-id="efa52-115">The ProductController is responsible for generating the response to the browser request.</span></span> <span data-ttu-id="efa52-116">Por ejemplo, el controlador puede devolver una vista concreta al explorador o el controlador podría redireccionar al usuario a otro controlador.</span><span class="sxs-lookup"><span data-stu-id="efa52-116">For example, the controller might return a particular view back to the browser or the controller might redirect the user to another controller.</span></span>

<span data-ttu-id="efa52-117">Listado 1 contiene un controlador simple llamado ProductController.</span><span class="sxs-lookup"><span data-stu-id="efa52-117">Listing 1 contains a simple controller named ProductController.</span></span>

<span data-ttu-id="efa52-118">**Listado 1 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="efa52-118">**Listing1 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample1.vb)]

<span data-ttu-id="efa52-119">Como puede ver en el listado 1, un controlador es simplemente una clase (una clase de C# o Visual Basic. NET).</span><span class="sxs-lookup"><span data-stu-id="efa52-119">As you can see from Listing 1, a controller is just a class (a Visual Basic .NET or C# class).</span></span> <span data-ttu-id="efa52-120">Un controlador es una clase que derive de la clase System.Web.Mvc.Controller base.</span><span class="sxs-lookup"><span data-stu-id="efa52-120">A controller is a class that derives from the base System.Web.Mvc.Controller class.</span></span> <span data-ttu-id="efa52-121">Dado que un controlador que se hereda de esta clase base, un controlador hereda varios métodos útiles de forma gratuita (se describen estos métodos en un momento).</span><span class="sxs-lookup"><span data-stu-id="efa52-121">Because a controller inherits from this base class, a controller inherits several useful methods for free (We discuss these methods in a moment).</span></span>

## <a name="understanding-controller-actions"></a><span data-ttu-id="efa52-122">Descripción de las acciones de controlador</span><span class="sxs-lookup"><span data-stu-id="efa52-122">Understanding Controller Actions</span></span>

<span data-ttu-id="efa52-123">Un controlador expone las acciones de controlador.</span><span class="sxs-lookup"><span data-stu-id="efa52-123">A controller exposes controller actions.</span></span> <span data-ttu-id="efa52-124">Una acción es un método en un controlador que se invoca cuando escriba una dirección URL determinada en la barra de direcciones del explorador.</span><span class="sxs-lookup"><span data-stu-id="efa52-124">An action is a method on a controller that gets called when you enter a particular URL in your browser address bar.</span></span> <span data-ttu-id="efa52-125">Por ejemplo, imagine que realiza una solicitud para la dirección URL siguiente:</span><span class="sxs-lookup"><span data-stu-id="efa52-125">For example, imagine that you make a request for the following URL:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="efa52-126">En este caso, se llama al método de Index() en la clase ProductController.</span><span class="sxs-lookup"><span data-stu-id="efa52-126">In this case, the Index() method is called on the ProductController class.</span></span> <span data-ttu-id="efa52-127">El método Index() es un ejemplo de una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="efa52-127">The Index() method is an example of a controller action.</span></span>

<span data-ttu-id="efa52-128">Una acción de controlador debe ser un método público de una clase de controlador.</span><span class="sxs-lookup"><span data-stu-id="efa52-128">A controller action must be a public method of a controller class.</span></span> <span data-ttu-id="efa52-129">Métodos de Visual Basic.NET, de forma predeterminada, son métodos públicos.</span><span class="sxs-lookup"><span data-stu-id="efa52-129">Visual Basic.NET methods, by default, are public methods.</span></span> <span data-ttu-id="efa52-130">Tenga en cuenta que cualquier método público que se agrega a una clase de controlador se expone como una acción de controlador automáticamente (debe tener cuidado esto como una acción de controlador se puede invocar cualquier usuario en el universo escribiendo la URL a la derecha en la barra de direcciones del explorador).</span><span class="sxs-lookup"><span data-stu-id="efa52-130">Realize that any public method that you add to a controller class is exposed as a controller action automatically (You must be careful about this since a controller action can be invoked by anyone in the universe simply by typing the right URL into a browser address bar).</span></span>

<span data-ttu-id="efa52-131">Hay algunos requisitos adicionales que deben cumplirse mediante una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="efa52-131">There are some additional requirements that must be satisfied by a controller action.</span></span> <span data-ttu-id="efa52-132">No se puede sobrecargar un método usado como una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="efa52-132">A method used as a controller action cannot be overloaded.</span></span> <span data-ttu-id="efa52-133">Además, una acción de controlador no puede ser un método estático.</span><span class="sxs-lookup"><span data-stu-id="efa52-133">Furthermore, a controller action cannot be a static method.</span></span> <span data-ttu-id="efa52-134">Aparte de eso, puede usar cualquier método como una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="efa52-134">Other than that, you can use just about any method as a controller action.</span></span>

## <a name="understanding-action-results"></a><span data-ttu-id="efa52-135">Descripción de los resultados de acción</span><span class="sxs-lookup"><span data-stu-id="efa52-135">Understanding Action Results</span></span>

<span data-ttu-id="efa52-136">Devuelve una acción de controlador algo llamado un *resultado de acción*.</span><span class="sxs-lookup"><span data-stu-id="efa52-136">A controller action returns something called an *action result*.</span></span> <span data-ttu-id="efa52-137">Un resultado de acción es lo que devuelve una acción de controlador en respuesta a una solicitud del explorador.</span><span class="sxs-lookup"><span data-stu-id="efa52-137">An action result is what a controller action returns in response to a browser request.</span></span>

<span data-ttu-id="efa52-138">El marco de ASP.NET MVC admite varios tipos de resultados de acción incluidos:</span><span class="sxs-lookup"><span data-stu-id="efa52-138">The ASP.NET MVC framework supports several types of action results including:</span></span>

1. <span data-ttu-id="efa52-139">ViewResult - representa HTML y marcado.</span><span class="sxs-lookup"><span data-stu-id="efa52-139">ViewResult - Represents HTML and markup.</span></span>
2. <span data-ttu-id="efa52-140">EmptyResult - no representa ningún resultado.</span><span class="sxs-lookup"><span data-stu-id="efa52-140">EmptyResult - Represents no result.</span></span>
3. <span data-ttu-id="efa52-141">RedirectResult - representa una redirección a una nueva dirección URL.</span><span class="sxs-lookup"><span data-stu-id="efa52-141">RedirectResult - Represents a redirection to a new URL.</span></span>
4. <span data-ttu-id="efa52-142">JsonResult - representa un resultado de la notación de objetos JavaScript que puede usarse en una aplicación de AJAX.</span><span class="sxs-lookup"><span data-stu-id="efa52-142">JsonResult - Represents a JavaScript Object Notation result that can be used in an AJAX application.</span></span>
5. <span data-ttu-id="efa52-143">JavaScriptResult - representa una secuencia de comandos de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="efa52-143">JavaScriptResult - Represents a JavaScript script.</span></span>
6. <span data-ttu-id="efa52-144">ContentResult - representa un resultado de texto.</span><span class="sxs-lookup"><span data-stu-id="efa52-144">ContentResult - Represents a text result.</span></span>
7. <span data-ttu-id="efa52-145">FileContentResult - representa un archivo descargable (con el contenido binario).</span><span class="sxs-lookup"><span data-stu-id="efa52-145">FileContentResult - Represents a downloadable file (with the binary content).</span></span>
8. <span data-ttu-id="efa52-146">FilePathResult - representa un archivo descargable (con una ruta de acceso).</span><span class="sxs-lookup"><span data-stu-id="efa52-146">FilePathResult - Represents a downloadable file (with a path).</span></span>
9. <span data-ttu-id="efa52-147">FileStreamResult - representa un archivo descargable (con una secuencia de archivo).</span><span class="sxs-lookup"><span data-stu-id="efa52-147">FileStreamResult - Represents a downloadable file (with a file stream).</span></span>

<span data-ttu-id="efa52-148">Todos estos resultados de acción se heredan de la clase ActionResult base.</span><span class="sxs-lookup"><span data-stu-id="efa52-148">All of these action results inherit from the base ActionResult class.</span></span>

<span data-ttu-id="efa52-149">En la mayoría de los casos, una acción de controlador devuelve ViewResult.</span><span class="sxs-lookup"><span data-stu-id="efa52-149">In most cases, a controller action returns a ViewResult.</span></span> <span data-ttu-id="efa52-150">Por ejemplo, la acción de controlador de índice en el listado 2 devuelve ViewResult.</span><span class="sxs-lookup"><span data-stu-id="efa52-150">For example, the Index controller action in Listing 2 returns a ViewResult.</span></span>

<span data-ttu-id="efa52-151">**Listado 2 - Controllers\BookController.vb**</span><span class="sxs-lookup"><span data-stu-id="efa52-151">**Listing 2 - Controllers\BookController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample2.vb)]

<span data-ttu-id="efa52-152">Cuando una acción que devuelve un ViewResult HTML se devuelve al explorador.</span><span class="sxs-lookup"><span data-stu-id="efa52-152">When an action returns a ViewResult, HTML is returned to the browser.</span></span> <span data-ttu-id="efa52-153">El método Index() en el listado 2 devuelve una vista llamada Index en el explorador.</span><span class="sxs-lookup"><span data-stu-id="efa52-153">The Index() method in Listing 2 returns a view named Index to the browser.</span></span>

<span data-ttu-id="efa52-154">Tenga en cuenta que la acción de Index() en el listado 2 no devuelve un ViewResult().</span><span class="sxs-lookup"><span data-stu-id="efa52-154">Notice that the Index() action in Listing 2 does not return a ViewResult().</span></span> <span data-ttu-id="efa52-155">En su lugar, se llama al método View() de la clase base del controlador.</span><span class="sxs-lookup"><span data-stu-id="efa52-155">Instead, the View() method of the Controller base class is called.</span></span> <span data-ttu-id="efa52-156">Normalmente, no se devuelven directamente un resultado de acción.</span><span class="sxs-lookup"><span data-stu-id="efa52-156">Normally, you do not return an action result directly.</span></span> <span data-ttu-id="efa52-157">En su lugar, llamar a uno de los métodos siguientes de la clase base del controlador:</span><span class="sxs-lookup"><span data-stu-id="efa52-157">Instead, you call one of the following methods of the Controller base class:</span></span>

1. <span data-ttu-id="efa52-158">Ver: devuelve un resultado de acción ViewResult.</span><span class="sxs-lookup"><span data-stu-id="efa52-158">View - Returns a ViewResult action result.</span></span>
2. <span data-ttu-id="efa52-159">Redirigir: devuelve un resultado de acción RedirectResult.</span><span class="sxs-lookup"><span data-stu-id="efa52-159">Redirect - Returns a RedirectResult action result.</span></span>
3. <span data-ttu-id="efa52-160">RedirectToAction - devuelve un resultado de acción RedirectToRouteResult.</span><span class="sxs-lookup"><span data-stu-id="efa52-160">RedirectToAction - Returns a RedirectToRouteResult action result.</span></span>
4. <span data-ttu-id="efa52-161">RedirectToRoute - devuelve un resultado de acción RedirectToRouteResult.</span><span class="sxs-lookup"><span data-stu-id="efa52-161">RedirectToRoute - Returns a RedirectToRouteResult action result.</span></span>
5. <span data-ttu-id="efa52-162">JSON - devuelve un resultado de acción JsonResult.</span><span class="sxs-lookup"><span data-stu-id="efa52-162">Json - Returns a JsonResult action result.</span></span>
6. <span data-ttu-id="efa52-163">JavaScriptResult - devuelve un JavaScriptResult.</span><span class="sxs-lookup"><span data-stu-id="efa52-163">JavaScriptResult - Returns a JavaScriptResult.</span></span>
7. <span data-ttu-id="efa52-164">Content - devuelve un resultado de acción ContentResult.</span><span class="sxs-lookup"><span data-stu-id="efa52-164">Content - Returns a ContentResult action result.</span></span>
8. <span data-ttu-id="efa52-165">Archivo - devuelve un FileContentResult, FilePathResult o FileStreamResult dependiendo de los parámetros que se pasa al método.</span><span class="sxs-lookup"><span data-stu-id="efa52-165">File - Returns a FileContentResult, FilePathResult, or FileStreamResult depending on the parameters passed to the method.</span></span>

<span data-ttu-id="efa52-166">Por lo tanto, si desea devolver una vista en el explorador, se llame al método View().</span><span class="sxs-lookup"><span data-stu-id="efa52-166">So, if you want to return a View to the browser, you call the View() method.</span></span> <span data-ttu-id="efa52-167">Si desea redirigir al usuario de acción de un controlador a otro, llama al método RedirectToAction().</span><span class="sxs-lookup"><span data-stu-id="efa52-167">If you want to redirect the user from one controller action to another, you call the RedirectToAction() method.</span></span> <span data-ttu-id="efa52-168">Por ejemplo, la acción Details() en el listado 3 muestra una vista o redirige al usuario a la acción de Index() dependiendo de si el parámetro Id tiene un valor.</span><span class="sxs-lookup"><span data-stu-id="efa52-168">For example, the Details() action in Listing 3 either displays a view or redirects the user to the Index() action depending on whether the Id parameter has a value.</span></span>

<span data-ttu-id="efa52-169">**Listado 3 - CustomerController.vb**</span><span class="sxs-lookup"><span data-stu-id="efa52-169">**Listing 3 - CustomerController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample3.vb)]

<span data-ttu-id="efa52-170">El resultado de acción ContentResult es especial.</span><span class="sxs-lookup"><span data-stu-id="efa52-170">The ContentResult action result is special.</span></span> <span data-ttu-id="efa52-171">Puede usar el resultado de acción ContentResult para devolver un resultado de acción como texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="efa52-171">You can use the ContentResult action result to return an action result as plain text.</span></span> <span data-ttu-id="efa52-172">Por ejemplo, el método Index() en el listado 4 devuelve un mensaje como texto sin formato y no como HTML.</span><span class="sxs-lookup"><span data-stu-id="efa52-172">For example, the Index() method in Listing 4 returns a message as plain text and not as HTML.</span></span>

<span data-ttu-id="efa52-173">**Listado 4 - Controllers\StatusController.vb**</span><span class="sxs-lookup"><span data-stu-id="efa52-173">**Listing 4 - Controllers\StatusController.vb**</span></span>

> <span data-ttu-id="efa52-174">StatusController</span><span class="sxs-lookup"><span data-stu-id="efa52-174">StatusController</span></span>
> 
> 
> <span data-ttu-id="efa52-175">System.Web.Mvc.Controller</span><span class="sxs-lookup"><span data-stu-id="efa52-175">System.Web.Mvc.Controller</span></span>


[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample4.vb)]

<span data-ttu-id="efa52-176">Cuando se invoca la acción StatusController.Index(), no se devuelve una vista.</span><span class="sxs-lookup"><span data-stu-id="efa52-176">When the StatusController.Index() action is invoked, a view is not returned.</span></span> <span data-ttu-id="efa52-177">En su lugar, el texto sin formato "¡Hello World!"</span><span class="sxs-lookup"><span data-stu-id="efa52-177">Instead, the raw text "Hello World!"</span></span> <span data-ttu-id="efa52-178">se devuelve al explorador.</span><span class="sxs-lookup"><span data-stu-id="efa52-178">is returned to the browser.</span></span>

<span data-ttu-id="efa52-179">Si una acción de controlador devuelve un resultado que es no es un resultado de acción - por ejemplo, una fecha o un entero -, a continuación, el resultado se encapsula en un ContentResult automáticamente.</span><span class="sxs-lookup"><span data-stu-id="efa52-179">If a controller action returns a result that is not an action result - for example, a date or an integer - then the result is wrapped in a ContentResult automatically.</span></span> <span data-ttu-id="efa52-180">Por ejemplo, cuando se invoca la acción de Index() de la WorkController en el listado 5, la fecha se devuelve como un ContentResult automáticamente.</span><span class="sxs-lookup"><span data-stu-id="efa52-180">For example, when the Index() action of the WorkController in Listing 5 is invoked, the date is returned as a ContentResult automatically.</span></span>

<span data-ttu-id="efa52-181">**Listado 5 - WorkController.vb**</span><span class="sxs-lookup"><span data-stu-id="efa52-181">**Listing 5 - WorkController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample5.vb)]

<span data-ttu-id="efa52-182">La acción de Index() en el listado 5 devuelve un objeto DateTime.</span><span class="sxs-lookup"><span data-stu-id="efa52-182">The Index() action in Listing 5 returns a DateTime object.</span></span> <span data-ttu-id="efa52-183">El marco de MVC de ASP.NET convierte el objeto de fecha y hora en una cadena y ajusta el valor de fecha y hora en un ContentResult automáticamente.</span><span class="sxs-lookup"><span data-stu-id="efa52-183">The ASP.NET MVC framework converts the DateTime object to a string and wraps the DateTime value in a ContentResult automatically.</span></span> <span data-ttu-id="efa52-184">El explorador recibe la fecha y hora como texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="efa52-184">The browser receives the date and time as plain text.</span></span>

## <a name="summary"></a><span data-ttu-id="efa52-185">Resumen</span><span class="sxs-lookup"><span data-stu-id="efa52-185">Summary</span></span>

<span data-ttu-id="efa52-186">El propósito de este tutorial era para presentarle a los conceptos de controladores de ASP.NET MVC, las acciones de controlador y los resultados de acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="efa52-186">The purpose of this tutorial was to introduce you to the concepts of ASP.NET MVC controllers, controller actions, and controller action results.</span></span> <span data-ttu-id="efa52-187">En la primera sección, ha aprendido cómo agregar controladores nuevos a un proyecto ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="efa52-187">In the first section, you learned how to add new controllers to an ASP.NET MVC project.</span></span> <span data-ttu-id="efa52-188">A continuación, ha aprendido cómo públicos métodos de un controlador se exponen al universo como acciones de controlador.</span><span class="sxs-lookup"><span data-stu-id="efa52-188">Next, you learned how public methods of a controller are exposed to the universe as controller actions.</span></span> <span data-ttu-id="efa52-189">Finalmente, se analizan los diferentes tipos de resultados de acción que se pueden devolver desde una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="efa52-189">Finally, we discussed the different types of action results that can be returned from a controller action.</span></span> <span data-ttu-id="efa52-190">En concreto, explicamos cómo devolver un ViewResult, RedirectToActionResult y ContentResult desde una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="efa52-190">In particular, we discussed how to return a ViewResult, RedirectToActionResult, and ContentResult from a controller action.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="efa52-191">[Anterior](creating-a-custom-route-constraint-cs.md)
> [Siguiente](creating-custom-routes-vb.md)</span><span class="sxs-lookup"><span data-stu-id="efa52-191">[Previous](creating-a-custom-route-constraint-cs.md)
[Next](creating-custom-routes-vb.md)</span></span>
