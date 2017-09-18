---
title: Trabajar con SQL Server LocalDB y ASP.NET Core
author: rick-anderson
description: "Explica cómo trabajar con SQL Server LocalDB y ASP.NET Core."
keywords: "ASP.NET Core,páginas de Razor,Razor,MVC,SQL,LocalDB"
ms.author: riande
manager: wpickett
ms.date: 8/7/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 173bdcca80a599ec2d87ff4158614727b35f984a
ms.sourcegitcommit: d02d90b6272372178723ff932e8a9b9566afedb8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/15/2017
---
# <a name="working-with-sql-server-localdb-and-aspnet-core"></a><span data-ttu-id="25b6d-104">Trabajar con SQL Server LocalDB y ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="25b6d-104">Working with SQL Server LocalDB and ASP.NET Core</span></span>

<span data-ttu-id="25b6d-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="25b6d-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span> 

<span data-ttu-id="25b6d-106">El objeto `MovieContext` controla la tarea de conexión a la base de datos y de asignación de objetos `Movie` a los registros de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="25b6d-106">The `MovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="25b6d-107">El contexto de base de datos se registra con el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) en el método `ConfigureServices` del archivo *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="25b6d-107">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

<span data-ttu-id="25b6d-108">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=6-7)]</span><span class="sxs-lookup"><span data-stu-id="25b6d-108">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=6-7)]</span></span>

<span data-ttu-id="25b6d-109">El sistema [Configuración](xref:fundamentals/configuration) de ASP.NET Core lee el elemento `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="25b6d-109">The ASP.NET Core [Configuration](xref:fundamentals/configuration) system reads the `ConnectionString`.</span></span> <span data-ttu-id="25b6d-110">Para el desarrollo local, obtiene la cadena de conexión del archivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="25b6d-110">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

<span data-ttu-id="25b6d-111">[!code-javascript[Main](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]</span><span class="sxs-lookup"><span data-stu-id="25b6d-111">[!code-javascript[Main](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]</span></span>

<span data-ttu-id="25b6d-112">Al implementar la aplicación en un servidor de producción o de prueba, puede usar una variable de entorno u otro enfoque para establecer la cadena de conexión en una instancia real de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="25b6d-112">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="25b6d-113">Para más información, vea [Configuración](xref:fundamentals/configuration).</span><span class="sxs-lookup"><span data-stu-id="25b6d-113">See [Configuration](xref:fundamentals/configuration) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="25b6d-114">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="25b6d-114">SQL Server Express LocalDB</span></span>

<span data-ttu-id="25b6d-115">LocalDB es una versión ligera del motor de base de datos de SQL Server Express dirigida al desarrollo de programas.</span><span class="sxs-lookup"><span data-stu-id="25b6d-115">LocalDB is a lightweight version of the SQL Server Express Database Engine that is targeted for program development.</span></span> <span data-ttu-id="25b6d-116">LocalDB se inicia a petición y se ejecuta en modo de usuario, sin necesidad de una configuración compleja.</span><span class="sxs-lookup"><span data-stu-id="25b6d-116">LocalDB starts on demand and runs in user mode, so there is no complex configuration.</span></span> <span data-ttu-id="25b6d-117">De forma predeterminada, la base de datos LocalDB crea archivos "\*.mdf" en el directorio *C:/Users/\<usuario\>*.</span><span class="sxs-lookup"><span data-stu-id="25b6d-117">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="25b6d-118">En el menú **Ver**, abra **Explorador de objetos de SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="25b6d-118">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menú Ver](sql/_static/ssox.png)

* <span data-ttu-id="25b6d-120">Haga clic con el botón derecho en la tabla `Movie` **> Diseñador de vistas**</span><span class="sxs-lookup"><span data-stu-id="25b6d-120">Right click on the `Movie` table **> View Designer**</span></span>

  ![Menú contextual abierto en la tabla Movie](sql/_static/design.png)

  ![Tabla Movie abierta en el diseñador](sql/_static/dv.png)

<span data-ttu-id="25b6d-123">Observe el icono de llave junto a `ID`.</span><span class="sxs-lookup"><span data-stu-id="25b6d-123">Note the key icon next to `ID`.</span></span> <span data-ttu-id="25b6d-124">De forma predeterminada, EF convierte a una propiedad denominada `ID` en la clave principal.</span><span class="sxs-lookup"><span data-stu-id="25b6d-124">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="25b6d-125">Haga clic con el botón derecho en la tabla `Movie` **> Ver datos**</span><span class="sxs-lookup"><span data-stu-id="25b6d-125">Right click on the `Movie` table **> View Data**</span></span>

  ![Tabla Movie abierta mostrando datos de la tabla](sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="25b6d-127">Inicialización de la base de datos</span><span class="sxs-lookup"><span data-stu-id="25b6d-127">Seed the database</span></span>

<span data-ttu-id="25b6d-128">Cree una nueva clase denominada `SeedData` en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="25b6d-128">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="25b6d-129">Reemplace el código generado por el siguiente:</span><span class="sxs-lookup"><span data-stu-id="25b6d-129">Replace the generated code with the following:</span></span>

<span data-ttu-id="25b6d-130">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="25b6d-130">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]</span></span>

<span data-ttu-id="25b6d-131">Si hay alguna película en la base de datos, se devuelve el inicializador y no se agrega ninguna película.</span><span class="sxs-lookup"><span data-stu-id="25b6d-131">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="25b6d-132">Adición del inicializador</span><span class="sxs-lookup"><span data-stu-id="25b6d-132">Add the seed initializer</span></span>

<span data-ttu-id="25b6d-133">Agregue el inicializador al final del método `Main` del archivo *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="25b6d-133">Add the seed initializer to the end of the `Main` method in the *Program.cs* file:</span></span>

<span data-ttu-id="25b6d-134">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Program.cs?highlight=6,17-32)]</span><span class="sxs-lookup"><span data-stu-id="25b6d-134">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Program.cs?highlight=6,17-32)]</span></span>

<span data-ttu-id="25b6d-135">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="25b6d-135">Test the app</span></span>

* <span data-ttu-id="25b6d-136">Elimine todos los registros de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="25b6d-136">Delete all the records in the DB.</span></span> <span data-ttu-id="25b6d-137">Puede hacerlo con los vínculos de eliminación en el explorador o desde [SSOX](xref:tutorials/razor-pages/new-field#ssox).</span><span class="sxs-lookup"><span data-stu-id="25b6d-137">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="25b6d-138">Obligue a la aplicación a inicializarse (llame a los métodos de la clase `Startup`) para que se ejecute el método de inicialización.</span><span class="sxs-lookup"><span data-stu-id="25b6d-138">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="25b6d-139">Para forzar la inicialización, se debe detener y reiniciar IIS Express.</span><span class="sxs-lookup"><span data-stu-id="25b6d-139">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="25b6d-140">Puede hacerlo con cualquiera de los siguientes enfoques:</span><span class="sxs-lookup"><span data-stu-id="25b6d-140">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="25b6d-141">Haga clic con el botón derecho en el icono Bandeja del sistema de IIS Express del área de notificación y pulse en **Salir** o en **Detener sitio**.</span><span class="sxs-lookup"><span data-stu-id="25b6d-141">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![Icono Bandeja del sistema de IIS Express](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menú contextual](sql/_static/stopIIS.png)

   * <span data-ttu-id="25b6d-144">Si está ejecutando VS en modo de no depuración, presione F5 para ejecutar en modo de depuración.</span><span class="sxs-lookup"><span data-stu-id="25b6d-144">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
   * <span data-ttu-id="25b6d-145">Si está ejecutando VS en modo de depuración, detenga el depurador y presione F5.</span><span class="sxs-lookup"><span data-stu-id="25b6d-145">If you were running VS in debug mode, stop the debugger and press F5.</span></span>
   
<span data-ttu-id="25b6d-146">La aplicación muestra los datos de inicialización.</span><span class="sxs-lookup"><span data-stu-id="25b6d-146">The app shows the seeded data.</span></span>

![La aplicación Movie se abre en Chrome y muestra datos de la película](sql/_static/m55.png)

<span data-ttu-id="25b6d-148">El siguiente tutorial limpia la presentación de los datos.</span><span class="sxs-lookup"><span data-stu-id="25b6d-148">The next tutorial will clean up the presentation of the data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="25b6d-149">[Anterior: Páginas de Razor con scaffolding](xref:tutorials/razor-pages/page) </span><span class="sxs-lookup"><span data-stu-id="25b6d-149">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page) </span></span>  
[<span data-ttu-id="25b6d-150">Siguiente: Actualización de páginas</span><span class="sxs-lookup"><span data-stu-id="25b6d-150">Next: Updating the pages</span></span>](xref:tutorials/razor-pages/da1)
