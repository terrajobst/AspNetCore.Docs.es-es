---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: Crear una acción (C#) | Documentos de Microsoft
author: microsoft
description: Obtenga información acerca de cómo agregar una nueva acción a un controlador de MVC de ASP.NET. Obtenga información acerca de los requisitos para un método como una acción.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: 7c6145902db59b07e96a5563b138c1a6323946b2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="creating-an-action-c"></a>Crear una acción (C#)
====================
por [Microsoft](https://github.com/microsoft)

> Obtenga información acerca de cómo agregar una nueva acción a un controlador de MVC de ASP.NET. Obtenga información acerca de los requisitos para un método como una acción.


El objetivo de este tutorial es explicar cómo puede crear una nueva acción de controlador. Conocer los requisitos de un método de acción. También aprenderá cómo impedir que un método que se expone como una acción.

## <a name="adding-an-action-to-a-controller"></a>Agregar una acción a un controlador

Agregar una nueva acción a un controlador mediante la adición de un nuevo método al controlador. Por ejemplo, el controlador en el listado 1 contiene una acción denominada Index() y una acción denominada SayHello(). Ambos métodos se exponen como acciones.

**Lista 1 - controllers\homecontroller**

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

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

Si necesita crear un método público en una clase de controlador y no desea exponer el método como una acción de controlador, a continuación, se puede evitar que el método que se invoca mediante el atributo [NonAction]. Por ejemplo, el controlador en el listado 2 contiene un método público denominado CompanySecrets() que se decora con el atributo [NonAction].

**La lista 2 - Controllers\WorkController.cs**

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

Si se intenta invocar la acción del controlador CompanySecrets() escribiendo /Work/CompanySecrets en la barra de direcciones del explorador, a continuación, obtendrá el mensaje de error en la figura 1.


[![Invocar un método NonAction](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)

**Figura 01**: invocar un método NonAction ([haga clic aquí para ver la imagen a tamaño completo](creating-an-action-cs/_static/image2.png))

> [!div class="step-by-step"]
> [Anterior](creating-a-controller-cs.md)
> [Siguiente](asp-net-mvc-routing-overview-vb.md)
