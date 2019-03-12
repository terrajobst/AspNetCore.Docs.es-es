---
title: Trabajar con una base de datos y ASP.NET Core
author: rick-anderson
description: En este artículo se explica cómo se trabaja con una base de datos y ASP.NET Core.
ms.author: riande
ms.date: 12/07/2017
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 9f01f5c9fcf9c423ccc2af8ae6ca55c2f427c9c1
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346033"
---
# <a name="work-with-a-database-and-aspnet-core"></a><span data-ttu-id="47ad6-103">Trabajar con una base de datos y ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="47ad6-103">Work with a database and ASP.NET Core</span></span>

<span data-ttu-id="47ad6-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="47ad6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="47ad6-105">El objeto `RazorPagesMovieContext` controla la tarea de conexión a la base de datos y asignación de objetos `Movie` a los registros de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="47ad6-105">The `RazorPagesMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="47ad6-106">El contexto de base de datos se registra con el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) en el método `ConfigureServices` de *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="47ad6-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in *Startup.cs*:</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="47ad6-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="47ad6-107">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="47ad6-108">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="47ad6-108">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="47ad6-109">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="47ad6-109">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

---  
<!-- End of VS tabs -->

<span data-ttu-id="47ad6-110">Para más información sobre los modelos empleados en `ConfigureServices`, vea:</span><span class="sxs-lookup"><span data-stu-id="47ad6-110">For more information on the methods used in `ConfigureServices`, see:</span></span>

* <span data-ttu-id="47ad6-111">[Compatibilidad con el Reglamento general de protección de datos (RGPD) en ASP.NET Core](xref:security/gdpr) para `CookiePolicyOptions`.</span><span class="sxs-lookup"><span data-stu-id="47ad6-111">[EU General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr) for `CookiePolicyOptions`.</span></span>
* [<span data-ttu-id="47ad6-112">SetCompatibilityVersion</span><span class="sxs-lookup"><span data-stu-id="47ad6-112">SetCompatibilityVersion</span></span>](xref:mvc/compatibility-version)

<span data-ttu-id="47ad6-113">El sistema [Configuración](xref:fundamentals/configuration/index) de ASP.NET Core lee el elemento `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="47ad6-113">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="47ad6-114">Para el desarrollo local, obtiene la cadena de conexión del archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="47ad6-114">For local development, it gets the connection string from the *appsettings.json* file.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="47ad6-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="47ad6-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="47ad6-116">El valor de nombre de la base de datos (`Database={Database name}`) será distinto en su código generado.</span><span class="sxs-lookup"><span data-stu-id="47ad6-116">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="47ad6-117">El valor de nombre es arbitrario.</span><span class="sxs-lookup"><span data-stu-id="47ad6-117">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie22/appsettings.json)]

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="47ad6-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="47ad6-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="47ad6-119">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="47ad6-119">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

---  
<!-- End of VS tabs -->

<span data-ttu-id="47ad6-120">Cuando la aplicación se implementa en un servidor de prueba o producción, se puede utilizar una variable de entorno para establecer la cadena de conexión en un servidor de base de datos real.</span><span class="sxs-lookup"><span data-stu-id="47ad6-120">When the app is deployed to a test or production server, an environment variable can be used to set the connection string to a real database server.</span></span> <span data-ttu-id="47ad6-121">Para más información, vea [Configuración](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="47ad6-121">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="47ad6-122">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="47ad6-122">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="47ad6-123">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="47ad6-123">SQL Server Express LocalDB</span></span>

<span data-ttu-id="47ad6-124">LocalDB es una versión ligera del motor de base de datos de SQL Server Express dirigida al desarrollo de programas.</span><span class="sxs-lookup"><span data-stu-id="47ad6-124">LocalDB is a lightweight version of the SQL Server Express database engine that's targeted for program development.</span></span> <span data-ttu-id="47ad6-125">LocalDB se inicia a petición y se ejecuta en modo de usuario, sin necesidad de una configuración compleja.</span><span class="sxs-lookup"><span data-stu-id="47ad6-125">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="47ad6-126">De forma predeterminada, la base de datos LocalDB crea archivos `*.mdf` en el directorio `C:/Users/<user/>`.</span><span class="sxs-lookup"><span data-stu-id="47ad6-126">By default, LocalDB database creates `*.mdf` files in the `C:/Users/<user/>` directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="47ad6-127">En el menú **Ver**, abra **Explorador de objetos de SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="47ad6-127">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menú Ver](sql/_static/ssox.png)

* <span data-ttu-id="47ad6-129">Haga clic con el botón derecho en la tabla `Movie` y seleccione **Diseñador de vistas**:</span><span class="sxs-lookup"><span data-stu-id="47ad6-129">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Menú contextual abierto en la tabla Movie](sql/_static/design.png)

  ![Tabla Movie abierta en el diseñador](sql/_static/dv.png)

<span data-ttu-id="47ad6-132">Observe el icono de llave junto a `ID`.</span><span class="sxs-lookup"><span data-stu-id="47ad6-132">Note the key icon next to `ID`.</span></span> <span data-ttu-id="47ad6-133">De forma predeterminada, EF crea una propiedad denominada `ID` para la clave principal.</span><span class="sxs-lookup"><span data-stu-id="47ad6-133">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="47ad6-134">Haga clic con el botón derecho en la tabla `Movie` y seleccione **Ver datos**:</span><span class="sxs-lookup"><span data-stu-id="47ad6-134">Right click on the `Movie` table and select **View Data**:</span></span>

  <span data-ttu-id="47ad6-135">![Tabla Movie abierta mostrando datos de la tabla](sql/_static/vd22.png)
<!-- Code --------------------------></span><span class="sxs-lookup"><span data-stu-id="47ad6-135">![Movie table open showing table data](sql/_static/vd22.png)
<!-- Code --------------------------></span></span>
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="47ad6-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="47ad6-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/rp/sqlite.md)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="47ad6-137">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="47ad6-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]

---  
<!-- End of VS tabs -->

## <a name="seed-the-database"></a><span data-ttu-id="47ad6-138">Inicializar la base de datos</span><span class="sxs-lookup"><span data-stu-id="47ad6-138">Seed the database</span></span>

<span data-ttu-id="47ad6-139">Cree una nueva clase denominada `SeedData` en la carpeta *Models* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="47ad6-139">Create a new class named `SeedData` in the *Models* folder with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="47ad6-140">Si hay alguna película en la base de datos, se devuelve el inicializador y no se agrega ninguna película.</span><span class="sxs-lookup"><span data-stu-id="47ad6-140">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="47ad6-141">Agregar el inicializador</span><span class="sxs-lookup"><span data-stu-id="47ad6-141">Add the seed initializer</span></span>

<span data-ttu-id="47ad6-142">En *Program.cs*, modifique el método `Main` para que haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="47ad6-142">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="47ad6-143">Obtener una instancia del contexto de base de datos desde el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="47ad6-143">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="47ad6-144">Llamar al método de inicialización, pasándolo al contexto.</span><span class="sxs-lookup"><span data-stu-id="47ad6-144">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="47ad6-145">Eliminar el contexto cuando el método de inicialización finalice.</span><span class="sxs-lookup"><span data-stu-id="47ad6-145">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="47ad6-146">En el código siguiente se muestra el archivo *Program.cs* actualizado.</span><span class="sxs-lookup"><span data-stu-id="47ad6-146">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Program.cs)]

<span data-ttu-id="47ad6-147">Una aplicación de producción no llamaría a `Database.Migrate`.</span><span class="sxs-lookup"><span data-stu-id="47ad6-147">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="47ad6-148">Se agrega al código anterior para evitar que se produzca la siguiente excepción cuando `Update-Database` no se ha ejecutado:</span><span class="sxs-lookup"><span data-stu-id="47ad6-148">It's added to the preceding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="47ad6-149">SqlException: No se puede abrir la base de datos "RazorPagesMovieContext-21" solicitada por el inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="47ad6-149">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="47ad6-150">Error de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="47ad6-150">The login failed.</span></span>
<span data-ttu-id="47ad6-151">Error de inicio de sesión del usuario <nombre de usuario>.</span><span class="sxs-lookup"><span data-stu-id="47ad6-151">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="47ad6-152">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="47ad6-152">Test the app</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="47ad6-153">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="47ad6-153">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="47ad6-154">Elimine todos los registros de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="47ad6-154">Delete all the records in the DB.</span></span> <span data-ttu-id="47ad6-155">Puede hacerlo con los vínculos de eliminación en el explorador o desde [SSOX](xref:tutorials/razor-pages/new-field#ssox).</span><span class="sxs-lookup"><span data-stu-id="47ad6-155">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="47ad6-156">Obligue a la aplicación a inicializarse (llame a los métodos de la clase `Startup`) para que se ejecute el método de inicialización.</span><span class="sxs-lookup"><span data-stu-id="47ad6-156">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="47ad6-157">Para forzar la inicialización, se debe detener y reiniciar IIS Express.</span><span class="sxs-lookup"><span data-stu-id="47ad6-157">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="47ad6-158">Puede hacerlo con cualquiera de los siguientes enfoques:</span><span class="sxs-lookup"><span data-stu-id="47ad6-158">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="47ad6-159">Haga clic con el botón derecho en el icono Bandeja del sistema de IIS Express del área de notificación y pulse en **Salir** o en **Detener sitio**:</span><span class="sxs-lookup"><span data-stu-id="47ad6-159">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Icono Bandeja del sistema de IIS Express](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menú contextual](sql/_static/stopIIS.png)

    * <span data-ttu-id="47ad6-162">Si está ejecutando VS en modo de no depuración, presione F5 para ejecutar en modo de depuración.</span><span class="sxs-lookup"><span data-stu-id="47ad6-162">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="47ad6-163">Si está ejecutando VS en modo de depuración, detenga el depurador y presione F5.</span><span class="sxs-lookup"><span data-stu-id="47ad6-163">If you were running VS in debug mode, stop the debugger and press F5.</span></span>

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="47ad6-164">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="47ad6-164">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="47ad6-165">Elimine todos los registros de la base de datos (para que se ejecute el método de inicialización).</span><span class="sxs-lookup"><span data-stu-id="47ad6-165">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="47ad6-166">Detenga e inicie la aplicación para inicializar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="47ad6-166">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="47ad6-167">La aplicación muestra los datos inicializados.</span><span class="sxs-lookup"><span data-stu-id="47ad6-167">The app shows the seeded data.</span></span>

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="47ad6-168">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="47ad6-168">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="47ad6-169">Elimine todos los registros de la base de datos (para que se ejecute el método de inicialización).</span><span class="sxs-lookup"><span data-stu-id="47ad6-169">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="47ad6-170">Detenga e inicie la aplicación para inicializar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="47ad6-170">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="47ad6-171">La aplicación muestra los datos inicializados.</span><span class="sxs-lookup"><span data-stu-id="47ad6-171">The app shows the seeded data.</span></span>

---  
<!-- End of VS tabs -->


   
<span data-ttu-id="47ad6-172">La aplicación muestra los datos inicializados:</span><span class="sxs-lookup"><span data-stu-id="47ad6-172">The app shows the seeded data:</span></span>

![La aplicación Movie se abre en Chrome y muestra datos de la película](sql/_static/m55.png)

<span data-ttu-id="47ad6-174">El siguiente tutorial limpia la presentación de los datos.</span><span class="sxs-lookup"><span data-stu-id="47ad6-174">The next tutorial will clean up the presentation of the data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="47ad6-175">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="47ad6-175">Additional resources</span></span>

* [<span data-ttu-id="47ad6-176">Versión en YouTube de este tutorial</span><span class="sxs-lookup"><span data-stu-id="47ad6-176">YouTube version of this tutorial</span></span>](https://youtu.be/A_5ff11sDHY)

> [!div class="step-by-step"]
> <span data-ttu-id="47ad6-177">[Anterior: Razor Pages con scaffolding](xref:tutorials/razor-pages/page)
> [Siguiente: Actualización de páginas](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="47ad6-177">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>
