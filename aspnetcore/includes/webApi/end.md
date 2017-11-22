## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="ccc62-101">Implementar las otras operaciones CRUD</span><span class="sxs-lookup"><span data-stu-id="ccc62-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="ccc62-102">En las secciones siguientes, se agregan los métodos `Create`, `Update` y `Delete` al controlador.</span><span class="sxs-lookup"><span data-stu-id="ccc62-102">In the following sections, `Create`, `Update`, and `Delete` methods are added to the controller.</span></span>

### <a name="create"></a><span data-ttu-id="ccc62-103">Crear</span><span class="sxs-lookup"><span data-stu-id="ccc62-103">Create</span></span>

<span data-ttu-id="ccc62-104">Agregue el siguiente método `Create`.</span><span class="sxs-lookup"><span data-stu-id="ccc62-104">Add the following `Create` method.</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="ccc62-105">El código anterior es un método HTTP POST, indicado por el atributo [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="ccc62-105">The preceding code is an HTTP POST method, indicated by the [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="ccc62-106">El atributo [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) indica a MVC que obtenga el valor de la tarea pendiente del cuerpo de la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="ccc62-106">The [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="ccc62-107">El método `CreatedAtRoute` realiza las acciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="ccc62-107">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="ccc62-108">Devuelve una respuesta 201.</span><span class="sxs-lookup"><span data-stu-id="ccc62-108">Returns a 201 response.</span></span> <span data-ttu-id="ccc62-109">HTTP 201 es la respuesta estándar para un método HTTP POST que crea un recurso en el servidor.</span><span class="sxs-lookup"><span data-stu-id="ccc62-109">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="ccc62-110">Agrega un encabezado de ubicación a la respuesta.</span><span class="sxs-lookup"><span data-stu-id="ccc62-110">Adds a Location header to the response.</span></span> <span data-ttu-id="ccc62-111">El encabezado de ubicación especifica el URI de la tarea pendiente recién creada.</span><span class="sxs-lookup"><span data-stu-id="ccc62-111">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="ccc62-112">Vea [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (10.2.2 201 creada).</span><span class="sxs-lookup"><span data-stu-id="ccc62-112">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="ccc62-113">Usa la ruta denominada "GetTodo" para crear la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="ccc62-113">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="ccc62-114">La ruta con nombre "GetTodo" se define en `GetById`:</span><span class="sxs-lookup"><span data-stu-id="ccc62-114">The "GetTodo" named route is defined in `GetById`:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="ccc62-115">Usar Postman para enviar una solicitud de creación</span><span class="sxs-lookup"><span data-stu-id="ccc62-115">Use Postman to send a Create request</span></span>

![Consola de Postman](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="ccc62-117">Establezca el método HTTP en `POST`.</span><span class="sxs-lookup"><span data-stu-id="ccc62-117">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="ccc62-118">Seleccione el botón de radio **Body** (Cuerpo).</span><span class="sxs-lookup"><span data-stu-id="ccc62-118">Select the **Body** radio button</span></span>
* <span data-ttu-id="ccc62-119">Seleccione el botón de radio **Raw** (Sin formato).</span><span class="sxs-lookup"><span data-stu-id="ccc62-119">Select the **raw** radio button</span></span>
* <span data-ttu-id="ccc62-120">Establezca el tipo en JSON.</span><span class="sxs-lookup"><span data-stu-id="ccc62-120">Set the type to JSON</span></span>
* <span data-ttu-id="ccc62-121">En el editor de pares clave-valor, escriba una tarea pendiente como la siguiente:</span><span class="sxs-lookup"><span data-stu-id="ccc62-121">In the key-value editor, enter a Todo item such as</span></span>

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="ccc62-122">Seleccione **Send** (Enviar).</span><span class="sxs-lookup"><span data-stu-id="ccc62-122">Select **Send**</span></span>
* <span data-ttu-id="ccc62-123">Seleccione la pestaña Headers (Encabezados) en el panel inferior y copie el encabezado **Location** (Ubicación):</span><span class="sxs-lookup"><span data-stu-id="ccc62-123">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Pestaña Headers (Encabezados) de la consola de Postman](../../tutorials/first-web-api/_static/pmget.png)

<span data-ttu-id="ccc62-125">El URI del encabezado de ubicación puede utilizarse para acceder al nuevo elemento.</span><span class="sxs-lookup"><span data-stu-id="ccc62-125">The Location header URI can be used to access the new item.</span></span>

### <a name="update"></a><span data-ttu-id="ccc62-126">Actualizar</span><span class="sxs-lookup"><span data-stu-id="ccc62-126">Update</span></span>

<span data-ttu-id="ccc62-127">Agregue el siguiente método `Update`:</span><span class="sxs-lookup"><span data-stu-id="ccc62-127">Add the following `Update` method:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="ccc62-128">`Update` es similar a `Create`, pero usa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="ccc62-128">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="ccc62-129">La respuesta es [204 Sin contenido](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="ccc62-129">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="ccc62-130">Según la especificación HTTP, una solicitud PUT requiere que el cliente envíe toda la entidad actualizada, no solo los deltas.</span><span class="sxs-lookup"><span data-stu-id="ccc62-130">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="ccc62-131">Para admitir actualizaciones parciales, use HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="ccc62-131">To support partial updates, use HTTP PATCH.</span></span>

![Consola de Postman que muestra la respuesta 204 Sin contenido](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="ccc62-133">Eliminar</span><span class="sxs-lookup"><span data-stu-id="ccc62-133">Delete</span></span>

<span data-ttu-id="ccc62-134">Agregue el siguiente método `Delete`:</span><span class="sxs-lookup"><span data-stu-id="ccc62-134">Add the following `Delete` method:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="ccc62-135">La respuesta de `Delete` es [204 Sin contenido](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="ccc62-135">The `Delete` response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

<span data-ttu-id="ccc62-136">Probar `Delete`:</span><span class="sxs-lookup"><span data-stu-id="ccc62-136">Test `Delete`:</span></span> 

![Consola de Postman que muestra la respuesta 204 Sin contenido](../../tutorials/first-web-api/_static/pmd.png)
