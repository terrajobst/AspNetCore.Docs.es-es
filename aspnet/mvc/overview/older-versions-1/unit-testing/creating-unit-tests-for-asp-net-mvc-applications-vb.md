---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
title: Crear pruebas unitarias para aplicaciones de ASP.NET MVC (VB) | Microsoft Docs
author: StephenWalther
description: Obtenga información sobre cómo crear pruebas unitarias para las acciones de controlador. En este tutorial, Stephen Walther muestra cómo probar si una acción de controlador devuelve un ParteI...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: eb35710d-1d99-44ac-b61f-e50af8cb328a
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
msc.type: authoredcontent
ms.openlocfilehash: 8edd134f6534b2a53be7f475cf0cb35ca93d3067
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384088"
---
<a name="creating-unit-tests-for-aspnet-mvc-applications-vb"></a>Crear pruebas unitarias para aplicaciones de ASP.NET MVC (VB)
====================
por [Stephen Walther](https://github.com/StephenWalther)

[Descargar PDF](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_VB.pdf)

> Obtenga información sobre cómo crear pruebas unitarias para las acciones de controlador. En este tutorial, Stephen Walther muestra cómo probar si una acción de controlador devuelve una vista determinada, devuelve un conjunto de datos determinado o devuelve un tipo diferente del resultado de la acción.


El objetivo de este tutorial es demostrar cómo puede escribir pruebas unitarias para los controladores en su ASP.NET MVC aplicaciones. Se describe cómo crear tres tipos diferentes de las pruebas unitarias. Obtenga información sobre cómo probar la vista devuelta por una acción de controlador, los datos de vista devuelta por una acción de controlador de pruebas y cómo probar si una acción de controlador le redirige a una segunda acción de controlador.

## <a name="creating-the-controller-under-test"></a>Crear el controlador sometida a prueba

Comencemos por crear el controlador que se va a probar. El controlador, denominado el `ProductController`, se encuentra en el listado 1.

**Listado 1: `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample1.vb)]

El `ProductController` contiene dos métodos de acción denominados `Index()` y `Details()`. Ambos métodos de acción devuelven una vista. Tenga en cuenta que el `Details()` acción acepta un parámetro denominado identificador.

## <a name="testing-the-view-returned-by-a-controller"></a>Probar la vista devuelta por un controlador

Imagine que va a probar si el `ProductController` devuelve la vista derecha. Queremos asegurarse de que, cuando el `ProductController.Details()` se invoca la acción, se devuelve la vista de detalles. La clase de prueba en el listado 2 contiene una prueba unitaria para probar la vista devuelta por la `ProductController.Details()` acción.

**Listado 2: `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample2.vb)]

La clase en el listado 2 incluye un método de prueba denominado `TestDetailsView()`. Este método contiene tres líneas de código. La primera línea de código crea una nueva instancia de la `ProductController` clase. La segunda línea de código invoca el controlador `Details()` método de acción. Por último, la última línea de comprobaciones de código o no la vista devuelta por la `Details()` acción es la vista de detalles.

El `ViewResult.ViewName` propiedad representa el nombre de la vista devuelta por un controlador. Una advertencia de gran tamaño sobre las pruebas de esta propiedad. Hay dos maneras de que un controlador puede devolver una vista. Un controlador de forma explícita puede devolver una vista similar al siguiente:

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample3.vb)]

Como alternativa, el nombre de la vista puede deducirse a partir del nombre de la acción de controlador similar al siguiente:

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample4.vb)]

Esta acción de controlador también devuelve una vista denominada `Details`. Sin embargo, el nombre de la vista se deriva el nombre de acción. Si desea probar el nombre de vista, explícitamente debe devolver el nombre de vista de la acción del controlador.

Puede ejecutar la prueba unitaria en el listado 2 escribiendo la combinación de teclado **Ctrl-R, A** o haciendo clic en el **ejecutar todas las pruebas de la solución** (consulte la figura 1). Si se supera la prueba, verá la ventana de resultados de pruebas en la figura 2.


[![Ejecutar todas las pruebas de solución](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image1.png)

**Figura 01**: ejecute todas las pruebas de la solución ([haga clic aquí para ver imagen en tamaño completo](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image3.png))


[![¡Success!](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image4.png)

**Figura 02**: Success! ([Haga clic aquí para ver imagen en tamaño completo](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image6.png))


## <a name="testing-the-view-data-returned-by-a-controller"></a>Comprobación de los datos de vista devuelta por un controlador

Un controlador MVC pasa datos a una vista mediante el uso de algo llamado *`View Data`*. Por ejemplo, imagine que desea mostrar los detalles de un determinado producto al invocar el `ProductController Details()` acción. En ese caso, puede crear una instancia de un `Product` clase (definido en el modelo) y pasar la instancia a la `Details` aprovechando las ventajas de la vista `View Data`.

Modificado `ProductController` en el listado 3 incluye actualizada `Details()` acción que devuelva un producto.

**Listado 3: `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample5.vb)]

En primer lugar, el `Details()` acción crea una nueva instancia de la `Product` clase que representa un equipo portátil. A continuación, la instancia de la `Product` clase se pasa como segundo parámetro para el `View()` método.

Puede escribir pruebas unitarias comprobar si los datos esperados están contenida en la vista datos. La prueba unitaria en el listado 4 pruebas si un producto que representa un equipo portátil se devuelve al llamar a la `ProductController Details()` método de acción.

**Listado 4: `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample6.vb)]

En el listado 4 el `TestDetailsView()` método prueba los datos de vista devuelto al invocar el `Details()` método. El `ViewData` se expone como una propiedad en el `ViewResult` devuelto al invocar el `Details()` método. El `ViewData.Model` propiedad contiene el producto pasado a la vista. La prueba simplemente comprueba que el producto contenido en los datos de vista tiene el nombre de equipo portátil.

## <a name="testing-the-action-result-returned-by-a-controller"></a>Probar el resultado de acción devuelta por un controlador

Una acción de controlador más compleja podría devolver distintos tipos de resultados de acción según los valores de los parámetros pasados a la acción del controlador. Una acción de controlador puede devolver una variedad de tipos de resultados de acción, incluso un `ViewResult`, `RedirectToRouteResult`, o `JsonResult`.

Por ejemplo, la modificación `Details()` acción en el listado 5 devuelve el `Details` ver al pasar un Id. de producto válida para la acción. Si se pasa un producto no válido: Id. de un identificador con un valor menor que 1, entonces se le redirigirá a la `Index()` acción.

**Listado 5: `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample7.vb)]

Puede probar el comportamiento de la `Details()` acción con la prueba unitaria en el listado 6. La prueba unitaria en el listado 6 comprueba que se le redirigirá a la `Index` ver cuando se pasa un identificador con el valor -1 a la `Details()` método.

**Listado 6: `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample8.vb)]

Cuando se llama a la `RedirectToAction()` la acción del controlador de método en una acción de controlador, devuelve un `RedirectToRouteResult`. Las comprobaciones de prueba si el `RedirectToRouteResult` redirigirá al usuario a una acción de controlador denominada `Index`.

## <a name="summary"></a>Resumen

En este tutorial, aprendió a crear pruebas unitarias de acciones de controlador MVC. En primer lugar, ha aprendido cómo comprobar si la vista derecha devuelto por una acción de controlador. Ha aprendido a usar el `ViewResult.ViewName` propiedad para comprobar el nombre de una vista.

A continuación, hemos visto cómo puede probar el contenido de `View Data`. Ha aprendido cómo comprobar si se devolvió el producto adecuado en `View Data` después de llamar a una acción de controlador.

Finalmente, explicamos cómo puede probar si se devuelven tipos diferentes de los resultados de acción de una acción de controlador. Ha aprendido cómo comprobar si un controlador devuelve un `ViewResult` o `RedirectToRouteResult`.

> [!div class="step-by-step"]
> [Anterior](creating-unit-tests-for-asp-net-mvc-applications-cs.md)
