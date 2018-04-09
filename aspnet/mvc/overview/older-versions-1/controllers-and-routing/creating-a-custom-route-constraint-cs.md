---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: Crear una restricción de ruta personalizada (C#) | Documentos de Microsoft
author: StephenWalther
description: Stephen Walther muestra cómo puede crear una restricción de ruta personalizados. Implementamos un sencillo personalizado restricción que impide que una ruta que se va a coincide w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 4c120a102b117433b6774f2ea7800f1c4a609f8b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-custom-route-constraint-c"></a>Crear una restricción de ruta personalizada (C#)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther muestra cómo puede crear una restricción de ruta personalizados. Implementamos una restricción personalizada simple que impide que una ruta que se va a buscar cuando se realiza una solicitud de explorador desde un equipo remoto.


El objetivo de este tutorial es mostrar cómo puede crear una restricción de ruta personalizados. Una restricción de ruta personalizada le permite impedir que una ruta que se coteja a menos que se compara una condición personalizada.

En este tutorial, se creará una restricción de ruta de host local. La restricción de ruta de host local solo coincide con las solicitudes realizadas desde el equipo local. No se cotejan solicitudes remotas de a través de Internet.

Implementar una restricción de ruta personalizada implementando la interfaz IRouteConstraint. Se trata de una interfaz muy simple que describe un único método:

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

El método devuelve un valor booleano. Si devuelve false, la ruta asociada a la restricción no coincide con la solicitud del explorador.

La restricción de Localhost está incluida en la lista 1.

**Lista 1 - LocalhostConstraint.cs**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

La restricción en la lista 1 aprovecha las ventajas de la propiedad IsLocal expuesta por la clase HttpRequest. Esta propiedad devuelve true cuando la dirección IP de la solicitud es 127.0.0.1 o cuando la dirección IP de la solicitud es el mismo que la dirección IP del servidor.

Usar una restricción personalizada dentro de una ruta definida en el archivo Global.asax. El archivo Global.asax en el listado 2 utiliza la restricción de Localhost para impedir que alguien solicita una página de administración a menos que realice la solicitud desde el servidor local. Por ejemplo, una solicitud para /Admin/DeleteAll se producirá un error cuando se realiza desde un servidor remoto.

**Listing 2 - Global.asax**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

La restricción de host local se utiliza en la definición de la ruta de administración. Esta ruta no se asocian con una solicitud de explorador remoto. Sin embargo, tenga en cuenta que otras rutas definidas en Global.asax podrían coincidir con la misma solicitud. Es importante comprender que una restricción impide que una determinada ruta de una solicitud de búsqueda de coincidencias y no todas las rutas definen en el archivo Global.asax.

Tenga en cuenta que la ruta predeterminada se convirtió en comentario en el archivo Global.asax en el listado 2. Si incluye la ruta predeterminada, la ruta predeterminada coincidiría con las solicitudes para el controlador de administración. En ese caso, los usuarios remotos todavía pudieron invocar acciones del controlador de administración, aunque sus solicitudes no coinciden con la ruta de administración.

> [!div class="step-by-step"]
> [Anterior](creating-a-route-constraint-cs.md)
> [Siguiente](asp-net-mvc-controller-overview-vb.md)
