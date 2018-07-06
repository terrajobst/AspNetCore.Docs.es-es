---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: Las relaciones de entidad en OData v4 con ASP.NET Web API 2.2 | Microsoft Docs
author: MikeWasson
description: 'La mayoría de los conjuntos de datos definen las relaciones entre entidades: los clientes tienen pedidos; los libros en pantalla tienen autores; los productos tienen proveedores. Uso de OData, los clientes pueden navegar a través de...'
ms.author: aspnetcontent
ms.date: 06/26/2014
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 98f65b068d8f22e3eeef48ca7fa441434939db8b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827951"
---
<a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="92280-104">Relaciones de entidad en OData v4 con ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="92280-104">Entity Relations in OData v4 Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="92280-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="92280-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="92280-106">La mayoría de los conjuntos de datos definen las relaciones entre entidades: los clientes tienen pedidos; los libros en pantalla tienen autores; los productos tienen proveedores.</span><span class="sxs-lookup"><span data-stu-id="92280-106">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="92280-107">Con OData, los clientes pueden navegar por las relaciones de entidad.</span><span class="sxs-lookup"><span data-stu-id="92280-107">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="92280-108">Dado un producto, puede encontrar el proveedor.</span><span class="sxs-lookup"><span data-stu-id="92280-108">Given a product, you can find the supplier.</span></span> <span data-ttu-id="92280-109">También puede crear o eliminar las relaciones.</span><span class="sxs-lookup"><span data-stu-id="92280-109">You can also create or remove relationships.</span></span> <span data-ttu-id="92280-110">Por ejemplo, puede establecer el proveedor de un producto.</span><span class="sxs-lookup"><span data-stu-id="92280-110">For example, you can set the supplier for a product.</span></span>
> 
> <span data-ttu-id="92280-111">Este tutorial muestra cómo admitir estas operaciones en OData v4 mediante ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="92280-111">This tutorial shows how to support these operations in OData v4 using ASP.NET Web API.</span></span> <span data-ttu-id="92280-112">El tutorial se basa en el tutorial [crear OData v4 extremo mediante ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="92280-112">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="92280-113">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="92280-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="92280-114">Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="92280-114">Web API 2.1</span></span>
> - <span data-ttu-id="92280-115">OData v4</span><span class="sxs-lookup"><span data-stu-id="92280-115">OData v4</span></span>
> - [<span data-ttu-id="92280-116">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="92280-116">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="92280-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="92280-117">Entity Framework 6</span></span>
> - <span data-ttu-id="92280-118">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="92280-118">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="92280-119">Versiones de tutoriales</span><span class="sxs-lookup"><span data-stu-id="92280-119">Tutorial versions</span></span>
> 
> <span data-ttu-id="92280-120">Para la versión 3 de OData, consulte [que admiten relaciones de entidad en OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).</span><span class="sxs-lookup"><span data-stu-id="92280-120">For the OData Version 3, see [Supporting Entity Relations in OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).</span></span>


## <a name="add-a-supplier-entity"></a><span data-ttu-id="92280-121">Agregar una entidad de proveedor</span><span class="sxs-lookup"><span data-stu-id="92280-121">Add a Supplier Entity</span></span>

> [!NOTE]
> <span data-ttu-id="92280-122">El tutorial se basa en el tutorial [crear OData v4 extremo mediante ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="92280-122">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span></span>


<span data-ttu-id="92280-123">En primer lugar, necesitamos una entidad relacionada.</span><span class="sxs-lookup"><span data-stu-id="92280-123">First, we need a related entity.</span></span> <span data-ttu-id="92280-124">Agregue una clase denominada `Supplier` en la carpeta Models.</span><span class="sxs-lookup"><span data-stu-id="92280-124">Add a class named `Supplier` in the Models folder.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

<span data-ttu-id="92280-125">Agregue una propiedad de navegación a la `Product` clase:</span><span class="sxs-lookup"><span data-stu-id="92280-125">Add a navigation property to the `Product` class:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

<span data-ttu-id="92280-126">Agregue un nuevo **DbSet** a la `ProductsContext` clase, para que Entity Framework incluirá la tabla de proveedor en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="92280-126">Add a new **DbSet** to the `ProductsContext` class, so that Entity Framework will include the Supplier table in the database.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

<span data-ttu-id="92280-127">En WebApiConfig.cs, agregue un &quot;proveedores&quot; del conjunto de entidades para el entity data model:</span><span class="sxs-lookup"><span data-stu-id="92280-127">In WebApiConfig.cs, add a &quot;Suppliers&quot; entity set to the entity data model:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a><span data-ttu-id="92280-128">Agregar un controlador de proveedores</span><span class="sxs-lookup"><span data-stu-id="92280-128">Add a Suppliers Controller</span></span>

<span data-ttu-id="92280-129">Agregar un `SuppliersController` clase a la carpeta Controllers.</span><span class="sxs-lookup"><span data-stu-id="92280-129">Add a `SuppliersController` class to the Controllers folder.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="92280-130">No se muestra cómo agregar operaciones CRUD para este controlador.</span><span class="sxs-lookup"><span data-stu-id="92280-130">I won't show how to add CRUD operations for this controller.</span></span> <span data-ttu-id="92280-131">Los pasos son los mismos que para el controlador de productos (consulte [crear un punto de conexión de OData v4](create-an-odata-v4-endpoint.md)).</span><span class="sxs-lookup"><span data-stu-id="92280-131">The steps are the same as for the Products controller (see [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md)).</span></span>

## <a name="getting-related-entities"></a><span data-ttu-id="92280-132">Obtener entidades relacionadas</span><span class="sxs-lookup"><span data-stu-id="92280-132">Getting Related Entities</span></span>

<span data-ttu-id="92280-133">Para obtener el proveedor de un producto, el cliente envía una solicitud GET:</span><span class="sxs-lookup"><span data-stu-id="92280-133">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

<span data-ttu-id="92280-134">Para admitir esta solicitud, agregue el método siguiente a la `ProductsController` clase:</span><span class="sxs-lookup"><span data-stu-id="92280-134">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

<span data-ttu-id="92280-135">Este método usa una convención de nomenclatura predeterminado</span><span class="sxs-lookup"><span data-stu-id="92280-135">This method uses a default naming convention</span></span>

- <span data-ttu-id="92280-136">Nombre del método: GetX, donde X es la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="92280-136">Method name: GetX, where X is the navigation property.</span></span>
- <span data-ttu-id="92280-137">Nombre del parámetro: *clave*</span><span class="sxs-lookup"><span data-stu-id="92280-137">Parameter name: *key*</span></span>

<span data-ttu-id="92280-138">Si sigue esta convención de nomenclatura, Web API asigna automáticamente la solicitud HTTP para el método del controlador.</span><span class="sxs-lookup"><span data-stu-id="92280-138">If you follow this naming convention, Web API automatically maps the HTTP request to the controller method.</span></span>

<span data-ttu-id="92280-139">Solicitud HTTP de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="92280-139">Example HTTP request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

<span data-ttu-id="92280-140">Respuesta HTTP de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="92280-140">Example HTTP response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a><span data-ttu-id="92280-141">Obtención de una colección relacionada</span><span class="sxs-lookup"><span data-stu-id="92280-141">Getting a related collection</span></span>

<span data-ttu-id="92280-142">En el ejemplo anterior, un producto tiene un proveedor.</span><span class="sxs-lookup"><span data-stu-id="92280-142">In the previous example, a product has one supplier.</span></span> <span data-ttu-id="92280-143">Una propiedad de navegación también puede devolver una colección.</span><span class="sxs-lookup"><span data-stu-id="92280-143">A navigation property can also return a collection.</span></span> <span data-ttu-id="92280-144">El código siguiente obtiene los productos para un distribuidor:</span><span class="sxs-lookup"><span data-stu-id="92280-144">The following code gets the products for a supplier:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

<span data-ttu-id="92280-145">En este caso, el método devuelve un **IQueryable** en lugar de un **SingleResult&lt;T&gt;**</span><span class="sxs-lookup"><span data-stu-id="92280-145">In this case, the method returns an **IQueryable** instead of a **SingleResult&lt;T&gt;**</span></span>

<span data-ttu-id="92280-146">Solicitud HTTP de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="92280-146">Example HTTP request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

<span data-ttu-id="92280-147">Respuesta HTTP de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="92280-147">Example HTTP response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a><span data-ttu-id="92280-148">Creación de una relación entre entidades</span><span class="sxs-lookup"><span data-stu-id="92280-148">Creating a Relationship Between Entities</span></span>

<span data-ttu-id="92280-149">OData admite la creación o eliminación de relaciones entre dos entidades existentes.</span><span class="sxs-lookup"><span data-stu-id="92280-149">OData supports creating or removing relationships between two existing entities.</span></span> <span data-ttu-id="92280-150">En la terminología de OData v4, la relación es una &quot;referencia&quot;.</span><span class="sxs-lookup"><span data-stu-id="92280-150">In OData v4 terminology, the relationship is a &quot;reference&quot;.</span></span> <span data-ttu-id="92280-151">(En OData v3, se llamó a la relación una *vínculo*.</span><span class="sxs-lookup"><span data-stu-id="92280-151">(In OData v3, the relationship was called a *link*.</span></span> <span data-ttu-id="92280-152">Las diferencias de protocolo no importan para este tutorial.)</span><span class="sxs-lookup"><span data-stu-id="92280-152">The protocol differences don't matter for this tutorial.)</span></span>

<span data-ttu-id="92280-153">Una referencia tiene su propio URI, con el formulario `/Entity/NavigationProperty/$ref`.</span><span class="sxs-lookup"><span data-stu-id="92280-153">A reference has its own URI, with the form `/Entity/NavigationProperty/$ref`.</span></span> <span data-ttu-id="92280-154">Por ejemplo, este es el URI para resolver la referencia entre un producto y su proveedor:</span><span class="sxs-lookup"><span data-stu-id="92280-154">For example, here is the URI to address the reference between a product and its supplier:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

<span data-ttu-id="92280-155">Para agregar una relación, el cliente envía una solicitud POST o PUT a esta dirección.</span><span class="sxs-lookup"><span data-stu-id="92280-155">To add a relationship, the client sends a POST or PUT request to this address.</span></span>

- <span data-ttu-id="92280-156">Si la propiedad de navegación es una sola entidad, como `Product.Supplier`.</span><span class="sxs-lookup"><span data-stu-id="92280-156">PUT if the navigation property is a single entity, such as `Product.Supplier`.</span></span>
- <span data-ttu-id="92280-157">REGISTRAR si la propiedad de navegación es una colección, como `Supplier.Products`.</span><span class="sxs-lookup"><span data-stu-id="92280-157">POST if the navigation property is a collection, such as `Supplier.Products`.</span></span>

<span data-ttu-id="92280-158">El cuerpo de la solicitud contiene el URI de la otra entidad en la relación.</span><span class="sxs-lookup"><span data-stu-id="92280-158">The body of the request contains the URI of the other entity in the relation.</span></span> <span data-ttu-id="92280-159">Este es un ejemplo de solicitud:</span><span class="sxs-lookup"><span data-stu-id="92280-159">Here is an example request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

<span data-ttu-id="92280-160">En este ejemplo, el cliente envía una solicitud PUT para `/Products(6)/Supplier/$ref`, que es el URI de $ref para el `Supplier` del producto con el Id. = 6.</span><span class="sxs-lookup"><span data-stu-id="92280-160">In this example, the client sends a PUT request to `/Products(6)/Supplier/$ref`, which is the $ref URI for the `Supplier` of the product with ID = 6.</span></span> <span data-ttu-id="92280-161">Si la solicitud se realiza correctamente, el servidor envía una respuesta 204 (sin contenido):</span><span class="sxs-lookup"><span data-stu-id="92280-161">If the request succeeds, the server sends a 204 (No Content) response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

<span data-ttu-id="92280-162">Este es el método de controlador para agregar una relación con un `Product`:</span><span class="sxs-lookup"><span data-stu-id="92280-162">Here is the controller method to add a relationship to a `Product`:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

<span data-ttu-id="92280-163">El *navigationProperty* parámetro especifica qué relación establezca.</span><span class="sxs-lookup"><span data-stu-id="92280-163">The *navigationProperty* parameter specifies which relationship to set.</span></span> <span data-ttu-id="92280-164">(Si hay más de una propiedad de navegación en la entidad, puede agregar más `case` instrucciones.)</span><span class="sxs-lookup"><span data-stu-id="92280-164">(If there is more than one navigation property on the entity, you can add more `case` statements.)</span></span>

<span data-ttu-id="92280-165">El *vínculo* parámetro contiene el URI del proveedor.</span><span class="sxs-lookup"><span data-stu-id="92280-165">The *link* parameter contains the URI of the supplier.</span></span> <span data-ttu-id="92280-166">API Web analiza automáticamente el cuerpo de solicitud para obtener el valor de este parámetro.</span><span class="sxs-lookup"><span data-stu-id="92280-166">Web API automatically parses the request body to get the value for this parameter.</span></span>

<span data-ttu-id="92280-167">Para buscar el proveedor, se necesita el identificador (o clave), que forma parte de la *vínculo* parámetro.</span><span class="sxs-lookup"><span data-stu-id="92280-167">To look up the supplier, we need the ID (or key), which is part of the *link* parameter.</span></span> <span data-ttu-id="92280-168">Para ello, use el siguiente método auxiliar:</span><span class="sxs-lookup"><span data-stu-id="92280-168">To do this, use the following helper method:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

<span data-ttu-id="92280-169">Básicamente, este método usa la biblioteca de OData para dividir la ruta de acceso URI en segmentos, busque el segmento que contiene la clave y convertir la clave en el tipo correcto.</span><span class="sxs-lookup"><span data-stu-id="92280-169">Basically, this method uses the OData library to split the URI path into segments, find the segment that contains the key, and convert the key into the correct type.</span></span>

## <a name="deleting-a-relationship-between-entities"></a><span data-ttu-id="92280-170">Eliminar una relación entre entidades</span><span class="sxs-lookup"><span data-stu-id="92280-170">Deleting a Relationship Between Entities</span></span>

<span data-ttu-id="92280-171">Para eliminar una relación, el cliente envía una solicitud HTTP DELETE al URI $ref:</span><span class="sxs-lookup"><span data-stu-id="92280-171">To delete a relationship, the client sends an HTTP DELETE request to the $ref URI:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

<span data-ttu-id="92280-172">Este es el método de controlador para eliminar la relación entre un producto y un proveedor:</span><span class="sxs-lookup"><span data-stu-id="92280-172">Here is the controller method to delete the relationship between a Product and a Supplier:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

<span data-ttu-id="92280-173">En este caso, `Product.Supplier` es el &quot;1&quot; final de una relación de 1 a muchos, por lo que puede eliminar la relación simplemente estableciendo `Product.Supplier` a `null`.</span><span class="sxs-lookup"><span data-stu-id="92280-173">In this case, `Product.Supplier` is the &quot;1&quot; end of a 1-to-many relation, so you can remove the relationship just by setting `Product.Supplier` to `null`.</span></span>

<span data-ttu-id="92280-174">En el &quot;muchos&quot; final de una relación, el cliente debe especificar que relacionados con la entidad que se va a quitar.</span><span class="sxs-lookup"><span data-stu-id="92280-174">In the &quot;many&quot; end of a relationship, the client must specify which related entity to remove.</span></span> <span data-ttu-id="92280-175">Para ello, el cliente envía el URI de la entidad relacionada en la cadena de consulta de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="92280-175">To do so, the client sends the URI of the related entity in the query string of the request.</span></span> <span data-ttu-id="92280-176">Por ejemplo, para quitar "Product 1" de "proveedor 1":</span><span class="sxs-lookup"><span data-stu-id="92280-176">For example, to remove "Product 1" from "Supplier 1":</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

<span data-ttu-id="92280-177">Para admitir esto en la API Web, es necesario incluir un parámetro adicional en el `DeleteRef` método.</span><span class="sxs-lookup"><span data-stu-id="92280-177">To support this in Web API, we need to include an extra parameter in the `DeleteRef` method.</span></span> <span data-ttu-id="92280-178">Este es el método de controlador para quitar un producto de la `Supplier.Products` relación.</span><span class="sxs-lookup"><span data-stu-id="92280-178">Here is the controller method to remove a product from the `Supplier.Products` relation.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

<span data-ttu-id="92280-179">El *clave* parámetro es la clave para el proveedor y el *relatedKey* parámetro es la clave del producto que se quitará el `Products` relación.</span><span class="sxs-lookup"><span data-stu-id="92280-179">The *key* parameter is the key for the supplier, and the *relatedKey* parameter is the key for the product to remove from the `Products` relationship.</span></span> <span data-ttu-id="92280-180">Tenga en cuenta que Web API obtiene automáticamente la clave de la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="92280-180">Note that Web API automatically gets the key from the query string.</span></span>
