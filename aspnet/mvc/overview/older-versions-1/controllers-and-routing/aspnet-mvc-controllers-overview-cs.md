---
uid: mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
title: Información general del controlador de MVC de ASP.NET (C#) | Microsoft Docs
author: StephenWalther
description: En este tutorial, Stephen Walther presenta los controladores ASP.NET MVC. Aprenda a crear nuevos controladores y devolver tipos diferentes de res de acción...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: b985c49a-3668-455c-a366-f85f6bc64b12
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: e1834498d5a0f5d84f650faf32bb6352f2ad3e70
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370092"
---
<a name="aspnet-mvc-controller-overview-c"></a>Información general del controlador de MVC de ASP.NET (C#)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> En este tutorial, Stephen Walther presenta los controladores ASP.NET MVC. Aprenda a crear nuevos controladores y devolver tipos diferentes de los resultados de acción.


Este tutorial exploran el tema de los controladores de ASP.NET MVC, las acciones de controlador y los resultados de acción. Después de completar este tutorial, comprenderá cómo se usan los controladores para controlar la manera en que un visitante interactúa con un sitio Web de ASP.NET MVC.

## <a name="understanding-controllers"></a>Descripción de los controladores

Los controladores MVC están responsables de responder a las solicitudes realizadas en un sitio Web de ASP.NET MVC. Cada solicitud del explorador se asigna a un controlador determinado. Por ejemplo, imagine que escriba la dirección URL siguiente en la barra de direcciones del explorador:

`http://localhost/Product/Index/3`

En este caso, se invoca un controlador llamado ProductController. ProductController es responsable de generar la respuesta a la solicitud del explorador. Por ejemplo, el controlador puede devolver una vista concreta al explorador o el controlador podría redireccionar al usuario a otro controlador.

Listado 1 contiene un controlador simple llamado ProductController.

**Listado 1 - Controllers\ProductController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample1.cs)]

Como puede ver en el listado 1, un controlador es simplemente una clase (una clase de C# o Visual Basic. NET). Un controlador es una clase que derive de la clase System.Web.Mvc.Controller base. Dado que un controlador que se hereda de esta clase base, un controlador hereda varios métodos útiles de forma gratuita (se describen estos métodos en un momento).

## <a name="understanding-controller-actions"></a>Descripción de las acciones de controlador

Un controlador expone las acciones de controlador. Una acción es un método en un controlador que se invoca cuando escriba una dirección URL determinada en la barra de direcciones del explorador. Por ejemplo, imagine que realiza una solicitud para la dirección URL siguiente:

`http://localhost/Product/Index/3`

En este caso, se llama al método de Index() en la clase ProductController. El método Index() es un ejemplo de una acción de controlador.

Una acción de controlador debe ser un método público de una clase de controlador. Métodos de C#, de forma predeterminada, son métodos privados. Tenga en cuenta que cualquier método público que se agrega a una clase de controlador se expone como una acción de controlador automáticamente (debe tener cuidado esto como una acción de controlador se puede invocar cualquier usuario en el universo escribiendo la URL a la derecha en la barra de direcciones del explorador).

Hay algunos requisitos adicionales que deben cumplirse mediante una acción de controlador. No se puede sobrecargar un método usado como una acción de controlador. Además, una acción de controlador no puede ser un método estático. Aparte de eso, puede usar cualquier método como una acción de controlador.

## <a name="understanding-action-results"></a>Descripción de los resultados de acción

Devuelve una acción de controlador algo llamado un *resultado de acción*. Un resultado de acción es lo que devuelve una acción de controlador en respuesta a una solicitud del explorador.

El marco de ASP.NET MVC admite varios tipos de resultados de acción incluidos:

1. ViewResult - representa HTML y marcado.
2. EmptyResult - no representa ningún resultado.
3. RedirectResult - representa una redirección a una nueva dirección URL.
4. JsonResult - representa un resultado de la notación de objetos JavaScript que puede usarse en una aplicación de AJAX.
5. JavaScriptResult - representa una secuencia de comandos de JavaScript.
6. ContentResult - representa un resultado de texto.
7. FileContentResult - representa un archivo descargable (con el contenido binario).
8. FilePathResult - representa un archivo descargable (con una ruta de acceso).
9. FileStreamResult - representa un archivo descargable (con una secuencia de archivo).

Todos estos resultados de acción se heredan de la clase ActionResult base.

En la mayoría de los casos, una acción de controlador devuelve ViewResult. Por ejemplo, la acción de controlador de índice en el listado 2 devuelve ViewResult.

**Listado 2 - Controllers\BookController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample2.cs)]

Cuando una acción que devuelve un ViewResult HTML se devuelve al explorador. El método Index() en el listado 2 devuelve una vista llamada Index en el explorador.

Tenga en cuenta que la acción de Index() en el listado 2 no devuelve un ViewResult(). En su lugar, se llama al método View() de la clase base del controlador. Normalmente, no se devuelven directamente un resultado de acción. En su lugar, llamar a uno de los métodos siguientes de la clase base del controlador:

1. Ver: devuelve un resultado de acción ViewResult.
2. Redirigir: devuelve un resultado de acción RedirectResult.
3. RedirectToAction - devuelve un resultado de acción RedirectToRouteResult.
4. RedirectToRoute - devuelve un resultado de acción RedirectToRouteResult.
5. JSON - devuelve un resultado de acción JsonResult.
6. JavaScriptResult - devuelve un JavaScriptResult.
7. Content - devuelve un resultado de acción ContentResult.
8. Archivo - devuelve un FileContentResult, FilePathResult o FileStreamResult dependiendo de los parámetros que se pasa al método.

Por lo tanto, si desea devolver una vista en el explorador, se llame al método View(). Si desea redirigir al usuario de acción de un controlador a otro, llama al método RedirectToAction(). Por ejemplo, la acción Details() en el listado 3 muestra una vista o redirige al usuario a la acción de Index() dependiendo de si el parámetro Id tiene un valor.

**Listado 3 - CustomerController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample3.cs)]

El resultado de acción ContentResult es especial. Puede usar el resultado de acción ContentResult para devolver un resultado de acción como texto sin formato. Por ejemplo, el método Index() en el listado 4 devuelve un mensaje como texto sin formato y no como HTML.

**Listado 4 - Controllers\StatusController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample4.cs)]

Cuando se invoca la acción StatusController.Index(), no se devuelve una vista. En su lugar, el texto sin formato "¡Hello World!" se devuelve al explorador.

Si una acción de controlador devuelve un resultado que es no es un resultado de acción - por ejemplo, una fecha o un entero -, a continuación, el resultado se encapsula en un ContentResult automáticamente. Por ejemplo, cuando se invoca la acción de Index() de la WorkController en el listado 5, la fecha se devuelve como un ContentResult automáticamente.

**Listado 5 - WorkController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample5.cs)]

La acción de Index() en el listado 5 devuelve un objeto DateTime. El marco de MVC de ASP.NET convierte el objeto de fecha y hora en una cadena y ajusta el valor de fecha y hora en un ContentResult automáticamente. El explorador recibe la fecha y hora como texto sin formato.

## <a name="summary"></a>Resumen

El propósito de este tutorial era para presentarle a los conceptos de controladores de ASP.NET MVC, las acciones de controlador y los resultados de acción de controlador. En la primera sección, ha aprendido cómo agregar controladores nuevos a un proyecto ASP.NET MVC. A continuación, ha aprendido cómo públicos métodos de un controlador se exponen al universo como acciones de controlador. Finalmente, se analizan los diferentes tipos de resultados de acción que se pueden devolver desde una acción de controlador. En concreto, explicamos cómo devolver un ViewResult, RedirectToActionResult y ContentResult desde una acción de controlador.

> [!div class="step-by-step"]
> [Anterior](creating-an-action-vb.md)
> [Siguiente](creating-custom-routes-cs.md)
