---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Crear la vista (IU) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: eeaa36ede45b82afd112a270acf68105d1ae6dbc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820991"
---
<a name="create-the-view-ui"></a><span data-ttu-id="52633-102">Crear la vista (IU)</span><span class="sxs-lookup"><span data-stu-id="52633-102">Create the View (UI)</span></span>
====================
<span data-ttu-id="52633-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="52633-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="52633-104">Descargue el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="52633-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="52633-105">En esta sección, se iniciará definir el código HTML de la aplicación y Agregar enlace de datos entre el código HTML y el modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="52633-105">In this section, you will start to define the HTML for the app, and add data binding between the HTML and the view model.</span></span>

<span data-ttu-id="52633-106">Abra el archivo Views/Home/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="52633-106">Open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="52633-107">Reemplace todo el contenido de ese archivo por el siguiente.</span><span class="sxs-lookup"><span data-stu-id="52633-107">Replace the entire contents of that file with the following.</span></span>

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

<span data-ttu-id="52633-108">La mayoría de los `div` elementos existen para [Bootstrap](http://getbootstrap.com/) estilo.</span><span class="sxs-lookup"><span data-stu-id="52633-108">Most of the `div` elements are there for [Bootstrap](http://getbootstrap.com/) styling.</span></span> <span data-ttu-id="52633-109">Los elementos importantes son aquellas con `data-bind` atributos.</span><span class="sxs-lookup"><span data-stu-id="52633-109">The important elements are the ones with `data-bind` attributes.</span></span> <span data-ttu-id="52633-110">Este atributo vincula el código HTML para el modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="52633-110">This attribute links the HTML to the view model.</span></span>

<span data-ttu-id="52633-111">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="52633-111">For example:</span></span>

[!code-html[Main](part-7/samples/sample2.html)]

<span data-ttu-id="52633-112">En este ejemplo, el &quot; `text` &quot; enlace hace que el `<p>` elemento para mostrar el valor de la `error` propiedad desde el modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="52633-112">In this example, the &quot;`text`&quot; binding causes the `<p>` element to show the value of the `error` property from the view model.</span></span> <span data-ttu-id="52633-113">Recuerde que `error` se ha declarado como un `ko.observable`:</span><span class="sxs-lookup"><span data-stu-id="52633-113">Recall that `error` was declared as a `ko.observable`:</span></span>

[!code-javascript[Main](part-7/samples/sample3.js)]

<span data-ttu-id="52633-114">Cada vez que se asigna un nuevo valor a `error`, Knockout actualiza el texto en el `<p>` elemento.</span><span class="sxs-lookup"><span data-stu-id="52633-114">Whenever a new value is assigned to `error`, Knockout updates the text in the `<p>` element.</span></span>

<span data-ttu-id="52633-115">El `foreach` enlace indica a Knockout que recorra en iteración el contenido de la `books` matriz.</span><span class="sxs-lookup"><span data-stu-id="52633-115">The `foreach` binding tells Knockout to loop through the contents of the `books` array.</span></span> <span data-ttu-id="52633-116">Para cada elemento de la matriz, Knockout crea un nuevo &lt;li&gt; elemento.</span><span class="sxs-lookup"><span data-stu-id="52633-116">For each item in the array, Knockout creates a new &lt;li&gt; element.</span></span> <span data-ttu-id="52633-117">Los enlaces dentro del contexto de la `foreach` hacen referencia a propiedades en el elemento de matriz.</span><span class="sxs-lookup"><span data-stu-id="52633-117">Bindings inside the context of the `foreach` refer to properties on the array item.</span></span> <span data-ttu-id="52633-118">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="52633-118">For example:</span></span>

[!code-html[Main](part-7/samples/sample4.html)]

<span data-ttu-id="52633-119">Aquí el `text` enlace lee la propiedad del autor de cada libro.</span><span class="sxs-lookup"><span data-stu-id="52633-119">Here the `text` binding reads the Author property of each book.</span></span>

<span data-ttu-id="52633-120">Si ejecuta la aplicación ahora, debe ver así:</span><span class="sxs-lookup"><span data-stu-id="52633-120">If you run the application now, it should look like this:</span></span>

![](part-7/_static/image1.png)

<span data-ttu-id="52633-121">La lista de los libros en pantalla se carga de forma asincrónica, una vez cargada la página.</span><span class="sxs-lookup"><span data-stu-id="52633-121">The list of books loads asynchronously, after the page loads.</span></span> <span data-ttu-id="52633-122">Ahora, el &quot;detalles&quot; vínculos no son funcionales.</span><span class="sxs-lookup"><span data-stu-id="52633-122">Right now, the &quot;Details&quot; links are not functional.</span></span> <span data-ttu-id="52633-123">Vamos a agregar esta funcionalidad en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="52633-123">We'll add this functionality in the next section.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="52633-124">[Anterior](part-6.md)
> [Siguiente](part-8.md)</span><span class="sxs-lookup"><span data-stu-id="52633-124">[Previous](part-6.md)
[Next](part-8.md)</span></span>
