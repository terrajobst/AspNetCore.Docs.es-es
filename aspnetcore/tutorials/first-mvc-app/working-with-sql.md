---
title: Trabajo con SQL en una aplicación ASP.NET Core MVC
author: rick-anderson
description: Obtenga información sobre cómo usar SQL Server LocalDB o SQLite en una aplicación ASP.NET Core MVC.
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: a6b2026a43bda94a84a05749e9ea395308dec231
ms.sourcegitcommit: 34bf9fc6ea814c039401fca174642f0acb14be3c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/14/2019
ms.locfileid: "57841447"
---
# <a name="work-with-sql-in-aspnet-core"></a><span data-ttu-id="84e44-103">Trabajo con SQL en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="84e44-103">Work with SQL in ASP.NET Core</span></span>

<span data-ttu-id="84e44-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="84e44-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="84e44-105">El objeto `MvcMovieContext` controla la tarea de conexión a la base de datos y asignación de objetos `Movie` a los registros de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="84e44-105">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="84e44-106">El contexto de base de datos se registra con el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) en el método `ConfigureServices` del archivo *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="84e44-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="84e44-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="84e44-107">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

<span data-ttu-id="84e44-108">El sistema [Configuración](xref:fundamentals/configuration/index) de ASP.NET Core lee el elemento `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="84e44-108">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="84e44-109">Para el desarrollo local, obtiene la cadena de conexión del archivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="84e44-109">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

<!-- Code -------------------------->
# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="84e44-110">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="84e44-110">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="84e44-111">El sistema [Configuración](xref:fundamentals/configuration/index) de ASP.NET Core lee el elemento `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="84e44-111">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="84e44-112">Para el desarrollo local, obtiene la cadena de conexión del archivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="84e44-112">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/appsettingsSQLite.json?highlight=2&range=8-10)]

---  
<!-- End of VS tabs -->

<span data-ttu-id="84e44-113">Al implementar la aplicación en un servidor de producción o de prueba, puede usar una variable de entorno u otro enfoque para establecer la cadena de conexión en una instancia real de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="84e44-113">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="84e44-114">Para más información, vea [Configuración](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="84e44-114">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="84e44-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="84e44-115">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="84e44-116">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="84e44-116">SQL Server Express LocalDB</span></span>

<span data-ttu-id="84e44-117">LocalDB es una versión ligera del motor de base de datos de SQL Server Express dirigida al desarrollo de programas.</span><span class="sxs-lookup"><span data-stu-id="84e44-117">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="84e44-118">LocalDB se inicia a petición y se ejecuta en modo de usuario, sin necesidad de una configuración compleja.</span><span class="sxs-lookup"><span data-stu-id="84e44-118">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="84e44-119">De forma predeterminada, la base de datos LocalDB crea archivos *.mdf* en el directorio *C:/Users/{usuario}*.</span><span class="sxs-lookup"><span data-stu-id="84e44-119">By default, LocalDB database creates *.mdf* files in the *C:/Users/{user}* directory.</span></span>

* <span data-ttu-id="84e44-120">En el menú **Ver**, abra **Explorador de objetos de SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="84e44-120">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menú Ver](working-with-sql/_static/ssox.png)

* <span data-ttu-id="84e44-122">Haga clic con el botón derecho en la tabla `Movie` **> Diseñador de vistas**.</span><span class="sxs-lookup"><span data-stu-id="84e44-122">Right click on the `Movie` table **> View Designer**</span></span>

  ![Menú contextual abierto en la tabla Movie](working-with-sql/_static/design.png)

  ![Tabla Movie abierta en el diseñador](working-with-sql/_static/dv.png)

<span data-ttu-id="84e44-125">Observe el icono de llave junto a `ID`.</span><span class="sxs-lookup"><span data-stu-id="84e44-125">Note the key icon next to `ID`.</span></span> <span data-ttu-id="84e44-126">De forma predeterminada, EF convierte una propiedad denominada `ID` en la clave principal.</span><span class="sxs-lookup"><span data-stu-id="84e44-126">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="84e44-127">Haga clic con el botón derecho en la tabla `Movie` **> Ver datos**</span><span class="sxs-lookup"><span data-stu-id="84e44-127">Right click on the `Movie` table **> View Data**</span></span>

  ![Menú contextual abierto en la tabla Movie](working-with-sql/_static/ssox2.png)

  ![Tabla Movie abierta mostrando datos de la tabla](working-with-sql/_static/vd22.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="84e44-130">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="84e44-130">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---  
<!-- End of VS tabs -->

## <a name="seed-the-database"></a><span data-ttu-id="84e44-131">Inicializar la base de datos</span><span class="sxs-lookup"><span data-stu-id="84e44-131">Seed the database</span></span>

<span data-ttu-id="84e44-132">Cree una nueva clase denominada `SeedData` en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="84e44-132">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="84e44-133">Reemplace el código generado con el siguiente:</span><span class="sxs-lookup"><span data-stu-id="84e44-133">Replace the generated code with the following:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="84e44-134">Si hay alguna película en la base de datos, se devuelve el inicializador y no se agrega ninguna película.</span><span class="sxs-lookup"><span data-stu-id="84e44-134">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="84e44-135">Agregar el inicializador</span><span class="sxs-lookup"><span data-stu-id="84e44-135">Add the seed initializer</span></span>

<span data-ttu-id="84e44-136">Reemplace el contenido de *Program.cs* por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="84e44-136">Replace the contents of *Program.cs* with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Program.cs)]

<span data-ttu-id="84e44-137">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="84e44-137">Test the app</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="84e44-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="84e44-138">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="84e44-139">Elimine todos los registros de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="84e44-139">Delete all the records in the DB.</span></span> <span data-ttu-id="84e44-140">Puede hacerlo con los vínculos de eliminación en el explorador o desde SSOX.</span><span class="sxs-lookup"><span data-stu-id="84e44-140">You can do this with the delete links in the browser or from SSOX.</span></span>
* <span data-ttu-id="84e44-141">Obligue a la aplicación a inicializarse (llame a los métodos de la clase `Startup`) para que se ejecute el método de inicialización.</span><span class="sxs-lookup"><span data-stu-id="84e44-141">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="84e44-142">Para forzar la inicialización, se debe detener y reiniciar IIS Express.</span><span class="sxs-lookup"><span data-stu-id="84e44-142">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="84e44-143">Puede hacerlo con cualquiera de los siguientes enfoques:</span><span class="sxs-lookup"><span data-stu-id="84e44-143">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="84e44-144">Haga clic con el botón derecho en el icono Bandeja del sistema de IIS Express del área de notificación y pulse en **Salir** o en **Detener sitio**.</span><span class="sxs-lookup"><span data-stu-id="84e44-144">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![Icono Bandeja del sistema de IIS Express](working-with-sql/_static/iisExIcon.png)

    ![Menú contextual](working-with-sql/_static/stopIIS.png)

    * <span data-ttu-id="84e44-147">Si está ejecutando VS en modo de no depuración, presione F5 para ejecutar en modo de depuración</span><span class="sxs-lookup"><span data-stu-id="84e44-147">If you were running VS in non-debug mode, press F5 to run in debug mode</span></span>
    * <span data-ttu-id="84e44-148">Si está ejecutando VS en modo de depuración, detenga el depurador y presione F5</span><span class="sxs-lookup"><span data-stu-id="84e44-148">If you were running VS in debug mode, stop the debugger and press F5</span></span>

<!-- Code -------------------------->
# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="84e44-149">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="84e44-149">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="84e44-150">Elimine todos los registros de la base de datos (para que se ejecute el método de inicialización).</span><span class="sxs-lookup"><span data-stu-id="84e44-150">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="84e44-151">Detenga e inicie la aplicación para inicializar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="84e44-151">Stop and start the app to seed the database.</span></span>

---  
<!-- End of VS tabs -->

<span data-ttu-id="84e44-152">La aplicación muestra los datos inicializados.</span><span class="sxs-lookup"><span data-stu-id="84e44-152">The app shows the seeded data.</span></span>

![La aplicación Movie de MVC se abre en Microsoft Edge y muestra datos de la película](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> <span data-ttu-id="84e44-154">[Anterior](adding-model.md)
> [Siguiente](controller-methods-views.md)</span><span class="sxs-lookup"><span data-stu-id="84e44-154">[Previous](adding-model.md)
[Next](controller-methods-views.md)</span></span>  
