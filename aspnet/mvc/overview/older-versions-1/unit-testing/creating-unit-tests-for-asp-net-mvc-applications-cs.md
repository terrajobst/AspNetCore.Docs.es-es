---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs
title: Crear pruebas unitarias para aplicaciones de ASP.NET MVC (C#) | Microsoft Docs
author: StephenWalther
description: Obtenga información sobre cómo crear pruebas unitarias para las acciones de controlador. En este tutorial, Stephen Walther muestra cómo probar si una acción de controlador devuelve un ParteI...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: d3a270b9-d7b1-47f2-8775-fc3beb518b5c
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs
msc.type: authoredcontent
ms.openlocfilehash: 4c88024a85d418befc9b2a111d96643ca0249ff4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396622"
---
<a name="creating-unit-tests-for-aspnet-mvc-applications-c"></a><span data-ttu-id="0db24-104">Crear pruebas unitarias para aplicaciones de ASP.NET MVC (C#)</span><span class="sxs-lookup"><span data-stu-id="0db24-104">Creating Unit Tests for ASP.NET MVC Applications (C#)</span></span>
====================
<span data-ttu-id="0db24-105">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="0db24-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

[<span data-ttu-id="0db24-106">Descargar PDF</span><span class="sxs-lookup"><span data-stu-id="0db24-106">Download PDF</span></span>](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_CS.pdf)

> <span data-ttu-id="0db24-107">Obtenga información sobre cómo crear pruebas unitarias para las acciones de controlador.</span><span class="sxs-lookup"><span data-stu-id="0db24-107">Learn how to create unit tests for controller actions.</span></span> <span data-ttu-id="0db24-108">En este tutorial, Stephen Walther muestra cómo probar si una acción de controlador devuelve una vista determinada, devuelve un conjunto de datos determinado o devuelve un tipo diferente del resultado de la acción.</span><span class="sxs-lookup"><span data-stu-id="0db24-108">In this tutorial, Stephen Walther demonstrates how to test whether a controller action returns a particular view, returns a particular set of data, or returns a different type of action result.</span></span>


<span data-ttu-id="0db24-109">El objetivo de este tutorial es demostrar cómo puede escribir pruebas unitarias para los controladores en su ASP.NET MVC aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="0db24-109">The goal of this tutorial is to demonstrate how you can write unit tests for the controllers in your ASP.NET MVC applications.</span></span> <span data-ttu-id="0db24-110">Se describe cómo crear tres tipos diferentes de las pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="0db24-110">We discuss how to build three different types of unit tests.</span></span> <span data-ttu-id="0db24-111">Obtenga información sobre cómo probar la vista devuelta por una acción de controlador, los datos de vista devuelta por una acción de controlador de pruebas y cómo probar si una acción de controlador le redirige a una segunda acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="0db24-111">You learn how to test the view returned by a controller action, how to test the View Data returned by a controller action, and how to test whether or not one controller action redirects you to a second controller action.</span></span>

## <a name="creating-the-controller-under-test"></a><span data-ttu-id="0db24-112">Crear el controlador sometida a prueba</span><span class="sxs-lookup"><span data-stu-id="0db24-112">Creating the Controller under Test</span></span>

<span data-ttu-id="0db24-113">Comencemos por crear el controlador que se va a probar.</span><span class="sxs-lookup"><span data-stu-id="0db24-113">Let's start by creating the controller that we intend to test.</span></span> <span data-ttu-id="0db24-114">El controlador, denominado el `ProductController`, se encuentra en el listado 1.</span><span class="sxs-lookup"><span data-stu-id="0db24-114">The controller, named the `ProductController`, is contained in Listing 1.</span></span>

<span data-ttu-id="0db24-115">**Listado 1: `ProductController.cs`**</span><span class="sxs-lookup"><span data-stu-id="0db24-115">**Listing 1 – `ProductController.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample1.cs)]

<span data-ttu-id="0db24-116">El `ProductController` contiene dos métodos de acción denominados `Index()` y `Details()`.</span><span class="sxs-lookup"><span data-stu-id="0db24-116">The `ProductController` contains two action methods named `Index()` and `Details()`.</span></span> <span data-ttu-id="0db24-117">Ambos métodos de acción devuelven una vista.</span><span class="sxs-lookup"><span data-stu-id="0db24-117">Both action methods return a view.</span></span> <span data-ttu-id="0db24-118">Tenga en cuenta que el `Details()` acción acepta un parámetro denominado identificador.</span><span class="sxs-lookup"><span data-stu-id="0db24-118">Notice that the `Details()` action accepts a parameter named Id.</span></span>

## <a name="testing-the-view-returned-by-a-controller"></a><span data-ttu-id="0db24-119">Probar la vista devuelta por un controlador</span><span class="sxs-lookup"><span data-stu-id="0db24-119">Testing the View returned by a Controller</span></span>

<span data-ttu-id="0db24-120">Imagine que va a probar si el `ProductController` devuelve la vista derecha.</span><span class="sxs-lookup"><span data-stu-id="0db24-120">Imagine that we want to test whether or not the `ProductController` returns the right view.</span></span> <span data-ttu-id="0db24-121">Queremos asegurarse de que, cuando el `ProductController.Details()` se invoca la acción, se devuelve la vista de detalles.</span><span class="sxs-lookup"><span data-stu-id="0db24-121">We want to make sure that when the `ProductController.Details()` action is invoked, the Details view is returned.</span></span> <span data-ttu-id="0db24-122">La clase de prueba en el listado 2 contiene una prueba unitaria para probar la vista devuelta por la `ProductController.Details()` acción.</span><span class="sxs-lookup"><span data-stu-id="0db24-122">The test class in Listing 2 contains a unit test for testing the view returned by the `ProductController.Details()` action.</span></span>

<span data-ttu-id="0db24-123">**Listado 2: `ProductControllerTest.cs`**</span><span class="sxs-lookup"><span data-stu-id="0db24-123">**Listing 2 – `ProductControllerTest.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample2.cs)]

<span data-ttu-id="0db24-124">La clase en el listado 2 incluye un método de prueba denominado `TestDetailsView()`.</span><span class="sxs-lookup"><span data-stu-id="0db24-124">The class in Listing 2 includes a test method named `TestDetailsView()`.</span></span> <span data-ttu-id="0db24-125">Este método contiene tres líneas de código.</span><span class="sxs-lookup"><span data-stu-id="0db24-125">This method contains three lines of code.</span></span> <span data-ttu-id="0db24-126">La primera línea de código crea una nueva instancia de la `ProductController` clase.</span><span class="sxs-lookup"><span data-stu-id="0db24-126">The first line of code creates a new instance of the `ProductController` class.</span></span> <span data-ttu-id="0db24-127">La segunda línea de código invoca el controlador `Details()` método de acción.</span><span class="sxs-lookup"><span data-stu-id="0db24-127">The second line of code invokes the controller's `Details()` action method.</span></span> <span data-ttu-id="0db24-128">Por último, la última línea de comprobaciones de código o no la vista devuelta por la `Details()` acción es la vista de detalles.</span><span class="sxs-lookup"><span data-stu-id="0db24-128">Finally, the last line of code checks whether or not the view returned by the `Details()` action is the Details view.</span></span>

<span data-ttu-id="0db24-129">El `ViewResult.ViewName` propiedad representa el nombre de la vista devuelta por un controlador.</span><span class="sxs-lookup"><span data-stu-id="0db24-129">The `ViewResult.ViewName` property represents the name of the view returned by a controller.</span></span> <span data-ttu-id="0db24-130">Una advertencia de gran tamaño sobre las pruebas de esta propiedad.</span><span class="sxs-lookup"><span data-stu-id="0db24-130">One big warning about testing this property.</span></span> <span data-ttu-id="0db24-131">Hay dos maneras de que un controlador puede devolver una vista.</span><span class="sxs-lookup"><span data-stu-id="0db24-131">There are two ways that a controller can return a view.</span></span> <span data-ttu-id="0db24-132">Un controlador de forma explícita puede devolver una vista similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="0db24-132">A controller can explicitly return a view like this:</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample3.cs)]

<span data-ttu-id="0db24-133">Como alternativa, el nombre de la vista puede deducirse a partir del nombre de la acción de controlador similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="0db24-133">Alternatively, the name of the view can be inferred from the name of the controller action like this:</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample4.cs)]

<span data-ttu-id="0db24-134">Esta acción de controlador también devuelve una vista denominada `Details`.</span><span class="sxs-lookup"><span data-stu-id="0db24-134">This controller action also returns a view named `Details`.</span></span> <span data-ttu-id="0db24-135">Sin embargo, el nombre de la vista se deriva el nombre de acción.</span><span class="sxs-lookup"><span data-stu-id="0db24-135">However, the name of the view is inferred from the action name.</span></span> <span data-ttu-id="0db24-136">Si desea probar el nombre de vista, explícitamente debe devolver el nombre de vista de la acción del controlador.</span><span class="sxs-lookup"><span data-stu-id="0db24-136">If you want to test the view name, then you must explicitly return the view name from the controller action.</span></span>

<span data-ttu-id="0db24-137">Puede ejecutar la prueba unitaria en el listado 2 escribiendo la combinación de teclado **Ctrl-R, A** o haciendo clic en el **ejecutar todas las pruebas de la solución** (consulte la figura 1).</span><span class="sxs-lookup"><span data-stu-id="0db24-137">You can run the unit test in Listing 2 by either entering the keyboard combination **Ctrl-R, A** or by clicking the **Run All Tests in Solution** button (see Figure 1).</span></span> <span data-ttu-id="0db24-138">Si se supera la prueba, verá la ventana de resultados de pruebas en la figura 2.</span><span class="sxs-lookup"><span data-stu-id="0db24-138">If the test passes, you'll see the Test Results window in Figure 2.</span></span>


<span data-ttu-id="0db24-139">[![Ejecutar todas las pruebas de solución](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0db24-139">[![Run All Tests in Solution](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image1.png)</span></span>

<span data-ttu-id="0db24-140">**Figura 01**: ejecute todas las pruebas de la solución ([haga clic aquí para ver imagen en tamaño completo](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0db24-140">**Figure 01**: Run All Tests in Solution ([Click to view full-size image](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image3.png))</span></span>


<span data-ttu-id="0db24-141">[![¡Success!](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="0db24-141">[![Success!](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image4.png)</span></span>

<span data-ttu-id="0db24-142">**Figura 02**: Success!</span><span class="sxs-lookup"><span data-stu-id="0db24-142">**Figure 02**: Success!</span></span> <span data-ttu-id="0db24-143">([Haga clic aquí para ver imagen en tamaño completo](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="0db24-143">([Click to view full-size image](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image6.png))</span></span>


## <a name="testing-the-view-data-returned-by-a-controller"></a><span data-ttu-id="0db24-144">Comprobación de los datos de vista devuelta por un controlador</span><span class="sxs-lookup"><span data-stu-id="0db24-144">Testing the View Data returned by a Controller</span></span>

<span data-ttu-id="0db24-145">Un controlador MVC pasa datos a una vista mediante el uso de algo llamado *`View Data`*.</span><span class="sxs-lookup"><span data-stu-id="0db24-145">An MVC controller passes data to a view by using something called *`View Data`*.</span></span> <span data-ttu-id="0db24-146">Por ejemplo, imagine que desea mostrar los detalles de un determinado producto al invocar el `ProductController Details()` acción.</span><span class="sxs-lookup"><span data-stu-id="0db24-146">For example, imagine that you want to display the details for a particular product when you invoke the `ProductController Details()` action.</span></span> <span data-ttu-id="0db24-147">En ese caso, puede crear una instancia de un `Product` clase (definido en el modelo) y pasar la instancia a la `Details` aprovechando las ventajas de la vista `View Data`.</span><span class="sxs-lookup"><span data-stu-id="0db24-147">In that case, you can create an instance of a `Product` class (defined in your model) and pass the instance to the `Details` view by taking advantage of `View Data`.</span></span>

<span data-ttu-id="0db24-148">Modificado `ProductController` en el listado 3 incluye actualizada `Details()` acción que devuelva un producto.</span><span class="sxs-lookup"><span data-stu-id="0db24-148">The modified `ProductController` in Listing 3 includes an updated `Details()` action that returns a Product.</span></span>

<span data-ttu-id="0db24-149">**Listado 3: `ProductController.cs`**</span><span class="sxs-lookup"><span data-stu-id="0db24-149">**Listing 3 – `ProductController.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample5.cs)]

<span data-ttu-id="0db24-150">En primer lugar, el `Details()` acción crea una nueva instancia de la `Product` clase que representa un equipo portátil.</span><span class="sxs-lookup"><span data-stu-id="0db24-150">First, the `Details()` action creates a new instance of the `Product` class that represents a laptop computer.</span></span> <span data-ttu-id="0db24-151">A continuación, la instancia de la `Product` clase se pasa como segundo parámetro para el `View()` método.</span><span class="sxs-lookup"><span data-stu-id="0db24-151">Next, the instance of the `Product` class is passed as the second parameter to the `View()` method.</span></span>

<span data-ttu-id="0db24-152">Puede escribir pruebas unitarias comprobar si los datos esperados están contenida en la vista datos.</span><span class="sxs-lookup"><span data-stu-id="0db24-152">You can write unit tests to test whether the expected data is contained in view data.</span></span> <span data-ttu-id="0db24-153">La prueba unitaria en el listado 4 pruebas si un producto que representa un equipo portátil se devuelve al llamar a la `ProductController Details()` método de acción.</span><span class="sxs-lookup"><span data-stu-id="0db24-153">The unit test in Listing 4 tests whether or not a Product representing a laptop computer is returned when you call the `ProductController Details()` action method.</span></span>

<span data-ttu-id="0db24-154">**Listado 4: `ProductControllerTest.cs`**</span><span class="sxs-lookup"><span data-stu-id="0db24-154">**Listing 4 – `ProductControllerTest.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample6.cs)]

<span data-ttu-id="0db24-155">En el listado 4 el `TestDetailsView()` método prueba los datos de vista devuelto al invocar el `Details()` método.</span><span class="sxs-lookup"><span data-stu-id="0db24-155">In Listing 4, the `TestDetailsView()` method tests the View Data returned by invoking the `Details()` method.</span></span> <span data-ttu-id="0db24-156">El `ViewData` se expone como una propiedad en el `ViewResult` devuelto al invocar el `Details()` método.</span><span class="sxs-lookup"><span data-stu-id="0db24-156">The `ViewData` is exposed as a property on the `ViewResult` returned by invoking the `Details()` method.</span></span> <span data-ttu-id="0db24-157">El `ViewData.Model` propiedad contiene el producto pasado a la vista.</span><span class="sxs-lookup"><span data-stu-id="0db24-157">The `ViewData.Model` property contains the product passed to the view.</span></span> <span data-ttu-id="0db24-158">La prueba simplemente comprueba que el producto contenido en los datos de vista tiene el nombre de equipo portátil.</span><span class="sxs-lookup"><span data-stu-id="0db24-158">The test simply verifies that the product contained in the View Data has the name Laptop.</span></span>

## <a name="testing-the-action-result-returned-by-a-controller"></a><span data-ttu-id="0db24-159">Probar el resultado de acción devuelta por un controlador</span><span class="sxs-lookup"><span data-stu-id="0db24-159">Testing the Action Result returned by a Controller</span></span>

<span data-ttu-id="0db24-160">Una acción de controlador más compleja podría devolver distintos tipos de resultados de acción según los valores de los parámetros pasados a la acción del controlador.</span><span class="sxs-lookup"><span data-stu-id="0db24-160">A more complex controller action might return different types of action results depending on the values of the parameters passed to the controller action.</span></span> <span data-ttu-id="0db24-161">Una acción de controlador puede devolver una variedad de tipos de resultados de acción, incluso un `ViewResult`, `RedirectToRouteResult`, o `JsonResult`.</span><span class="sxs-lookup"><span data-stu-id="0db24-161">A controller action can return a variety of types of action results including a `ViewResult`, `RedirectToRouteResult`, or `JsonResult`.</span></span>

<span data-ttu-id="0db24-162">Por ejemplo, la modificación `Details()` acción en el listado 5 devuelve el `Details` ver al pasar un Id. de producto válida para la acción.</span><span class="sxs-lookup"><span data-stu-id="0db24-162">For example, the modified `Details()` action in Listing 5 returns the `Details` view when you pass a valid product Id to the action.</span></span> <span data-ttu-id="0db24-163">Si se pasa un producto no válido: Id. de un identificador con un valor menor que 1, entonces se le redirigirá a la `Index()` acción.</span><span class="sxs-lookup"><span data-stu-id="0db24-163">If you pass an invalid product Id -- an Id with a value less than 1 -- then you are redirected to the `Index()` action.</span></span>

<span data-ttu-id="0db24-164">**Listado 5: `ProductController.cs`**</span><span class="sxs-lookup"><span data-stu-id="0db24-164">**Listing 5 – `ProductController.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample7.cs)]

<span data-ttu-id="0db24-165">Puede probar el comportamiento de la `Details()` acción con la prueba unitaria en el listado 6.</span><span class="sxs-lookup"><span data-stu-id="0db24-165">You can test the behavior of the `Details()` action with the unit test in Listing 6.</span></span> <span data-ttu-id="0db24-166">La prueba unitaria en el listado 6 comprueba que se le redirigirá a la `Index` ver cuando se pasa un identificador con el valor -1 a la `Details()` método.</span><span class="sxs-lookup"><span data-stu-id="0db24-166">The unit test in Listing 6 verifies that you are redirected to the `Index` view when an Id with the value -1 is passed to the `Details()` method.</span></span>

<span data-ttu-id="0db24-167">**Listado 6: `ProductControllerTest.cs`**</span><span class="sxs-lookup"><span data-stu-id="0db24-167">**Listing 6 – `ProductControllerTest.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample8.cs)]

<span data-ttu-id="0db24-168">Cuando se llama a la `RedirectToAction()` la acción del controlador de método en una acción de controlador, devuelve un `RedirectToRouteResult`.</span><span class="sxs-lookup"><span data-stu-id="0db24-168">When you call the `RedirectToAction()` method in a controller action, the controller action returns a `RedirectToRouteResult`.</span></span> <span data-ttu-id="0db24-169">Las comprobaciones de prueba si el `RedirectToRouteResult` redirigirá al usuario a una acción de controlador denominada `Index`.</span><span class="sxs-lookup"><span data-stu-id="0db24-169">The test checks whether the `RedirectToRouteResult` will redirect the user to a controller action named `Index`.</span></span>

## <a name="summary"></a><span data-ttu-id="0db24-170">Resumen</span><span class="sxs-lookup"><span data-stu-id="0db24-170">Summary</span></span>

<span data-ttu-id="0db24-171">En este tutorial, aprendió a crear pruebas unitarias de acciones de controlador MVC.</span><span class="sxs-lookup"><span data-stu-id="0db24-171">In this tutorial, you learned how to build unit tests for MVC controller actions.</span></span> <span data-ttu-id="0db24-172">En primer lugar, ha aprendido cómo comprobar si la vista derecha devuelto por una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="0db24-172">First, you learned how to verify whether the right view is returned by a controller action.</span></span> <span data-ttu-id="0db24-173">Ha aprendido a usar el `ViewResult.ViewName` propiedad para comprobar el nombre de una vista.</span><span class="sxs-lookup"><span data-stu-id="0db24-173">You learned how to use the `ViewResult.ViewName` property to verify the name of a view.</span></span>

<span data-ttu-id="0db24-174">A continuación, hemos visto cómo puede probar el contenido de `View Data`.</span><span class="sxs-lookup"><span data-stu-id="0db24-174">Next, we examined how you can test the contents of `View Data`.</span></span> <span data-ttu-id="0db24-175">Ha aprendido cómo comprobar si se devolvió el producto adecuado en `View Data` después de llamar a una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="0db24-175">You learned how to check whether the right product was returned in `View Data` after calling a controller action.</span></span>

<span data-ttu-id="0db24-176">Finalmente, explicamos cómo puede probar si se devuelven tipos diferentes de los resultados de acción de una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="0db24-176">Finally, we discussed how you can test whether different types of action results are returned from a controller action.</span></span> <span data-ttu-id="0db24-177">Ha aprendido cómo comprobar si un controlador devuelve un `ViewResult` o `RedirectToRouteResult`.</span><span class="sxs-lookup"><span data-stu-id="0db24-177">You learned how to test whether a controller returns a `ViewResult` or a `RedirectToRouteResult`.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0db24-178">Siguiente</span><span class="sxs-lookup"><span data-stu-id="0db24-178">Next</span></span>](creating-unit-tests-for-asp-net-mvc-applications-vb.md)
