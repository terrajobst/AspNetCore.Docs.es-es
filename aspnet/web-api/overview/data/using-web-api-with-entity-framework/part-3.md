---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Usar migraciones de Code First para inicializar la base de datos | Documentos de Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 33bc6d82daa9ca5f46452a1adf4e2eebea04fa6c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869938"
---
<a name="use-code-first-migrations-to-seed-the-database"></a><span data-ttu-id="86f8f-102">Usar migraciones de Code First para inicializar la base de datos</span><span class="sxs-lookup"><span data-stu-id="86f8f-102">Use Code First Migrations to Seed the Database</span></span>
====================
<span data-ttu-id="86f8f-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="86f8f-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="86f8f-104">Descargar el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="86f8f-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="86f8f-105">En esta sección, va a usar [migraciones de Code First](https://msdn.microsoft.com/data/jj591621) en EF para inicializar la base de datos con datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="86f8f-105">In this section, you will use [Code First Migrations](https://msdn.microsoft.com/data/jj591621) in EF to seed the database with test data.</span></span>

<span data-ttu-id="86f8f-106">Desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**, a continuación, seleccione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="86f8f-106">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="86f8f-107">En la ventana de la consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="86f8f-107">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-3/samples/sample1.cmd)]

<span data-ttu-id="86f8f-108">Este comando agrega una carpeta denominada migraciones a su proyecto, además de un archivo de código con el nombre de archivo Configuration.cs que en la carpeta Migrations.</span><span class="sxs-lookup"><span data-stu-id="86f8f-108">This command adds a folder named Migrations to your project, plus a code file named Configuration.cs in the Migrations folder.</span></span>

![](part-3/_static/image1.png)

<span data-ttu-id="86f8f-109">Abra el archivo del archivo Configuration.cs que.</span><span class="sxs-lookup"><span data-stu-id="86f8f-109">Open the Configuration.cs file.</span></span> <span data-ttu-id="86f8f-110">Agregue el siguiente **con** instrucción.</span><span class="sxs-lookup"><span data-stu-id="86f8f-110">Add the following **using** statement.</span></span>

[!code-csharp[Main](part-3/samples/sample2.cs)]

<span data-ttu-id="86f8f-111">A continuación, agregue el código siguiente a la **Configuration.Seed** método:</span><span class="sxs-lookup"><span data-stu-id="86f8f-111">Then add the following code to the **Configuration.Seed** method:</span></span>

[!code-csharp[Main](part-3/samples/sample3.cs)]

<span data-ttu-id="86f8f-112">En la ventana de la consola de administrador de paquetes, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="86f8f-112">In the Package Manager Console window, type the following commands:</span></span>

[!code-console[Main](part-3/samples/sample4.cmd)]

<span data-ttu-id="86f8f-113">El primer comando genera código que crea la base de datos, y el segundo comando ejecuta ese código.</span><span class="sxs-lookup"><span data-stu-id="86f8f-113">The first command generates code that creates the database, and the second command executes that code.</span></span> <span data-ttu-id="86f8f-114">La base de datos se crea de forma local, mediante [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).</span><span class="sxs-lookup"><span data-stu-id="86f8f-114">The database is created locally, using [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).</span></span>

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a><span data-ttu-id="86f8f-115">Explorar la API (opcional)</span><span class="sxs-lookup"><span data-stu-id="86f8f-115">Explore the API (Optional)</span></span>

<span data-ttu-id="86f8f-116">Presione F5 para ejecutar la aplicación en modo de depuración.</span><span class="sxs-lookup"><span data-stu-id="86f8f-116">Press F5 to run the application in debug mode.</span></span> <span data-ttu-id="86f8f-117">Visual Studio inicia IIS Express y ejecuta la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="86f8f-117">Visual Studio starts IIS Express and runs your web app.</span></span> <span data-ttu-id="86f8f-118">Visual Studio, a continuación, inicia un explorador y abre la página principal de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="86f8f-118">Visual Studio then launches a browser and opens the app's home page.</span></span>

<span data-ttu-id="86f8f-119">Cuando Visual Studio se ejecuta un proyecto web, le asigna a un número de puerto.</span><span class="sxs-lookup"><span data-stu-id="86f8f-119">When Visual Studio runs a web project, it assigns a port number.</span></span> <span data-ttu-id="86f8f-120">En la imagen siguiente, el número de puerto es 50524.</span><span class="sxs-lookup"><span data-stu-id="86f8f-120">In the image below, the port number is 50524.</span></span> <span data-ttu-id="86f8f-121">Al ejecutar la aplicación, verá un número de puerto diferente.</span><span class="sxs-lookup"><span data-stu-id="86f8f-121">When you run the application, you'll see a different port number.</span></span>

![](part-3/_static/image3.png)

<span data-ttu-id="86f8f-122">La página de inicio se implementa mediante ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="86f8f-122">The home page is implemented using ASP.NET MVC.</span></span> <span data-ttu-id="86f8f-123">En la parte superior de la página, hay un vínculo que dice "API".</span><span class="sxs-lookup"><span data-stu-id="86f8f-123">At the top of the page, there is a link that says "API".</span></span> <span data-ttu-id="86f8f-124">Este vínculo abre una página de ayuda generada automáticamente para la API web.</span><span class="sxs-lookup"><span data-stu-id="86f8f-124">This link brings you to an auto-generated help page for the web API.</span></span> <span data-ttu-id="86f8f-125">(Para obtener información sobre cómo se genera esta página de ayuda, y cómo puede agregar su propia documentación a la página, vea [crear páginas de ayuda para ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) Puede hacer clic en la Ayuda de vínculos de página para ver detalles acerca de la API, incluido el formato de solicitud y respuesta.</span><span class="sxs-lookup"><span data-stu-id="86f8f-125">(To learn how this help page is generated, and how you can add your own documentation to the page, see [Creating Help Pages for ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) You can click on the help page links to see details about the API, including the request and response format.</span></span>

![](part-3/_static/image4.png)

<span data-ttu-id="86f8f-126">La API permite operaciones CRUD en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="86f8f-126">The API enables CRUD operations on the database.</span></span> <span data-ttu-id="86f8f-127">A continuación resume la API.</span><span class="sxs-lookup"><span data-stu-id="86f8f-127">The following summarizes the API.</span></span>

| <span data-ttu-id="86f8f-128">Authors</span><span class="sxs-lookup"><span data-stu-id="86f8f-128">Authors</span></span> |  |
| --- | -- |
| <span data-ttu-id="86f8f-129">OBTENER api/autores</span><span class="sxs-lookup"><span data-stu-id="86f8f-129">GET api/authors</span></span> | <span data-ttu-id="86f8f-130">Obtener a todos los autores.</span><span class="sxs-lookup"><span data-stu-id="86f8f-130">Get all authors.</span></span> |
| <span data-ttu-id="86f8f-131">Api GET/autores / {id}</span><span class="sxs-lookup"><span data-stu-id="86f8f-131">GET api/authors/{id}</span></span> | <span data-ttu-id="86f8f-132">Obtener a un autor por identificador.</span><span class="sxs-lookup"><span data-stu-id="86f8f-132">Get an author by ID.</span></span> |
| <span data-ttu-id="86f8f-133">Autores/api/POST</span><span class="sxs-lookup"><span data-stu-id="86f8f-133">POST /api/authors</span></span> | <span data-ttu-id="86f8f-134">Crear a un nuevo autor.</span><span class="sxs-lookup"><span data-stu-id="86f8f-134">Create a new author.</span></span> |
| <span data-ttu-id="86f8f-135">PUT /api/authors/{id}</span><span class="sxs-lookup"><span data-stu-id="86f8f-135">PUT /api/authors/{id}</span></span> | <span data-ttu-id="86f8f-136">Actualizar a un autor existente.</span><span class="sxs-lookup"><span data-stu-id="86f8f-136">Update an existing author.</span></span> |
| <span data-ttu-id="86f8f-137">Eliminar/API/autores / {id}</span><span class="sxs-lookup"><span data-stu-id="86f8f-137">DELETE /api/authors/{id}</span></span> | <span data-ttu-id="86f8f-138">Eliminar a un autor.</span><span class="sxs-lookup"><span data-stu-id="86f8f-138">Delete an author.</span></span> |

| <span data-ttu-id="86f8f-139">Libros</span><span class="sxs-lookup"><span data-stu-id="86f8f-139">Books</span></span> |  |
| --- | -- |
| <span data-ttu-id="86f8f-140">OBTENER /api/books</span><span class="sxs-lookup"><span data-stu-id="86f8f-140">GET /api/books</span></span> | <span data-ttu-id="86f8f-141">Obtener todos los libros.</span><span class="sxs-lookup"><span data-stu-id="86f8f-141">Get all books.</span></span> |
| <span data-ttu-id="86f8f-142">GET /api/books/{id}</span><span class="sxs-lookup"><span data-stu-id="86f8f-142">GET /api/books/{id}</span></span> | <span data-ttu-id="86f8f-143">Obtener un libro por identificador.</span><span class="sxs-lookup"><span data-stu-id="86f8f-143">Get a book by ID.</span></span> |
| <span data-ttu-id="86f8f-144">REGISTRAR/api/libros</span><span class="sxs-lookup"><span data-stu-id="86f8f-144">POST /api/books</span></span> | <span data-ttu-id="86f8f-145">Crear un nuevo libro.</span><span class="sxs-lookup"><span data-stu-id="86f8f-145">Create a new book.</span></span> |
| <span data-ttu-id="86f8f-146">PUT /api/books/{id}</span><span class="sxs-lookup"><span data-stu-id="86f8f-146">PUT /api/books/{id}</span></span> | <span data-ttu-id="86f8f-147">Actualizar un libro existente.</span><span class="sxs-lookup"><span data-stu-id="86f8f-147">Update an existing book.</span></span> |
| <span data-ttu-id="86f8f-148">Eliminar/API/libros / {id}</span><span class="sxs-lookup"><span data-stu-id="86f8f-148">DELETE /api/books/{id}</span></span> | <span data-ttu-id="86f8f-149">Eliminar un libro.</span><span class="sxs-lookup"><span data-stu-id="86f8f-149">Delete a book.</span></span> |

## <a name="view-the-database-optional"></a><span data-ttu-id="86f8f-150">Vista de la base de datos (opcional)</span><span class="sxs-lookup"><span data-stu-id="86f8f-150">View the Database (Optional)</span></span>

<span data-ttu-id="86f8f-151">Cuando ejecutó el comando Update-Database, EF crea la base de datos y se llama el `Seed` método.</span><span class="sxs-lookup"><span data-stu-id="86f8f-151">When you ran the Update-Database command, EF created the database and called the `Seed` method.</span></span> <span data-ttu-id="86f8f-152">Cuando se ejecuta la aplicación localmente, usa EF [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span><span class="sxs-lookup"><span data-stu-id="86f8f-152">When you run the application locally, EF uses [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span></span> <span data-ttu-id="86f8f-153">Puede ver la base de datos en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="86f8f-153">You can view the database in Visual Studio.</span></span> <span data-ttu-id="86f8f-154">Desde el **vista** menú, seleccione **Explorador de objetos de SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="86f8f-154">From the **View** menu, select **SQL Server Object Explorer**.</span></span>

![](part-3/_static/image5.png)

<span data-ttu-id="86f8f-155">En el **conectar al servidor** cuadro de diálogo, en la **nombre del servidor** cuadro de edición, escriba "(localdb) \v11.0".</span><span class="sxs-lookup"><span data-stu-id="86f8f-155">In the **Connect to Server** dialog, in the **Server Name** edit box, type "(localdb)\v11.0".</span></span> <span data-ttu-id="86f8f-156">Deje el **autenticación** opción como "Autenticación de Windows".</span><span class="sxs-lookup"><span data-stu-id="86f8f-156">Leave the **Authentication** option as "Windows Authentication".</span></span> <span data-ttu-id="86f8f-157">Haga clic en **conectar**.</span><span class="sxs-lookup"><span data-stu-id="86f8f-157">Click **Connect**.</span></span>

![](part-3/_static/image6.png)

<span data-ttu-id="86f8f-158">Visual Studio se conecta a LocalDB y muestra las bases de datos existentes en la ventana del explorador de objetos de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="86f8f-158">Visual Studio connects to LocalDB and shows your existing databases in the SQL Server Object Explorer window.</span></span> <span data-ttu-id="86f8f-159">Puede expandir los nodos para ver las tablas que EF creado.</span><span class="sxs-lookup"><span data-stu-id="86f8f-159">You can expand the nodes to see the tables that EF created.</span></span>

![](part-3/_static/image7.png)

<span data-ttu-id="86f8f-160">Para ver los datos, haga clic en una tabla y seleccione **ver datos**.</span><span class="sxs-lookup"><span data-stu-id="86f8f-160">To view the data, right-click a table and select **View Data**.</span></span>

![](part-3/_static/image8.png)

<span data-ttu-id="86f8f-161">Captura de pantalla siguiente muestra los resultados de la tabla de libros.</span><span class="sxs-lookup"><span data-stu-id="86f8f-161">The following screenshot shows the results for the Books table.</span></span> <span data-ttu-id="86f8f-162">Tenga en cuenta que EF rellena la base de datos con los datos de inicialización y la tabla contiene la clave externa a la tabla Authors.</span><span class="sxs-lookup"><span data-stu-id="86f8f-162">Notice that EF populated the database with the seed data, and the table contains the foreign key to the Authors table.</span></span>

![](part-3/_static/image9.png)

> [!div class="step-by-step"]
> <span data-ttu-id="86f8f-163">[Anterior](part-2.md)
> [Siguiente](part-4.md)</span><span class="sxs-lookup"><span data-stu-id="86f8f-163">[Previous](part-2.md)
[Next](part-4.md)</span></span>
