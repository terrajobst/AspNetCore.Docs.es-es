## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="0f702-101">Implementar las otras operaciones CRUD</span><span class="sxs-lookup"><span data-stu-id="0f702-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="0f702-102">En las secciones siguientes, se agregan los métodos `Create`, `Update` y `Delete` al controlador.</span><span class="sxs-lookup"><span data-stu-id="0f702-102">In the following sections, `Create`, `Update`, and `Delete` methods are added to the controller.</span></span>

### <a name="create"></a><span data-ttu-id="0f702-103">Crear</span><span class="sxs-lookup"><span data-stu-id="0f702-103">Create</span></span>

<span data-ttu-id="0f702-104">Agregue el siguiente método `Create`:</span><span class="sxs-lookup"><span data-stu-id="0f702-104">Add the following `Create` method:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="0f702-105">El código anterior es un método HTTP POST, según indica el atributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="0f702-105">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="0f702-106">El atributo [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) indica a MVC que obtenga el valor de la tarea pendiente del cuerpo de la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="0f702-106">The [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="0f702-107">El código anterior es un método HTTP POST, según indica el atributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="0f702-107">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="0f702-108">MVC obtiene el valor de la tarea pendiente del cuerpo de la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="0f702-108">MVC gets the value of the to-do item from the body of the HTTP request.</span></span>

::: moniker-end

<span data-ttu-id="0f702-109">El método `CreatedAtRoute` realiza las acciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="0f702-109">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="0f702-110">Devuelve una respuesta 201.</span><span class="sxs-lookup"><span data-stu-id="0f702-110">Returns a 201 response.</span></span> <span data-ttu-id="0f702-111">HTTP 201 es la respuesta estándar para un método HTTP POST que crea un recurso en el servidor.</span><span class="sxs-lookup"><span data-stu-id="0f702-111">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="0f702-112">Agrega un encabezado de ubicación a la respuesta.</span><span class="sxs-lookup"><span data-stu-id="0f702-112">Adds a Location header to the response.</span></span> <span data-ttu-id="0f702-113">El encabezado de ubicación especifica el URI de la tarea pendiente recién creada.</span><span class="sxs-lookup"><span data-stu-id="0f702-113">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="0f702-114">Vea [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (10.2.2 201 creada).</span><span class="sxs-lookup"><span data-stu-id="0f702-114">See [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="0f702-115">Usa la ruta denominada "GetTodo" para crear la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="0f702-115">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="0f702-116">La ruta con nombre "GetTodo" se define en `GetById`:</span><span class="sxs-lookup"><span data-stu-id="0f702-116">The "GetTodo" named route is defined in `GetById`:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="0f702-117">Usar Postman para enviar una solicitud de creación</span><span class="sxs-lookup"><span data-stu-id="0f702-117">Use Postman to send a Create request</span></span>

* <span data-ttu-id="0f702-118">Inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0f702-118">Start the app.</span></span>
* <span data-ttu-id="0f702-119">Abra Postman.</span><span class="sxs-lookup"><span data-stu-id="0f702-119">Open Postman.</span></span>

![Consola de Postman](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="0f702-121">Actualice el número de puerto en la dirección URL de localhost.</span><span class="sxs-lookup"><span data-stu-id="0f702-121">Update the port number in the localhost URL.</span></span>
* <span data-ttu-id="0f702-122">Establezca el método HTTP en *POST*.</span><span class="sxs-lookup"><span data-stu-id="0f702-122">Set the HTTP method to *POST*.</span></span>
* <span data-ttu-id="0f702-123">Haga clic en la pestaña **Body** (Cuerpo).</span><span class="sxs-lookup"><span data-stu-id="0f702-123">Click the **Body** tab.</span></span>
* <span data-ttu-id="0f702-124">Seleccione el botón de radio **Raw** (Sin formato).</span><span class="sxs-lookup"><span data-stu-id="0f702-124">Select the **raw** radio button.</span></span>
* <span data-ttu-id="0f702-125">Establezca el tipo en *JSON (application/json)*.</span><span class="sxs-lookup"><span data-stu-id="0f702-125">Set the type to *JSON (application/json)*.</span></span>
* <span data-ttu-id="0f702-126">Escriba un cuerpo de solicitud con una tarea pendiente parecida al siguiente JSON:</span><span class="sxs-lookup"><span data-stu-id="0f702-126">Enter a request body with a to-do item resembling the following JSON:</span></span>

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* <span data-ttu-id="0f702-127">Haga clic en el botón **Send** (Enviar).</span><span class="sxs-lookup"><span data-stu-id="0f702-127">Click the **Send** button.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!TIP]
> <span data-ttu-id="0f702-128">Si no aparece ninguna respuesta tras hacer clic en **Send** (Enviar), deshabilite la opción **SSL certification verification** (Comprobación de certificación SSL).</span><span class="sxs-lookup"><span data-stu-id="0f702-128">If no response displays after clicking **Send**, disable the **SSL certification verification** option.</span></span> <span data-ttu-id="0f702-129">La encontrará en **File** > **Settings** (Archivo > Configuración).</span><span class="sxs-lookup"><span data-stu-id="0f702-129">This is found under **File** > **Settings**.</span></span> <span data-ttu-id="0f702-130">Vuelva a hacer clic en el botón **Send** (Enviar) después de deshabilitar la configuración.</span><span class="sxs-lookup"><span data-stu-id="0f702-130">Click the **Send** button again after disabling the setting.</span></span>

::: moniker-end

<span data-ttu-id="0f702-131">Haga clic en la pestaña **Headers** (Encabezados) del panel **Response** (Respuesta) y copie el valor de encabezado de **Location** (Ubicación):</span><span class="sxs-lookup"><span data-stu-id="0f702-131">Click the **Headers** tab in the **Response** pane and copy the **Location** header value:</span></span>

![Pestaña Headers (Encabezados) de la consola de Postman](../../tutorials/first-web-api/_static/pmc2.png)

<span data-ttu-id="0f702-133">El URI del encabezado de ubicación puede utilizarse para acceder al nuevo elemento.</span><span class="sxs-lookup"><span data-stu-id="0f702-133">The Location header URI can be used to access the new item.</span></span>

### <a name="update"></a><span data-ttu-id="0f702-134">Actualizar</span><span class="sxs-lookup"><span data-stu-id="0f702-134">Update</span></span>

<span data-ttu-id="0f702-135">Agregue el siguiente método `Update`:</span><span class="sxs-lookup"><span data-stu-id="0f702-135">Add the following `Update` method:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

<span data-ttu-id="0f702-136">`Update` es similar a `Create`, salvo por el hecho de que usa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="0f702-136">`Update` is similar to `Create`, except it uses HTTP PUT.</span></span> <span data-ttu-id="0f702-137">La respuesta es [204 Sin contenido](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="0f702-137">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="0f702-138">Según la especificación HTTP, una solicitud PUT requiere que el cliente envíe toda la entidad actualizada, no solo los deltas.</span><span class="sxs-lookup"><span data-stu-id="0f702-138">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="0f702-139">Para admitir actualizaciones parciales, use HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="0f702-139">To support partial updates, use HTTP PATCH.</span></span>

<span data-ttu-id="0f702-140">Use Postman para actualizar el nombre de la tarea pendiente a "walk cat":</span><span class="sxs-lookup"><span data-stu-id="0f702-140">Use Postman to update the to-do item's name to "walk cat":</span></span>

![Consola de Postman que muestra la respuesta 204 Sin contenido](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="0f702-142">Eliminar</span><span class="sxs-lookup"><span data-stu-id="0f702-142">Delete</span></span>

<span data-ttu-id="0f702-143">Agregue el siguiente método `Delete`:</span><span class="sxs-lookup"><span data-stu-id="0f702-143">Add the following `Delete` method:</span></span>

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="0f702-144">La respuesta de `Delete` es [204 Sin contenido](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="0f702-144">The `Delete` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

<span data-ttu-id="0f702-145">Use Postman para eliminar la tarea pendiente:</span><span class="sxs-lookup"><span data-stu-id="0f702-145">Use Postman to delete the to-do item:</span></span>

![Consola de Postman que muestra la respuesta 204 Sin contenido](../../tutorials/first-web-api/_static/pmd.png)
