
## <a name="test-the-app"></a><span data-ttu-id="64b60-101">Probar la aplicación</span><span class="sxs-lookup"><span data-stu-id="64b60-101">Test the app</span></span>

* <span data-ttu-id="64b60-102">Ejecute la aplicación y pulse en el vínculo **Mvc Movie** (Película Mvc).</span><span class="sxs-lookup"><span data-stu-id="64b60-102">Run the app and tap the **Mvc Movie** link.</span></span>
* <span data-ttu-id="64b60-103">Pulse **Create New** (Crear nueva) y cree una película.</span><span class="sxs-lookup"><span data-stu-id="64b60-103">Tap the **Create New** link and create a movie.</span></span>

  ![Crear vista con campos de género, precio, fecha de lanzamiento y título](../../tutorials/first-mvc-app/adding-model/_static/movies.png)

* <span data-ttu-id="64b60-105">Es posible no que pueda escribir comas ni puntos decimales en el campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="64b60-105">You may not be able to enter decimal points or commas in the `Price` field.</span></span> <span data-ttu-id="64b60-106">Para admitir la [validación de jQuery](https://jqueryvalidation.org/) para configuraciones regionales distintas del inglés que usan una coma (",") en lugar de un punto decimal y formatos de fecha distintos de Estados Unidos, debe seguir unos pasos globalizar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="64b60-106">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="64b60-107">Consulte https://github.com/aspnet/Docs/issues/4076 y [recursos adicionales](#additional-resources) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="64b60-107">See https://github.com/aspnet/Docs/issues/4076 and [Additional resources](#additional-resources) for more information.</span></span> <span data-ttu-id="64b60-108">Por ahora, tan solo debe escribir números enteros como 10.</span><span class="sxs-lookup"><span data-stu-id="64b60-108">For now, just enter whole numbers like 10.</span></span>

<a name="displayformatdatelocal"></a>

* <span data-ttu-id="64b60-109">En algunas configuraciones regionales debe especificar el formato de fecha.</span><span class="sxs-lookup"><span data-stu-id="64b60-109">In some locales you need to specify the date format.</span></span> <span data-ttu-id="64b60-110">Vea el código que aparece resaltado a continuación.</span><span class="sxs-lookup"><span data-stu-id="64b60-110">See the highlighted code below.</span></span>

<span data-ttu-id="64b60-111">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateFormat.cs?name=snippet_1&highlight=2,10)]</span><span class="sxs-lookup"><span data-stu-id="64b60-111">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateFormat.cs?name=snippet_1&highlight=2,10)]</span></span>

<span data-ttu-id="64b60-112">Hablaremos sobre `DataAnnotations` más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="64b60-112">We'll talk about `DataAnnotations` later in the tutorial.</span></span>

<span data-ttu-id="64b60-113">Al pulsar en **Crear** el formulario se envía al servidor, donde se guarda la información de la película en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="64b60-113">Tapping **Create** causes the form to be posted to the server, where the movie information is saved in a database.</span></span> <span data-ttu-id="64b60-114">La aplicación redirige a la URL */Movies*, donde se muestra la información de la película que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="64b60-114">The app redirects to the */Movies* URL, where the newly created movie information is displayed.</span></span>

![Vista de películas que muestra la lista de películas que acaba de crear](../../tutorials/first-mvc-app/adding-model/_static/h.png)

<span data-ttu-id="64b60-116">Cree un par de entradas más de película.</span><span class="sxs-lookup"><span data-stu-id="64b60-116">Create a couple more movie entries.</span></span> <span data-ttu-id="64b60-117">Compruebe que todos los vínculos **Editar**, **Detalles** y **Eliminar** son funcionales.</span><span class="sxs-lookup"><span data-stu-id="64b60-117">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>
