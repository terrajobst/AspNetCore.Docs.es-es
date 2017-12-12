---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
title: "Crear una restricción de ruta (C#) | Documentos de Microsoft"
author: StephenWalther
description: "En este tutorial, Stephen Walther se muestra cómo se puede controlar cómo el explorador solicita coincidencia rutas mediante la creación de restricciones de la ruta con expresiones regulares."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 0bfd06b1-12d3-4fbb-9779-a82e5eb7fe7d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: ee83a134dcbdd1abfb296f3126a64c7d4ebab7f5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-route-constraint-c"></a>Crear una restricción de ruta (C#)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> En este tutorial, Stephen Walther se muestra cómo se puede controlar cómo el explorador solicita coincidencia rutas mediante la creación de restricciones de la ruta con expresiones regulares.


Utilizar restricciones de la ruta para restringir las solicitudes del explorador que coinciden con una ruta determinada. Puede usar una expresión regular para especificar una restricción de ruta.

Por ejemplo, imagine que ha definido la ruta en el listado 1 en el archivo Global.asax.

**Lista 1 - Global.asax.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample1.cs)]

Lista 1 contiene una ruta con el nombre de producto. Puede usar la ruta de producto para asignar las solicitudes del explorador a la ProductController contenido en el listado 2.

**La lista 2 - Controllers\ProductController.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample2.cs)]

Observe que la acción de Details() expuesta por el controlador del producto acepta un solo parámetro denominado productId. Este parámetro es un parámetro de número entero.

La ruta definida en la lista 1 coincide con cualquiera de las direcciones URL siguientes:

- / Productos/23
- / / De producto 7

Desafortunadamente, la ruta también coincidirá con las direcciones URL siguientes:

- / Productos/bLa, bla
- / Productos/apple

Dado que la acción Details() espera un parámetro de número entero, que realiza una solicitud que contiene un valor distinto de un valor entero se producirá un error. Por ejemplo, si escribe la dirección URL /Product/apple en el explorador, a continuación, obtendrá la página de error en la figura 1.


[![El cuadro de diálogo nuevo proyecto](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)

**Figura 01**: ver una página seccionar ([haga clic aquí para ver la imagen a tamaño completo](creating-a-route-constraint-cs/_static/image2.png))


¿Qué desea hacer en realidad es solo coincidirá con direcciones URL que contienen un productId entero adecuado. Puede usar una restricción al definir una ruta para restringir las direcciones URL que coinciden con la ruta. La ruta de producto modificada en el listado 3 contiene una restricción de expresión regular que coincida solo con enteros.

**El listado 3 - Global.asax.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample3.cs)]

La expresión regular \d+ coincide con uno o más números enteros. Esta restricción hace que la ruta de producto para que coincida con las direcciones URL siguientes:

- / Productos/3
- / Productos/8999

Pero no las direcciones URL siguientes:

- / Productos/apple
- / Producto

- Estas solicitudes de explorador será procesadas por otra ruta o, si no hay ninguna ruta coincidente, un *no se pudo encontrar el recurso* se devolverá el error.

>[!div class="step-by-step"]
[Anterior](creating-custom-routes-cs.md)
[Siguiente](creating-a-custom-route-constraint-cs.md)
