---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Agregar un nuevo elemento a la base de datos | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 36251ba907a6f580b63f0fded0591c26b6ff879e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818513"
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
