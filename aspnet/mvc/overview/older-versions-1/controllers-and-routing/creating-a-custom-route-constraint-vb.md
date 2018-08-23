---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
title: Creación de una restricción de ruta personalizada (VB) | Microsoft Docs
author: StephenWalther
description: Stephen Walther demuestra cómo puede crear una restricción de ruta personalizada. Implementamos un simple personalizada restricción que impide que una ruta coincidente w...
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 892edb27-1cc2-4eaf-8314-dbc2efc6228a
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: d088380152adcb025857176b4396cab48fa64b66
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836366"
---
<a name="creating-a-custom-route-constraint-vb"></a>Creación de una restricción de ruta personalizada (VB)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther demuestra cómo puede crear una restricción de ruta personalizada. Implementamos una restricción personalizada simple que impide que una ruta que se va a coincidir cuando se realiza una solicitud de explorador desde un equipo remoto.


El objetivo de este tutorial es mostrar cómo puede crear una restricción de ruta personalizada. Una restricción de ruta personalizada le permite impedir que una ruta que se coteja a menos que se hace coincidir con alguna condición personalizada.

En este tutorial, creamos una restricción de ruta de host local. La restricción de ruta Localhost solo coincide con las solicitudes realizadas desde el equipo local. No se hacen coincidir las solicitudes remotas de a través de Internet.

Implementar una restricción de ruta personalizada implementando la interfaz IRouteConstraint. Se trata de una interfaz muy simple que describe un único método:

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample1.vb)]

El método devuelve un valor booleano. Si devuelve False, la ruta asociada con la restricción no coincidirá con la solicitud del explorador.

La restricción de Localhost se encuentra en el listado 1.

**Listado 1 - LocalhostConstraint.vb**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample2.vb)]

La restricción en el listado 1 aprovecha las ventajas de la propiedad IsLocal expuesta por la clase HttpRequest. Esta propiedad devuelve true cuando la dirección IP de la solicitud es 127.0.0.1 o cuando la dirección IP de la solicitud es el mismo que la dirección IP del servidor.

Usar una restricción personalizada dentro de una ruta definida en el archivo Global.asax. El archivo Global.asax en el listado 2 utiliza la restricción de Localhost para impedir que cualquier persona que solicita una página de administración a menos que la solicitud desde el servidor local. Por ejemplo, una solicitud para /Admin/DeleteAll se producirá un error cuando se realiza desde un servidor remoto.

**Listado 2 - Global.asax**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample3.vb)]

Se utiliza la restricción de Localhost en la definición de la ruta de administrador. Esta ruta no coincide con una solicitud de explorador remoto. Sin embargo, tenga en cuenta que otras rutas definidas en Global.asax podrían coincidir con la misma solicitud. Es importante comprender que una restricción impide que una determinada ruta de coincidencia de una solicitud y no todas las rutas se definen en el archivo Global.asax.

Tenga en cuenta que la ruta predeterminada se ha comentado en el archivo Global.asax en el listado 2. Si incluye la ruta predeterminada, la ruta predeterminada coincidiría con las solicitudes para el controlador de administración. En ese caso, los usuarios remotos todavía podrían invocar acciones del controlador de administración, aunque sus solicitudes no coinciden con la ruta de administrador.

> [!div class="step-by-step"]
> [Anterior](creating-a-route-constraint-vb.md)
