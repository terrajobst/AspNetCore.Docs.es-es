---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
title: Crear una acción (VB) | Documentos de Microsoft
author: microsoft
description: Obtenga información acerca de cómo agregar una nueva acción a un controlador de MVC de ASP.NET. Obtenga información acerca de los requisitos para un método como una acción.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: c8d93e11-ef78-4a30-afbc-f30419000a60
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
msc.type: authoredcontent
ms.openlocfilehash: c77e4738444c61d60bdd78a50b36f98be41fc271
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30867897"
---
<a name="creating-an-action-vb"></a>Crear una acción (VB)
====================
por [Microsoft](https://github.com/microsoft)

> Obtenga información acerca de cómo agregar una nueva acción a un controlador de MVC de ASP.NET. Obtenga información acerca de los requisitos para un método como una acción.


El objetivo de este tutorial es explicar cómo puede crear una nueva acción de controlador. Conocer los requisitos de un método de acción. También aprenderá cómo impedir que un método que se expone como una acción.

## <a name="adding-an-action-to-a-controller"></a>Agregar una acción a un controlador

Agregar una nueva acción a un controlador mediante la adición de un nuevo método al controlador. Por ejemplo, el controlador en el listado 1 contiene una acción denominada Index() y una acción denominada SayHello(). Ambos métodos se exponen como acciones.

**Lista 1 - Controllers\HomeController.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample1.vb)]

Con el fin de exponer al universo como una acción, un método debe cumplir ciertos requisitos:

- El método debe ser público.
- El método no puede ser un método estático.
- El método no puede ser un método de extensión.
- El método no puede ser un constructor, un captador o un establecedor.
- El método no puede tener los tipos genéricos abiertos.
- El método no es un método de la clase base del controlador.
- El método no puede contener **ref** o **out** parámetros.

Tenga en cuenta que no hay ninguna restricción en el tipo de valor devuelto de una acción del controlador. Una acción de controlador puede devolver una cadena, un valor DateTime, una instancia de la clase Random, o nulo. El marco de MVC de ASP.NET convertirá cualquier tipo de valor devuelto no es un resultado de acción en una cadena y procesar la cadena incluida en el explorador.

Cuando se agrega cualquier método que no infrinja estos requisitos a un controlador, el método se expone como una acción de controlador. Tenga cuidado aquí. Cualquier persona conectada a Internet puede invocar una acción de controlador. No, por ejemplo, cree una acción de controlador DeleteMyWebsite().

## <a name="preventing-a-public-method-from-being-invoked"></a>Impide que se está llamando a un método público

Si necesita crear un método público en una clase de controlador y no desea exponer el método como una acción de controlador, a continuación, puede impedir que el método que se invoca mediante el uso de la &lt;NonAction&gt; atributo. Por ejemplo, el controlador en el listado 2 contiene un método público denominado CompanySecrets() que se decora con el &lt;NonAction&gt; atributo.

**La lista 2 - Controllers\WorkController.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample2.vb)]

Si se intenta invocar la acción del controlador CompanySecrets() escribiendo /Work/CompanySecrets en la barra de direcciones del explorador, a continuación, obtendrá el mensaje de error en la figura 1.


[![Invocar un método NonAction](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)

**Figura 01**: invocar un método NonAction ([haga clic aquí para ver la imagen a tamaño completo](creating-an-action-vb/_static/image2.png))

> [!div class="step-by-step"]
> [Anterior](creating-a-controller-vb.md)
> [Siguiente](aspnet-mvc-controllers-overview-cs.md)
