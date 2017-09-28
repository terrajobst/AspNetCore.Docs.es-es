## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="6b760-101">Implementar las otras operaciones CRUD</span><span class="sxs-lookup"><span data-stu-id="6b760-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="6b760-102">Vamos a agregar los métodos `Create`, `Update` y `Delete` al controlador.</span><span class="sxs-lookup"><span data-stu-id="6b760-102">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="6b760-103">Son variaciones de un tema, así que solo mostraré el código y comentaré las diferencias principales.</span><span class="sxs-lookup"><span data-stu-id="6b760-103">These are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="6b760-104">Compile el proyecto después de agregar o cambiar el código.</span><span class="sxs-lookup"><span data-stu-id="6b760-104">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="6b760-105">Crear</span><span class="sxs-lookup"><span data-stu-id="6b760-105">Create</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="6b760-106">Se trata de un método HTTP POST, indicado por el atributo [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="6b760-106">This is an HTTP POST method, indicated by the [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="6b760-107">El atributo [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) indica a MVC que obtenga el valor de la tarea pendiente del cuerpo de la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="6b760-107">The [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="6b760-108">El método `CreatedAtRoute` devuelve una respuesta 201, que es la respuesta estándar para un método HTTP POST que crea un nuevo recurso en el servidor.</span><span class="sxs-lookup"><span data-stu-id="6b760-108">The `CreatedAtRoute` method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="6b760-109">`CreatedAtRoute` también agrega un encabezado de ubicación a la respuesta.</span><span class="sxs-lookup"><span data-stu-id="6b760-109">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="6b760-110">El encabezado de ubicación especifica el URI de la tarea pendiente recién creada.</span><span class="sxs-lookup"><span data-stu-id="6b760-110">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="6b760-111">Vea [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (10.2.2 201 creada).</span><span class="sxs-lookup"><span data-stu-id="6b760-111">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="6b760-112">Usar Postman para enviar una solicitud de creación</span><span class="sxs-lookup"><span data-stu-id="6b760-112">Use Postman to send a Create request</span></span>

![Consola de Postman](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="6b760-114">Establezca el método HTTP en `POST`.</span><span class="sxs-lookup"><span data-stu-id="6b760-114">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="6b760-115">Seleccione el botón de radio **Body** (Cuerpo).</span><span class="sxs-lookup"><span data-stu-id="6b760-115">Select the **Body** radio button</span></span>
* <span data-ttu-id="6b760-116">Seleccione el botón de radio **Raw** (Sin formato).</span><span class="sxs-lookup"><span data-stu-id="6b760-116">Select the **raw** radio button</span></span>
* <span data-ttu-id="6b760-117">Establezca el tipo en JSON.</span><span class="sxs-lookup"><span data-stu-id="6b760-117">Set the type to JSON</span></span>
* <span data-ttu-id="6b760-118">En el editor de pares clave-valor, escriba una tarea pendiente como la siguiente:</span><span class="sxs-lookup"><span data-stu-id="6b760-118">In the key-value editor, enter a Todo item such as</span></span> 

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="6b760-119">Seleccione **Send** (Enviar).</span><span class="sxs-lookup"><span data-stu-id="6b760-119">Select **Send**</span></span>

* <span data-ttu-id="6b760-120">Seleccione la pestaña Headers (Encabezados) en el panel inferior y copie el encabezado **Location** (Ubicación):</span><span class="sxs-lookup"><span data-stu-id="6b760-120">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Pestaña Headers (Encabezados) de la consola de Postman](../../tutorials/first-web-api/_static/pmget.png)

<span data-ttu-id="6b760-122">Puede usar el URI del encabezado Location (Ubicación) para acceder al recurso que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="6b760-122">You can use the Location header URI to access the resource you just created.</span></span> <span data-ttu-id="6b760-123">Recuerde que el método `GetById` creó la ruta denominada `"GetTodo"`:</span><span class="sxs-lookup"><span data-stu-id="6b760-123">Recall the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

### <a name="update"></a><span data-ttu-id="6b760-124">Actualizar</span><span class="sxs-lookup"><span data-stu-id="6b760-124">Update</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="6b760-125">`Update` es similar a `Create`, pero usa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="6b760-125">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="6b760-126">La respuesta es [204 Sin contenido](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="6b760-126">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="6b760-127">Según la especificación HTTP, una solicitud PUT requiere que el cliente envíe toda la entidad actualizada, no solo los deltas.</span><span class="sxs-lookup"><span data-stu-id="6b760-127">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="6b760-128">Para admitir actualizaciones parciales, use HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="6b760-128">To support partial updates, use HTTP PATCH.</span></span>

![Consola de Postman que muestra la respuesta 204 Sin contenido](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="6b760-130">Eliminar</span><span class="sxs-lookup"><span data-stu-id="6b760-130">Delete</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="6b760-131">La respuesta es [204 Sin contenido](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="6b760-131">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![Consola de Postman que muestra la respuesta 204 Sin contenido](../../tutorials/first-web-api/_static/pmd.png)
