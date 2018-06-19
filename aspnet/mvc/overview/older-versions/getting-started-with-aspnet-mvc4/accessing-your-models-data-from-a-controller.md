---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: Obtiene acceso a datos de su modelo desde un controlador | Documentos de Microsoft
author: Rick-Anderson
description: 'Nota: Una versión actualizada de este tutorial está disponible aquí que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro y mucho más fácil de seguir y demostraciones...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: cf896a6a9ce6cb8cd4adb13c3d87c4e7c3095fa6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873422"
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="08bb7-104">Obtiene acceso a datos de su modelo desde un controlador</span><span class="sxs-lookup"><span data-stu-id="08bb7-104">Accessing Your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="08bb7-105">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="08bb7-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="08bb7-106">Hay disponible una versión actualizada de este tutorial [aquí](../../getting-started/introduction/getting-started.md) que usa ASP.NET MVC 5 y Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="08bb7-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="08bb7-107">Es más seguro y mucho más fácil de seguir y se muestra más características.</span><span class="sxs-lookup"><span data-stu-id="08bb7-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="08bb7-108">En esta sección, creará un nuevo `MoviesController` clase y escribir código que recupera los datos de la película y lo muestra en el explorador utilizando una plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="08bb7-108">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span>

<span data-ttu-id="08bb7-109">**Compile la aplicación** antes de pasar al paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="08bb7-109">**Build the application** before going on to the next step.</span></span>

<span data-ttu-id="08bb7-110">Haga clic en el *controladores* carpeta y cree un nuevo `MoviesController` controlador.</span><span class="sxs-lookup"><span data-stu-id="08bb7-110">Right-click the *Controllers* folder and create a new `MoviesController` controller.</span></span> <span data-ttu-id="08bb7-111">Las opciones siguientes no aparecerán hasta que se genere la aplicación.</span><span class="sxs-lookup"><span data-stu-id="08bb7-111">The options below will not appear until you build your application.</span></span> <span data-ttu-id="08bb7-112">Seleccione las opciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="08bb7-112">Select the following options:</span></span>

- <span data-ttu-id="08bb7-113">Nombre del controlador: **MoviesController**.</span><span class="sxs-lookup"><span data-stu-id="08bb7-113">Controller name: **MoviesController**.</span></span> <span data-ttu-id="08bb7-114">(Este es el valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="08bb7-114">(This is the default.</span></span> <span data-ttu-id="08bb7-115">)</span><span class="sxs-lookup"><span data-stu-id="08bb7-115">)</span></span>
- <span data-ttu-id="08bb7-116">Plantilla: **controlador de MVC con acciones de lectura/escritura y vistas, mediante Entity Framework**.</span><span class="sxs-lookup"><span data-stu-id="08bb7-116">Template: **MVC Controller with read/write actions and views, using Entity Framework**.</span></span>
- <span data-ttu-id="08bb7-117">Clase de modelo: **película (MvcMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="08bb7-117">Model class: **Movie (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="08bb7-118">Clase de contexto de datos: **MovieDBContext (MvcMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="08bb7-118">Data context class: **MovieDBContext (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="08bb7-119">Vistas: **Razor (CSHTML)**.</span><span class="sxs-lookup"><span data-stu-id="08bb7-119">Views: **Razor (CSHTML)**.</span></span> <span data-ttu-id="08bb7-120">(El valor predeterminado.)</span><span class="sxs-lookup"><span data-stu-id="08bb7-120">(The default.)</span></span>

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

<span data-ttu-id="08bb7-122">Haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="08bb7-122">Click **Add**.</span></span> <span data-ttu-id="08bb7-123">Express de Visual Studio crea los siguientes archivos y carpetas:</span><span class="sxs-lookup"><span data-stu-id="08bb7-123">Visual Studio Express creates the following files and folders:</span></span>

- <span data-ttu-id="08bb7-124">*Un MoviesController.cs* archivo en el proyecto *controladores* carpeta.</span><span class="sxs-lookup"><span data-stu-id="08bb7-124">*A MoviesController.cs* file in the project's *Controllers* folder.</span></span>
- <span data-ttu-id="08bb7-125">A *películas* carpeta en el proyecto *vistas* carpeta.</span><span class="sxs-lookup"><span data-stu-id="08bb7-125">A *Movies* folder in the project's *Views* folder.</span></span>
- <span data-ttu-id="08bb7-126">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, y *Index.cshtml* en el nuevo *Views\Movies* carpeta.</span><span class="sxs-lookup"><span data-stu-id="08bb7-126">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="08bb7-127">ASP.NET MVC 4 crea automáticamente el CRUD (crear, leer, actualizar y eliminar) los métodos de acción y vistas por usted (la creación automática de los métodos de acción CRUD y vistas se conoce como scaffolding).</span><span class="sxs-lookup"><span data-stu-id="08bb7-127">ASP.NET MVC 4 automatically created the CRUD (create, read, update, and delete) action methods and views for you (the automatic creation of CRUD action methods and views is known as scaffolding).</span></span> <span data-ttu-id="08bb7-128">Ahora tiene una aplicación web totalmente funcional que le permite crear, enumerar, editar y eliminar las entradas de la película.</span><span class="sxs-lookup"><span data-stu-id="08bb7-128">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="08bb7-129">Ejecute la aplicación y busque el `Movies` controlador anexando */Movies* a la dirección URL en la barra de direcciones del explorador.</span><span class="sxs-lookup"><span data-stu-id="08bb7-129">Run the application and browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser.</span></span> <span data-ttu-id="08bb7-130">Dado que la aplicación es de confianza en la ruta predeterminada (definidos en el *Global.asax* archivo), la solicitud del explorador `http://localhost:xxxxx/Movies` se enruta hacia el valor predeterminado `Index` método de acción de la `Movies` controlador.</span><span class="sxs-lookup"><span data-stu-id="08bb7-130">Because the application is relying on the default routing (defined in the *Global.asax* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="08bb7-131">En otras palabras, la solicitud del explorador `http://localhost:xxxxx/Movies` es lo mismo que la solicitud de explorador `http://localhost:xxxxx/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="08bb7-131">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="08bb7-132">El resultado es una lista vacía de películas, porque no agregó ningún todavía.</span><span class="sxs-lookup"><span data-stu-id="08bb7-132">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a><span data-ttu-id="08bb7-133">Crear una película</span><span class="sxs-lookup"><span data-stu-id="08bb7-133">Creating a Movie</span></span>

<span data-ttu-id="08bb7-134">Seleccione el vínculo **Crear nuevo**.</span><span class="sxs-lookup"><span data-stu-id="08bb7-134">Select the **Create New** link.</span></span> <span data-ttu-id="08bb7-135">Escriba algunos detalles acerca de una película y, a continuación, haga clic en el **crear** botón.</span><span class="sxs-lookup"><span data-stu-id="08bb7-135">Enter some details about a movie and then click the **Create** button.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image3.png)

<span data-ttu-id="08bb7-136">Al hacer clic en el **crear** botón hace que el formulario se envía al servidor, donde se guarda la información de la película en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="08bb7-136">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="08bb7-137">A continuación, se le redirigirá a la */Movies* dirección URL, donde puede ver la película recién creada en la lista.</span><span class="sxs-lookup"><span data-stu-id="08bb7-137">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

<span data-ttu-id="08bb7-138">![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")</span><span class="sxs-lookup"><span data-stu-id="08bb7-138">![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")</span></span>

<span data-ttu-id="08bb7-139">Cree un par de entradas más de película.</span><span class="sxs-lookup"><span data-stu-id="08bb7-139">Create a couple more movie entries.</span></span> <span data-ttu-id="08bb7-140">Compruebe que todos los vínculos **Editar**, **Detalles** y **Eliminar** son funcionales.</span><span class="sxs-lookup"><span data-stu-id="08bb7-140">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="08bb7-141">Examinar el código generado</span><span class="sxs-lookup"><span data-stu-id="08bb7-141">Examining the Generated Code</span></span>

<span data-ttu-id="08bb7-142">Abra la *Controllers\MoviesController.cs* archivo y examine la generado `Index` método.</span><span class="sxs-lookup"><span data-stu-id="08bb7-142">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="08bb7-143">Una parte del controlador de película con la `Index` método se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="08bb7-143">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="08bb7-144">La siguiente línea de la `MoviesController` clase crea una instancia de un contexto de base de datos de la película, como se describió anteriormente.</span><span class="sxs-lookup"><span data-stu-id="08bb7-144">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="08bb7-145">Puede utilizar el contexto de base de datos de la película para consultar, modificar y eliminar películas.</span><span class="sxs-lookup"><span data-stu-id="08bb7-145">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

<span data-ttu-id="08bb7-146">Una solicitud para la `Movies` controlador devuelve todas las entradas de la `Movies` tabla de la base de datos de la película y, a continuación, envía los resultados a la `Index` vista.</span><span class="sxs-lookup"><span data-stu-id="08bb7-146">A request to the `Movies` controller returns all the entries in the `Movies` table of the movie database and then passes the results to the `Index` view.</span></span>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="08bb7-147">Establecimiento inflexible de tipos modelos y la @model (palabra clave)</span><span class="sxs-lookup"><span data-stu-id="08bb7-147">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="08bb7-148">Anteriormente en este tutorial, ha visto cómo un controlador puede pasar datos u objetos a una plantilla de vista mediante la `ViewBag` objeto.</span><span class="sxs-lookup"><span data-stu-id="08bb7-148">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="08bb7-149">El `ViewBag` es un objeto dinámico que proporciona una manera cómoda de tiempo de ejecución para pasar información a una vista.</span><span class="sxs-lookup"><span data-stu-id="08bb7-149">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="08bb7-150">ASP.NET MVC también proporciona la capacidad de pasar fuertemente tipados datos u objetos a una plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="08bb7-150">ASP.NET MVC also provides the ability to pass strongly typed data or objects to a view template.</span></span> <span data-ttu-id="08bb7-151">Esto fuertemente tipado enfoque permite un mejor tiempo de compilación la comprobación de su código y los transmite IntelliSense en el editor de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="08bb7-151">This strongly typed approach enables better compile-time checking of your code and richer IntelliSense in the Visual Studio editor.</span></span> <span data-ttu-id="08bb7-152">El mecanismo de scaffolding en Visual Studio usa este enfoque con el `MoviesController` plantillas de clase y la vista cuando crea los métodos y las vistas.</span><span class="sxs-lookup"><span data-stu-id="08bb7-152">The scaffolding mechanism in Visual Studio used this approach with the `MoviesController` class and view templates when it created the methods and views.</span></span>

<span data-ttu-id="08bb7-153">En el *Controllers\MoviesController.cs* archivo examinar generado `Details` método.</span><span class="sxs-lookup"><span data-stu-id="08bb7-153">In the *Controllers\MoviesController.cs* file examine the generated `Details` method.</span></span> <span data-ttu-id="08bb7-154">Una parte del controlador de película con la `Details` método se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="08bb7-154">A portion of the movie controller with the `Details` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

<span data-ttu-id="08bb7-155">Si un `Movie` se encuentra una instancia de la `Movie` modelo se pasa a la vista de detalles.</span><span class="sxs-lookup"><span data-stu-id="08bb7-155">If a `Movie` is found, an instance of the `Movie` model is passed to the Details view.</span></span> <span data-ttu-id="08bb7-156">Examine el contenido de la *Views\Movies\Details.cshtml* archivo.</span><span class="sxs-lookup"><span data-stu-id="08bb7-156">Examine the contents of the *Views\Movies\Details.cshtml* file.</span></span>

<span data-ttu-id="08bb7-157">Mediante la inclusión de un `@model` instrucción en la parte superior del archivo de plantilla de vista, puede especificar el tipo de objeto que espera de la vista.</span><span class="sxs-lookup"><span data-stu-id="08bb7-157">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="08bb7-158">Cuando se creó el controlador de película, Visual Studio incluyó automáticamente la siguiente instrucción `@model` en la parte superior del archivo *Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="08bb7-158">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

<span data-ttu-id="08bb7-159">Esta directiva `@model` permite acceder a la película que el controlador pasó a la vista usando un objeto `Model` fuertemente tipado.</span><span class="sxs-lookup"><span data-stu-id="08bb7-159">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="08bb7-160">Por ejemplo, en la *Details.cshtml* plantilla, el código pasa cada campo de la película a la `DisplayNameFor` y [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) aplicaciones auxiliares de HTML con fuertemente tipado `Model` objeto.</span><span class="sxs-lookup"><span data-stu-id="08bb7-160">For example, in the *Details.cshtml* template, the code passes each movie field to the `DisplayNameFor` and [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="08bb7-161">Las plantillas de vista y los métodos de creación y edición de pasan un objeto de modelo de la película.</span><span class="sxs-lookup"><span data-stu-id="08bb7-161">The Create and Edit methods and view templates also pass a movie model object.</span></span>

<span data-ttu-id="08bb7-162">Examine la *Index.cshtml* plantilla de vista y el `Index` método en el *MoviesController.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="08bb7-162">Examine the *Index.cshtml* view template and the `Index` method in the *MoviesController.cs* file.</span></span> <span data-ttu-id="08bb7-163">Observe cómo el código crea un [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) objeto cuando se llama a la `View` método auxiliar en el `Index` método de acción.</span><span class="sxs-lookup"><span data-stu-id="08bb7-163">Notice how the code creates a [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="08bb7-164">A continuación, el código pasa esto `Movies` lista desde el controlador a la vista:</span><span class="sxs-lookup"><span data-stu-id="08bb7-164">The code then passes this `Movies` list from the controller to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

<span data-ttu-id="08bb7-165">Al crear el controlador de película, Visual Studio Express incluye automáticamente los siguientes `@model` instrucción en la parte superior de la *Index.cshtml* archivo:</span><span class="sxs-lookup"><span data-stu-id="08bb7-165">When you created the movie controller, Visual Studio Express automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

<span data-ttu-id="08bb7-166">Esto `@model` directiva permite obtener acceso a la lista de películas que el controlador pasa a la vista mediante un `Model` objeto fuertemente tipado.</span><span class="sxs-lookup"><span data-stu-id="08bb7-166">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="08bb7-167">Por ejemplo, en la *Index.cshtml* plantilla, el código recorre las películas de manera práctica un `foreach` instrucción sobre fuertemente tipado `Model` objeto:</span><span class="sxs-lookup"><span data-stu-id="08bb7-167">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

<span data-ttu-id="08bb7-168">Dado que la `Model` objeto está fuertemente tipado (como un `IEnumerable<Movie>` objeto), cada `item` objeto en el bucle se escribe como `Movie`.</span><span class="sxs-lookup"><span data-stu-id="08bb7-168">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="08bb7-169">Entre otras ventajas, esto significa que obtiene la comprobación en tiempo de compilación del código y compatibilidad con IntelliSense en el editor de código completa:</span><span class="sxs-lookup"><span data-stu-id="08bb7-169">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a><span data-ttu-id="08bb7-171">Trabajar con SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="08bb7-171">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="08bb7-172">Entity Framework Code First detectó que la cadena de conexión de base de datos que se proporcionó que apunta a un `Movies` base de datos que no existe todavía, por lo que Code First crea la base de datos automáticamente.</span><span class="sxs-lookup"><span data-stu-id="08bb7-172">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="08bb7-173">Puede comprobar que se ha creado mediante una búsqueda en el *aplicación\_datos* carpeta.</span><span class="sxs-lookup"><span data-stu-id="08bb7-173">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="08bb7-174">Si no ve el *Movies.mdf* de archivos, haga clic en el **mostrar todos los archivos** botón en el **el Explorador de soluciones** barra de herramientas, haga clic en el **actualizar** botón y, a continuación, expanda el *aplicación\_datos* carpeta.</span><span class="sxs-lookup"><span data-stu-id="08bb7-174">If you don't see the *Movies.mdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="08bb7-175">Haga doble clic en *Movies.mdf* para abrir **el Explorador de base de datos**, a continuación, expanda el **tablas** carpeta para ver la tabla de películas.</span><span class="sxs-lookup"><span data-stu-id="08bb7-175">Double-click *Movies.mdf* to open **DATABASE EXPLORER**, then expand the **Tables** folder to see the Movies table.</span></span>

<span data-ttu-id="08bb7-176">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")</span><span class="sxs-lookup"><span data-stu-id="08bb7-176">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")</span></span>

> [!NOTE]
> <span data-ttu-id="08bb7-177">Si no aparece el Explorador de base de datos, desde el **herramientas** menú, seleccione **conectar con base de datos**, a continuación, cancelar la **Elegir origen de datos** cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="08bb7-177">If the database explorer doesn't appear, from the **TOOLS** menu, select **Connect to Database**, then cancel the **Choose Data Source** dialog.</span></span> <span data-ttu-id="08bb7-178">Esto forzará abra el Explorador de base de datos.</span><span class="sxs-lookup"><span data-stu-id="08bb7-178">This will force open the database explorer.</span></span>


> [!NOTE]
> <span data-ttu-id="08bb7-179">Si está utilizando VWD o Visual Studio 2010 y recibe un error similar a alguno de los siguientes valores siguientes:</span><span class="sxs-lookup"><span data-stu-id="08bb7-179">If you are using VWD or Visual Studio 2010 and get an error similar to any of the following following:</span></span>
> 
> - <span data-ttu-id="08bb7-180">La base de datos ' C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES. MDF' no se puede abrir porque está versión 706.</span><span class="sxs-lookup"><span data-stu-id="08bb7-180">The database 'C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES.MDF' cannot be opened because it is version 706.</span></span> <span data-ttu-id="08bb7-181">Este servidor es compatible con la versión 655 y versiones anterior.</span><span class="sxs-lookup"><span data-stu-id="08bb7-181">This server supports version 655 and earlier.</span></span> <span data-ttu-id="08bb7-182">No se admite la ruta de actualización.</span><span class="sxs-lookup"><span data-stu-id="08bb7-182">A downgrade path is not supported.</span></span>
> - <span data-ttu-id="08bb7-183">&quot;Excepción de Objectnotfound no estaba controlada por código de usuario&quot; la SqlConnection proporcionada no especifica ningún catálogo inicial.</span><span class="sxs-lookup"><span data-stu-id="08bb7-183">&quot;InvalidOperation Exception was unhandled by user code&quot; The supplied SqlConnection does not specify an initial catalog.</span></span>
> 
> <span data-ttu-id="08bb7-184">Debe instalar la [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) y [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0).</span><span class="sxs-lookup"><span data-stu-id="08bb7-184">You need to install the [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) and [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0).</span></span> <span data-ttu-id="08bb7-185">Compruebe el `MovieDBContext` cadena de conexión especificada en la página anterior.</span><span class="sxs-lookup"><span data-stu-id="08bb7-185">Verify the `MovieDBContext` connection string specified on the previous page.</span></span>


<span data-ttu-id="08bb7-186">Haga clic en el `Movies` de tabla y seleccione **mostrar datos de tabla** para ver los datos que creó.</span><span class="sxs-lookup"><span data-stu-id="08bb7-186">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image8.png)

<span data-ttu-id="08bb7-187">Haga clic en el `Movies` de tabla y seleccione **Abrir definición de tabla** para ver la tabla de la estructura que Entity Framework Code First creado para usted.</span><span class="sxs-lookup"><span data-stu-id="08bb7-187">Right-click the `Movies` table and select **Open Table Definition** to see the table structure that Entity Framework Code First created for you.</span></span>

<span data-ttu-id="08bb7-188">![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")</span><span class="sxs-lookup"><span data-stu-id="08bb7-188">![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")</span></span>

![](accessing-your-models-data-from-a-controller/_static/image10.png)

<span data-ttu-id="08bb7-189">Observe cómo el esquema de la `Movies` tabla se asigna a la `Movie` clase que creó anteriormente.</span><span class="sxs-lookup"><span data-stu-id="08bb7-189">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="08bb7-190">Entity Framework Code First crea automáticamente este esquema para el se basa en su `Movie` clase.</span><span class="sxs-lookup"><span data-stu-id="08bb7-190">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="08bb7-191">Cuando haya terminado, cierre la conexión haciendo clic *MovieDBContext* y seleccionando **cerrar conexión**.</span><span class="sxs-lookup"><span data-stu-id="08bb7-191">When you're finished, close the connection by right clicking *MovieDBContext* and selecting **Close Connection**.</span></span> <span data-ttu-id="08bb7-192">(Si no cierra la conexión, se podría obtener un error la próxima vez que ejecute el proyecto).</span><span class="sxs-lookup"><span data-stu-id="08bb7-192">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

<span data-ttu-id="08bb7-193">![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")</span><span class="sxs-lookup"><span data-stu-id="08bb7-193">![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")</span></span>

<span data-ttu-id="08bb7-194">Ahora tiene la base de datos y una página de lista simple para mostrar el contenido del mismo.</span><span class="sxs-lookup"><span data-stu-id="08bb7-194">You now have the database and a simple listing page to display content from it.</span></span> <span data-ttu-id="08bb7-195">En el siguiente tutorial, podrá examinar el resto del código con scaffolding y agregue un `SearchIndex` método y un `SearchIndex` vista que le permite buscar películas en esta base de datos.</span><span class="sxs-lookup"><span data-stu-id="08bb7-195">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="08bb7-196">[Anterior](adding-a-model.md)
> [Siguiente](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="08bb7-196">[Previous](adding-a-model.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
