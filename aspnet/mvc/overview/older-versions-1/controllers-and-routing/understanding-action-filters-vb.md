---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-vb
title: Descripción de los filtros de acción (VB) | Microsoft Docs
author: microsoft
description: El objetivo de este tutorial es explicar los filtros de acción. Un filtro de acción es un atributo que se puede aplicar a una acción de controlador o un controlador todo...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: e83812f2-c53e-4a43-a7c1-d64c59ecf694
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-vb
msc.type: authoredcontent
ms.openlocfilehash: 0116306afdf21cb24a374013bb54ada54e5699ea
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829208"
---
<a name="understanding-action-filters-vb"></a>Descripción de los filtros de acción (VB)
====================
por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_VB.pdf)

> El objetivo de este tutorial es explicar los filtros de acción. Un filtro de acción es un atributo que se puede aplicar a una acción de controlador o un controlador de todo, que modifica la forma en que se ejecuta la acción.


## <a name="understanding-action-filters"></a>Descripción de los filtros de acción

El objetivo de este tutorial es explicar los filtros de acción. Un filtro de acción es un atributo que se puede aplicar a una acción de controlador o un controlador de todo, que modifica la forma en que se ejecuta la acción. El marco de ASP.NET MVC incluye varios filtros de acción:

- OutputCache – este filtro de acción almacena en caché el resultado de una acción de controlador para un período de tiempo especificado.
- HandleError – este filtro de acción controla los errores que se genera cuando se ejecuta una acción de controlador.
- Autorizar: este filtro de acción le permite restringir el acceso a un usuario determinado o un rol.

También puede crear sus propios filtros de acción personalizada. Por ejemplo, puede crear un filtro de acción personalizado con el fin de implementar un sistema de autenticación personalizada. O bien, si desea crear un filtro de acción que modifica los datos de vista devueltos por una acción de controlador.

En este tutorial, aprenderá a crear un filtro de acción desde el principio. Creamos un filtro de acción de registro que registra las distintas fases del procesamiento de una acción a la ventana de salida de Visual Studio.

### <a name="using-an-action-filter"></a>Uso de un filtro de acción

Un filtro de acción es un atributo. Puede aplicar la mayoría de los filtros de acción para una acción de controlador individuales o todo un controlador.

Por ejemplo, el controlador de datos en el listado 1 expone una acción denominada `Index()` que devuelve la hora actual. Esta acción se decora con el `OutputCache` filtro de acción. Este filtro hace que el valor devuelto por la acción que se almacenará en caché durante 10 segundos.

**Listado 1: `Controllers\DataController.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample1.vb)]

Si se invoca varias veces el `Index()` acción al introducir la dirección URL operaciones/Data/índice en la barra de direcciones del explorador y presionar la actualización de botón varias veces, verá la misma hora durante 10 segundos. La salida de la `Index()` acción se almacena en caché durante 10 segundos (consulte la figura 1).


[![Tiempo en caché](understanding-action-filters-vb/_static/image2.png)](understanding-action-filters-vb/_static/image1.png)

**Figura 01**: almacenar en caché de tiempo ([haga clic aquí para ver imagen en tamaño completo](understanding-action-filters-vb/_static/image3.png))


En el listado 1, un filtro de acción única: el `OutputCache` filtro de acción: se aplica a la `Index()` método. Si necesita, puede aplicar varios filtros de acción a la misma acción. Por ejemplo, desea aplicar tanto la `OutputCache` y `HandleError` los filtros de acción a la misma acción.

En el listado 1, el `OutputCache` filtro de acción se aplica a la `Index()` acción. También podría aplicar este atributo a la `DataController` propia clase. En ese caso, el resultado devuelto por cualquier acción expuesta por el controlador almacenaría en caché durante 10 segundos.

### <a name="the-different-types-of-filters"></a>Los diferentes tipos de filtros

El marco de ASP.NET MVC admite cuatro tipos diferentes de filtros:

1. Filtros de autorización: implementa la `IAuthorizationFilter` atributo.
2. Implementa los filtros de acción el `IActionFilter` atributo.
3. Como resultado de filtros: implementa la `IResultFilter` atributo.
4. Filtros de excepciones: implementa la `IExceptionFilter` atributo.

Los filtros se ejecutan en el orden mostrado anteriormente. Por ejemplo, los filtros de autorización se ejecutan siempre antes de los filtros de acción y los filtros de excepciones siempre se ejecutan después de todos los demás tipos de filtro.

Los filtros de autorización se usan para implementar la autenticación y autorización de acciones del controlador. Por ejemplo, el filtro de autorización es un ejemplo de un filtro de autorización.

Los filtros de acción contienen lógica que se ejecuta antes y después se ejecuta una acción de controlador. Puede usar un filtro de acción, por ejemplo, para modificar los datos de vista que devuelve una acción de controlador.

Filtros de resultados contienen lógica que se ejecuta antes y después de ejecutar un resultado de la vista. Por ejemplo, desea modificar una vista de resultados justo antes de la vista se presenta en el explorador.

Los filtros de excepciones son el último tipo de filtro se ejecute. Puede usar un filtro de excepciones para controlar errores generados por sus acciones de controlador o los resultados de acción de controlador. También puede usar los filtros de excepciones para registrar los errores.

Cada tipo de filtro diferentes se ejecuta en un orden concreto. Si desea controlar el orden en que se ejecutan los filtros del mismo tipo, a continuación, puede establecer la propiedad de orden de un filtro.

La clase base para todos los filtros de acción es la `System.Web.Mvc.FilterAttribute` clase. Si desea implementar un tipo determinado de filtro, a continuación, deberá crear una clase que hereda de la clase base del filtro e implementa uno o varios de los IAuthorizationFilter, IActionFilter, IResultFilter o ExceptionFilter interfaces.

### <a name="the-base-actionfilterattribute-class"></a>La clase ActionFilterAttribute Base

Para que resulte más fácil de implementar un filtro de acción personalizada, el marco ASP.NET MVC incluye una base `ActionFilterAttribute` clase. Esta clase implementa tanto la `IActionFilter` y `IResultFilter` e interfaces hereda el `Filter` clase.

La terminología de este artículo no es totalmente coherente. Técnicamente, una clase que hereda de la clase ActionFilterAttribute es un filtro de acción y un filtro de resultados. Sin embargo, en el sentido flexible, el filtro de acción de word se usa para referirse a cualquier tipo de filtro en el marco de MVC de ASP.NET.

La clase ActionFilterAttribute base tiene los siguientes métodos que se pueden reemplazar:

- OnActionExecuting: este método se llama antes de ejecuta una acción de controlador.
- OnActionExecuted: este método se llama después de ejecuta una acción de controlador.
- OnResultExecuting: este método se llama antes de que se ejecute un resultado de acción de controlador.
- OnResultExecuted: este método se llama después de ejecuta un resultado de acción de controlador.

En la siguiente sección, veremos cómo puede implementar cada uno de estos métodos diferentes.

### <a name="creating-a-log-action-filter"></a>Crear un filtro de acción de registro

Para ilustrar cómo puede crear un filtro de acción personalizado, vamos a crear un filtro de acción personalizada que registra las fases de procesamiento de una acción de controlador a la ventana de salida de Visual Studio. Nuestro `LogActionFilter` se encuentra en el listado 2.

**Listado 2: `ActionFilters\LogActionFilter.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample2.vb)]

En el listado 2, el `OnActionExecuting()`, `OnActionExecuted()`, `OnResultExecuting()`, y `OnResultExecuted()` todos los métodos llaman a la `Log()` método. El nombre del método y los datos de ruta actual se pasa a la `Log()` método. El `Log()` método escribe un mensaje en la ventana de salida de Visual Studio (consulte la figura 2).


[![Escribir en la ventana de salida de Visual Studio](understanding-action-filters-vb/_static/image5.png)](understanding-action-filters-vb/_static/image4.png)

**Figura 02**: escribir en la ventana de salida de Visual Studio ([haga clic aquí para ver imagen en tamaño completo](understanding-action-filters-vb/_static/image6.png))


El controlador Home en el listado 3 muestra cómo puede aplicar el filtro de acción de registro a una clase de controlador completo. Cada vez que cualquiera de las acciones expuestas por el controlador Home se invocan: ya sea el `Index()` método o la `About()` método – las fases de procesamiento de la acción se registran en la ventana de salida de Visual Studio.

**Listado 3: `Controllers\HomeController.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample3.vb)]

### <a name="summary"></a>Resumen

En este tutorial, se presentaron a los filtros de acción de MVC de ASP.NET. Ha aprendido acerca de los cuatro tipos diferentes de filtros: filtros de autorización, los filtros de acción, filtros de resultados y los filtros de excepciones. También ha aprendido acerca de la base `ActionFilterAttribute` clase.

Por último, ha aprendido cómo implementar un filtro de acción simple. Hemos creado un filtro de acción de registro que registra las fases de procesamiento de una acción de controlador a la ventana de salida de Visual Studio.

> [!div class="step-by-step"]
> [Anterior](asp-net-mvc-routing-overview-vb.md)
> [Siguiente](improving-performance-with-output-caching-vb.md)
