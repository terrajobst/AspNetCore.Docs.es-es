---
title: Trabajar con SQL Server LocalDB
author: rick-anderson
description: "Empleo de SQL Server LocalDB con una aplicación MVC sencilla"
keywords: ASP.NET Core,SQL Server LocalDB,SQL Server,LocalDB
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.assetid: ff8fd9b8-7c98-424d-8641-7524e23bf541
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: e44b6de13540d93337bf9a128d287808cffbfb46
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2017
---
# <a name="working-with-sql-server-localdb"></a><span data-ttu-id="e4b05-104">Trabajar con SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="e4b05-104">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="e4b05-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e4b05-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e4b05-106">El objeto `MvcMovieContext` controla la tarea de conexión a la base de datos y asignación de objetos `Movie` a los registros de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="e4b05-106">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="e4b05-107">El contexto de base de datos se registra con el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) en el método `ConfigureServices` del archivo *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e4b05-107">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

<span data-ttu-id="e4b05-108">El sistema [Configuración](xref:fundamentals/configuration) de ASP.NET Core lee el elemento `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="e4b05-108">The ASP.NET Core [Configuration](xref:fundamentals/configuration) system reads the `ConnectionString`.</span></span> <span data-ttu-id="e4b05-109">Para el desarrollo local, obtiene la cadena de conexión del archivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="e4b05-109">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-javascript[Main](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="e4b05-110">Al implementar la aplicación en un servidor de producción o de prueba, puede usar una variable de entorno u otro enfoque para establecer la cadena de conexión en una instancia real de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e4b05-110">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="e4b05-111">Para más información, vea [Configuración](xref:fundamentals/configuration).</span><span class="sxs-lookup"><span data-stu-id="e4b05-111">See [Configuration](xref:fundamentals/configuration) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="e4b05-112">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="e4b05-112">SQL Server Express LocalDB</span></span>

<span data-ttu-id="e4b05-113">LocalDB es una versión ligera del motor de base de datos de SQL Server Express dirigida al desarrollo de programas.</span><span class="sxs-lookup"><span data-stu-id="e4b05-113">LocalDB is a lightweight version of the SQL Server Express Database Engine that is targeted for program development.</span></span> <span data-ttu-id="e4b05-114">LocalDB se inicia a petición y se ejecuta en modo de usuario, sin necesidad de una configuración compleja.</span><span class="sxs-lookup"><span data-stu-id="e4b05-114">LocalDB starts on demand and runs in user mode, so there is no complex configuration.</span></span> <span data-ttu-id="e4b05-115">De forma predeterminada, la base de datos LocalDB crea archivos "\*.mdf" en el directorio *C:/Users/\<usuario\>*.</span><span class="sxs-lookup"><span data-stu-id="e4b05-115">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

* <span data-ttu-id="e4b05-116">En el menú **Ver**, abra **Explorador de objetos de SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="e4b05-116">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menú Ver](working-with-sql/_static/ssox.png)

* <span data-ttu-id="e4b05-118">Haga clic con el botón derecho en la tabla `Movie` **> Diseñador de vistas**.</span><span class="sxs-lookup"><span data-stu-id="e4b05-118">Right click on the `Movie` table **> View Designer**</span></span>

  ![Menú contextual abierto en la tabla Movie](working-with-sql/_static/design.png)

  ![Tabla Movie abierta en el diseñador](working-with-sql/_static/dv.png)

<span data-ttu-id="e4b05-121">Observe el icono de llave junto a `ID`.</span><span class="sxs-lookup"><span data-stu-id="e4b05-121">Note the key icon next to `ID`.</span></span> <span data-ttu-id="e4b05-122">De forma predeterminada, EF convierte una propiedad denominada `ID` en la clave principal.</span><span class="sxs-lookup"><span data-stu-id="e4b05-122">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="e4b05-123">Haga clic con el botón derecho en la tabla `Movie` **> Ver datos**</span><span class="sxs-lookup"><span data-stu-id="e4b05-123">Right click on the `Movie` table **> View Data**</span></span>

  ![Menú contextual abierto en la tabla Movie](working-with-sql/_static/ssox2.png)

  ![Tabla Movie abierta mostrando datos de la tabla](working-with-sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="e4b05-126">Inicialización de la base de datos</span><span class="sxs-lookup"><span data-stu-id="e4b05-126">Seed the database</span></span>

<span data-ttu-id="e4b05-127">Cree una nueva clase denominada `SeedData` en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="e4b05-127">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="e4b05-128">Reemplace el código generado por el siguiente:</span><span class="sxs-lookup"><span data-stu-id="e4b05-128">Replace the generated code with the following:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="e4b05-129">Si hay alguna película en la base de datos, se devuelve el inicializador y no se agrega ninguna película.</span><span class="sxs-lookup"><span data-stu-id="e4b05-129">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="e4b05-130">Adición del inicializador</span><span class="sxs-lookup"><span data-stu-id="e4b05-130">Add the seed initializer</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e4b05-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e4b05-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e4b05-132">Agregue el inicializador al método `Main` del archivo *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="e4b05-132">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e4b05-133">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e4b05-133">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e4b05-134">Agregue el inicializador al final del método `Configure` del archivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="e4b05-134">Add the seed initializer to the end of the `Configure` method in the *Startup.cs* file.</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?highlight=9&name=snippet_seed)]

---

<span data-ttu-id="e4b05-135">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="e4b05-135">Test the app</span></span>

* <span data-ttu-id="e4b05-136">Elimine todos los registros de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="e4b05-136">Delete all the records in the DB.</span></span> <span data-ttu-id="e4b05-137">Puede hacerlo con los vínculos de eliminación en el explorador o desde SSOX.</span><span class="sxs-lookup"><span data-stu-id="e4b05-137">You can do this with the delete links in the browser or from SSOX.</span></span>
* <span data-ttu-id="e4b05-138">Obligue a la aplicación a inicializarse (llame a los métodos de la clase `Startup`) para que se ejecute el método de inicialización.</span><span class="sxs-lookup"><span data-stu-id="e4b05-138">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="e4b05-139">Para forzar la inicialización, se debe detener y reiniciar IIS Express.</span><span class="sxs-lookup"><span data-stu-id="e4b05-139">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="e4b05-140">Puede hacerlo con cualquiera de los siguientes enfoques:</span><span class="sxs-lookup"><span data-stu-id="e4b05-140">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="e4b05-141">Haga clic con el botón derecho en el icono Bandeja del sistema de IIS Express del área de notificación y pulse en **Salir** o en **Detener sitio**.</span><span class="sxs-lookup"><span data-stu-id="e4b05-141">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![Icono Bandeja del sistema de IIS Express](working-with-sql/_static/iisExIcon.png)

    ![Menú contextual](working-with-sql/_static/stopIIS.png)

   * <span data-ttu-id="e4b05-144">Si está ejecutando VS en modo de no depuración, presione F5 para ejecutar en modo de depuración</span><span class="sxs-lookup"><span data-stu-id="e4b05-144">If you were running VS in non-debug mode, press F5 to run in debug mode</span></span>
   * <span data-ttu-id="e4b05-145">Si está ejecutando VS en modo de depuración, detenga el depurador y presione F5</span><span class="sxs-lookup"><span data-stu-id="e4b05-145">If you were running VS in debug mode, stop the debugger and press F5</span></span>
   
<span data-ttu-id="e4b05-146">La aplicación muestra los datos de inicialización.</span><span class="sxs-lookup"><span data-stu-id="e4b05-146">The app shows the seeded data.</span></span>

![La aplicación Movie de MVC se abre en Microsoft Edge y muestra datos de la película](working-with-sql/_static/m55.png)

>[!div class="step-by-step"]
<span data-ttu-id="e4b05-148">[Anterior](adding-model.md)
[Siguiente](controller-methods-views.md)</span><span class="sxs-lookup"><span data-stu-id="e4b05-148">[Previous](adding-model.md)
[Next](controller-methods-views.md)</span></span>  
