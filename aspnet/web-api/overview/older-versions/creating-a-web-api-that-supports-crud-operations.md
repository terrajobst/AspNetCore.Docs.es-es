---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Habilitar las operaciones CRUD en ASP.NET Web API 1 | Documentos de Microsoft
author: MikeWasson
description: "Este tutorial muestra cómo admitir operaciones CRUD en un servicio HTTP mediante ASP.NET Web API. Versiones de software que se usa en el tutorial Web PA de Visual Studio 2012..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2012
ms.topic: article
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: 69b7d5453b6ff36d6e28a69428b016cb8cfd06e9
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2018
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a><span data-ttu-id="63cb3-104">Habilitar las operaciones CRUD en ASP.NET Web API 1</span><span class="sxs-lookup"><span data-stu-id="63cb3-104">Enabling CRUD Operations in ASP.NET Web API 1</span></span>
====================
<span data-ttu-id="63cb3-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="63cb3-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="63cb3-106">Descargar el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="63cb3-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> <span data-ttu-id="63cb3-107">Este tutorial muestra cómo admitir operaciones CRUD en un servicio HTTP mediante ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="63cb3-107">This tutorial shows how to support CRUD operations in an HTTP service using ASP.NET Web API.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="63cb3-108">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="63cb3-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="63cb3-109">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="63cb3-109">Visual Studio 2012</span></span>
> - <span data-ttu-id="63cb3-110">API Web 1 (también funciona con la API Web 2)</span><span class="sxs-lookup"><span data-stu-id="63cb3-110">Web API 1 (also works with Web API 2)</span></span>


<span data-ttu-id="63cb3-111">Es el acrónimo CRUD &quot;creación, lectura, actualización y eliminación,&quot; ¿cuáles son las cuatro operaciones de base de datos básica.</span><span class="sxs-lookup"><span data-stu-id="63cb3-111">CRUD stands for &quot;Create, Read, Update, and Delete,&quot; which are the four basic database operations.</span></span> <span data-ttu-id="63cb3-112">Muchos servicios HTTP también modelar las operaciones CRUD a través de REST o API de REST.</span><span class="sxs-lookup"><span data-stu-id="63cb3-112">Many HTTP services also model CRUD operations through REST or REST-like APIs.</span></span>

<span data-ttu-id="63cb3-113">En este tutorial, compilará una API para administrar una lista de productos de web muy sencilla.</span><span class="sxs-lookup"><span data-stu-id="63cb3-113">In this tutorial, you will build a very simple web API to manage a list of products.</span></span> <span data-ttu-id="63cb3-114">Cada producto contendrá un nombre, el precio y la categoría (como &quot;toys&quot; o &quot;hardware&quot;), además de un identificador de producto.</span><span class="sxs-lookup"><span data-stu-id="63cb3-114">Each product will contain a name, price, and category (such as &quot;toys&quot; or &quot;hardware&quot;), plus a product ID.</span></span>

<span data-ttu-id="63cb3-115">Los productos de API expondrá siguientes métodos.</span><span class="sxs-lookup"><span data-stu-id="63cb3-115">The products API will expose following methods.</span></span>

| <span data-ttu-id="63cb3-116">Acción</span><span class="sxs-lookup"><span data-stu-id="63cb3-116">Action</span></span> | <span data-ttu-id="63cb3-117">Método HTTP</span><span class="sxs-lookup"><span data-stu-id="63cb3-117">HTTP method</span></span> | <span data-ttu-id="63cb3-118">URI relativo</span><span class="sxs-lookup"><span data-stu-id="63cb3-118">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="63cb3-119">Obtener una lista de todos los productos</span><span class="sxs-lookup"><span data-stu-id="63cb3-119">Get a list of all products</span></span> | <span data-ttu-id="63cb3-120">GET</span><span class="sxs-lookup"><span data-stu-id="63cb3-120">GET</span></span> | <span data-ttu-id="63cb3-121">productos/api /</span><span class="sxs-lookup"><span data-stu-id="63cb3-121">/api/products</span></span> |
| <span data-ttu-id="63cb3-122">Obtener un producto por Id.</span><span class="sxs-lookup"><span data-stu-id="63cb3-122">Get a product by ID</span></span> | <span data-ttu-id="63cb3-123">GET</span><span class="sxs-lookup"><span data-stu-id="63cb3-123">GET</span></span> | <span data-ttu-id="63cb3-124">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="63cb3-124">/api/products/*id*</span></span> |
| <span data-ttu-id="63cb3-125">Obtener un producto por categoría</span><span class="sxs-lookup"><span data-stu-id="63cb3-125">Get a product by category</span></span> | <span data-ttu-id="63cb3-126">GET</span><span class="sxs-lookup"><span data-stu-id="63cb3-126">GET</span></span> | <span data-ttu-id="63cb3-127">/api/products?category=*category*</span><span class="sxs-lookup"><span data-stu-id="63cb3-127">/api/products?category=*category*</span></span> |
| <span data-ttu-id="63cb3-128">Crear un nuevo producto</span><span class="sxs-lookup"><span data-stu-id="63cb3-128">Create a new product</span></span> | <span data-ttu-id="63cb3-129">EXPONER</span><span class="sxs-lookup"><span data-stu-id="63cb3-129">POST</span></span> | <span data-ttu-id="63cb3-130">productos/api /</span><span class="sxs-lookup"><span data-stu-id="63cb3-130">/api/products</span></span> |
| <span data-ttu-id="63cb3-131">Actualizar un producto</span><span class="sxs-lookup"><span data-stu-id="63cb3-131">Update a product</span></span> | <span data-ttu-id="63cb3-132">PUT</span><span class="sxs-lookup"><span data-stu-id="63cb3-132">PUT</span></span> | <span data-ttu-id="63cb3-133">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="63cb3-133">/api/products/*id*</span></span> |
| <span data-ttu-id="63cb3-134">Eliminar un producto</span><span class="sxs-lookup"><span data-stu-id="63cb3-134">Delete a product</span></span> | <span data-ttu-id="63cb3-135">SUPRIMIR</span><span class="sxs-lookup"><span data-stu-id="63cb3-135">DELETE</span></span> | <span data-ttu-id="63cb3-136">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="63cb3-136">/api/products/*id*</span></span> |

<span data-ttu-id="63cb3-137">Ten en cuenta que algunos de los URI incluyen el identificador de producto en la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="63cb3-137">Notice that some of the URIs include the product ID in path.</span></span> <span data-ttu-id="63cb3-138">Por ejemplo, para obtener el producto cuyo identificador es 28, el cliente envía una solicitud GET `http://hostname/api/products/28`.</span><span class="sxs-lookup"><span data-stu-id="63cb3-138">For example, to get the product whose ID is 28, the client sends a GET request for `http://hostname/api/products/28`.</span></span>

### <a name="resources"></a><span data-ttu-id="63cb3-139">Recursos</span><span class="sxs-lookup"><span data-stu-id="63cb3-139">Resources</span></span>

<span data-ttu-id="63cb3-140">Los productos de API define el URI para dos tipos de recursos:</span><span class="sxs-lookup"><span data-stu-id="63cb3-140">The products API defines URIs for two resource types:</span></span>

| <span data-ttu-id="63cb3-141">Recurso</span><span class="sxs-lookup"><span data-stu-id="63cb3-141">Resource</span></span> | <span data-ttu-id="63cb3-142">Identificador URI</span><span class="sxs-lookup"><span data-stu-id="63cb3-142">URI</span></span> |
| --- | --- |
| <span data-ttu-id="63cb3-143">La lista de todos los productos.</span><span class="sxs-lookup"><span data-stu-id="63cb3-143">The list of all the products.</span></span> | <span data-ttu-id="63cb3-144">productos/api /</span><span class="sxs-lookup"><span data-stu-id="63cb3-144">/api/products</span></span> |
| <span data-ttu-id="63cb3-145">Un producto individual.</span><span class="sxs-lookup"><span data-stu-id="63cb3-145">An individual product.</span></span> | <span data-ttu-id="63cb3-146">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="63cb3-146">/api/products/*id*</span></span> |

### <a name="methods"></a><span data-ttu-id="63cb3-147">Métodos</span><span class="sxs-lookup"><span data-stu-id="63cb3-147">Methods</span></span>

<span data-ttu-id="63cb3-148">Los cuatro principales métodos HTTP (GET, PUT, POST y DELETE) se pueden asignar a las operaciones CRUD como sigue:</span><span class="sxs-lookup"><span data-stu-id="63cb3-148">The four main HTTP methods (GET, PUT, POST, and DELETE) can be mapped to CRUD operations as follows:</span></span>

- <span data-ttu-id="63cb3-149">GET recupera la representación del recurso en un URI especificado.</span><span class="sxs-lookup"><span data-stu-id="63cb3-149">GET retrieves the representation of the resource at a specified URI.</span></span> <span data-ttu-id="63cb3-150">GET no debe tener efectos deseados en el servidor.</span><span class="sxs-lookup"><span data-stu-id="63cb3-150">GET should have no side effects on the server.</span></span>
- <span data-ttu-id="63cb3-151">PUT actualiza un recurso en un URI especificado.</span><span class="sxs-lookup"><span data-stu-id="63cb3-151">PUT updates a resource at a specified URI.</span></span> <span data-ttu-id="63cb3-152">PUT también puede utilizarse para crear un nuevo recurso en un URI especificado, si el servidor permite a los clientes especificar a URI nuevo.</span><span class="sxs-lookup"><span data-stu-id="63cb3-152">PUT can also be used to create a new resource at a specified URI, if the server allows clients to specify new URIs.</span></span> <span data-ttu-id="63cb3-153">Para este tutorial, la API no admitirá la creación a través de PUT.</span><span class="sxs-lookup"><span data-stu-id="63cb3-153">For this tutorial, the API will not support creation through PUT.</span></span>
- <span data-ttu-id="63cb3-154">POST crea un nuevo recurso.</span><span class="sxs-lookup"><span data-stu-id="63cb3-154">POST creates a new resource.</span></span> <span data-ttu-id="63cb3-155">El servidor asigna el URI para el nuevo objeto y devuelve este URI como parte del mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="63cb3-155">The server assigns the URI for the new object and returns this URI as part of the response message.</span></span>
- <span data-ttu-id="63cb3-156">El método DELETE Elimina un recurso en un URI especificado.</span><span class="sxs-lookup"><span data-stu-id="63cb3-156">DELETE deletes a resource at a specified URI.</span></span>

<span data-ttu-id="63cb3-157">Nota: El método PUT reemplaza la entidad completa de productos.</span><span class="sxs-lookup"><span data-stu-id="63cb3-157">Note: The PUT method replaces the entire product entity.</span></span> <span data-ttu-id="63cb3-158">Es decir, el cliente debe enviar una representación completa del producto actualizada.</span><span class="sxs-lookup"><span data-stu-id="63cb3-158">That is, the client is expected to send a complete representation of the updated product.</span></span> <span data-ttu-id="63cb3-159">Si desea admitir actualizaciones parciales, es preferible el método de revisión.</span><span class="sxs-lookup"><span data-stu-id="63cb3-159">If you want to support partial updates, the PATCH method is preferred.</span></span> <span data-ttu-id="63cb3-160">Este tutorial no implementa la revisión.</span><span class="sxs-lookup"><span data-stu-id="63cb3-160">This tutorial does not implement PATCH.</span></span>

## <a name="create-a-new-web-api-project"></a><span data-ttu-id="63cb3-161">Crear un nuevo proyecto de API de Web</span><span class="sxs-lookup"><span data-stu-id="63cb3-161">Create a New Web API Project</span></span>

<span data-ttu-id="63cb3-162">Comience ejecutando Visual Studio y seleccione **nuevo proyecto** desde el **iniciar** página.</span><span class="sxs-lookup"><span data-stu-id="63cb3-162">Start by running Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="63cb3-163">O bien, en la **archivo** menú, seleccione **New** y, a continuación, **proyecto**.</span><span class="sxs-lookup"><span data-stu-id="63cb3-163">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="63cb3-164">En el **plantillas** panel, seleccione **plantillas instaladas** y expanda el **Visual C#** nodo.</span><span class="sxs-lookup"><span data-stu-id="63cb3-164">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="63cb3-165">En **Visual C#**, seleccione **Web**.</span><span class="sxs-lookup"><span data-stu-id="63cb3-165">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="63cb3-166">En la lista de plantillas de proyecto, seleccione **aplicación Web de ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="63cb3-166">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="63cb3-167">Denomine el proyecto &quot;ProductStore&quot; y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="63cb3-167">Name the project &quot;ProductStore&quot; and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

<span data-ttu-id="63cb3-168">En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione **API Web** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="63cb3-168">In the **New ASP.NET MVC 4 Project** dialog, select **Web API** and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a><span data-ttu-id="63cb3-169">Agregar un modelo</span><span class="sxs-lookup"><span data-stu-id="63cb3-169">Adding a Model</span></span>

<span data-ttu-id="63cb3-170">Un *modelo* es un objeto que representa los datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="63cb3-170">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="63cb3-171">En ASP.NET Web API, puede usar los objetos CLR fuertemente tipados como modelos y automáticamente se serializará a XML o JSON para el cliente.</span><span class="sxs-lookup"><span data-stu-id="63cb3-171">In ASP.NET Web API, you can use strongly typed CLR objects as models, and they will automatically be serialized to XML or JSON for the client.</span></span>

<span data-ttu-id="63cb3-172">Para la API ProductStore, nuestros datos consisten de los productos, por lo que vamos a crear una nueva clase denominada `Product`.</span><span class="sxs-lookup"><span data-stu-id="63cb3-172">For the ProductStore API, our data consists of products, so we'll create a new class named `Product`.</span></span>

<span data-ttu-id="63cb3-173">Si el Explorador de soluciones no está visible, haga clic en el **vista** menú y seleccione **el Explorador de soluciones**.</span><span class="sxs-lookup"><span data-stu-id="63cb3-173">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="63cb3-174">En el Explorador de soluciones, haga clic en el **modelos** carpeta.</span><span class="sxs-lookup"><span data-stu-id="63cb3-174">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="63cb3-175">En el contexto meny, seleccione **agregar**, a continuación, seleccione **clase**.</span><span class="sxs-lookup"><span data-stu-id="63cb3-175">From the context meny, select **Add**, then select **Class**.</span></span> <span data-ttu-id="63cb3-176">Nombre de la clase &quot;producto&quot;.</span><span class="sxs-lookup"><span data-stu-id="63cb3-176">Name the class &quot;Product&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

<span data-ttu-id="63cb3-177">Agregue las siguientes propiedades para el `Product` clase.</span><span class="sxs-lookup"><span data-stu-id="63cb3-177">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a><span data-ttu-id="63cb3-178">Agregar un repositorio</span><span class="sxs-lookup"><span data-stu-id="63cb3-178">Adding a Repository</span></span>

<span data-ttu-id="63cb3-179">Es necesario almacenar una colección de productos.</span><span class="sxs-lookup"><span data-stu-id="63cb3-179">We need to store a collection of products.</span></span> <span data-ttu-id="63cb3-180">Es una buena idea para separar la colección de la implementación del servicio.</span><span class="sxs-lookup"><span data-stu-id="63cb3-180">It's a good idea to separate the collection from our service implementation.</span></span> <span data-ttu-id="63cb3-181">De este modo, podemos cambiar la memoria auxiliar sin volver a escribir la clase de servicio.</span><span class="sxs-lookup"><span data-stu-id="63cb3-181">That way, we can change the backing store without rewriting the service class.</span></span> <span data-ttu-id="63cb3-182">Este tipo de diseño se denomina la *repositorio* patrón.</span><span class="sxs-lookup"><span data-stu-id="63cb3-182">This type of design is called the *repository* pattern.</span></span> <span data-ttu-id="63cb3-183">Empiece por definir una interfaz genérica para el repositorio.</span><span class="sxs-lookup"><span data-stu-id="63cb3-183">Start by defining a generic interface for the repository.</span></span>

<span data-ttu-id="63cb3-184">En el Explorador de soluciones, haga clic en el **modelos** carpeta.</span><span class="sxs-lookup"><span data-stu-id="63cb3-184">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="63cb3-185">Seleccione **agregar**, a continuación, seleccione **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="63cb3-185">Select **Add**, then select **New Item**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

<span data-ttu-id="63cb3-186">En el **plantillas** panel, seleccione **plantillas instaladas** y expanda el nodo C#.</span><span class="sxs-lookup"><span data-stu-id="63cb3-186">In the **Templates** pane, select **Installed Templates** and expand the C# node.</span></span> <span data-ttu-id="63cb3-187">En C#, seleccione **código**.</span><span class="sxs-lookup"><span data-stu-id="63cb3-187">Under C#, select **Code**.</span></span> <span data-ttu-id="63cb3-188">En la lista de plantillas de código, seleccione **interfaz**.</span><span class="sxs-lookup"><span data-stu-id="63cb3-188">In the list of code templates, select **Interface**.</span></span> <span data-ttu-id="63cb3-189">Nombre de la interfaz &quot;IProductRepository&quot;.</span><span class="sxs-lookup"><span data-stu-id="63cb3-189">Name the interface &quot;IProductRepository&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

<span data-ttu-id="63cb3-190">Agregue la siguiente implementación:</span><span class="sxs-lookup"><span data-stu-id="63cb3-190">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

<span data-ttu-id="63cb3-191">Ahora agregue otra clase en la carpeta Models, denominada &quot;ProductRepository.&quot; Esta clase implementará la interfaz `IProductRespository`.</span><span class="sxs-lookup"><span data-stu-id="63cb3-191">Now add another class to the Models folder, named &quot;ProductRepository.&quot; This class will implement the `IProductRespository` interface.</span></span> <span data-ttu-id="63cb3-192">Agregue la siguiente implementación:</span><span class="sxs-lookup"><span data-stu-id="63cb3-192">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

<span data-ttu-id="63cb3-193">El repositorio mantiene la lista en la memoria local.</span><span class="sxs-lookup"><span data-stu-id="63cb3-193">The repository keeps the list in local memory.</span></span> <span data-ttu-id="63cb3-194">Esto es correcto para obtener un tutorial, pero en una aplicación real, podría almacenar los datos externamente, una base de datos o en el almacenamiento en la nube.</span><span class="sxs-lookup"><span data-stu-id="63cb3-194">This is OK for a tutorial, but in a real application, you would store the data externally, either a database or in cloud storage.</span></span> <span data-ttu-id="63cb3-195">El modelo de repositorio le resultará más fácil cambiar la implementación más adelante.</span><span class="sxs-lookup"><span data-stu-id="63cb3-195">The repository pattern will make it easier to change the implementation later.</span></span>

## <a name="adding-a-web-api-controller"></a><span data-ttu-id="63cb3-196">Cómo agregar un controlador de API Web</span><span class="sxs-lookup"><span data-stu-id="63cb3-196">Adding a Web API Controller</span></span>

<span data-ttu-id="63cb3-197">Si ha trabajado con ASP.NET MVC, a continuación, ya están familiarizados con los controladores.</span><span class="sxs-lookup"><span data-stu-id="63cb3-197">If you have worked with ASP.NET MVC, then you are already familiar with controllers.</span></span> <span data-ttu-id="63cb3-198">En ASP.NET Web API, un *controlador* es una clase que controla las solicitudes HTTP desde el cliente.</span><span class="sxs-lookup"><span data-stu-id="63cb3-198">In ASP.NET Web API, a *controller* is a class that handles HTTP requests from the client.</span></span> <span data-ttu-id="63cb3-199">El Asistente para nuevo proyecto crea dos controladores cuando creó el proyecto.</span><span class="sxs-lookup"><span data-stu-id="63cb3-199">The New Project wizard created two controllers for you when it created the project.</span></span> <span data-ttu-id="63cb3-200">Para verlas, expanda la carpeta de controladores en el Explorador de soluciones.</span><span class="sxs-lookup"><span data-stu-id="63cb3-200">To see them, expand the Controllers folder in Solution Explorer.</span></span>

- <span data-ttu-id="63cb3-201">HomeController es un controlador de MVC de ASP.NET tradicional.</span><span class="sxs-lookup"><span data-stu-id="63cb3-201">HomeController is a traditional ASP.NET MVC controller.</span></span> <span data-ttu-id="63cb3-202">Es responsable de servir páginas HTML para el sitio y no está directamente relacionado con la API web.</span><span class="sxs-lookup"><span data-stu-id="63cb3-202">It is responsible for serving HTML pages for the site, and is not directly related to our web API.</span></span>
- <span data-ttu-id="63cb3-203">ValuesController es un controlador de WebAPI de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="63cb3-203">ValuesController is an example WebAPI controller.</span></span>

<span data-ttu-id="63cb3-204">Voy a eliminar ValuesController, haciendo clic en el archivo en el Explorador de soluciones y seleccionando **eliminar.**</span><span class="sxs-lookup"><span data-stu-id="63cb3-204">Go ahead and delete ValuesController, by right-clicking the file in Solution Explorer and selecting **Delete.**</span></span> <span data-ttu-id="63cb3-205">Ahora agregue un nuevo controlador, como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="63cb3-205">Now add a new controller, as follows:</span></span>

<span data-ttu-id="63cb3-206">En **el Explorador de soluciones**, haga clic en la carpeta de controladores.</span><span class="sxs-lookup"><span data-stu-id="63cb3-206">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="63cb3-207">Seleccione **agregar** y, a continuación, seleccione **controlador**.</span><span class="sxs-lookup"><span data-stu-id="63cb3-207">Select **Add** and then select **Controller**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

<span data-ttu-id="63cb3-208">En el **Agregar controlador** asistente, el nombre del controlador &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="63cb3-208">In the **Add Controller** wizard, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="63cb3-209">En el **plantilla** lista desplegable, seleccione **controlador de API en blanco**.</span><span class="sxs-lookup"><span data-stu-id="63cb3-209">In the **Template** drop-down list, select **Empty API Controller**.</span></span> <span data-ttu-id="63cb3-210">A continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="63cb3-210">Then click **Add**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="63cb3-211">No es necesario poner los controladores en una carpeta denominada controladores.</span><span class="sxs-lookup"><span data-stu-id="63cb3-211">It is not necessary to put your contollers into a folder named Controllers.</span></span> <span data-ttu-id="63cb3-212">El nombre de carpeta no es importante; es simplemente una manera cómoda de organizar los archivos de origen.</span><span class="sxs-lookup"><span data-stu-id="63cb3-212">The folder name is not important; it is simply a convenient way to organize your source files.</span></span>


<span data-ttu-id="63cb3-213">El **Agregar controlador** asistente creará un archivo denominado ProductsController.cs en la carpeta de controladores.</span><span class="sxs-lookup"><span data-stu-id="63cb3-213">The **Add Controller** wizard will create a file named ProductsController.cs in the Controllers folder.</span></span> <span data-ttu-id="63cb3-214">Si este archivo ya no está abierto, haga doble clic en el archivo para abrirlo.</span><span class="sxs-lookup"><span data-stu-id="63cb3-214">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="63cb3-215">Agregue el siguiente **con** instrucción:</span><span class="sxs-lookup"><span data-stu-id="63cb3-215">Add the following **using** statement:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

<span data-ttu-id="63cb3-216">Agregar un campo que contiene un **IProductRepository** instancia.</span><span class="sxs-lookup"><span data-stu-id="63cb3-216">Add a field that holds an **IProductRepository** instance.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> <span data-ttu-id="63cb3-217">Al llamar a `new ProductRepository()` en el controlador no es el mejor diseño, porque asocia el controlador a una implementación concreta de `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="63cb3-217">Calling `new ProductRepository()` in the controller is not the best design, because it ties the controller to a particular implementation of `IProductRepository`.</span></span> <span data-ttu-id="63cb3-218">Para obtener un mejor enfoque, consulte [mediante la resolución de dependencia de la API de Web](../advanced/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="63cb3-218">For a better approach, see [Using the Web API Dependency Resolver](../advanced/dependency-injection.md).</span></span>


## <a name="getting-a-resource"></a><span data-ttu-id="63cb3-219">Obtener un recurso</span><span class="sxs-lookup"><span data-stu-id="63cb3-219">Getting a Resource</span></span>

<span data-ttu-id="63cb3-220">La API ProductStore expondrá varios &quot;leer&quot; acciones como métodos GET de HTTP.</span><span class="sxs-lookup"><span data-stu-id="63cb3-220">The ProductStore API will expose several &quot;read&quot; actions as HTTP GET methods.</span></span> <span data-ttu-id="63cb3-221">Cada acción se corresponderá a un método en el `ProductsController` clase.</span><span class="sxs-lookup"><span data-stu-id="63cb3-221">Each action will correspond to a method in the `ProductsController` class.</span></span>

| <span data-ttu-id="63cb3-222">Acción</span><span class="sxs-lookup"><span data-stu-id="63cb3-222">Action</span></span> | <span data-ttu-id="63cb3-223">Método HTTP</span><span class="sxs-lookup"><span data-stu-id="63cb3-223">HTTP method</span></span> | <span data-ttu-id="63cb3-224">URI relativo</span><span class="sxs-lookup"><span data-stu-id="63cb3-224">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="63cb3-225">Obtener una lista de todos los productos</span><span class="sxs-lookup"><span data-stu-id="63cb3-225">Get a list of all products</span></span> | <span data-ttu-id="63cb3-226">GET</span><span class="sxs-lookup"><span data-stu-id="63cb3-226">GET</span></span> | <span data-ttu-id="63cb3-227">productos/api /</span><span class="sxs-lookup"><span data-stu-id="63cb3-227">/api/products</span></span> |
| <span data-ttu-id="63cb3-228">Obtener un producto por Id.</span><span class="sxs-lookup"><span data-stu-id="63cb3-228">Get a product by ID</span></span> | <span data-ttu-id="63cb3-229">GET</span><span class="sxs-lookup"><span data-stu-id="63cb3-229">GET</span></span> | <span data-ttu-id="63cb3-230">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="63cb3-230">/api/products/*id*</span></span> |
| <span data-ttu-id="63cb3-231">Obtener un producto por categoría</span><span class="sxs-lookup"><span data-stu-id="63cb3-231">Get a product by category</span></span> | <span data-ttu-id="63cb3-232">GET</span><span class="sxs-lookup"><span data-stu-id="63cb3-232">GET</span></span> | <span data-ttu-id="63cb3-233">/api/products?category=*category*</span><span class="sxs-lookup"><span data-stu-id="63cb3-233">/api/products?category=*category*</span></span> |

<span data-ttu-id="63cb3-234">Para obtener la lista de todos los productos, agregue este método a la `ProductsController` clase:</span><span class="sxs-lookup"><span data-stu-id="63cb3-234">To get the list of all products, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

<span data-ttu-id="63cb3-235">El nombre del método empieza con &quot;obtener&quot;, por lo que por convención, se asigna a las solicitudes GET.</span><span class="sxs-lookup"><span data-stu-id="63cb3-235">The method name starts with &quot;Get&quot;, so by convention it maps to GET requests.</span></span> <span data-ttu-id="63cb3-236">Además, dado que el método no tiene parámetros, se asigna a un URI que no contenga un  *&quot;identificador&quot;*  segmento en la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="63cb3-236">Also, because the method has no parameters, it maps to a URI that does not contain an *&quot;id&quot;* segment in the path.</span></span>

<span data-ttu-id="63cb3-237">Para obtener un producto por el identificador, agregue este método para el `ProductsController` clase:</span><span class="sxs-lookup"><span data-stu-id="63cb3-237">To get a product by ID, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

<span data-ttu-id="63cb3-238">Este nombre de método también se inicia con &quot;obtener&quot;, pero el método tiene un parámetro denominado *identificador*. Este parámetro se asigna a la &quot;identificador&quot; segmento de la ruta de acceso URI.</span><span class="sxs-lookup"><span data-stu-id="63cb3-238">This method name also starts with &quot;Get&quot;, but the method has a parameter named *id*. This parameter is mapped to the &quot;id&quot; segment of the URI path.</span></span> <span data-ttu-id="63cb3-239">El marco de trabajo de ASP.NET Web API convierte automáticamente el identificador para el tipo de datos correcto (**int**) para el parámetro.</span><span class="sxs-lookup"><span data-stu-id="63cb3-239">The ASP.NET Web API framework automatically converts the ID to the correct data type (**int**) for the parameter.</span></span>

<span data-ttu-id="63cb3-240">El método GetProduct produce una excepción de tipo **HttpResponseException** si *identificador* no es válido.</span><span class="sxs-lookup"><span data-stu-id="63cb3-240">The GetProduct method throws an exception of type **HttpResponseException** if *id* is not valid.</span></span> <span data-ttu-id="63cb3-241">Esta excepción se traducirá por el marco de trabajo en un error 404 (no encontrado).</span><span class="sxs-lookup"><span data-stu-id="63cb3-241">This exception will be translated by the framework into a 404 (Not Found) error.</span></span>

<span data-ttu-id="63cb3-242">Por último, agregue un método para buscar productos por categoría:</span><span class="sxs-lookup"><span data-stu-id="63cb3-242">Finally, add a method to find products by category:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

<span data-ttu-id="63cb3-243">Si el URI de solicitud tiene una cadena de consulta, API Web intenta buscar coincidencias con los parámetros de consulta en los parámetros en el método del controlador.</span><span class="sxs-lookup"><span data-stu-id="63cb3-243">If the request URI has a query string, Web API tries to match the query parameters to parameters on the controller method.</span></span> <span data-ttu-id="63cb3-244">Por lo tanto, un URI del formulario "api/products? categoría =*categoría*" se asignará a este método.</span><span class="sxs-lookup"><span data-stu-id="63cb3-244">Therefore, a URI of the form "api/products?category=*category*" will map to this method.</span></span>

## <a name="creating-a-resource"></a><span data-ttu-id="63cb3-245">Crear un recurso</span><span class="sxs-lookup"><span data-stu-id="63cb3-245">Creating a Resource</span></span>

<span data-ttu-id="63cb3-246">A continuación, vamos a agregar un método a la `ProductsController` clase para crear un nuevo producto.</span><span class="sxs-lookup"><span data-stu-id="63cb3-246">Next, we'll add a method to the `ProductsController` class to create a new product.</span></span> <span data-ttu-id="63cb3-247">Esta es una implementación sencilla del método:</span><span class="sxs-lookup"><span data-stu-id="63cb3-247">Here is a simple implementation of the method:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

<span data-ttu-id="63cb3-248">Tenga en cuenta dos cosas acerca de este método:</span><span class="sxs-lookup"><span data-stu-id="63cb3-248">Note two things about this method:</span></span>

- <span data-ttu-id="63cb3-249">El nombre del método empieza con &quot;Post... &quot;.</span><span class="sxs-lookup"><span data-stu-id="63cb3-249">The method name starts with &quot;Post...&quot;.</span></span> <span data-ttu-id="63cb3-250">Para crear un nuevo producto, el cliente envía una solicitud HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="63cb3-250">To create a new product, the client sends an HTTP POST request.</span></span>
- <span data-ttu-id="63cb3-251">El método toma un parámetro de tipo Product.</span><span class="sxs-lookup"><span data-stu-id="63cb3-251">The method takes a parameter of type Product.</span></span> <span data-ttu-id="63cb3-252">En la API de Web, parámetros con tipos complejos se deserializan el cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="63cb3-252">In Web API, parameters with complex types are deserialized from the request body.</span></span> <span data-ttu-id="63cb3-253">Por lo tanto, esperamos que el cliente envíe una representación serializada de un objeto de producto, en formato XML o JSON.</span><span class="sxs-lookup"><span data-stu-id="63cb3-253">Therefore, we expect the client to send a serialized representation of a product object, in either XML or JSON format.</span></span>

<span data-ttu-id="63cb3-254">Esta implementación funcionará, pero no es muy completa.</span><span class="sxs-lookup"><span data-stu-id="63cb3-254">This implementation will work, but it is not quite complete.</span></span> <span data-ttu-id="63cb3-255">Idealmente, nos gustaría que la respuesta HTTP para incluir lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="63cb3-255">Ideally, we would like the HTTP response to include the following:</span></span>

- <span data-ttu-id="63cb3-256">**Código de respuesta:** de forma predeterminada, el marco Web API establece el código de estado de respuesta en 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="63cb3-256">**Response code:** By default, the Web API framework sets the response status code to 200 (OK).</span></span> <span data-ttu-id="63cb3-257">Pero, según el protocolo HTTP/1.1, cuando una solicitud POST da como resultado la creación de un recurso, el servidor debe responder con el estado 201 (creado).</span><span class="sxs-lookup"><span data-stu-id="63cb3-257">But according to the HTTP/1.1 protocol, when a POST request results in the creation of a resource, the server should reply with status 201 (Created).</span></span>
- <span data-ttu-id="63cb3-258">**Ubicación:** cuando el servidor crea un recurso, debe incluir el URI del recurso nuevo en el encabezado Location de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="63cb3-258">**Location:** When the server creates a resource, it should include the URI of the new resource in the Location header of the response.</span></span>

<span data-ttu-id="63cb3-259">ASP.NET Web API facilita la manipular el mensaje de respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="63cb3-259">ASP.NET Web API makes it easy to manipulate the HTTP response message.</span></span> <span data-ttu-id="63cb3-260">Aquí se muestra la implementación mejorada:</span><span class="sxs-lookup"><span data-stu-id="63cb3-260">Here is the improved implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

<span data-ttu-id="63cb3-261">Observe que el tipo de valor devuelto del método es ahora **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="63cb3-261">Notice that the method return type is now **HttpResponseMessage**.</span></span> <span data-ttu-id="63cb3-262">Devolviendo un **HttpResponseMessage** en lugar de un producto, podemos controlar los detalles del mensaje de respuesta HTTP, incluido el código de estado y el encabezado de ubicación.</span><span class="sxs-lookup"><span data-stu-id="63cb3-262">By returning an **HttpResponseMessage** instead of a Product, we can control the details of the HTTP response message, including the status code and the Location header.</span></span>

<span data-ttu-id="63cb3-263">El **CreateResponse** método crea un **HttpResponseMessage** y automáticamente se escribe una representación serializada del objeto Product en el cuerpo fo el mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="63cb3-263">The **CreateResponse** method creates an **HttpResponseMessage** and automatically writes a serialized representation of the Product object into the body fo the response message.</span></span>

> [!NOTE]
> <span data-ttu-id="63cb3-264">En este ejemplo no valida el `Product`.</span><span class="sxs-lookup"><span data-stu-id="63cb3-264">This example does not validate the `Product`.</span></span> <span data-ttu-id="63cb3-265">Para obtener información acerca de la validación del modelo, vea [validación del modelo en ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="63cb3-265">For information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>


## <a name="updating-a-resource"></a><span data-ttu-id="63cb3-266">Actualización de un recurso</span><span class="sxs-lookup"><span data-stu-id="63cb3-266">Updating a Resource</span></span>

<span data-ttu-id="63cb3-267">Actualizar un producto con PUT es sencillo:</span><span class="sxs-lookup"><span data-stu-id="63cb3-267">Updating a product with PUT is straightforward:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

<span data-ttu-id="63cb3-268">El nombre del método empieza con &quot;colocar... &quot;, por lo que la API Web hace coincidir con las solicitudes PUT.</span><span class="sxs-lookup"><span data-stu-id="63cb3-268">The method name starts with &quot;Put...&quot;, so Web API matches it to PUT requests.</span></span> <span data-ttu-id="63cb3-269">El método toma dos parámetros, el identificador de producto y el producto actualizado.</span><span class="sxs-lookup"><span data-stu-id="63cb3-269">The method takes two parameters, the product ID and the updated product.</span></span> <span data-ttu-id="63cb3-270">El *identificador* parámetro procede de la ruta de acceso URI y el *producto* parámetro se deserializa el cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="63cb3-270">The *id* parameter is taken from the URI path, and the *product* parameter is deserialized from the request body.</span></span> <span data-ttu-id="63cb3-271">De forma predeterminada, el marco de trabajo de ASP.NET Web API tiene tipos de parámetro simples desde la ruta de acceso y tipos complejos en el cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="63cb3-271">By default, the ASP.NET Web API framework takes simple parameter types from the route and complex types from the request body.</span></span>

## <a name="deleting-a-resource"></a><span data-ttu-id="63cb3-272">Si se elimina un recurso</span><span class="sxs-lookup"><span data-stu-id="63cb3-272">Deleting a Resource</span></span>

<span data-ttu-id="63cb3-273">Para eliminar un recurso, defina un método de "Eliminar...".</span><span class="sxs-lookup"><span data-stu-id="63cb3-273">To delete a resourse, define a "Delete..." method.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

<span data-ttu-id="63cb3-274">Si una solicitud de eliminación se realiza correctamente, puede devolver el estado 200 (OK) con un cuerpo de entidad que describe el estado; estado 202 (aceptado) si la eliminación sigue siendo pendiente; o estado 204 (sin contenido) con ningún cuerpo de la entidad.</span><span class="sxs-lookup"><span data-stu-id="63cb3-274">If a DELETE request succeeds, it can return status 200 (OK) with an entity-body that describes the status; status 202 (Accepted) if the deletion is still pending; or status 204 (No Content) with no entity body.</span></span> <span data-ttu-id="63cb3-275">En este caso, el `DeleteProduct` método tiene un `void` tipo de valor devuelto, por lo que ASP.NET Web API lo convierte automáticamente en el estado de código 204 (sin contenido).</span><span class="sxs-lookup"><span data-stu-id="63cb3-275">In this case, the `DeleteProduct` method has a `void` return type, so ASP.NET Web API automatically translates this into status code 204 (No Content).</span></span>
