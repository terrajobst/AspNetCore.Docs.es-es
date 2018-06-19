---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Agregar un nuevo elemento a la base de datos | Documentos de Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 5845c092c4d7aee12b33b3f0a49c0e944c0fb9aa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868378"
---
<a name="add-a-new-item-to-the-database"></a><span data-ttu-id="beeb0-102">Agregar un nuevo elemento a la base de datos</span><span class="sxs-lookup"><span data-stu-id="beeb0-102">Add a New Item to the Database</span></span>
====================
<span data-ttu-id="beeb0-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="beeb0-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="beeb0-104">Descargar el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="beeb0-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="beeb0-105">En esta sección, agregará la capacidad de los usuarios crear un nuevo libro.</span><span class="sxs-lookup"><span data-stu-id="beeb0-105">In this section, you will add the ability for users to create a new book.</span></span> <span data-ttu-id="beeb0-106">En app.js, agregue el código siguiente para el modelo de vista:</span><span class="sxs-lookup"><span data-stu-id="beeb0-106">In app.js, add the following code to the view model:</span></span>

[!code-javascript[Main](part-9/samples/sample1.js)]

<span data-ttu-id="beeb0-107">En Index.cshtml, reemplace el marcado siguiente:</span><span class="sxs-lookup"><span data-stu-id="beeb0-107">In Index.cshtml, replace the following markup:</span></span>

[!code-html[Main](part-9/samples/sample2.html)]

<span data-ttu-id="beeb0-108">Por:</span><span class="sxs-lookup"><span data-stu-id="beeb0-108">With:</span></span>

[!code-html[Main](part-9/samples/sample3.html)]

<span data-ttu-id="beeb0-109">Este marcado crea un formulario para enviar a un autor de nuevo.</span><span class="sxs-lookup"><span data-stu-id="beeb0-109">This markup creates a form for submitting a new author.</span></span> <span data-ttu-id="beeb0-110">Los valores de la lista desplegable de autor están enlazado a datos a la `authors` observable en el modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="beeb0-110">The values for the author drop-down list are data-bound to the `authors` observable in the view model.</span></span> <span data-ttu-id="beeb0-111">Para las entradas de formulario, los valores están enlazados a datos a la `newBook` propiedad del modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="beeb0-111">For the other form inputs, the values are data-bound to the `newBook` property of the view model.</span></span>

<span data-ttu-id="beeb0-112">El controlador de envío en el formulario se enlaza a la `addBook` función:</span><span class="sxs-lookup"><span data-stu-id="beeb0-112">The submit handler on the form is bound to the `addBook` function:</span></span>

[!code-html[Main](part-9/samples/sample4.html)]

<span data-ttu-id="beeb0-113">El `addBook` función lee los valores actuales de las entradas de formulario enlazado a datos para crear un objeto JSON.</span><span class="sxs-lookup"><span data-stu-id="beeb0-113">The `addBook` function reads the current values of the data-bound form inputs to create a JSON object.</span></span> <span data-ttu-id="beeb0-114">A continuación, devuelve el objeto JSON para `/api/books`.</span><span class="sxs-lookup"><span data-stu-id="beeb0-114">Then it POSTs the JSON object to `/api/books`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="beeb0-115">[Anterior](part-8.md)
> [Siguiente](part-10.md)</span><span class="sxs-lookup"><span data-stu-id="beeb0-115">[Previous](part-8.md)
[Next](part-10.md)</span></span>
