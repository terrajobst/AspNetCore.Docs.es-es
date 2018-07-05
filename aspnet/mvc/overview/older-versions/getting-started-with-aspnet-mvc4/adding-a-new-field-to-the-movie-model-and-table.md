---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Agregar un nuevo campo a la tabla y el modelo de película | Microsoft Docs
author: Rick-Anderson
description: 'Nota: Una versión actualizada de este tutorial está disponible aquí que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro y mucho más fácil de seguir y demostraciones...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: 25a29e783f02e66e1713d8120eb526e1a02961a3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366481"
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a><span data-ttu-id="2eaa3-104">Agregar un nuevo campo a la tabla y el modelo de película</span><span class="sxs-lookup"><span data-stu-id="2eaa3-104">Adding a New Field to the Movie Model and Table</span></span>
====================
<span data-ttu-id="2eaa3-105">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="2eaa3-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="2eaa3-106">Hay disponible una versión actualizada de este tutorial [aquí](../../getting-started/introduction/getting-started.md) que usa ASP.NET MVC 5 y Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="2eaa3-107">Es más seguro y mucho más fácil de seguir y se muestran las características más.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="2eaa3-108">En esta sección usará migraciones de Entity Framework Code First para migrar algunos cambios a las clases del modelo, por lo que el cambio se aplica a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-108">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="2eaa3-109">De forma predeterminada, al usar Entity Framework Code First para crear automáticamente una base de datos, como hizo anteriormente en este tutorial, Code First agrega una tabla para ayudar a saber si el esquema de la base de datos está sincronizado con las clases del modelo se generó a partir de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-109">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="2eaa3-110">Si no están sincronizados, Entity Framework produce un error.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-110">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="2eaa3-111">Esto facilita realizar un seguimiento de los problemas en tiempo de desarrollo en caso contrario, podría encontrar solamente (por errores poco conocidos) en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-111">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="2eaa3-112">Configuración de migraciones de Code First para cambios en el modelo</span><span class="sxs-lookup"><span data-stu-id="2eaa3-112">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="2eaa3-113">Si utiliza Visual Studio 2012, haga doble clic en el *Movies.mdf* archivo desde el Explorador de soluciones para abrir la herramienta de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-113">If you are using Visual Studio 2012, double click the *Movies.mdf* file from Solution Explorer to open the database tool.</span></span> <span data-ttu-id="2eaa3-114">Visual Studio Express para Web mostrará el Explorador de base de datos, que Visual Studio 2012 se mostrará el Explorador de servidores.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-114">Visual Studio Express for Web will show Database Explorer, Visual Studio 2012 will show Server Explorer.</span></span> <span data-ttu-id="2eaa3-115">Si utiliza Visual Studio 2010, use el Explorador de objetos de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-115">If you are using Visual Studio 2010, use SQL Server Object Explorer.</span></span>

<span data-ttu-id="2eaa3-116">En la herramienta de la base de datos (Explorador de base de datos, Explorador de servidores o explorador de objetos de SQL Server), haga clic en `MovieDBContext` y seleccione **eliminar** va a quitar de la base de datos de películas.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-116">In the database tool (Database Explorer, Server Explorer or SQL Server Object Explorer), right click on `MovieDBContext` and select **Delete** to drop the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

<span data-ttu-id="2eaa3-117">Navegue hasta el Explorador de soluciones.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-117">Navigate back to Solution Explorer.</span></span> <span data-ttu-id="2eaa3-118">Haga clic con el botón derecho en el *Movies.mdf* de archivo y seleccione **eliminar** para quitar la base de datos de películas.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-118">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

<span data-ttu-id="2eaa3-119">Compile la aplicación para asegurarse de que no hay ningún error.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-119">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="2eaa3-120">Desde el **herramientas** menú, haga clic en **Administrador de paquetes de biblioteca** y, a continuación, **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-120">From the **Tools** menu, click **Library Package Manager** and then **Package Manager Console**.</span></span>

![Agregar módulo Man](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

<span data-ttu-id="2eaa3-122">En el **Package Manager Console** ventana en la `PM>` símbolo del sistema escriba "Enable-Migrations - ContextTypeName MvcMovie.Models.MovieDBContext".</span><span class="sxs-lookup"><span data-stu-id="2eaa3-122">In the **Package Manager Console** window at the `PM>` prompt enter "Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext".</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

<span data-ttu-id="2eaa3-123">El **Enable-Migrations** comando (mostrado arriba) crea un *Configuration.cs* archivo en una nueva *migraciones* carpeta.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-123">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

<span data-ttu-id="2eaa3-124">Visual Studio abre el *Configuration.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-124">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="2eaa3-125">Reemplace el `Seed` método en el *Configuration.cs* archivo con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="2eaa3-125">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

<span data-ttu-id="2eaa3-126">Haga clic con el botón derecho en la línea ondulada roja bajo `Movie` y seleccione **resolver** , a continuación, **mediante** **MvcMovie.Models;**</span><span class="sxs-lookup"><span data-stu-id="2eaa3-126">Right click on the red squiggly line under `Movie` and select **Resolve** then **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

<span data-ttu-id="2eaa3-127">Si lo hace, agrega la siguiente instrucción using:</span><span class="sxs-lookup"><span data-stu-id="2eaa3-127">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="2eaa3-128">El código llama a las migraciones de primera la `Seed` método después de cada migración (es decir, una llamada a **Actualizar base de datos** en la consola de administrador de paquetes), y este método actualiza las filas que ya se han insertado o insertan si se aún no existen.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-128">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>


<span data-ttu-id="2eaa3-129">**Presione CTRL-MAYÚS-B para compilar el proyecto.** (Se producirá un error en los pasos siguientes si su no se compilan en este momento.)</span><span class="sxs-lookup"><span data-stu-id="2eaa3-129">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if your don't build at this point.)</span></span>

<span data-ttu-id="2eaa3-130">El siguiente paso es crear un `DbMigration` clase para la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-130">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="2eaa3-131">Esta migración crea una nueva base de datos, por eso se eliminó el *movie.mdf* archivo en el paso anterior.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-131">This migration to creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="2eaa3-132">En el **Package Manager Console** ventana, escriba el comando "add-migration Initial" para crear la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-132">In the **Package Manager Console** window, enter the command "add-migration Initial" to create the initial migration.</span></span> <span data-ttu-id="2eaa3-133">El nombre "Inicial" es arbitrario y se usa un nombre al archivo de migración creado.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-133">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

<span data-ttu-id="2eaa3-134">Migraciones de Code First crea otro archivo de clase en el *migraciones* carpeta (con el nombre *{fecha}\_Initial.cs* ), y esta clase contiene código que crea el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-134">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="2eaa3-135">Previamente se ha corregido el nombre de archivo de migración con una marca de tiempo para ayudar a con la ordenación.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-135">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="2eaa3-136">Examine el *{fecha}\_Initial.cs* archivo, contiene las instrucciones para crear la tabla de películas para la base de datos de la película.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-136">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the Movies table for the Movie DB.</span></span> <span data-ttu-id="2eaa3-137">Al actualizar la base de datos en las instrucciones que aparecen a continuación, esto *{fecha}\_Initial.cs* archivo ejecutará y crear el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-137">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="2eaa3-138">El **inicialización** método se ejecutará para rellenar la base de datos con datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-138">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="2eaa3-139">En el **Package Manager Console**, escriba el comando "update-database" para crear la base de datos y ejecutar el **inicialización** método.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-139">In the **Package Manager Console**, enter the command "update-database" to create the database and run the **Seed** method.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

<span data-ttu-id="2eaa3-140">Si se produce un error que indica una tabla ya existe y no se puede crear, probablemente es porque se ejecutó la aplicación después de eliminar la base de datos y antes de ejecutar `update-database`.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-140">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="2eaa3-141">En ese caso, elimine la *Movies.mdf* archivo nuevo y vuelva a intentar la `update-database` comando.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-141">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="2eaa3-142">Si sigue apareciendo un error, elimine la carpeta de migraciones y contenido, comience con las instrucciones que aparecen en la parte superior de esta página (que es eliminar el *Movies.mdf* de archivos, a continuación, continúe con Enable-Migrations).</span><span class="sxs-lookup"><span data-stu-id="2eaa3-142">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span>

<span data-ttu-id="2eaa3-143">Ejecute la aplicación y vaya a la */Movies* dirección URL.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-143">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="2eaa3-144">Se muestran los datos de inicialización.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-144">The seed data is displayed.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="2eaa3-145">Adición de una propiedad de clasificación al modelo Movie</span><span class="sxs-lookup"><span data-stu-id="2eaa3-145">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="2eaa3-146">Empiece agregando un nuevo `Rating` propiedad existente `Movie` clase.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-146">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="2eaa3-147">Abra el *Models\Movie.cs* y agréguele el `Rating` propiedad como esta:</span><span class="sxs-lookup"><span data-stu-id="2eaa3-147">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

<span data-ttu-id="2eaa3-148">La completa `Movie` clase ahora parece similar al código siguiente:</span><span class="sxs-lookup"><span data-stu-id="2eaa3-148">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

<span data-ttu-id="2eaa3-149">Compilar la aplicación mediante el **compilar** &gt; **crear películas** menú comando o presionando CTRL-MAYÚS-B.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-149">Build the application using the **Build** &gt;**Build Movie** menu command or by pressing CTRL-SHIFT-B.</span></span>

<span data-ttu-id="2eaa3-150">Ahora que ha actualizado el `Model` (clase), también deberá actualizar el *\Views\Movies\Index.cshtml* y *\Views\Movies\Create.cshtml* ver las plantillas con el fin de mostrar el nuevo `Rating`propiedad en la vista de explorador.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-150">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.cshtml* and *\Views\Movies\Create.cshtml* view templates in order to display the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="2eaa3-151">Abra el<em>\Views\Movies\Index.cshtml</em> archivo y agregue un `<th>Rating</th>` justo después de encabezado de columna la <strong>precio</strong> columna.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-151">Open the<em>\Views\Movies\Index.cshtml</em> file and add a `<th>Rating</th>` column heading just after the <strong>Price</strong> column.</span></span> <span data-ttu-id="2eaa3-152">A continuación, agregue un `<td>` columna cerca del final de la plantilla para representar el `@item.Rating` valor.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-152">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="2eaa3-153">A continuación es lo que la actualización <em>Index.cshtml</em> plantilla de vista es similar:</span><span class="sxs-lookup"><span data-stu-id="2eaa3-153">Below is what the updated <em>Index.cshtml</em> view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

<span data-ttu-id="2eaa3-154">A continuación, abra el *\Views\Movies\Create.cshtml* archivo y agregue el siguiente marcado cerca del final del formulario.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-154">Next, open the *\Views\Movies\Create.cshtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="2eaa3-155">Esto representa un cuadro de texto para que pueda especificar una clasificación cuando se crea una nueva película.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-155">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

<span data-ttu-id="2eaa3-156">Se ha actualizado el código de aplicación para admitir los nuevos `Rating` propiedad.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-156">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="2eaa3-157">Ahora ejecute la aplicación y vaya a la */Movies* dirección URL.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-157">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="2eaa3-158">Al hacerlo, sin embargo, verá uno de los errores siguientes:</span><span class="sxs-lookup"><span data-stu-id="2eaa3-158">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

<span data-ttu-id="2eaa3-159">Si ve este error porque la actualización `Movie` ahora es diferente del esquema de clase de modelo en la aplicación la `Movie` tabla de la base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-159">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="2eaa3-160">(No hay ninguna columna `Rating` en la tabla de la base de datos).</span><span class="sxs-lookup"><span data-stu-id="2eaa3-160">(There's no `Rating` column in the database table.)</span></span>


<span data-ttu-id="2eaa3-161">Este error se puede resolver de varias maneras:</span><span class="sxs-lookup"><span data-stu-id="2eaa3-161">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="2eaa3-162">Haga que Entity Framework quite de forma automática la base de datos y la vuelva a crear basándose en el nuevo esquema de la clase del modelo.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-162">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="2eaa3-163">Este enfoque resulta muy conveniente al realizar el desarrollo activo en una base de datos de prueba; permite desarrollar rápidamente el esquema de modelo y la base de datos entre sí.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-163">This approach is very convenient when doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="2eaa3-164">La desventaja, sin embargo, es la que se pierden los datos existentes en la base de datos, por lo que se *no* va a utilizar este enfoque en una base de datos de producción.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-164">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="2eaa3-165">Usar a un inicializador para inicializar automáticamente una base de datos con datos de prueba a menudo es una manera productiva de desarrollar una aplicación.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-165">Using an initializer to automatically seed a database with test data is often a productive way to develope an application.</span></span> <span data-ttu-id="2eaa3-166">Para obtener más información sobre los inicializadores de base de datos de Entity Framework, vea de Tom Dykstra [tutorial de ASP.NET MVC/Entity Framework](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="2eaa3-166">For more information on Entity Framework database initializers, see Tom Dykstra's [ASP.NET MVC/Entity Framework tutorial](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="2eaa3-167">Modifique explícitamente el esquema de la base de datos existente para que coincida con las clases del modelo.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-167">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="2eaa3-168">La ventaja de este enfoque es que se conservan los datos.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-168">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="2eaa3-169">Puede realizar este cambio de forma manual o mediante la creación de un script de cambio de base de datos.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-169">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="2eaa3-170">Use Migraciones de Code First para actualizar el esquema de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-170">Use Code First Migrations to update the database schema.</span></span>


<span data-ttu-id="2eaa3-171">Para este tutorial se usa Migraciones de Code First.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-171">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="2eaa3-172">Actualice el método de inicialización para que proporcione un valor para la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-172">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="2eaa3-173">Abrir archivo migrations\configuration. cs y agregue un campo de clasificación para cada objeto de película.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-173">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

<span data-ttu-id="2eaa3-174">Compile la solución y, a continuación, abra el **Package Manager Console** ventana y escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="2eaa3-174">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration AddRatingMig`

<span data-ttu-id="2eaa3-175">El `add-migration` comando indica el marco de trabajo de migración para examinar el modelo de película actual con el esquema actual de la base de datos de la película y crear el código necesario para migrar la base de datos al nuevo modelo.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-175">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="2eaa3-176">El AddRatingMig es arbitrario y se usa un nombre al archivo de migración.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-176">The AddRatingMig is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="2eaa3-177">Resulta útil usar un nombre descriptivo para el paso de migración.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-177">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="2eaa3-178">Cuando finalice este comando, Visual Studio abre el archivo de clase que define la nueva `DbMIgration` clase derivada y en el `Up` método puede ver el código que crea la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-178">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

<span data-ttu-id="2eaa3-179">Compile la solución y, a continuación, escriba el comando "update-database" en el **Package Manager Console** ventana.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-179">Build the solution, and then enter the "update-database" command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="2eaa3-180">La siguiente imagen muestra la salida en el **Package Manager Console** ventana (será diferente la marca de fecha anteposición AddRatingMig.)</span><span class="sxs-lookup"><span data-stu-id="2eaa3-180">The following image shows the output in the **Package Manager Console** window (The date stamp prepending AddRatingMig will be different.)</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

<span data-ttu-id="2eaa3-181">Vuelva a ejecutar la aplicación y vaya a la URL /Movies.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-181">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="2eaa3-182">Puede ver el nuevo campo de clasificación.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-182">You can see the new Rating field.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

<span data-ttu-id="2eaa3-183">Haga clic en el **crear nuevo** el vínculo para agregar una nueva película.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-183">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="2eaa3-184">Tenga en cuenta que puede agregar una clasificación.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-184">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

<span data-ttu-id="2eaa3-186">Haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-186">Click **Create**.</span></span> <span data-ttu-id="2eaa3-187">La nueva película, incluida la clasificación, se muestra ahora en la lista de películas:</span><span class="sxs-lookup"><span data-stu-id="2eaa3-187">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

<span data-ttu-id="2eaa3-189">También debe agregar el `Rating` campo para la edición, detalles e SearchIndex plantillas de vista.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-189">You should also add the `Rating` field to the Edit, Details and SearchIndex view templates.</span></span>

<span data-ttu-id="2eaa3-190">Puede escribir el comando "update-database" en el **Package Manager Console** ventana nuevo y no hay cambios que se aplicarían, porque el esquema coincide con el modelo.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-190">You could enter the "update-database" command in the **Package Manager Console** window again and no changes would be made, because the schema matches the model.</span></span>

<span data-ttu-id="2eaa3-191">En esta sección, ha visto cómo puede modificar los objetos de modelo y mantener sincronizados con los cambios de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-191">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="2eaa3-192">También ha aprendido cómo rellenar una base de datos recién creada con datos de ejemplo para que pueda probar escenarios.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-192">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="2eaa3-193">A continuación, echemos un vistazo a cómo puede agregar lógica de validación más completa para las clases del modelo y habilitar algunas reglas de negocios que se aplicará.</span><span class="sxs-lookup"><span data-stu-id="2eaa3-193">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2eaa3-194">[Anterior](examining-the-edit-methods-and-edit-view.md)
> [Siguiente](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="2eaa3-194">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
