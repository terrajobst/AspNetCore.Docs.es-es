---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Llamar a un servicio de OData desde un cliente .NET (C#) | Microsoft Docs
author: MikeWasson
description: Este tutorial muestra cómo llamar a un servicio de OData desde una aplicación cliente de C#. Versiones de software que se usa en el tutorial Visual Studio 2013 (funciona con Visual S...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: d987e7fbe737055b3e2b690ef3e8de5ca7e2b937
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369788"
---
<a name="calling-an-odata-service-from-a-net-client-c"></a><span data-ttu-id="24f08-104">Llamar a un servicio de OData desde un cliente .NET (C#)</span><span class="sxs-lookup"><span data-stu-id="24f08-104">Calling an OData Service From a .NET Client (C#)</span></span>
====================
<span data-ttu-id="24f08-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="24f08-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="24f08-106">Descargue el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="24f08-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="24f08-107">Este tutorial muestra cómo llamar a un servicio de OData desde una aplicación cliente de C#.</span><span class="sxs-lookup"><span data-stu-id="24f08-107">This tutorial shows how to call an OData service from a C# client application.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="24f08-108">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="24f08-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="24f08-109">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (funciona con Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="24f08-109">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (works with Visual Studio 2012)</span></span>
> - [<span data-ttu-id="24f08-110">Biblioteca cliente de Servicios de datos de WCF</span><span class="sxs-lookup"><span data-stu-id="24f08-110">WCF Data Services Client Library</span></span>](https://msdn.microsoft.com/library/cc668772.aspx)
> - <span data-ttu-id="24f08-111">Web API 2.</span><span class="sxs-lookup"><span data-stu-id="24f08-111">Web API 2.</span></span> <span data-ttu-id="24f08-112">(El servicio de OData de ejemplo se compila con Web API 2, pero la aplicación cliente no depende de la API Web).</span><span class="sxs-lookup"><span data-stu-id="24f08-112">(The example OData service is built using Web API 2, but the client application does not depend on Web API.)</span></span>


<span data-ttu-id="24f08-113">En este tutorial, lo guiaré a través de la creación de una aplicación cliente que llama a un servicio de OData.</span><span class="sxs-lookup"><span data-stu-id="24f08-113">In this tutorial, I'll walk through creating a client application that calls an OData service.</span></span> <span data-ttu-id="24f08-114">El servicio de OData expone las siguientes entidades:</span><span class="sxs-lookup"><span data-stu-id="24f08-114">The OData service exposes the following entities:</span></span>

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

<span data-ttu-id="24f08-115">Los artículos siguientes describen cómo implementar el servicio de OData en Web API.</span><span class="sxs-lookup"><span data-stu-id="24f08-115">The following articles describe how to implement the OData service in Web API.</span></span> <span data-ttu-id="24f08-116">(No necesita leerlos para entender este tutorial, sin embargo).</span><span class="sxs-lookup"><span data-stu-id="24f08-116">(You don't need to read them to understand this tutorial, however.)</span></span>

- [<span data-ttu-id="24f08-117">Creación de un extremo de OData en Web API 2</span><span class="sxs-lookup"><span data-stu-id="24f08-117">Creating an OData Endpoint in Web API 2</span></span>](creating-an-odata-endpoint.md)
- [<span data-ttu-id="24f08-118">Relaciones de entidad de OData en Web API 2</span><span class="sxs-lookup"><span data-stu-id="24f08-118">OData Entity Relations in Web API 2</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="24f08-119">Acciones de OData en Web API 2</span><span class="sxs-lookup"><span data-stu-id="24f08-119">OData Actions in Web API 2</span></span>](odata-actions.md)

## <a name="generate-the-service-proxy"></a><span data-ttu-id="24f08-120">Generar al Proxy de servicio</span><span class="sxs-lookup"><span data-stu-id="24f08-120">Generate the Service Proxy</span></span>

<span data-ttu-id="24f08-121">El primer paso es generar a un proxy de servicio.</span><span class="sxs-lookup"><span data-stu-id="24f08-121">The first step is to generate a service proxy.</span></span> <span data-ttu-id="24f08-122">El proxy de servicio es una clase .NET que define los métodos para acceder al servicio de OData.</span><span class="sxs-lookup"><span data-stu-id="24f08-122">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="24f08-123">El proxy traduce llamadas de método en las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="24f08-123">The proxy translates method calls into HTTP requests.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

<span data-ttu-id="24f08-124">Comience abriendo el proyecto de servicio de OData en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="24f08-124">Start by opening the OData service project in Visual Studio.</span></span> <span data-ttu-id="24f08-125">Presione CTRL + F5 para ejecutar el servicio localmente en IIS Express.</span><span class="sxs-lookup"><span data-stu-id="24f08-125">Press CTRL+F5 to run the service locally in IIS Express.</span></span> <span data-ttu-id="24f08-126">Tenga en cuenta la dirección local, incluido el número de puerto que Visual Studio asigna.</span><span class="sxs-lookup"><span data-stu-id="24f08-126">Note the local address, including the port number that Visual Studio assigns.</span></span> <span data-ttu-id="24f08-127">Necesitará esta dirección cuando se crea el proxy.</span><span class="sxs-lookup"><span data-stu-id="24f08-127">You will need this address when you create the proxy.</span></span>

<span data-ttu-id="24f08-128">A continuación, abra otra instancia de Visual Studio y cree un proyecto de aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="24f08-128">Next, open another instance of Visual Studio and create a console application project.</span></span> <span data-ttu-id="24f08-129">La aplicación de consola será nuestra aplicación de cliente de OData.</span><span class="sxs-lookup"><span data-stu-id="24f08-129">The console application will be our OData client application.</span></span> <span data-ttu-id="24f08-130">(También puede agregar el proyecto a la misma solución que el servicio.)</span><span class="sxs-lookup"><span data-stu-id="24f08-130">(You can also add the project to the same solution as the service.)</span></span>

> [!NOTE]
> <span data-ttu-id="24f08-131">El proyecto de consola, consulte los pasos restantes.</span><span class="sxs-lookup"><span data-stu-id="24f08-131">The remaining steps refer the console project.</span></span>


<span data-ttu-id="24f08-132">En el Explorador de soluciones, haga clic en **referencias** y seleccione **Add Service Reference**.</span><span class="sxs-lookup"><span data-stu-id="24f08-132">In Solution Explorer, right-click **References** and select **Add Service Reference**.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

<span data-ttu-id="24f08-133">En el **Add Service Reference** cuadro de diálogo, escriba la dirección del servicio OData:</span><span class="sxs-lookup"><span data-stu-id="24f08-133">In the **Add Service Reference** dialog, type the address of the OData service:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

<span data-ttu-id="24f08-134">donde *puerto* es el número de puerto.</span><span class="sxs-lookup"><span data-stu-id="24f08-134">where *port* is the port number.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

<span data-ttu-id="24f08-135">Para **Namespace**, escriba "ProductService".</span><span class="sxs-lookup"><span data-stu-id="24f08-135">For **Namespace**, type "ProductService".</span></span> <span data-ttu-id="24f08-136">Esta opción define el espacio de nombres de la clase de proxy.</span><span class="sxs-lookup"><span data-stu-id="24f08-136">This option defines the namespace of the proxy class.</span></span>

<span data-ttu-id="24f08-137">Haga clic en **Ir**.</span><span class="sxs-lookup"><span data-stu-id="24f08-137">Click **Go**.</span></span> <span data-ttu-id="24f08-138">Visual Studio lee el documento de metadatos de OData para detectar las entidades en el servicio.</span><span class="sxs-lookup"><span data-stu-id="24f08-138">Visual Studio reads the OData metadata document to discover the entities in the service.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

<span data-ttu-id="24f08-139">Haga clic en **Aceptar** para agregar la clase de proxy a su proyecto.</span><span class="sxs-lookup"><span data-stu-id="24f08-139">Click **OK** to add the proxy class to your project.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a><span data-ttu-id="24f08-140">Cree una instancia de la clase de Proxy de servicio</span><span class="sxs-lookup"><span data-stu-id="24f08-140">Create an Instance of the Service Proxy Class</span></span>

<span data-ttu-id="24f08-141">Dentro de su `Main` método, cree una nueva instancia de la clase de proxy, como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="24f08-141">Inside your `Main` method, create a new instance of the proxy class, as follows:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

<span data-ttu-id="24f08-142">De nuevo, use el número de puerto real donde se está ejecutando el servicio.</span><span class="sxs-lookup"><span data-stu-id="24f08-142">Again, use the actual port number where your service is running.</span></span> <span data-ttu-id="24f08-143">Al implementar el servicio, usará el URI del servicio en directo.</span><span class="sxs-lookup"><span data-stu-id="24f08-143">When you deploy your service, you will use the URI of the live service.</span></span> <span data-ttu-id="24f08-144">No es necesario actualizar al servidor proxy.</span><span class="sxs-lookup"><span data-stu-id="24f08-144">You don't need to update the proxy.</span></span>

<span data-ttu-id="24f08-145">El código siguiente agrega un controlador de eventos que imprime los URI de solicitud a la ventana de consola.</span><span class="sxs-lookup"><span data-stu-id="24f08-145">The following code adds an event handler that prints the request URIs to the console window.</span></span> <span data-ttu-id="24f08-146">Este paso no es necesario, pero resulta interesante ver a los URI para cada consulta.</span><span class="sxs-lookup"><span data-stu-id="24f08-146">This step isn't required, but it's interesting to see the URIs for each query.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a><span data-ttu-id="24f08-147">Consultar el servicio</span><span class="sxs-lookup"><span data-stu-id="24f08-147">Query the Service</span></span>

<span data-ttu-id="24f08-148">El código siguiente obtiene la lista de productos desde el servicio de OData.</span><span class="sxs-lookup"><span data-stu-id="24f08-148">The following code gets the list of products from the OData service.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

<span data-ttu-id="24f08-149">Tenga en cuenta que no es necesario escribir ningún código para enviar la solicitud HTTP o analizar la respuesta.</span><span class="sxs-lookup"><span data-stu-id="24f08-149">Notice that you don't need to write any code to send the HTTP request or parse the response.</span></span> <span data-ttu-id="24f08-150">La clase de proxy encarga automáticamente al enumerar los `Container.Products` colección en el **foreach** bucle.</span><span class="sxs-lookup"><span data-stu-id="24f08-150">The proxy class does this automatically when you enumerate the `Container.Products` collection in the **foreach** loop.</span></span>

<span data-ttu-id="24f08-151">Al ejecutar la aplicación, el resultado debe ser similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="24f08-151">When you run the application, the output should look like the following:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

<span data-ttu-id="24f08-152">Para obtener una entidad por identificador, use un `where` cláusula.</span><span class="sxs-lookup"><span data-stu-id="24f08-152">To get an entity by ID, use a `where` clause.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

<span data-ttu-id="24f08-153">Para el resto de este tema, que no se muestra todo el `Main` funcione, solo el código necesario para llamar al servicio.</span><span class="sxs-lookup"><span data-stu-id="24f08-153">For the rest of this topic, I won't show the entire `Main` function, just the code needed to call the service.</span></span>

## <a name="apply-query-options"></a><span data-ttu-id="24f08-154">Aplicar las opciones de consulta</span><span class="sxs-lookup"><span data-stu-id="24f08-154">Apply Query Options</span></span>

<span data-ttu-id="24f08-155">OData define [opciones de consulta](../supporting-odata-query-options.md) que se puede utilizar para filtrar, ordenar, paginar los datos y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="24f08-155">OData defines [query options](../supporting-odata-query-options.md) that can be used to filter, sort, page data, and so forth.</span></span> <span data-ttu-id="24f08-156">En el proxy de servicio, puede aplicar estas opciones mediante el uso de varias expresiones de LINQ.</span><span class="sxs-lookup"><span data-stu-id="24f08-156">In the service proxy, you can apply these options by using various LINQ expressions.</span></span>

<span data-ttu-id="24f08-157">En esta sección, le mostraré ejemplos breves.</span><span class="sxs-lookup"><span data-stu-id="24f08-157">In this section, I'll show brief examples.</span></span> <span data-ttu-id="24f08-158">Para obtener más información, vea el tema [consideraciones sobre LINQ (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) en MSDN.</span><span class="sxs-lookup"><span data-stu-id="24f08-158">For more details, see the topic [LINQ Considerations (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) on MSDN.</span></span>

### <a name="filtering-filter"></a><span data-ttu-id="24f08-159">Filtrado ($filter)</span><span class="sxs-lookup"><span data-stu-id="24f08-159">Filtering ($filter)</span></span>

<span data-ttu-id="24f08-160">Para filtrar, use un `where` cláusula.</span><span class="sxs-lookup"><span data-stu-id="24f08-160">To filter, use a `where` clause.</span></span> <span data-ttu-id="24f08-161">El ejemplo siguiente se filtra por categoría de producto.</span><span class="sxs-lookup"><span data-stu-id="24f08-161">The following example filters by product category.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

<span data-ttu-id="24f08-162">Este código corresponde a la siguiente consulta de OData.</span><span class="sxs-lookup"><span data-stu-id="24f08-162">This code corresponds to the following OData query.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

<span data-ttu-id="24f08-163">Tenga en cuenta que el proxy convierte la `where` cláusula en una OData `$filter` expresión.</span><span class="sxs-lookup"><span data-stu-id="24f08-163">Notice that the proxy converts the `where` clause into an OData `$filter` expression.</span></span>

### <a name="sorting-orderby"></a><span data-ttu-id="24f08-164">Ordenación ($orderby)</span><span class="sxs-lookup"><span data-stu-id="24f08-164">Sorting ($orderby)</span></span>

<span data-ttu-id="24f08-165">Para ordenar, use un `orderby` cláusula.</span><span class="sxs-lookup"><span data-stu-id="24f08-165">To sort, use an `orderby` clause.</span></span> <span data-ttu-id="24f08-166">El ejemplo siguiente se ordena por el precio de mayor a menor.</span><span class="sxs-lookup"><span data-stu-id="24f08-166">The following example sorts by price, from highest to lowest.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

<span data-ttu-id="24f08-167">Esta es la solicitud de OData correspondiente.</span><span class="sxs-lookup"><span data-stu-id="24f08-167">Here is the corresponding OData request.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a><span data-ttu-id="24f08-168">Paginación del cliente ($skip y $top)</span><span class="sxs-lookup"><span data-stu-id="24f08-168">Client-Side Paging ($skip and $top)</span></span>

<span data-ttu-id="24f08-169">Para los conjuntos de entidades de gran tamaño, el cliente podría limitar el número de resultados.</span><span class="sxs-lookup"><span data-stu-id="24f08-169">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="24f08-170">Por ejemplo, un cliente podría mostrar 10 entradas a la vez.</span><span class="sxs-lookup"><span data-stu-id="24f08-170">For example, a client might show 10 entries at a time.</span></span> <span data-ttu-id="24f08-171">Esto se denomina *paginación del lado cliente*.</span><span class="sxs-lookup"><span data-stu-id="24f08-171">This is called *client-side paging*.</span></span> <span data-ttu-id="24f08-172">(También hay [paginación del lado servidor](../supporting-odata-query-options.md#server-paging), donde el servidor limita el número de resultados.) Para realizar la paginación del lado cliente, use el LINQ **omitir** y **tomar** métodos.</span><span class="sxs-lookup"><span data-stu-id="24f08-172">(There is also [server-side paging](../supporting-odata-query-options.md#server-paging), where the server limits the number of results.) To perform client-side paging, use the LINQ **Skip** and **Take** methods.</span></span> <span data-ttu-id="24f08-173">El ejemplo siguiente omite los primeros 40 resultados y toma los 10 siguientes.</span><span class="sxs-lookup"><span data-stu-id="24f08-173">The following example skips the first 40 results and takes the next 10.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

<span data-ttu-id="24f08-174">Esta es la solicitud de OData correspondiente:</span><span class="sxs-lookup"><span data-stu-id="24f08-174">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a><span data-ttu-id="24f08-175">Select ($select) y expandir ($expand)</span><span class="sxs-lookup"><span data-stu-id="24f08-175">Select ($select) and Expand ($expand)</span></span>

<span data-ttu-id="24f08-176">Para incluir las entidades relacionadas, use el `DataServiceQuery<t>.Expand` método.</span><span class="sxs-lookup"><span data-stu-id="24f08-176">To include related entities, use the `DataServiceQuery<t>.Expand` method.</span></span> <span data-ttu-id="24f08-177">Por ejemplo, para incluir la `Supplier` para cada `Product`:</span><span class="sxs-lookup"><span data-stu-id="24f08-177">For example, to include the `Supplier` for each `Product`:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

<span data-ttu-id="24f08-178">Esta es la solicitud de OData correspondiente:</span><span class="sxs-lookup"><span data-stu-id="24f08-178">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

<span data-ttu-id="24f08-179">Para cambiar la forma de la respuesta, use el LINQ **seleccione** cláusula.</span><span class="sxs-lookup"><span data-stu-id="24f08-179">To change the shape of the response, use the LINQ **select** clause.</span></span> <span data-ttu-id="24f08-180">El ejemplo siguiente obtiene solo el nombre de cada producto, con ninguna otra propiedad.</span><span class="sxs-lookup"><span data-stu-id="24f08-180">The following example gets just the name of each product, with no other properties.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

<span data-ttu-id="24f08-181">Esta es la solicitud de OData correspondiente:</span><span class="sxs-lookup"><span data-stu-id="24f08-181">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

<span data-ttu-id="24f08-182">Una cláusula select puede incluir las entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="24f08-182">A select clause can include related entities.</span></span> <span data-ttu-id="24f08-183">En ese caso, no llame a **expandir**; el proxy incluye automáticamente la expansión en este caso.</span><span class="sxs-lookup"><span data-stu-id="24f08-183">In that case, do not call **Expand**; the proxy automatically includes the expansion in this case.</span></span> <span data-ttu-id="24f08-184">En el ejemplo siguiente se obtiene el nombre y el proveedor de cada producto.</span><span class="sxs-lookup"><span data-stu-id="24f08-184">The following example gets the name and supplier of each product.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

<span data-ttu-id="24f08-185">Esta es la solicitud de OData correspondiente.</span><span class="sxs-lookup"><span data-stu-id="24f08-185">Here is the corresponding OData request.</span></span> <span data-ttu-id="24f08-186">Tenga en cuenta que incluye la **$expand** opción.</span><span class="sxs-lookup"><span data-stu-id="24f08-186">Notice that it includes the **$expand** option.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

<span data-ttu-id="24f08-187">Para obtener más información acerca de $select y $expand, expanda, consulte [usar $select, $expand y $value en Web API 2](../using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="24f08-187">For more information about $select and $expand, see [Using $select, $expand, and $value in Web API 2](../using-select-expand-and-value.md).</span></span>

## <a name="add-a-new-entity"></a><span data-ttu-id="24f08-188">Agregar una nueva entidad</span><span class="sxs-lookup"><span data-stu-id="24f08-188">Add a New Entity</span></span>

<span data-ttu-id="24f08-189">Para agregar una nueva entidad a un conjunto de entidades, llame `AddToEntitySet`, donde *EntitySet* es el nombre del conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="24f08-189">To add a new entity to an entity set, call `AddToEntitySet`, where *EntitySet* is the name of the entity set.</span></span> <span data-ttu-id="24f08-190">Por ejemplo, `AddToProducts` agrega un nuevo `Product` a la `Products` conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="24f08-190">For example, `AddToProducts` adds a new `Product` to the `Products` entity set.</span></span> <span data-ttu-id="24f08-191">Al generar el proxy, WCF Data Services crea automáticamente estos fuertemente tipado **AddTo** métodos.</span><span class="sxs-lookup"><span data-stu-id="24f08-191">When you generate the proxy, WCF Data Services automatically creates these strongly-typed **AddTo** methods.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

<span data-ttu-id="24f08-192">Para agregar un vínculo entre dos entidades, utilice el **AddLink** y **SetLink** métodos.</span><span class="sxs-lookup"><span data-stu-id="24f08-192">To add a link between two entities, use the **AddLink** and **SetLink** methods.</span></span> <span data-ttu-id="24f08-193">El código siguiente agrega un nuevo proveedor y un nuevo producto y, a continuación, crea vínculos entre ellos.</span><span class="sxs-lookup"><span data-stu-id="24f08-193">The following code adds a new supplier and a new product, and then creates links between them.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

<span data-ttu-id="24f08-194">Use **AddLink** cuando la propiedad de navegación es una colección.</span><span class="sxs-lookup"><span data-stu-id="24f08-194">Use **AddLink** when the navigation property is a collection.</span></span> <span data-ttu-id="24f08-195">En este ejemplo, estamos agregando un producto para el `Products` colección en el proveedor.</span><span class="sxs-lookup"><span data-stu-id="24f08-195">In this example, we are adding a product to the `Products` collection on the supplier.</span></span>

<span data-ttu-id="24f08-196">Use **SetLink** cuando la propiedad de navegación es una entidad única.</span><span class="sxs-lookup"><span data-stu-id="24f08-196">Use **SetLink** when the navigation property is a single entity.</span></span> <span data-ttu-id="24f08-197">En este ejemplo, estamos estableciendo el `Supplier` propiedad sobre el producto.</span><span class="sxs-lookup"><span data-stu-id="24f08-197">In this example, we are setting the `Supplier` property on the product.</span></span>

## <a name="update--patch"></a><span data-ttu-id="24f08-198">Actualizar / aplicar revisiones</span><span class="sxs-lookup"><span data-stu-id="24f08-198">Update / Patch</span></span>

<span data-ttu-id="24f08-199">Para actualizar una entidad, llame a la **UpdateObject** método.</span><span class="sxs-lookup"><span data-stu-id="24f08-199">To update an entity, call the **UpdateObject** method.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

<span data-ttu-id="24f08-200">La actualización se realiza cuando se llama a **SaveChanges**.</span><span class="sxs-lookup"><span data-stu-id="24f08-200">The update is performed when you call **SaveChanges**.</span></span> <span data-ttu-id="24f08-201">De forma predeterminada, WCF envía una solicitud HTTP MERGE.</span><span class="sxs-lookup"><span data-stu-id="24f08-201">By default, WCF sends an HTTP MERGE request.</span></span> <span data-ttu-id="24f08-202">El **PatchOnUpdate** opción indica a WCF para enviar un HTTP PATCH en su lugar.</span><span class="sxs-lookup"><span data-stu-id="24f08-202">The **PatchOnUpdate** option tells WCF to send an HTTP PATCH instead.</span></span>

> [!NOTE]
> <span data-ttu-id="24f08-203">¿Por qué revisiones frente a la combinación?</span><span class="sxs-lookup"><span data-stu-id="24f08-203">Why PATCH versus MERGE?</span></span> <span data-ttu-id="24f08-204">La especificación HTTP 1.1 original ([RCF 2616](http://tools.ietf.org/html/rfc2616)) no se definió ningún método HTTP con la semántica de "actualización parcial".</span><span class="sxs-lookup"><span data-stu-id="24f08-204">The original HTTP 1.1 specification ([RCF 2616](http://tools.ietf.org/html/rfc2616)) did not define any HTTP method with "partial update" semantics.</span></span> <span data-ttu-id="24f08-205">Para admitir actualizaciones parciales, la especificación OData define el método MERGE.</span><span class="sxs-lookup"><span data-stu-id="24f08-205">To support partial updates, the OData specification defined the MERGE method.</span></span> <span data-ttu-id="24f08-206">En 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) definido por el método de revisión para las actualizaciones parciales.</span><span class="sxs-lookup"><span data-stu-id="24f08-206">In 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) defined the PATCH method for partial updates.</span></span> <span data-ttu-id="24f08-207">Puede leer algunos del historial de este [entrada de blog](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) en el Blog de WCF Data Services.</span><span class="sxs-lookup"><span data-stu-id="24f08-207">You can read some of the history in this [blog post](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) on the WCF Data Services Blog.</span></span> <span data-ttu-id="24f08-208">Hoy en día, la revisión es preferible a combinar.</span><span class="sxs-lookup"><span data-stu-id="24f08-208">Today, PATCH is preferred over MERGE.</span></span> <span data-ttu-id="24f08-209">El controlador de OData creado por el scaffolding de Web API es compatible con ambos métodos.</span><span class="sxs-lookup"><span data-stu-id="24f08-209">The OData controller created by the Web API scaffolding supports both methods.</span></span>


<span data-ttu-id="24f08-210">Si desea reemplazar toda la entidad (semántica PUT), especifique el **ReplaceOnUpdate** opción.</span><span class="sxs-lookup"><span data-stu-id="24f08-210">If you want to replace the entire entity (PUT semantics), specify the **ReplaceOnUpdate** option.</span></span> <span data-ttu-id="24f08-211">Esto hace que WCF enviar una solicitud HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="24f08-211">This causes WCF to send an HTTP PUT request.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a><span data-ttu-id="24f08-212">Eliminar una entidad</span><span class="sxs-lookup"><span data-stu-id="24f08-212">Delete an Entity</span></span>

<span data-ttu-id="24f08-213">Para eliminar una entidad, llame a **DeleteObject**.</span><span class="sxs-lookup"><span data-stu-id="24f08-213">To delete an entity, call **DeleteObject**.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a><span data-ttu-id="24f08-214">Invocar una acción de OData</span><span class="sxs-lookup"><span data-stu-id="24f08-214">Invoke an OData Action</span></span>

<span data-ttu-id="24f08-215">En OData, [acciones](odata-actions.md) son una forma de agregar comportamientos de lado servidor que no se definen fácilmente como operaciones CRUD en entidades.</span><span class="sxs-lookup"><span data-stu-id="24f08-215">In OData, [actions](odata-actions.md) are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span>

<span data-ttu-id="24f08-216">Aunque el documento de metadatos de OData describe las acciones, la clase de proxy no crea ningún método fuertemente tipado para ellos.</span><span class="sxs-lookup"><span data-stu-id="24f08-216">Although the OData metadata document describes the actions, the proxy class does not create any strongly-typed methods for them.</span></span> <span data-ttu-id="24f08-217">Se puede invocar una acción de OData mediante el uso genérico **Execute** método.</span><span class="sxs-lookup"><span data-stu-id="24f08-217">You can still invoke an OData action by using the generic **Execute** method.</span></span> <span data-ttu-id="24f08-218">Sin embargo, deberá conocer los tipos de datos de los parámetros y el valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="24f08-218">However, you will need to know the data types of the parameters and the return value.</span></span>

<span data-ttu-id="24f08-219">Por ejemplo, el `RateProduct` acción toma el parámetro denominado "Rating" de tipo `Int32` y devuelve un `double`.</span><span class="sxs-lookup"><span data-stu-id="24f08-219">For example, the `RateProduct` action takes parameter named "Rating" of type `Int32` and returns a `double`.</span></span> <span data-ttu-id="24f08-220">El código siguiente muestra cómo invocar esta acción.</span><span class="sxs-lookup"><span data-stu-id="24f08-220">The following code shows how to invoke this action.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

<span data-ttu-id="24f08-221">Para obtener más información, consulte[llamar a operaciones de servicio y las acciones](https://msdn.microsoft.com/library/hh230677.aspx).</span><span class="sxs-lookup"><span data-stu-id="24f08-221">For more information, see[Calling Service Operations and Actions](https://msdn.microsoft.com/library/hh230677.aspx).</span></span>

<span data-ttu-id="24f08-222">Es una opción ampliar la **contenedor** clase para proporcionar un método fuertemente tipado que invoca la acción:</span><span class="sxs-lookup"><span data-stu-id="24f08-222">One option is to extend the **Container** class to provide a strongly typed method that invokes the action:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
