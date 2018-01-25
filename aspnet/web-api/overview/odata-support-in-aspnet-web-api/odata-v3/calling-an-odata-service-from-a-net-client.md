---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Llamar a un servicio de OData desde un cliente .NET (C#) | Documentos de Microsoft
author: MikeWasson
description: "Este tutorial muestra cómo llamar a un servicio de OData desde una aplicación de cliente de C#. Versiones de software que se usa en el tutorial Visual Studio 2013 (funciona con Visual S..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 497102cfa98680f2156a56ff9e36d84b7c820020
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="calling-an-odata-service-from-a-net-client-c"></a><span data-ttu-id="ee8d8-104">Llamar a un servicio de OData desde un cliente .NET (C#)</span><span class="sxs-lookup"><span data-stu-id="ee8d8-104">Calling an OData Service From a .NET Client (C#)</span></span>
====================
<span data-ttu-id="ee8d8-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ee8d8-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ee8d8-106">Descargar el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="ee8d8-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="ee8d8-107">Este tutorial muestra cómo llamar a un servicio de OData desde una aplicación de cliente de C#.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-107">This tutorial shows how to call an OData service from a C# client application.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ee8d8-108">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="ee8d8-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="ee8d8-109">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (funciona con Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="ee8d8-109">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (works with Visual Studio 2012)</span></span>
> - [<span data-ttu-id="ee8d8-110">Biblioteca cliente de Servicios de datos de WCF</span><span class="sxs-lookup"><span data-stu-id="ee8d8-110">WCF Data Services Client Library</span></span>](https://msdn.microsoft.com/library/cc668772.aspx)
> - <span data-ttu-id="ee8d8-111">Web API 2.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-111">Web API 2.</span></span> <span data-ttu-id="ee8d8-112">(El servicio de OData de ejemplo se compila mediante la API Web 2, pero la aplicación cliente no depende de la API Web).</span><span class="sxs-lookup"><span data-stu-id="ee8d8-112">(The example OData service is built using Web API 2, but the client application does not depend on Web API.)</span></span>


<span data-ttu-id="ee8d8-113">En este tutorial, le guían por la creación de una aplicación cliente que llama a un servicio de OData.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-113">In this tutorial, I'll walk through creating a client application that calls an OData service.</span></span> <span data-ttu-id="ee8d8-114">El servicio de OData expone las siguientes entidades:</span><span class="sxs-lookup"><span data-stu-id="ee8d8-114">The OData service exposes the following entities:</span></span>

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

<span data-ttu-id="ee8d8-115">Los artículos siguientes describen cómo implementar el servicio de OData en Web API.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-115">The following articles describe how to implement the OData service in Web API.</span></span> <span data-ttu-id="ee8d8-116">(No necesita leyó para entender este tutorial, sin embargo).</span><span class="sxs-lookup"><span data-stu-id="ee8d8-116">(You don't need to read them to understand this tutorial, however.)</span></span>

- [<span data-ttu-id="ee8d8-117">Creación de un extremo de OData en Web API 2</span><span class="sxs-lookup"><span data-stu-id="ee8d8-117">Creating an OData Endpoint in Web API 2</span></span>](creating-an-odata-endpoint.md)
- [<span data-ttu-id="ee8d8-118">Relaciones de entidad de OData en Web API 2</span><span class="sxs-lookup"><span data-stu-id="ee8d8-118">OData Entity Relations in Web API 2</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="ee8d8-119">Acciones de OData en Web API 2</span><span class="sxs-lookup"><span data-stu-id="ee8d8-119">OData Actions in Web API 2</span></span>](odata-actions.md)

## <a name="generate-the-service-proxy"></a><span data-ttu-id="ee8d8-120">Generar al Proxy de servicio</span><span class="sxs-lookup"><span data-stu-id="ee8d8-120">Generate the Service Proxy</span></span>

<span data-ttu-id="ee8d8-121">El primer paso consiste en generar a un proxy de servicio.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-121">The first step is to generate a service proxy.</span></span> <span data-ttu-id="ee8d8-122">El proxy de servicio es una clase .NET que define los métodos para tener acceso al servicio de OData.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-122">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="ee8d8-123">El proxy convierte las llamadas de método en las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-123">The proxy translates method calls into HTTP requests.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

<span data-ttu-id="ee8d8-124">Primer lugar, abra el proyecto de servicio de OData en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-124">Start by opening the OData service project in Visual Studio.</span></span> <span data-ttu-id="ee8d8-125">Presione CTRL + F5 para ejecutar el servicio localmente en IIS Express.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-125">Press CTRL+F5 to run the service locally in IIS Express.</span></span> <span data-ttu-id="ee8d8-126">Tenga en cuenta la dirección local, incluido el número de puerto que Visual Studio asigna.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-126">Note the local address, including the port number that Visual Studio assigns.</span></span> <span data-ttu-id="ee8d8-127">Necesitará esta dirección cuando se crea el proxy.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-127">You will need this address when you create the proxy.</span></span>

<span data-ttu-id="ee8d8-128">A continuación, abra otra instancia de Visual Studio y cree un proyecto de aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-128">Next, open another instance of Visual Studio and create a console application project.</span></span> <span data-ttu-id="ee8d8-129">La aplicación de consola será nuestra aplicación de cliente de OData.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-129">The console application will be our OData client application.</span></span> <span data-ttu-id="ee8d8-130">(También puede agregar el proyecto a la misma solución que el servicio.)</span><span class="sxs-lookup"><span data-stu-id="ee8d8-130">(You can also add the project to the same solution as the service.)</span></span>

> [!NOTE]
> <span data-ttu-id="ee8d8-131">El proyecto de consola, consulte los pasos restantes.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-131">The remaining steps refer the console project.</span></span>


<span data-ttu-id="ee8d8-132">En el Explorador de soluciones, haga clic en **referencias** y seleccione **Agregar referencia de servicio**.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-132">In Solution Explorer, right-click **References** and select **Add Service Reference**.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

<span data-ttu-id="ee8d8-133">En el **Agregar referencia de servicio** cuadro de diálogo, escriba la dirección del servicio OData:</span><span class="sxs-lookup"><span data-stu-id="ee8d8-133">In the **Add Service Reference** dialog, type the address of the OData service:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

<span data-ttu-id="ee8d8-134">donde *puerto* es el número de puerto.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-134">where *port* is the port number.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

<span data-ttu-id="ee8d8-135">Para **Namespace**, escriba "ProductService".</span><span class="sxs-lookup"><span data-stu-id="ee8d8-135">For **Namespace**, type "ProductService".</span></span> <span data-ttu-id="ee8d8-136">Esta opción define el espacio de nombres de la clase de proxy.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-136">This option defines the namespace of the proxy class.</span></span>

<span data-ttu-id="ee8d8-137">Haga clic en **Ir**.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-137">Click **Go**.</span></span> <span data-ttu-id="ee8d8-138">Visual Studio lee el documento de metadatos de OData para detectar las entidades en el servicio.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-138">Visual Studio reads the OData metadata document to discover the entities in the service.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

<span data-ttu-id="ee8d8-139">Haga clic en **Aceptar** para agregar la clase de proxy para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-139">Click **OK** to add the proxy class to your project.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a><span data-ttu-id="ee8d8-140">Cree una instancia de la clase de Proxy de servicio</span><span class="sxs-lookup"><span data-stu-id="ee8d8-140">Create an Instance of the Service Proxy Class</span></span>

<span data-ttu-id="ee8d8-141">Dentro de su `Main` (método), crear una nueva instancia de la clase de proxy, como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="ee8d8-141">Inside your `Main` method, create a new instance of the proxy class, as follows:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

<span data-ttu-id="ee8d8-142">De nuevo, use el número de puerto real donde se está ejecutando el servicio.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-142">Again, use the actual port number where your service is running.</span></span> <span data-ttu-id="ee8d8-143">Al implementar su servicio, usará el URI del servicio en vivo.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-143">When you deploy your service, you will use the URI of the live service.</span></span> <span data-ttu-id="ee8d8-144">No es necesario actualizar al servidor proxy.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-144">You don't need to update the proxy.</span></span>

<span data-ttu-id="ee8d8-145">El código siguiente agrega un controlador de eventos que imprime los URI de solicitud a la ventana de consola.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-145">The following code adds an event handler that prints the request URIs to the console window.</span></span> <span data-ttu-id="ee8d8-146">Este paso no es necesario, pero es interesante ver a los URI para cada consulta.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-146">This step isn't required, but it's interesting to see the URIs for each query.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a><span data-ttu-id="ee8d8-147">Consultar al servicio</span><span class="sxs-lookup"><span data-stu-id="ee8d8-147">Query the Service</span></span>

<span data-ttu-id="ee8d8-148">El código siguiente obtiene la lista de productos del servicio OData.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-148">The following code gets the list of products from the OData service.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

<span data-ttu-id="ee8d8-149">Tenga en cuenta que no es necesario escribir ningún código para enviar la solicitud HTTP o analizar la respuesta.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-149">Notice that you don't need to write any code to send the HTTP request or parse the response.</span></span> <span data-ttu-id="ee8d8-150">La clase de proxy encarga automáticamente al enumerar las `Container.Products` colección en la **foreach** bucle.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-150">The proxy class does this automatically when you enumerate the `Container.Products` collection in the **foreach** loop.</span></span>

<span data-ttu-id="ee8d8-151">Al ejecutar la aplicación, el resultado debería ser similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="ee8d8-151">When you run the application, the output should look like the following:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

<span data-ttu-id="ee8d8-152">Para obtener una entidad por identificador, use un `where` cláusula.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-152">To get an entity by ID, use a `where` clause.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

<span data-ttu-id="ee8d8-153">Para el resto de este tema, no mostrar toda la matriz `Main` funcione, solo el código necesario para llamar al servicio.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-153">For the rest of this topic, I won't show the entire `Main` function, just the code needed to call the service.</span></span>

## <a name="apply-query-options"></a><span data-ttu-id="ee8d8-154">Aplicar opciones de consulta</span><span class="sxs-lookup"><span data-stu-id="ee8d8-154">Apply Query Options</span></span>

<span data-ttu-id="ee8d8-155">OData define [opciones de consulta](../supporting-odata-query-options.md) que se puede utilizar para filtrar, ordenar, datos de la página y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-155">OData defines [query options](../supporting-odata-query-options.md) that can be used to filter, sort, page data, and so forth.</span></span> <span data-ttu-id="ee8d8-156">En el proxy de servicio, puede aplicar estas opciones mediante el uso de varias expresiones de LINQ.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-156">In the service proxy, you can apply these options by using various LINQ expressions.</span></span>

<span data-ttu-id="ee8d8-157">En esta sección, voy a explicar ejemplos breves.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-157">In this section, I'll show brief examples.</span></span> <span data-ttu-id="ee8d8-158">Para obtener más información, vea el tema [consideraciones sobre LINQ (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) en MSDN.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-158">For more details, see the topic [LINQ Considerations (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) on MSDN.</span></span>

### <a name="filtering-filter"></a><span data-ttu-id="ee8d8-159">Filtrado ($filter)</span><span class="sxs-lookup"><span data-stu-id="ee8d8-159">Filtering ($filter)</span></span>

<span data-ttu-id="ee8d8-160">Para filtrar, use un `where` cláusula.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-160">To filter, use a `where` clause.</span></span> <span data-ttu-id="ee8d8-161">El ejemplo siguiente se filtra por categoría de producto.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-161">The following example filters by product category.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

<span data-ttu-id="ee8d8-162">Este código corresponde a la siguiente consulta de OData.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-162">This code corresponds to the following OData query.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

<span data-ttu-id="ee8d8-163">Tenga en cuenta que el proxy convierte el `where` cláusula en una OData `$filter` expresión.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-163">Notice that the proxy converts the `where` clause into an OData `$filter` expression.</span></span>

### <a name="sorting-orderby"></a><span data-ttu-id="ee8d8-164">Ordenación ($orderby)</span><span class="sxs-lookup"><span data-stu-id="ee8d8-164">Sorting ($orderby)</span></span>

<span data-ttu-id="ee8d8-165">Para ordenar, use un `orderby` cláusula.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-165">To sort, use an `orderby` clause.</span></span> <span data-ttu-id="ee8d8-166">En el ejemplo siguiente se ordenan por precio, de mayor a menor.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-166">The following example sorts by price, from highest to lowest.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

<span data-ttu-id="ee8d8-167">Esta es la solicitud de OData correspondiente.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-167">Here is the corresponding OData request.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a><span data-ttu-id="ee8d8-168">Paginación del cliente ($skip y $top)</span><span class="sxs-lookup"><span data-stu-id="ee8d8-168">Client-Side Paging ($skip and $top)</span></span>

<span data-ttu-id="ee8d8-169">Para los conjuntos de entidades de gran tamaño, el cliente desee limitar el número de resultados.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-169">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="ee8d8-170">Por ejemplo, un cliente podría mostrar 10 entradas a la vez.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-170">For example, a client might show 10 entries at a time.</span></span> <span data-ttu-id="ee8d8-171">Esto se denomina *paginación del lado cliente*.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-171">This is called *client-side paging*.</span></span> <span data-ttu-id="ee8d8-172">(También hay [paginación del lado servidor](../supporting-odata-query-options.md#server-paging), donde el servidor limita el número de resultados.) Para llevar a cabo la paginación del lado cliente, usar LINQ **omitir** y **tomar** métodos.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-172">(There is also [server-side paging](../supporting-odata-query-options.md#server-paging), where the server limits the number of results.) To perform client-side paging, use the LINQ **Skip** and **Take** methods.</span></span> <span data-ttu-id="ee8d8-173">En el ejemplo siguiente se omite los primeros 40 resultados y toma los 10 siguientes.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-173">The following example skips the first 40 results and takes the next 10.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

<span data-ttu-id="ee8d8-174">Esta es la solicitud de OData correspondiente:</span><span class="sxs-lookup"><span data-stu-id="ee8d8-174">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a><span data-ttu-id="ee8d8-175">Select ($select) y expandir ($expand)</span><span class="sxs-lookup"><span data-stu-id="ee8d8-175">Select ($select) and Expand ($expand)</span></span>

<span data-ttu-id="ee8d8-176">Para incluir las entidades relacionadas, use la `DataServiceQuery<t>.Expand` método.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-176">To include related entities, use the `DataServiceQuery<t>.Expand` method.</span></span> <span data-ttu-id="ee8d8-177">Por ejemplo, para incluir la `Supplier` para cada `Product`:</span><span class="sxs-lookup"><span data-stu-id="ee8d8-177">For example, to include the `Supplier` for each `Product`:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

<span data-ttu-id="ee8d8-178">Esta es la solicitud de OData correspondiente:</span><span class="sxs-lookup"><span data-stu-id="ee8d8-178">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

<span data-ttu-id="ee8d8-179">Para cambiar la forma de la respuesta, use LINQ **seleccione** cláusula.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-179">To change the shape of the response, use the LINQ **select** clause.</span></span> <span data-ttu-id="ee8d8-180">En el ejemplo siguiente se obtiene simplemente el nombre de cada producto, con ninguna otra propiedad.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-180">The following example gets just the name of each product, with no other properties.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

<span data-ttu-id="ee8d8-181">Esta es la solicitud de OData correspondiente:</span><span class="sxs-lookup"><span data-stu-id="ee8d8-181">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

<span data-ttu-id="ee8d8-182">Una cláusula select puede incluir las entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-182">A select clause can include related entities.</span></span> <span data-ttu-id="ee8d8-183">En ese caso, no llame a **expandir**; el proxy automáticamente incluye la expansión en este caso.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-183">In that case, do not call **Expand**; the proxy automatically includes the expansion in this case.</span></span> <span data-ttu-id="ee8d8-184">En el ejemplo siguiente se obtiene el nombre y el proveedor de cada producto.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-184">The following example gets the name and supplier of each product.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

<span data-ttu-id="ee8d8-185">Esta es la solicitud de OData correspondiente.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-185">Here is the corresponding OData request.</span></span> <span data-ttu-id="ee8d8-186">Tenga en cuenta que incluye el **$expand** opción.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-186">Notice that it includes the **$expand** option.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

<span data-ttu-id="ee8d8-187">Para obtener más información acerca de $select y $, expanda, consulte [con $select, $expand y $value en API Web 2](../using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="ee8d8-187">For more information about $select and $expand, see [Using $select, $expand, and $value in Web API 2](../using-select-expand-and-value.md).</span></span>

## <a name="add-a-new-entity"></a><span data-ttu-id="ee8d8-188">Agregar una nueva entidad</span><span class="sxs-lookup"><span data-stu-id="ee8d8-188">Add a New Entity</span></span>

<span data-ttu-id="ee8d8-189">Para agregar una nueva entidad a un conjunto de entidades, llame `AddToEntitySet`, donde *EntitySet* es el nombre del conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-189">To add a new entity to an entity set, call `AddToEntitySet`, where *EntitySet* is the name of the entity set.</span></span> <span data-ttu-id="ee8d8-190">Por ejemplo, `AddToProducts` agrega un nuevo `Product` a la `Products` conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-190">For example, `AddToProducts` adds a new `Product` to the `Products` entity set.</span></span> <span data-ttu-id="ee8d8-191">Al generar el proxy, WCF Data Services crea automáticamente estos fuertemente tipado **AddTo** métodos.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-191">When you generate the proxy, WCF Data Services automatically creates these strongly-typed **AddTo** methods.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

<span data-ttu-id="ee8d8-192">Para agregar un vínculo entre dos entidades, use la **AddLink** y **SetLink** métodos.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-192">To add a link between two entities, use the **AddLink** and **SetLink** methods.</span></span> <span data-ttu-id="ee8d8-193">El código siguiente agrega un nuevo proveedor y un nuevo producto y, a continuación, crea vínculos entre ellos.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-193">The following code adds a new supplier and a new product, and then creates links between them.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

<span data-ttu-id="ee8d8-194">Use **AddLink** cuando la propiedad de navegación es una colección.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-194">Use **AddLink** when the navigation property is a collection.</span></span> <span data-ttu-id="ee8d8-195">En este ejemplo, vamos a agregar un producto a la `Products` colección en el proveedor.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-195">In this example, we are adding a product to the `Products` collection on the supplier.</span></span>

<span data-ttu-id="ee8d8-196">Use **SetLink** cuando la propiedad de navegación es una entidad única.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-196">Use **SetLink** when the navigation property is a single entity.</span></span> <span data-ttu-id="ee8d8-197">En este ejemplo, estamos estableciendo la `Supplier` propiedad en el producto.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-197">In this example, we are setting the `Supplier` property on the product.</span></span>

## <a name="update--patch"></a><span data-ttu-id="ee8d8-198">Actualizar / Patch</span><span class="sxs-lookup"><span data-stu-id="ee8d8-198">Update / Patch</span></span>

<span data-ttu-id="ee8d8-199">Para actualizar una entidad, llame a la **UpdateObject** método.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-199">To update an entity, call the **UpdateObject** method.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

<span data-ttu-id="ee8d8-200">La actualización se realiza cuando se llama a **SaveChanges**.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-200">The update is performed when you call **SaveChanges**.</span></span> <span data-ttu-id="ee8d8-201">De forma predeterminada, WCF envía una solicitud HTTP MERGE.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-201">By default, WCF sends an HTTP MERGE request.</span></span> <span data-ttu-id="ee8d8-202">El **PatchOnUpdate** opción indica a WCF para enviar una revisión de HTTP en su lugar.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-202">The **PatchOnUpdate** option tells WCF to send an HTTP PATCH instead.</span></span>

> [!NOTE]
> <span data-ttu-id="ee8d8-203">¿Por qué revisión frente a combinación?</span><span class="sxs-lookup"><span data-stu-id="ee8d8-203">Why PATCH versus MERGE?</span></span> <span data-ttu-id="ee8d8-204">La especificación de HTTP 1.1 original ([RCF 2616](http://tools.ietf.org/html/rfc2616)) no se definió ningún método HTTP con la semántica de "actualización parcial".</span><span class="sxs-lookup"><span data-stu-id="ee8d8-204">The original HTTP 1.1 specification ([RCF 2616](http://tools.ietf.org/html/rfc2616)) did not define any HTTP method with "partial update" semantics.</span></span> <span data-ttu-id="ee8d8-205">Para admitir actualizaciones parciales, la especificación OData define el método de mezcla.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-205">To support partial updates, the OData specification defined the MERGE method.</span></span> <span data-ttu-id="ee8d8-206">En 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) definido por el método de revisión para actualizaciones parciales.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-206">In 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) defined the PATCH method for partial updates.</span></span> <span data-ttu-id="ee8d8-207">Puede leer parte del historial en este [entrada de blog](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) en el Blog de WCF Data Services.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-207">You can read some of the history in this [blog post](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) on the WCF Data Services Blog.</span></span> <span data-ttu-id="ee8d8-208">En la actualidad, revisión es preferible MERGE.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-208">Today, PATCH is preferred over MERGE.</span></span> <span data-ttu-id="ee8d8-209">El controlador de OData creado mediante el scaffolding de Web API es compatible con ambos métodos.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-209">The OData controller created by the Web API scaffolding supports both methods.</span></span>


<span data-ttu-id="ee8d8-210">Si desea reemplazar toda la entidad (PUT semántica), especifique la **ReplaceOnUpdate** opción.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-210">If you want to replace the entire entity (PUT semantics), specify the **ReplaceOnUpdate** option.</span></span> <span data-ttu-id="ee8d8-211">Esto hace que WCF enviar una solicitud HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-211">This causes WCF to send an HTTP PUT request.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a><span data-ttu-id="ee8d8-212">Eliminar una entidad</span><span class="sxs-lookup"><span data-stu-id="ee8d8-212">Delete an Entity</span></span>

<span data-ttu-id="ee8d8-213">Para eliminar una entidad, llame a **DeleteObject**.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-213">To delete an entity, call **DeleteObject**.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a><span data-ttu-id="ee8d8-214">Invocar una acción de OData</span><span class="sxs-lookup"><span data-stu-id="ee8d8-214">Invoke an OData Action</span></span>

<span data-ttu-id="ee8d8-215">En OData, [acciones](odata-actions.md) son una forma de agregar los comportamientos de servidor que no se definen fácilmente como operaciones CRUD en entidades.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-215">In OData, [actions](odata-actions.md) are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span>

<span data-ttu-id="ee8d8-216">Aunque el documento de metadatos de OData describe las acciones, la clase de proxy no crea ningún método fuertemente tipado para ellos.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-216">Although the OData metadata document describes the actions, the proxy class does not create any strongly-typed methods for them.</span></span> <span data-ttu-id="ee8d8-217">Todavía se puede invocar una acción de OData mediante el uso de la interfaz genérica **Execute** método.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-217">You can still invoke an OData action by using the generic **Execute** method.</span></span> <span data-ttu-id="ee8d8-218">Sin embargo, debe conocer los tipos de datos de los parámetros y el valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-218">However, you will need to know the data types of the parameters and the return value.</span></span>

<span data-ttu-id="ee8d8-219">Por ejemplo, el `RateProduct` acción toma el parámetro denominado "Clasificación" de tipo `Int32` y devuelve un `double`.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-219">For example, the `RateProduct` action takes parameter named "Rating" of type `Int32` and returns a `double`.</span></span> <span data-ttu-id="ee8d8-220">El código siguiente muestra cómo invocar esta acción.</span><span class="sxs-lookup"><span data-stu-id="ee8d8-220">The following code shows how to invoke this action.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

<span data-ttu-id="ee8d8-221">Para obtener más información, consulte[al llamar a operaciones de servicio y las acciones](https://msdn.microsoft.com/library/hh230677.aspx).</span><span class="sxs-lookup"><span data-stu-id="ee8d8-221">For more information, see[Calling Service Operations and Actions](https://msdn.microsoft.com/library/hh230677.aspx).</span></span>

<span data-ttu-id="ee8d8-222">Es una opción ampliar la **contenedor** clase para proporcionar un método fuertemente tipado que invoca la acción:</span><span class="sxs-lookup"><span data-stu-id="ee8d8-222">One option is to extend the **Container** class to provide a strongly typed method that invokes the action:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
