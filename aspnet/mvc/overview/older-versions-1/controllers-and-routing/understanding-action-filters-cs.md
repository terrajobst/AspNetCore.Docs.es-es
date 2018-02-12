---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
title: "Descripción de los filtros de acción (C#) | Documentos de Microsoft"
author: microsoft
description: "El objetivo de este tutorial es explicar los filtros de acción. Un filtro de acción es un atributo que se puede aplicar a una acción de controlador o un controlador de todo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: a94e4e81-40c1-47b7-8613-126a1a6cc93d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
msc.type: authoredcontent
ms.openlocfilehash: 86d5d429d9900d4c04391804598626705e6c88b4
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2018
---
<a name="understanding-action-filters-c"></a>Descripción de los filtros de acción (C#)
====================
por [Microsoft](https://github.com/microsoft)

[Descarga de PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_CS.pdf)

> El objetivo de este tutorial es explicar los filtros de acción. Un filtro de acción es un atributo que se puede aplicar a una acción de controlador o un controlador completo--que modifica el modo en que se ejecuta la acción.


## <a name="understanding-action-filters"></a>Descripción de los filtros de acción

El objetivo de este tutorial es explicar los filtros de acción. Un filtro de acción es un atributo que se puede aplicar a una acción de controlador o un controlador completo--que modifica el modo en que se ejecuta la acción. El marco de ASP.NET MVC incluye varios filtros de acción:

- OutputCache – este filtro de acción almacena en caché el resultado de una acción de controlador para un período de tiempo especificado.
- HandleError – este filtro de acción controla los errores que se desencadena cuando se ejecuta una acción de controlador.
- Autorizar: este filtro de acción permite restringir el acceso a un usuario determinado o un rol.

También puede crear sus propios filtros de acción personalizada. Por ejemplo, puede crear un filtro de acción personalizado con el fin de implementar un sistema de autenticación personalizado. O bien, puede crear un filtro de acción que modifica los datos de vista devueltos por una acción de controlador.

En este tutorial, aprenderá a crear un filtro de acción desde el principio. Creamos un filtro de acción de registro que registra distintas fases del procesamiento de una acción en la ventana Resultados de Visual Studio.

### <a name="using-an-action-filter"></a>Utilizar un filtro de acción

Un filtro de acción es un atributo. Puede aplicar la mayoría de los filtros de acción para una acción de controladores individuales o un controlador de todo.

Por ejemplo, el controlador de datos en la lista 1 expone una acción denominada `Index()` que devuelve la hora actual. Esta acción se decora con el `OutputCache` filtro de acción. Este filtro hace que el valor devuelto por la acción que se almacenarán en caché durante 10 segundos.

**Lista 1:`Controllers\DataController.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample1.cs)]

Si se invoca varias veces el `Index()` acción escribiendo la dirección URL datos/índice en la barra de direcciones del explorador y alcanzar la actualización botón varias veces, verá el mismo tiempo durante 10 segundos. El resultado de la `Index()` acción se almacena en caché durante 10 segundos (consulte la figura 1).


[![Tiempo en caché](understanding-action-filters-cs/_static/image2.png)](understanding-action-filters-cs/_static/image1.png)

**Figura 01**: en memoria caché vez ([haga clic aquí para ver la imagen a tamaño completo](understanding-action-filters-cs/_static/image3.png))


En la lista 1, un filtro de acción única: la `OutputCache` filtro de acción: se aplica a la `Index()` método. Si necesita, puede aplicar varios filtros de acción a la misma acción. Por ejemplo, puede aplicar tanto la `OutputCache` y `HandleError` filtros de acción para la misma acción.

En la lista 1, el `OutputCache` filtro de acción se aplica a la `Index()` acción. También puede aplicar este atributo a la `DataController` propia clase. En ese caso, se almacenarán en caché el resultado devuelto por cualquier acción expuesto por el controlador de 10 segundos.

### <a name="the-different-types-of-filters"></a>Los diferentes tipos de filtros

El marco de MVC de ASP.NET admite cuatro tipos diferentes de filtros:

1. Los filtros de autorización: implementa la `IAuthorizationFilter` atributo.
2. Filtros de acción: implementa la `IActionFilter` atributo.
3. Como resultado de filtros: implementa la `IResultFilter` atributo.
4. Filtros de excepciones: implementa la `IExceptionFilter` atributo.

Los filtros se ejecutan en el orden mostrado anteriormente. Por ejemplo, los filtros de autorización se ejecutan siempre antes que los filtros de acción y los filtros de excepción siempre se ejecutan después de todos los demás tipos de filtro.

Los filtros de autorización se usan para implementar la autenticación y autorización para las acciones de controlador. Por ejemplo, el filtro de autorización es un ejemplo de un filtro de autorización.

Filtros de acción contienen lógica que se ejecuta antes y después se ejecuta una acción de controlador. Puede utilizar un filtro de acción, por ejemplo, para modificar los datos de vista que devuelve una acción de controlador.

Filtros de resultados contienen lógica que se ejecuta antes y después de que se ejecute un resultado de la vista. Por ejemplo, puede modificar una vista de resultados correctamente antes de la vista se representa en el explorador.

Filtros de excepciones son el último tipo de filtro que se va a ejecutar. Puede utilizar un filtro de excepciones para controlar los errores generados por sus acciones del controlador o el resultado de acción del controlador. También puede utilizar filtros de excepciones para registrar los errores.

Cada tipo de filtro diferente se ejecuta en un orden concreto. Si desea controlar el orden en que se ejecutan los filtros del mismo tipo, a continuación, puede establecer la propiedad de orden del filtro.

La clase base para todos los filtros de acción es la `System.Web.Mvc.FilterAttribute` clase. Si va a implementar un tipo determinado de filtro, debe crear una clase que hereda de la clase base de filtro e implementa uno o varios de los `IAuthorizationFilter`, `IActionFilter`, `IResultFilter`, o `IExceptionFilter` interfaces.

### <a name="the-base-actionfilterattribute-class"></a>La clase ActionFilterAttribute Base

Para que sea más fácil de implementar un filtro de acción personalizado, el marco de MVC de ASP.NET incluye una base de `ActionFilterAttribute` clase. Esta clase implementa tanto la `IActionFilter` y `IResultFilter` interfaces y se hereda de la `Filter` clase.

La terminología aquí no es totalmente coherente. Técnicamente, una clase que hereda de la clase ActionFilterAttribute es un filtro de acción y un filtro de resultado. Sin embargo, en el sentido flexible, el filtro de acción de word se utiliza para hacer referencia a cualquier tipo de filtro en el marco de MVC de ASP.NET.

La base de `ActionFilterAttribute` clase tiene los siguientes métodos que se pueden reemplazar:

- OnActionExecuting: llama a este método antes de que se ejecuta una acción de controlador.
- OnActionExecuted: llama a este método después de ejecuta una acción de controlador.
- OnResultExecuting: llama a este método antes de que se ejecute un resultado de acción de controlador.
- OnResultExecuted: llama a este método después de que se ejecute un resultado de acción de controlador.

En la siguiente sección, veremos cómo puede implementar cada uno de estos métodos diferentes.

### <a name="creating-a-log-action-filter"></a>Crear un filtro de acción de registro

Para ilustrar cómo se puede crear un filtro de acción personalizado, vamos a crear un filtro de acción personalizado que registra las fases de procesamiento de una acción de controlador a la ventana Resultados de Visual Studio. Nuestro `LogActionFilter` se encuentra en el listado 2.

**La lista 2:`ActionFilters\LogActionFilter.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample2.cs)]

En el listado 2, el `OnActionExecuting()`, `OnActionExecuted()`, `OnResultExecuting()`, y `OnResultExecuted()` todos los métodos llaman a la `Log()` método. El nombre del método y los datos actuales de la ruta se pasa a la `Log()` método. El `Log()` método escribe un mensaje en la ventana Resultados de Visual Studio (consulte la figura 2).


[![Escribir en la ventana Resultados de Visual Studio](understanding-action-filters-cs/_static/image5.png)](understanding-action-filters-cs/_static/image4.png)

**Figura 02**: escribir en la ventana Resultados de Visual Studio ([haga clic aquí para ver la imagen a tamaño completo](understanding-action-filters-cs/_static/image6.png))


El controlador Home en el listado 3 muestra cómo puede aplicar el filtro de acción de registro a una clase de controlador completo. Cada vez que cualquiera de las acciones expuestas por el controlador Home se invocan: ya sea el `Index()` método o la `About()` método – las fases de procesamiento de la acción se registran en la ventana Resultados de Visual Studio.

**Enumerar 3:`Controllers\HomeController.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample3.cs)]

### <a name="summary"></a>Resumen

En este tutorial, se introdujeron en filtros de acción de MVC de ASP.NET. Aprendió a utilizar los cuatro tipos de filtros: filtros de autorización, los filtros de acción, filtros de resultados y los filtros de excepción. También aprendió acerca de la base de `ActionFilterAttribute` clase.

Por último, aprendió cómo implementar un filtro de acción simple. Hemos creado un filtro de acción de registro que registra las fases de procesamiento de una acción de controlador a la ventana Resultados de Visual Studio.

>[!div class="step-by-step"]
[Anterior](asp-net-mvc-routing-overview-cs.md)
[Siguiente](improving-performance-with-output-caching-cs.md)
