[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="8974d-101">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="8974d-101">The preceding code:</span></span>

* <span data-ttu-id="8974d-102">Define una clase de controlador vacía.</span><span class="sxs-lookup"><span data-stu-id="8974d-102">Defines an empty controller class.</span></span> <span data-ttu-id="8974d-103">En las secciones siguientes, se agregan métodos para implementar la API.</span><span class="sxs-lookup"><span data-stu-id="8974d-103">In the next sections, methods are added to implement the API.</span></span>
* <span data-ttu-id="8974d-104">El constructor usa la [inserción de dependencias](xref:fundamentals/dependency-injection) para insertar el contexto de base de datos (`TodoContext `) en el controlador.</span><span class="sxs-lookup"><span data-stu-id="8974d-104">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext `) into the controller.</span></span> <span data-ttu-id="8974d-105">El contexto de base de datos se usa en cada uno de los métodos [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) del controlador.</span><span class="sxs-lookup"><span data-stu-id="8974d-105">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="8974d-106">El constructor agrega un elemento a la base de datos en memoria si no existe ninguno.</span><span class="sxs-lookup"><span data-stu-id="8974d-106">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="getting-to-do-items"></a><span data-ttu-id="8974d-107">Obtención de tareas pendientes</span><span class="sxs-lookup"><span data-stu-id="8974d-107">Getting to-do items</span></span>

<span data-ttu-id="8974d-108">Para obtener tareas pendientes, agregue estos métodos a la clase `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="8974d-108">To get to-do items, add the following methods to the `TodoController` class.</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="8974d-109">Estos métodos implementan los dos métodos GET:</span><span class="sxs-lookup"><span data-stu-id="8974d-109">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="8974d-110">Esta es una respuesta HTTP de ejemplo para el método `GetAll`:</span><span class="sxs-lookup"><span data-stu-id="8974d-110">Here is an example HTTP response for the `GetAll` method:</span></span>

```
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
   ```

<span data-ttu-id="8974d-111">Más adelante en el tutorial, se mostrará cómo se puede ver la respuesta HTTP mediante [Postman](https://www.getpostman.com/) o [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span><span class="sxs-lookup"><span data-stu-id="8974d-111">Later in the tutorial I'll show how the HTTP response can be viewed with [Postman](https://www.getpostman.com/) or [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="8974d-112">Enrutamiento y rutas URL</span><span class="sxs-lookup"><span data-stu-id="8974d-112">Routing and URL paths</span></span>

<span data-ttu-id="8974d-113">El atributo `[HttpGet]` especifica un método HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="8974d-113">The `[HttpGet]` attribute specifies an HTTP GET method.</span></span> <span data-ttu-id="8974d-114">La ruta de dirección URL para cada método se construye como sigue:</span><span class="sxs-lookup"><span data-stu-id="8974d-114">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="8974d-115">Tome la cadena de plantilla en el atributo `Route` del controlador:</span><span class="sxs-lookup"><span data-stu-id="8974d-115">Take the template string in the controller's `Route` attribute:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="8974d-116">Reemplace `[controller]` por el nombre del controlador, que es el nombre de clase de controlador sin el sufijo "Controller".</span><span class="sxs-lookup"><span data-stu-id="8974d-116">Replaces `[controller]` with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="8974d-117">En este ejemplo, el nombre de clase de controlador es **Todo**Controller y el nombre de raíz es "todo".</span><span class="sxs-lookup"><span data-stu-id="8974d-117">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="8974d-118">El [enrutamiento](xref:mvc/controllers/routing) en ASP.NET Core no distingue entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="8974d-118">ASP.NET Core [routing](xref:mvc/controllers/routing) isn't case sensitive.</span></span>
* <span data-ttu-id="8974d-119">Si el atributo `[HttpGet]` tiene una plantilla de ruta (como `[HttpGet("/products")]`), anexiónela a la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="8974d-119">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="8974d-120">En este ejemplo no se usa una plantilla.</span><span class="sxs-lookup"><span data-stu-id="8974d-120">This sample doesn't use a template.</span></span> <span data-ttu-id="8974d-121">Para más información, vea [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) (Enrutamiento de atributos con atributos Http[Verb]).</span><span class="sxs-lookup"><span data-stu-id="8974d-121">See [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) for more information.</span></span>

<span data-ttu-id="8974d-122">En el método `GetById`:</span><span class="sxs-lookup"><span data-stu-id="8974d-122">In the `GetById` method:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

<span data-ttu-id="8974d-123">`"{id}"` es una variable de marcador de posición para el identificador del elemento `todo`.</span><span class="sxs-lookup"><span data-stu-id="8974d-123">`"{id}"` is a placeholder variable for the ID of the `todo` item.</span></span> <span data-ttu-id="8974d-124">Cuando se invoca `GetById`, asigna el valor de "{id}" en la URL al parámetro del método `id`.</span><span class="sxs-lookup"><span data-stu-id="8974d-124">When `GetById` is invoked, it assigns the value of "{id}" in the URL to the method's `id` parameter.</span></span>

<span data-ttu-id="8974d-125">`Name = "GetTodo"` crea una ruta con nombre.</span><span class="sxs-lookup"><span data-stu-id="8974d-125">`Name = "GetTodo"` creates a named route.</span></span> <span data-ttu-id="8974d-126">Rutas con nombre:</span><span class="sxs-lookup"><span data-stu-id="8974d-126">Named routes:</span></span>

* <span data-ttu-id="8974d-127">Permita que la aplicación cree un vínculo HTTP mediante el nombre de ruta.</span><span class="sxs-lookup"><span data-stu-id="8974d-127">Enable the app to create an HTTP link using the route name.</span></span>
* <span data-ttu-id="8974d-128">Se explican más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="8974d-128">Are explained later in the tutorial.</span></span>

### <a name="return-values"></a><span data-ttu-id="8974d-129">Valores devueltos</span><span class="sxs-lookup"><span data-stu-id="8974d-129">Return values</span></span>

<span data-ttu-id="8974d-130">El método `GetAll` devuelve un objeto `IEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="8974d-130">The `GetAll` method returns an `IEnumerable`.</span></span> <span data-ttu-id="8974d-131">MVC serializa automáticamente el objeto a [JSON](http://www.json.org/) y escribe el JSON en el cuerpo del mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="8974d-131">MVC automatically serializes the object to [JSON](http://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="8974d-132">El código de respuesta para este método es 200, suponiendo que no haya ninguna excepción no controlada.</span><span class="sxs-lookup"><span data-stu-id="8974d-132">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="8974d-133">(Las excepciones no controladas se convierten en errores 5xx).</span><span class="sxs-lookup"><span data-stu-id="8974d-133">(Unhandled exceptions are translated into 5xx errors.)</span></span>

<span data-ttu-id="8974d-134">En cambio, el método `GetById` devuelve el tipo más general `IActionResult`, que representa una amplia gama de tipos de valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="8974d-134">In contrast, the `GetById` method returns the more general `IActionResult` type, which represents a wide range of return types.</span></span> <span data-ttu-id="8974d-135">`GetById` tiene dos tipos de valor devueltos distintos:</span><span class="sxs-lookup"><span data-stu-id="8974d-135">`GetById` has two different return types:</span></span>

* <span data-ttu-id="8974d-136">Si no hay ningún elemento que coincida con el identificador solicitado, el método devuelve un error 404.</span><span class="sxs-lookup"><span data-stu-id="8974d-136">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="8974d-137">Devolver `NotFound` genera una respuesta HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="8974d-137">Returning `NotFound` returns an HTTP 404 response.</span></span>

* <span data-ttu-id="8974d-138">En caso contrario, el método devuelve 200 con un cuerpo de respuesta JSON.</span><span class="sxs-lookup"><span data-stu-id="8974d-138">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="8974d-139">Devolver `ObjectResult` genera una respuesta HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="8974d-139">Returning `ObjectResult` returns an HTTP 200 response.</span></span>
