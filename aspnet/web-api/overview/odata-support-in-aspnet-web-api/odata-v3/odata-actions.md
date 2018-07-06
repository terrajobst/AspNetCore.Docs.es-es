---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: Compatibilidad con las acciones de OData en ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: 'En OData, las acciones son una manera de agregar comportamientos de lado servidor que no se definen fácilmente como operaciones CRUD en entidades. Algunos usos de las acciones incluyen: implemente...'
ms.author: aspnetcontent
ms.date: 02/25/2014
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: e02ab21b864e328fe6892a00e5d5aca3f88eb9a2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837777"
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a><span data-ttu-id="87643-104">Compatibilidad con las acciones de OData en ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="87643-104">Supporting OData Actions in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="87643-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="87643-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="87643-106">Descargue el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="87643-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="87643-107">En OData, *acciones* son una forma de agregar comportamientos de lado servidor que no se definen fácilmente como operaciones CRUD en entidades.</span><span class="sxs-lookup"><span data-stu-id="87643-107">In OData, *actions* are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="87643-108">Algunos usos de las acciones incluyen:</span><span class="sxs-lookup"><span data-stu-id="87643-108">Some uses for actions include:</span></span>
> 
> - <span data-ttu-id="87643-109">Implementación de las transacciones complejas.</span><span class="sxs-lookup"><span data-stu-id="87643-109">Implementing complex transactions.</span></span>
> - <span data-ttu-id="87643-110">Manipular varias entidades a la vez.</span><span class="sxs-lookup"><span data-stu-id="87643-110">Manipulating several entities at once.</span></span>
> - <span data-ttu-id="87643-111">Permitir actualizaciones únicamente a determinadas propiedades de una entidad.</span><span class="sxs-lookup"><span data-stu-id="87643-111">Allowing updates only to certain properties of an entity.</span></span>
> - <span data-ttu-id="87643-112">Enviar información al servidor que no está definido en una entidad.</span><span class="sxs-lookup"><span data-stu-id="87643-112">Sending information to the server that is not defined in an entity.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="87643-113">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="87643-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="87643-114">Web API 2</span><span class="sxs-lookup"><span data-stu-id="87643-114">Web API 2</span></span>
> - <span data-ttu-id="87643-115">OData versión 3</span><span class="sxs-lookup"><span data-stu-id="87643-115">OData Version 3</span></span>
> - <span data-ttu-id="87643-116">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="87643-116">Entity Framework 6</span></span>


## <a name="example-rating-a-product"></a><span data-ttu-id="87643-117">Ejemplo: Clasificación de un producto</span><span class="sxs-lookup"><span data-stu-id="87643-117">Example: Rating a Product</span></span>

<span data-ttu-id="87643-118">En este ejemplo, queremos permitir que los usuarios clasificar los productos y, a continuación, exponer el promedio de clasificaciones para cada producto.</span><span class="sxs-lookup"><span data-stu-id="87643-118">In this example, we want to let users rate products, and then expose the average ratings for each product.</span></span> <span data-ttu-id="87643-119">En la base de datos, se almacenará una lista de las clasificaciones, ordenados a los productos.</span><span class="sxs-lookup"><span data-stu-id="87643-119">On the database, we will store a list of ratings, keyed to products.</span></span>

<span data-ttu-id="87643-120">Este es el modelo que podríamos utilizar para representar las clasificaciones en Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="87643-120">Here is the model we might use to represent the ratings in Entity Framework:</span></span>

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

<span data-ttu-id="87643-121">Pero no queremos que los clientes para registrar un `ProductRating` objeto a una colección de "Restricciones".</span><span class="sxs-lookup"><span data-stu-id="87643-121">But we don't want clients to POST a `ProductRating` object to a "Ratings" collection.</span></span> <span data-ttu-id="87643-122">De manera intuitiva, la clasificación está asociada a la colección de productos y el cliente solo debe registrar el valor de clasificación.</span><span class="sxs-lookup"><span data-stu-id="87643-122">Intuitively, the rating is associated with the Products collection, and the client should only need to post the rating value.</span></span>

<span data-ttu-id="87643-123">Por lo tanto, en lugar de las operaciones CRUD normales, definimos una acción que un cliente puede invocar en un producto.</span><span class="sxs-lookup"><span data-stu-id="87643-123">Therefore, instead of using the normal CRUD operations, we define an action that a client can invoke on a Product.</span></span> <span data-ttu-id="87643-124">En la terminología de OData, la acción es *enlazados* a entidades de producto.</span><span class="sxs-lookup"><span data-stu-id="87643-124">In OData terminology, the action is *bound* to Product entities.</span></span>

><span data-ttu-id="87643-125">Las acciones tienen efectos secundarios en el servidor.</span><span class="sxs-lookup"><span data-stu-id="87643-125">Actions have side-effects on the server.</span></span> <span data-ttu-id="87643-126">Por este motivo, se invocan mediante solicitudes HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="87643-126">For this reason, they are invoked using HTTP POST requests.</span></span> <span data-ttu-id="87643-127">Las acciones pueden tener parámetros y tipos de valor devuelto, que se describen en los metadatos del servicio.</span><span class="sxs-lookup"><span data-stu-id="87643-127">Actions can have parameters and return types, which are described in the service metadata.</span></span> <span data-ttu-id="87643-128">El cliente envía los parámetros en el cuerpo de solicitud y el servidor envía el valor devuelto en el cuerpo de respuesta.</span><span class="sxs-lookup"><span data-stu-id="87643-128">The client sends the parameters in the request body, and the server sends the return value in the response body.</span></span> <span data-ttu-id="87643-129">Para invocar la acción de "Velocidad del producto", el cliente envía un POST a un URI similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="87643-129">To invoke the "Rate Product" action, the client sends a POST to a URI like the following:</span></span>

[!code-console[Main](odata-actions/samples/sample2.cmd)]

<span data-ttu-id="87643-130">Los datos en la solicitud POST están simplemente la clasificación de producto:</span><span class="sxs-lookup"><span data-stu-id="87643-130">The data in the POST request is simply the product rating:</span></span>

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a><span data-ttu-id="87643-131">Declarar la acción en el Entity Data Model</span><span class="sxs-lookup"><span data-stu-id="87643-131">Declare the Action in the Entity Data Model</span></span>

<span data-ttu-id="87643-132">En la configuración de Web API, agregue la acción para el entity data model (EDM):</span><span class="sxs-lookup"><span data-stu-id="87643-132">In your Web API configuration, add the action to the entity data model (EDM):</span></span>

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

<span data-ttu-id="87643-133">Este código define "RateProduct" como una acción que se puede realizar en entidades de producto.</span><span class="sxs-lookup"><span data-stu-id="87643-133">This code defines "RateProduct" as an action that can be performed on Product entities.</span></span> <span data-ttu-id="87643-134">También declara que la acción toma un **int** parámetro denominado "Clasificación" y devuelve un **int** valor.</span><span class="sxs-lookup"><span data-stu-id="87643-134">It also declares that the action takes an **int** parameter named "Rating", and returns an **int** value.</span></span>

## <a name="add-the-action-to-the-controller"></a><span data-ttu-id="87643-135">Agregue la acción al controlador</span><span class="sxs-lookup"><span data-stu-id="87643-135">Add the Action to the Controller</span></span>

<span data-ttu-id="87643-136">La acción "RateProduct" se enlaza a las entidades de producto.</span><span class="sxs-lookup"><span data-stu-id="87643-136">The "RateProduct" action is bound to Product entities.</span></span> <span data-ttu-id="87643-137">Para implementar la acción, agregue un método denominado `RateProduct` al controlador de productos:</span><span class="sxs-lookup"><span data-stu-id="87643-137">To implement the action, add a method named `RateProduct` to the Products controller:</span></span>

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

<span data-ttu-id="87643-138">Tenga en cuenta que el nombre del método coincide con el nombre de la acción en el EDM.</span><span class="sxs-lookup"><span data-stu-id="87643-138">Notice that the method name matches the name of the action in the EDM.</span></span> <span data-ttu-id="87643-139">El método tiene dos parámetros:</span><span class="sxs-lookup"><span data-stu-id="87643-139">The method has two parameters:</span></span>

- <span data-ttu-id="87643-140">*clave*: la clave del producto a velocidad.</span><span class="sxs-lookup"><span data-stu-id="87643-140">*key*: The key for the product to rate.</span></span>
- <span data-ttu-id="87643-141">*parámetros*: un diccionario de valores de parámetro de acción.</span><span class="sxs-lookup"><span data-stu-id="87643-141">*parameters*: A dictionary of action parameter values.</span></span>

<span data-ttu-id="87643-142">Si usa las convenciones de enrutamiento de forma predeterminada, el parámetro de clave debe denominarse "clave".</span><span class="sxs-lookup"><span data-stu-id="87643-142">If you are using the default routing conventions, the key parameter must be named "key".</span></span> <span data-ttu-id="87643-143">También es importante incluir la **[FromOdataUri]** de atributo, como se muestra.</span><span class="sxs-lookup"><span data-stu-id="87643-143">It is also important to include the **[FromOdataUri]** attribute, as shown.</span></span> <span data-ttu-id="87643-144">Este atributo indica a Web API para usar las reglas de sintaxis de OData cuando analiza la clave desde el URI de solicitud.</span><span class="sxs-lookup"><span data-stu-id="87643-144">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

<span data-ttu-id="87643-145">Use la *parámetros* diccionario para obtener los parámetros de acción:</span><span class="sxs-lookup"><span data-stu-id="87643-145">Use the *parameters* dictionary to get the action parameters:</span></span>

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

<span data-ttu-id="87643-146">Si el cliente envía los parámetros de acción en el valor correcto de formato, el valor de **ModelState.IsValid** es true.</span><span class="sxs-lookup"><span data-stu-id="87643-146">If the client sends the action parameters in the correct format, the value of **ModelState.IsValid** is true.</span></span> <span data-ttu-id="87643-147">En ese caso, puede usar el **ODataActionParameters** diccionario para obtener los valores de parámetro.</span><span class="sxs-lookup"><span data-stu-id="87643-147">In that case, you can use the **ODataActionParameters** dictionary to get the parameter values.</span></span> <span data-ttu-id="87643-148">En este ejemplo, el `RateProduct` acción toma un único parámetro denominado "Rating".</span><span class="sxs-lookup"><span data-stu-id="87643-148">In this example, the `RateProduct` action takes a single parameter named "Rating".</span></span>

## <a name="action-metadata"></a><span data-ttu-id="87643-149">Metadatos de acción</span><span class="sxs-lookup"><span data-stu-id="87643-149">Action Metadata</span></span>

<span data-ttu-id="87643-150">Para ver los metadatos del servicio, envía una solicitud GET a los metadatos de /odata/$.</span><span class="sxs-lookup"><span data-stu-id="87643-150">To view the service metadata, send a GET request to /odata/$metadata.</span></span> <span data-ttu-id="87643-151">Esta es la parte de los metadatos que declara el `RateProduct` acción:</span><span class="sxs-lookup"><span data-stu-id="87643-151">Here is the portion of the metadata that declares the `RateProduct` action:</span></span>

[!code-xml[Main](odata-actions/samples/sample7.xml)]

<span data-ttu-id="87643-152">El **FunctionImport** elemento declara la acción.</span><span class="sxs-lookup"><span data-stu-id="87643-152">The **FunctionImport** element declares the action.</span></span> <span data-ttu-id="87643-153">La mayoría de los campos es autoexplicativos, pero dos son cabe destacar:</span><span class="sxs-lookup"><span data-stu-id="87643-153">Most of the fields are self-explanatory, but two are worth noting:</span></span>

- <span data-ttu-id="87643-154">**IsBindable** significa que la acción se puede invocar en la entidad de destino, al menos parte del tiempo.</span><span class="sxs-lookup"><span data-stu-id="87643-154">**IsBindable** means the action can be invoked on the target entity, at least some of the time.</span></span>
- <span data-ttu-id="87643-155">**IsAlwaysBindable** significa que siempre se puede invocar la acción en la entidad de destino.</span><span class="sxs-lookup"><span data-stu-id="87643-155">**IsAlwaysBindable** means the action can always be invoked on the target entity.</span></span>

<span data-ttu-id="87643-156">La diferencia es que algunas acciones siempre están disponibles para los clientes, pero es posible que dependen de otras acciones en el estado de la entidad.</span><span class="sxs-lookup"><span data-stu-id="87643-156">The difference is that some actions are always available to clients, but other actions might depend on the state of the entity.</span></span> <span data-ttu-id="87643-157">Por ejemplo, supongamos que define una acción de "Comprar".</span><span class="sxs-lookup"><span data-stu-id="87643-157">For example, suppose you define a "Purchase" action.</span></span> <span data-ttu-id="87643-158">Sólo puede adquirir un elemento que está en existencias.</span><span class="sxs-lookup"><span data-stu-id="87643-158">You can only purchase an item that is in stock.</span></span> <span data-ttu-id="87643-159">Si el elemento está fuera de existencias, un cliente no puede invocar la acción.</span><span class="sxs-lookup"><span data-stu-id="87643-159">If the item is out of stock, a client cannot invoke that action.</span></span>

<span data-ttu-id="87643-160">Cuando se define el EDM, el **acción** método crea una acción siempre puede enlazar:</span><span class="sxs-lookup"><span data-stu-id="87643-160">When you define the EDM, the **Action** method creates an always-bindable action:</span></span>

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

<span data-ttu-id="87643-161">Hablaré sobre las acciones no siempre enlazable (también denominada *transitorios* acciones) más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="87643-161">I'll talk about not-always-bindable actions (also called *transient* actions) later in this topic.</span></span>

## <a name="invoking-the-action"></a><span data-ttu-id="87643-162">Invocar la acción</span><span class="sxs-lookup"><span data-stu-id="87643-162">Invoking the Action</span></span>

<span data-ttu-id="87643-163">Ahora veamos cómo un cliente podría invocar esta acción.</span><span class="sxs-lookup"><span data-stu-id="87643-163">Now let's see how a client would invoke this action.</span></span> <span data-ttu-id="87643-164">Supongamos que el cliente desea evaluarlos 2 para el producto con el identificador = 4.</span><span class="sxs-lookup"><span data-stu-id="87643-164">Suppose the client wants to give a rating of 2 to the product with ID = 4.</span></span> <span data-ttu-id="87643-165">Este es un mensaje de solicitud de ejemplo con formato JSON para el cuerpo de solicitud:</span><span class="sxs-lookup"><span data-stu-id="87643-165">Here is an example request message, using JSON format for the request body:</span></span>

[!code-console[Main](odata-actions/samples/sample9.cmd)]

<span data-ttu-id="87643-166">Este es el mensaje de respuesta:</span><span class="sxs-lookup"><span data-stu-id="87643-166">Here is the response message:</span></span>

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a><span data-ttu-id="87643-167">Enlazar una acción a un conjunto de entidades</span><span class="sxs-lookup"><span data-stu-id="87643-167">Binding an Action to an Entity Set</span></span>

<span data-ttu-id="87643-168">En el ejemplo anterior, la acción está enlazada a una sola entidad: el cliente evalúa un solo producto.</span><span class="sxs-lookup"><span data-stu-id="87643-168">In the previous example, the action is bound to a single entity: The client rates a single product.</span></span> <span data-ttu-id="87643-169">También puede enlazar una acción a una colección de entidades.</span><span class="sxs-lookup"><span data-stu-id="87643-169">You can also bind an action to a collection of entities.</span></span> <span data-ttu-id="87643-170">Simplemente, realice los cambios siguientes:</span><span class="sxs-lookup"><span data-stu-id="87643-170">Just make the following changes:</span></span>

<span data-ttu-id="87643-171">En el EDM, agregue la acción a la entidad **colección** propiedad.</span><span class="sxs-lookup"><span data-stu-id="87643-171">In the EDM, add the action to the entity's **Collection** property.</span></span>

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

<span data-ttu-id="87643-172">En el método de controlador, omita el *clave* parámetro.</span><span class="sxs-lookup"><span data-stu-id="87643-172">In the controller method, omit the *key* parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

<span data-ttu-id="87643-173">Ahora el cliente invoca la acción en el conjunto de entidades de productos:</span><span class="sxs-lookup"><span data-stu-id="87643-173">Now the client invokes the action on the Products entity set:</span></span>

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a><span data-ttu-id="87643-174">Acciones con los parámetros de recopilación</span><span class="sxs-lookup"><span data-stu-id="87643-174">Actions with Collection Parameters</span></span>

<span data-ttu-id="87643-175">Las acciones pueden tener parámetros que toman una colección de valores.</span><span class="sxs-lookup"><span data-stu-id="87643-175">Actions can have parameters that take a collection of values.</span></span> <span data-ttu-id="87643-176">En el EDM, utilice **CollectionParameter&lt;T&gt;**  para declarar el parámetro.</span><span class="sxs-lookup"><span data-stu-id="87643-176">In the EDM, use **CollectionParameter&lt;T&gt;** to declare the parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

<span data-ttu-id="87643-177">Esto declara un parámetro denominado "Clasificaciones" que toma una colección de **int** valores.</span><span class="sxs-lookup"><span data-stu-id="87643-177">This declares a parameter named "Ratings" that takes a collection of **int** values.</span></span> <span data-ttu-id="87643-178">En el método de controlador, obteniendo el valor del parámetro desde el **ODataActionParameters** objeto, pero ahora el valor es un **ICollection&lt;int&gt;**  valor:</span><span class="sxs-lookup"><span data-stu-id="87643-178">In the controller method, you still get the parameter value from the **ODataActionParameters** object, but now the value is an **ICollection&lt;int&gt;** value:</span></span>

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a><span data-ttu-id="87643-179">Acciones transitorias</span><span class="sxs-lookup"><span data-stu-id="87643-179">Transient Actions</span></span>

<span data-ttu-id="87643-180">En el ejemplo "RateProduct", los usuarios siempre pueden clasificar un producto, por lo que siempre está disponible la acción.</span><span class="sxs-lookup"><span data-stu-id="87643-180">In the "RateProduct" example, users can always rate a product, so the action is always available.</span></span> <span data-ttu-id="87643-181">Pero algunas acciones dependen del estado de la entidad.</span><span class="sxs-lookup"><span data-stu-id="87643-181">But some actions depend on the state of the entity.</span></span> <span data-ttu-id="87643-182">Por ejemplo, en un servicio de alquiler de vídeos, la acción de "Extracción" no está siempre disponible.</span><span class="sxs-lookup"><span data-stu-id="87643-182">For example, in a video rental service, the "CheckOut" action is not always available.</span></span> <span data-ttu-id="87643-183">(Depende de si hay disponible una copia de ese vídeo.) Este tipo de acción se denomina un *transitorios* acción.</span><span class="sxs-lookup"><span data-stu-id="87643-183">(It depends whether a copy of that video is available.) This type of action is called a *transient* action.</span></span>

<span data-ttu-id="87643-184">En los metadatos del servicio, tiene una acción transitoria **IsAlwaysBindable** igual a false.</span><span class="sxs-lookup"><span data-stu-id="87643-184">In the service metadata, a transient action has **IsAlwaysBindable** equal to false.</span></span> <span data-ttu-id="87643-185">Que es realmente el valor predeterminado, por lo que los metadatos tendrá este aspecto:</span><span class="sxs-lookup"><span data-stu-id="87643-185">That's actually the default value, so the metadata will look like this:</span></span>

[!code-xml[Main](odata-actions/samples/sample16.xml)]

<span data-ttu-id="87643-186">Aquí es ¿por qué esto es importante: si una acción es transitoria, el servidor debe indicar al cliente cuando la acción está disponible.</span><span class="sxs-lookup"><span data-stu-id="87643-186">Here's why this matters: If an action is transient, the server needs to tell the client when the action is available.</span></span> <span data-ttu-id="87643-187">Para ello incluye un vínculo a la acción de la entidad.</span><span class="sxs-lookup"><span data-stu-id="87643-187">It does this by including a link to the action in the entity.</span></span> <span data-ttu-id="87643-188">Este es un ejemplo de una entidad de película:</span><span class="sxs-lookup"><span data-stu-id="87643-188">Here is an example for a Movie entity:</span></span>

[!code-console[Main](odata-actions/samples/sample17.cmd)]

<span data-ttu-id="87643-189">La propiedad "#CheckOut" contiene un vínculo a la acción de extracción.</span><span class="sxs-lookup"><span data-stu-id="87643-189">The "#CheckOut" property contains a link to the CheckOut action.</span></span> <span data-ttu-id="87643-190">Si la acción no está disponible, el servidor omite el vínculo.</span><span class="sxs-lookup"><span data-stu-id="87643-190">If the action is not available, the server omits the link.</span></span>

<span data-ttu-id="87643-191">Para declarar una acción transitoria en el EDM, llame a la **TransientAction** método:</span><span class="sxs-lookup"><span data-stu-id="87643-191">To declare a transient action in the EDM, call the **TransientAction** method:</span></span>

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

<span data-ttu-id="87643-192">Además, debe proporcionar una función que devuelve un vínculo de acción para una entidad determinada.</span><span class="sxs-lookup"><span data-stu-id="87643-192">Also, you must provide a function that returns an action link for a given entity.</span></span> <span data-ttu-id="87643-193">Establezca esta función mediante una llamada a **HasActionLink**.</span><span class="sxs-lookup"><span data-stu-id="87643-193">Set this function by calling **HasActionLink**.</span></span> <span data-ttu-id="87643-194">Puede escribir la función como una expresión lambda:</span><span class="sxs-lookup"><span data-stu-id="87643-194">You can write the function as a lambda expression:</span></span>

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

<span data-ttu-id="87643-195">Si la acción está disponible, la expresión lambda devuelve un vínculo a la acción.</span><span class="sxs-lookup"><span data-stu-id="87643-195">If the action is available, the lambda expression returns a link to the action.</span></span> <span data-ttu-id="87643-196">El serializador de OData incluye este vínculo cuando serializa la entidad.</span><span class="sxs-lookup"><span data-stu-id="87643-196">The OData serializer includes this link when it serializes the entity.</span></span> <span data-ttu-id="87643-197">Cuando la acción no está disponible, la función devuelve `null`.</span><span class="sxs-lookup"><span data-stu-id="87643-197">When the action is not available, the function returns `null`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="87643-198">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="87643-198">Additional Resources</span></span>

[<span data-ttu-id="87643-199">Ejemplo de acciones de OData</span><span class="sxs-lookup"><span data-stu-id="87643-199">OData Actions Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
