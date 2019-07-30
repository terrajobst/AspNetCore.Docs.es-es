---
title: Trabajar con una base de datos y ASP.NET Core
author: rick-anderson
description: En este artículo se explica cómo se trabaja con una base de datos y ASP.NET Core.
ms.author: riande
ms.date: 7/22/2019
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 197697f28e9faa45c1ac2b7f993bde15994957e5
ms.sourcegitcommit: 051f068c78931432e030b60094c38376d64d013e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2019
ms.locfileid: "68440391"
---
# <a name="work-with-a-database-and-aspnet-core"></a><span data-ttu-id="5967a-103">Trabajar con una base de datos y ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5967a-103">Work with a database and ASP.NET Core</span></span>

<span data-ttu-id="5967a-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="5967a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="5967a-105">El objeto `RazorPagesMovieContext` controla la tarea de conexión a la base de datos y asignación de objetos `Movie` a los registros de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5967a-105">The `RazorPagesMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="5967a-106">El contexto de base de datos se registra con el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) en el método `ConfigureServices` de *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="5967a-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in *Startup.cs*:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5967a-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5967a-107">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="5967a-108">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="5967a-108">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

---

<span data-ttu-id="5967a-109">El sistema [Configuración](xref:fundamentals/configuration/index) de ASP.NET Core lee el elemento `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="5967a-109">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="5967a-110">Para el desarrollo local, obtiene la cadena de conexión del archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="5967a-110">For local development, it gets the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5967a-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5967a-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5967a-112">El valor de nombre de la base de datos (`Database={Database name}`) será distinto en su código generado.</span><span class="sxs-lookup"><span data-stu-id="5967a-112">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="5967a-113">El valor de nombre es arbitrario.</span><span class="sxs-lookup"><span data-stu-id="5967a-113">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie30/appsettings.json)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="5967a-114">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="5967a-114">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

---

<span data-ttu-id="5967a-115">Cuando la aplicación se implementa en un servidor de prueba o producción, se puede utilizar una variable de entorno para establecer la cadena de conexión en un servidor de base de datos real.</span><span class="sxs-lookup"><span data-stu-id="5967a-115">When the app is deployed to a test or production server, an environment variable can be used to set the connection string to a real database server.</span></span> <span data-ttu-id="5967a-116">Para más información, vea [Configuración](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="5967a-116">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5967a-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5967a-117">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="5967a-118">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="5967a-118">SQL Server Express LocalDB</span></span>

<span data-ttu-id="5967a-119">LocalDB es una versión ligera del motor de base de datos de SQL Server Express dirigida al desarrollo de programas.</span><span class="sxs-lookup"><span data-stu-id="5967a-119">LocalDB is a lightweight version of the SQL Server Express database engine that's targeted for program development.</span></span> <span data-ttu-id="5967a-120">LocalDB se inicia a petición y se ejecuta en modo de usuario, sin necesidad de una configuración compleja.</span><span class="sxs-lookup"><span data-stu-id="5967a-120">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="5967a-121">De forma predeterminada, la base de datos LocalDB crea archivos `*.mdf` en el directorio `C:/Users/<user/>`.</span><span class="sxs-lookup"><span data-stu-id="5967a-121">By default, LocalDB database creates `*.mdf` files in the `C:/Users/<user/>` directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="5967a-122">En el menú **Ver**, abra **Explorador de objetos de SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="5967a-122">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menú Ver](sql/_static/ssox.png)

* <span data-ttu-id="5967a-124">Haga clic con el botón derecho en la tabla `Movie` y seleccione **Diseñador de vistas**:</span><span class="sxs-lookup"><span data-stu-id="5967a-124">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Menús contextuales abiertos en la tabla Movie](sql/_static/design.png)

  ![Tablas Movie abiertas en el diseñador](sql/_static/dv.png)

<span data-ttu-id="5967a-127">Observe el icono de llave junto a `ID`.</span><span class="sxs-lookup"><span data-stu-id="5967a-127">Note the key icon next to `ID`.</span></span> <span data-ttu-id="5967a-128">De forma predeterminada, EF crea una propiedad denominada `ID` para la clave principal.</span><span class="sxs-lookup"><span data-stu-id="5967a-128">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="5967a-129">Haga clic con el botón derecho en la tabla `Movie` y seleccione **Ver datos**:</span><span class="sxs-lookup"><span data-stu-id="5967a-129">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Tabla Movie abierta mostrando datos de la tabla](sql/_static/vd22.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="5967a-131">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="5967a-131">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---

## <a name="seed-the-database"></a><span data-ttu-id="5967a-132">Inicializar la base de datos</span><span class="sxs-lookup"><span data-stu-id="5967a-132">Seed the database</span></span>

<span data-ttu-id="5967a-133">Cree una nueva clase denominada `SeedData` en la carpeta *Models* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="5967a-133">Create a new class named `SeedData` in the *Models* folder with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="5967a-134">Si hay alguna película en la base de datos, se devuelve el inicializador y no se agrega ninguna película.</span><span class="sxs-lookup"><span data-stu-id="5967a-134">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="5967a-135">Agregar el inicializador</span><span class="sxs-lookup"><span data-stu-id="5967a-135">Add the seed initializer</span></span>

<span data-ttu-id="5967a-136">En *Program.cs*, modifique el método `Main` para que haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="5967a-136">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="5967a-137">Obtener una instancia del contexto de base de datos desde el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="5967a-137">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="5967a-138">Llamar al método de inicialización, pasándolo al contexto.</span><span class="sxs-lookup"><span data-stu-id="5967a-138">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="5967a-139">Eliminar el contexto cuando el método de inicialización finalice.</span><span class="sxs-lookup"><span data-stu-id="5967a-139">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="5967a-140">En el código siguiente se muestra el archivo *Program.cs* actualizado.</span><span class="sxs-lookup"><span data-stu-id="5967a-140">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Program.cs)]

<span data-ttu-id="5967a-141">Una aplicación de producción no llamaría a `Database.Migrate`.</span><span class="sxs-lookup"><span data-stu-id="5967a-141">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="5967a-142">Se agrega al código anterior para evitar que se produzca la siguiente excepción cuando `Update-Database` no se ha ejecutado:</span><span class="sxs-lookup"><span data-stu-id="5967a-142">It's added to the preceding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="5967a-143">SqlException: No se puede abrir la base de datos "RazorPagesMovieContext-21" solicitada por el inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="5967a-143">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="5967a-144">Error de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="5967a-144">The login failed.</span></span>
<span data-ttu-id="5967a-145">Error de inicio de sesión del usuario <nombre de usuario>.</span><span class="sxs-lookup"><span data-stu-id="5967a-145">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="5967a-146">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="5967a-146">Test the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5967a-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5967a-147">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5967a-148">Elimine todos los registros de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5967a-148">Delete all the records in the DB.</span></span> <span data-ttu-id="5967a-149">Puede hacerlo con los vínculos de eliminación en el explorador o desde [SSOX](xref:tutorials/razor-pages/new-field#ssox).</span><span class="sxs-lookup"><span data-stu-id="5967a-149">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="5967a-150">Obligue a la aplicación a inicializarse (llame a los métodos de la clase `Startup`) para que se ejecute el método de inicialización.</span><span class="sxs-lookup"><span data-stu-id="5967a-150">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="5967a-151">Para forzar la inicialización, se debe detener y reiniciar IIS Express.</span><span class="sxs-lookup"><span data-stu-id="5967a-151">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="5967a-152">Puede hacerlo con cualquiera de los siguientes enfoques:</span><span class="sxs-lookup"><span data-stu-id="5967a-152">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="5967a-153">Haga clic con el botón derecho en el icono Bandeja del sistema de IIS Express del área de notificación y pulse en **Salir** o en **Detener sitio**:</span><span class="sxs-lookup"><span data-stu-id="5967a-153">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Icono Bandeja del sistema de IIS Express](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menú contextual](sql/_static/stopIIS.png)

    * <span data-ttu-id="5967a-156">Si está ejecutando VS en modo de no depuración, presione F5 para ejecutar en modo de depuración.</span><span class="sxs-lookup"><span data-stu-id="5967a-156">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="5967a-157">Si está ejecutando VS en modo de depuración, detenga el depurador y presione F5.</span><span class="sxs-lookup"><span data-stu-id="5967a-157">If you were running VS in debug mode, stop the debugger and press F5.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="5967a-158">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="5967a-158">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="5967a-159">Elimine todos los registros de la base de datos (para que se ejecute el método de inicialización).</span><span class="sxs-lookup"><span data-stu-id="5967a-159">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="5967a-160">Detenga e inicie la aplicación para inicializar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5967a-160">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="5967a-161">La aplicación muestra los datos inicializados.</span><span class="sxs-lookup"><span data-stu-id="5967a-161">The app shows the seeded data.</span></span>

---

<span data-ttu-id="5967a-162">En el tutorial siguiente se mejorará la presentación de los datos.</span><span class="sxs-lookup"><span data-stu-id="5967a-162">The next tutorial will improve the presentation of the data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5967a-163">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="5967a-163">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5967a-164">[Anterior: Razor Pages con scaffolding](xref:tutorials/razor-pages/page)
> [Siguiente: Actualización de páginas](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="5967a-164">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="5967a-165">El objeto `RazorPagesMovieContext` controla la tarea de conexión a la base de datos y asignación de objetos `Movie` a los registros de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5967a-165">The `RazorPagesMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="5967a-166">El contexto de base de datos se registra con el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) en el método `ConfigureServices` de *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="5967a-166">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in *Startup.cs*:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5967a-167">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5967a-167">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="5967a-168">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="5967a-168">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

---

<span data-ttu-id="5967a-169">Para más información sobre los modelos empleados en `ConfigureServices`, vea:</span><span class="sxs-lookup"><span data-stu-id="5967a-169">For more information on the methods used in `ConfigureServices`, see:</span></span>

* <span data-ttu-id="5967a-170">[Compatibilidad con el Reglamento general de protección de datos (RGPD) en ASP.NET Core](xref:security/gdpr) para `CookiePolicyOptions`.</span><span class="sxs-lookup"><span data-stu-id="5967a-170">[EU General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr) for `CookiePolicyOptions`.</span></span>
* [<span data-ttu-id="5967a-171">SetCompatibilityVersion</span><span class="sxs-lookup"><span data-stu-id="5967a-171">SetCompatibilityVersion</span></span>](xref:mvc/compatibility-version)

<span data-ttu-id="5967a-172">El sistema [Configuración](xref:fundamentals/configuration/index) de ASP.NET Core lee el elemento `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="5967a-172">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="5967a-173">Para el desarrollo local, obtiene la cadena de conexión del archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="5967a-173">For local development, it gets the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5967a-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5967a-174">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5967a-175">El valor de nombre de la base de datos (`Database={Database name}`) será distinto en su código generado.</span><span class="sxs-lookup"><span data-stu-id="5967a-175">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="5967a-176">El valor de nombre es arbitrario.</span><span class="sxs-lookup"><span data-stu-id="5967a-176">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie22/appsettings.json)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5967a-177">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5967a-177">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5967a-178">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="5967a-178">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

---

<span data-ttu-id="5967a-179">Cuando la aplicación se implementa en un servidor de prueba o producción, se puede utilizar una variable de entorno para establecer la cadena de conexión en un servidor de base de datos real.</span><span class="sxs-lookup"><span data-stu-id="5967a-179">When the app is deployed to a test or production server, an environment variable can be used to set the connection string to a real database server.</span></span> <span data-ttu-id="5967a-180">Para más información, vea [Configuración](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="5967a-180">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5967a-181">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5967a-181">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="5967a-182">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="5967a-182">SQL Server Express LocalDB</span></span>

<span data-ttu-id="5967a-183">LocalDB es una versión ligera del motor de base de datos de SQL Server Express dirigida al desarrollo de programas.</span><span class="sxs-lookup"><span data-stu-id="5967a-183">LocalDB is a lightweight version of the SQL Server Express database engine that's targeted for program development.</span></span> <span data-ttu-id="5967a-184">LocalDB se inicia a petición y se ejecuta en modo de usuario, sin necesidad de una configuración compleja.</span><span class="sxs-lookup"><span data-stu-id="5967a-184">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="5967a-185">De forma predeterminada, la base de datos LocalDB crea archivos `*.mdf` en el directorio `C:/Users/<user/>`.</span><span class="sxs-lookup"><span data-stu-id="5967a-185">By default, LocalDB database creates `*.mdf` files in the `C:/Users/<user/>` directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="5967a-186">En el menú **Ver**, abra **Explorador de objetos de SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="5967a-186">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menú Ver](sql/_static/ssox.png)

* <span data-ttu-id="5967a-188">Haga clic con el botón derecho en la tabla `Movie` y seleccione **Diseñador de vistas**:</span><span class="sxs-lookup"><span data-stu-id="5967a-188">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Menú contextual abierto en la tabla Movie](sql/_static/design.png)

  ![Tabla Movie abierta en el diseñador](sql/_static/dv.png)

<span data-ttu-id="5967a-191">Observe el icono de llave junto a `ID`.</span><span class="sxs-lookup"><span data-stu-id="5967a-191">Note the key icon next to `ID`.</span></span> <span data-ttu-id="5967a-192">De forma predeterminada, EF crea una propiedad denominada `ID` para la clave principal.</span><span class="sxs-lookup"><span data-stu-id="5967a-192">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="5967a-193">Haga clic con el botón derecho en la tabla `Movie` y seleccione **Ver datos**:</span><span class="sxs-lookup"><span data-stu-id="5967a-193">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Tabla Movie abierta mostrando datos de la tabla](sql/_static/vd22.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5967a-195">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5967a-195">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5967a-196">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="5967a-196">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---

## <a name="seed-the-database"></a><span data-ttu-id="5967a-197">Inicializar la base de datos</span><span class="sxs-lookup"><span data-stu-id="5967a-197">Seed the database</span></span>

<span data-ttu-id="5967a-198">Cree una nueva clase denominada `SeedData` en la carpeta *Models* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="5967a-198">Create a new class named `SeedData` in the *Models* folder with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="5967a-199">Si hay alguna película en la base de datos, se devuelve el inicializador y no se agrega ninguna película.</span><span class="sxs-lookup"><span data-stu-id="5967a-199">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="5967a-200">Agregar el inicializador</span><span class="sxs-lookup"><span data-stu-id="5967a-200">Add the seed initializer</span></span>

<span data-ttu-id="5967a-201">En *Program.cs*, modifique el método `Main` para que haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="5967a-201">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="5967a-202">Obtener una instancia del contexto de base de datos desde el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="5967a-202">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="5967a-203">Llamar al método de inicialización, pasándolo al contexto.</span><span class="sxs-lookup"><span data-stu-id="5967a-203">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="5967a-204">Eliminar el contexto cuando el método de inicialización finalice.</span><span class="sxs-lookup"><span data-stu-id="5967a-204">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="5967a-205">En el código siguiente se muestra el archivo *Program.cs* actualizado.</span><span class="sxs-lookup"><span data-stu-id="5967a-205">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Program.cs)]

<span data-ttu-id="5967a-206">Una aplicación de producción no llamaría a `Database.Migrate`.</span><span class="sxs-lookup"><span data-stu-id="5967a-206">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="5967a-207">Se agrega al código anterior para evitar que se produzca la siguiente excepción cuando `Update-Database` no se ha ejecutado:</span><span class="sxs-lookup"><span data-stu-id="5967a-207">It's added to the preceding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="5967a-208">SqlException: No se puede abrir la base de datos "RazorPagesMovieContext-21" solicitada por el inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="5967a-208">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="5967a-209">Error de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="5967a-209">The login failed.</span></span>
<span data-ttu-id="5967a-210">Error de inicio de sesión del usuario <nombre de usuario>.</span><span class="sxs-lookup"><span data-stu-id="5967a-210">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="5967a-211">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="5967a-211">Test the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5967a-212">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5967a-212">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5967a-213">Elimine todos los registros de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5967a-213">Delete all the records in the DB.</span></span> <span data-ttu-id="5967a-214">Puede hacerlo con los vínculos de eliminación en el explorador o desde [SSOX](xref:tutorials/razor-pages/new-field#ssox).</span><span class="sxs-lookup"><span data-stu-id="5967a-214">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="5967a-215">Obligue a la aplicación a inicializarse (llame a los métodos de la clase `Startup`) para que se ejecute el método de inicialización.</span><span class="sxs-lookup"><span data-stu-id="5967a-215">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="5967a-216">Para forzar la inicialización, se debe detener y reiniciar IIS Express.</span><span class="sxs-lookup"><span data-stu-id="5967a-216">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="5967a-217">Puede hacerlo con cualquiera de los siguientes enfoques:</span><span class="sxs-lookup"><span data-stu-id="5967a-217">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="5967a-218">Haga clic con el botón derecho en el icono Bandeja del sistema de IIS Express del área de notificación y pulse en **Salir** o en **Detener sitio**:</span><span class="sxs-lookup"><span data-stu-id="5967a-218">Right-click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Icono Bandeja del sistema de IIS Express](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menú contextual](sql/_static/stopIIS.png)

    * <span data-ttu-id="5967a-221">Si está ejecutando VS en modo de no depuración, presione F5 para ejecutar en modo de depuración.</span><span class="sxs-lookup"><span data-stu-id="5967a-221">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="5967a-222">Si está ejecutando VS en modo de depuración, detenga el depurador y presione F5.</span><span class="sxs-lookup"><span data-stu-id="5967a-222">If you were running VS in debug mode, stop the debugger and press F5.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5967a-223">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5967a-223">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="5967a-224">Elimine todos los registros de la base de datos (para que se ejecute el método de inicialización).</span><span class="sxs-lookup"><span data-stu-id="5967a-224">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="5967a-225">Detenga e inicie la aplicación para inicializar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5967a-225">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="5967a-226">La aplicación muestra los datos inicializados.</span><span class="sxs-lookup"><span data-stu-id="5967a-226">The app shows the seeded data.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5967a-227">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="5967a-227">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="5967a-228">Elimine todos los registros de la base de datos (para que se ejecute el método de inicialización).</span><span class="sxs-lookup"><span data-stu-id="5967a-228">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="5967a-229">Detenga e inicie la aplicación para inicializar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5967a-229">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="5967a-230">La aplicación muestra los datos inicializados.</span><span class="sxs-lookup"><span data-stu-id="5967a-230">The app shows the seeded data.</span></span>

---

<span data-ttu-id="5967a-231">La aplicación muestra los datos inicializados:</span><span class="sxs-lookup"><span data-stu-id="5967a-231">The app shows the seeded data:</span></span>

![La aplicación Movie se abre en Chrome y muestra datos de la película](sql/_static/m55.png)

<span data-ttu-id="5967a-233">El siguiente tutorial limpia la presentación de los datos.</span><span class="sxs-lookup"><span data-stu-id="5967a-233">The next tutorial will clean up the presentation of the data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5967a-234">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="5967a-234">Additional resources</span></span>

* [<span data-ttu-id="5967a-235">Versión en YouTube de este tutorial</span><span class="sxs-lookup"><span data-stu-id="5967a-235">YouTube version of this tutorial</span></span>](https://youtu.be/A_5ff11sDHY)

> [!div class="step-by-step"]
> <span data-ttu-id="5967a-236">[Anterior: Razor Pages con scaffolding](xref:tutorials/razor-pages/page)
> [Siguiente: Actualización de páginas](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="5967a-236">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>

::: moniker-end