<span data-ttu-id="516b8-101">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span><span class="sxs-lookup"><span data-stu-id="516b8-101">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span></span>

<span data-ttu-id="516b8-102">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="516b8-102">The preceding code:</span></span>

* <span data-ttu-id="516b8-103">Define una clase de controlador vacía.</span><span class="sxs-lookup"><span data-stu-id="516b8-103">Defines an empty controller class.</span></span> <span data-ttu-id="516b8-104">En las secciones siguientes, agregaremos métodos para implementar la API.</span><span class="sxs-lookup"><span data-stu-id="516b8-104">In the next sections, we'll add methods to implement the API.</span></span>
* <span data-ttu-id="516b8-105">El constructor usa la [inserción de dependencias](xref:fundamentals/dependency-injection) para insertar el contexto de base de datos (`TodoContext `) en el controlador.</span><span class="sxs-lookup"><span data-stu-id="516b8-105">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext `) into the controller.</span></span> <span data-ttu-id="516b8-106">El contexto de base de datos se usa en cada uno de los métodos [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) del controlador.</span><span class="sxs-lookup"><span data-stu-id="516b8-106">The database context is used in each of the [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="516b8-107">El constructor agrega un elemento a la base de datos en memoria si no existe ninguno.</span><span class="sxs-lookup"><span data-stu-id="516b8-107">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="getting-to-do-items"></a><span data-ttu-id="516b8-108">Obtención de tareas pendientes</span><span class="sxs-lookup"><span data-stu-id="516b8-108">Getting to-do items</span></span>

<span data-ttu-id="516b8-109">Para obtener tareas pendientes, agregue estos métodos a la clase `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="516b8-109">To get to-do items, add the following methods to the `TodoController` class.</span></span>

<span data-ttu-id="516b8-110">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span><span class="sxs-lookup"><span data-stu-id="516b8-110">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span></span>

<span data-ttu-id="516b8-111">Estos métodos implementan los dos métodos GET:</span><span class="sxs-lookup"><span data-stu-id="516b8-111">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="516b8-112">Esta es una respuesta HTTP de ejemplo para el método `GetAll`:</span><span class="sxs-lookup"><span data-stu-id="516b8-112">Here is an example HTTP response for the `GetAll` method:</span></span>

```
HTTP/1.1 200 OK
   Content-Type: application/json; charset=utf-8
   Server: Microsoft-IIS/10.0
   Date: Thu, 18 Jun 2015 20:51:10 GMT
   Content-Length: 82

   [{"Key":"1", "Name":"Item1","IsComplete":false}]
   ```

<span data-ttu-id="516b8-113">Más adelante en el tutorial, mostraré cómo puede ver la respuesta HTTP mediante [Postman](https://www.getpostman.com/) o [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span><span class="sxs-lookup"><span data-stu-id="516b8-113">Later in the tutorial I'll show how you can view the HTTP response using [Postman](https://www.getpostman.com/) or [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="516b8-114">Enrutamiento y rutas URL</span><span class="sxs-lookup"><span data-stu-id="516b8-114">Routing and URL paths</span></span>

<span data-ttu-id="516b8-115">El atributo `[HttpGet]` especifica un método HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="516b8-115">The `[HttpGet]` attribute specifies an HTTP GET method.</span></span> <span data-ttu-id="516b8-116">La ruta de dirección URL para cada método se construye como sigue:</span><span class="sxs-lookup"><span data-stu-id="516b8-116">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="516b8-117">Tome la cadena de plantilla en el atributo de ruta del controlador:</span><span class="sxs-lookup"><span data-stu-id="516b8-117">Take the template string in the controller’s route attribute:</span></span>

<span data-ttu-id="516b8-118">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="516b8-118">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span></span>

* <span data-ttu-id="516b8-119">Reemplace "[Controller]" por el nombre del controlador, que es el nombre de clase de controlador sin el sufijo "Controller".</span><span class="sxs-lookup"><span data-stu-id="516b8-119">Replace "[Controller]" with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="516b8-120">En este ejemplo, el nombre de clase de controlador es **Todo**Controller y el nombre de raíz es "todo".</span><span class="sxs-lookup"><span data-stu-id="516b8-120">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="516b8-121">El [enrutamiento](xref:mvc/controllers/routing) en ASP.NET Core no distingue entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="516b8-121">ASP.NET Core [routing](xref:mvc/controllers/routing) is not case sensitive.</span></span>
* <span data-ttu-id="516b8-122">Si el atributo `[HttpGet]` tiene una plantilla de ruta (como `[HttpGet("/products")]`), anexiónela a la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="516b8-122">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="516b8-123">En este ejemplo no se usa una plantilla.</span><span class="sxs-lookup"><span data-stu-id="516b8-123">This sample doesn't use a template.</span></span> <span data-ttu-id="516b8-124">Para más información, vea [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) (Enrutamiento de atributos con atributos Http[Verb]).</span><span class="sxs-lookup"><span data-stu-id="516b8-124">See [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) for more information.</span></span>

<span data-ttu-id="516b8-125">En el método `GetById`:</span><span class="sxs-lookup"><span data-stu-id="516b8-125">In the `GetById` method:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

<span data-ttu-id="516b8-126">`"{id}"` es una variable de marcador de posición para el identificador del elemento `todo`.</span><span class="sxs-lookup"><span data-stu-id="516b8-126">`"{id}"` is a placeholder variable for the ID of the `todo` item.</span></span> <span data-ttu-id="516b8-127">Cuando se invoca `GetById`, asigna el valor de "{id}" en la URL al parámetro del método `id`.</span><span class="sxs-lookup"><span data-stu-id="516b8-127">When `GetById` is invoked, it assigns the value of "{id}" in the URL to the method's `id` parameter.</span></span>

<span data-ttu-id="516b8-128">`Name = "GetTodo"` crea una ruta con nombre y permite establecer un vínculo a esta ruta en una respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="516b8-128">`Name = "GetTodo"` creates a named route and allows you to link to this route in an HTTP Response.</span></span> <span data-ttu-id="516b8-129">Explicaré esto con un ejemplo más adelante.</span><span class="sxs-lookup"><span data-stu-id="516b8-129">I'll explain it with an example later.</span></span> <span data-ttu-id="516b8-130">Para más información, vea [Routing to Controller Actions](xref:mvc/controllers/routing) (Enrutamiento a acciones del controlador).</span><span class="sxs-lookup"><span data-stu-id="516b8-130">See [Routing to Controller Actions](xref:mvc/controllers/routing) for detailed information.</span></span>

### <a name="return-values"></a><span data-ttu-id="516b8-131">Valores devueltos</span><span class="sxs-lookup"><span data-stu-id="516b8-131">Return values</span></span>

<span data-ttu-id="516b8-132">El método `GetAll` devuelve un objeto `IEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="516b8-132">The `GetAll` method returns an `IEnumerable`.</span></span> <span data-ttu-id="516b8-133">MVC serializa automáticamente el objeto a [JSON](http://www.json.org/) y escribe el JSON en el cuerpo del mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="516b8-133">MVC automatically serializes the object to [JSON](http://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="516b8-134">El código de respuesta para este método es 200, suponiendo que no haya ninguna excepción no controlada.</span><span class="sxs-lookup"><span data-stu-id="516b8-134">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="516b8-135">(Las excepciones no controladas se convierten en errores 5xx).</span><span class="sxs-lookup"><span data-stu-id="516b8-135">(Unhandled exceptions are translated into 5xx errors.)</span></span>

<span data-ttu-id="516b8-136">En cambio, el método `GetById` devuelve el tipo más general `IActionResult`, que representa una amplia gama de tipos de valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="516b8-136">In contrast, the `GetById` method returns the more general `IActionResult` type, which represents a wide range of return types.</span></span> <span data-ttu-id="516b8-137">`GetById` tiene dos tipos de valor devueltos distintos:</span><span class="sxs-lookup"><span data-stu-id="516b8-137">`GetById` has two different return types:</span></span>

* <span data-ttu-id="516b8-138">Si no hay ningún elemento que coincida con el identificador solicitado, el método devuelve un error 404.</span><span class="sxs-lookup"><span data-stu-id="516b8-138">If no item matches the requested ID, the method returns a 404 error.</span></span>  <span data-ttu-id="516b8-139">Esto se hace devolviendo `NotFound`.</span><span class="sxs-lookup"><span data-stu-id="516b8-139">This is done by returning `NotFound`.</span></span>

* <span data-ttu-id="516b8-140">En caso contrario, el método devuelve 200 con un cuerpo de respuesta JSON.</span><span class="sxs-lookup"><span data-stu-id="516b8-140">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="516b8-141">Esto se hace devolviendo un `ObjectResult`</span><span class="sxs-lookup"><span data-stu-id="516b8-141">This is done by returning an `ObjectResult`</span></span>
