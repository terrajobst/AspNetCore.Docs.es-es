---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Agregar un nuevo elemento a la base de datos | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: b1f7935c70efcc3ee486e76fc356ff43716632dd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368882"
---
<a name="add-a-new-item-to-the-database"></a>Agregar un nuevo elemento a la base de datos
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargue el proyecto completado](https://github.com/MikeWasson/BookService)

En esta sección, agregará la capacidad de los usuarios crear un nuevo libro. En app.js, agregue el siguiente código al modelo de vista:

[!code-javascript[Main](part-9/samples/sample1.js)]

En Index.cshtml, reemplace el marcado siguiente:

[!code-html[Main](part-9/samples/sample2.html)]

Por:

[!code-html[Main](part-9/samples/sample3.html)]

Este marcado crea un formulario para enviar a un nuevo autor. Los valores de la lista desplegable de autor son datos enlazados a la `authors` perceptible en el modelo de vista. Para las entradas de formulario, los valores están enlazados a datos a la `newBook` propiedad del modelo de vista.

El controlador de envío del formulario se enlaza a la `addBook` función:

[!code-html[Main](part-9/samples/sample4.html)]

El `addBook` función lee los valores actuales de las entradas de formulario enlazado a datos para crear un objeto JSON. A continuación, envía el objeto JSON a `/api/books`.

> [!div class="step-by-step"]
> [Anterior](part-8.md)
> [Siguiente](part-10.md)
