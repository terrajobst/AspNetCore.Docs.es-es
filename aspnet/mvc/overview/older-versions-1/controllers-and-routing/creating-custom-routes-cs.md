---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
title: Crear rutas personalizadas (C#) | Microsoft Docs
author: microsoft
description: Obtenga información sobre cómo agregar rutas personalizadas a una aplicación ASP.NET MVC. En este tutorial, obtendrá información sobre cómo modificar la tabla de rutas predeterminadas en el archivo Global.asax.
ms.author: aspnetcontent
ms.date: 02/16/2009
ms.assetid: 3cd08f02-8763-490a-b625-2ac96a24b73f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
msc.type: authoredcontent
ms.openlocfilehash: c8ba653579c5f983a7086d15cf6245eff527bfcf
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827668"
---
<a name="creating-custom-routes-c"></a>Crear rutas personalizadas (C#)
====================
por [Microsoft](https://github.com/microsoft)

> Obtenga información sobre cómo agregar rutas personalizadas a una aplicación ASP.NET MVC. En este tutorial, obtendrá información sobre cómo modificar la tabla de rutas predeterminadas en el archivo Global.asax.


En este tutorial, obtendrá información sobre cómo agregar una ruta personalizada a una aplicación ASP.NET MVC. Obtendrá información sobre cómo modificar la tabla de rutas predeterminadas en el archivo Global.asax con una ruta personalizada.

Para muchas aplicaciones de ASP.NET MVC simple, la tabla de enrutamiento predeterminada funcionará bien. Sin embargo, es posible que descubra que dispone de las necesidades de enrutamientos especializado. En ese caso, puede crear una ruta personalizada.

Por ejemplo, imagine que está creando una aplicación de blog. Es posible que desee controlar las solicitudes entrantes que tienen un aspecto similar al siguiente:

/ Archive/12-25-2009

Cuando un usuario escribe esta solicitud, desea devolver la entrada de blog que corresponde a la fecha 25/12/2009. Para controlar este tipo de solicitud, deberá crear una ruta personalizada.

El archivo Global.asax en el listado 1 contiene una ruta personalizada nueva, denominada Blog, que controla las solicitudes que se parecen a /Archive/*fecha de entrada*.

**Listado 1 - Global.asax (con una ruta personalizada)**

[!code-csharp[Main](creating-custom-routes-cs/samples/sample1.cs)]

Es importante el orden de las rutas que se agregan a la tabla de rutas. Nuestra nueva ruta Blog personalizado se agrega antes de la ruta predeterminada existente. Si invierte el orden, a continuación, la ruta predeterminada siempre se llamará en lugar de la ruta personalizada.

La ruta de blogs personalizada coincide con cualquier solicitud que empieza con/Archive /. Por lo tanto, ajusta a todas las direcciones URL siguientes:

- / Archive/12-25-2009

- / Archive/10-6-2004

- / Archive/apple

La ruta personalizada la solicitud entrante asigna a un controlador con el nombre de archivo e invoca la acción Entry(). Cuando se llama al método Entry(), la fecha de entrada se pasa como un parámetro denominado entryDate.

Puede usar la ruta personalizada de Blog con el controlador en el listado 2.

**Listado 2 - ArchiveController.cs**

[!code-csharp[Main](creating-custom-routes-cs/samples/sample2.cs)]

Tenga en cuenta que el método Entry() en el listado 2 acepta un parámetro de tipo DateTime. El marco de MVC es lo suficientemente inteligente como para convertir la fecha de entrada de la dirección URL en un valor de fecha y hora automáticamente. Si el parámetro de entrada de fecha de la dirección URL no puede convertirse en una fecha y hora, se produce un error (consulte la figura 1).

**Figura 1: Error en la conversión de parámetro**


[![El cuadro de diálogo nuevo proyecto](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)

**Figura 01**: Error en la conversión de parámetro ([haga clic aquí para ver imagen en tamaño completo](creating-custom-routes-cs/_static/image2.png))


## <a name="summary"></a>Resumen

El objetivo de este tutorial era demostrar cómo puede crear una ruta personalizada. Ha aprendido cómo agregar una ruta personalizada a la tabla de rutas en el archivo Global.asax que representa las entradas de blog. Analizamos cómo asignar las solicitudes de entradas de blog con un controlador denominado ArchiveController y una acción de controlador denominado Entry().

> [!div class="step-by-step"]
> [Anterior](aspnet-mvc-controllers-overview-cs.md)
> [Siguiente](creating-a-route-constraint-cs.md)
