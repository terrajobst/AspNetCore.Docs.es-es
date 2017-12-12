---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs
title: Crear pruebas unitarias para aplicaciones de ASP.NET MVC (C#) | Documentos de Microsoft
author: StephenWalther
description: "Obtenga información acerca de cómo crear pruebas unitarias para acciones de controlador. En este tutorial, Stephen Walther muestra cómo probar si una acción de controlador devuelve un ParteI..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: d3a270b9-d7b1-47f2-8775-fc3beb518b5c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs
msc.type: authoredcontent
ms.openlocfilehash: 56c981363f1905c1c9869dbaf2adb6b5ac1c28a5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="creating-unit-tests-for-aspnet-mvc-applications-c"></a>Crear pruebas unitarias para aplicaciones de ASP.NET MVC (C#)
====================
por [Stephen Walther](https://github.com/StephenWalther)

[Descarga de PDF](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_CS.pdf)

> Obtenga información acerca de cómo crear pruebas unitarias para acciones de controlador. En este tutorial, Stephen Walther muestra cómo probar si una acción de controlador devuelve una vista determinada, devuelve un conjunto de datos determinado o devuelve un tipo diferente del resultado de la acción.


El objetivo de este tutorial es mostrar cómo se pueden escribir pruebas unitarias para los controladores en su ASP.NET MVC aplicaciones. Se describe cómo crear tres tipos diferentes de las pruebas unitarias. Obtenga información acerca de cómo probar la vista devuelta por una acción de controlador, cómo los datos de vista devueltos por una acción de controlador de pruebas y cómo comprobar si una acción de controlador le redirige a una segunda acción de controlador.

## <a name="creating-the-controller-under-test"></a>Crear el controlador de pruebas

Empecemos creando el controlador que se va a probar. El controlador, denominado el `ProductController`, se encuentra en la lista 1.

**Lista 1:`ProductController.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample1.cs)]

El `ProductController` contiene dos métodos de acción denominados `Index()` y `Details()`. Ambos métodos de acción devuelven una vista. Tenga en cuenta que el `Details()` acción acepta un parámetro denominado identificador.

## <a name="testing-the-view-returned-by-a-controller"></a>Probar la vista devuelto por un controlador

Suponga que desea probar o no el `ProductController` devuelve la vista derecha. Queremos para asegurarse de que, cuando el `ProductController.Details()` se invoca la acción, se devuelve la vista de detalles. La clase de prueba en el listado 2 contiene una prueba unitaria para probar la vista devuelve el `ProductController.Details()` acción.

**La lista 2:`ProductControllerTest.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample2.cs)]

La clase en el listado 2 incluye un método de prueba denominado `TestDetailsView()`. Este método contiene tres líneas de código. La primera línea de código crea una nueva instancia de la `ProductController` clase. La segunda línea de código, invoca el controlador `Details()` método de acción. Por último, la última línea de comprobaciones del código o no la vista devuelve el `Details()` es la acción de la vista de detalles.

El `ViewResult.ViewName` propiedad representa el nombre de la vista devuelto por un controlador. Una advertencia big acerca de las pruebas de esta propiedad. Hay dos maneras de que un controlador puede devolver una vista. Un controlador de forma explícita puede obtener una vista similar al siguiente:

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample3.cs)]

O bien, puede puede deducir el nombre de la vista desde el nombre de la acción del controlador similar al siguiente:

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample4.cs)]

Esta acción de controlador también devuelve una vista denominada `Details`. Sin embargo, el nombre de la vista se deduce del nombre de acción. Si desea probar el nombre de vista, debe devolver explícitamente el nombre de vista de la acción del controlador.

Puede ejecutar la prueba unitaria en el listado 2 escribiendo la combinación de teclado **Ctrl-R, A** o haciendo clic en el **ejecutar todas las pruebas de la solución** botón (consulte la figura 1). Si la prueba se supere, podrá ver la ventana Resultados de pruebas en la figura 2.


[![Ejecutar todas las pruebas de solución](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image1.png)

**Figura 01**: ejecutar todas las pruebas de la solución ([haga clic aquí para ver la imagen a tamaño completo](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image3.png))


[![Correcto.](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image4.png)

**Figura 02**: correcto! ([Haga clic aquí para ver la imagen a tamaño completo](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image6.png))


## <a name="testing-the-view-data-returned-by-a-controller"></a>Comprobación de los datos de vista devuelta por un controlador

Controlador de MVC pasa los datos a una vista mediante el uso de lo que se denomina  *`View Data`* . Por ejemplo, imagine que desea mostrar los detalles de un producto específico cuando se invoca el `ProductController Details()` acción. En ese caso, puede crear una instancia de un `Product` clase (definido en el modelo) y pasar la instancia a la `Details` vista aprovechando las ventajas de `View Data`.

Modificados `ProductController` en el listado 3 incluye un controlador actualizado, `Details()` acción que devuelve un producto.

**Enumerar 3:`ProductController.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample5.cs)]

En primer lugar, el `Details()` acción crea una nueva instancia de la `Product` clase que representa un equipo portátil. Siguiente, la instancia de la `Product` clase se pasa como el segundo parámetro para el `View()` método.

Puede escribir pruebas unitarias probar si los datos esperados están contenidos en la vista datos. La prueba unitaria en el listado 4 pruebas si o no es un producto que representa un equipo portátil que se devuelve cuando se llama a la `ProductController Details()` método de acción.

**Enumerar 4:`ProductControllerTest.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample6.cs)]

En el listado 4, el `TestDetailsView()` método comprueba los datos de vista devuelto al invocar el `Details()` método. El `ViewData` se expone como una propiedad en el `ViewResult` devuelto al invocar el `Details()` método. El `ViewData.Model` propiedad contiene el producto pasado a la vista. La prueba comprueba simplemente que el producto contenido en los datos de vista tiene el nombre de equipo portátil.

## <a name="testing-the-action-result-returned-by-a-controller"></a>Probar el resultado de la acción devuelto por un controlador

Una acción de controlador más compleja podría devolver distintos tipos de resultados de la acción según los valores de los parámetros pasados a la acción del controlador. Una acción de controlador puede devolver una variedad de tipos de resultados de acción, incluso un `ViewResult`, `RedirectToRouteResult`, o `JsonResult`.

Por ejemplo, la modificación `Details()` acción en el listado 5 devuelve el `Details` ver cuando se pasa un identificador de producto válido para la acción. Si se pasa un producto no es válida: Id. de un identificador con un valor menor que 1, a continuación, se le redirigirá a la `Index()` acción.

**Enumerar 5:`ProductController.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample7.cs)]

Puede probar el comportamiento de la `Details()` acción con la prueba unitaria en el listado 6. La prueba unitaria en el listado 6 comprueba que se le redirigirá a la `Index` ver cuando se pasa un identificador con el valor -1 para el `Details()` método.

**Enumerar 6:`ProductControllerTest.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample8.cs)]

Cuando se llama a la `RedirectToAction()` método en una acción de controlador, la acción del controlador devuelve un `RedirectToRouteResult`. Las comprobaciones de prueba si la `RedirectToRouteResult` redirigirá al usuario a una acción del controlador denominada `Index`.

## <a name="summary"></a>Resumen

En este tutorial, aprendió a crear pruebas unitarias para acciones de controlador MVC. En primer lugar, aprendió a comprobar si la vista derecha devuelto por una acción de controlador. Ha aprendido cómo utilizar la `ViewResult.ViewName` propiedad para comprobar el nombre de una vista.

A continuación, se examina cómo puede probar el contenido de `View Data`. Ha aprendido cómo comprobar si el producto adecuado se devolvió en `View Data` después de llamar a una acción de controlador.

Por último, analizamos cómo puede probar si se devuelven tipos diferentes de los resultados de la acción de una acción de controlador. Ha aprendido cómo probar si un controlador devuelve un `ViewResult` o `RedirectToRouteResult`.

>[!div class="step-by-step"]
[Siguiente](creating-unit-tests-for-asp-net-mvc-applications-vb.md)
