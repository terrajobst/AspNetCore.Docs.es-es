---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'Parte 6: Creación de producto y controladores de orden | Documentos de Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: 6bd485d29821af12b9ebe31b2d04a2d9ab826731
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="part-6-creating-product-and-order-controllers"></a><span data-ttu-id="05920-102">Parte 6: Creación de producto y controladores de orden</span><span class="sxs-lookup"><span data-stu-id="05920-102">Part 6: Creating Product and Order Controllers</span></span>
====================
<span data-ttu-id="05920-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="05920-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="05920-104">Descargar el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="05920-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a><span data-ttu-id="05920-105">Agregar un controlador de productos</span><span class="sxs-lookup"><span data-stu-id="05920-105">Add a Products Controller</span></span>

<span data-ttu-id="05920-106">El controlador de administración es para los usuarios que tienen privilegios de administrador.</span><span class="sxs-lookup"><span data-stu-id="05920-106">The Admin controller is for users who have administrator privileges.</span></span> <span data-ttu-id="05920-107">Los clientes, por otro lado, pueden ver productos, pero no se puede crear, actualizar o eliminarlos.</span><span class="sxs-lookup"><span data-stu-id="05920-107">Customers, on the other hand, can view products but cannot create, update, or delete them.</span></span>

<span data-ttu-id="05920-108">Se podemos restringir fácilmente el acceso a los métodos Post, Put y Delete, mientras que los métodos Get se deja abierta.</span><span class="sxs-lookup"><span data-stu-id="05920-108">We can easily restrict access to the Post, Put, and Delete methods, while leaving the Get methods open.</span></span> <span data-ttu-id="05920-109">Pero veamos los datos que se devuelven para un producto:</span><span class="sxs-lookup"><span data-stu-id="05920-109">But look at the data that is returned for a product:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

<span data-ttu-id="05920-110">El `ActualCost` propiedad no debe ser visible para otros clientes.</span><span class="sxs-lookup"><span data-stu-id="05920-110">The `ActualCost` property should not be visible to customers!</span></span> <span data-ttu-id="05920-111">La solución consiste en definir un *objeto de transferencia de datos* (DTO) que incluye un subconjunto de propiedades que deben estar visibles para los clientes.</span><span class="sxs-lookup"><span data-stu-id="05920-111">The solution is to define a *data transfer object* (DTO) that includes a subset of properties that should be visible to customers.</span></span> <span data-ttu-id="05920-112">Se va a usar LINQ al proyecto `Product` instancias de `ProductDTO` instancias.</span><span class="sxs-lookup"><span data-stu-id="05920-112">We will use LINQ to project `Product` instances to `ProductDTO` instances.</span></span>

<span data-ttu-id="05920-113">Agregue una clase denominada `ProductDTO` en la carpeta Models.</span><span class="sxs-lookup"><span data-stu-id="05920-113">Add a class named `ProductDTO` to the Models folder.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

<span data-ttu-id="05920-114">Ahora agregue el controlador.</span><span class="sxs-lookup"><span data-stu-id="05920-114">Now add the controller.</span></span> <span data-ttu-id="05920-115">En el Explorador de soluciones, haga clic en la carpeta de controladores.</span><span class="sxs-lookup"><span data-stu-id="05920-115">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="05920-116">Seleccione **agregar**, a continuación, seleccione **controlador**.</span><span class="sxs-lookup"><span data-stu-id="05920-116">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="05920-117">En el **Agregar controlador** cuadro de diálogo, el nombre del controlador &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="05920-117">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="05920-118">En **plantilla**, seleccione **controlador API vacía**.</span><span class="sxs-lookup"><span data-stu-id="05920-118">Under **Template**, select **Empty API controller**.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

<span data-ttu-id="05920-119">Reemplace todo el contenido del archivo de origen con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="05920-119">Replace everything in the source file with the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

<span data-ttu-id="05920-120">El controlador sigue usando el `OrdersContext` para consultar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="05920-120">The controller still uses the `OrdersContext` to query the database.</span></span> <span data-ttu-id="05920-121">Pero en lugar de devolver `Product` instancias directamente, llamamos a `MapProducts` a ellos en proyectos `ProductDTO` instancias:</span><span class="sxs-lookup"><span data-stu-id="05920-121">But instead of returning `Product` instances directly, we call `MapProducts` to project them onto `ProductDTO` instances:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

<span data-ttu-id="05920-122">El `MapProducts` método devuelve un **IQueryable**, por lo que se puede crear el resultado con otros parámetros de consulta.</span><span class="sxs-lookup"><span data-stu-id="05920-122">The `MapProducts` method returns an **IQueryable**, so we can compose the result with other query parameters.</span></span> <span data-ttu-id="05920-123">Puede verlo en el `GetProduct` método, que agrega un **donde** cláusula a la consulta:</span><span class="sxs-lookup"><span data-stu-id="05920-123">You can see this in the `GetProduct` method, which adds a **where** clause to the query:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a><span data-ttu-id="05920-124">Agregar un controlador de pedidos</span><span class="sxs-lookup"><span data-stu-id="05920-124">Add an Orders Controller</span></span>

<span data-ttu-id="05920-125">A continuación, agregue un controlador que permite a los usuarios crear y ver los pedidos.</span><span class="sxs-lookup"><span data-stu-id="05920-125">Next, add a controller that lets users create and view orders.</span></span>

<span data-ttu-id="05920-126">Comenzaremos con otro DTO.</span><span class="sxs-lookup"><span data-stu-id="05920-126">We'll start with another DTO.</span></span> <span data-ttu-id="05920-127">En el Explorador de soluciones, haga clic en la carpeta Models y agregue una clase denominada `OrderDTO` usar la implementación siguiente:</span><span class="sxs-lookup"><span data-stu-id="05920-127">In Solution Explorer, right-click the Models folder and add a class named `OrderDTO` Use the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

<span data-ttu-id="05920-128">Ahora agregue el controlador.</span><span class="sxs-lookup"><span data-stu-id="05920-128">Now add the controller.</span></span> <span data-ttu-id="05920-129">En el Explorador de soluciones, haga clic en la carpeta de controladores.</span><span class="sxs-lookup"><span data-stu-id="05920-129">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="05920-130">Seleccione **agregar**, a continuación, seleccione **controlador**.</span><span class="sxs-lookup"><span data-stu-id="05920-130">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="05920-131">En el **Agregar controlador** cuadro de diálogo, establezca las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="05920-131">In the **Add Controller** dialog, set the following options:</span></span>

- <span data-ttu-id="05920-132">En **nombre del controlador**, escriba "OrdersController".</span><span class="sxs-lookup"><span data-stu-id="05920-132">Under **Controller Name**, enter "OrdersController".</span></span>
- <span data-ttu-id="05920-133">En **plantilla**, seleccione "Controlador de API con acciones de lectura/escritura, mediante Entity Framework".</span><span class="sxs-lookup"><span data-stu-id="05920-133">Under **Template**, select "API controller with read/write actions, using Entity Framework".</span></span>
- <span data-ttu-id="05920-134">En **clase modelo**, seleccione &quot;orden (ProductStore.Models)&quot;.</span><span class="sxs-lookup"><span data-stu-id="05920-134">Under **Model class**, select &quot;Order (ProductStore.Models)&quot;.</span></span>
- <span data-ttu-id="05920-135">En **clase de contexto de datos**, seleccione &quot;OrdersContext (ProductStore.Models)&quot;.</span><span class="sxs-lookup"><span data-stu-id="05920-135">Under **Data context class**, select &quot;OrdersContext (ProductStore.Models)&quot;.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

<span data-ttu-id="05920-136">Haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="05920-136">Click **Add**.</span></span> <span data-ttu-id="05920-137">Esto agrega un archivo denominado OrdersController.cs.</span><span class="sxs-lookup"><span data-stu-id="05920-137">This adds a file named OrdersController.cs.</span></span> <span data-ttu-id="05920-138">A continuación, es necesario modificar la implementación predeterminada del controlador.</span><span class="sxs-lookup"><span data-stu-id="05920-138">Next, we need to modify the default implementation of the controller.</span></span>

<span data-ttu-id="05920-139">En primer lugar, elimine la `PutOrder` y `DeleteOrder` métodos.</span><span class="sxs-lookup"><span data-stu-id="05920-139">First, delete the `PutOrder` and `DeleteOrder` methods.</span></span> <span data-ttu-id="05920-140">En este ejemplo, los clientes no se pueden modificar ni eliminar pedidos existentes.</span><span class="sxs-lookup"><span data-stu-id="05920-140">For this sample, customers cannot modify or delete existing orders.</span></span> <span data-ttu-id="05920-141">En una aplicación real, necesitaría una gran cantidad de lógica de back-end para controlar estos casos.</span><span class="sxs-lookup"><span data-stu-id="05920-141">In a real application, you would need lots of back-end logic to handle these cases.</span></span> <span data-ttu-id="05920-142">(Por ejemplo, se envía el pedido ya?)</span><span class="sxs-lookup"><span data-stu-id="05920-142">(For example, was the order already shipped?)</span></span>

<span data-ttu-id="05920-143">Cambiar el `GetOrders` método para devolver sólo los pedidos que pertenecen al usuario:</span><span class="sxs-lookup"><span data-stu-id="05920-143">Change the `GetOrders` method to return just the orders that belong to the user:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

<span data-ttu-id="05920-144">Cambiar el `GetOrder` método tal como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="05920-144">Change the `GetOrder` method as follows:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

<span data-ttu-id="05920-145">Estos son los cambios que hemos realizado al método:</span><span class="sxs-lookup"><span data-stu-id="05920-145">Here are the changes that we made to the method:</span></span>

- <span data-ttu-id="05920-146">El valor devuelto es un `OrderDTO` instancia, en lugar de un `Order`.</span><span class="sxs-lookup"><span data-stu-id="05920-146">The return value is an `OrderDTO` instance, instead of an `Order`.</span></span>
- <span data-ttu-id="05920-147">Cuando se consulta la base de datos para el pedido, usamos el [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) método capturar relacionado `OrderDetail` y `Product` entidades.</span><span class="sxs-lookup"><span data-stu-id="05920-147">When we query the database for the order, we use the [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) method to fetch the related `OrderDetail` and `Product` entities.</span></span>
- <span data-ttu-id="05920-148">El resultado se recopila mediante el uso de una proyección.</span><span class="sxs-lookup"><span data-stu-id="05920-148">We flatten the result by using a projection.</span></span>

<span data-ttu-id="05920-149">La respuesta HTTP contiene una matriz de productos con cantidades:</span><span class="sxs-lookup"><span data-stu-id="05920-149">The HTTP response will contain an array of products with quantities:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

<span data-ttu-id="05920-150">Este formato es más fácil para los clientes consumir que el gráfico de objetos original, que contiene entidades anidadas (orden, los detalles y productos).</span><span class="sxs-lookup"><span data-stu-id="05920-150">This format is easier for clients to consume than the original object graph, which contains nested entities (order, details, and products).</span></span>

<span data-ttu-id="05920-151">El último método para tener en cuenta `PostOrder`.</span><span class="sxs-lookup"><span data-stu-id="05920-151">The last method to consider it `PostOrder`.</span></span> <span data-ttu-id="05920-152">En este momento, este método toma un `Order` instancia.</span><span class="sxs-lookup"><span data-stu-id="05920-152">Right now, this method takes an `Order` instance.</span></span> <span data-ttu-id="05920-153">Pero tenga en cuenta lo que sucede si un cliente envía un cuerpo de solicitud similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="05920-153">But consider what happens if a client sends a request body like this:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

<span data-ttu-id="05920-154">Se trata de un orden bien estructurado y Entity Framework, inserta, en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="05920-154">This is a well-structured order, and Entity Framework will happily insert it into the database.</span></span> <span data-ttu-id="05920-155">Pero contiene una entidad de producto que no existía anteriormente.</span><span class="sxs-lookup"><span data-stu-id="05920-155">But it contains a Product entity that did not exist previously.</span></span> <span data-ttu-id="05920-156">El cliente que acaba de crear un nuevo producto en nuestra base de datos.</span><span class="sxs-lookup"><span data-stu-id="05920-156">The client just created a new product in our database!</span></span> <span data-ttu-id="05920-157">Se trata una sorpresa al departamento de fullfilment orden, cuando vean un pedido para osos koala.</span><span class="sxs-lookup"><span data-stu-id="05920-157">This will be a suprise to the order fullfilment department, when they see an order for koala bears.</span></span> <span data-ttu-id="05920-158">Es la moraleja, tenga cuidado realmente los datos que se aceptan en una solicitud POST o PUT.</span><span class="sxs-lookup"><span data-stu-id="05920-158">The moral is, be really careful about the data you accept in a POST or PUT request.</span></span>

<span data-ttu-id="05920-159">Para evitar este problema, cambie la `PostOrder` método tomar un `OrderDTO` instancia.</span><span class="sxs-lookup"><span data-stu-id="05920-159">To avoid this problem, change the `PostOrder` method to take an `OrderDTO` instance.</span></span> <span data-ttu-id="05920-160">Use la `OrderDTO` para crear la `Order`.</span><span class="sxs-lookup"><span data-stu-id="05920-160">Use the `OrderDTO` to create the `Order`.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

<span data-ttu-id="05920-161">Observe que utilizamos la `ProductID` y `Quantity` propiedades y se omiten los valores que el cliente envía para su nombre de producto o precio.</span><span class="sxs-lookup"><span data-stu-id="05920-161">Notice that we use the `ProductID` and `Quantity` properties, and we ignore any values that the client sent for either product name or price.</span></span> <span data-ttu-id="05920-162">Si el identificador de producto no es válido, infringirán la restricción foreign key en la base de datos y se producirá un error la inserción, como debería.</span><span class="sxs-lookup"><span data-stu-id="05920-162">If the product ID is not valid, it will violate the foreign key constraint in the database, and the insert will fail, as it should.</span></span>

<span data-ttu-id="05920-163">Aquí es el conjunto completo `PostOrder` método:</span><span class="sxs-lookup"><span data-stu-id="05920-163">Here is the complete `PostOrder` method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

<span data-ttu-id="05920-164">Por último, agregue el **Authorize** atributo al controlador:</span><span class="sxs-lookup"><span data-stu-id="05920-164">Finally, add the **Authorize** attribute to the controller:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

<span data-ttu-id="05920-165">Ahora sólo los usuarios registrados pueden crear o ver los pedidos.</span><span class="sxs-lookup"><span data-stu-id="05920-165">Now only registered users can create or view orders.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="05920-166">[Anterior](using-web-api-with-entity-framework-part-5.md)
> [Siguiente](using-web-api-with-entity-framework-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="05920-166">[Previous](using-web-api-with-entity-framework-part-5.md)
[Next](using-web-api-with-entity-framework-part-7.md)</span></span>
