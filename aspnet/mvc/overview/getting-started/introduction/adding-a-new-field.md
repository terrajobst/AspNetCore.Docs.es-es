---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: Agregar un nuevo campo | Documentos de Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 0dac798eba586cdcc232cedd262e610b954004df
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874189"
---
<a name="adding-a-new-field"></a><span data-ttu-id="7d49b-102">Adición de un nuevo campo</span><span class="sxs-lookup"><span data-stu-id="7d49b-102">Adding a New Field</span></span>
====================
<span data-ttu-id="7d49b-103">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="7d49b-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="7d49b-104">En esta sección usará Entity Framework Code First Migrations migrar algunos cambios a las clases del modelo, por lo que el cambio se aplica a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7d49b-104">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="7d49b-105">De forma predeterminada, cuando se usa Entity Framework Code First para crear automáticamente una base de datos, como lo hizo anteriormente en este tutorial, Code First agrega una tabla para ayudar a realizar el seguimiento de si el esquema de la base de datos está sincronizado con las clases del modelo se generó a partir de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7d49b-105">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="7d49b-106">Si no están sincronizados, Entity Framework produce un error.</span><span class="sxs-lookup"><span data-stu-id="7d49b-106">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="7d49b-107">Esto hace más fácil realizar un seguimiento de los problemas en tiempo de desarrollo que en caso contrario, podría encontrar solamente (por errores poco claros) en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="7d49b-107">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="7d49b-108">Configuración de migraciones de Code First para cambios de modelo</span><span class="sxs-lookup"><span data-stu-id="7d49b-108">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="7d49b-109">Vaya al explorador de soluciones.</span><span class="sxs-lookup"><span data-stu-id="7d49b-109">Navigate to Solution Explorer.</span></span> <span data-ttu-id="7d49b-110">Haga clic con el botón secundario en el *Movies.mdf* de archivos y seleccione **eliminar** para quitar la base de datos de películas.</span><span class="sxs-lookup"><span data-stu-id="7d49b-110">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span> <span data-ttu-id="7d49b-111">Si no ve el *Movies.mdf* de archivos, haga clic en el **mostrar todos los archivos** icono se muestra a continuación en el contorno de color rojo.</span><span class="sxs-lookup"><span data-stu-id="7d49b-111">If you don't see the *Movies.mdf* file, click on the **Show All Files** icon shown below in the red outline.</span></span>

![](adding-a-new-field/_static/image1.png)

<span data-ttu-id="7d49b-112">Compile la aplicación para asegurarse de que no hay ningún error.</span><span class="sxs-lookup"><span data-stu-id="7d49b-112">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="7d49b-113">Desde el **herramientas** menú, haga clic en **Administrador de paquetes de NuGet** y, a continuación, **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="7d49b-113">From the **Tools** menu, click **NuGet Package Manager** and then **Package Manager Console**.</span></span>

![Agregar módulo Man](adding-a-new-field/_static/image2.png)

<span data-ttu-id="7d49b-115">En el **Package Manager Console** ventana en el `PM>` símbolo del sistema escriba</span><span class="sxs-lookup"><span data-stu-id="7d49b-115">In the **Package Manager Console** window at the `PM>` prompt enter</span></span>

<span data-ttu-id="7d49b-116">Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext</span><span class="sxs-lookup"><span data-stu-id="7d49b-116">Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext</span></span>

![](adding-a-new-field/_static/image3.png)

<span data-ttu-id="7d49b-117">El **Enable-Migrations** comando (se muestra arriba) crea un *archivo Configuration.cs que* archivo en una nueva *migraciones* carpeta.</span><span class="sxs-lookup"><span data-stu-id="7d49b-117">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field/_static/image4.png)

<span data-ttu-id="7d49b-118">Visual Studio abre el *archivo Configuration.cs que* archivos.</span><span class="sxs-lookup"><span data-stu-id="7d49b-118">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="7d49b-119">Reemplace el `Seed` método en el *archivo Configuration.cs que* archivos con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="7d49b-119">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

<span data-ttu-id="7d49b-120">Mantenga el mouse sobre la línea ondulada roja bajo `Movie` y haga clic en `Show Potential Fixes` y, a continuación, haga clic en **con** **MvcMovie.Models;**</span><span class="sxs-lookup"><span data-stu-id="7d49b-120">Hover over the red squiggly line under `Movie` and click `Show Potential Fixes` and then click **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field/_static/image5.png)

<span data-ttu-id="7d49b-121">Si lo hace, agrega la siguiente instrucción using:</span><span class="sxs-lookup"><span data-stu-id="7d49b-121">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE]
> 
> <span data-ttu-id="7d49b-122">Code First Migrations llamadas el `Seed` método después de cada migración (es decir, una llamada a **Actualizar base de datos** en la consola de administrador de paquetes), y este método actualiza las filas que ya se han insertado o insertan si únicamente aún no existen.</span><span class="sxs-lookup"><span data-stu-id="7d49b-122">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>
> 
> <span data-ttu-id="7d49b-123">El [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) método en el código siguiente realiza una operación "upsert":</span><span class="sxs-lookup"><span data-stu-id="7d49b-123">The [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method in the following code performs an "upsert" operation:</span></span>
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> <span data-ttu-id="7d49b-124">Dado que la [inicialización](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) método se ejecuta con cada migración, simplemente no se puede insertar datos, porque las filas que se está intentando agregar ya estarán ahí después de la primera migración que crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7d49b-124">Because the [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) method runs with every migration, you can't just insert data, because the rows you are trying to add will already be there after the first migration that creates the database.</span></span> <span data-ttu-id="7d49b-125">El "[upsert](http://en.wikipedia.org/wiki/Upsert)" operación evita los errores que sucedería si se intenta insertar una fila que ya existe, pero reemplaza los cambios a los datos que haya podido realizar durante la comprobación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7d49b-125">The "[upsert](http://en.wikipedia.org/wiki/Upsert)" operation prevents errors that would happen if you try to insert a row that already exists, but it overrides any changes to data that you may have made while testing the application.</span></span> <span data-ttu-id="7d49b-126">Con datos de prueba en algunas tablas puede no ser conveniente que ocurra esto: en algunos casos si cambia los datos durante las pruebas se desea que permanecen después de las actualizaciones de base de datos los cambios.</span><span class="sxs-lookup"><span data-stu-id="7d49b-126">With test data in some tables you might not want that to happen: in some cases when you change data while testing you want your changes to remain after database updates.</span></span> <span data-ttu-id="7d49b-127">En ese caso en el que desea realizar una operación de inserción condicional: insertar una fila sólo si aún no existe.</span><span class="sxs-lookup"><span data-stu-id="7d49b-127">In that case you want to do a conditional insert operation: insert a row only if it doesn't already exist.</span></span>   
> 
> <span data-ttu-id="7d49b-128">El primer parámetro pasado a la [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) método especifica la propiedad que se va a usar para comprobar si ya existe una fila.</span><span class="sxs-lookup"><span data-stu-id="7d49b-128">The first parameter passed to the [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method specifies the property to use to check if a row already exists.</span></span> <span data-ttu-id="7d49b-129">Para los datos de la película de prueba que se va a proporcionar, el `Title` propiedad puede utilizarse para este propósito, puesto que cada título de la lista es único:</span><span class="sxs-lookup"><span data-stu-id="7d49b-129">For the test movie data that you are providing, the `Title` property can be used for this purpose since each title in the list is unique:</span></span>
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> <span data-ttu-id="7d49b-130">Este código supone que los títulos sean únicos.</span><span class="sxs-lookup"><span data-stu-id="7d49b-130">This code assumes that titles are unique.</span></span> <span data-ttu-id="7d49b-131">Si agrega manualmente un título duplicado, obtendrá la excepción siguiente la próxima vez que se realiza una migración.</span><span class="sxs-lookup"><span data-stu-id="7d49b-131">If you manually add a duplicate title, you'll get the following exception the next time you perform a migration.</span></span>   
> 
>  <span data-ttu-id="7d49b-132">*La secuencia contiene más de un elemento*</span><span class="sxs-lookup"><span data-stu-id="7d49b-132">*Sequence contains more than one element*</span></span>  
> 
> <span data-ttu-id="7d49b-133">Para obtener más información sobre la [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) método, consulte [tener cuidado con el método de EF 4.3 AddOrUpdate](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)...</span><span class="sxs-lookup"><span data-stu-id="7d49b-133">For more information about the [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method, see [Take care with EF 4.3 AddOrUpdate Method](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)..</span></span>


<span data-ttu-id="7d49b-134">**Presione CTRL-MAYÚS + B para compilar el proyecto.** (Los pasos siguientes se producirá un error si no se compilan en este momento.)</span><span class="sxs-lookup"><span data-stu-id="7d49b-134">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if you don't build at this point.)</span></span>

<span data-ttu-id="7d49b-135">El paso siguiente consiste en crear un `DbMigration` clase para la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="7d49b-135">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="7d49b-136">Esta migración crea una nueva base de datos, que es por lo que se elimina la *movie.mdf* archivo en un paso anterior.</span><span class="sxs-lookup"><span data-stu-id="7d49b-136">This migration creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="7d49b-137">En el **Package Manager Console** ventana, escriba el comando `add-migration Initial` para crear la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="7d49b-137">In the **Package Manager Console** window, enter the command `add-migration Initial` to create the initial migration.</span></span> <span data-ttu-id="7d49b-138">El nombre "Inicial" es arbitrario y se utiliza para asignar nombre al archivo de migración que creó.</span><span class="sxs-lookup"><span data-stu-id="7d49b-138">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field/_static/image6.png)

<span data-ttu-id="7d49b-139">Migraciones de Code First crea otro archivo de clase en el *migraciones* carpeta (con el nombre *{DateStamp}\_Initial.cs* ), y esta clase contiene código que crea el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="7d49b-139">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="7d49b-140">El nombre de archivo de migración previamente se fija con una marca de tiempo para ayudar a con la ordenación.</span><span class="sxs-lookup"><span data-stu-id="7d49b-140">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="7d49b-141">Examine la *{DateStamp}\_Initial.cs* archivo, contiene las instrucciones para crear el `Movies` tabla para la base de datos de la película.</span><span class="sxs-lookup"><span data-stu-id="7d49b-141">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the `Movies` table for the Movie DB.</span></span> <span data-ttu-id="7d49b-142">Al actualizar la base de datos en las instrucciones a continuación, esto *{DateStamp}\_Initial.cs* archivo ejecutará y crear el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="7d49b-142">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="7d49b-143">La **inicialización** método se ejecutará para rellenar la base de datos con datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="7d49b-143">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="7d49b-144">En el **Package Manager Console**, escriba el comando `update-database` para crear la base de datos y ejecutar el `Seed` método.</span><span class="sxs-lookup"><span data-stu-id="7d49b-144">In the **Package Manager Console**, enter the command `update-database` to create the database and run the `Seed` method.</span></span>

![](adding-a-new-field/_static/image7.png)

<span data-ttu-id="7d49b-145">Si se produce un error que indica una tabla ya existe y no se puede crear, probablemente es porque ejecutó la aplicación después de eliminar la base de datos y antes de ejecutar `update-database`.</span><span class="sxs-lookup"><span data-stu-id="7d49b-145">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="7d49b-146">En ese caso, elimine la *Movies.mdf* archivo nuevo y vuelva a intentar la `update-database` comando.</span><span class="sxs-lookup"><span data-stu-id="7d49b-146">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="7d49b-147">Si sigue recibiendo un error, elimine la carpeta migrations y contenido e inicie con las instrucciones que aparecen en la parte superior de esta página (que es eliminar la *Movies.mdf* de archivos, a continuación, continúe con Enable-Migrations).</span><span class="sxs-lookup"><span data-stu-id="7d49b-147">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span> <span data-ttu-id="7d49b-148">Si sigue recibiendo un encontraron, abra el Explorador de objetos de SQL Server y quitar la base de datos de la lista.</span><span class="sxs-lookup"><span data-stu-id="7d49b-148">If you still get an eror, open SQL Server Object Explorer and remove the database from the list.</span></span>

<span data-ttu-id="7d49b-149">Ejecute la aplicación y navegue hasta la */Movies* dirección URL.</span><span class="sxs-lookup"><span data-stu-id="7d49b-149">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="7d49b-150">Se muestran los datos de inicialización.</span><span class="sxs-lookup"><span data-stu-id="7d49b-150">The seed data is displayed.</span></span>

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="7d49b-151">Adición de una propiedad de clasificación al modelo Movie</span><span class="sxs-lookup"><span data-stu-id="7d49b-151">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="7d49b-152">Empiece agregando un nuevo `Rating` propiedad existente `Movie` clase.</span><span class="sxs-lookup"><span data-stu-id="7d49b-152">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="7d49b-153">Abra la *Models\Movie.cs* de archivos y agregar el `Rating` propiedad como esta:</span><span class="sxs-lookup"><span data-stu-id="7d49b-153">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

<span data-ttu-id="7d49b-154">La sección completa `Movie` clase ahora parece similar al código siguiente:</span><span class="sxs-lookup"><span data-stu-id="7d49b-154">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

<span data-ttu-id="7d49b-155">Compilar la aplicación (Ctrl + Mayús + B).</span><span class="sxs-lookup"><span data-stu-id="7d49b-155">Build the application (Ctrl+Shift+B).</span></span>

<span data-ttu-id="7d49b-156">Dado que ha agregado un nuevo campo a la `Movie` (clase), también debe actualizar el enlace *lista de permitidos* por lo que se incluirá esta nueva propiedad.</span><span class="sxs-lookup"><span data-stu-id="7d49b-156">Because you've added a new field to the `Movie` class, you also need to update the binding *white list* so this new property will be included.</span></span> <span data-ttu-id="7d49b-157">Actualización de la `bind` atributo `Create` y `Edit` métodos de acción para incluir la `Rating` propiedad:</span><span class="sxs-lookup"><span data-stu-id="7d49b-157">Update the `bind` attribute for `Create` and `Edit` action methods to include the `Rating` property:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

<span data-ttu-id="7d49b-158">También debe actualizar las plantillas de vista para mostrar, crear y editar la nueva propiedad `Rating` en la vista del explorador.</span><span class="sxs-lookup"><span data-stu-id="7d49b-158">You also need to update the view templates in order to display, create and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="7d49b-159">Abra la *\Views\Movies\Index.cshtml* de archivos y agregar un `<th>Rating</th>` justo después de encabezado de columna la **precio** columna.</span><span class="sxs-lookup"><span data-stu-id="7d49b-159">Open the *\Views\Movies\Index.cshtml* file and add a `<th>Rating</th>` column heading just after the **Price** column.</span></span> <span data-ttu-id="7d49b-160">A continuación, agregue un `<td>` columna cerca del final de la plantilla para representar la `@item.Rating` valor.</span><span class="sxs-lookup"><span data-stu-id="7d49b-160">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="7d49b-161">A continuación se muestra qué actualizado *Index.cshtml* plantilla de vista es similar:</span><span class="sxs-lookup"><span data-stu-id="7d49b-161">Below is what the updated *Index.cshtml* view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

<span data-ttu-id="7d49b-162">A continuación, abra el *\Views\Movies\Create.cshtml* de archivos y agregar el `Rating` campo con el siguiente marcado marcadas.</span><span class="sxs-lookup"><span data-stu-id="7d49b-162">Next, open the *\Views\Movies\Create.cshtml* file and add the `Rating` field with the following highlighed markup.</span></span> <span data-ttu-id="7d49b-163">Esto representa un cuadro de texto para que pueda especificar una clasificación cuando se crea una película nuevo.</span><span class="sxs-lookup"><span data-stu-id="7d49b-163">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

<span data-ttu-id="7d49b-164">Ahora ha actualizado el código de aplicación para admitir los nuevos `Rating` propiedad.</span><span class="sxs-lookup"><span data-stu-id="7d49b-164">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="7d49b-165">Ejecute la aplicación y navegue hasta la */Movies* dirección URL.</span><span class="sxs-lookup"><span data-stu-id="7d49b-165">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="7d49b-166">Al hacer esto, sin embargo, verá uno de los siguientes errores:</span><span class="sxs-lookup"><span data-stu-id="7d49b-166">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field/_static/image9.png)  
  
<span data-ttu-id="7d49b-167">El modelo de realizar una copia del contexto de 'MovieDBContext' ha cambiado desde que se creó la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7d49b-167">The model backing the 'MovieDBContext' context has changed since the database was created.</span></span> <span data-ttu-id="7d49b-168">Considere la posibilidad de usar migraciones de Code First para actualizar la base de datos (https://go.microsoft.com/fwlink/?LinkId=238269).</span><span class="sxs-lookup"><span data-stu-id="7d49b-168">Consider using Code First Migrations to update the database (https://go.microsoft.com/fwlink/?LinkId=238269).</span></span>

![](adding-a-new-field/_static/image10.png)

<span data-ttu-id="7d49b-169">Está viendo este error porque la actualización `Movie` clase de modelo en la aplicación ahora es diferente que el esquema de la `Movie` tabla de la base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="7d49b-169">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="7d49b-170">(No hay ninguna columna `Rating` en la tabla de la base de datos).</span><span class="sxs-lookup"><span data-stu-id="7d49b-170">(There's no `Rating` column in the database table.)</span></span>


<span data-ttu-id="7d49b-171">Este error se puede resolver de varias maneras:</span><span class="sxs-lookup"><span data-stu-id="7d49b-171">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="7d49b-172">Haga que Entity Framework quite de forma automática la base de datos y la vuelva a crear basándose en el nuevo esquema de la clase del modelo.</span><span class="sxs-lookup"><span data-stu-id="7d49b-172">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="7d49b-173">Este enfoque resulta muy conveniente al principio del ciclo de desarrollo cuando se está realizando el desarrollo activo en una base de datos de prueba; permite desarrollar rápidamente el esquema del modelo y la base de datos juntos.</span><span class="sxs-lookup"><span data-stu-id="7d49b-173">This approach is very convenient early in the development cycle when you are doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="7d49b-174">La desventaja, sin embargo, es que se pierden los datos existentes en la base de datos, por lo que *no* va a utilizar este enfoque en una base de datos de producción.</span><span class="sxs-lookup"><span data-stu-id="7d49b-174">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="7d49b-175">Usar un inicializador para inicializar automáticamente una base de datos con datos de prueba suele ser una manera productiva de desarrollar una aplicación.</span><span class="sxs-lookup"><span data-stu-id="7d49b-175">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span> <span data-ttu-id="7d49b-176">Para obtener más información sobre los inicializadores de base de datos de Entity Framework, vea [tutorial de ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="7d49b-176">For more information on Entity Framework database initializers, see [ASP.NET MVC/Entity Framework tutorial](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="7d49b-177">Modifique explícitamente el esquema de la base de datos existente para que coincida con las clases del modelo.</span><span class="sxs-lookup"><span data-stu-id="7d49b-177">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="7d49b-178">La ventaja de este enfoque es que se conservan los datos.</span><span class="sxs-lookup"><span data-stu-id="7d49b-178">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="7d49b-179">Puede realizar este cambio de forma manual o mediante la creación de un script de cambio de base de datos.</span><span class="sxs-lookup"><span data-stu-id="7d49b-179">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="7d49b-180">Use Migraciones de Code First para actualizar el esquema de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7d49b-180">Use Code First Migrations to update the database schema.</span></span>


<span data-ttu-id="7d49b-181">Para este tutorial se usa Migraciones de Code First.</span><span class="sxs-lookup"><span data-stu-id="7d49b-181">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="7d49b-182">Actualizar el método de inicialización para que proporcione un valor para la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="7d49b-182">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="7d49b-183">Abra el archivo Migrations\Configuration.cs y agregar un campo de clasificación para cada objeto de la película.</span><span class="sxs-lookup"><span data-stu-id="7d49b-183">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

<span data-ttu-id="7d49b-184">Compile la solución y, a continuación, abra el **Package Manager Console** ventana y escriba el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="7d49b-184">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration Rating`

<span data-ttu-id="7d49b-185">El `add-migration` comando indica el marco de trabajo de migración para examinar el modelo de película actual con el esquema actual de la base de datos de la película y crear el código necesario para migrar la base de datos al nuevo modelo.</span><span class="sxs-lookup"><span data-stu-id="7d49b-185">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="7d49b-186">El nombre *clasificación* es arbitrario y se utiliza para asignar nombre al archivo de migración.</span><span class="sxs-lookup"><span data-stu-id="7d49b-186">The name *Rating* is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="7d49b-187">Es útil emplear un nombre descriptivo para el paso de migración.</span><span class="sxs-lookup"><span data-stu-id="7d49b-187">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="7d49b-188">Cuando finaliza este comando, Visual Studio abre el archivo de clase que define la nueva `DbMigration` clase derivada y en el `Up` método puede ver el código que crea la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="7d49b-188">When this command finishes, Visual Studio opens the class file that defines the new `DbMigration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

<span data-ttu-id="7d49b-189">Compile la solución y, a continuación, escriba la `update-database` comando en el **Package Manager Console** ventana.</span><span class="sxs-lookup"><span data-stu-id="7d49b-189">Build the solution, and then enter the `update-database` command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="7d49b-190">La siguiente imagen muestra la salida en el **Package Manager Console** ventana (la marca de fecha anteponiendo *clasificación* será diferente.)</span><span class="sxs-lookup"><span data-stu-id="7d49b-190">The following image shows the output in the **Package Manager Console** window (The date stamp prepending *Rating* will be different.)</span></span>

![](adding-a-new-field/_static/image11.png)

<span data-ttu-id="7d49b-191">Vuelva a ejecutar la aplicación y navegue a la dirección URL /Movies.</span><span class="sxs-lookup"><span data-stu-id="7d49b-191">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="7d49b-192">Puede ver el nuevo campo de clasificación.</span><span class="sxs-lookup"><span data-stu-id="7d49b-192">You can see the new Rating field.</span></span>

![](adding-a-new-field/_static/image12.png)

<span data-ttu-id="7d49b-193">Haga clic en el **crear nuevo** el vínculo para agregar una película nuevo.</span><span class="sxs-lookup"><span data-stu-id="7d49b-193">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="7d49b-194">Tenga en cuenta que puede agregar una clasificación.</span><span class="sxs-lookup"><span data-stu-id="7d49b-194">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field/_static/image13.png)

<span data-ttu-id="7d49b-196">Haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="7d49b-196">Click **Create**.</span></span> <span data-ttu-id="7d49b-197">La película nuevo, incluida la clasificación, se muestra ahora en la lista de películas:</span><span class="sxs-lookup"><span data-stu-id="7d49b-197">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

<span data-ttu-id="7d49b-199">Ahora que el proyecto está utilizando las migraciones, no debe quitar la base de datos al agregar un nuevo campo o en caso contrario, actualizar el esquema.</span><span class="sxs-lookup"><span data-stu-id="7d49b-199">Now that the project is using migrations, you won't need to drop the database when you add a new field or otherwise update the schema.</span></span> <span data-ttu-id="7d49b-200">En la siguiente sección, se podrá realizar más cambios de esquema y use migraciones para actualizar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7d49b-200">In the next section, we'll make more schema changes and use migrations to update the database.</span></span>

<span data-ttu-id="7d49b-201">También debe agregar el `Rating` campo a las plantillas de vista de edición, detalles y Delete.</span><span class="sxs-lookup"><span data-stu-id="7d49b-201">You should also add the `Rating` field to the Edit, Details, and Delete view templates.</span></span>

<span data-ttu-id="7d49b-202">Puede escribir el comando "update-database" en la **Package Manager Console** volver a la ventana y ningún código de migración se ejecutaría, porque el esquema coincide con el modelo.</span><span class="sxs-lookup"><span data-stu-id="7d49b-202">You could enter the "update-database" command in the **Package Manager Console** window again and no migration code would run, because the schema matches the model.</span></span> <span data-ttu-id="7d49b-203">Sin embargo, se ejecutará la ejecución "update-database" el `Seed` método nuevo, y si ha cambiado alguno de los datos de inicialización, se perderán los cambios porque el `Seed` datos sobre métodos upserts.</span><span class="sxs-lookup"><span data-stu-id="7d49b-203">However, running "update-database" will run the `Seed` method again, and if you changed any of the Seed data, the changes will be lost because the `Seed` method upserts data.</span></span> <span data-ttu-id="7d49b-204">Más información sobre la `Seed` método en de Tom Dykstra popular [tutorial de ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="7d49b-204">You can read more about the `Seed` method in Tom Dykstra's popular [ASP.NET MVC/Entity Framework tutorial](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="7d49b-205">En esta sección se ha visto cómo puede modificar objetos de modelo y mantenerlos sincronizados con los cambios de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7d49b-205">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="7d49b-206">También aprendió una forma de rellenar una base de datos recién creada con datos de ejemplo para probar escenarios.</span><span class="sxs-lookup"><span data-stu-id="7d49b-206">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="7d49b-207">Esto era simplemente una breve introducción a Code First, vea [crear un modelo de datos de Entity Framework para una aplicación de MVC de ASP.NET](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) para ver un tutorial más completado sobre el tema.</span><span class="sxs-lookup"><span data-stu-id="7d49b-207">This was just a quick introduction to Code First, see [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) for a more complete tutorial on the subject.</span></span> <span data-ttu-id="7d49b-208">A continuación, echemos un vistazo a cómo puede agregar una lógica de validación a las clases del modelo y habilitar algunas reglas de negocios que se aplicará.</span><span class="sxs-lookup"><span data-stu-id="7d49b-208">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7d49b-209">[Anterior](adding-search.md)
> [Siguiente](adding-validation.md)</span><span class="sxs-lookup"><span data-stu-id="7d49b-209">[Previous](adding-search.md)
[Next](adding-validation.md)</span></span>
