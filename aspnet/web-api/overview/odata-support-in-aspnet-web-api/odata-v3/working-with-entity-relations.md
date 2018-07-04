---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Compatibilidad con las relaciones de entidad en OData v3 con Web API 2 | Microsoft Docs
author: MikeWasson
description: 'La mayoría de los conjuntos de datos definen las relaciones entre entidades: los clientes tienen pedidos; los libros en pantalla tienen autores; los productos tienen proveedores. Uso de OData, los clientes pueden navegar a través de...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: 311e84a2beb3ec7661fd650b277f23458bcb0cb2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377482"
---
<a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a><span data-ttu-id="a2989-104">Compatibilidad con las relaciones de entidad en OData v3 con Web API 2</span><span class="sxs-lookup"><span data-stu-id="a2989-104">Supporting Entity Relations in OData v3 with Web API 2</span></span>
====================
<span data-ttu-id="a2989-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a2989-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="a2989-106">Descargue el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="a2989-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="a2989-107">La mayoría de los conjuntos de datos definen las relaciones entre entidades: los clientes tienen pedidos; los libros en pantalla tienen autores; los productos tienen proveedores.</span><span class="sxs-lookup"><span data-stu-id="a2989-107">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="a2989-108">Con OData, los clientes pueden navegar por las relaciones de entidad.</span><span class="sxs-lookup"><span data-stu-id="a2989-108">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="a2989-109">Dado un producto, puede encontrar el proveedor.</span><span class="sxs-lookup"><span data-stu-id="a2989-109">Given a product, you can find the supplier.</span></span> <span data-ttu-id="a2989-110">También puede crear o eliminar las relaciones.</span><span class="sxs-lookup"><span data-stu-id="a2989-110">You can also create or remove relationships.</span></span> <span data-ttu-id="a2989-111">Por ejemplo, puede establecer el proveedor de un producto.</span><span class="sxs-lookup"><span data-stu-id="a2989-111">For example, you can set the supplier for a product.</span></span>
> 
> <span data-ttu-id="a2989-112">Este tutorial muestra cómo admitir estas operaciones en ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="a2989-112">This tutorial shows how to support these operations in ASP.NET Web API.</span></span> <span data-ttu-id="a2989-113">El tutorial se basa en el tutorial [crear un punto de conexión de OData v3 con Web API 2](creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="a2989-113">The tutorial builds on the tutorial [Creating an OData v3 Endpoint with Web API 2](creating-an-odata-endpoint.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a2989-114">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="a2989-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="a2989-115">Web API 2</span><span class="sxs-lookup"><span data-stu-id="a2989-115">Web API 2</span></span>
> - <span data-ttu-id="a2989-116">OData versión 3</span><span class="sxs-lookup"><span data-stu-id="a2989-116">OData Version 3</span></span>
> - <span data-ttu-id="a2989-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="a2989-117">Entity Framework 6</span></span>


## <a name="add-a-supplier-entity"></a><span data-ttu-id="a2989-118">Agregar una entidad de proveedor</span><span class="sxs-lookup"><span data-stu-id="a2989-118">Add a Supplier Entity</span></span>

<span data-ttu-id="a2989-119">En primer lugar, necesitamos agregar un nuevo tipo de entidad a nuestra fuente de OData.</span><span class="sxs-lookup"><span data-stu-id="a2989-119">First we need to add a new entity type to our OData feed.</span></span> <span data-ttu-id="a2989-120">Vamos a agregar un `Supplier` clase.</span><span class="sxs-lookup"><span data-stu-id="a2989-120">We'll add a `Supplier` class.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

<span data-ttu-id="a2989-121">Esta clase utiliza una cadena para la clave de entidad.</span><span class="sxs-lookup"><span data-stu-id="a2989-121">This class uses a string for the entity key.</span></span> <span data-ttu-id="a2989-122">En la práctica, que podría ser menos frecuentes que usar una clave de tipo entero.</span><span class="sxs-lookup"><span data-stu-id="a2989-122">In practice, that might be less common than using an integer key.</span></span> <span data-ttu-id="a2989-123">Pero merece la pena viendo cómo OData controla otros tipos de clave además de enteros.</span><span class="sxs-lookup"><span data-stu-id="a2989-123">But it's worth seeing how OData handles other key types besides integers.</span></span>

<span data-ttu-id="a2989-124">A continuación, vamos a crear una relación mediante la adición de un `Supplier` propiedad a la `Product` clase:</span><span class="sxs-lookup"><span data-stu-id="a2989-124">Next, we'll create a relation by adding a `Supplier` property to the `Product` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

<span data-ttu-id="a2989-125">Agregue un nuevo **DbSet** a la `ProductServiceContext` clase, para que Entity Framework incluirá el `Supplier` tabla en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a2989-125">Add a new **DbSet** to the `ProductServiceContext` class, so that Entity Framework will include the `Supplier` table in the database.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

<span data-ttu-id="a2989-126">En WebApiConfig.cs, agregue una entidad "Proveedores" para el modelo EDM:</span><span class="sxs-lookup"><span data-stu-id="a2989-126">In WebApiConfig.cs, add a "Suppliers" entity to the EDM model:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a><span data-ttu-id="a2989-127">Propiedades de navegación</span><span class="sxs-lookup"><span data-stu-id="a2989-127">Navigation Properties</span></span>

<span data-ttu-id="a2989-128">Para obtener el proveedor de un producto, el cliente envía una solicitud GET:</span><span class="sxs-lookup"><span data-stu-id="a2989-128">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

<span data-ttu-id="a2989-129">Aquí "Proveedor" es una propiedad de navegación en el `Product` tipo.</span><span class="sxs-lookup"><span data-stu-id="a2989-129">Here "Supplier" is a navigation property on the `Product` type.</span></span> <span data-ttu-id="a2989-130">En este caso, `Supplier` hace referencia a un único elemento, pero una navegación propiedad también puede devolver una colección (relación de uno a varios o varios a varios).</span><span class="sxs-lookup"><span data-stu-id="a2989-130">In this case, `Supplier` refers to a single item, but a navigation property can also return a collection (one-to-many or many-to-many relation).</span></span>

<span data-ttu-id="a2989-131">Para admitir esta solicitud, agregue el método siguiente a la `ProductsController` clase:</span><span class="sxs-lookup"><span data-stu-id="a2989-131">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

<span data-ttu-id="a2989-132">El *clave* parámetro es la clave del producto.</span><span class="sxs-lookup"><span data-stu-id="a2989-132">The *key* parameter is the key of the product.</span></span> <span data-ttu-id="a2989-133">El método devuelve la entidad relacionada & #8212 en este caso, un `Supplier` instancia.</span><span class="sxs-lookup"><span data-stu-id="a2989-133">The method returns the related entity&#8212in this case, a `Supplier` instance.</span></span> <span data-ttu-id="a2989-134">El nombre del método y el nombre de parámetro son importantes.</span><span class="sxs-lookup"><span data-stu-id="a2989-134">The method name and parameter name are both important.</span></span> <span data-ttu-id="a2989-135">En general, si la propiedad de navegación se denomina "X", deberá agregar un método denominado "GetX".</span><span class="sxs-lookup"><span data-stu-id="a2989-135">In general, if the navigation property is named "X", you need to add a method named "GetX".</span></span> <span data-ttu-id="a2989-136">El método debe tomar un parámetro denominado "*clave*" que coincida con el tipo de datos de clave del elemento primario.</span><span class="sxs-lookup"><span data-stu-id="a2989-136">The method must take a parameter named "*key*" that matches the data type of the parent's key.</span></span>

<span data-ttu-id="a2989-137">También es importante incluir la **[FromOdataUri]** atributo en el *clave* parámetro.</span><span class="sxs-lookup"><span data-stu-id="a2989-137">It is also important to include the **[FromOdataUri]** attribute in the *key* parameter.</span></span> <span data-ttu-id="a2989-138">Este atributo indica a Web API para usar las reglas de sintaxis de OData cuando analiza la clave desde el URI de solicitud.</span><span class="sxs-lookup"><span data-stu-id="a2989-138">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

## <a name="creating-and-deleting-links"></a><span data-ttu-id="a2989-139">Creación y eliminación de vínculos</span><span class="sxs-lookup"><span data-stu-id="a2989-139">Creating and Deleting Links</span></span>

<span data-ttu-id="a2989-140">OData admite la creación o eliminación de las relaciones entre dos entidades.</span><span class="sxs-lookup"><span data-stu-id="a2989-140">OData supports creating or removing relationships between two entities.</span></span> <span data-ttu-id="a2989-141">En la terminología de OData, la relación es un "vínculo".</span><span class="sxs-lookup"><span data-stu-id="a2989-141">In OData terminology, the relationship is a "link."</span></span> <span data-ttu-id="a2989-142">Cada vínculo tiene un URI con el formulario *entidad*/$links /*entidad*.</span><span class="sxs-lookup"><span data-stu-id="a2989-142">Each link has a URI with the form *entity*/$links/*entity*.</span></span> <span data-ttu-id="a2989-143">Por ejemplo, el vínculo del producto a proveedor tiene este aspecto:</span><span class="sxs-lookup"><span data-stu-id="a2989-143">For example, the link from product to supplier looks like this:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

<span data-ttu-id="a2989-144">Para crear un nuevo vínculo, el cliente envía una solicitud POST al URI de vínculo.</span><span class="sxs-lookup"><span data-stu-id="a2989-144">To create a new link, the client sends a POST request to the link URI.</span></span> <span data-ttu-id="a2989-145">El cuerpo de la solicitud es el URI de la entidad de destino.</span><span class="sxs-lookup"><span data-stu-id="a2989-145">The body of the request is the URI of the target entity.</span></span> <span data-ttu-id="a2989-146">Por ejemplo, suponga que hay un proveedor con la clave "CTSO".</span><span class="sxs-lookup"><span data-stu-id="a2989-146">For example, suppose there is a supplier with the key "CTSO".</span></span> <span data-ttu-id="a2989-147">Para crear un vínculo de "Product(1)" a "Supplier('CTSO')", el cliente envía una solicitud similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="a2989-147">To create a link from "Product(1)" to "Supplier('CTSO')", the client sends a request like the following:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

<span data-ttu-id="a2989-148">Para eliminar un vínculo, el cliente envía una solicitud DELETE al URI de vínculo.</span><span class="sxs-lookup"><span data-stu-id="a2989-148">To delete a link, the client sends a DELETE request to the link URI.</span></span>

<span data-ttu-id="a2989-149">**Creación de vínculos**</span><span class="sxs-lookup"><span data-stu-id="a2989-149">**Creating Links**</span></span>

<span data-ttu-id="a2989-150">Para habilitar un cliente crear vínculos de proveedor del producto, agregue el código siguiente a la `ProductsController` clase:</span><span class="sxs-lookup"><span data-stu-id="a2989-150">To enable a client to create product-supplier links, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

<span data-ttu-id="a2989-151">Este método toma tres parámetros:</span><span class="sxs-lookup"><span data-stu-id="a2989-151">This method takes three parameters:</span></span>

- <span data-ttu-id="a2989-152">*clave*: la clave para la entidad primaria (el producto)</span><span class="sxs-lookup"><span data-stu-id="a2989-152">*key*: The key to the parent entity (the product)</span></span>
- <span data-ttu-id="a2989-153">*navigationProperty*: el nombre de la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="a2989-153">*navigationProperty*: The name of the navigation property.</span></span> <span data-ttu-id="a2989-154">En este ejemplo, la propiedad de navegación solo es válido es "Proveedor".</span><span class="sxs-lookup"><span data-stu-id="a2989-154">In this example, the only valid navigation property is "Supplier".</span></span>
- <span data-ttu-id="a2989-155">*vínculo*: el URI de OData de la entidad relacionada.</span><span class="sxs-lookup"><span data-stu-id="a2989-155">*link*: The OData URI of the related entity.</span></span> <span data-ttu-id="a2989-156">Este valor se toma del cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="a2989-156">This value is taken from the request body.</span></span> <span data-ttu-id="a2989-157">Por ejemplo, el vínculo que URI podría ser "`http://localhost/odata/Suppliers('CTSO')`, lo que significa que el proveedor con el Id. = 'CTSO'.</span><span class="sxs-lookup"><span data-stu-id="a2989-157">For example, the link URI might be "`http://localhost/odata/Suppliers('CTSO')`, meaning the supplier with ID = ‘CTSO'.</span></span>

<span data-ttu-id="a2989-158">El método utiliza el vínculo para buscar el proveedor.</span><span class="sxs-lookup"><span data-stu-id="a2989-158">The method uses the link to look up the supplier.</span></span> <span data-ttu-id="a2989-159">Si se encuentra el proveedor de búsqueda de coincidencias, el método establece el `Product.Supplier` propiedad y guarda el resultado en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a2989-159">If the matching supplier is found, the method sets the `Product.Supplier` property and saves the result to the database.</span></span>

<span data-ttu-id="a2989-160">La parte más difícil es analizar el URI del vínculo.</span><span class="sxs-lookup"><span data-stu-id="a2989-160">The hardest part is parsing the link URI.</span></span> <span data-ttu-id="a2989-161">Básicamente, deberá simular el resultado de enviar una solicitud GET a ese URI.</span><span class="sxs-lookup"><span data-stu-id="a2989-161">Basically, you need to simulate the result of sending a GET request to that URI.</span></span> <span data-ttu-id="a2989-162">El método auxiliar siguiente muestra cómo hacerlo.</span><span class="sxs-lookup"><span data-stu-id="a2989-162">The following helper method shows how to do this.</span></span> <span data-ttu-id="a2989-163">El método invoca el proceso de enrutamiento de Web API y recibe un **ODataPath** instancia que representa la ruta de acceso de OData analizado.</span><span class="sxs-lookup"><span data-stu-id="a2989-163">The method invokes the Web API routing process and gets back an **ODataPath** instance that represents the parsed OData path.</span></span> <span data-ttu-id="a2989-164">Para un URI de vínculo, uno de los segmentos debe ser la clave de entidad.</span><span class="sxs-lookup"><span data-stu-id="a2989-164">For a link URI, one of the segments should be the entity key.</span></span> <span data-ttu-id="a2989-165">(Si no, el cliente envió un URI incorrecto.)</span><span class="sxs-lookup"><span data-stu-id="a2989-165">(If not, the client sent a bad URI.)</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

<span data-ttu-id="a2989-166">**Eliminar vínculos**</span><span class="sxs-lookup"><span data-stu-id="a2989-166">**Deleting Links**</span></span>

<span data-ttu-id="a2989-167">Para eliminar un vínculo, agregue el código siguiente a la `ProductsController` clase:</span><span class="sxs-lookup"><span data-stu-id="a2989-167">To delete a link, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

<span data-ttu-id="a2989-168">En este ejemplo, la propiedad de navegación es una sola `Supplier` entidad.</span><span class="sxs-lookup"><span data-stu-id="a2989-168">In this example, the navigation property is a single `Supplier` entity.</span></span> <span data-ttu-id="a2989-169">Si la propiedad de navegación es una colección, el URI para eliminar un vínculo debe incluir una clave para la entidad relacionada.</span><span class="sxs-lookup"><span data-stu-id="a2989-169">If the navigation property is a collection, the URI to delete a link must include a key for the related entity.</span></span> <span data-ttu-id="a2989-170">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="a2989-170">For example:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

<span data-ttu-id="a2989-171">Esta solicitud quita el pedido 1 cliente 1.</span><span class="sxs-lookup"><span data-stu-id="a2989-171">This request removes order 1 from customer 1.</span></span> <span data-ttu-id="a2989-172">En este caso, el método DeleteLink tendrá la siguiente firma:</span><span class="sxs-lookup"><span data-stu-id="a2989-172">In this case, the DeleteLink method will have the following signature:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

<span data-ttu-id="a2989-173">El *relatedKey* parámetro proporciona la clave para la entidad relacionada.</span><span class="sxs-lookup"><span data-stu-id="a2989-173">The *relatedKey* parameter gives the key for the related entity.</span></span> <span data-ttu-id="a2989-174">Por lo tanto en su `DeleteLink` método, buscar la entidad principal mediante el *clave* parámetro, busque la entidad relacionada por la *relatedKey* parámetro y, a continuación, quite la asociación.</span><span class="sxs-lookup"><span data-stu-id="a2989-174">So in your `DeleteLink` method, look up the primary entity by the *key* parameter, find the related entity by the *relatedKey* parameter, and then remove the association.</span></span> <span data-ttu-id="a2989-175">Dependiendo del modelo de datos, es posible que deba implementar ambas versiones de `DeleteLink`.</span><span class="sxs-lookup"><span data-stu-id="a2989-175">Depending on your data model, you might need to implement both versions of `DeleteLink`.</span></span> <span data-ttu-id="a2989-176">API Web llamará a la versión correcta según el URI de solicitud.</span><span class="sxs-lookup"><span data-stu-id="a2989-176">Web API will call the correct version based on the request URI.</span></span>
