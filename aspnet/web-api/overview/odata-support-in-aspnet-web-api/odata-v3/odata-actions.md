---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: Compatibilidad con las acciones de OData en ASP.NET Web API 2 | Documentos de Microsoft
author: MikeWasson
description: "En OData, las acciones son una manera de agregar los comportamientos de servidor que no se definen fácilmente como operaciones CRUD en entidades. Algunos usos de las acciones son: implemente..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: acb369ca8f1bab8d7cad14c15f46cfd44beb9fdd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a><span data-ttu-id="9f7d9-104">Compatibilidad con las acciones de OData en ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="9f7d9-104">Supporting OData Actions in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="9f7d9-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9f7d9-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="9f7d9-106">Descargar el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="9f7d9-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="9f7d9-107">En OData, *acciones* son una forma de agregar los comportamientos de servidor que no se definen fácilmente como operaciones CRUD en entidades.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-107">In OData, *actions* are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="9f7d9-108">Algunos usos de las acciones son:</span><span class="sxs-lookup"><span data-stu-id="9f7d9-108">Some uses for actions include:</span></span>
> 
> - <span data-ttu-id="9f7d9-109">Implementación de las transacciones complejas.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-109">Implementing complex transactions.</span></span>
> - <span data-ttu-id="9f7d9-110">La manipulación de varias entidades a la vez.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-110">Manipulating several entities at once.</span></span>
> - <span data-ttu-id="9f7d9-111">Permitir actualizaciones solo a determinadas propiedades de una entidad.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-111">Allowing updates only to certain properties of an entity.</span></span>
> - <span data-ttu-id="9f7d9-112">Enviar información al servidor que no está definido en una entidad.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-112">Sending information to the server that is not defined in an entity.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="9f7d9-113">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="9f7d9-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="9f7d9-114">Web API 2</span><span class="sxs-lookup"><span data-stu-id="9f7d9-114">Web API 2</span></span>
> - <span data-ttu-id="9f7d9-115">Versión 3 de OData</span><span class="sxs-lookup"><span data-stu-id="9f7d9-115">OData Version 3</span></span>
> - <span data-ttu-id="9f7d9-116">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="9f7d9-116">Entity Framework 6</span></span>


## <a name="example-rating-a-product"></a><span data-ttu-id="9f7d9-117">Ejemplo: Clasificación de un producto</span><span class="sxs-lookup"><span data-stu-id="9f7d9-117">Example: Rating a Product</span></span>

<span data-ttu-id="9f7d9-118">En este ejemplo, queremos que los usuarios puedan clasificar los productos y, a continuación, exponer el promedio de clasificaciones para cada producto.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-118">In this example, we want to let users rate products, and then expose the average ratings for each product.</span></span> <span data-ttu-id="9f7d9-119">En la base de datos, se almacenará una lista de clasificaciones, organizados en productos.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-119">On the database, we will store a list of ratings, keyed to products.</span></span>

<span data-ttu-id="9f7d9-120">Este es el modelo que podríamos utilizar para representar las clasificaciones en Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="9f7d9-120">Here is the model we might use to represent the ratings in Entity Framework:</span></span>

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

<span data-ttu-id="9f7d9-121">Pero no queremos que los clientes para registrar un `ProductRating` objeto a una colección de "Clasificaciones".</span><span class="sxs-lookup"><span data-stu-id="9f7d9-121">But we don't want clients to POST a `ProductRating` object to a "Ratings" collection.</span></span> <span data-ttu-id="9f7d9-122">De manera intuitiva, la clasificación está asociada a la colección de productos y el cliente solo se tiene que registrar el valor de clasificación.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-122">Intuitively, the rating is associated with the Products collection, and the client should only need to post the rating value.</span></span>

<span data-ttu-id="9f7d9-123">Por lo tanto, en lugar de utilizar las operaciones CRUD normales, definimos una acción que un cliente puede invocar en un producto.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-123">Therefore, instead of using the normal CRUD operations, we define an action that a client can invoke on a Product.</span></span> <span data-ttu-id="9f7d9-124">En la terminología de OData, la acción es *enlazados* a entidades de producto.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-124">In OData terminology, the action is *bound* to Product entities.</span></span>

><span data-ttu-id="9f7d9-125">Las acciones tienen efectos secundarios en el servidor.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-125">Actions have side-effects on the server.</span></span> <span data-ttu-id="9f7d9-126">Por este motivo, se invocan mediante solicitudes HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-126">For this reason, they are invoked using HTTP POST requests.</span></span> <span data-ttu-id="9f7d9-127">Las acciones pueden tener parámetros y tipos de valor devuelto, que se describen en los metadatos del servicio.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-127">Actions can have parameters and return types, which are described in the service metadata.</span></span> <span data-ttu-id="9f7d9-128">El cliente envía los parámetros en el cuerpo de solicitud y el servidor envía el valor devuelto en el cuerpo de respuesta.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-128">The client sends the parameters in the request body, and the server sends the return value in the response body.</span></span> <span data-ttu-id="9f7d9-129">Para invocar la acción "Velocidad producto", el cliente envía una operación POST a un URI similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="9f7d9-129">To invoke the "Rate Product" action, the client sends a POST to a URI like the following:</span></span>

[!code-console[Main](odata-actions/samples/sample2.cmd)]

<span data-ttu-id="9f7d9-130">Los datos de la solicitud POST están simplemente la clasificación de producto:</span><span class="sxs-lookup"><span data-stu-id="9f7d9-130">The data in the POST request is simply the product rating:</span></span>

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a><span data-ttu-id="9f7d9-131">Declarar la acción en el Entity Data Model</span><span class="sxs-lookup"><span data-stu-id="9f7d9-131">Declare the Action in the Entity Data Model</span></span>

<span data-ttu-id="9f7d9-132">En la configuración de la API Web, agregue la acción para el entity data model (EDM):</span><span class="sxs-lookup"><span data-stu-id="9f7d9-132">In your Web API configuration, add the action to the entity data model (EDM):</span></span>

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

<span data-ttu-id="9f7d9-133">Este código define "RateProduct" como una acción que se pueden realizar en entidades de producto.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-133">This code defines "RateProduct" as an action that can be performed on Product entities.</span></span> <span data-ttu-id="9f7d9-134">También declara que la acción que emprende una **int** parámetro denominado "Clasificación" y devuelve un **int** valor.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-134">It also declares that the action takes an **int** parameter named "Rating", and returns an **int** value.</span></span>

## <a name="add-the-action-to-the-controller"></a><span data-ttu-id="9f7d9-135">Agregar la acción para el controlador</span><span class="sxs-lookup"><span data-stu-id="9f7d9-135">Add the Action to the Controller</span></span>

<span data-ttu-id="9f7d9-136">La acción "RateProduct" se enlaza a las entidades de producto.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-136">The "RateProduct" action is bound to Product entities.</span></span> <span data-ttu-id="9f7d9-137">Para implementar la acción, agregue un método denominado `RateProduct` al controlador de productos:</span><span class="sxs-lookup"><span data-stu-id="9f7d9-137">To implement the action, add a method named `RateProduct` to the Products controller:</span></span>

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

<span data-ttu-id="9f7d9-138">Tenga en cuenta que el nombre del método coincide con el nombre de la acción en el EDM.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-138">Notice that the method name matches the name of the action in the EDM.</span></span> <span data-ttu-id="9f7d9-139">El método tiene dos parámetros:</span><span class="sxs-lookup"><span data-stu-id="9f7d9-139">The method has two parameters:</span></span>

- <span data-ttu-id="9f7d9-140">*clave*: la clave del producto a velocidad.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-140">*key*: The key for the product to rate.</span></span>
- <span data-ttu-id="9f7d9-141">*parámetros*: un diccionario de valores de parámetro de acción.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-141">*parameters*: A dictionary of action parameter values.</span></span>

<span data-ttu-id="9f7d9-142">Si usa las convenciones de enrutamiento de forma predeterminada, el parámetro de clave debe denominarse "clave".</span><span class="sxs-lookup"><span data-stu-id="9f7d9-142">If you are using the default routing conventions, the key parameter must be named "key".</span></span> <span data-ttu-id="9f7d9-143">También es importante incluir la **[FromOdataUri]** de atributo, como se muestra.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-143">It is also important to include the **[FromOdataUri]** attribute, as shown.</span></span> <span data-ttu-id="9f7d9-144">Este atributo indica a API Web para usar las reglas de sintaxis de OData cuando analiza la clave de URI de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-144">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

<span data-ttu-id="9f7d9-145">Use la *parámetros* diccionario para obtener los parámetros de acción:</span><span class="sxs-lookup"><span data-stu-id="9f7d9-145">Use the *parameters* dictionary to get the action parameters:</span></span>

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

<span data-ttu-id="9f7d9-146">Si el cliente envía los parámetros de acción en el valor correcto de formato, el valor de **ModelState.IsValid** es true.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-146">If the client sends the action parameters in the correct format, the value of **ModelState.IsValid** is true.</span></span> <span data-ttu-id="9f7d9-147">En ese caso, puede usar el **ODataActionParameters** diccionario para obtener los valores de parámetro.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-147">In that case, you can use the **ODataActionParameters** dictionary to get the parameter values.</span></span> <span data-ttu-id="9f7d9-148">En este ejemplo, el `RateProduct` acción toma un parámetro único denominado "Clasificación".</span><span class="sxs-lookup"><span data-stu-id="9f7d9-148">In this example, the `RateProduct` action takes a single parameter named "Rating".</span></span>

## <a name="action-metadata"></a><span data-ttu-id="9f7d9-149">Metadatos de acción</span><span class="sxs-lookup"><span data-stu-id="9f7d9-149">Action Metadata</span></span>

<span data-ttu-id="9f7d9-150">Para ver los metadatos del servicio, envía una solicitud GET a los metadatos de /odata/$.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-150">To view the service metadata, send a GET request to /odata/$metadata.</span></span> <span data-ttu-id="9f7d9-151">Esta es la parte de los metadatos que declara el `RateProduct` acción:</span><span class="sxs-lookup"><span data-stu-id="9f7d9-151">Here is the portion of the metadata that declares the `RateProduct` action:</span></span>

[!code-xml[Main](odata-actions/samples/sample7.xml)]

<span data-ttu-id="9f7d9-152">El **FunctionImport** elemento declara la acción.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-152">The **FunctionImport** element declares the action.</span></span> <span data-ttu-id="9f7d9-153">La mayoría de los campos es autoexplicativo, pero dos son cabe destacar:</span><span class="sxs-lookup"><span data-stu-id="9f7d9-153">Most of the fields are self-explanatory, but two are worth noting:</span></span>

- <span data-ttu-id="9f7d9-154">**IsBindable** significa que la acción se puede invocar en la entidad de destino, al menos parte del tiempo.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-154">**IsBindable** means the action can be invoked on the target entity, at least some of the time.</span></span>
- <span data-ttu-id="9f7d9-155">**IsAlwaysBindable** significa que siempre se puede invocar la acción en la entidad de destino.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-155">**IsAlwaysBindable** means the action can always be invoked on the target entity.</span></span>

<span data-ttu-id="9f7d9-156">La diferencia es que algunas acciones siempre están disponibles para los clientes, pero otras acciones podrían depender del estado de la entidad.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-156">The difference is that some actions are always available to clients, but other actions might depend on the state of the entity.</span></span> <span data-ttu-id="9f7d9-157">Por ejemplo, supongamos que define una acción de "Compra".</span><span class="sxs-lookup"><span data-stu-id="9f7d9-157">For example, suppose you define a "Purchase" action.</span></span> <span data-ttu-id="9f7d9-158">Solo puede adquirir un elemento que está en el almacén.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-158">You can only purchase an item that is in stock.</span></span> <span data-ttu-id="9f7d9-159">Si el elemento está agotado, un cliente no puede invocar la acción.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-159">If the item is out of stock, a client cannot invoke that action.</span></span>

<span data-ttu-id="9f7d9-160">Al definir el EDM, el **acción** método crea una acción siempre puede enlazar:</span><span class="sxs-lookup"><span data-stu-id="9f7d9-160">When you define the EDM, the **Action** method creates an always-bindable action:</span></span>

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

<span data-ttu-id="9f7d9-161">Explicaré acciones no siempre enlazable (también denominada *transitorio* acciones) más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-161">I'll talk about not-always-bindable actions (also called *transient* actions) later in this topic.</span></span>

## <a name="invoking-the-action"></a><span data-ttu-id="9f7d9-162">Invocar la acción</span><span class="sxs-lookup"><span data-stu-id="9f7d9-162">Invoking the Action</span></span>

<span data-ttu-id="9f7d9-163">Ahora veamos cómo un cliente invocaría esta acción.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-163">Now let's see how a client would invoke this action.</span></span> <span data-ttu-id="9f7d9-164">Suponga que el cliente desea proporcionar una clasificación de 2 para el producto con el identificador = 4.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-164">Suppose the client wants to give a rating of 2 to the product with ID = 4.</span></span> <span data-ttu-id="9f7d9-165">Este es un mensaje de solicitud de ejemplo, con formato JSON para el cuerpo de solicitud:</span><span class="sxs-lookup"><span data-stu-id="9f7d9-165">Here is an example request message, using JSON format for the request body:</span></span>

[!code-console[Main](odata-actions/samples/sample9.cmd)]

<span data-ttu-id="9f7d9-166">Este es el mensaje de respuesta:</span><span class="sxs-lookup"><span data-stu-id="9f7d9-166">Here is the response message:</span></span>

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a><span data-ttu-id="9f7d9-167">Enlazar una acción a un conjunto de entidades</span><span class="sxs-lookup"><span data-stu-id="9f7d9-167">Binding an Action to an Entity Set</span></span>

<span data-ttu-id="9f7d9-168">En el ejemplo anterior, la acción está enlazada a una entidad individual: el cliente clasifica un único producto.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-168">In the previous example, the action is bound to a single entity: The client rates a single product.</span></span> <span data-ttu-id="9f7d9-169">También puede enlazar una acción a una colección de entidades.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-169">You can also bind an action to a collection of entities.</span></span> <span data-ttu-id="9f7d9-170">Solo realice los cambios siguientes:</span><span class="sxs-lookup"><span data-stu-id="9f7d9-170">Just make the following changes:</span></span>

<span data-ttu-id="9f7d9-171">En el EDM, agregue la acción a la entidad **colección** propiedad.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-171">In the EDM, add the action to the entity's **Collection** property.</span></span>

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

<span data-ttu-id="9f7d9-172">En el método del controlador, omitir el *clave* parámetro.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-172">In the controller method, omit the *key* parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

<span data-ttu-id="9f7d9-173">Ahora el cliente invoca la acción en el conjunto de entidades de productos:</span><span class="sxs-lookup"><span data-stu-id="9f7d9-173">Now the client invokes the action on the Products entity set:</span></span>

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a><span data-ttu-id="9f7d9-174">Acciones con los parámetros de recopilación</span><span class="sxs-lookup"><span data-stu-id="9f7d9-174">Actions with Collection Parameters</span></span>

<span data-ttu-id="9f7d9-175">Las acciones pueden tener parámetros que toman una colección de valores.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-175">Actions can have parameters that take a collection of values.</span></span> <span data-ttu-id="9f7d9-176">En el EDM, use **CollectionParameter&lt;T&gt;**  para declarar el parámetro.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-176">In the EDM, use **CollectionParameter&lt;T&gt;** to declare the parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

<span data-ttu-id="9f7d9-177">Esto declara un parámetro denominado "Clasificaciones" que toma una colección de **int** valores.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-177">This declares a parameter named "Ratings" that takes a collection of **int** values.</span></span> <span data-ttu-id="9f7d9-178">En el método del controlador, obtendrá aún el valor del parámetro de la **ODataActionParameters** objeto, pero ahora el valor es un **ICollection&lt;int&gt;**  valor:</span><span class="sxs-lookup"><span data-stu-id="9f7d9-178">In the controller method, you still get the parameter value from the **ODataActionParameters** object, but now the value is an **ICollection&lt;int&gt;** value:</span></span>

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a><span data-ttu-id="9f7d9-179">Acciones transitorias</span><span class="sxs-lookup"><span data-stu-id="9f7d9-179">Transient Actions</span></span>

<span data-ttu-id="9f7d9-180">En el ejemplo "RateProduct", los usuarios siempre pueden clasificar un producto, por lo que la acción siempre está disponible.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-180">In the "RateProduct" example, users can always rate a product, so the action is always available.</span></span> <span data-ttu-id="9f7d9-181">Pero algunas acciones dependen del estado de la entidad.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-181">But some actions depend on the state of the entity.</span></span> <span data-ttu-id="9f7d9-182">Por ejemplo, en un servicio de alquiler de vídeo, la acción de "Extracción" no está siempre disponible.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-182">For example, in a video rental service, the "CheckOut" action is not always available.</span></span> <span data-ttu-id="9f7d9-183">(Depende si hay disponible una copia de dicho vídeo.) Este tipo de acción se denomina un *transitorio* acción.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-183">(It depends whether a copy of that video is available.) This type of action is called a *transient* action.</span></span>

<span data-ttu-id="9f7d9-184">En los metadatos del servicio, tiene una acción transitoria **IsAlwaysBindable** igual a false.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-184">In the service metadata, a transient action has **IsAlwaysBindable** equal to false.</span></span> <span data-ttu-id="9f7d9-185">Que es realmente el valor predeterminado, por lo que los metadatos tendrá un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="9f7d9-185">That's actually the default value, so the metadata will look like this:</span></span>

[!code-xml[Main](odata-actions/samples/sample16.xml)]

<span data-ttu-id="9f7d9-186">Aquí por qué esto es importante: si una acción es transitoria, el servidor debe indicar al cliente cuando la acción está disponible.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-186">Here's why this matters: If an action is transient, the server needs to tell the client when the action is available.</span></span> <span data-ttu-id="9f7d9-187">Para ello incluye un vínculo a la acción de la entidad.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-187">It does this by including a link to the action in the entity.</span></span> <span data-ttu-id="9f7d9-188">Este es un ejemplo de una entidad de película:</span><span class="sxs-lookup"><span data-stu-id="9f7d9-188">Here is an example for a Movie entity:</span></span>

[!code-console[Main](odata-actions/samples/sample17.cmd)]

<span data-ttu-id="9f7d9-189">La propiedad "#CheckOut" contiene un vínculo a la acción de extracción del repositorio.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-189">The "#CheckOut" property contains a link to the CheckOut action.</span></span> <span data-ttu-id="9f7d9-190">Si la acción no está disponible, el servidor omite el vínculo.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-190">If the action is not available, the server omits the link.</span></span>

<span data-ttu-id="9f7d9-191">Para declarar una acción transitoria en el EDM, llame a la **TransientAction** método:</span><span class="sxs-lookup"><span data-stu-id="9f7d9-191">To declare a transient action in the EDM, call the **TransientAction** method:</span></span>

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

<span data-ttu-id="9f7d9-192">Además, debe proporcionar una función que devuelve un vínculo de acción para una entidad dada.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-192">Also, you must provide a function that returns an action link for a given entity.</span></span> <span data-ttu-id="9f7d9-193">Establezca esta función mediante una llamada a **HasActionLink**.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-193">Set this function by calling **HasActionLink**.</span></span> <span data-ttu-id="9f7d9-194">Puede escribir la función como una expresión lambda:</span><span class="sxs-lookup"><span data-stu-id="9f7d9-194">You can write the function as a lambda expression:</span></span>

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

<span data-ttu-id="9f7d9-195">Si la acción está disponible, la expresión lambda devuelve un vínculo a la acción.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-195">If the action is available, the lambda expression returns a link to the action.</span></span> <span data-ttu-id="9f7d9-196">El serializador de OData incluye este vínculo cuando serializa la entidad.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-196">The OData serializer includes this link when it serializes the entity.</span></span> <span data-ttu-id="9f7d9-197">Cuando la acción no está disponible, la función devuelve `null`.</span><span class="sxs-lookup"><span data-stu-id="9f7d9-197">When the action is not available, the function returns `null`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9f7d9-198">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="9f7d9-198">Additional Resources</span></span>

[<span data-ttu-id="9f7d9-199">Ejemplo de acciones de OData</span><span class="sxs-lookup"><span data-stu-id="9f7d9-199">OData Actions Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
