---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Agregar un nuevo elemento a la base de datos | Documentos de Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: d33355b1bd286513958f71ce5521942a6cbb584f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="add-a-new-item-to-the-database"></a>Agregar un nuevo elemento a la base de datos
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargar el proyecto completado](https://github.com/MikeWasson/BookService)

En esta sección, agregará la capacidad de los usuarios crear un nuevo libro. En app.js, agregue el código siguiente para el modelo de vista:

[!code-javascript[Main](part-9/samples/sample1.js)]

En Index.cshtml, reemplace el marcado siguiente:

[!code-html[Main](part-9/samples/sample2.html)]

Con:

[!code-html[Main](part-9/samples/sample3.html)]

Este marcado crea un formulario para enviar a un autor de nuevo. Los valores de la lista desplegable de autor están enlazado a datos a la `authors` observable en el modelo de vista. Para las entradas de formulario, los valores están enlazados a datos a la `newBook` propiedad del modelo de vista.

El controlador de envío en el formulario se enlaza a la `addBook` función:

[!code-html[Main](part-9/samples/sample4.html)]

El `addBook` función lee los valores actuales de las entradas de formulario enlazado a datos para crear un objeto JSON. A continuación, devuelve el objeto JSON para `/api/books`.

>[!div class="step-by-step"]
[Anterior](part-8.md)
[Siguiente](part-10.md)
