## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="cac88-101">Implementar las otras operaciones CRUD</span><span class="sxs-lookup"><span data-stu-id="cac88-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="cac88-102">Vamos a agregar los métodos `Create`, `Update` y `Delete` al controlador.</span><span class="sxs-lookup"><span data-stu-id="cac88-102">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="cac88-103">Son variaciones de un tema, así que solo mostraré el código y comentaré las diferencias principales.</span><span class="sxs-lookup"><span data-stu-id="cac88-103">These are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="cac88-104">Compile el proyecto después de agregar o cambiar el código.</span><span class="sxs-lookup"><span data-stu-id="cac88-104">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="cac88-105">Crear</span><span class="sxs-lookup"><span data-stu-id="cac88-105">Create</span></span>

<span data-ttu-id="cac88-106">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="cac88-106">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="cac88-107">Se trata de un método HTTP POST, indicado por el atributo [`[HttpPost]`](https://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/HttpPostAttribute/index.html).</span><span class="sxs-lookup"><span data-stu-id="cac88-107">This is an HTTP POST method, indicated by the [`[HttpPost]`](https://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/HttpPostAttribute/index.html) attribute.</span></span> <span data-ttu-id="cac88-108">El atributo [`[FromBody]`](https://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/FromBodyAttribute/index.html) indica a MVC que obtenga el valor de la tarea pendiente del cuerpo de la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="cac88-108">The [`[FromBody]`](https://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/FromBodyAttribute/index.html) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="cac88-109">El método `CreatedAtRoute` devuelve una respuesta 201, que es la respuesta estándar para un método HTTP POST que crea un nuevo recurso en el servidor.</span><span class="sxs-lookup"><span data-stu-id="cac88-109">The `CreatedAtRoute` method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="cac88-110">`CreatedAtRoute` también agrega un encabezado de ubicación a la respuesta.</span><span class="sxs-lookup"><span data-stu-id="cac88-110">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="cac88-111">El encabezado de ubicación especifica el URI de la tarea pendiente recién creada.</span><span class="sxs-lookup"><span data-stu-id="cac88-111">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="cac88-112">Vea [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (10.2.2 201 creada).</span><span class="sxs-lookup"><span data-stu-id="cac88-112">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="cac88-113">Usar Postman para enviar una solicitud de creación</span><span class="sxs-lookup"><span data-stu-id="cac88-113">Use Postman to send a Create request</span></span>

![Consola de Postman](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="cac88-115">Establezca el método HTTP en `POST`.</span><span class="sxs-lookup"><span data-stu-id="cac88-115">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="cac88-116">Seleccione el botón de radio **Body** (Cuerpo).</span><span class="sxs-lookup"><span data-stu-id="cac88-116">Select the **Body** radio button</span></span>
* <span data-ttu-id="cac88-117">Seleccione el botón de radio **Raw** (Sin formato).</span><span class="sxs-lookup"><span data-stu-id="cac88-117">Select the **raw** radio button</span></span>
* <span data-ttu-id="cac88-118">Establezca el tipo en JSON.</span><span class="sxs-lookup"><span data-stu-id="cac88-118">Set the type to JSON</span></span>
* <span data-ttu-id="cac88-119">En el editor de pares clave-valor, escriba una tarea pendiente como la siguiente:</span><span class="sxs-lookup"><span data-stu-id="cac88-119">In the key-value editor, enter a Todo item such as</span></span> 

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="cac88-120">Seleccione **Send** (Enviar).</span><span class="sxs-lookup"><span data-stu-id="cac88-120">Select **Send**</span></span>

* <span data-ttu-id="cac88-121">Seleccione la pestaña Headers (Encabezados) en el panel inferior y copie el encabezado **Location** (Ubicación):</span><span class="sxs-lookup"><span data-stu-id="cac88-121">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Pestaña Headers (Encabezados) de la consola de Postman](../../tutorials/first-web-api/_static/pmget.png)

<span data-ttu-id="cac88-123">Puede usar el URI del encabezado Location (Ubicación) para acceder al recurso que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="cac88-123">You can use the Location header URI to access the resource you just created.</span></span> <span data-ttu-id="cac88-124">Recuerde que el método `GetById` creó la ruta denominada `"GetTodo"`:</span><span class="sxs-lookup"><span data-stu-id="cac88-124">Recall the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

### <a name="update"></a><span data-ttu-id="cac88-125">Actualizar</span><span class="sxs-lookup"><span data-stu-id="cac88-125">Update</span></span>

<span data-ttu-id="cac88-126">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="cac88-126">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>

<span data-ttu-id="cac88-127">`Update` es similar a `Create`, pero usa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="cac88-127">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="cac88-128">La respuesta es [204 Sin contenido](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="cac88-128">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="cac88-129">Según la especificación HTTP, una solicitud PUT requiere que el cliente envíe toda la entidad actualizada, no solo los deltas.</span><span class="sxs-lookup"><span data-stu-id="cac88-129">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="cac88-130">Para admitir actualizaciones parciales, use HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="cac88-130">To support partial updates, use HTTP PATCH.</span></span>

![Consola de Postman que muestra la respuesta 204 Sin contenido](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="cac88-132">Eliminar</span><span class="sxs-lookup"><span data-stu-id="cac88-132">Delete</span></span>

<span data-ttu-id="cac88-133">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]</span><span class="sxs-lookup"><span data-stu-id="cac88-133">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]</span></span>

<span data-ttu-id="cac88-134">La respuesta es [204 Sin contenido](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="cac88-134">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![Consola de Postman que muestra la respuesta 204 Sin contenido](../../tutorials/first-web-api/_static/pmd.png)
