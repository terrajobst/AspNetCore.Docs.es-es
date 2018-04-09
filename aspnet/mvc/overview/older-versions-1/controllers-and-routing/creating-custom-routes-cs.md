---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
title: Crear rutas personalizadas (C#) | Documentos de Microsoft
author: microsoft
description: Obtenga información acerca de cómo agregar rutas personalizadas a una aplicación ASP.NET MVC. En este tutorial, aprenderá a modificar la tabla de rutas predeterminadas en el archivo Global.asax.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 3cd08f02-8763-490a-b625-2ac96a24b73f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
msc.type: authoredcontent
ms.openlocfilehash: 573b6a3360124feea92788ff7a3de363840fa1ef
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="creating-custom-routes-c"></a>Crear rutas personalizadas (C#)
====================
por [Microsoft](https://github.com/microsoft)

> Obtenga información acerca de cómo agregar rutas personalizadas a una aplicación ASP.NET MVC. En este tutorial, aprenderá a modificar la tabla de rutas predeterminadas en el archivo Global.asax.


En este tutorial, aprenderá a agregar una ruta personalizada a una aplicación de ASP.NET MVC. Obtenga información acerca de cómo modificar la tabla de rutas predeterminadas en el archivo Global.asax con una ruta personalizada.

Para muchas aplicaciones de ASP.NET MVC simples, la tabla de rutas predeterminadas funcionará correctamente. Sin embargo, puede que descubra que tenga necesidades de enrutamientos especiales. En ese caso, puede crear una ruta personalizada.

Por ejemplo, imagine que está creando una aplicación de blog. Puede administrar las solicitudes entrantes que tengan un aspecto similar al siguiente:

/ Archivo/12-25-2009

Cuando un usuario escribe esta solicitud, que desea devolver la entrada de blog que corresponde a la fecha 25/12/2009. Para controlar este tipo de solicitud, debe crear una ruta personalizada.

El archivo Global.asax en el listado 1 contiene una ruta personalizada nueva, denominada Blog, que trata las solicitudes que se parecen /Archive/*fecha de entrada*.

**Lista 1 - Global.asax (con una ruta personalizada)**

[!code-csharp[Main](creating-custom-routes-cs/samples/sample1.cs)]

Es importante el orden de las rutas que se agregan a la tabla de rutas. Se agregó la nueva ruta de Blog personalizado antes de la ruta predeterminada existente. Si se invierte el orden, a continuación, la ruta predeterminada siempre se llamará en lugar de la ruta personalizada.

La ruta de Blog personalizada coincide con cualquier solicitud que se inicia con/archivo /. Por lo tanto, ajusta a todas las direcciones URL siguientes:

- / Archivo/12-25-2009

- /Archive/10-6-2004

- / Archivo/apple

La ruta personalizada la solicitud entrante asigna a un controlador con el nombre de archivo e invoca la acción de Entry(). Cuando se llama al método de Entry(), la fecha de entrada se pasa como un parámetro denominado entryDate.

Puede usar la ruta personalizada Blog con el controlador en el listado 2.

**La lista 2 - ArchiveController.cs**

[!code-csharp[Main](creating-custom-routes-cs/samples/sample2.cs)]

Observe que el método Entry() en el listado 2 acepta un parámetro de tipo DateTime. El marco de MVC es lo suficientemente inteligente como para convertir la fecha de entrada de la dirección URL en un valor de fecha y hora automáticamente. Si el parámetro de fecha de entrada de la dirección URL no se puede convertir en un valor DateTime, se produce un error (consulte la figura 1).

**Figura 1: Error en la conversión de parámetros**


[![El cuadro de diálogo nuevo proyecto](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)

**Figura 01**: Error en la conversión de parámetro ([haga clic aquí para ver la imagen a tamaño completo](creating-custom-routes-cs/_static/image2.png))


## <a name="summary"></a>Resumen

El objetivo de este tutorial era demostrar cómo puede crear una ruta personalizada. Aprendido cómo agregar una ruta personalizada a la tabla de rutas en el archivo Global.asax que representa las entradas de blog. Se describe cómo asignar las solicitudes para las entradas de blog a un controlador denominado ArchiveController y una acción de controlador denominado Entry().

> [!div class="step-by-step"]
> [Anterior](aspnet-mvc-controllers-overview-cs.md)
> [Siguiente](creating-a-route-constraint-cs.md)
