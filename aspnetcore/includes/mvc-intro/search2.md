<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](~/tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

<span data-ttu-id="9fc47-101">El método `Index` anterior:</span><span class="sxs-lookup"><span data-stu-id="9fc47-101">The previous `Index` method:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_1stSearch)]

<span data-ttu-id="9fc47-102">El método `Index` actualizado con el parámetro `id`:</span><span class="sxs-lookup"><span data-stu-id="9fc47-102">The updated `Index` method with `id` parameter:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_SearchID)]

<span data-ttu-id="9fc47-103">Ahora puede pasar el título de la búsqueda como datos de ruta (un segmento de dirección URL) en lugar de como un valor de cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="9fc47-103">You can now pass the search title as route data (a URL segment) instead of as a query string value.</span></span>

![Vista de índice con la palabra Ghost (Fantasma) agregada a la dirección URL y una lista de las películas obtenidas, con dos películas, Ghostbusters y Ghostbusters 2](~/tutorials/first-mvc-app/search/_static/g2.png)

<span data-ttu-id="9fc47-105">Sin embargo, no se puede esperar que los usuarios modifiquen la dirección URL cada vez que quieran buscar una película.</span><span class="sxs-lookup"><span data-stu-id="9fc47-105">However, you can't expect users to modify the URL every time they want to search for a movie.</span></span> <span data-ttu-id="9fc47-106">Por tanto, ahora deberá agregar elementos de la interfaz de usuario con los que podrán filtrar las películas.</span><span class="sxs-lookup"><span data-stu-id="9fc47-106">So now you'll add UI elements to help them filter movies.</span></span> <span data-ttu-id="9fc47-107">Si cambió la firma del método `Index` para probar cómo pasar el parámetro `ID` enlazado a una ruta, vuelva a cambiarlo para que tome un parámetro denominado `searchString`:</span><span class="sxs-lookup"><span data-stu-id="9fc47-107">If you changed the signature of the `Index` method to test how to pass the route-bound `ID` parameter, change it back so that it takes a parameter named `searchString`:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_1stSearch)]

<span data-ttu-id="9fc47-108">Abra el archivo *Views/Movies/Index.cshtml* y agregue el marcado `<form>` resaltado a continuación:</span><span class="sxs-lookup"><span data-stu-id="9fc47-108">Open the *Views/Movies/Index.cshtml* file, and add the `<form>` markup highlighted below:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexForm1.cshtml?highlight=10-16&range=4-21)]

<span data-ttu-id="9fc47-109">La etiqueta HTML `<form>` usa la [aplicación auxiliar de etiquetas de formulario](xref:mvc/views/working-with-forms), por lo que cuando se envía el formulario, la cadena de filtro se registra en la acción `Index` del controlador de películas.</span><span class="sxs-lookup"><span data-stu-id="9fc47-109">The HTML `<form>` tag uses the [Form Tag Helper](xref:mvc/views/working-with-forms), so when you submit the form, the filter string is posted to the `Index` action of the movies controller.</span></span> <span data-ttu-id="9fc47-110">Guarde los cambios y después pruebe el filtro.</span><span class="sxs-lookup"><span data-stu-id="9fc47-110">Save your changes and then test the filter.</span></span>

![Vista de índice con la palabra Ghost (Fantasma) escrita en el cuadro de texto del filtro Title (Título)](~/tutorials/first-mvc-app/search/_static/filter.png)

<span data-ttu-id="9fc47-112">No hay ninguna sobrecarga `[HttpPost]` del método `Index` como cabría esperar.</span><span class="sxs-lookup"><span data-stu-id="9fc47-112">There's no `[HttpPost]` overload of the `Index` method as you might expect.</span></span> <span data-ttu-id="9fc47-113">No es necesario, porque el método no cambia el estado de la aplicación, simplemente filtra los datos.</span><span class="sxs-lookup"><span data-stu-id="9fc47-113">You don't need it, because the method isn't changing the state of the app, just filtering data.</span></span>

<span data-ttu-id="9fc47-114">Después, puede agregar el método `[HttpPost] Index` siguiente.</span><span class="sxs-lookup"><span data-stu-id="9fc47-114">You could add the following `[HttpPost] Index` method.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_SearchPost)]

<span data-ttu-id="9fc47-115">El parámetro `notUsed` se usa para crear una sobrecarga para el método `Index`.</span><span class="sxs-lookup"><span data-stu-id="9fc47-115">The `notUsed` parameter is used to create an overload for the `Index` method.</span></span> <span data-ttu-id="9fc47-116">Hablaremos sobre esto más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="9fc47-116">We'll talk about that later in the tutorial.</span></span>

<span data-ttu-id="9fc47-117">Si agrega este método, el invocador de acción coincidiría con el método `[HttpPost] Index`, mientras que el método `[HttpPost] Index` se ejecutaría tal como se muestra en la imagen de abajo.</span><span class="sxs-lookup"><span data-stu-id="9fc47-117">If you add this method, the action invoker would match the `[HttpPost] Index` method, and the `[HttpPost] Index` method would run as shown in the image below.</span></span>

![Ventana del explorador con la respuesta: From HttpPost Index: filter on ghost (Desde el índice HttpPost: filtrar en Ghost)](~/tutorials/first-mvc-app/search/_static/fo.png)

<span data-ttu-id="9fc47-119">Sin embargo, aunque agregue esta versión de `[HttpPost]` al método `Index`, hay una limitación en cómo se ha implementado todo esto.</span><span class="sxs-lookup"><span data-stu-id="9fc47-119">However, even if you add this `[HttpPost]` version of the `Index` method, there's a limitation in how this has all been implemented.</span></span> <span data-ttu-id="9fc47-120">Supongamos que quiere marcar una búsqueda en particular o que quiere enviar un vínculo a sus amigos donde puedan hacer clic para ver la misma lista filtrada de películas.</span><span class="sxs-lookup"><span data-stu-id="9fc47-120">Imagine that you want to bookmark a particular search or you want to send a link to friends that they can click in order to see the same filtered list of movies.</span></span> <span data-ttu-id="9fc47-121">Tenga en cuenta que la dirección URL de la solicitud HTTP POST es la misma que la dirección URL de la solicitud GET (localhost:xxxxx/Movies/Index): no hay información de búsqueda en la URL.</span><span class="sxs-lookup"><span data-stu-id="9fc47-121">Notice that the URL for the HTTP POST request is the same as the URL for the GET request (localhost:xxxxx/Movies/Index) -- there's no search information in the URL.</span></span> <span data-ttu-id="9fc47-122">La información de la cadena de búsqueda se envía al servidor como un [valor de campo de formulario](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data).</span><span class="sxs-lookup"><span data-stu-id="9fc47-122">The search string information is sent to the server as a [form field value](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data).</span></span> <span data-ttu-id="9fc47-123">Puede comprobarlo con las herramientas de desarrollo del explorador o con la excelente [herramienta Fiddler](http://www.telerik.com/fiddler).</span><span class="sxs-lookup"><span data-stu-id="9fc47-123">You can verify that with the browser Developer tools or the excellent [Fiddler tool](http://www.telerik.com/fiddler).</span></span> <span data-ttu-id="9fc47-124">En la imagen de abajo se muestran las herramientas de desarrollo del explorador Chrome:</span><span class="sxs-lookup"><span data-stu-id="9fc47-124">The image below shows the Chrome browser Developer tools:</span></span>

![Pestaña Red de las herramientas de desarrollo en Microsoft Edge, que muestra un cuerpo de solicitud con un valor searchString de la palabra Ghost](~/tutorials/first-mvc-app/search/_static/f12_rb.png)

<span data-ttu-id="9fc47-126">Puede ver el parámetro de búsqueda y el token [XSRF](xref:security/anti-request-forgery) en el cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="9fc47-126">You can see the search parameter and [XSRF](xref:security/anti-request-forgery) token in the request body.</span></span> <span data-ttu-id="9fc47-127">Tenga en cuenta, como se mencionó en el tutorial anterior, que la [aplicación auxiliar de etiquetas de formulario](xref:mvc/views/working-with-forms) genera un token [XSRF](xref:security/anti-request-forgery) antifalsificación.</span><span class="sxs-lookup"><span data-stu-id="9fc47-127">Note, as mentioned in the previous tutorial, the [Form Tag Helper](xref:mvc/views/working-with-forms) generates an [XSRF](xref:security/anti-request-forgery) anti-forgery token.</span></span> <span data-ttu-id="9fc47-128">Como no se van a modificar datos, no es necesario validar el token con el método del controlador.</span><span class="sxs-lookup"><span data-stu-id="9fc47-128">We're not modifying data, so we don't need to validate the token in the controller method.</span></span>

<span data-ttu-id="9fc47-129">El parámetro de búsqueda se encuentra en el cuerpo de solicitud y no en la dirección URL. Por eso no se puede capturar dicha información para marcarla o compartirla con otros usuarios.</span><span class="sxs-lookup"><span data-stu-id="9fc47-129">Because the search parameter is in the request body and not the URL, you can't capture that search information to bookmark or share with others.</span></span> <span data-ttu-id="9fc47-130">Para solucionar este problema, especificaremos que la solicitud sea `HTTP GET`.</span><span class="sxs-lookup"><span data-stu-id="9fc47-130">We'll fix this by specifying the request should be `HTTP GET`.</span></span>
