---
title: Trabajar con SQL Server LocalDB y ASP.NET Core
author: rick-anderson
description: Explica cómo trabajar con SQL Server LocalDB y ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/07/2017
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 1fd86fb61b7f5ddb760992ac10f9bee43ab831cf
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272148"
---
# <a name="work-with-sql-server-localdb-and-aspnet-core"></a><span data-ttu-id="bc1ff-103">Trabajar con SQL Server LocalDB y ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bc1ff-103">Work with SQL Server LocalDB and ASP.NET Core</span></span>

<span data-ttu-id="bc1ff-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="bc1ff-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span> 

<span data-ttu-id="bc1ff-105">El objeto `MovieContext` controla la tarea de conexión a la base de datos y de asignación de objetos `Movie` a los registros de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="bc1ff-105">The `MovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="bc1ff-106">El contexto de base de datos se registra con el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) en el método `ConfigureServices` del archivo *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="bc1ff-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="bc1ff-107">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]</span><span class="sxs-lookup"><span data-stu-id="bc1ff-107">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="bc1ff-108">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]</span><span class="sxs-lookup"><span data-stu-id="bc1ff-108">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]</span></span>

<span data-ttu-id="bc1ff-109">Para más información sobre los modelos empleados en `ConfigureServices`, vea:</span><span class="sxs-lookup"><span data-stu-id="bc1ff-109">For more information on the methods used in `ConfigureServices`, see:</span></span>

* <span data-ttu-id="bc1ff-110">[Compatibilidad con el Reglamento general de protección de datos (RGPD) en ASP.NET Core](xref:security/gdpr) para `CookiePolicyOptions`.</span><span class="sxs-lookup"><span data-stu-id="bc1ff-110">[EU General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr) for `CookiePolicyOptions`.</span></span>
* [<span data-ttu-id="bc1ff-111">SetCompatibilityVersion</span><span class="sxs-lookup"><span data-stu-id="bc1ff-111">SetCompatibilityVersion</span></span>](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc)

::: moniker-end

<span data-ttu-id="bc1ff-112">El sistema [Configuración](xref:fundamentals/configuration/index) de ASP.NET Core lee el elemento `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="bc1ff-112">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="bc1ff-113">Para el desarrollo local, obtiene la cadena de conexión del archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="bc1ff-113">For local development, it gets the connection string from the *appsettings.json* file.</span></span> <span data-ttu-id="bc1ff-114">El valor de nombre de la base de datos (`Database={Database name}`) será distinto en su código generado.</span><span class="sxs-lookup"><span data-stu-id="bc1ff-114">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="bc1ff-115">El valor de nombre es arbitrario.</span><span class="sxs-lookup"><span data-stu-id="bc1ff-115">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="bc1ff-116">Al implementar la aplicación en un servidor de producción o de prueba, puede usar una variable de entorno u otro enfoque para establecer la cadena de conexión en una instancia real de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="bc1ff-116">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="bc1ff-117">Para más información, vea [Configuración](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="bc1ff-117">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="bc1ff-118">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="bc1ff-118">SQL Server Express LocalDB</span></span>

<span data-ttu-id="bc1ff-119">LocalDB es una versión ligera del motor de base de datos de SQL Server Express dirigida al desarrollo de programas.</span><span class="sxs-lookup"><span data-stu-id="bc1ff-119">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="bc1ff-120">LocalDB se inicia a petición y se ejecuta en modo de usuario, sin necesidad de una configuración compleja.</span><span class="sxs-lookup"><span data-stu-id="bc1ff-120">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="bc1ff-121">De forma predeterminada, la base de datos LocalDB crea archivos "\*.mdf" en el directorio *C:/Users/\<usuario\>*.</span><span class="sxs-lookup"><span data-stu-id="bc1ff-121">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="bc1ff-122">En el menú **Ver**, abra **Explorador de objetos de SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="bc1ff-122">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menú Ver](sql/_static/ssox.png)

* <span data-ttu-id="bc1ff-124">Haga clic con el botón derecho en la tabla `Movie` y seleccione **Diseñador de vistas**:</span><span class="sxs-lookup"><span data-stu-id="bc1ff-124">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Menú contextual abierto en la tabla Movie](sql/_static/design.png)

  ![Tabla Movie abierta en el diseñador](sql/_static/dv.png)

<span data-ttu-id="bc1ff-127">Observe el icono de llave junto a `ID`.</span><span class="sxs-lookup"><span data-stu-id="bc1ff-127">Note the key icon next to `ID`.</span></span> <span data-ttu-id="bc1ff-128">De forma predeterminada, EF crea una propiedad denominada `ID` para la clave principal.</span><span class="sxs-lookup"><span data-stu-id="bc1ff-128">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="bc1ff-129">Haga clic con el botón derecho en la tabla `Movie` y seleccione **Ver datos**:</span><span class="sxs-lookup"><span data-stu-id="bc1ff-129">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Tabla Movie abierta mostrando datos de la tabla](sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="bc1ff-131">Inicialización de la base de datos</span><span class="sxs-lookup"><span data-stu-id="bc1ff-131">Seed the database</span></span>

<span data-ttu-id="bc1ff-132">Cree una nueva clase denominada `SeedData` en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="bc1ff-132">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="bc1ff-133">Reemplace el código generado con el siguiente:</span><span class="sxs-lookup"><span data-stu-id="bc1ff-133">Replace the generated code with the following:</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="bc1ff-134">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="bc1ff-134">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="bc1ff-135">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/SeedData.cs?name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="bc1ff-135">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/SeedData.cs?name=snippet_1)]</span></span>

::: moniker-end

<span data-ttu-id="bc1ff-136">Si hay alguna película en la base de datos, se devuelve el inicializador y no se agrega ninguna película.</span><span class="sxs-lookup"><span data-stu-id="bc1ff-136">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="bc1ff-137">Agregar el inicializador</span><span class="sxs-lookup"><span data-stu-id="bc1ff-137">Add the seed initializer</span></span>

<span data-ttu-id="bc1ff-138">En *Program.cs*, modifique el método `Main` para que haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="bc1ff-138">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="bc1ff-139">Obtener una instancia del contexto de base de datos desde el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="bc1ff-139">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="bc1ff-140">Llamar al método de inicialización, pasándolo al contexto.</span><span class="sxs-lookup"><span data-stu-id="bc1ff-140">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="bc1ff-141">Eliminar el contexto cuando el método de inicialización finalice.</span><span class="sxs-lookup"><span data-stu-id="bc1ff-141">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="bc1ff-142">En el código siguiente se muestra el archivo *Program.cs* actualizado.</span><span class="sxs-lookup"><span data-stu-id="bc1ff-142">The following code shows the updated *Program.cs* file.</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="bc1ff-143">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="bc1ff-143">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Program.cs)]</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="bc1ff-144">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="bc1ff-144">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Program.cs)]</span></span>

::: moniker-end

<span data-ttu-id="bc1ff-145">Una aplicación de producción no llamaría a `Database.Migrate`.</span><span class="sxs-lookup"><span data-stu-id="bc1ff-145">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="bc1ff-146">Se agrega al código anterior para evitar que se produzca la siguiente excepción cuando `Update-Database` no se ha ejecutado:</span><span class="sxs-lookup"><span data-stu-id="bc1ff-146">It's added to the preceeding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="bc1ff-147">SqlException: No se puede abrir la base de datos "RazorPagesMovieContext-21" solicitada por el inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="bc1ff-147">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="bc1ff-148">Error de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="bc1ff-148">The login failed.</span></span>
<span data-ttu-id="bc1ff-149">Error de inicio de sesión del usuario <nombre de usuario>.</span><span class="sxs-lookup"><span data-stu-id="bc1ff-149">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="bc1ff-150">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="bc1ff-150">Test the app</span></span>

* <span data-ttu-id="bc1ff-151">Elimine todos los registros de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="bc1ff-151">Delete all the records in the DB.</span></span> <span data-ttu-id="bc1ff-152">Puede hacerlo con los vínculos de eliminación en el explorador o desde [SSOX](xref:tutorials/razor-pages/new-field#ssox).</span><span class="sxs-lookup"><span data-stu-id="bc1ff-152">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="bc1ff-153">Obligue a la aplicación a inicializarse (llame a los métodos de la clase `Startup`) para que se ejecute el método de inicialización.</span><span class="sxs-lookup"><span data-stu-id="bc1ff-153">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="bc1ff-154">Para forzar la inicialización, se debe detener y reiniciar IIS Express.</span><span class="sxs-lookup"><span data-stu-id="bc1ff-154">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="bc1ff-155">Puede hacerlo con cualquiera de los siguientes enfoques:</span><span class="sxs-lookup"><span data-stu-id="bc1ff-155">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="bc1ff-156">Haga clic con el botón derecho en el icono Bandeja del sistema de IIS Express del área de notificación y pulse en **Salir** o en **Detener sitio**:</span><span class="sxs-lookup"><span data-stu-id="bc1ff-156">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Icono Bandeja del sistema de IIS Express](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menú contextual](sql/_static/stopIIS.png)

    * <span data-ttu-id="bc1ff-159">Si está ejecutando VS en modo de no depuración, presione F5 para ejecutar en modo de depuración.</span><span class="sxs-lookup"><span data-stu-id="bc1ff-159">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="bc1ff-160">Si está ejecutando VS en modo de depuración, detenga el depurador y presione F5.</span><span class="sxs-lookup"><span data-stu-id="bc1ff-160">If you were running VS in debug mode, stop the debugger and press F5.</span></span>
   
<span data-ttu-id="bc1ff-161">La aplicación muestra los datos inicializados:</span><span class="sxs-lookup"><span data-stu-id="bc1ff-161">The app shows the seeded data:</span></span>

![La aplicación Movie se abre en Chrome y muestra datos de la película](sql/_static/m55.png)

<span data-ttu-id="bc1ff-163">El siguiente tutorial limpia la presentación de los datos.</span><span class="sxs-lookup"><span data-stu-id="bc1ff-163">The next tutorial will clean up the presentation of the data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bc1ff-164">[Anterior: Páginas de Razor con scaffolding](xref:tutorials/razor-pages/page)
> [Siguiente: Actualización de las páginas](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="bc1ff-164">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>
