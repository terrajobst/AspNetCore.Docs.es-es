---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Habilitar las operaciones de CRUD en ASP.NET Web API 1 | Microsoft Docs
author: MikeWasson
description: Este tutorial muestra cómo admitir operaciones CRUD en un servicio HTTP mediante ASP.NET Web API. Versiones de software que se usa en el tutorial Web AP de Visual Studio 2012...
ms.author: riande
ms.date: 01/28/2012
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: ba061b26b8527e447f25f6046057542a54f989a8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829378"
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a><span data-ttu-id="ebbab-104">Habilitar las operaciones de CRUD en ASP.NET Web API 1</span><span class="sxs-lookup"><span data-stu-id="ebbab-104">Enabling CRUD Operations in ASP.NET Web API 1</span></span>
====================
<span data-ttu-id="ebbab-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ebbab-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ebbab-106">Descargue el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="ebbab-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> <span data-ttu-id="ebbab-107">Este tutorial muestra cómo admitir operaciones CRUD en un servicio HTTP mediante ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="ebbab-107">This tutorial shows how to support CRUD operations in an HTTP service using ASP.NET Web API.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ebbab-108">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="ebbab-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="ebbab-109">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="ebbab-109">Visual Studio 2012</span></span>
> - <span data-ttu-id="ebbab-110">Web API 1 (que también funciona con Web API 2)</span><span class="sxs-lookup"><span data-stu-id="ebbab-110">Web API 1 (also works with Web API 2)</span></span>


<span data-ttu-id="ebbab-111">Es el acrónimo CRUD &quot;creación, lectura, actualización y eliminación,&quot; cuáles son las cuatro operaciones de base de datos básica.</span><span class="sxs-lookup"><span data-stu-id="ebbab-111">CRUD stands for &quot;Create, Read, Update, and Delete,&quot; which are the four basic database operations.</span></span> <span data-ttu-id="ebbab-112">Muchos servicios HTTP también modelar las operaciones de CRUD a través de REST o API de REST.</span><span class="sxs-lookup"><span data-stu-id="ebbab-112">Many HTTP services also model CRUD operations through REST or REST-like APIs.</span></span>

<span data-ttu-id="ebbab-113">En este tutorial, compilará una API para administrar una lista de productos de web muy sencilla.</span><span class="sxs-lookup"><span data-stu-id="ebbab-113">In this tutorial, you will build a very simple web API to manage a list of products.</span></span> <span data-ttu-id="ebbab-114">Cada producto contendrá un nombre, precio y la categoría (como &quot;toys&quot; o &quot;hardware&quot;), además de un identificador de producto.</span><span class="sxs-lookup"><span data-stu-id="ebbab-114">Each product will contain a name, price, and category (such as &quot;toys&quot; or &quot;hardware&quot;), plus a product ID.</span></span>

<span data-ttu-id="ebbab-115">Los productos de API expone los siguientes métodos.</span><span class="sxs-lookup"><span data-stu-id="ebbab-115">The products API will expose following methods.</span></span>

| <span data-ttu-id="ebbab-116">Acción</span><span class="sxs-lookup"><span data-stu-id="ebbab-116">Action</span></span> | <span data-ttu-id="ebbab-117">Método HTTP</span><span class="sxs-lookup"><span data-stu-id="ebbab-117">HTTP method</span></span> | <span data-ttu-id="ebbab-118">URI relativo</span><span class="sxs-lookup"><span data-stu-id="ebbab-118">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ebbab-119">Obtener una lista de todos los productos</span><span class="sxs-lookup"><span data-stu-id="ebbab-119">Get a list of all products</span></span> | <span data-ttu-id="ebbab-120">GET</span><span class="sxs-lookup"><span data-stu-id="ebbab-120">GET</span></span> | <span data-ttu-id="ebbab-121">productos/api /</span><span class="sxs-lookup"><span data-stu-id="ebbab-121">/api/products</span></span> |
| <span data-ttu-id="ebbab-122">Obtener un producto por Id.</span><span class="sxs-lookup"><span data-stu-id="ebbab-122">Get a product by ID</span></span> | <span data-ttu-id="ebbab-123">GET</span><span class="sxs-lookup"><span data-stu-id="ebbab-123">GET</span></span> | <span data-ttu-id="ebbab-124">/API/productos/*Id.*</span><span class="sxs-lookup"><span data-stu-id="ebbab-124">/api/products/*id*</span></span> |
| <span data-ttu-id="ebbab-125">Obtener un producto por categoría</span><span class="sxs-lookup"><span data-stu-id="ebbab-125">Get a product by category</span></span> | <span data-ttu-id="ebbab-126">GET</span><span class="sxs-lookup"><span data-stu-id="ebbab-126">GET</span></span> | <span data-ttu-id="ebbab-127">productos/api /? categoría =*categoría*</span><span class="sxs-lookup"><span data-stu-id="ebbab-127">/api/products?category=*category*</span></span> |
| <span data-ttu-id="ebbab-128">Crear un nuevo producto</span><span class="sxs-lookup"><span data-stu-id="ebbab-128">Create a new product</span></span> | <span data-ttu-id="ebbab-129">EXPONER</span><span class="sxs-lookup"><span data-stu-id="ebbab-129">POST</span></span> | <span data-ttu-id="ebbab-130">productos/api /</span><span class="sxs-lookup"><span data-stu-id="ebbab-130">/api/products</span></span> |
| <span data-ttu-id="ebbab-131">Actualizar un producto</span><span class="sxs-lookup"><span data-stu-id="ebbab-131">Update a product</span></span> | <span data-ttu-id="ebbab-132">PUT</span><span class="sxs-lookup"><span data-stu-id="ebbab-132">PUT</span></span> | <span data-ttu-id="ebbab-133">/API/productos/*Id.*</span><span class="sxs-lookup"><span data-stu-id="ebbab-133">/api/products/*id*</span></span> |
| <span data-ttu-id="ebbab-134">Eliminar un producto</span><span class="sxs-lookup"><span data-stu-id="ebbab-134">Delete a product</span></span> | <span data-ttu-id="ebbab-135">SUPRIMIR</span><span class="sxs-lookup"><span data-stu-id="ebbab-135">DELETE</span></span> | <span data-ttu-id="ebbab-136">/API/productos/*Id.*</span><span class="sxs-lookup"><span data-stu-id="ebbab-136">/api/products/*id*</span></span> |

<span data-ttu-id="ebbab-137">Tenga en cuenta que algunos de los URI incluyen el identificador de producto en la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="ebbab-137">Notice that some of the URIs include the product ID in path.</span></span> <span data-ttu-id="ebbab-138">Por ejemplo, para obtener el producto cuyo identificador es 28, el cliente envía una solicitud GET `http://hostname/api/products/28`.</span><span class="sxs-lookup"><span data-stu-id="ebbab-138">For example, to get the product whose ID is 28, the client sends a GET request for `http://hostname/api/products/28`.</span></span>

### <a name="resources"></a><span data-ttu-id="ebbab-139">Recursos</span><span class="sxs-lookup"><span data-stu-id="ebbab-139">Resources</span></span>

<span data-ttu-id="ebbab-140">Los productos de API define los identificadores URI para dos tipos de recursos:</span><span class="sxs-lookup"><span data-stu-id="ebbab-140">The products API defines URIs for two resource types:</span></span>

| <span data-ttu-id="ebbab-141">Recurso</span><span class="sxs-lookup"><span data-stu-id="ebbab-141">Resource</span></span> | <span data-ttu-id="ebbab-142">Identificador URI</span><span class="sxs-lookup"><span data-stu-id="ebbab-142">URI</span></span> |
| --- | --- |
| <span data-ttu-id="ebbab-143">La lista de todos los productos.</span><span class="sxs-lookup"><span data-stu-id="ebbab-143">The list of all the products.</span></span> | <span data-ttu-id="ebbab-144">productos/api /</span><span class="sxs-lookup"><span data-stu-id="ebbab-144">/api/products</span></span> |
| <span data-ttu-id="ebbab-145">Un producto individual.</span><span class="sxs-lookup"><span data-stu-id="ebbab-145">An individual product.</span></span> | <span data-ttu-id="ebbab-146">/API/productos/*Id.*</span><span class="sxs-lookup"><span data-stu-id="ebbab-146">/api/products/*id*</span></span> |

### <a name="methods"></a><span data-ttu-id="ebbab-147">Métodos</span><span class="sxs-lookup"><span data-stu-id="ebbab-147">Methods</span></span>

<span data-ttu-id="ebbab-148">Los cuatro principales métodos HTTP (GET, PUT, POST y DELETE) se pueden asignar a las operaciones de CRUD como sigue:</span><span class="sxs-lookup"><span data-stu-id="ebbab-148">The four main HTTP methods (GET, PUT, POST, and DELETE) can be mapped to CRUD operations as follows:</span></span>

- <span data-ttu-id="ebbab-149">GET recupera la representación del recurso en un URI especificado.</span><span class="sxs-lookup"><span data-stu-id="ebbab-149">GET retrieves the representation of the resource at a specified URI.</span></span> <span data-ttu-id="ebbab-150">GET no debe tener ningún efecto secundario en el servidor.</span><span class="sxs-lookup"><span data-stu-id="ebbab-150">GET should have no side effects on the server.</span></span>
- <span data-ttu-id="ebbab-151">PUT actualiza un recurso en un URI especificado.</span><span class="sxs-lookup"><span data-stu-id="ebbab-151">PUT updates a resource at a specified URI.</span></span> <span data-ttu-id="ebbab-152">PUT puede usarse para crear un nuevo recurso en un URI especificado, si el servidor permite a los clientes especificar a nuevos URI.</span><span class="sxs-lookup"><span data-stu-id="ebbab-152">PUT can also be used to create a new resource at a specified URI, if the server allows clients to specify new URIs.</span></span> <span data-ttu-id="ebbab-153">Para este tutorial, la API no admitirá la creación mediante PUT.</span><span class="sxs-lookup"><span data-stu-id="ebbab-153">For this tutorial, the API will not support creation through PUT.</span></span>
- <span data-ttu-id="ebbab-154">POST crea un nuevo recurso.</span><span class="sxs-lookup"><span data-stu-id="ebbab-154">POST creates a new resource.</span></span> <span data-ttu-id="ebbab-155">El servidor le asigna el URI para el nuevo objeto y devuelve este identificador URI como parte del mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="ebbab-155">The server assigns the URI for the new object and returns this URI as part of the response message.</span></span>
- <span data-ttu-id="ebbab-156">El método DELETE Elimina un recurso en un URI especificado.</span><span class="sxs-lookup"><span data-stu-id="ebbab-156">DELETE deletes a resource at a specified URI.</span></span>

<span data-ttu-id="ebbab-157">Nota: El método PUT reemplaza la entidad de producto completo.</span><span class="sxs-lookup"><span data-stu-id="ebbab-157">Note: The PUT method replaces the entire product entity.</span></span> <span data-ttu-id="ebbab-158">Es decir, se espera que el cliente envía una representación completa del producto actualizada.</span><span class="sxs-lookup"><span data-stu-id="ebbab-158">That is, the client is expected to send a complete representation of the updated product.</span></span> <span data-ttu-id="ebbab-159">Si desea admitir actualizaciones parciales, se prefiere el método de revisión.</span><span class="sxs-lookup"><span data-stu-id="ebbab-159">If you want to support partial updates, the PATCH method is preferred.</span></span> <span data-ttu-id="ebbab-160">En este tutorial no implementa la revisión.</span><span class="sxs-lookup"><span data-stu-id="ebbab-160">This tutorial does not implement PATCH.</span></span>

## <a name="create-a-new-web-api-project"></a><span data-ttu-id="ebbab-161">Cree un nuevo proyecto de API Web</span><span class="sxs-lookup"><span data-stu-id="ebbab-161">Create a New Web API Project</span></span>

<span data-ttu-id="ebbab-162">Comience ejecutando Visual Studio y seleccione **nuevo proyecto** desde el **iniciar** página.</span><span class="sxs-lookup"><span data-stu-id="ebbab-162">Start by running Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="ebbab-163">O bien, en el **archivo** menú, seleccione **New** y, a continuación, **proyecto**.</span><span class="sxs-lookup"><span data-stu-id="ebbab-163">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="ebbab-164">En el **plantillas** panel, seleccione **plantillas instaladas** y expanda el **Visual C#** nodo.</span><span class="sxs-lookup"><span data-stu-id="ebbab-164">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="ebbab-165">En **Visual C#**, seleccione **Web**.</span><span class="sxs-lookup"><span data-stu-id="ebbab-165">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="ebbab-166">En la lista de plantillas de proyecto, seleccione **aplicación Web de ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="ebbab-166">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="ebbab-167">Denomine el proyecto &quot;ProductStore&quot; y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="ebbab-167">Name the project &quot;ProductStore&quot; and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

<span data-ttu-id="ebbab-168">En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione **API Web** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="ebbab-168">In the **New ASP.NET MVC 4 Project** dialog, select **Web API** and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a><span data-ttu-id="ebbab-169">Adición de un modelo</span><span class="sxs-lookup"><span data-stu-id="ebbab-169">Adding a Model</span></span>

<span data-ttu-id="ebbab-170">Un *modelo* es un objeto que representa los datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ebbab-170">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="ebbab-171">En ASP.NET Web API, puede utilizar objetos CLR fuertemente tipados como modelos y, automáticamente se serializarán en JSON o XML para el cliente.</span><span class="sxs-lookup"><span data-stu-id="ebbab-171">In ASP.NET Web API, you can use strongly typed CLR objects as models, and they will automatically be serialized to XML or JSON for the client.</span></span>

<span data-ttu-id="ebbab-172">Para la API ProductStore, nuestros datos constan de los productos, por lo que vamos a crear una nueva clase denominada `Product`.</span><span class="sxs-lookup"><span data-stu-id="ebbab-172">For the ProductStore API, our data consists of products, so we'll create a new class named `Product`.</span></span>

<span data-ttu-id="ebbab-173">Si el Explorador de soluciones no está visible, haga clic en el **vista** menú y seleccione **el Explorador de soluciones**.</span><span class="sxs-lookup"><span data-stu-id="ebbab-173">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="ebbab-174">En el Explorador de soluciones, haga clic en el **modelos** carpeta.</span><span class="sxs-lookup"><span data-stu-id="ebbab-174">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="ebbab-175">En el menú contextual, seleccione **agregar**, a continuación, seleccione **clase**.</span><span class="sxs-lookup"><span data-stu-id="ebbab-175">From the context meny, select **Add**, then select **Class**.</span></span> <span data-ttu-id="ebbab-176">Nombre de la clase &quot;producto&quot;.</span><span class="sxs-lookup"><span data-stu-id="ebbab-176">Name the class &quot;Product&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

<span data-ttu-id="ebbab-177">Agregue las siguientes propiedades para el `Product` clase.</span><span class="sxs-lookup"><span data-stu-id="ebbab-177">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a><span data-ttu-id="ebbab-178">Adición de un repositorio</span><span class="sxs-lookup"><span data-stu-id="ebbab-178">Adding a Repository</span></span>

<span data-ttu-id="ebbab-179">Es necesario almacenar una colección de productos.</span><span class="sxs-lookup"><span data-stu-id="ebbab-179">We need to store a collection of products.</span></span> <span data-ttu-id="ebbab-180">Es una buena idea para separar la colección de nuestra implementación de servicio.</span><span class="sxs-lookup"><span data-stu-id="ebbab-180">It's a good idea to separate the collection from our service implementation.</span></span> <span data-ttu-id="ebbab-181">De este modo, podemos cambiar la memoria auxiliar sin volver a escribir la clase de servicio.</span><span class="sxs-lookup"><span data-stu-id="ebbab-181">That way, we can change the backing store without rewriting the service class.</span></span> <span data-ttu-id="ebbab-182">Se llama a este tipo de diseño de la *repositorio* patrón.</span><span class="sxs-lookup"><span data-stu-id="ebbab-182">This type of design is called the *repository* pattern.</span></span> <span data-ttu-id="ebbab-183">Empiece por definir una interfaz genérica para el repositorio.</span><span class="sxs-lookup"><span data-stu-id="ebbab-183">Start by defining a generic interface for the repository.</span></span>

<span data-ttu-id="ebbab-184">En el Explorador de soluciones, haga clic en el **modelos** carpeta.</span><span class="sxs-lookup"><span data-stu-id="ebbab-184">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="ebbab-185">Seleccione **agregar**, a continuación, seleccione **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="ebbab-185">Select **Add**, then select **New Item**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

<span data-ttu-id="ebbab-186">En el **plantillas** panel, seleccione **plantillas instaladas** y expanda el nodo de C#.</span><span class="sxs-lookup"><span data-stu-id="ebbab-186">In the **Templates** pane, select **Installed Templates** and expand the C# node.</span></span> <span data-ttu-id="ebbab-187">En C#, seleccione **código**.</span><span class="sxs-lookup"><span data-stu-id="ebbab-187">Under C#, select **Code**.</span></span> <span data-ttu-id="ebbab-188">En la lista de plantillas de código, seleccione **interfaz**.</span><span class="sxs-lookup"><span data-stu-id="ebbab-188">In the list of code templates, select **Interface**.</span></span> <span data-ttu-id="ebbab-189">Nombre de la interfaz &quot;IProductRepository&quot;.</span><span class="sxs-lookup"><span data-stu-id="ebbab-189">Name the interface &quot;IProductRepository&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

<span data-ttu-id="ebbab-190">Agregue la siguiente implementación:</span><span class="sxs-lookup"><span data-stu-id="ebbab-190">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

<span data-ttu-id="ebbab-191">Ahora agregue otra clase a la carpeta Models, denominada &quot;ProductRepository.&quot; Esta clase implementará la interfaz `IProductRespository`.</span><span class="sxs-lookup"><span data-stu-id="ebbab-191">Now add another class to the Models folder, named &quot;ProductRepository.&quot; This class will implement the `IProductRespository` interface.</span></span> <span data-ttu-id="ebbab-192">Agregue la siguiente implementación:</span><span class="sxs-lookup"><span data-stu-id="ebbab-192">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

<span data-ttu-id="ebbab-193">El repositorio mantiene la lista en la memoria local.</span><span class="sxs-lookup"><span data-stu-id="ebbab-193">The repository keeps the list in local memory.</span></span> <span data-ttu-id="ebbab-194">Esto es correcto para ver un tutorial, pero en una aplicación real, podría almacenar los datos externamente, una base de datos o en el almacenamiento en la nube.</span><span class="sxs-lookup"><span data-stu-id="ebbab-194">This is OK for a tutorial, but in a real application, you would store the data externally, either a database or in cloud storage.</span></span> <span data-ttu-id="ebbab-195">El modelo de repositorio le resultará más fácil cambiar la implementación más adelante.</span><span class="sxs-lookup"><span data-stu-id="ebbab-195">The repository pattern will make it easier to change the implementation later.</span></span>

## <a name="adding-a-web-api-controller"></a><span data-ttu-id="ebbab-196">Agregar un controlador de API Web</span><span class="sxs-lookup"><span data-stu-id="ebbab-196">Adding a Web API Controller</span></span>

<span data-ttu-id="ebbab-197">Si ha trabajado con ASP.NET MVC, a continuación, ya está familiarizados con los controladores.</span><span class="sxs-lookup"><span data-stu-id="ebbab-197">If you have worked with ASP.NET MVC, then you are already familiar with controllers.</span></span> <span data-ttu-id="ebbab-198">En ASP.NET Web API, un *controlador* es una clase que controla las solicitudes HTTP desde el cliente.</span><span class="sxs-lookup"><span data-stu-id="ebbab-198">In ASP.NET Web API, a *controller* is a class that handles HTTP requests from the client.</span></span> <span data-ttu-id="ebbab-199">El Asistente para nuevo proyecto crea dos controladores cuando crea el proyecto.</span><span class="sxs-lookup"><span data-stu-id="ebbab-199">The New Project wizard created two controllers for you when it created the project.</span></span> <span data-ttu-id="ebbab-200">Para verlas, expanda la carpeta controladores en el Explorador de soluciones.</span><span class="sxs-lookup"><span data-stu-id="ebbab-200">To see them, expand the Controllers folder in Solution Explorer.</span></span>

- <span data-ttu-id="ebbab-201">HomeController es un controlador de ASP.NET MVC tradicional.</span><span class="sxs-lookup"><span data-stu-id="ebbab-201">HomeController is a traditional ASP.NET MVC controller.</span></span> <span data-ttu-id="ebbab-202">Es responsable de servir páginas HTML para el sitio y no está directamente relacionado con nuestra web API.</span><span class="sxs-lookup"><span data-stu-id="ebbab-202">It is responsible for serving HTML pages for the site, and is not directly related to our web API.</span></span>
- <span data-ttu-id="ebbab-203">Clase ValuesController es un controlador de WebAPI de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="ebbab-203">ValuesController is an example WebAPI controller.</span></span>

<span data-ttu-id="ebbab-204">Seguir adelante y eliminar la clase ValuesController, haciendo clic en el archivo en el Explorador de soluciones y seleccionar **eliminar.**</span><span class="sxs-lookup"><span data-stu-id="ebbab-204">Go ahead and delete ValuesController, by right-clicking the file in Solution Explorer and selecting **Delete.**</span></span> <span data-ttu-id="ebbab-205">Ahora agregue un nuevo controlador, como sigue:</span><span class="sxs-lookup"><span data-stu-id="ebbab-205">Now add a new controller, as follows:</span></span>

<span data-ttu-id="ebbab-206">En **el Explorador de soluciones**, haga clic en la carpeta Controllers.</span><span class="sxs-lookup"><span data-stu-id="ebbab-206">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="ebbab-207">Seleccione **agregar** y, a continuación, seleccione **controlador**.</span><span class="sxs-lookup"><span data-stu-id="ebbab-207">Select **Add** and then select **Controller**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

<span data-ttu-id="ebbab-208">En el **Agregar controlador** asistente, el nombre del controlador &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="ebbab-208">In the **Add Controller** wizard, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="ebbab-209">En el **plantilla** lista desplegable, seleccione **controlador de API en blanco**.</span><span class="sxs-lookup"><span data-stu-id="ebbab-209">In the **Template** drop-down list, select **Empty API Controller**.</span></span> <span data-ttu-id="ebbab-210">A continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="ebbab-210">Then click **Add**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="ebbab-211">No es necesario poner los controladores en una carpeta denominada controladores.</span><span class="sxs-lookup"><span data-stu-id="ebbab-211">It is not necessary to put your contollers into a folder named Controllers.</span></span> <span data-ttu-id="ebbab-212">El nombre de carpeta no es importante; es simplemente una manera cómoda de organizar los archivos de origen.</span><span class="sxs-lookup"><span data-stu-id="ebbab-212">The folder name is not important; it is simply a convenient way to organize your source files.</span></span>


<span data-ttu-id="ebbab-213">El **Agregar controlador** asistente creará un archivo denominado ProductsController.cs en la carpeta Controllers.</span><span class="sxs-lookup"><span data-stu-id="ebbab-213">The **Add Controller** wizard will create a file named ProductsController.cs in the Controllers folder.</span></span> <span data-ttu-id="ebbab-214">Si este archivo ya no está abierto, haga doble clic en el archivo para abrirlo.</span><span class="sxs-lookup"><span data-stu-id="ebbab-214">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="ebbab-215">Agregue el siguiente **mediante** instrucción:</span><span class="sxs-lookup"><span data-stu-id="ebbab-215">Add the following **using** statement:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

<span data-ttu-id="ebbab-216">Agregar un campo que contiene un **IProductRepository** instancia.</span><span class="sxs-lookup"><span data-stu-id="ebbab-216">Add a field that holds an **IProductRepository** instance.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> <span data-ttu-id="ebbab-217">Una llamada a `new ProductRepository()` en el controlador no es el mejor diseño, porque enlaza el controlador a una implementación concreta de `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="ebbab-217">Calling `new ProductRepository()` in the controller is not the best design, because it ties the controller to a particular implementation of `IProductRepository`.</span></span> <span data-ttu-id="ebbab-218">Para un mejor enfoque, consulte [mediante la resolución de dependencia de Web API](../advanced/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="ebbab-218">For a better approach, see [Using the Web API Dependency Resolver](../advanced/dependency-injection.md).</span></span>


## <a name="getting-a-resource"></a><span data-ttu-id="ebbab-219">Obtención de un recurso</span><span class="sxs-lookup"><span data-stu-id="ebbab-219">Getting a Resource</span></span>

<span data-ttu-id="ebbab-220">La API ProductStore va a exponer varios &quot;leer&quot; acciones como métodos HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="ebbab-220">The ProductStore API will expose several &quot;read&quot; actions as HTTP GET methods.</span></span> <span data-ttu-id="ebbab-221">Cada acción se corresponderá con un método en el `ProductsController` clase.</span><span class="sxs-lookup"><span data-stu-id="ebbab-221">Each action will correspond to a method in the `ProductsController` class.</span></span>

| <span data-ttu-id="ebbab-222">Acción</span><span class="sxs-lookup"><span data-stu-id="ebbab-222">Action</span></span> | <span data-ttu-id="ebbab-223">Método HTTP</span><span class="sxs-lookup"><span data-stu-id="ebbab-223">HTTP method</span></span> | <span data-ttu-id="ebbab-224">URI relativo</span><span class="sxs-lookup"><span data-stu-id="ebbab-224">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ebbab-225">Obtener una lista de todos los productos</span><span class="sxs-lookup"><span data-stu-id="ebbab-225">Get a list of all products</span></span> | <span data-ttu-id="ebbab-226">GET</span><span class="sxs-lookup"><span data-stu-id="ebbab-226">GET</span></span> | <span data-ttu-id="ebbab-227">productos/api /</span><span class="sxs-lookup"><span data-stu-id="ebbab-227">/api/products</span></span> |
| <span data-ttu-id="ebbab-228">Obtener un producto por Id.</span><span class="sxs-lookup"><span data-stu-id="ebbab-228">Get a product by ID</span></span> | <span data-ttu-id="ebbab-229">GET</span><span class="sxs-lookup"><span data-stu-id="ebbab-229">GET</span></span> | <span data-ttu-id="ebbab-230">/API/productos/*Id.*</span><span class="sxs-lookup"><span data-stu-id="ebbab-230">/api/products/*id*</span></span> |
| <span data-ttu-id="ebbab-231">Obtener un producto por categoría</span><span class="sxs-lookup"><span data-stu-id="ebbab-231">Get a product by category</span></span> | <span data-ttu-id="ebbab-232">GET</span><span class="sxs-lookup"><span data-stu-id="ebbab-232">GET</span></span> | <span data-ttu-id="ebbab-233">productos/api /? categoría =*categoría*</span><span class="sxs-lookup"><span data-stu-id="ebbab-233">/api/products?category=*category*</span></span> |

<span data-ttu-id="ebbab-234">Para obtener la lista de todos los productos, agregue este método para el `ProductsController` clase:</span><span class="sxs-lookup"><span data-stu-id="ebbab-234">To get the list of all products, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

<span data-ttu-id="ebbab-235">El nombre del método comienza con &quot;obtener&quot;, por lo que por convención, asigna a las solicitudes GET.</span><span class="sxs-lookup"><span data-stu-id="ebbab-235">The method name starts with &quot;Get&quot;, so by convention it maps to GET requests.</span></span> <span data-ttu-id="ebbab-236">Además, dado que el método no tiene parámetros, se asigna a un URI que no contiene un *&quot;id&quot;* segmento en la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="ebbab-236">Also, because the method has no parameters, it maps to a URI that does not contain an *&quot;id&quot;* segment in the path.</span></span>

<span data-ttu-id="ebbab-237">Para obtener un producto por identificador, agregue este método para el `ProductsController` clase:</span><span class="sxs-lookup"><span data-stu-id="ebbab-237">To get a product by ID, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

<span data-ttu-id="ebbab-238">El nombre de este método también se inicia con &quot;obtener&quot;, pero el método tiene un parámetro denominado *id*. Este parámetro se asigna a la &quot;id&quot; segmento de ruta de acceso URI.</span><span class="sxs-lookup"><span data-stu-id="ebbab-238">This method name also starts with &quot;Get&quot;, but the method has a parameter named *id*. This parameter is mapped to the &quot;id&quot; segment of the URI path.</span></span> <span data-ttu-id="ebbab-239">El marco ASP.NET Web API convierte automáticamente el identificador para el tipo de datos correcto (**int**) para el parámetro.</span><span class="sxs-lookup"><span data-stu-id="ebbab-239">The ASP.NET Web API framework automatically converts the ID to the correct data type (**int**) for the parameter.</span></span>

<span data-ttu-id="ebbab-240">El método GetProduct produce una excepción de tipo **HttpResponseException** si *identificador* no es válido.</span><span class="sxs-lookup"><span data-stu-id="ebbab-240">The GetProduct method throws an exception of type **HttpResponseException** if *id* is not valid.</span></span> <span data-ttu-id="ebbab-241">Esta excepción se convertirá el marco de trabajo a un error 404 (no encontrado).</span><span class="sxs-lookup"><span data-stu-id="ebbab-241">This exception will be translated by the framework into a 404 (Not Found) error.</span></span>

<span data-ttu-id="ebbab-242">Por último, agregue un método para buscar productos por categoría:</span><span class="sxs-lookup"><span data-stu-id="ebbab-242">Finally, add a method to find products by category:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

<span data-ttu-id="ebbab-243">Si el URI de solicitud tiene una cadena de consulta, API Web intenta hacer coincidir los parámetros de consulta para los parámetros del método de controlador.</span><span class="sxs-lookup"><span data-stu-id="ebbab-243">If the request URI has a query string, Web API tries to match the query parameters to parameters on the controller method.</span></span> <span data-ttu-id="ebbab-244">Por lo tanto, un URI del formulario "api/products? categoría =*categoría*" se asignarán a este método.</span><span class="sxs-lookup"><span data-stu-id="ebbab-244">Therefore, a URI of the form "api/products?category=*category*" will map to this method.</span></span>

## <a name="creating-a-resource"></a><span data-ttu-id="ebbab-245">Creación de un recurso</span><span class="sxs-lookup"><span data-stu-id="ebbab-245">Creating a Resource</span></span>

<span data-ttu-id="ebbab-246">A continuación, agregaremos un método para el `ProductsController` clase para crear un nuevo producto.</span><span class="sxs-lookup"><span data-stu-id="ebbab-246">Next, we'll add a method to the `ProductsController` class to create a new product.</span></span> <span data-ttu-id="ebbab-247">Aquí es una implementación sencilla del método:</span><span class="sxs-lookup"><span data-stu-id="ebbab-247">Here is a simple implementation of the method:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

<span data-ttu-id="ebbab-248">Tenga en cuenta dos cosas acerca de este método:</span><span class="sxs-lookup"><span data-stu-id="ebbab-248">Note two things about this method:</span></span>

- <span data-ttu-id="ebbab-249">El nombre del método comienza con &quot;Post... &quot;.</span><span class="sxs-lookup"><span data-stu-id="ebbab-249">The method name starts with &quot;Post...&quot;.</span></span> <span data-ttu-id="ebbab-250">Para crear un nuevo producto, el cliente envía una solicitud HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="ebbab-250">To create a new product, the client sends an HTTP POST request.</span></span>
- <span data-ttu-id="ebbab-251">El método toma un parámetro de tipo Product.</span><span class="sxs-lookup"><span data-stu-id="ebbab-251">The method takes a parameter of type Product.</span></span> <span data-ttu-id="ebbab-252">En la API Web, los parámetros con tipos complejos se deserializan desde el cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="ebbab-252">In Web API, parameters with complex types are deserialized from the request body.</span></span> <span data-ttu-id="ebbab-253">Por lo tanto, se espera que el cliente envíe una representación serializada de un objeto de producto, en formato XML o JSON.</span><span class="sxs-lookup"><span data-stu-id="ebbab-253">Therefore, we expect the client to send a serialized representation of a product object, in either XML or JSON format.</span></span>

<span data-ttu-id="ebbab-254">Esta implementación funcionará, pero no es muy completa.</span><span class="sxs-lookup"><span data-stu-id="ebbab-254">This implementation will work, but it is not quite complete.</span></span> <span data-ttu-id="ebbab-255">Idealmente, nos gustaría que la respuesta HTTP debe incluir lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ebbab-255">Ideally, we would like the HTTP response to include the following:</span></span>

- <span data-ttu-id="ebbab-256">**Código de respuesta:** de forma predeterminada, el marco API Web establece el código de estado de respuesta a 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="ebbab-256">**Response code:** By default, the Web API framework sets the response status code to 200 (OK).</span></span> <span data-ttu-id="ebbab-257">Pero, según el protocolo HTTP/1.1, cuando una solicitud POST da como resultado la creación de un recurso, el servidor debe responder con el estado 201 (creado).</span><span class="sxs-lookup"><span data-stu-id="ebbab-257">But according to the HTTP/1.1 protocol, when a POST request results in the creation of a resource, the server should reply with status 201 (Created).</span></span>
- <span data-ttu-id="ebbab-258">**Ubicación:** cuando el servidor crea un recurso, debe incluir el URI del nuevo recurso en el encabezado Location de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="ebbab-258">**Location:** When the server creates a resource, it should include the URI of the new resource in the Location header of the response.</span></span>

<span data-ttu-id="ebbab-259">ASP.NET Web API simplifica manipular el mensaje de respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="ebbab-259">ASP.NET Web API makes it easy to manipulate the HTTP response message.</span></span> <span data-ttu-id="ebbab-260">Esta es la implementación mejorada:</span><span class="sxs-lookup"><span data-stu-id="ebbab-260">Here is the improved implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

<span data-ttu-id="ebbab-261">Tenga en cuenta que el tipo de valor devuelto del método es ahora **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="ebbab-261">Notice that the method return type is now **HttpResponseMessage**.</span></span> <span data-ttu-id="ebbab-262">Devolviendo un **HttpResponseMessage** en lugar de un producto, podemos controlar los detalles del mensaje de respuesta HTTP, incluidos el código de estado y el encabezado Location.</span><span class="sxs-lookup"><span data-stu-id="ebbab-262">By returning an **HttpResponseMessage** instead of a Product, we can control the details of the HTTP response message, including the status code and the Location header.</span></span>

<span data-ttu-id="ebbab-263">El **CreateResponse** método crea un **HttpResponseMessage** y automáticamente se escribe una representación serializada del objeto Product en el cuerpo fo el mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="ebbab-263">The **CreateResponse** method creates an **HttpResponseMessage** and automatically writes a serialized representation of the Product object into the body fo the response message.</span></span>

> [!NOTE]
> <span data-ttu-id="ebbab-264">En este ejemplo no valida el `Product`.</span><span class="sxs-lookup"><span data-stu-id="ebbab-264">This example does not validate the `Product`.</span></span> <span data-ttu-id="ebbab-265">Para obtener información acerca de la validación del modelo, vea [validación de modelos en ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="ebbab-265">For information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>


## <a name="updating-a-resource"></a><span data-ttu-id="ebbab-266">Actualizar un recurso</span><span class="sxs-lookup"><span data-stu-id="ebbab-266">Updating a Resource</span></span>

<span data-ttu-id="ebbab-267">Actualizar un producto con PUT es sencilla:</span><span class="sxs-lookup"><span data-stu-id="ebbab-267">Updating a product with PUT is straightforward:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

<span data-ttu-id="ebbab-268">El nombre del método comienza con &quot;colocar... &quot;, por lo que la API Web hace coincidir con las solicitudes PUT.</span><span class="sxs-lookup"><span data-stu-id="ebbab-268">The method name starts with &quot;Put...&quot;, so Web API matches it to PUT requests.</span></span> <span data-ttu-id="ebbab-269">El método toma dos parámetros, el identificador de producto y el producto actualizado.</span><span class="sxs-lookup"><span data-stu-id="ebbab-269">The method takes two parameters, the product ID and the updated product.</span></span> <span data-ttu-id="ebbab-270">El *id* parámetro procede de la ruta de acceso URI y el *producto* parámetro se deserializa desde el cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="ebbab-270">The *id* parameter is taken from the URI path, and the *product* parameter is deserialized from the request body.</span></span> <span data-ttu-id="ebbab-271">De forma predeterminada, el marco ASP.NET Web API tiene tipos de parámetro simples desde la ruta de acceso y los tipos complejos desde el cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="ebbab-271">By default, the ASP.NET Web API framework takes simple parameter types from the route and complex types from the request body.</span></span>

## <a name="deleting-a-resource"></a><span data-ttu-id="ebbab-272">Eliminar un recurso</span><span class="sxs-lookup"><span data-stu-id="ebbab-272">Deleting a Resource</span></span>

<span data-ttu-id="ebbab-273">Para eliminar un recurso, defina un método "..." eliminar".</span><span class="sxs-lookup"><span data-stu-id="ebbab-273">To delete a resourse, define a "Delete..." method.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

<span data-ttu-id="ebbab-274">Si una solicitud de eliminación se realiza correctamente, puede devolver el código de estado 200 (OK) con un cuerpo de entidad que describe el estado; estado de 202 (aceptado) si la eliminación sigue siendo pendiente; o estado 204 (sin contenido) con ningún cuerpo de entidad.</span><span class="sxs-lookup"><span data-stu-id="ebbab-274">If a DELETE request succeeds, it can return status 200 (OK) with an entity-body that describes the status; status 202 (Accepted) if the deletion is still pending; or status 204 (No Content) with no entity body.</span></span> <span data-ttu-id="ebbab-275">En este caso, el `DeleteProduct` método tiene un `void` tipo de valor devuelto, por lo que ASP.NET Web API lo traduce automáticamente en estado 204 (sin contenido) de código.</span><span class="sxs-lookup"><span data-stu-id="ebbab-275">In this case, the `DeleteProduct` method has a `void` return type, so ASP.NET Web API automatically translates this into status code 204 (No Content).</span></span>
