---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
title: Crear pruebas unitarias para aplicaciones de MVC de ASP.NET (VB) | Documentos de Microsoft
author: StephenWalther
description: Obtenga información acerca de cómo crear pruebas unitarias para acciones de controlador. En este tutorial, Stephen Walther muestra cómo probar si una acción de controlador devuelve un ParteI...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: eb35710d-1d99-44ac-b61f-e50af8cb328a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
msc.type: authoredcontent
ms.openlocfilehash: 299665f45d72fee33f92344ed53c87dfb1a76d60
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869678"
---
<a name="creating-unit-tests-for-aspnet-mvc-applications-vb"></a><span data-ttu-id="a0456-104">Crear pruebas unitarias para aplicaciones de MVC de ASP.NET (VB)</span><span class="sxs-lookup"><span data-stu-id="a0456-104">Creating Unit Tests for ASP.NET MVC Applications (VB)</span></span>
====================
<span data-ttu-id="a0456-105">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="a0456-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

[<span data-ttu-id="a0456-106">Descarga de PDF</span><span class="sxs-lookup"><span data-stu-id="a0456-106">Download PDF</span></span>](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_VB.pdf)

> <span data-ttu-id="a0456-107">Obtenga información acerca de cómo crear pruebas unitarias para acciones de controlador.</span><span class="sxs-lookup"><span data-stu-id="a0456-107">Learn how to create unit tests for controller actions.</span></span> <span data-ttu-id="a0456-108">En este tutorial, Stephen Walther muestra cómo probar si una acción de controlador devuelve una vista determinada, devuelve un conjunto de datos determinado o devuelve un tipo diferente del resultado de la acción.</span><span class="sxs-lookup"><span data-stu-id="a0456-108">In this tutorial, Stephen Walther demonstrates how to test whether a controller action returns a particular view, returns a particular set of data, or returns a different type of action result.</span></span>


<span data-ttu-id="a0456-109">El objetivo de este tutorial es mostrar cómo se pueden escribir pruebas unitarias para los controladores en su ASP.NET MVC aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="a0456-109">The goal of this tutorial is to demonstrate how you can write unit tests for the controllers in your ASP.NET MVC applications.</span></span> <span data-ttu-id="a0456-110">Se describe cómo crear tres tipos diferentes de las pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="a0456-110">We discuss how to build three different types of unit tests.</span></span> <span data-ttu-id="a0456-111">Obtenga información acerca de cómo probar la vista devuelta por una acción de controlador, cómo los datos de vista devueltos por una acción de controlador de pruebas y cómo comprobar si una acción de controlador le redirige a una segunda acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="a0456-111">You learn how to test the view returned by a controller action, how to test the View Data returned by a controller action, and how to test whether or not one controller action redirects you to a second controller action.</span></span>

## <a name="creating-the-controller-under-test"></a><span data-ttu-id="a0456-112">Crear el controlador de pruebas</span><span class="sxs-lookup"><span data-stu-id="a0456-112">Creating the Controller under Test</span></span>

<span data-ttu-id="a0456-113">Empecemos creando el controlador que se va a probar.</span><span class="sxs-lookup"><span data-stu-id="a0456-113">Let's start by creating the controller that we intend to test.</span></span> <span data-ttu-id="a0456-114">El controlador, denominado el `ProductController`, se encuentra en la lista 1.</span><span class="sxs-lookup"><span data-stu-id="a0456-114">The controller, named the `ProductController`, is contained in Listing 1.</span></span>

<span data-ttu-id="a0456-115">**Lista 1: `ProductController.vb`**</span><span class="sxs-lookup"><span data-stu-id="a0456-115">**Listing 1 – `ProductController.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample1.vb)]

<span data-ttu-id="a0456-116">El `ProductController` contiene dos métodos de acción denominados `Index()` y `Details()`.</span><span class="sxs-lookup"><span data-stu-id="a0456-116">The `ProductController` contains two action methods named `Index()` and `Details()`.</span></span> <span data-ttu-id="a0456-117">Ambos métodos de acción devuelven una vista.</span><span class="sxs-lookup"><span data-stu-id="a0456-117">Both action methods return a view.</span></span> <span data-ttu-id="a0456-118">Tenga en cuenta que el `Details()` acción acepta un parámetro denominado identificador.</span><span class="sxs-lookup"><span data-stu-id="a0456-118">Notice that the `Details()` action accepts a parameter named Id.</span></span>

## <a name="testing-the-view-returned-by-a-controller"></a><span data-ttu-id="a0456-119">Probar la vista devuelto por un controlador</span><span class="sxs-lookup"><span data-stu-id="a0456-119">Testing the View returned by a Controller</span></span>

<span data-ttu-id="a0456-120">Suponga que desea probar o no el `ProductController` devuelve la vista derecha.</span><span class="sxs-lookup"><span data-stu-id="a0456-120">Imagine that we want to test whether or not the `ProductController` returns the right view.</span></span> <span data-ttu-id="a0456-121">Queremos para asegurarse de que, cuando el `ProductController.Details()` se invoca la acción, se devuelve la vista de detalles.</span><span class="sxs-lookup"><span data-stu-id="a0456-121">We want to make sure that when the `ProductController.Details()` action is invoked, the Details view is returned.</span></span> <span data-ttu-id="a0456-122">La clase de prueba en el listado 2 contiene una prueba unitaria para probar la vista devuelve el `ProductController.Details()` acción.</span><span class="sxs-lookup"><span data-stu-id="a0456-122">The test class in Listing 2 contains a unit test for testing the view returned by the `ProductController.Details()` action.</span></span>

<span data-ttu-id="a0456-123">**La lista 2: `ProductControllerTest.vb`**</span><span class="sxs-lookup"><span data-stu-id="a0456-123">**Listing 2 – `ProductControllerTest.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample2.vb)]

<span data-ttu-id="a0456-124">La clase en el listado 2 incluye un método de prueba denominado `TestDetailsView()`.</span><span class="sxs-lookup"><span data-stu-id="a0456-124">The class in Listing 2 includes a test method named `TestDetailsView()`.</span></span> <span data-ttu-id="a0456-125">Este método contiene tres líneas de código.</span><span class="sxs-lookup"><span data-stu-id="a0456-125">This method contains three lines of code.</span></span> <span data-ttu-id="a0456-126">La primera línea de código crea una nueva instancia de la `ProductController` clase.</span><span class="sxs-lookup"><span data-stu-id="a0456-126">The first line of code creates a new instance of the `ProductController` class.</span></span> <span data-ttu-id="a0456-127">La segunda línea de código, invoca el controlador `Details()` método de acción.</span><span class="sxs-lookup"><span data-stu-id="a0456-127">The second line of code invokes the controller's `Details()` action method.</span></span> <span data-ttu-id="a0456-128">Por último, la última línea de comprobaciones del código o no la vista devuelve el `Details()` es la acción de la vista de detalles.</span><span class="sxs-lookup"><span data-stu-id="a0456-128">Finally, the last line of code checks whether or not the view returned by the `Details()` action is the Details view.</span></span>

<span data-ttu-id="a0456-129">El `ViewResult.ViewName` propiedad representa el nombre de la vista devuelto por un controlador.</span><span class="sxs-lookup"><span data-stu-id="a0456-129">The `ViewResult.ViewName` property represents the name of the view returned by a controller.</span></span> <span data-ttu-id="a0456-130">Una advertencia big acerca de las pruebas de esta propiedad.</span><span class="sxs-lookup"><span data-stu-id="a0456-130">One big warning about testing this property.</span></span> <span data-ttu-id="a0456-131">Hay dos maneras de que un controlador puede devolver una vista.</span><span class="sxs-lookup"><span data-stu-id="a0456-131">There are two ways that a controller can return a view.</span></span> <span data-ttu-id="a0456-132">Un controlador de forma explícita puede obtener una vista similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="a0456-132">A controller can explicitly return a view like this:</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample3.vb)]

<span data-ttu-id="a0456-133">O bien, puede puede deducir el nombre de la vista desde el nombre de la acción del controlador similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="a0456-133">Alternatively, the name of the view can be inferred from the name of the controller action like this:</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample4.vb)]

<span data-ttu-id="a0456-134">Esta acción de controlador también devuelve una vista denominada `Details`.</span><span class="sxs-lookup"><span data-stu-id="a0456-134">This controller action also returns a view named `Details`.</span></span> <span data-ttu-id="a0456-135">Sin embargo, el nombre de la vista se deduce del nombre de acción.</span><span class="sxs-lookup"><span data-stu-id="a0456-135">However, the name of the view is inferred from the action name.</span></span> <span data-ttu-id="a0456-136">Si desea probar el nombre de vista, debe devolver explícitamente el nombre de vista de la acción del controlador.</span><span class="sxs-lookup"><span data-stu-id="a0456-136">If you want to test the view name, then you must explicitly return the view name from the controller action.</span></span>

<span data-ttu-id="a0456-137">Puede ejecutar la prueba unitaria en el listado 2 escribiendo la combinación de teclado **Ctrl-R, A** o haciendo clic en el **ejecutar todas las pruebas de la solución** botón (consulte la figura 1).</span><span class="sxs-lookup"><span data-stu-id="a0456-137">You can run the unit test in Listing 2 by either entering the keyboard combination **Ctrl-R, A** or by clicking the **Run All Tests in Solution** button (see Figure 1).</span></span> <span data-ttu-id="a0456-138">Si la prueba se supere, podrá ver la ventana Resultados de pruebas en la figura 2.</span><span class="sxs-lookup"><span data-stu-id="a0456-138">If the test passes, you'll see the Test Results window in Figure 2.</span></span>


<span data-ttu-id="a0456-139">[![Ejecutar todas las pruebas de solución](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a0456-139">[![Run All Tests in Solution](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image1.png)</span></span>

<span data-ttu-id="a0456-140">**Figura 01**: ejecutar todas las pruebas de la solución ([haga clic aquí para ver la imagen a tamaño completo](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a0456-140">**Figure 01**: Run All Tests in Solution ([Click to view full-size image](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image3.png))</span></span>


<span data-ttu-id="a0456-141">[![Correcto.](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="a0456-141">[![Success!](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image4.png)</span></span>

<span data-ttu-id="a0456-142">**Figura 02**: correcto!</span><span class="sxs-lookup"><span data-stu-id="a0456-142">**Figure 02**: Success!</span></span> <span data-ttu-id="a0456-143">([Haga clic aquí para ver la imagen a tamaño completo](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="a0456-143">([Click to view full-size image](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image6.png))</span></span>


## <a name="testing-the-view-data-returned-by-a-controller"></a><span data-ttu-id="a0456-144">Comprobación de los datos de vista devuelta por un controlador</span><span class="sxs-lookup"><span data-stu-id="a0456-144">Testing the View Data returned by a Controller</span></span>

<span data-ttu-id="a0456-145">Controlador de MVC pasa los datos a una vista mediante el uso de lo que se denomina *`View Data`*.</span><span class="sxs-lookup"><span data-stu-id="a0456-145">An MVC controller passes data to a view by using something called *`View Data`*.</span></span> <span data-ttu-id="a0456-146">Por ejemplo, imagine que desea mostrar los detalles de un producto específico cuando se invoca el `ProductController Details()` acción.</span><span class="sxs-lookup"><span data-stu-id="a0456-146">For example, imagine that you want to display the details for a particular product when you invoke the `ProductController Details()` action.</span></span> <span data-ttu-id="a0456-147">En ese caso, puede crear una instancia de un `Product` clase (definido en el modelo) y pasar la instancia a la `Details` vista aprovechando las ventajas de `View Data`.</span><span class="sxs-lookup"><span data-stu-id="a0456-147">In that case, you can create an instance of a `Product` class (defined in your model) and pass the instance to the `Details` view by taking advantage of `View Data`.</span></span>

<span data-ttu-id="a0456-148">Modificados `ProductController` en el listado 3 incluye un controlador actualizado, `Details()` acción que devuelve un producto.</span><span class="sxs-lookup"><span data-stu-id="a0456-148">The modified `ProductController` in Listing 3 includes an updated `Details()` action that returns a Product.</span></span>

<span data-ttu-id="a0456-149">**Enumerar 3: `ProductController.vb`**</span><span class="sxs-lookup"><span data-stu-id="a0456-149">**Listing 3 – `ProductController.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample5.vb)]

<span data-ttu-id="a0456-150">En primer lugar, el `Details()` acción crea una nueva instancia de la `Product` clase que representa un equipo portátil.</span><span class="sxs-lookup"><span data-stu-id="a0456-150">First, the `Details()` action creates a new instance of the `Product` class that represents a laptop computer.</span></span> <span data-ttu-id="a0456-151">Siguiente, la instancia de la `Product` clase se pasa como el segundo parámetro para el `View()` método.</span><span class="sxs-lookup"><span data-stu-id="a0456-151">Next, the instance of the `Product` class is passed as the second parameter to the `View()` method.</span></span>

<span data-ttu-id="a0456-152">Puede escribir pruebas unitarias probar si los datos esperados están contenidos en la vista datos.</span><span class="sxs-lookup"><span data-stu-id="a0456-152">You can write unit tests to test whether the expected data is contained in view data.</span></span> <span data-ttu-id="a0456-153">La prueba unitaria en el listado 4 pruebas si o no es un producto que representa un equipo portátil que se devuelve cuando se llama a la `ProductController Details()` método de acción.</span><span class="sxs-lookup"><span data-stu-id="a0456-153">The unit test in Listing 4 tests whether or not a Product representing a laptop computer is returned when you call the `ProductController Details()` action method.</span></span>

<span data-ttu-id="a0456-154">**Enumerar 4: `ProductControllerTest.vb`**</span><span class="sxs-lookup"><span data-stu-id="a0456-154">**Listing 4 – `ProductControllerTest.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample6.vb)]

<span data-ttu-id="a0456-155">En el listado 4, el `TestDetailsView()` método comprueba los datos de vista devuelto al invocar el `Details()` método.</span><span class="sxs-lookup"><span data-stu-id="a0456-155">In Listing 4, the `TestDetailsView()` method tests the View Data returned by invoking the `Details()` method.</span></span> <span data-ttu-id="a0456-156">El `ViewData` se expone como una propiedad en el `ViewResult` devuelto al invocar el `Details()` método.</span><span class="sxs-lookup"><span data-stu-id="a0456-156">The `ViewData` is exposed as a property on the `ViewResult` returned by invoking the `Details()` method.</span></span> <span data-ttu-id="a0456-157">El `ViewData.Model` propiedad contiene el producto pasado a la vista.</span><span class="sxs-lookup"><span data-stu-id="a0456-157">The `ViewData.Model` property contains the product passed to the view.</span></span> <span data-ttu-id="a0456-158">La prueba comprueba simplemente que el producto contenido en los datos de vista tiene el nombre de equipo portátil.</span><span class="sxs-lookup"><span data-stu-id="a0456-158">The test simply verifies that the product contained in the View Data has the name Laptop.</span></span>

## <a name="testing-the-action-result-returned-by-a-controller"></a><span data-ttu-id="a0456-159">Probar el resultado de la acción devuelto por un controlador</span><span class="sxs-lookup"><span data-stu-id="a0456-159">Testing the Action Result returned by a Controller</span></span>

<span data-ttu-id="a0456-160">Una acción de controlador más compleja podría devolver distintos tipos de resultados de la acción según los valores de los parámetros pasados a la acción del controlador.</span><span class="sxs-lookup"><span data-stu-id="a0456-160">A more complex controller action might return different types of action results depending on the values of the parameters passed to the controller action.</span></span> <span data-ttu-id="a0456-161">Una acción de controlador puede devolver una variedad de tipos de resultados de acción, incluso un `ViewResult`, `RedirectToRouteResult`, o `JsonResult`.</span><span class="sxs-lookup"><span data-stu-id="a0456-161">A controller action can return a variety of types of action results including a `ViewResult`, `RedirectToRouteResult`, or `JsonResult`.</span></span>

<span data-ttu-id="a0456-162">Por ejemplo, la modificación `Details()` acción en el listado 5 devuelve el `Details` ver cuando se pasa un identificador de producto válido para la acción.</span><span class="sxs-lookup"><span data-stu-id="a0456-162">For example, the modified `Details()` action in Listing 5 returns the `Details` view when you pass a valid product Id to the action.</span></span> <span data-ttu-id="a0456-163">Si se pasa un producto no es válida: Id. de un identificador con un valor menor que 1, a continuación, se le redirigirá a la `Index()` acción.</span><span class="sxs-lookup"><span data-stu-id="a0456-163">If you pass an invalid product Id -- an Id with a value less than 1 -- then you are redirected to the `Index()` action.</span></span>

<span data-ttu-id="a0456-164">**Enumerar 5: `ProductController.vb`**</span><span class="sxs-lookup"><span data-stu-id="a0456-164">**Listing 5 – `ProductController.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample7.vb)]

<span data-ttu-id="a0456-165">Puede probar el comportamiento de la `Details()` acción con la prueba unitaria en el listado 6.</span><span class="sxs-lookup"><span data-stu-id="a0456-165">You can test the behavior of the `Details()` action with the unit test in Listing 6.</span></span> <span data-ttu-id="a0456-166">La prueba unitaria en el listado 6 comprueba que se le redirigirá a la `Index` ver cuando se pasa un identificador con el valor -1 para el `Details()` método.</span><span class="sxs-lookup"><span data-stu-id="a0456-166">The unit test in Listing 6 verifies that you are redirected to the `Index` view when an Id with the value -1 is passed to the `Details()` method.</span></span>

<span data-ttu-id="a0456-167">**Enumerar 6: `ProductControllerTest.vb`**</span><span class="sxs-lookup"><span data-stu-id="a0456-167">**Listing 6 – `ProductControllerTest.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample8.vb)]

<span data-ttu-id="a0456-168">Cuando se llama a la `RedirectToAction()` método en una acción de controlador, la acción del controlador devuelve un `RedirectToRouteResult`.</span><span class="sxs-lookup"><span data-stu-id="a0456-168">When you call the `RedirectToAction()` method in a controller action, the controller action returns a `RedirectToRouteResult`.</span></span> <span data-ttu-id="a0456-169">Las comprobaciones de prueba si la `RedirectToRouteResult` redirigirá al usuario a una acción del controlador denominada `Index`.</span><span class="sxs-lookup"><span data-stu-id="a0456-169">The test checks whether the `RedirectToRouteResult` will redirect the user to a controller action named `Index`.</span></span>

## <a name="summary"></a><span data-ttu-id="a0456-170">Resumen</span><span class="sxs-lookup"><span data-stu-id="a0456-170">Summary</span></span>

<span data-ttu-id="a0456-171">En este tutorial, aprendió a crear pruebas unitarias para acciones de controlador MVC.</span><span class="sxs-lookup"><span data-stu-id="a0456-171">In this tutorial, you learned how to build unit tests for MVC controller actions.</span></span> <span data-ttu-id="a0456-172">En primer lugar, aprendió a comprobar si la vista derecha devuelto por una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="a0456-172">First, you learned how to verify whether the right view is returned by a controller action.</span></span> <span data-ttu-id="a0456-173">Ha aprendido cómo utilizar la `ViewResult.ViewName` propiedad para comprobar el nombre de una vista.</span><span class="sxs-lookup"><span data-stu-id="a0456-173">You learned how to use the `ViewResult.ViewName` property to verify the name of a view.</span></span>

<span data-ttu-id="a0456-174">A continuación, se examina cómo puede probar el contenido de `View Data`.</span><span class="sxs-lookup"><span data-stu-id="a0456-174">Next, we examined how you can test the contents of `View Data`.</span></span> <span data-ttu-id="a0456-175">Ha aprendido cómo comprobar si el producto adecuado se devolvió en `View Data` después de llamar a una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="a0456-175">You learned how to check whether the right product was returned in `View Data` after calling a controller action.</span></span>

<span data-ttu-id="a0456-176">Por último, analizamos cómo puede probar si se devuelven tipos diferentes de los resultados de la acción de una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="a0456-176">Finally, we discussed how you can test whether different types of action results are returned from a controller action.</span></span> <span data-ttu-id="a0456-177">Ha aprendido cómo probar si un controlador devuelve un `ViewResult` o `RedirectToRouteResult`.</span><span class="sxs-lookup"><span data-stu-id="a0456-177">You learned how to test whether a controller returns a `ViewResult` or a `RedirectToRouteResult`.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a0456-178">Anterior</span><span class="sxs-lookup"><span data-stu-id="a0456-178">Previous</span></span>](creating-unit-tests-for-asp-net-mvc-applications-cs.md)
