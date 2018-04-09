---
uid: mvc/overview/getting-started/introduction/adding-search
title: Búsqueda | Documentos de Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: 8afa72d4dbc4695e7d26c6ef4052be08a7c69080
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="search"></a><span data-ttu-id="0e28f-102">Buscar</span><span class="sxs-lookup"><span data-stu-id="0e28f-102">Search</span></span>
====================
<span data-ttu-id="0e28f-103">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="0e28f-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="adding-a-search-method-and-search-view"></a><span data-ttu-id="0e28f-104">Agregar un método de búsqueda y la vista de búsqueda</span><span class="sxs-lookup"><span data-stu-id="0e28f-104">Adding a Search Method and Search View</span></span>

<span data-ttu-id="0e28f-105">En esta sección agregará capacidad de búsqueda para el `Index` método de acción que le permite buscar películas por género o por nombre.</span><span class="sxs-lookup"><span data-stu-id="0e28f-105">In this section you'll add search capability to the `Index` action method that lets you search movies by genre or name.</span></span>

## <a name="updating-the-index-form"></a><span data-ttu-id="0e28f-106">Actualizar el formulario de índice</span><span class="sxs-lookup"><span data-stu-id="0e28f-106">Updating the Index Form</span></span>

<span data-ttu-id="0e28f-107">Inicie actualizando el `Index` método de acción a la existente `MoviesController` clase.</span><span class="sxs-lookup"><span data-stu-id="0e28f-107">Start by updating the `Index` action method to the existing `MoviesController` class.</span></span> <span data-ttu-id="0e28f-108">Este es el código:</span><span class="sxs-lookup"><span data-stu-id="0e28f-108">Here's the code:</span></span>

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

<span data-ttu-id="0e28f-109">La primera línea de la `Index` método crea las siguientes [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) consulta para seleccionar las películas:</span><span class="sxs-lookup"><span data-stu-id="0e28f-109">The first line of the `Index` method creates the following [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) query to select the movies:</span></span>

[!code-csharp[Main](adding-search/samples/sample2.cs)]

<span data-ttu-id="0e28f-110">La consulta se define en este momento, pero aún no se ha ejecutado en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0e28f-110">The query is defined at this point, but hasn't yet been run against the database.</span></span>

<span data-ttu-id="0e28f-111">Si el `searchString` parámetro contiene una cadena, se modificará la consulta de películas para filtrar según el valor de la cadena de búsqueda, utilizando el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="0e28f-111">If the `searchString` parameter contains a string, the movies query is modified to filter on the value of the search string, using the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample3.cs)]

<span data-ttu-id="0e28f-112">El código `s => s.Title` anterior es una [expresión Lambda](https://msdn.microsoft.com/library/bb397687.aspx).</span><span class="sxs-lookup"><span data-stu-id="0e28f-112">The `s => s.Title` code above is a [Lambda Expression](https://msdn.microsoft.com/library/bb397687.aspx).</span></span> <span data-ttu-id="0e28f-113">Las lambdas se usan en basadas en métodos [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) consultas como argumentos para los métodos de operador de consulta estándar, como el [donde](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) método usado en el código anterior.</span><span class="sxs-lookup"><span data-stu-id="0e28f-113">Lambdas are used in method-based [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) queries as arguments to standard query operator methods such as the [Where](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) method used in the above code.</span></span> <span data-ttu-id="0e28f-114">Las consultas LINQ no se ejecutan cuando se definen o cuando se modifican mediante una llamada a un método como `Where` o `OrderBy`.</span><span class="sxs-lookup"><span data-stu-id="0e28f-114">LINQ queries are not executed when they are defined or when they are modified by calling a method such as `Where` or `OrderBy`.</span></span> <span data-ttu-id="0e28f-115">En su lugar, se aplaza la ejecución de la consulta, lo que significa que la evaluación de una expresión se retrasa hasta que su valor producida realmente se recorre en iteración o el [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) se llama al método.</span><span class="sxs-lookup"><span data-stu-id="0e28f-115">Instead, query execution is deferred, which means that the evaluation of an expression is delayed until its realized value is actually iterated over or the [`ToList`](https://msdn.microsoft.com/library/bb342261.aspx) method is called.</span></span> <span data-ttu-id="0e28f-116">En el `Search` ejemplo, la consulta se ejecuta en el *Index.cshtml* vista.</span><span class="sxs-lookup"><span data-stu-id="0e28f-116">In the `Search` sample, the query is executed in the *Index.cshtml* view.</span></span> <span data-ttu-id="0e28f-117">Para más información sobre la ejecución de consultas en diferido, vea [Ejecución de la consulta](https://msdn.microsoft.com/library/bb738633.aspx).</span><span class="sxs-lookup"><span data-stu-id="0e28f-117">For more information about deferred query execution, see [Query Execution](https://msdn.microsoft.com/library/bb738633.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="0e28f-118">El [Contains](https://msdn.microsoft.com/library/bb155125.aspx) el método se ejecuta en la base de datos, no el código c# anterior.</span><span class="sxs-lookup"><span data-stu-id="0e28f-118">The [Contains](https://msdn.microsoft.com/library/bb155125.aspx) method is run on the database, not the c# code above.</span></span> <span data-ttu-id="0e28f-119">En la base de datos, [Contains](https://msdn.microsoft.com/library/bb155125.aspx) se asigna a [SQL como](https://msdn.microsoft.com/library/ms179859.aspx), que distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="0e28f-119">On the database, [Contains](https://msdn.microsoft.com/library/bb155125.aspx) maps to [SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx), which is case insensitive.</span></span>

<span data-ttu-id="0e28f-120">Ahora puede actualizar el `Index` vista que se mostrará el formulario al usuario.</span><span class="sxs-lookup"><span data-stu-id="0e28f-120">Now you can update the `Index` view that will display the form to the user.</span></span>

<span data-ttu-id="0e28f-121">Ejecute la aplicación y vaya a */películas/índice*.</span><span class="sxs-lookup"><span data-stu-id="0e28f-121">Run the application and navigate to */Movies/Index*.</span></span> <span data-ttu-id="0e28f-122">Anexe una cadena de consulta como `?searchString=ghost` a la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="0e28f-122">Append a query string such as `?searchString=ghost` to the URL.</span></span> <span data-ttu-id="0e28f-123">Se muestran las películas filtradas.</span><span class="sxs-lookup"><span data-stu-id="0e28f-123">The filtered movies are displayed.</span></span>

![SearchQryStr](adding-search/_static/image1.png)

<span data-ttu-id="0e28f-125">Si cambia la firma de la `Index` método que tiene un parámetro denominado `id`, el `id` parámetro coincidirá la `{id}` enruta el marcador de posición para el valor predeterminado establecido en el *aplicación\_Start RouteConfig.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="0e28f-125">If you change the signature of the `Index` method to have a parameter named `id`, the `id` parameter will match the `{id}` placeholder for the default routes set in the *App\_Start\RouteConfig.cs* file.</span></span>

[!code-json[Main](adding-search/samples/sample4.json)]

<span data-ttu-id="0e28f-126">La versión original `Index` método tiene el siguiente aspecto::</span><span class="sxs-lookup"><span data-stu-id="0e28f-126">The original `Index` method looks like this::</span></span>

[!code-csharp[Main](adding-search/samples/sample5.cs)]

<span data-ttu-id="0e28f-127">Modificados `Index` método tendría el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="0e28f-127">The modified `Index` method would look as follows:</span></span>

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

<span data-ttu-id="0e28f-128">Ahora puede pasar el título de la búsqueda como datos de ruta (un segmento de dirección URL) en lugar de como un valor de cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="0e28f-128">You can now pass the search title as route data (a URL segment) instead of as a query string value.</span></span>

![](adding-search/_static/image2.png)

<span data-ttu-id="0e28f-129">Sin embargo, no se puede esperar que los usuarios modifiquen la dirección URL cada vez que quieran buscar una película.</span><span class="sxs-lookup"><span data-stu-id="0e28f-129">However, you can't expect users to modify the URL every time they want to search for a movie.</span></span> <span data-ttu-id="0e28f-130">Por lo que ahora se agregará la interfaz de usuario para ayudar a ellos filtrar películas.</span><span class="sxs-lookup"><span data-stu-id="0e28f-130">So now you you'll add UI to help them filter movies.</span></span> <span data-ttu-id="0e28f-131">Si cambia la firma de la `Index` cambiarlo de método para probar cómo pasar el parámetro de identificador límite de ruta, modo que su `Index` método toma un parámetro de cadena denominado `searchString`:</span><span class="sxs-lookup"><span data-stu-id="0e28f-131">If you changed the signature of the `Index` method to test how to pass the route-bound ID parameter, change it back so that your `Index` method takes a string parameter named `searchString`:</span></span>

[!code-csharp[Main](adding-search/samples/sample7.cs)]

<span data-ttu-id="0e28f-132">Abra la *Views\Movies\Index.cshtml* archivo y, justo después `@Html.ActionLink("Create New", "Create")`, agregue el marcado de formulario resaltado a continuación:</span><span class="sxs-lookup"><span data-stu-id="0e28f-132">Open the *Views\Movies\Index.cshtml* file, and just after `@Html.ActionLink("Create New", "Create")`, add the form markup highlighted below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

<span data-ttu-id="0e28f-133">El `Html.BeginForm` auxiliar crea apertura `<form>` etiqueta.</span><span class="sxs-lookup"><span data-stu-id="0e28f-133">The `Html.BeginForm` helper creates an opening `<form>` tag.</span></span> <span data-ttu-id="0e28f-134">El `Html.BeginForm` auxiliar hace que el formulario registrar a sí mismo cuando el usuario envía el formulario, haga clic en el **filtro** botón.</span><span class="sxs-lookup"><span data-stu-id="0e28f-134">The `Html.BeginForm` helper causes the form to post to itself when the user submits the form by clicking the **Filter** button.</span></span>

<span data-ttu-id="0e28f-135">Visual Studio 2013 tiene una mejora "nice" al mostrar y editar archivos de vista.</span><span class="sxs-lookup"><span data-stu-id="0e28f-135">Visual Studio 2013 has a nice improvement when displaying and editing View files.</span></span> <span data-ttu-id="0e28f-136">Al ejecutar la aplicación con un archivo de vista abierta, Visual Studio 2013, se invoca el método de acción de controlador correcto para mostrar la vista.</span><span class="sxs-lookup"><span data-stu-id="0e28f-136">When you run the application with a view file open, Visual Studio 2013 invokes the correct controller action method to display the view.</span></span>

![](adding-search/_static/image3.png)

<span data-ttu-id="0e28f-137">Con la vista de índice abierto en Visual Studio (como se muestra en la imagen anterior), pulse Ctr F5 o F5 para ejecutar la aplicación y, a continuación, pruebe a buscar una película.</span><span class="sxs-lookup"><span data-stu-id="0e28f-137">With the Index view open in Visual Studio (as shown in the image above), tap Ctr F5 or F5 to run the application and then try searching for a movie.</span></span>

![](adding-search/_static/image4.png)

<span data-ttu-id="0e28f-138">No hay ningún `HttpPost` sobrecarga de la `Index` método.</span><span class="sxs-lookup"><span data-stu-id="0e28f-138">There's no `HttpPost` overload of the `Index` method.</span></span> <span data-ttu-id="0e28f-139">No es necesario, porque el método no cambia el estado de la aplicación, simplemente filtrar datos.</span><span class="sxs-lookup"><span data-stu-id="0e28f-139">You don't need it, because the method isn't changing the state of the application, just filtering data.</span></span>

<span data-ttu-id="0e28f-140">Después, puede agregar el método `HttpPost Index` siguiente.</span><span class="sxs-lookup"><span data-stu-id="0e28f-140">You could add the following `HttpPost Index` method.</span></span> <span data-ttu-id="0e28f-141">En ese caso, coincidiría con el invocador de acción el `HttpPost Index` método y el `HttpPost Index` método ejecutaría tal como se muestra en la imagen siguiente.</span><span class="sxs-lookup"><span data-stu-id="0e28f-141">In that case, the action invoker would match the `HttpPost Index` method, and the `HttpPost Index` method would run as shown in the image below.</span></span>

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

<span data-ttu-id="0e28f-143">Sin embargo, aunque agregue esta versión de `HttpPost` al método `Index`, hay una limitación en cómo se ha implementado todo esto.</span><span class="sxs-lookup"><span data-stu-id="0e28f-143">However, even if you add this `HttpPost` version of the `Index` method, there's a limitation in how this has all been implemented.</span></span> <span data-ttu-id="0e28f-144">Supongamos que quiere marcar una búsqueda en particular o que quiere enviar un vínculo a sus amigos donde puedan hacer clic para ver la misma lista filtrada de películas.</span><span class="sxs-lookup"><span data-stu-id="0e28f-144">Imagine that you want to bookmark a particular search or you want to send a link to friends that they can click in order to see the same filtered list of movies.</span></span> <span data-ttu-id="0e28f-145">Tenga en cuenta que la dirección URL de la solicitud HTTP POST es el mismo que la dirección URL de la solicitud de obtención (localhost:xxxxx/películas/índice), no hay información de búsqueda en la propia dirección URL.</span><span class="sxs-lookup"><span data-stu-id="0e28f-145">Notice that the URL for the HTTP POST request is the same as the URL for the GET request (localhost:xxxxx/Movies/Index) -- there's no search information in the URL itself.</span></span> <span data-ttu-id="0e28f-146">Derecha ahora, la información de la cadena de búsqueda se envía al servidor como un valor de campo de formulario.</span><span class="sxs-lookup"><span data-stu-id="0e28f-146">Right now, the search string information is sent to the server as a form field value.</span></span> <span data-ttu-id="0e28f-147">Esto significa que no se puede capturar dicha información de búsqueda para marcar o enviar a amigos en una dirección URL.</span><span class="sxs-lookup"><span data-stu-id="0e28f-147">This means you can't capture that search information to bookmark or send to friends in a URL.</span></span>

<span data-ttu-id="0e28f-148">La solución consiste en utilizar una sobrecarga de `BeginForm` que especifica que la solicitud POST debe agregar la información de búsqueda a la dirección URL y que se enrute a la `HttpGet` versión de la `Index` método.</span><span class="sxs-lookup"><span data-stu-id="0e28f-148">The solution is to use an overload of `BeginForm` that specifies that the POST request should add the search information to the URL and that it should be routed to the `HttpGet` version of the `Index` method.</span></span> <span data-ttu-id="0e28f-149">Reemplazar el existente sin parámetros `BeginForm` método con el siguiente marcado:</span><span class="sxs-lookup"><span data-stu-id="0e28f-149">Replace the existing parameterless `BeginForm` method with the following markup:</span></span>

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

<span data-ttu-id="0e28f-151">Ahora cuando se envía una búsqueda, la dirección URL contiene una cadena de consulta de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="0e28f-151">Now when you submit a search, the URL contains a search query string.</span></span> <span data-ttu-id="0e28f-152">La búsqueda también será dirigida al método de acción `HttpGet Index`, aunque tenga un método `HttpPost Index`.</span><span class="sxs-lookup"><span data-stu-id="0e28f-152">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a><span data-ttu-id="0e28f-154">Adición de búsqueda por género</span><span class="sxs-lookup"><span data-stu-id="0e28f-154">Adding Search by Genre</span></span>

<span data-ttu-id="0e28f-155">Si agregó la `HttpPost` versión de la `Index` método elimina ahora.</span><span class="sxs-lookup"><span data-stu-id="0e28f-155">If you added the `HttpPost` version of the `Index` method, delete it now.</span></span>

<span data-ttu-id="0e28f-156">A continuación, agregará una característica para que los usuarios puedan buscar películas por género.</span><span class="sxs-lookup"><span data-stu-id="0e28f-156">Next, you'll add a feature to let users search for movies by genre.</span></span> <span data-ttu-id="0e28f-157">Reemplace el método `Index` con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="0e28f-157">Replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample11.cs)]

<span data-ttu-id="0e28f-158">Esta versión de la `Index` método toma un parámetro adicional, es decir, `movieGenre`.</span><span class="sxs-lookup"><span data-stu-id="0e28f-158">This version of the `Index` method takes an additional parameter, namely `movieGenre`.</span></span> <span data-ttu-id="0e28f-159">Las primeras líneas de código crean un `List` objeto para mantener géneros de película de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0e28f-159">The first few lines of code create a `List` object to hold movie genres from the database.</span></span>

<span data-ttu-id="0e28f-160">El código siguiente es una consulta LINQ que recupera todos los géneros de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0e28f-160">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[Main](adding-search/samples/sample12.cs)]

<span data-ttu-id="0e28f-161">El código usa el `AddRange` método de la clase genérica `List` colección para agregar todos los géneros distintos a la lista.</span><span class="sxs-lookup"><span data-stu-id="0e28f-161">The code uses the `AddRange` method of the generic `List` collection to add all the distinct genres to the list.</span></span> <span data-ttu-id="0e28f-162">(Sin el `Distinct` modificador, se agregaría géneros duplicados, por ejemplo, se agregaría Comedia dos veces en nuestro ejemplo).</span><span class="sxs-lookup"><span data-stu-id="0e28f-162">(Without the `Distinct` modifier, duplicate genres would be added — for example, comedy would be added twice in our sample).</span></span> <span data-ttu-id="0e28f-163">El código, a continuación, almacena la lista de géneros en la `ViewBag.MovieGenre` objeto.</span><span class="sxs-lookup"><span data-stu-id="0e28f-163">The code then stores the list of genres in the `ViewBag.MovieGenre` object.</span></span> <span data-ttu-id="0e28f-164">Almacenar datos de categoría (del tal un película género) como un [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) objeto en un `ViewBag`, a continuación, el acceso a los datos de categoría en un cuadro de lista desplegable es un enfoque típico para las aplicaciones de MVC.</span><span class="sxs-lookup"><span data-stu-id="0e28f-164">Storing category data (such a movie genre's) as a [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) object in a `ViewBag`, then accessing the category data in a dropdown list box is a typical approach for MVC applications.</span></span>

<span data-ttu-id="0e28f-165">El código siguiente muestra cómo comprobar el `movieGenre` parámetro.</span><span class="sxs-lookup"><span data-stu-id="0e28f-165">The following code shows how to check the `movieGenre` parameter.</span></span> <span data-ttu-id="0e28f-166">Si no está vacía, el código más restringe la consulta de películas para limitar las películas seleccionadas para el género especificado.</span><span class="sxs-lookup"><span data-stu-id="0e28f-166">If it's not empty, the code further constrains the movies query to limit the selected movies to the specified genre.</span></span>

[!code-csharp[Main](adding-search/samples/sample13.cs)]

<span data-ttu-id="0e28f-167">Como se mencionó anteriormente, la consulta no se ejecuta en la base de datos hasta que se recorre en iteración la lista de películas (lo que ocurre en la vista, después de la `Index` devuelve el método de acción).</span><span class="sxs-lookup"><span data-stu-id="0e28f-167">As stated previously, the query is not run on the data base until the movie list is iterated over (which happens in the View, after the `Index` action method returns).</span></span>

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a><span data-ttu-id="0e28f-168">Agregar marcas a la vista de índice para admitir la búsqueda por género</span><span class="sxs-lookup"><span data-stu-id="0e28f-168">Adding Markup to the Index View to Support Search by Genre</span></span>

<span data-ttu-id="0e28f-169">Agregar un `Html.DropDownList` auxiliar para la *Views\Movies\Index.cshtml* archivo, justo antes del `TextBox` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="0e28f-169">Add an `Html.DropDownList` helper to the *Views\Movies\Index.cshtml* file, just before the `TextBox` helper.</span></span> <span data-ttu-id="0e28f-170">A continuación se muestra el marcado completado:</span><span class="sxs-lookup"><span data-stu-id="0e28f-170">The completed markup is shown below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

<span data-ttu-id="0e28f-171">En el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="0e28f-171">In the following code:</span></span>

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

<span data-ttu-id="0e28f-172">El parámetro "MovieGenre" proporciona la clave para la `DropDownList` auxiliar para buscar un `IEnumerable<SelectListItem>` en el `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="0e28f-172">The parameter "MovieGenre" provides the key for the `DropDownList` helper to find a `IEnumerable<SelectListItem>` in the `ViewBag`.</span></span> <span data-ttu-id="0e28f-173">El `ViewBag` se rellena en el método de acción:</span><span class="sxs-lookup"><span data-stu-id="0e28f-173">The `ViewBag` was populated in the action method:</span></span>

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

<span data-ttu-id="0e28f-174">El parámetro "All" proporciona una etiqueta de opción.</span><span class="sxs-lookup"><span data-stu-id="0e28f-174">The parameter "All" provides an option label.</span></span> <span data-ttu-id="0e28f-175">Si examina esta elección en el explorador, verá que el atributo "valor" está vacío.</span><span class="sxs-lookup"><span data-stu-id="0e28f-175">If you inspect that choice in your browser, you'll see that its "value" attribute is empty.</span></span> <span data-ttu-id="0e28f-176">Puesto que nuestro controlador solo filtra `if` la cadena no es `null` o está vacía, enviando un valor vacío para `movieGenre` muestra todos los juegos.</span><span class="sxs-lookup"><span data-stu-id="0e28f-176">Since our controller only filters `if` the string is not `null` or empty, submitting an empty value for `movieGenre` shows all genres.</span></span>

<span data-ttu-id="0e28f-177">También puede establecer una opción que se seleccione de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="0e28f-177">You can also set an option to be selected by default.</span></span> <span data-ttu-id="0e28f-178">Si deseara "Comedia" como opción de forma predeterminada, debería cambiar el código en el controlador de este modo:</span><span class="sxs-lookup"><span data-stu-id="0e28f-178">If you wanted "Comedy" as your default option, you would change the code in the Controller like so:</span></span>

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

<span data-ttu-id="0e28f-179">Ejecute la aplicación y vaya a */películas/índice*.</span><span class="sxs-lookup"><span data-stu-id="0e28f-179">Run the application and browse to */Movies/Index*.</span></span> <span data-ttu-id="0e28f-180">Intenta una búsqueda por género, nombre de la película y ambos criterios.</span><span class="sxs-lookup"><span data-stu-id="0e28f-180">Try a search by genre, by movie name, and by both criteria.</span></span>

![](adding-search/_static/image8.png)

<span data-ttu-id="0e28f-181">En esta sección se crean un método de acción de búsqueda y una vista que los usuarios pueden buscar por título de la película y el género.</span><span class="sxs-lookup"><span data-stu-id="0e28f-181">In this section you created a search action method and view that let users search by movie title and genre.</span></span> <span data-ttu-id="0e28f-182">En la siguiente sección, examinará cómo agregar una propiedad a la `Movie` modelo y cómo agregar un inicializador que creará automáticamente una base de datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="0e28f-182">In the next section, you'll look at how to add a property to the `Movie` model and how to add an initializer that will automatically create a test database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0e28f-183">[Anterior](examining-the-edit-methods-and-edit-view.md)
> [Siguiente](adding-a-new-field.md)</span><span class="sxs-lookup"><span data-stu-id="0e28f-183">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-a-new-field.md)</span></span>
