---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: "Parte 3: Crear un controlador de administración | Documentos de Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: 6fadfb6e96ae287fc5f81516b7535e03853c7e6a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="part-3-creating-an-admin-controller"></a><span data-ttu-id="31daa-102">Parte 3: Crear un controlador de administración</span><span class="sxs-lookup"><span data-stu-id="31daa-102">Part 3: Creating an Admin Controller</span></span>
====================
<span data-ttu-id="31daa-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="31daa-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="31daa-104">Descargar el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="31daa-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a><span data-ttu-id="31daa-105">Agregar un controlador de administración</span><span class="sxs-lookup"><span data-stu-id="31daa-105">Add an Admin Controller</span></span>

<span data-ttu-id="31daa-106">En esta sección, vamos a agregar un controlador de API Web que admita CRUD (crear, leer, actualizar y eliminar) operaciones sobre productos.</span><span class="sxs-lookup"><span data-stu-id="31daa-106">In this section, we'll add a Web API controller that supports CRUD (create, read, update, and delete) operations on products.</span></span> <span data-ttu-id="31daa-107">El controlador utilizará Entity Framework para comunicarse con el nivel de base de datos.</span><span class="sxs-lookup"><span data-stu-id="31daa-107">The controller will use Entity Framework to communicate with the database layer.</span></span> <span data-ttu-id="31daa-108">Sólo los administradores podrán usar este controlador.</span><span class="sxs-lookup"><span data-stu-id="31daa-108">Only administrators will be able to use this controller.</span></span> <span data-ttu-id="31daa-109">Los clientes tendrán acceso a los productos a través de otro controlador.</span><span class="sxs-lookup"><span data-stu-id="31daa-109">Customers will access the products through another controller.</span></span>

<span data-ttu-id="31daa-110">En el Explorador de soluciones, haga clic en la carpeta de controladores.</span><span class="sxs-lookup"><span data-stu-id="31daa-110">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="31daa-111">Seleccione **agregar** y, a continuación, **controlador**.</span><span class="sxs-lookup"><span data-stu-id="31daa-111">Select **Add** and then **Controller**.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

<span data-ttu-id="31daa-112">En el **Agregar controlador** cuadro de diálogo, el nombre del controlador `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="31daa-112">In the **Add Controller** dialog, name the controller `AdminController`.</span></span> <span data-ttu-id="31daa-113">En **plantilla**, seleccione &quot;controlador API con acciones de lectura/escritura, mediante Entity Framework&quot;.</span><span class="sxs-lookup"><span data-stu-id="31daa-113">Under **Template**, select &quot;API controller with read/write actions, using Entity Framework&quot;.</span></span> <span data-ttu-id="31daa-114">En **clase modelo**, seleccione "Product (ProductStore.Models)".</span><span class="sxs-lookup"><span data-stu-id="31daa-114">Under **Model class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="31daa-115">En **contexto de datos**, seleccione "&lt;nuevo contexto de datos&gt;".</span><span class="sxs-lookup"><span data-stu-id="31daa-115">Under **Data Context**, select "&lt;New Data Context&gt;".</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="31daa-116">Si el **clase modelo** desplegable no muestra todas las clases de modelo, asegúrese de que ha compilado el proyecto.</span><span class="sxs-lookup"><span data-stu-id="31daa-116">If the **Model class** drop-down does not show any model classes, make sure you compiled the project.</span></span> <span data-ttu-id="31daa-117">Entity Framework usa la reflexión, por lo que necesita el ensamblado compilado.</span><span class="sxs-lookup"><span data-stu-id="31daa-117">Entity Framework uses reflection, so it needs the compiled assembly.</span></span>


<span data-ttu-id="31daa-118">Al seleccionar "&lt;nuevo contexto de datos&gt;" se abrirá la **nuevo contexto de datos** cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="31daa-118">Selecting "&lt;New Data Context&gt;" will open the **New Data Context** dialog.</span></span> <span data-ttu-id="31daa-119">El contexto de datos el nombre `ProductStore.Models.OrdersContext`.</span><span class="sxs-lookup"><span data-stu-id="31daa-119">Name the data context `ProductStore.Models.OrdersContext`.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

<span data-ttu-id="31daa-120">Haga clic en **Aceptar** para descartar la **nuevo contexto de datos** cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="31daa-120">Click **OK** to dismiss the **New Data Context** dialog.</span></span> <span data-ttu-id="31daa-121">En el **Agregar controlador** cuadro de diálogo, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="31daa-121">In the **Add Controller** dialog, click **Add**.</span></span>

<span data-ttu-id="31daa-122">Aquí es lo que se agregó al proyecto:</span><span class="sxs-lookup"><span data-stu-id="31daa-122">Here's what got added to the project:</span></span>

- <span data-ttu-id="31daa-123">Una clase denominada `OrdersContext` que se deriva de **DbContext**.</span><span class="sxs-lookup"><span data-stu-id="31daa-123">A class named `OrdersContext` that derives from **DbContext**.</span></span> <span data-ttu-id="31daa-124">Esta clase proporciona la unión entre los modelos POCO y la base de datos.</span><span class="sxs-lookup"><span data-stu-id="31daa-124">This class provides the glue between the POCO models and the database.</span></span>
- <span data-ttu-id="31daa-125">Un controlador de API Web denominado `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="31daa-125">A Web API controller named `AdminController`.</span></span> <span data-ttu-id="31daa-126">Este controlador admite operaciones CRUD en `Product` instancias.</span><span class="sxs-lookup"><span data-stu-id="31daa-126">This controller supports CRUD operations on `Product` instances.</span></span> <span data-ttu-id="31daa-127">Usa el `OrdersContext` clase para comunicarse con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="31daa-127">It uses the `OrdersContext` class to communicate with Entity Framework.</span></span>
- <span data-ttu-id="31daa-128">Una nueva cadena de conexión de base de datos en el archivo Web.config.</span><span class="sxs-lookup"><span data-stu-id="31daa-128">A new database connection string in the Web.config file.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

<span data-ttu-id="31daa-129">Abra el archivo OrdersContext.cs.</span><span class="sxs-lookup"><span data-stu-id="31daa-129">Open the OrdersContext.cs file.</span></span> <span data-ttu-id="31daa-130">Tenga en cuenta que el constructor especifica el nombre de la cadena de conexión de base de datos.</span><span class="sxs-lookup"><span data-stu-id="31daa-130">Notice that the constructor specifies the name of the database connection string.</span></span> <span data-ttu-id="31daa-131">Este nombre hace referencia a la cadena de conexión que se ha agregado al archivo Web.config.</span><span class="sxs-lookup"><span data-stu-id="31daa-131">This name refers to the connection string that was added to Web.config.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

<span data-ttu-id="31daa-132">Agregue las propiedades siguientes a la clase `OrdersContext`:</span><span class="sxs-lookup"><span data-stu-id="31daa-132">Add the following properties to the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

<span data-ttu-id="31daa-133">A **DbSet** representa un conjunto de entidades que se pueden consultar.</span><span class="sxs-lookup"><span data-stu-id="31daa-133">A **DbSet** represents a set of entities that can be queried.</span></span> <span data-ttu-id="31daa-134">Esta es la lista completa para el `OrdersContext` clase:</span><span class="sxs-lookup"><span data-stu-id="31daa-134">Here is the complete listing for the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

<span data-ttu-id="31daa-135">La `AdminController` clase define cinco métodos que implementan la funcionalidad básica de CRUD.</span><span class="sxs-lookup"><span data-stu-id="31daa-135">The `AdminController` class defines five methods that implement basic CRUD functionality.</span></span> <span data-ttu-id="31daa-136">Cada método se corresponde con un URI que el cliente puede invocar:</span><span class="sxs-lookup"><span data-stu-id="31daa-136">Each method corresponds to a URI that the client can invoke:</span></span>

| <span data-ttu-id="31daa-137">Método de controlador</span><span class="sxs-lookup"><span data-stu-id="31daa-137">Controller Method</span></span> | <span data-ttu-id="31daa-138">Descripción</span><span class="sxs-lookup"><span data-stu-id="31daa-138">Description</span></span> | <span data-ttu-id="31daa-139">Identificador URI</span><span class="sxs-lookup"><span data-stu-id="31daa-139">URI</span></span> | <span data-ttu-id="31daa-140">Método HTTP</span><span class="sxs-lookup"><span data-stu-id="31daa-140">HTTP Method</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="31daa-141">GetProducts</span><span class="sxs-lookup"><span data-stu-id="31daa-141">GetProducts</span></span> | <span data-ttu-id="31daa-142">Obtiene todos los productos.</span><span class="sxs-lookup"><span data-stu-id="31daa-142">Gets all products.</span></span> | <span data-ttu-id="31daa-143">API y productos</span><span class="sxs-lookup"><span data-stu-id="31daa-143">api/products</span></span> | <span data-ttu-id="31daa-144">GET</span><span class="sxs-lookup"><span data-stu-id="31daa-144">GET</span></span> |
| <span data-ttu-id="31daa-145">GetProduct</span><span class="sxs-lookup"><span data-stu-id="31daa-145">GetProduct</span></span> | <span data-ttu-id="31daa-146">Busca un producto por identificador.</span><span class="sxs-lookup"><span data-stu-id="31daa-146">Finds a product by ID.</span></span> | <span data-ttu-id="31daa-147">API/productos/*Id.*</span><span class="sxs-lookup"><span data-stu-id="31daa-147">api/products/*id*</span></span> | <span data-ttu-id="31daa-148">GET</span><span class="sxs-lookup"><span data-stu-id="31daa-148">GET</span></span> |
| <span data-ttu-id="31daa-149">PutProduct</span><span class="sxs-lookup"><span data-stu-id="31daa-149">PutProduct</span></span> | <span data-ttu-id="31daa-150">Actualiza un producto.</span><span class="sxs-lookup"><span data-stu-id="31daa-150">Updates a product.</span></span> | <span data-ttu-id="31daa-151">API/productos/*Id.*</span><span class="sxs-lookup"><span data-stu-id="31daa-151">api/products/*id*</span></span> | <span data-ttu-id="31daa-152">PUT</span><span class="sxs-lookup"><span data-stu-id="31daa-152">PUT</span></span> |
| <span data-ttu-id="31daa-153">PostProduct</span><span class="sxs-lookup"><span data-stu-id="31daa-153">PostProduct</span></span> | <span data-ttu-id="31daa-154">Crea un nuevo producto.</span><span class="sxs-lookup"><span data-stu-id="31daa-154">Creates a new product.</span></span> | <span data-ttu-id="31daa-155">API y productos</span><span class="sxs-lookup"><span data-stu-id="31daa-155">api/products</span></span> | <span data-ttu-id="31daa-156">EXPONER</span><span class="sxs-lookup"><span data-stu-id="31daa-156">POST</span></span> |
| <span data-ttu-id="31daa-157">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="31daa-157">DeleteProduct</span></span> | <span data-ttu-id="31daa-158">Elimina un producto.</span><span class="sxs-lookup"><span data-stu-id="31daa-158">Deletes a product.</span></span> | <span data-ttu-id="31daa-159">API/productos/*Id.*</span><span class="sxs-lookup"><span data-stu-id="31daa-159">api/products/*id*</span></span> | <span data-ttu-id="31daa-160">SUPRIMIR</span><span class="sxs-lookup"><span data-stu-id="31daa-160">DELETE</span></span> |

<span data-ttu-id="31daa-161">Cada método llama a `OrdersContext` para consultar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="31daa-161">Each method calls into `OrdersContext` to query the database.</span></span> <span data-ttu-id="31daa-162">Llamar los métodos que modifican la colección (PUT, POST y DELETE) `db.SaveChanges` para conservar los cambios a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="31daa-162">The methods that modify the collection (PUT, POST, and DELETE) call `db.SaveChanges` to persist the changes to the database.</span></span> <span data-ttu-id="31daa-163">Los controladores se crean por cada solicitud HTTP y, a continuación, elimina, por lo que es necesario conservar los cambios antes de que un método devuelve.</span><span class="sxs-lookup"><span data-stu-id="31daa-163">Controllers are created per HTTP request and then disposed, so it is necessary to persist changes before a method returns.</span></span>

## <a name="add-a-database-initializer"></a><span data-ttu-id="31daa-164">Agregar a un inicializador de base de datos</span><span class="sxs-lookup"><span data-stu-id="31daa-164">Add a Database Initializer</span></span>

<span data-ttu-id="31daa-165">Entity Framework tiene una característica interesante que permite rellenar la base de datos en el inicio y vuelva a crear automáticamente la base de datos cada vez que cambian los modelos.</span><span class="sxs-lookup"><span data-stu-id="31daa-165">Entity Framework has a nice feature that lets you populate the database on startup, and automatically recreate the database whenever the models change.</span></span> <span data-ttu-id="31daa-166">Esta característica es útil durante el desarrollo, porque siempre hay algunos datos de prueba, incluso si cambia los modelos.</span><span class="sxs-lookup"><span data-stu-id="31daa-166">This feature is useful during development, because you always have some test data, even if you change the models.</span></span>

<span data-ttu-id="31daa-167">En el Explorador de soluciones, haga clic en la carpeta modelos y cree una nueva clase denominada `OrdersContextInitializer`.</span><span class="sxs-lookup"><span data-stu-id="31daa-167">In Solution Explorer, right-click the Models folder and create a new class named `OrdersContextInitializer`.</span></span> <span data-ttu-id="31daa-168">Pegue la siguiente implementación:</span><span class="sxs-lookup"><span data-stu-id="31daa-168">Paste in the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

<span data-ttu-id="31daa-169">Al heredar de la **DropCreateDatabaseIfModelChanges** (clase), se está indicando Entity Framework para quitar la base de datos cada vez que se modifica las clases del modelo.</span><span class="sxs-lookup"><span data-stu-id="31daa-169">By inheriting from the **DropCreateDatabaseIfModelChanges** class, we are telling Entity Framework to drop the database whenever we modify the model classes.</span></span> <span data-ttu-id="31daa-170">Cuando Entity Framework crea o vuelve a crear la base de datos, se llama a la **inicialización** método para rellenar las tablas.</span><span class="sxs-lookup"><span data-stu-id="31daa-170">When Entity Framework creates (or recreates) the database, it calls the **Seed** method to populate the tables.</span></span> <span data-ttu-id="31daa-171">Usamos el **inicialización** método para agregar un pedido de ejemplo además de algunos productos de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="31daa-171">We use the **Seed** method to add some example products plus an example order.</span></span>

<span data-ttu-id="31daa-172">Esta característica es muy útil para pruebas, pero no use la **DropCreateDatabaseIfModelChanges** clase en producción, porque podría perder los datos si alguien cambie una clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="31daa-172">This feature is great for testing, but don't use the **DropCreateDatabaseIfModelChanges** class in production,, because you could lose your data if someone changes a model class.</span></span>

<span data-ttu-id="31daa-173">A continuación, abra Global.asax y agregue el código siguiente a la **aplicación\_iniciar** método:</span><span class="sxs-lookup"><span data-stu-id="31daa-173">Next, open Global.asax and add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a><span data-ttu-id="31daa-174">Envía una solicitud al controlador</span><span class="sxs-lookup"><span data-stu-id="31daa-174">Send a Request to the Controller</span></span>

<span data-ttu-id="31daa-175">En este momento, que no hemos escrito el código de cliente, pero puede invocar la API mediante un explorador web o una depuración HTTP como herramienta de web [Fiddler](http://www.fiddler2.com/fiddler2/).</span><span class="sxs-lookup"><span data-stu-id="31daa-175">At this point, we haven't written any client code, but you can invoke the web API using a web browser or an HTTP debugging tool such as [Fiddler](http://www.fiddler2.com/fiddler2/).</span></span> <span data-ttu-id="31daa-176">En Visual Studio, presione F5 para iniciar la depuración.</span><span class="sxs-lookup"><span data-stu-id="31daa-176">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="31daa-177">Se abrirá el explorador web en `http://localhost:*portnum*/`, donde *portnum* es un número de puerto.</span><span class="sxs-lookup"><span data-stu-id="31daa-177">Your web browser will open to `http://localhost:*portnum*/`, where *portnum* is some port number.</span></span>

<span data-ttu-id="31daa-178">Enviar una solicitud HTTP a "`http://localhost:*portnum*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="31daa-178">Send an HTTP request to "`http://localhost:*portnum*/api/admin`.</span></span> <span data-ttu-id="31daa-179">La primera solicitud puede ser tarda mucho en completarse, porque el marco de trabajo de Entify necesita crear e inicializar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="31daa-179">The first request may be slow to complete, because Entify Framework needs to create and seed the database.</span></span> <span data-ttu-id="31daa-180">La respuesta debe ser algo similar a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="31daa-180">The response should something similar to the following:</span></span>

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

>[!div class="step-by-step"]
<span data-ttu-id="31daa-181">[Anterior](using-web-api-with-entity-framework-part-2.md)
[Siguiente](using-web-api-with-entity-framework-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="31daa-181">[Previous](using-web-api-with-entity-framework-part-2.md)
[Next](using-web-api-with-entity-framework-part-4.md)</span></span>
