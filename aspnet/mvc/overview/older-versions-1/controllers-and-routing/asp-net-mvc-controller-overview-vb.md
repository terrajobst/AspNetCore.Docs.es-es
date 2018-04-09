---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
title: Información general sobre el controlador de MVC de ASP.NET (VB) | Documentos de Microsoft
author: StephenWalther
description: En este tutorial, Stephen Walther presenta controladores MVC de ASP.NET. Obtenga información acerca de cómo crear nuevos controladores y devolver tipos diferentes de res acción...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: 94c3e5d9-a904-445e-a34e-d92fd1ca108a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: 0a45630e8f2d5ae0548bb6b8496df08ca5877a40
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-mvc-controller-overview-vb"></a>Información general sobre el controlador de MVC de ASP.NET (VB)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> En este tutorial, Stephen Walther presenta controladores MVC de ASP.NET. Obtenga información acerca de cómo crear nuevos controladores y devolver distintos tipos de resultados de la acción.


Este tutorial explora el tema de controladores, las acciones de controlador y resultados de la acción de ASP.NET MVC. Después de completar este tutorial, comprenderá cómo se usan los controladores para controlar la manera en que un visitante interactúa con un sitio Web de ASP.NET MVC.

## <a name="understanding-controllers"></a>Descripción de controladores

Controladores MVC están responsables de responder a las solicitudes realizadas en un sitio Web de ASP.NET MVC. Cada solicitud del explorador se asigna a un controlador determinado. Por ejemplo, imagine que escriba la dirección URL siguiente en la barra de direcciones del explorador:

`http://localhost/Product/Index/3`

En este caso, se invoca un controlador denominado ProductController. El ProductController es responsable de generar la respuesta a la solicitud del explorador. Por ejemplo, el controlador puede devolver una vista determinada al explorador o el controlador podría redireccionar al usuario a otro controlador.

Lista 1 contiene un controlador simple denominado ProductController.

**Listing1 - Controllers\ProductController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample1.vb)]

Como puede ver en el listado 1, un controlador es solo una clase (una clase .NET de Visual Basic o C#). Un controlador es una clase que deriva de la clase System.Web.Mvc.Controller base. Puesto que un controlador se hereda de esta clase base, un controlador hereda varios métodos útiles de forma gratuita (se explican estos métodos en un momento).

## <a name="understanding-controller-actions"></a>Descripción de las acciones de controlador

Un controlador expone las acciones de controlador. Una acción es un método en un controlador que se invoca cuando escriba una dirección URL determinada en la barra de direcciones del explorador. Por ejemplo, imagine que se realiza una solicitud para la dirección URL siguiente:

`http://localhost/Product/Index/3`

En este caso, se llama al método de Index() en la clase ProductController. El método Index() es un ejemplo de una acción del controlador.

Una acción de controlador debe ser un método público de una clase de controlador. Los métodos de Visual Basic.NET, de forma predeterminada, son métodos públicos. Tenga en cuenta que cualquier método público que se agrega a una clase de controlador se expone como una acción de controlador automáticamente (debe ser cuidado con esto ya que una acción de controlador se puede invocar cualquier usuario en el universo escribiendo la dirección URL correcta en la barra de direcciones del explorador).

Hay algunos requisitos adicionales que deben cumplirse mediante una acción de controlador. No se puede sobrecargar un método que se utiliza como una acción de controlador. Además, una acción de controlador no puede ser un método estático. Aparte de eso, puede usar cualquier método como una acción de controlador.

## <a name="understanding-action-results"></a>Descripción de los resultados de acción

Lo que se denomina devuelve una acción de controlador un *resultado de la acción*. Un resultado de acción es lo que devuelve una acción de controlador en respuesta a una solicitud del explorador.

El marco de MVC de ASP.NET admite varios tipos de resultados de acción incluidas:

1. ViewResult - representa HTML y marcado.
2. EmptyResult - no representa ningún resultado.
3. RedirectResult - representa una redirección a una nueva dirección URL.
4. JsonResult - representa un resultado de notación de objetos JavaScript que puede usarse en una aplicación de AJAX.
5. JavaScriptResult - representa una secuencia de comandos de JavaScript.
6. ContentResult - representa un resultado de texto.
7. FileContentResult - representa un archivo descargable (con el contenido binario).
8. FilePathResult - representa un archivo descargable (con una ruta de acceso).
9. FileStreamResult - representa un archivo descargable (con una secuencia de archivo).

Todos estos resultados de acción de heredan de la clase ActionResult base.

En la mayoría de los casos, una acción de controlador devuelve un ViewResult. Por ejemplo, la acción de controlador de índice en el listado 2 devuelve un ViewResult.

**La lista 2 - Controllers\BookController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample2.vb)]

Cuando una acción devuelve un ViewResult, HTML se devuelve al explorador. El método Index() en el listado 2 devuelve una vista con el nombre de índice en el explorador.

Tenga en cuenta que la acción de Index() en el listado 2 no devuelve un ViewResult(). En su lugar, se llama al método de View() de la clase base del controlador. Normalmente, no se devuelve directamente un resultado de acción. En su lugar, se llame a uno de los siguientes métodos de la clase base del controlador:

1. Ver - devuelve un resultado de acción ViewResult.
2. Redirigir: devuelve un resultado de acción RedirectResult.
3. RedirectToAction - devuelve un resultado de acción de RedirectToRouteResult.
4. RedirectToRoute - devuelve un resultado de acción de RedirectToRouteResult.
5. JSON - devuelve un resultado de acción de JsonResult.
6. JavaScriptResult - devuelve un JavaScriptResult.
7. Content - devuelve un resultado de acción ContentResult.
8. Archivo - devuelve un FileContentResult, FilePathResult o FileStreamResult dependiendo de los parámetros pasado al método.

Por lo tanto, si desea obtener una vista en el explorador, llama al método View(). Si desea redirigir al usuario de la acción de un controlador, se llame al método RedirectToAction(). Por ejemplo, la acción Details() en la lista 3 muestra una vista o redirige al usuario a la acción de Index() dependiendo de si el parámetro Id tiene un valor.

**El listado 3 - CustomerController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample3.vb)]

El resultado de acción ContentResult es especial. Puede usar el resultado de acción ContentResult para devolver un resultado de acción como texto sin formato. Por ejemplo, el método Index() en el listado 4 devuelve un mensaje como texto sin formato y no como HTML.

**Listado 4 - Controllers\StatusController.vb**

> StatusController
> 
> 
> System.Web.Mvc.Controller


[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample4.vb)]

Cuando se invoca la acción StatusController.Index(), no se devuelve una vista. En su lugar, el texto sin formato "¡Hello World!" se devuelve al explorador.

Si una acción de controlador devuelve un resultado que es no es un resultado de acción: por ejemplo, una fecha o un entero -, a continuación, el resultado se encapsula en un ContentResult automáticamente. Por ejemplo, cuando se invoca la acción de Index() de la WorkController en el listado 5, la fecha se devuelve como un ContentResult automáticamente.

**Listado 5 - WorkController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample5.vb)]

La acción de Index() en el listado 5 devuelve un objeto DateTime. El marco de MVC de ASP.NET convierte el objeto de fecha y hora en una cadena y ajusta el valor de fecha y hora en un ContentResult automáticamente. El explorador recibe la fecha y hora como texto sin formato.

## <a name="summary"></a>Resumen

El objetivo de este tutorial era para presentarle los conceptos de controladores, las acciones de controlador y resultados de la acción de controlador de MVC de ASP.NET. En la primera sección, aprendió a agregar nuevos controladores a un proyecto de MVC de ASP.NET. A continuación, ha aprendido cómo públicos métodos de un controlador se exponen al universo como acciones del controlador. Por último, analizamos los diferentes tipos de resultados de la acción que se pueden devolver desde una acción de controlador. En concreto, se explicó cómo devolver un ViewResult, RedirectToActionResult y ContentResult desde una acción de controlador.

> [!div class="step-by-step"]
> [Anterior](creating-a-custom-route-constraint-cs.md)
> [Siguiente](creating-custom-routes-vb.md)
