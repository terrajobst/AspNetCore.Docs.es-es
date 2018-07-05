---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: 'Parte 5: Crear una IU dinámica con Knockout.js | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: 1f645b853fba999ade11c420eceba1b310f80ca3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368433"
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="3514e-102">Parte 5: Crear una IU dinámica con Knockout.js</span><span class="sxs-lookup"><span data-stu-id="3514e-102">Part 5: Creating a Dynamic UI with Knockout.js</span></span>
====================
<span data-ttu-id="3514e-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3514e-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="3514e-104">Descargue el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="3514e-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="3514e-105">Crear una IU dinámica con Knockout.js</span><span class="sxs-lookup"><span data-stu-id="3514e-105">Creating a Dynamic UI with Knockout.js</span></span>

<span data-ttu-id="3514e-106">En esta sección, vamos a usar Knockout.js para agregar funcionalidad a la vista de administración.</span><span class="sxs-lookup"><span data-stu-id="3514e-106">In this section, we'll use Knockout.js to add functionality to the Admin view.</span></span>

<span data-ttu-id="3514e-107">[Knockout.js](http://knockoutjs.com/) es una biblioteca de Javascript que facilita la tarea enlazar controles HTML a los datos.</span><span class="sxs-lookup"><span data-stu-id="3514e-107">[Knockout.js](http://knockoutjs.com/) is a Javascript library that makes it easy to bind HTML controls to data.</span></span> <span data-ttu-id="3514e-108">Knockout.js usa el patrón Model-View-ViewModel (MVVM).</span><span class="sxs-lookup"><span data-stu-id="3514e-108">Knockout.js uses the Model-View-ViewModel (MVVM) pattern.</span></span>

- <span data-ttu-id="3514e-109">El *modelo* es la representación del lado servidor de los datos en el dominio de negocio (en nuestro caso, productos y pedidos).</span><span class="sxs-lookup"><span data-stu-id="3514e-109">The *model* is the server-side representation of the data in the business domain (in our case, products and orders).</span></span>
- <span data-ttu-id="3514e-110">El *vista* es la capa de presentación (HTML).</span><span class="sxs-lookup"><span data-stu-id="3514e-110">The *view* is the presentation layer (HTML).</span></span>
- <span data-ttu-id="3514e-111">El *vista-modelo* es un objeto de Javascript que contiene los datos del modelo.</span><span class="sxs-lookup"><span data-stu-id="3514e-111">The *view-model* is a Javascript object that holds the model data.</span></span> <span data-ttu-id="3514e-112">El modelo de vista es una abstracción de código de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="3514e-112">The view-model is a code abstraction of the UI.</span></span> <span data-ttu-id="3514e-113">No tiene ningún conocimiento de la representación de HTML.</span><span class="sxs-lookup"><span data-stu-id="3514e-113">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="3514e-114">En su lugar, representa las características abstractas de la vista, como "una lista de elementos".</span><span class="sxs-lookup"><span data-stu-id="3514e-114">Instead, it represents abstract features of the view, such as "a list of items".</span></span>

<span data-ttu-id="3514e-115">La vista está enlazada a datos al modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="3514e-115">The view is data-bound to the view-model.</span></span> <span data-ttu-id="3514e-116">Actualizaciones para el modelo de vista se reflejan automáticamente en la vista.</span><span class="sxs-lookup"><span data-stu-id="3514e-116">Updates to the view-model are automatically reflected in the view.</span></span> <span data-ttu-id="3514e-117">El modelo de vista también obtiene los eventos de la vista, como los clics de botón y realiza operaciones en el modelo, como la creación de un pedido.</span><span class="sxs-lookup"><span data-stu-id="3514e-117">The view-model also gets events from the view, such as button clicks, and performs operations on the model, such as creating an order.</span></span>

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

<span data-ttu-id="3514e-118">En primer lugar, definiremos el modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="3514e-118">First we'll define the view-model.</span></span> <span data-ttu-id="3514e-119">Después de eso, se enlazará el marcado HTML para el modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="3514e-119">After that, we will bind the HTML markup to the view-model.</span></span>

<span data-ttu-id="3514e-120">Agregue la siguiente sección de Razor para Admin.cshtml:</span><span class="sxs-lookup"><span data-stu-id="3514e-120">Add the following Razor section to Admin.cshtml:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

<span data-ttu-id="3514e-121">Puede agregar esta sección en cualquier lugar en el archivo.</span><span class="sxs-lookup"><span data-stu-id="3514e-121">You can add this section anywhere in the file.</span></span> <span data-ttu-id="3514e-122">Cuando se representa la vista, la sección que aparece en la parte inferior de la página HTML, justo antes del cierre &lt;corporal&gt; etiqueta.</span><span class="sxs-lookup"><span data-stu-id="3514e-122">When the view is rendered, the section appears at the bottom of the HTML page, right before the closing &lt;/body&gt; tag.</span></span>

<span data-ttu-id="3514e-123">Todo el script de esta página se irá dentro de la etiqueta de secuencia de comandos indicada por el comentario:</span><span class="sxs-lookup"><span data-stu-id="3514e-123">All of the script for this page will go inside the script tag indicated by the comment:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

<span data-ttu-id="3514e-124">En primer lugar, defina una clase de modelo de vista:</span><span class="sxs-lookup"><span data-stu-id="3514e-124">First, define a view-model class:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

<span data-ttu-id="3514e-125">**ko.observableArray** es un tipo especial de objeto de Knockout, llama a un *observable*.</span><span class="sxs-lookup"><span data-stu-id="3514e-125">**ko.observableArray** is a special kind of object in Knockout, called an *observable*.</span></span> <span data-ttu-id="3514e-126">Desde el [Knockout.js documentación](http://knockoutjs.com/documentation/observables.html): un objeto observable es un "objeto de JavaScript que puede notificar a los suscriptores sobre los cambios."</span><span class="sxs-lookup"><span data-stu-id="3514e-126">From the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html): An observable is a "JavaScript object that can notify subscribers about changes."</span></span> <span data-ttu-id="3514e-127">Cuando se cambia el contenido de un objeto observable, la vista se actualiza automáticamente para que coincida con.</span><span class="sxs-lookup"><span data-stu-id="3514e-127">When the contents of an observable change, the view is automatically updated to match.</span></span>

<span data-ttu-id="3514e-128">Para rellenar el `products` de matriz, realice una solicitud AJAX a la API web.</span><span class="sxs-lookup"><span data-stu-id="3514e-128">To populate the `products` array, make an AJAX request to the web API.</span></span> <span data-ttu-id="3514e-129">Recuerde que se almacena el identificador URI base para la API en el contenedor de vista (consulte [4ª](using-web-api-with-entity-framework-part-4.md) del tutorial).</span><span class="sxs-lookup"><span data-stu-id="3514e-129">Recall that we stored the base URI for the API in the view bag (see [Part 4](using-web-api-with-entity-framework-part-4.md) of the tutorial).</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

<span data-ttu-id="3514e-130">A continuación, agregar funciones para el modelo de vista para crear, actualizar y eliminar productos.</span><span class="sxs-lookup"><span data-stu-id="3514e-130">Next, add functions to the view-model to create, update, and delete products.</span></span> <span data-ttu-id="3514e-131">Estas funciones enviar las llamadas AJAX a la API web y usar los resultados para actualizar el modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="3514e-131">These functions submit AJAX calls to the web API and use the results to update the view-model.</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

<span data-ttu-id="3514e-132">Ahora la parte más importante: cuando el DOM es fulled llamada cargado, el **ko.applyBindings** de función y pasar una nueva instancia de la `ProductsViewModel`:</span><span class="sxs-lookup"><span data-stu-id="3514e-132">Now the most important part: When the DOM is fulled loaded, call the **ko.applyBindings** function and pass in a new instance of the `ProductsViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

<span data-ttu-id="3514e-133">El **ko.applyBindings** método Knockout se activa y se conecta el modelo de vista a la vista.</span><span class="sxs-lookup"><span data-stu-id="3514e-133">The **ko.applyBindings** method activates Knockout and wires up the view-model to the view.</span></span>

<span data-ttu-id="3514e-134">Ahora que tenemos un modelo de vista, podemos crear los enlaces.</span><span class="sxs-lookup"><span data-stu-id="3514e-134">Now that we have a view-model, we can create the bindings.</span></span> <span data-ttu-id="3514e-135">En Knockout.js, hacerlo agregando `data-bind` atributos a elementos HTML.</span><span class="sxs-lookup"><span data-stu-id="3514e-135">In Knockout.js, you do this by adding `data-bind` attributes to HTML elements.</span></span> <span data-ttu-id="3514e-136">Por ejemplo, para enlazar una lista de HTML a una matriz, use el `foreach` enlace:</span><span class="sxs-lookup"><span data-stu-id="3514e-136">For example, to bind an HTML list to an array, use the `foreach` binding:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

<span data-ttu-id="3514e-137">El `foreach` enlace recorre en iteración la matriz y crea elementos para cada objeto de secundarios de la matriz.</span><span class="sxs-lookup"><span data-stu-id="3514e-137">The `foreach` binding iterates through the array and creates child elements for each object in the array.</span></span> <span data-ttu-id="3514e-138">Los enlaces en los elementos secundarios pueden hacer referencia a las propiedades de los objetos de la matriz.</span><span class="sxs-lookup"><span data-stu-id="3514e-138">Bindings on the child elements can refer to properties on the array objects.</span></span>

<span data-ttu-id="3514e-139">Agregue los siguientes enlaces a la lista de "productos de actualización":</span><span class="sxs-lookup"><span data-stu-id="3514e-139">Add the following bindings to the "update-products" list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

<span data-ttu-id="3514e-140">El `<li>` se produce un elemento dentro del ámbito de la **foreach** enlace.</span><span class="sxs-lookup"><span data-stu-id="3514e-140">The `<li>` element occurs within the scope of the **foreach** binding.</span></span> <span data-ttu-id="3514e-141">Que significa que tendrá Knockout representar el elemento una vez para cada producto en el `products` matriz.</span><span class="sxs-lookup"><span data-stu-id="3514e-141">That means Knockout will render the element once for each product in the `products` array.</span></span> <span data-ttu-id="3514e-142">Todos los enlaces dentro de la `<li>` elemento hacen referencia a esa instancia del producto.</span><span class="sxs-lookup"><span data-stu-id="3514e-142">All of the bindings within the `<li>` element refer to that product instance.</span></span> <span data-ttu-id="3514e-143">Por ejemplo, `$data.Name` hace referencia a la `Name` propiedad sobre el producto.</span><span class="sxs-lookup"><span data-stu-id="3514e-143">For example, `$data.Name` refers to the `Name` property on the product.</span></span>

<span data-ttu-id="3514e-144">Para establecer los valores de las entradas de texto, use la `value` enlace.</span><span class="sxs-lookup"><span data-stu-id="3514e-144">To set the values of the text inputs, use the `value` binding.</span></span> <span data-ttu-id="3514e-145">Los botones están enlazados a las funciones en la vista de modelo, utilizando el `click` enlace.</span><span class="sxs-lookup"><span data-stu-id="3514e-145">The buttons are bound to functions on the model-view, using the `click` binding.</span></span> <span data-ttu-id="3514e-146">La instancia de un producto se pasa como un parámetro para cada función.</span><span class="sxs-lookup"><span data-stu-id="3514e-146">The product instance is passed as a parameter to each function.</span></span> <span data-ttu-id="3514e-147">Para obtener más información, el [Knockout.js documentación](http://knockoutjs.com/documentation/observables.html) tiene buena descripciones de los distintos enlaces.</span><span class="sxs-lookup"><span data-stu-id="3514e-147">For more information, the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html) has good descriptions of the various bindings.</span></span>

<span data-ttu-id="3514e-148">A continuación, agregue un enlace para el **enviar** eventos en el formulario Agregar producto:</span><span class="sxs-lookup"><span data-stu-id="3514e-148">Next, add a binding for the **submit** event on the Add Product form:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

<span data-ttu-id="3514e-149">Llama este enlace el `create` función en el modelo de vista para crear un nuevo producto.</span><span class="sxs-lookup"><span data-stu-id="3514e-149">This binding calls the `create` function on the view-model to create a new product.</span></span>

<span data-ttu-id="3514e-150">Este es el código completo de la vista de administración:</span><span class="sxs-lookup"><span data-stu-id="3514e-150">Here is the complete code for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

<span data-ttu-id="3514e-151">Ejecute la aplicación, inicie sesión con la cuenta de administrador y haga clic en el vínculo "Admin".</span><span class="sxs-lookup"><span data-stu-id="3514e-151">Run the application, log in with the Administrator account, and click the "Admin" link.</span></span> <span data-ttu-id="3514e-152">Debería ver la lista de productos y ser capaz de crear, actualizar o eliminar los productos.</span><span class="sxs-lookup"><span data-stu-id="3514e-152">You should see the list of products, and be able to create, update, or delete products.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3514e-153">[Anterior](using-web-api-with-entity-framework-part-4.md)
> [Siguiente](using-web-api-with-entity-framework-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="3514e-153">[Previous](using-web-api-with-entity-framework-part-4.md)
[Next](using-web-api-with-entity-framework-part-6.md)</span></span>
