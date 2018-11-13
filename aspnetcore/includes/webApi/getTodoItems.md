::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="46c6c-101">El código anterior define una clase de controlador de API sin métodos.</span><span class="sxs-lookup"><span data-stu-id="46c6c-101">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="46c6c-102">En las secciones siguientes, se agregan métodos para implementar la API.</span><span class="sxs-lookup"><span data-stu-id="46c6c-102">In the next sections, methods are added to implement the API.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="46c6c-103">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="46c6c-103">The preceding code:</span></span>

* <span data-ttu-id="46c6c-104">Define una clase de controlador de API sin métodos.</span><span class="sxs-lookup"><span data-stu-id="46c6c-104">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="46c6c-105">Crea una tarea pendiente cuando `TodoItems` está vacío.</span><span class="sxs-lookup"><span data-stu-id="46c6c-105">Creates a new Todo item when `TodoItems` is empty.</span></span> <span data-ttu-id="46c6c-106">No podrá eliminar todas las tareas pendientes porque el constructor crea una si `TodoItems` está vacío.</span><span class="sxs-lookup"><span data-stu-id="46c6c-106">You won't be able to delete all the Todo items because the constructor creates a new one if `TodoItems` is empty.</span></span>

<span data-ttu-id="46c6c-107">En las secciones siguientes, se agregan métodos para implementar la API.</span><span class="sxs-lookup"><span data-stu-id="46c6c-107">In the next sections, methods are added to implement the API.</span></span> <span data-ttu-id="46c6c-108">La clase se anota con un atributo `[ApiController]` para habilitar algunas características muy prácticas.</span><span class="sxs-lookup"><span data-stu-id="46c6c-108">The class is annotated with an `[ApiController]` attribute to enable some convenient features.</span></span> <span data-ttu-id="46c6c-109">Para más información sobre las características que el atributo habilita, vea [Anotación con ApiControllerAttribute](xref:web-api/index#annotation-with-apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="46c6c-109">For information on features enabled by the attribute, see [Annotation with ApiControllerAttribute](xref:web-api/index#annotation-with-apicontrollerattribute).</span></span>

::: moniker-end

<span data-ttu-id="46c6c-110">El constructor del controlador usa la [inserción de dependencias](xref:fundamentals/dependency-injection) para insertar el contexto de base de datos (`TodoContext`) en el controlador.</span><span class="sxs-lookup"><span data-stu-id="46c6c-110">The controller's constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="46c6c-111">El contexto de base de datos se usa en cada uno de los métodos [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) del controlador.</span><span class="sxs-lookup"><span data-stu-id="46c6c-111">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span> <span data-ttu-id="46c6c-112">El constructor agrega un elemento a la base de datos en memoria si no existe ninguno.</span><span class="sxs-lookup"><span data-stu-id="46c6c-112">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="get-to-do-items"></a><span data-ttu-id="46c6c-113">Tareas pendientes</span><span class="sxs-lookup"><span data-stu-id="46c6c-113">Get to-do items</span></span>

<span data-ttu-id="46c6c-114">Para obtener tareas pendientes, agregue estos métodos a la clase `TodoController`:</span><span class="sxs-lookup"><span data-stu-id="46c6c-114">To get to-do items, add the following methods to the `TodoController` class:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

::: moniker-end

<span data-ttu-id="46c6c-115">Estos métodos implementan los dos métodos GET:</span><span class="sxs-lookup"><span data-stu-id="46c6c-115">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="46c6c-116">Esta es una respuesta HTTP de ejemplo del método `GetAll`:</span><span class="sxs-lookup"><span data-stu-id="46c6c-116">Here's a sample HTTP response for the `GetAll` method:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

<span data-ttu-id="46c6c-117">Más adelante en el tutorial, veremos cómo se puede ver la respuesta HTTP por medio de [Postman](https://www.getpostman.com/) o [curl](https://curl.haxx.se/docs/manpage.html).</span><span class="sxs-lookup"><span data-stu-id="46c6c-117">Later in the tutorial, I'll show how the HTTP response can be viewed with [Postman](https://www.getpostman.com/) or [curl](https://curl.haxx.se/docs/manpage.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="46c6c-118">Enrutamiento y rutas URL</span><span class="sxs-lookup"><span data-stu-id="46c6c-118">Routing and URL paths</span></span>

<span data-ttu-id="46c6c-119">El atributo `[HttpGet]` indica un método que responde a una solicitud HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="46c6c-119">The `[HttpGet]` attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="46c6c-120">La ruta de dirección URL para cada método se construye como sigue:</span><span class="sxs-lookup"><span data-stu-id="46c6c-120">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="46c6c-121">Tome la cadena de plantilla en el atributo `Route` del controlador:</span><span class="sxs-lookup"><span data-stu-id="46c6c-121">Take the template string in the controller's `Route` attribute:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

::: moniker-end

* <span data-ttu-id="46c6c-122">Reemplace `[controller]` por el nombre del controlador, que por convención es el nombre de clase de controlador sin el sufijo "Controller".</span><span class="sxs-lookup"><span data-stu-id="46c6c-122">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="46c6c-123">En este ejemplo, el nombre de clase de controlador es **Todo**Controller y el nombre de raíz es "todo".</span><span class="sxs-lookup"><span data-stu-id="46c6c-123">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="46c6c-124">El [enrutamiento](xref:mvc/controllers/routing) en ASP.NET Core no distingue entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="46c6c-124">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="46c6c-125">Si el atributo `[HttpGet]` tiene una plantilla de ruta (como `[HttpGet("/products")]`), anexiónela a la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="46c6c-125">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="46c6c-126">En este ejemplo no se usa una plantilla.</span><span class="sxs-lookup"><span data-stu-id="46c6c-126">This sample doesn't use a template.</span></span> <span data-ttu-id="46c6c-127">Para más información, vea [Enrutamiento mediante atributos con atributos Http[Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="46c6c-127">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="46c6c-128">En el siguiente método `GetById`, `"{id}"` es una variable de marcador de posición correspondiente al identificador único de la tarea pendiente.</span><span class="sxs-lookup"><span data-stu-id="46c6c-128">In the following `GetById` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="46c6c-129">Cuando `GetById` se invoca, asigna el valor `"{id}"` de la dirección URL al parámetro `id` del método.</span><span class="sxs-lookup"><span data-stu-id="46c6c-129">When `GetById` is invoked, it assigns the value of `"{id}"` in the URL to the method's `id` parameter.</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

<span data-ttu-id="46c6c-130">`Name = "GetTodo"` crea una ruta con nombre.</span><span class="sxs-lookup"><span data-stu-id="46c6c-130">`Name = "GetTodo"` creates a named route.</span></span> <span data-ttu-id="46c6c-131">Rutas con nombre:</span><span class="sxs-lookup"><span data-stu-id="46c6c-131">Named routes:</span></span>

* <span data-ttu-id="46c6c-132">Permita que la aplicación cree un vínculo HTTP mediante el nombre de ruta.</span><span class="sxs-lookup"><span data-stu-id="46c6c-132">Enable the app to create an HTTP link using the route name.</span></span>
* <span data-ttu-id="46c6c-133">Se explican más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="46c6c-133">Are explained later in the tutorial.</span></span>

### <a name="return-values"></a><span data-ttu-id="46c6c-134">Valores devueltos</span><span class="sxs-lookup"><span data-stu-id="46c6c-134">Return values</span></span>

<span data-ttu-id="46c6c-135">El método `GetAll` devuelve una colección de objetos `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="46c6c-135">The `GetAll` method returns a collection of `TodoItem` objects.</span></span> <span data-ttu-id="46c6c-136">MVC serializa automáticamente el objeto a [JSON](https://www.json.org/) y escribe el JSON en el cuerpo del mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="46c6c-136">MVC automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="46c6c-137">El código de respuesta para este método es 200, suponiendo que no haya ninguna excepción no controlada.</span><span class="sxs-lookup"><span data-stu-id="46c6c-137">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="46c6c-138">Las excepciones no controladas se convierten en errores 5xx.</span><span class="sxs-lookup"><span data-stu-id="46c6c-138">Unhandled exceptions are translated into 5xx errors.</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="46c6c-139">En cambio, el método `GetById` devuelve el tipo más general [IActionResult](xref:web-api/action-return-types#iactionresult-type), que representa una amplia gama de tipos de valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="46c6c-139">In contrast, the `GetById` method returns the more general [IActionResult type](xref:web-api/action-return-types#iactionresult-type), which represents a wide range of return types.</span></span> <span data-ttu-id="46c6c-140">`GetById` tiene dos tipos de valor devueltos distintos:</span><span class="sxs-lookup"><span data-stu-id="46c6c-140">`GetById` has two different return types:</span></span>

* <span data-ttu-id="46c6c-141">Si no hay ningún elemento que coincida con el identificador solicitado, el método devuelve un error 404.</span><span class="sxs-lookup"><span data-stu-id="46c6c-141">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="46c6c-142">Devolver [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) genera una respuesta HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="46c6c-142">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="46c6c-143">En caso contrario, el método devuelve 200 con un cuerpo de respuesta JSON.</span><span class="sxs-lookup"><span data-stu-id="46c6c-143">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="46c6c-144">Devolver [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) genera una respuesta HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="46c6c-144">Returning [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) results in an HTTP 200 response.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="46c6c-145">En cambio, el método `GetById` devuelve el tipo [ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type), que representa una amplia gama de tipos de valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="46c6c-145">In contrast, the `GetById` method returns the [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type), which represents a wide range of return types.</span></span> <span data-ttu-id="46c6c-146">`GetById` tiene dos tipos de valor devueltos distintos:</span><span class="sxs-lookup"><span data-stu-id="46c6c-146">`GetById` has two different return types:</span></span>

* <span data-ttu-id="46c6c-147">Si no hay ningún elemento que coincida con el identificador solicitado, el método devuelve un error 404.</span><span class="sxs-lookup"><span data-stu-id="46c6c-147">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="46c6c-148">Devolver [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) genera una respuesta HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="46c6c-148">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="46c6c-149">En caso contrario, el método devuelve 200 con un cuerpo de respuesta JSON.</span><span class="sxs-lookup"><span data-stu-id="46c6c-149">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="46c6c-150">Devolver `item` genera una respuesta HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="46c6c-150">Returning `item` results in an HTTP 200 response.</span></span>

::: moniker-end
