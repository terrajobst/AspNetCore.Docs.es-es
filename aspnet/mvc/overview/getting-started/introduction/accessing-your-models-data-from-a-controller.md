---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Obtiene acceso a datos de su modelo desde un controlador | Documentos de Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: d3dfa079c334e04f368531456ec2ec4e9728f893
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="47abd-102">Obtiene acceso a datos de su modelo desde un controlador</span><span class="sxs-lookup"><span data-stu-id="47abd-102">Accessing Your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="47abd-103">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="47abd-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="47abd-104">En esta sección, creará un nuevo `MoviesController` clase y escribir código que recupera los datos de la película y lo muestra en el explorador utilizando una plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="47abd-104">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span>

<span data-ttu-id="47abd-105">**Compile la aplicación** antes de pasar al paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="47abd-105">**Build the application** before going on to the next step.</span></span> <span data-ttu-id="47abd-106">Si no compila la aplicación, obtendrá un error al agregar un controlador.</span><span class="sxs-lookup"><span data-stu-id="47abd-106">If you don't build the application, you'll get an error adding a controller.</span></span>

<span data-ttu-id="47abd-107">En el Explorador de soluciones, haga clic en el *controladores* carpeta y, a continuación, haga clic en **agregar**, a continuación, **controlador**.</span><span class="sxs-lookup"><span data-stu-id="47abd-107">In Solution Explorer, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image1.png)

<span data-ttu-id="47abd-108">En el **agregar scaffolding** cuadro de diálogo, haga clic en **controlador de MVC 5 con vistas, mediante Entity Framework**y, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="47abd-108">In the **Add Scaffold** dialog box, click **MVC 5 Controller with views, using Entity Framework**, and then click **Add**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- <span data-ttu-id="47abd-109">Seleccione **película (MvcMovie.Models)** para la clase del modelo.</span><span class="sxs-lookup"><span data-stu-id="47abd-109">Select **Movie (MvcMovie.Models)** for the Model class.</span></span>
- <span data-ttu-id="47abd-110">Seleccione **MovieDBContext (MvcMovie.Models)** para la clase de contexto de datos.</span><span class="sxs-lookup"><span data-stu-id="47abd-110">Select **MovieDBContext (MvcMovie.Models)** for the Data context class.</span></span>
- <span data-ttu-id="47abd-111">Para el nombre del controlador, escriba **MoviesController**.</span><span class="sxs-lookup"><span data-stu-id="47abd-111">For the Controller name enter **MoviesController**.</span></span>

  <span data-ttu-id="47abd-112">La imagen siguiente muestra el cuadro de diálogo completada.</span><span class="sxs-lookup"><span data-stu-id="47abd-112">The image below shows the completed dialog.</span></span>  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

<span data-ttu-id="47abd-113">Haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="47abd-113">Click **Add**.</span></span> <span data-ttu-id="47abd-114">(Si se produce un error, probablemente no compile la aplicación antes de comenzar a agregar el controlador.) Visual Studio crea los siguientes archivos y carpetas:</span><span class="sxs-lookup"><span data-stu-id="47abd-114">(If you get an error, you probably didn't build the application before starting adding the controller.) Visual Studio creates the following files and folders:</span></span>

- <span data-ttu-id="47abd-115">*Un MoviesController.cs* un archivo en el *controladores* carpeta.</span><span class="sxs-lookup"><span data-stu-id="47abd-115">*A MoviesController.cs* file in the *Controllers* folder.</span></span>
- <span data-ttu-id="47abd-116">A *Views\Movies* carpeta.</span><span class="sxs-lookup"><span data-stu-id="47abd-116">A *Views\Movies* folder.</span></span>
- <span data-ttu-id="47abd-117">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, y *Index.cshtml* en el nuevo *Views\Movies* carpeta.</span><span class="sxs-lookup"><span data-stu-id="47abd-117">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="47abd-118">Visual Studio creada automáticamente la [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (crear, leer, actualizar y eliminar) los métodos de acción y vistas por usted (la creación automática de los métodos de acción CRUD y vistas se conoce como scaffolding).</span><span class="sxs-lookup"><span data-stu-id="47abd-118">Visual Studio automatically created the [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views for you (the automatic creation of CRUD action methods and views is known as scaffolding).</span></span> <span data-ttu-id="47abd-119">Ahora tiene una aplicación web totalmente funcional que le permite crear, enumerar, editar y eliminar las entradas de la película.</span><span class="sxs-lookup"><span data-stu-id="47abd-119">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="47abd-120">Ejecute la aplicación y haga clic en el **MVC película** vínculo (o vaya a la `Movies` controlador anexando */Movies* a la dirección URL en la barra de direcciones del explorador).</span><span class="sxs-lookup"><span data-stu-id="47abd-120">Run the application and click on the **MVC Movie** link (or browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser).</span></span> <span data-ttu-id="47abd-121">Dado que la aplicación es de confianza en la ruta predeterminada (definidos en el *aplicación\_Start\RouteConfig.cs* archivo), la solicitud del explorador `http://localhost:xxxxx/Movies` se enruta hacia el valor predeterminado `Index` método de acción de la `Movies` controlador.</span><span class="sxs-lookup"><span data-stu-id="47abd-121">Because the application is relying on the default routing (defined in the *App\_Start\RouteConfig.cs* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="47abd-122">En otras palabras, la solicitud del explorador `http://localhost:xxxxx/Movies` es lo mismo que la solicitud de explorador `http://localhost:xxxxx/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="47abd-122">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="47abd-123">El resultado es una lista vacía de películas, porque no agregó ningún todavía.</span><span class="sxs-lookup"><span data-stu-id="47abd-123">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a><span data-ttu-id="47abd-124">Crear una película</span><span class="sxs-lookup"><span data-stu-id="47abd-124">Creating a Movie</span></span>

<span data-ttu-id="47abd-125">Seleccione el vínculo **Crear nuevo**.</span><span class="sxs-lookup"><span data-stu-id="47abd-125">Select the **Create New** link.</span></span> <span data-ttu-id="47abd-126">Escriba algunos detalles acerca de una película y, a continuación, haga clic en el **crear** botón.</span><span class="sxs-lookup"><span data-stu-id="47abd-126">Enter some details about a movie and then click the **Create** button.</span></span>


![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="47abd-127">Es podrán que no pueda escribir decimales ni comas en el campo Precio.</span><span class="sxs-lookup"><span data-stu-id="47abd-127">You may not be able to enter decimal points or commas in the Price field.</span></span> <span data-ttu-id="47abd-128">para admitir la validación de jQuery para configuraciones regionales no inglesas que usan una coma (&quot;,&quot;) para obtener un separador decimal y formatos de fecha no es inglés de Estados Unidos, debe incluir *globalize.js* y específica de su  *Cultures/globalize.Cultures.js* archivo (de [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) y JavaScript para usar `Globalize.parseFloat`.</span><span class="sxs-lookup"><span data-stu-id="47abd-128">To support jQuery validation for non-English locales that use a comma (&quot;,&quot;) for a decimal point, and non US-English date formats, you must include *globalize.js* and your specific *cultures/globalize.cultures.js* file(from [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) and JavaScript to use `Globalize.parseFloat`.</span></span> <span data-ttu-id="47abd-129">Voy a explicar cómo hacerlo en el siguiente tutorial.</span><span class="sxs-lookup"><span data-stu-id="47abd-129">I'll show how to do this in the next tutorial.</span></span> <span data-ttu-id="47abd-130">Por ahora, escriba solamente números enteros como 10.</span><span class="sxs-lookup"><span data-stu-id="47abd-130">For now, just enter whole numbers like 10.</span></span>


<span data-ttu-id="47abd-131">Al hacer clic en el **crear** botón hace que el formulario se envía al servidor, donde se guarda la información de la película en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="47abd-131">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="47abd-132">A continuación, se le redirigirá a la */Movies* dirección URL, donde puede ver la película recién creada en la lista.</span><span class="sxs-lookup"><span data-stu-id="47abd-132">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="47abd-133">Cree un par de entradas más de película.</span><span class="sxs-lookup"><span data-stu-id="47abd-133">Create a couple more movie entries.</span></span> <span data-ttu-id="47abd-134">Compruebe que todos los vínculos **Editar**, **Detalles** y **Eliminar** son funcionales.</span><span class="sxs-lookup"><span data-stu-id="47abd-134">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="47abd-135">Examinar el código generado</span><span class="sxs-lookup"><span data-stu-id="47abd-135">Examining the Generated Code</span></span>

<span data-ttu-id="47abd-136">Abra la *Controllers\MoviesController.cs* archivo y examine la generado `Index` método.</span><span class="sxs-lookup"><span data-stu-id="47abd-136">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="47abd-137">Una parte del controlador de película con la `Index` método se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="47abd-137">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="47abd-138">Una solicitud para la `Movies` controlador devuelve todas las entradas de la `Movies` de tabla y, a continuación, envía los resultados a la `Index` vista.</span><span class="sxs-lookup"><span data-stu-id="47abd-138">A request to the `Movies` controller returns all the entries in the `Movies` table and then passes the results to the `Index` view.</span></span> <span data-ttu-id="47abd-139">La siguiente línea de la `MoviesController` clase crea una instancia de un contexto de base de datos de la película, como se describió anteriormente.</span><span class="sxs-lookup"><span data-stu-id="47abd-139">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="47abd-140">Puede utilizar el contexto de base de datos de la película para consultar, modificar y eliminar películas.</span><span class="sxs-lookup"><span data-stu-id="47abd-140">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="47abd-141">Establecimiento inflexible de tipos modelos y la @model (palabra clave)</span><span class="sxs-lookup"><span data-stu-id="47abd-141">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="47abd-142">Anteriormente en este tutorial, ha visto cómo un controlador puede pasar datos u objetos a una plantilla de vista mediante la `ViewBag` objeto.</span><span class="sxs-lookup"><span data-stu-id="47abd-142">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="47abd-143">El `ViewBag` es un objeto dinámico que proporciona una manera cómoda de tiempo de ejecución para pasar información a una vista.</span><span class="sxs-lookup"><span data-stu-id="47abd-143">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="47abd-144">MVC también proporciona la capacidad de pasar *fuertemente* con tipo de objetos a una plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="47abd-144">MVC also provides the ability to pass *strongly* typed objects to a view template.</span></span> <span data-ttu-id="47abd-145">Este enfoque fuertemente tipado permite un mejor tiempo de compilación más enriquecida y comprobación del código [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) en el editor de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="47abd-145">This strongly typed approach enables better compile-time checking of your code and richer [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) in the Visual Studio editor.</span></span> <span data-ttu-id="47abd-146">El mecanismo de scaffolding en Visual Studio usa este enfoque (es decir, pasando una *fuertemente* modelo con tipo) con el `MoviesController` plantillas de clase y la vista cuando crea los métodos y las vistas.</span><span class="sxs-lookup"><span data-stu-id="47abd-146">The scaffolding mechanism in Visual Studio used this approach (that is, passing a *strongly* typed model) with the `MoviesController` class and view templates when it created the methods and views.</span></span>

<span data-ttu-id="47abd-147">En el *Controllers\MoviesController.cs* archivo examinar generado `Details` método.</span><span class="sxs-lookup"><span data-stu-id="47abd-147">In the *Controllers\MoviesController.cs* file examine the generated `Details` method.</span></span> <span data-ttu-id="47abd-148">El `Details` método se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="47abd-148">The `Details` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

<span data-ttu-id="47abd-149">El `id` parámetro suele pasarse como datos de ruta, por ejemplo `http://localhost:1234/movies/details/1` establecerá el controlador para el controlador de película, la acción que `details` y `id` en 1.</span><span class="sxs-lookup"><span data-stu-id="47abd-149">The `id` parameter is generally passed as route data, for example `http://localhost:1234/movies/details/1` will set the controller to the movie controller, the action to `details` and the `id` to 1.</span></span> <span data-ttu-id="47abd-150">También podría pasar en el identificador con una cadena de consulta como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="47abd-150">You could also pass in the id with a query string as follows:</span></span>

`http://localhost:1234/movies/details?id=1`

<span data-ttu-id="47abd-151">Si un `Movie` se encuentra una instancia de la `Movie` modelo se pasa a la `Details` vista:</span><span class="sxs-lookup"><span data-stu-id="47abd-151">If a `Movie` is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

<span data-ttu-id="47abd-152">Examine el contenido de la *Views\Movies\Details.cshtml* archivo:</span><span class="sxs-lookup"><span data-stu-id="47abd-152">Examine the contents of the *Views\Movies\Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

<span data-ttu-id="47abd-153">Mediante la inclusión de un `@model` instrucción en la parte superior del archivo de plantilla de vista, puede especificar el tipo de objeto que espera de la vista.</span><span class="sxs-lookup"><span data-stu-id="47abd-153">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="47abd-154">Cuando se creó el controlador de película, Visual Studio incluyó automáticamente la siguiente instrucción `@model` en la parte superior del archivo *Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="47abd-154">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

<span data-ttu-id="47abd-155">Esta directiva `@model` permite acceder a la película que el controlador pasó a la vista usando un objeto `Model` fuertemente tipado.</span><span class="sxs-lookup"><span data-stu-id="47abd-155">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="47abd-156">Por ejemplo, en la *Details.cshtml* plantilla, el código pasa cada campo de la película a la `DisplayNameFor` y [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) aplicaciones auxiliares de HTML con fuertemente tipado `Model` objeto.</span><span class="sxs-lookup"><span data-stu-id="47abd-156">For example, in the *Details.cshtml* template, the code passes each movie field to the `DisplayNameFor` and [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="47abd-157">El `Create` y `Edit` métodos y las plantillas de vista pasan un objeto de modelo de la película.</span><span class="sxs-lookup"><span data-stu-id="47abd-157">The `Create` and `Edit` methods and view templates also pass a movie model object.</span></span>

<span data-ttu-id="47abd-158">Examine la *Index.cshtml* plantilla de vista y el `Index` método en el *MoviesController.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="47abd-158">Examine the *Index.cshtml* view template and the `Index` method in the *MoviesController.cs* file.</span></span> <span data-ttu-id="47abd-159">Observe cómo el código crea un [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) objeto cuando se llama a la `View` método auxiliar en el `Index` método de acción.</span><span class="sxs-lookup"><span data-stu-id="47abd-159">Notice how the code creates a [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="47abd-160">A continuación, el código pasa esto `Movies` lista desde el `Index` método de acción a la vista:</span><span class="sxs-lookup"><span data-stu-id="47abd-160">The code then passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

<span data-ttu-id="47abd-161">Al crear el controlador de película, Visual Studio incluye automáticamente los siguientes `@model` instrucción en la parte superior de la *Index.cshtml* archivo:</span><span class="sxs-lookup"><span data-stu-id="47abd-161">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

<span data-ttu-id="47abd-162">Esto `@model` directiva permite obtener acceso a la lista de películas que el controlador pasa a la vista mediante un `Model` objeto fuertemente tipado.</span><span class="sxs-lookup"><span data-stu-id="47abd-162">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="47abd-163">Por ejemplo, en la *Index.cshtml* plantilla, el código recorre las películas de manera práctica un `foreach` instrucción sobre fuertemente tipado `Model` objeto:</span><span class="sxs-lookup"><span data-stu-id="47abd-163">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

<span data-ttu-id="47abd-164">Dado que la `Model` objeto está fuertemente tipado (como un `IEnumerable<Movie>` objeto), cada `item` objeto en el bucle se escribe como `Movie`.</span><span class="sxs-lookup"><span data-stu-id="47abd-164">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="47abd-165">Entre otras ventajas, esto significa que obtiene la comprobación en tiempo de compilación del código y compatibilidad con IntelliSense en el editor de código completa:</span><span class="sxs-lookup"><span data-stu-id="47abd-165">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a><span data-ttu-id="47abd-167">Trabajar con SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="47abd-167">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="47abd-168">Entity Framework Code First detectó que la cadena de conexión de base de datos que se proporcionó que apunta a un `Movies` base de datos que no existe todavía, por lo que Code First crea la base de datos automáticamente.</span><span class="sxs-lookup"><span data-stu-id="47abd-168">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="47abd-169">Puede comprobar que se ha creado mediante una búsqueda en el *aplicación\_datos* carpeta.</span><span class="sxs-lookup"><span data-stu-id="47abd-169">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="47abd-170">Si no ve el *Movies.mdf* de archivos, haga clic en el **mostrar todos los archivos** botón en el **el Explorador de soluciones** barra de herramientas, haga clic en el **actualizar** botón y, a continuación, expanda el *aplicación\_datos* carpeta.</span><span class="sxs-lookup"><span data-stu-id="47abd-170">If you don't see the *Movies.mdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image9.png)

<span data-ttu-id="47abd-171">Haga doble clic en *Movies.mdf* para abrir **Explorador de servidores**, a continuación, expanda el **tablas** carpeta para ver la tabla de películas.</span><span class="sxs-lookup"><span data-stu-id="47abd-171">Double-click *Movies.mdf* to open **SERVER EXPLORER**, then expand the **Tables** folder to see the Movies table.</span></span> <span data-ttu-id="47abd-172">Tenga en cuenta el icono de llave junto al identificador.</span><span class="sxs-lookup"><span data-stu-id="47abd-172">Note the key icon next to ID.</span></span> <span data-ttu-id="47abd-173">De forma predeterminada, EF hará que una propiedad denominada Id. de la clave principal.</span><span class="sxs-lookup"><span data-stu-id="47abd-173">By default, EF will make a property named ID the primary key.</span></span> <span data-ttu-id="47abd-174">Para obtener más información sobre EF y MVC, vea el tutorial excelente de Tom Dykstra en [MVC y EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="47abd-174">For more information on EF and MVC, see Tom Dykstra's excellent tutorial on [MVC and EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="47abd-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span><span class="sxs-lookup"><span data-stu-id="47abd-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span></span>

<span data-ttu-id="47abd-176">Haga clic en el `Movies` de tabla y seleccione **mostrar datos de tabla** para ver los datos que creó.</span><span class="sxs-lookup"><span data-stu-id="47abd-176">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

<span data-ttu-id="47abd-177">Haga clic en el `Movies` de tabla y seleccione **Abrir definición de tabla** para ver la tabla de la estructura que Entity Framework Code First creado para usted.</span><span class="sxs-lookup"><span data-stu-id="47abd-177">Right-click the `Movies` table and select **Open Table Definition** to see the table structure that Entity Framework Code First created for you.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

<span data-ttu-id="47abd-178">Observe cómo el esquema de la `Movies` tabla se asigna a la `Movie` clase que creó anteriormente.</span><span class="sxs-lookup"><span data-stu-id="47abd-178">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="47abd-179">Entity Framework Code First crea automáticamente este esquema para el se basa en su `Movie` clase.</span><span class="sxs-lookup"><span data-stu-id="47abd-179">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="47abd-180">Cuando haya terminado, cierre la conexión haciendo clic *MovieDBContext* y seleccionando **cerrar conexión**.</span><span class="sxs-lookup"><span data-stu-id="47abd-180">When you're finished, close the connection by right clicking *MovieDBContext* and selecting **Close Connection**.</span></span> <span data-ttu-id="47abd-181">(Si no cierra la conexión, se podría obtener un error la próxima vez que ejecute el proyecto).</span><span class="sxs-lookup"><span data-stu-id="47abd-181">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

<span data-ttu-id="47abd-182">![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")</span><span class="sxs-lookup"><span data-stu-id="47abd-182">![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")</span></span>

<span data-ttu-id="47abd-183">Ahora dispone de una base de datos y de páginas para mostrar, editar, actualizar y eliminar datos.</span><span class="sxs-lookup"><span data-stu-id="47abd-183">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="47abd-184">En el siguiente tutorial, podrá examinar el resto del código con scaffolding y agregue un `SearchIndex` método y un `SearchIndex` vista que le permite buscar películas en esta base de datos.</span><span class="sxs-lookup"><span data-stu-id="47abd-184">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span> <span data-ttu-id="47abd-185">Para obtener más información sobre el uso de Entity Framework con MVC, consulte [crear un modelo de datos de Entity Framework para una aplicación de MVC de ASP.NET](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="47abd-185">For more information on using Entity Framework with MVC, see [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="47abd-186">[Anterior](creating-a-connection-string.md)
> [Siguiente](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="47abd-186">[Previous](creating-a-connection-string.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
