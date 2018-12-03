---
title: Trabajar con SQL Server LocalDB en una aplicación de ASP.NET Core MVC
author: rick-anderson
description: Obtenga información sobre cómo usar SQL Server LocalDB en una sencilla aplicación ASP.NET Core MVC.
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: 49615c25d51cfa671157c2e56b8e0753719c678a
ms.sourcegitcommit: c4572be5ebb301013a5698caf9b5572b76cb2e34
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "52710106"
---
# <a name="work-with-sql-server-localdb-in-aspnet-core"></a><span data-ttu-id="500f6-103">Trabajar con SQL Server LocalDB en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="500f6-103">Work with SQL Server LocalDB in ASP.NET Core</span></span>

<span data-ttu-id="500f6-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="500f6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="500f6-105">El objeto `MvcMovieContext` controla la tarea de conexión a la base de datos y asignación de objetos `Movie` a los registros de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="500f6-105">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="500f6-106">El contexto de base de datos se registra con el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) en el método `ConfigureServices` del archivo *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="500f6-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

::: moniker-end

<span data-ttu-id="500f6-107">El sistema [Configuración](xref:fundamentals/configuration/index) de ASP.NET Core lee el elemento `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="500f6-107">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="500f6-108">Para el desarrollo local, obtiene la cadena de conexión del archivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="500f6-108">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="500f6-109">Al implementar la aplicación en un servidor de producción o de prueba, puede usar una variable de entorno u otro enfoque para establecer la cadena de conexión en una instancia real de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="500f6-109">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="500f6-110">Para más información, vea [Configuración](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="500f6-110">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="500f6-111">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="500f6-111">SQL Server Express LocalDB</span></span>

<span data-ttu-id="500f6-112">LocalDB es una versión ligera del motor de base de datos de SQL Server Express dirigida al desarrollo de programas.</span><span class="sxs-lookup"><span data-stu-id="500f6-112">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="500f6-113">LocalDB se inicia a petición y se ejecuta en modo de usuario, sin necesidad de una configuración compleja.</span><span class="sxs-lookup"><span data-stu-id="500f6-113">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="500f6-114">De forma predeterminada, la base de datos LocalDB crea archivos "\*.mdf" en el directorio *C:/Users/\<usuario\>*.</span><span class="sxs-lookup"><span data-stu-id="500f6-114">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

* <span data-ttu-id="500f6-115">En el menú **Ver**, abra **Explorador de objetos de SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="500f6-115">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menú Ver](working-with-sql/_static/ssox.png)

* <span data-ttu-id="500f6-117">Haga clic con el botón derecho en la tabla `Movie` **> Diseñador de vistas**.</span><span class="sxs-lookup"><span data-stu-id="500f6-117">Right click on the `Movie` table **> View Designer**</span></span>

  ![Menú contextual abierto en la tabla Movie](working-with-sql/_static/design.png)

  ![Tabla Movie abierta en el diseñador](working-with-sql/_static/dv.png)

<span data-ttu-id="500f6-120">Observe el icono de llave junto a `ID`.</span><span class="sxs-lookup"><span data-stu-id="500f6-120">Note the key icon next to `ID`.</span></span> <span data-ttu-id="500f6-121">De forma predeterminada, EF convierte una propiedad denominada `ID` en la clave principal.</span><span class="sxs-lookup"><span data-stu-id="500f6-121">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="500f6-122">Haga clic con el botón derecho en la tabla `Movie` **> Ver datos**</span><span class="sxs-lookup"><span data-stu-id="500f6-122">Right click on the `Movie` table **> View Data**</span></span>

  ![Menú contextual abierto en la tabla Movie](working-with-sql/_static/ssox2.png)

  ![Tabla Movie abierta mostrando datos de la tabla](working-with-sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="500f6-125">Inicialización de la base de datos</span><span class="sxs-lookup"><span data-stu-id="500f6-125">Seed the database</span></span>

<span data-ttu-id="500f6-126">Cree una nueva clase denominada `SeedData` en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="500f6-126">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="500f6-127">Reemplace el código generado con el siguiente:</span><span class="sxs-lookup"><span data-stu-id="500f6-127">Replace the generated code with the following:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="500f6-128">Si hay alguna película en la base de datos, se devuelve el inicializador y no se agrega ninguna película.</span><span class="sxs-lookup"><span data-stu-id="500f6-128">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="500f6-129">Agregar el inicializador</span><span class="sxs-lookup"><span data-stu-id="500f6-129">Add the seed initializer</span></span>

<span data-ttu-id="500f6-130">Reemplace el contenido de *Program.cs* por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="500f6-130">Replace the contents of *Program.cs* with the following code:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Program.cs)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="500f6-131">Agregue el inicializador al método `Main` en el archivo *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="500f6-131">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]

::: moniker-end

<span data-ttu-id="500f6-132">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="500f6-132">Test the app</span></span>

* <span data-ttu-id="500f6-133">Elimine todos los registros de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="500f6-133">Delete all the records in the DB.</span></span> <span data-ttu-id="500f6-134">Puede hacerlo con los vínculos de eliminación en el explorador o desde SSOX.</span><span class="sxs-lookup"><span data-stu-id="500f6-134">You can do this with the delete links in the browser or from SSOX.</span></span>
* <span data-ttu-id="500f6-135">Obligue a la aplicación a inicializarse (llame a los métodos de la clase `Startup`) para que se ejecute el método de inicialización.</span><span class="sxs-lookup"><span data-stu-id="500f6-135">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="500f6-136">Para forzar la inicialización, se debe detener y reiniciar IIS Express.</span><span class="sxs-lookup"><span data-stu-id="500f6-136">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="500f6-137">Puede hacerlo con cualquiera de los siguientes enfoques:</span><span class="sxs-lookup"><span data-stu-id="500f6-137">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="500f6-138">Haga clic con el botón derecho en el icono Bandeja del sistema de IIS Express del área de notificación y pulse en **Salir** o en **Detener sitio**.</span><span class="sxs-lookup"><span data-stu-id="500f6-138">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![Icono Bandeja del sistema de IIS Express](working-with-sql/_static/iisExIcon.png)

    ![Menú contextual](working-with-sql/_static/stopIIS.png)

    * <span data-ttu-id="500f6-141">Si está ejecutando VS en modo de no depuración, presione F5 para ejecutar en modo de depuración</span><span class="sxs-lookup"><span data-stu-id="500f6-141">If you were running VS in non-debug mode, press F5 to run in debug mode</span></span>
    * <span data-ttu-id="500f6-142">Si está ejecutando VS en modo de depuración, detenga el depurador y presione F5</span><span class="sxs-lookup"><span data-stu-id="500f6-142">If you were running VS in debug mode, stop the debugger and press F5</span></span>

<span data-ttu-id="500f6-143">La aplicación muestra los datos inicializados.</span><span class="sxs-lookup"><span data-stu-id="500f6-143">The app shows the seeded data.</span></span>

![La aplicación Movie de MVC se abre en Microsoft Edge y muestra datos de la película](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> <span data-ttu-id="500f6-145">[Anterior](adding-model.md)
> [Siguiente](controller-methods-views.md)</span><span class="sxs-lookup"><span data-stu-id="500f6-145">[Previous](adding-model.md)
[Next](controller-methods-views.md)</span></span>  
