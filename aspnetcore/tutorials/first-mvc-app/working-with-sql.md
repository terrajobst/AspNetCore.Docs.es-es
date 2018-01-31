---
title: Trabajar con SQL Server LocalDB
author: rick-anderson
description: "Empleo de SQL Server LocalDB con una aplicación MVC sencilla"
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: a0aa6fdfa51650628021a4ba6d0533e7e0e39200
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="working-with-sql-server-localdb"></a><span data-ttu-id="00996-103">Trabajar con SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="00996-103">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="00996-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="00996-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="00996-105">El objeto `MvcMovieContext` controla la tarea de conexión a la base de datos y asignación de objetos `Movie` a los registros de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="00996-105">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="00996-106">El contexto de base de datos se registra con el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) en el método `ConfigureServices` del archivo *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="00996-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

<span data-ttu-id="00996-107">El sistema [Configuración](xref:fundamentals/configuration/index) de ASP.NET Core lee el elemento `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="00996-107">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="00996-108">Para el desarrollo local, obtiene la cadena de conexión del archivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="00996-108">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[Main](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="00996-109">Al implementar la aplicación en un servidor de producción o de prueba, puede usar una variable de entorno u otro enfoque para establecer la cadena de conexión en una instancia real de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="00996-109">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="00996-110">Para más información, vea [Configuración](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="00996-110">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="00996-111">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="00996-111">SQL Server Express LocalDB</span></span>

<span data-ttu-id="00996-112">LocalDB es una versión ligera del motor de base de datos de SQL Server Express dirigida al desarrollo de programas.</span><span class="sxs-lookup"><span data-stu-id="00996-112">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="00996-113">LocalDB se inicia a petición y se ejecuta en modo de usuario, sin necesidad de una configuración compleja.</span><span class="sxs-lookup"><span data-stu-id="00996-113">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="00996-114">De forma predeterminada, la base de datos LocalDB crea archivos "\*.mdf" en el directorio *C:/Users/\<usuario\>*.</span><span class="sxs-lookup"><span data-stu-id="00996-114">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

* <span data-ttu-id="00996-115">En el menú **Ver**, abra **Explorador de objetos de SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="00996-115">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menú Ver](working-with-sql/_static/ssox.png)

* <span data-ttu-id="00996-117">Haga clic con el botón derecho en la tabla `Movie` **> Diseñador de vistas**.</span><span class="sxs-lookup"><span data-stu-id="00996-117">Right click on the `Movie` table **> View Designer**</span></span>

  ![Menú contextual abierto en la tabla Movie](working-with-sql/_static/design.png)

  ![Tabla Movie abierta en el diseñador](working-with-sql/_static/dv.png)

<span data-ttu-id="00996-120">Observe el icono de llave junto a `ID`.</span><span class="sxs-lookup"><span data-stu-id="00996-120">Note the key icon next to `ID`.</span></span> <span data-ttu-id="00996-121">De forma predeterminada, EF convierte una propiedad denominada `ID` en la clave principal.</span><span class="sxs-lookup"><span data-stu-id="00996-121">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="00996-122">Haga clic con el botón derecho en la tabla `Movie` **> Ver datos**</span><span class="sxs-lookup"><span data-stu-id="00996-122">Right click on the `Movie` table **> View Data**</span></span>

  ![Menú contextual abierto en la tabla Movie](working-with-sql/_static/ssox2.png)

  ![Tabla Movie abierta mostrando datos de la tabla](working-with-sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="00996-125">Inicialización de la base de datos</span><span class="sxs-lookup"><span data-stu-id="00996-125">Seed the database</span></span>

<span data-ttu-id="00996-126">Cree una nueva clase denominada `SeedData` en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="00996-126">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="00996-127">Reemplace el código generado con el siguiente:</span><span class="sxs-lookup"><span data-stu-id="00996-127">Replace the generated code with the following:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="00996-128">Si hay alguna película en la base de datos, se devuelve el inicializador y no se agrega ninguna película.</span><span class="sxs-lookup"><span data-stu-id="00996-128">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="00996-129">Adición del inicializador</span><span class="sxs-lookup"><span data-stu-id="00996-129">Add the seed initializer</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="00996-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="00996-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="00996-131">Agregue el inicializador al método `Main` del archivo *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="00996-131">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="00996-132">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="00996-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="00996-133">Agregue el inicializador al final del método `Configure` del archivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="00996-133">Add the seed initializer to the end of the `Configure` method in the *Startup.cs* file.</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?highlight=9&name=snippet_seed)]

---

<span data-ttu-id="00996-134">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="00996-134">Test the app</span></span>

* <span data-ttu-id="00996-135">Elimine todos los registros de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="00996-135">Delete all the records in the DB.</span></span> <span data-ttu-id="00996-136">Puede hacerlo con los vínculos de eliminación en el explorador o desde SSOX.</span><span class="sxs-lookup"><span data-stu-id="00996-136">You can do this with the delete links in the browser or from SSOX.</span></span>
* <span data-ttu-id="00996-137">Obligue a la aplicación a inicializarse (llame a los métodos de la clase `Startup`) para que se ejecute el método de inicialización.</span><span class="sxs-lookup"><span data-stu-id="00996-137">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="00996-138">Para forzar la inicialización, se debe detener y reiniciar IIS Express.</span><span class="sxs-lookup"><span data-stu-id="00996-138">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="00996-139">Puede hacerlo con cualquiera de los siguientes enfoques:</span><span class="sxs-lookup"><span data-stu-id="00996-139">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="00996-140">Haga clic con el botón derecho en el icono Bandeja del sistema de IIS Express del área de notificación y pulse en **Salir** o en **Detener sitio**.</span><span class="sxs-lookup"><span data-stu-id="00996-140">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![Icono Bandeja del sistema de IIS Express](working-with-sql/_static/iisExIcon.png)

    ![Menú contextual](working-with-sql/_static/stopIIS.png)

   * <span data-ttu-id="00996-143">Si está ejecutando VS en modo de no depuración, presione F5 para ejecutar en modo de depuración</span><span class="sxs-lookup"><span data-stu-id="00996-143">If you were running VS in non-debug mode, press F5 to run in debug mode</span></span>
   * <span data-ttu-id="00996-144">Si está ejecutando VS en modo de depuración, detenga el depurador y presione F5</span><span class="sxs-lookup"><span data-stu-id="00996-144">If you were running VS in debug mode, stop the debugger and press F5</span></span>
   
<span data-ttu-id="00996-145">La aplicación muestra los datos inicializados.</span><span class="sxs-lookup"><span data-stu-id="00996-145">The app shows the seeded data.</span></span>

![La aplicación Movie de MVC se abre en Microsoft Edge y muestra datos de la película](working-with-sql/_static/m55.png)

>[!div class="step-by-step"]
<span data-ttu-id="00996-147">[Anterior](adding-model.md)
[Siguiente](controller-methods-views.md)</span><span class="sxs-lookup"><span data-stu-id="00996-147">[Previous](adding-model.md)
[Next](controller-methods-views.md)</span></span>  
