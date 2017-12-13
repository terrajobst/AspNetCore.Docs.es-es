---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Compatibilidad con las relaciones de entidad en OData v3 con Web API 2 | Documentos de Microsoft
author: MikeWasson
description: "La mayoría de los conjuntos de datos definen las relaciones entre entidades: los clientes tienen pedidos; los libros tienen autores; los productos tienen proveedores. Con OData, los clientes pueden navegar por..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: dec7e10e59cc2441c967afe062df227b105106a1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a><span data-ttu-id="3d294-104">Compatibilidad con las relaciones de entidad en OData v3 con Web API 2</span><span class="sxs-lookup"><span data-stu-id="3d294-104">Supporting Entity Relations in OData v3 with Web API 2</span></span>
====================
<span data-ttu-id="3d294-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3d294-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="3d294-106">Descargar el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="3d294-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="3d294-107">La mayoría de los conjuntos de datos definen las relaciones entre entidades: los clientes tienen pedidos; los libros tienen autores; los productos tienen proveedores.</span><span class="sxs-lookup"><span data-stu-id="3d294-107">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="3d294-108">Con OData, los clientes pueden navegar por relaciones de entidad.</span><span class="sxs-lookup"><span data-stu-id="3d294-108">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="3d294-109">Dado un producto, puede encontrar el proveedor.</span><span class="sxs-lookup"><span data-stu-id="3d294-109">Given a product, you can find the supplier.</span></span> <span data-ttu-id="3d294-110">También puede crear o eliminar las relaciones.</span><span class="sxs-lookup"><span data-stu-id="3d294-110">You can also create or remove relationships.</span></span> <span data-ttu-id="3d294-111">Por ejemplo, puede establecer el proveedor de un producto.</span><span class="sxs-lookup"><span data-stu-id="3d294-111">For example, you can set the supplier for a product.</span></span>
> 
> <span data-ttu-id="3d294-112">Este tutorial muestra cómo admitir estas operaciones en ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="3d294-112">This tutorial shows how to support these operations in ASP.NET Web API.</span></span> <span data-ttu-id="3d294-113">El tutorial se basa en el tutorial [crear un punto de conexión de OData v3 con API Web 2](creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="3d294-113">The tutorial builds on the tutorial [Creating an OData v3 Endpoint with Web API 2](creating-an-odata-endpoint.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="3d294-114">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="3d294-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="3d294-115">Web API 2</span><span class="sxs-lookup"><span data-stu-id="3d294-115">Web API 2</span></span>
> - <span data-ttu-id="3d294-116">Versión 3 de OData</span><span class="sxs-lookup"><span data-stu-id="3d294-116">OData Version 3</span></span>
> - <span data-ttu-id="3d294-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="3d294-117">Entity Framework 6</span></span>


## <a name="add-a-supplier-entity"></a><span data-ttu-id="3d294-118">Agregar una entidad de proveedor</span><span class="sxs-lookup"><span data-stu-id="3d294-118">Add a Supplier Entity</span></span>

<span data-ttu-id="3d294-119">Primero se debe agregar un nuevo tipo de entidad a nuestra fuente de OData.</span><span class="sxs-lookup"><span data-stu-id="3d294-119">First we need to add a new entity type to our OData feed.</span></span> <span data-ttu-id="3d294-120">Vamos a agregar un `Supplier` clase.</span><span class="sxs-lookup"><span data-stu-id="3d294-120">We'll add a `Supplier` class.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

<span data-ttu-id="3d294-121">Esta clase utiliza una cadena para la clave de entidad.</span><span class="sxs-lookup"><span data-stu-id="3d294-121">This class uses a string for the entity key.</span></span> <span data-ttu-id="3d294-122">En la práctica, que podría ser menos común que usar una clave de tipo entero.</span><span class="sxs-lookup"><span data-stu-id="3d294-122">In practice, that might be less common than using an integer key.</span></span> <span data-ttu-id="3d294-123">Pero vale la pena ver cómo OData controla otros tipos de clave además de números enteros.</span><span class="sxs-lookup"><span data-stu-id="3d294-123">But it's worth seeing how OData handles other key types besides integers.</span></span>

<span data-ttu-id="3d294-124">A continuación, vamos a crear una relación mediante la adición de un `Supplier` propiedad a la `Product` clase:</span><span class="sxs-lookup"><span data-stu-id="3d294-124">Next, we'll create a relation by adding a `Supplier` property to the `Product` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

<span data-ttu-id="3d294-125">Agregue un nuevo **DbSet** a la `ProductServiceContext` de la clase, lo que Entity Framework incluirá el `Supplier` tabla en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3d294-125">Add a new **DbSet** to the `ProductServiceContext` class, so that Entity Framework will include the `Supplier` table in the database.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

<span data-ttu-id="3d294-126">En WebApiConfig.cs, agregar una entidad de "Proveedores" en el modelo EDM:</span><span class="sxs-lookup"><span data-stu-id="3d294-126">In WebApiConfig.cs, add a "Suppliers" entity to the EDM model:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a><span data-ttu-id="3d294-127">Propiedades de navegación</span><span class="sxs-lookup"><span data-stu-id="3d294-127">Navigation Properties</span></span>

<span data-ttu-id="3d294-128">Para obtener el proveedor de un producto, el cliente envía una solicitud GET:</span><span class="sxs-lookup"><span data-stu-id="3d294-128">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

<span data-ttu-id="3d294-129">Aquí "Proveedor" es una propiedad de navegación en la `Product` tipo.</span><span class="sxs-lookup"><span data-stu-id="3d294-129">Here "Supplier" is a navigation property on the `Product` type.</span></span> <span data-ttu-id="3d294-130">En este caso, `Supplier` hace referencia a un único elemento, pero una navegación propiedad también puede devolver una colección (relación de uno a varios o varios a varios).</span><span class="sxs-lookup"><span data-stu-id="3d294-130">In this case, `Supplier` refers to a single item, but a navigation property can also return a collection (one-to-many or many-to-many relation).</span></span>

<span data-ttu-id="3d294-131">Para admitir esta solicitud, agregue el método siguiente a la `ProductsController` clase:</span><span class="sxs-lookup"><span data-stu-id="3d294-131">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

<span data-ttu-id="3d294-132">El *clave* parámetro es la clave del producto.</span><span class="sxs-lookup"><span data-stu-id="3d294-132">The *key* parameter is the key of the product.</span></span> <span data-ttu-id="3d294-133">El método devuelve la entidad relacionada &#8212;en este caso, un `Supplier` instancia.</span><span class="sxs-lookup"><span data-stu-id="3d294-133">The method returns the related entity&#8212in this case, a `Supplier` instance.</span></span> <span data-ttu-id="3d294-134">El nombre del método y el nombre de parámetro son importantes.</span><span class="sxs-lookup"><span data-stu-id="3d294-134">The method name and parameter name are both important.</span></span> <span data-ttu-id="3d294-135">En general, si la propiedad de navegación se denomina "X", debe agregar un método denominado "GetX".</span><span class="sxs-lookup"><span data-stu-id="3d294-135">In general, if the navigation property is named "X", you need to add a method named "GetX".</span></span> <span data-ttu-id="3d294-136">El método debe tomar un parámetro denominado "*clave*" que coincide con el tipo de datos de clave del elemento primario.</span><span class="sxs-lookup"><span data-stu-id="3d294-136">The method must take a parameter named "*key*" that matches the data type of the parent's key.</span></span>

<span data-ttu-id="3d294-137">También es importante incluir la **[FromOdataUri]** de atributo en el *clave* parámetro.</span><span class="sxs-lookup"><span data-stu-id="3d294-137">It is also important to include the **[FromOdataUri]** attribute in the *key* parameter.</span></span> <span data-ttu-id="3d294-138">Este atributo indica a API Web para usar las reglas de sintaxis de OData cuando analiza la clave de URI de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="3d294-138">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

## <a name="creating-and-deleting-links"></a><span data-ttu-id="3d294-139">Crear y eliminar vínculos</span><span class="sxs-lookup"><span data-stu-id="3d294-139">Creating and Deleting Links</span></span>

<span data-ttu-id="3d294-140">OData admite la creación o eliminación de las relaciones entre dos entidades.</span><span class="sxs-lookup"><span data-stu-id="3d294-140">OData supports creating or removing relationships between two entities.</span></span> <span data-ttu-id="3d294-141">En la terminología de OData, la relación es un "enlace".</span><span class="sxs-lookup"><span data-stu-id="3d294-141">In OData terminology, the relationship is a "link."</span></span> <span data-ttu-id="3d294-142">Cada vínculo tiene un URI con el formulario *entidad*/$links /*entidad*.</span><span class="sxs-lookup"><span data-stu-id="3d294-142">Each link has a URI with the form *entity*/$links/*entity*.</span></span> <span data-ttu-id="3d294-143">Por ejemplo, el vínculo de producto al proveedor tiene este aspecto:</span><span class="sxs-lookup"><span data-stu-id="3d294-143">For example, the link from product to supplier looks like this:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

<span data-ttu-id="3d294-144">Para crear un nuevo vínculo, el cliente envía una solicitud POST para el identificador URI del vínculo.</span><span class="sxs-lookup"><span data-stu-id="3d294-144">To create a new link, the client sends a POST request to the link URI.</span></span> <span data-ttu-id="3d294-145">El cuerpo de la solicitud es el URI de la entidad de destino.</span><span class="sxs-lookup"><span data-stu-id="3d294-145">The body of the request is the URI of the target entity.</span></span> <span data-ttu-id="3d294-146">Por ejemplo, suponga que hay un proveedor con la clave "CTSO".</span><span class="sxs-lookup"><span data-stu-id="3d294-146">For example, suppose there is a supplier with the key "CTSO".</span></span> <span data-ttu-id="3d294-147">Para crear un vínculo de "Product(1)" a "Supplier('CTSO')", el cliente envía una solicitud similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="3d294-147">To create a link from "Product(1)" to "Supplier('CTSO')", the client sends a request like the following:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

<span data-ttu-id="3d294-148">Para eliminar un vínculo, el cliente envía una solicitud de eliminación para el identificador URI del vínculo.</span><span class="sxs-lookup"><span data-stu-id="3d294-148">To delete a link, the client sends a DELETE request to the link URI.</span></span>

<span data-ttu-id="3d294-149">**Crear vínculos**</span><span class="sxs-lookup"><span data-stu-id="3d294-149">**Creating Links**</span></span>

<span data-ttu-id="3d294-150">Para habilitar al cliente para crear vínculos de proveedor del producto, agregue el código siguiente a la `ProductsController` clase:</span><span class="sxs-lookup"><span data-stu-id="3d294-150">To enable a client to create product-supplier links, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

<span data-ttu-id="3d294-151">Este método toma tres parámetros:</span><span class="sxs-lookup"><span data-stu-id="3d294-151">This method takes three parameters:</span></span>

- <span data-ttu-id="3d294-152">*clave*: la clave para la entidad primaria (el producto)</span><span class="sxs-lookup"><span data-stu-id="3d294-152">*key*: The key to the parent entity (the product)</span></span>
- <span data-ttu-id="3d294-153">*navigationProperty*: el nombre de la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="3d294-153">*navigationProperty*: The name of the navigation property.</span></span> <span data-ttu-id="3d294-154">En este ejemplo, la propiedad de navegación solo es válido es "Proveedor".</span><span class="sxs-lookup"><span data-stu-id="3d294-154">In this example, the only valid navigation property is "Supplier".</span></span>
- <span data-ttu-id="3d294-155">*vínculo*: el URI de OData de la entidad relacionada.</span><span class="sxs-lookup"><span data-stu-id="3d294-155">*link*: The OData URI of the related entity.</span></span> <span data-ttu-id="3d294-156">Este valor se toma del cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="3d294-156">This value is taken from the request body.</span></span> <span data-ttu-id="3d294-157">Por ejemplo, podría ser los URI del vínculo "`http://localhost/odata/Suppliers('CTSO')`, lo que significa que el proveedor con el Id. = 'CTSO'.</span><span class="sxs-lookup"><span data-stu-id="3d294-157">For example, the link URI might be "`http://localhost/odata/Suppliers('CTSO')`, meaning the supplier with ID = ‘CTSO'.</span></span>

<span data-ttu-id="3d294-158">El método utiliza el vínculo para buscar el proveedor.</span><span class="sxs-lookup"><span data-stu-id="3d294-158">The method uses the link to look up the supplier.</span></span> <span data-ttu-id="3d294-159">Si se encuentra el proveedor de búsqueda de coincidencias, el método establece la `Product.Supplier` propiedad y guarda el resultado en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3d294-159">If the matching supplier is found, the method sets the `Product.Supplier` property and saves the result to the database.</span></span>

<span data-ttu-id="3d294-160">La parte más difícil es analizar el identificador URI del vínculo.</span><span class="sxs-lookup"><span data-stu-id="3d294-160">The hardest part is parsing the link URI.</span></span> <span data-ttu-id="3d294-161">Básicamente, se necesita simular el resultado de enviar una solicitud GET a ese URI.</span><span class="sxs-lookup"><span data-stu-id="3d294-161">Basically, you need to simulate the result of sending a GET request to that URI.</span></span> <span data-ttu-id="3d294-162">El método auxiliar siguiente muestra cómo hacerlo.</span><span class="sxs-lookup"><span data-stu-id="3d294-162">The following helper method shows how to do this.</span></span> <span data-ttu-id="3d294-163">El método se invoca el proceso de enrutamiento de Web API y vuelve una **ODataPath** instancia que representa la ruta de acceso de OData analizado.</span><span class="sxs-lookup"><span data-stu-id="3d294-163">The method invokes the Web API routing process and gets back an **ODataPath** instance that represents the parsed OData path.</span></span> <span data-ttu-id="3d294-164">Para un identificador URI del vínculo, uno de los segmentos debe ser la clave de entidad.</span><span class="sxs-lookup"><span data-stu-id="3d294-164">For a link URI, one of the segments should be the entity key.</span></span> <span data-ttu-id="3d294-165">(Si no, el cliente envió un URI incorrecto.)</span><span class="sxs-lookup"><span data-stu-id="3d294-165">(If not, the client sent a bad URI.)</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

<span data-ttu-id="3d294-166">**Eliminar vínculos**</span><span class="sxs-lookup"><span data-stu-id="3d294-166">**Deleting Links**</span></span>

<span data-ttu-id="3d294-167">Para eliminar un vínculo, agregue el código siguiente a la `ProductsController` clase:</span><span class="sxs-lookup"><span data-stu-id="3d294-167">To delete a link, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

<span data-ttu-id="3d294-168">En este ejemplo, la propiedad de navegación es una sola `Supplier` entidad.</span><span class="sxs-lookup"><span data-stu-id="3d294-168">In this example, the navigation property is a single `Supplier` entity.</span></span> <span data-ttu-id="3d294-169">Si la propiedad de navegación es una colección, el URI para eliminar un vínculo debe incluir una clave para la entidad relacionada.</span><span class="sxs-lookup"><span data-stu-id="3d294-169">If the navigation property is a collection, the URI to delete a link must include a key for the related entity.</span></span> <span data-ttu-id="3d294-170">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="3d294-170">For example:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

<span data-ttu-id="3d294-171">Esta solicitud quita el orden 1 de cliente 1.</span><span class="sxs-lookup"><span data-stu-id="3d294-171">This request removes order 1 from customer 1.</span></span> <span data-ttu-id="3d294-172">En este caso, el método DeleteLink tendrá la siguiente firma:</span><span class="sxs-lookup"><span data-stu-id="3d294-172">In this case, the DeleteLink method will have the following signature:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

<span data-ttu-id="3d294-173">El *relatedKey* parámetro proporciona la clave para la entidad relacionada.</span><span class="sxs-lookup"><span data-stu-id="3d294-173">The *relatedKey* parameter gives the key for the related entity.</span></span> <span data-ttu-id="3d294-174">Por lo tanto en el `DeleteLink` método, buscar la entidad principal mediante la *clave* parámetro, buscar la entidad relacionada con el *relatedKey* parámetro y, después, quite la asociación.</span><span class="sxs-lookup"><span data-stu-id="3d294-174">So in your `DeleteLink` method, look up the primary entity by the *key* parameter, find the related entity by the *relatedKey* parameter, and then remove the association.</span></span> <span data-ttu-id="3d294-175">Dependiendo del modelo de datos, tendrá que implementar ambas versiones de `DeleteLink`.</span><span class="sxs-lookup"><span data-stu-id="3d294-175">Depending on your data model, you might need to implement both versions of `DeleteLink`.</span></span> <span data-ttu-id="3d294-176">API Web llamará a la versión correcta según el URI de solicitud.</span><span class="sxs-lookup"><span data-stu-id="3d294-176">Web API will call the correct version based on the request URI.</span></span>
