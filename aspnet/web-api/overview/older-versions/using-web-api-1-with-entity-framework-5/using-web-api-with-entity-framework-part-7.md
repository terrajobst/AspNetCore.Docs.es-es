---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'Parte 7: Creación de los principales página | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: bb4704e7f4f13fab04acdbdd642174884517e18a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829400"
---
<a name="part-7-creating-the-main-page"></a><span data-ttu-id="99e4b-102">Parte 7: Creación de los principales página</span><span class="sxs-lookup"><span data-stu-id="99e4b-102">Part 7: Creating the Main Page</span></span>
====================
<span data-ttu-id="99e4b-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="99e4b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="99e4b-104">Descargue el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="99e4b-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a><span data-ttu-id="99e4b-105">Creación de los principales página</span><span class="sxs-lookup"><span data-stu-id="99e4b-105">Creating the Main Page</span></span>

<span data-ttu-id="99e4b-106">En esta sección, creará la página principal de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="99e4b-106">In this section, you will create the main application page.</span></span> <span data-ttu-id="99e4b-107">Esta página será más compleja que la página de administración, por lo que se alcanzará en varios pasos.</span><span class="sxs-lookup"><span data-stu-id="99e4b-107">This page will be more complex than the Admin page, so we'll approach it in several steps.</span></span> <span data-ttu-id="99e4b-108">En el camino, verá algunas técnicas más avanzadas de Knockout.js.</span><span class="sxs-lookup"><span data-stu-id="99e4b-108">Along the way, you'll see some more advanced Knockout.js techniques.</span></span> <span data-ttu-id="99e4b-109">Este es el diseño básico de la página:</span><span class="sxs-lookup"><span data-stu-id="99e4b-109">Here is the basic layout of the page:</span></span>

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- <span data-ttu-id="99e4b-110">"Productos" contiene una matriz de productos.</span><span class="sxs-lookup"><span data-stu-id="99e4b-110">"Products" holds an array of products.</span></span>
- <span data-ttu-id="99e4b-111">"Cesta" contiene una matriz de productos con cantidades.</span><span class="sxs-lookup"><span data-stu-id="99e4b-111">"Cart" holds an array of products with quantities.</span></span> <span data-ttu-id="99e4b-112">Al hacer clic en "Add to Cart" actualiza el carro de compra.</span><span class="sxs-lookup"><span data-stu-id="99e4b-112">Clicking "Add to Cart" updates the cart.</span></span>
- <span data-ttu-id="99e4b-113">"Orders" contiene una matriz de identificadores de pedido.</span><span class="sxs-lookup"><span data-stu-id="99e4b-113">"Orders" holds an array of order IDs.</span></span>
- <span data-ttu-id="99e4b-114">"Detalles" contiene un detalle de pedido, que es una matriz de elementos (productos con cantidades)</span><span class="sxs-lookup"><span data-stu-id="99e4b-114">"Details" holds an order detail, which is an array of items (products with quantities)</span></span>

<span data-ttu-id="99e4b-115">Comenzaremos por definir algunas diseño básico en HTML, con ningún enlace de datos o la secuencia de comandos.</span><span class="sxs-lookup"><span data-stu-id="99e4b-115">We'll start by defining some basic layout in HTML, with no data binding or script.</span></span> <span data-ttu-id="99e4b-116">Abra el archivo Views/Home/Index.cshtml y reemplace todo el contenido por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="99e4b-116">Open the file Views/Home/Index.cshtml and replace all of the contents with the following:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

<span data-ttu-id="99e4b-117">A continuación, agregue una sección de Scripts y crear un modelo de vista vacío:</span><span class="sxs-lookup"><span data-stu-id="99e4b-117">Next, add a Scripts section and create an empty view-model:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

<span data-ttu-id="99e4b-118">En función del diseño esbozado anteriormente, nuestro modelo de vista debe observables para los productos, carro, pedidos y detalles.</span><span class="sxs-lookup"><span data-stu-id="99e4b-118">Based on the design sketched earlier, our view model needs observables for products, cart, orders, and details.</span></span> <span data-ttu-id="99e4b-119">Agregue las siguientes variables para el `AppViewModel` objeto:</span><span class="sxs-lookup"><span data-stu-id="99e4b-119">Add the following variables to the `AppViewModel` object:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

<span data-ttu-id="99e4b-120">Los usuarios pueden agregar elementos de la lista de productos en el carro de compra y quitar elementos del carro de la.</span><span class="sxs-lookup"><span data-stu-id="99e4b-120">Users can add items from the products list into the cart, and remove items from the cart.</span></span> <span data-ttu-id="99e4b-121">Para encapsular estas funciones, vamos a crear otra clase de modelo de vista que representa un producto.</span><span class="sxs-lookup"><span data-stu-id="99e4b-121">To encapsulate these functions, we'll create another view-model class that represents a product.</span></span> <span data-ttu-id="99e4b-122">Agrega el código siguiente a `AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="99e4b-122">Add the following code to `AppViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

<span data-ttu-id="99e4b-123">El `ProductViewModel` clase contiene dos funciones que se usan para mover el producto hacia y desde el carro de compra: `addItemToCart` agrega una unidad del producto al carro de compra, y `removeAllFromCart` quita todas las cantidades del producto.</span><span class="sxs-lookup"><span data-stu-id="99e4b-123">The `ProductViewModel` class contains two functions that are used to move the product to and from the cart: `addItemToCart` adds one unit of the product to the cart, and `removeAllFromCart` removes all quantities of the product.</span></span>

<span data-ttu-id="99e4b-124">Los usuarios pueden seleccionar un pedido existente y obtener los detalles del pedido.</span><span class="sxs-lookup"><span data-stu-id="99e4b-124">Users can select an existing order and get the order details.</span></span> <span data-ttu-id="99e4b-125">Encapsularemos esta funcionalidad en otro modelo de vista:</span><span class="sxs-lookup"><span data-stu-id="99e4b-125">We'll encapsulate this functionality into another view-model:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

<span data-ttu-id="99e4b-126">El `OrderDetailsViewModel` se inicializa con un pedido, y captura los detalles del pedido mediante el envío de una solicitud AJAX al servidor.</span><span class="sxs-lookup"><span data-stu-id="99e4b-126">The `OrderDetailsViewModel` is initialized with an order, and it fetches the order details by sending an AJAX request to the server.</span></span>

<span data-ttu-id="99e4b-127">Además, tenga en cuenta la `total` propiedad en el `OrderDetailsViewModel`.</span><span class="sxs-lookup"><span data-stu-id="99e4b-127">Also, notice the `total` property on the `OrderDetailsViewModel`.</span></span> <span data-ttu-id="99e4b-128">Esta propiedad es un tipo especial de objeto observable que se llama a un [calcula observable](http://knockoutjs.com/documentation/computedObservables.html).</span><span class="sxs-lookup"><span data-stu-id="99e4b-128">This property is a special kind of observable called a [computed observable](http://knockoutjs.com/documentation/computedObservables.html).</span></span> <span data-ttu-id="99e4b-129">Como el nombre implica, un objeto observable calculado permite enlazar datos a un valor calculado&#8212;en este caso, el costo total del pedido.</span><span class="sxs-lookup"><span data-stu-id="99e4b-129">As the name implies, a computed observable lets you data bind to a computed value&#8212;in this case, the total cost of the order.</span></span>

<span data-ttu-id="99e4b-130">A continuación, agregue estas funciones para `AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="99e4b-130">Next, add these functions to `AppViewModel`:</span></span>

- <span data-ttu-id="99e4b-131">`resetCart` Quita todos los elementos del carro de compra.</span><span class="sxs-lookup"><span data-stu-id="99e4b-131">`resetCart` removes all items from the cart.</span></span>
- <span data-ttu-id="99e4b-132">`getDetails` Obtiene los detalles de un pedido (por pusing un nuevo `OrderDetailsViewModel` hasta la `details` lista).</span><span class="sxs-lookup"><span data-stu-id="99e4b-132">`getDetails` gets the details for an order (by pusing a new `OrderDetailsViewModel` onto the `details` list).</span></span>
- <span data-ttu-id="99e4b-133">`createOrder` crea un nuevo pedido y vacía el carro de compra.</span><span class="sxs-lookup"><span data-stu-id="99e4b-133">`createOrder` creates a new order and empties the cart.</span></span>


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

<span data-ttu-id="99e4b-134">Por último, inicialice el modelo de vista realizando solicitudes AJAX para los productos y pedidos:</span><span class="sxs-lookup"><span data-stu-id="99e4b-134">Finally, initialize the view model by making AJAX requests for the products and orders:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

<span data-ttu-id="99e4b-135">Bien, que es una gran cantidad de código, pero lo construimos seguridad paso a paso, por lo que espero que el diseño está desactivado.</span><span class="sxs-lookup"><span data-stu-id="99e4b-135">OK, that's a lot of code, but we built it up step-by-step, so hopefully the design is clear.</span></span> <span data-ttu-id="99e4b-136">Ahora podemos agregar algunos enlaces de Knockout.js para el código HTML.</span><span class="sxs-lookup"><span data-stu-id="99e4b-136">Now we can add some Knockout.js bindings to the HTML.</span></span>

<span data-ttu-id="99e4b-137">**Productos**</span><span class="sxs-lookup"><span data-stu-id="99e4b-137">**Products**</span></span>

<span data-ttu-id="99e4b-138">Estos son los enlaces de la lista de productos:</span><span class="sxs-lookup"><span data-stu-id="99e4b-138">Here are the bindings for the product list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

<span data-ttu-id="99e4b-139">Esto se recorre en iteración la matriz de productos y muestra el nombre y el precio.</span><span class="sxs-lookup"><span data-stu-id="99e4b-139">This iterates over the products array and displays the name and price.</span></span> <span data-ttu-id="99e4b-140">El botón "Agregar a pedido" sólo es visible cuando el usuario ha iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="99e4b-140">The "Add to Order" button is visible only when the user is logged in.</span></span>

<span data-ttu-id="99e4b-141">Las llamadas del botón "Agregar a pedido" `addItemToCart` en el `ProductViewModel` instancia para el producto.</span><span class="sxs-lookup"><span data-stu-id="99e4b-141">The "Add to Order" button calls `addItemToCart` on the `ProductViewModel` instance for the product.</span></span> <span data-ttu-id="99e4b-142">Esto muestra una interesante característica de Knockout.js: cuando un modelo de vista contiene otros modelos de vista, puede aplicar los enlaces para el modelo interno.</span><span class="sxs-lookup"><span data-stu-id="99e4b-142">This demonstrates a nice feature of Knockout.js: When a view-model contains other view-models, you can apply the bindings to the inner model.</span></span> <span data-ttu-id="99e4b-143">En este ejemplo, los enlaces dentro de la `foreach` se aplican a cada uno de los `ProductViewModel` instancias.</span><span class="sxs-lookup"><span data-stu-id="99e4b-143">In this example, the bindings within the `foreach` are applied to each of the `ProductViewModel` instances.</span></span> <span data-ttu-id="99e4b-144">Este enfoque es mucho más limpio que poner toda la funcionalidad en un único modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="99e4b-144">This approach is much cleaner than putting all of the functionality into a single view-model.</span></span>

<span data-ttu-id="99e4b-145">**Carro de compra**</span><span class="sxs-lookup"><span data-stu-id="99e4b-145">**Cart**</span></span>

<span data-ttu-id="99e4b-146">Estos son los enlaces para el carro de compra:</span><span class="sxs-lookup"><span data-stu-id="99e4b-146">Here are the bindings for the cart:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

<span data-ttu-id="99e4b-147">Esto se recorre en iteración la matriz de carro y muestra el nombre, precio y cantidad.</span><span class="sxs-lookup"><span data-stu-id="99e4b-147">This iterates over the cart array and displays the name, price, and quantity.</span></span> <span data-ttu-id="99e4b-148">Tenga en cuenta que el vínculo "Remove" y el botón "Crear un pedido" están enlazados a las funciones del modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="99e4b-148">Note that the "Remove" link and the "Create Order" button are bound to view-model functions.</span></span>

<span data-ttu-id="99e4b-149">**Pedidos**</span><span class="sxs-lookup"><span data-stu-id="99e4b-149">**Orders**</span></span>

<span data-ttu-id="99e4b-150">Estos son los enlaces de la lista de pedidos:</span><span class="sxs-lookup"><span data-stu-id="99e4b-150">Here are the bindings for the orders list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

<span data-ttu-id="99e4b-151">Se recorre en iteración los pedidos y muestra el identificador de pedido.</span><span class="sxs-lookup"><span data-stu-id="99e4b-151">This iterates over the orders and shows the order ID.</span></span> <span data-ttu-id="99e4b-152">El evento click en el vínculo está enlazado a la `getDetails` función.</span><span class="sxs-lookup"><span data-stu-id="99e4b-152">The click event on the link is bound to the `getDetails` function.</span></span>

<span data-ttu-id="99e4b-153">**Detalles del pedido**</span><span class="sxs-lookup"><span data-stu-id="99e4b-153">**Order Details**</span></span>

<span data-ttu-id="99e4b-154">Estos son los enlaces para los detalles del pedido:</span><span class="sxs-lookup"><span data-stu-id="99e4b-154">Here are the bindings for the order details:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

<span data-ttu-id="99e4b-155">Esto se recorre en iteración los elementos en el orden y muestra el producto, precio y cantidad.</span><span class="sxs-lookup"><span data-stu-id="99e4b-155">This iterates over the items in the order and displays the product, price, and quanity.</span></span> <span data-ttu-id="99e4b-156">La etiqueta div circundante sólo está visible si la matriz de detalles contiene uno o varios elementos.</span><span class="sxs-lookup"><span data-stu-id="99e4b-156">The surrounding div is visible only if the details array contains one or more items.</span></span>

## <a name="conclusion"></a><span data-ttu-id="99e4b-157">Conclusión</span><span class="sxs-lookup"><span data-stu-id="99e4b-157">Conclusion</span></span>

<span data-ttu-id="99e4b-158">En este tutorial, ha creado una aplicación que utiliza Entity Framework para comunicarse con la base de datos y ASP.NET Web API para proporcionar una interfaz pública encima de la capa de datos.</span><span class="sxs-lookup"><span data-stu-id="99e4b-158">In this tutorial, you created an application that uses Entity Framework to communicate with the database, and ASP.NET Web API to provide a public-facing interface on top of the data layer.</span></span> <span data-ttu-id="99e4b-159">ASP.NET MVC 4 se usa para representar las páginas HTML y Knockout.js además de jQuery para proporcionar interacciones dinámicas sin las recargas de página.</span><span class="sxs-lookup"><span data-stu-id="99e4b-159">We use ASP.NET MVC 4 to render the HTML pages, and Knockout.js plus jQuery to provide dynamic interactions without page reloads.</span></span>

<span data-ttu-id="99e4b-160">Recursos adicionales:</span><span class="sxs-lookup"><span data-stu-id="99e4b-160">Additional resources:</span></span>

- [<span data-ttu-id="99e4b-161">Mapa de contenido de acceso de datos de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="99e4b-161">ASP.NET Data Access Content Map</span></span>](https://msdn.microsoft.com/library/6759sth4.aspx)
- [<span data-ttu-id="99e4b-162">Centro para desarrolladores de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="99e4b-162">Entity Framework Developer Center</span></span>](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [<span data-ttu-id="99e4b-163">Anterior</span><span class="sxs-lookup"><span data-stu-id="99e4b-163">Previous</span></span>](using-web-api-with-entity-framework-part-6.md)
