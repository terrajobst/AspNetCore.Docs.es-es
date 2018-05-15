## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="79104-101">Implementar las otras operaciones CRUD</span><span class="sxs-lookup"><span data-stu-id="79104-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="79104-102">En las secciones siguientes, se agregan los métodos `Create`, `Update` y `Delete` al controlador.</span><span class="sxs-lookup"><span data-stu-id="79104-102">In the following sections, `Create`, `Update`, and `Delete` methods are added to the controller.</span></span>

### <a name="create"></a><span data-ttu-id="79104-103">Crear</span><span class="sxs-lookup"><span data-stu-id="79104-103">Create</span></span>

<span data-ttu-id="79104-104">Agregue el siguiente método `Create`:</span><span class="sxs-lookup"><span data-stu-id="79104-104">Add the following `Create` method:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="79104-105">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="79104-105">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="79104-106">El código anterior es un método HTTP POST, según indica el atributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="79104-106">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="79104-107">El atributo [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) indica a MVC que obtenga el valor de la tarea pendiente del cuerpo de la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="79104-107">The [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="79104-108">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="79104-108">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="79104-109">El código anterior es un método HTTP POST, según indica el atributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="79104-109">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="79104-110">MVC obtiene el valor de la tarea pendiente del cuerpo de la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="79104-110">MVC gets the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end

<span data-ttu-id="79104-111">El método `CreatedAtRoute` realiza las acciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="79104-111">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="79104-112">Devuelve una respuesta 201.</span><span class="sxs-lookup"><span data-stu-id="79104-112">Returns a 201 response.</span></span> <span data-ttu-id="79104-113">HTTP 201 es la respuesta estándar para un método HTTP POST que crea un recurso en el servidor.</span><span class="sxs-lookup"><span data-stu-id="79104-113">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="79104-114">Agrega un encabezado de ubicación a la respuesta.</span><span class="sxs-lookup"><span data-stu-id="79104-114">Adds a Location header to the response.</span></span> <span data-ttu-id="79104-115">El encabezado de ubicación especifica el URI de la tarea pendiente recién creada.</span><span class="sxs-lookup"><span data-stu-id="79104-115">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="79104-116">Vea [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (10.2.2 201 creada).</span><span class="sxs-lookup"><span data-stu-id="79104-116">See [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="79104-117">Usa la ruta denominada "GetTodo" para crear la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="79104-117">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="79104-118">La ruta con nombre "GetTodo" se define en `GetById`:</span><span class="sxs-lookup"><span data-stu-id="79104-118">The "GetTodo" named route is defined in `GetById`:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="79104-119">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span><span class="sxs-lookup"><span data-stu-id="79104-119">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="79104-120">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span><span class="sxs-lookup"><span data-stu-id="79104-120">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span></span>
::: moniker-end

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="79104-121">Usar Postman para enviar una solicitud de creación</span><span class="sxs-lookup"><span data-stu-id="79104-121">Use Postman to send a Create request</span></span>

* <span data-ttu-id="79104-122">Inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="79104-122">Start the app.</span></span>
* <span data-ttu-id="79104-123">Abra Postman.</span><span class="sxs-lookup"><span data-stu-id="79104-123">Open Postman.</span></span>

![Consola de Postman](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="79104-125">Actualice el número de puerto en la dirección URL de localhost.</span><span class="sxs-lookup"><span data-stu-id="79104-125">Update the port number in the localhost URL.</span></span>
* <span data-ttu-id="79104-126">Establezca el método HTTP en *POST*.</span><span class="sxs-lookup"><span data-stu-id="79104-126">Set the HTTP method to *POST*.</span></span>
* <span data-ttu-id="79104-127">Haga clic en la pestaña **Body** (Cuerpo).</span><span class="sxs-lookup"><span data-stu-id="79104-127">Click the **Body** tab.</span></span>
* <span data-ttu-id="79104-128">Seleccione el botón de radio **Raw** (Sin formato).</span><span class="sxs-lookup"><span data-stu-id="79104-128">Select the **raw** radio button.</span></span>
* <span data-ttu-id="79104-129">Establezca el tipo en *JSON (application/json)*.</span><span class="sxs-lookup"><span data-stu-id="79104-129">Set the type to *JSON (application/json)*.</span></span>
* <span data-ttu-id="79104-130">Escriba un cuerpo de solicitud con una tarea pendiente parecida al siguiente JSON:</span><span class="sxs-lookup"><span data-stu-id="79104-130">Enter a request body with a to-do item resembling the following JSON:</span></span>

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* <span data-ttu-id="79104-131">Haga clic en el botón **Send** (Enviar).</span><span class="sxs-lookup"><span data-stu-id="79104-131">Click the **Send** button.</span></span>

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> <span data-ttu-id="79104-132">Si no aparece ninguna respuesta tras hacer clic en **Send** (Enviar), deshabilite la opción **SSL certification verification** (Comprobación de certificación SSL).</span><span class="sxs-lookup"><span data-stu-id="79104-132">If no response displays after clicking **Send**, disable the **SSL certification verification** option.</span></span> <span data-ttu-id="79104-133">La encontrará en **File** > **Settings** (Archivo > Configuración).</span><span class="sxs-lookup"><span data-stu-id="79104-133">This is found under **File** > **Settings**.</span></span> <span data-ttu-id="79104-134">Vuelva a hacer clic en el botón **Send** (Enviar) después de deshabilitar la configuración.</span><span class="sxs-lookup"><span data-stu-id="79104-134">Click the **Send** button again after disabling the setting.</span></span>
::: moniker-end

<span data-ttu-id="79104-135">Haga clic en la pestaña **Headers** (Encabezados) del panel **Response** (Respuesta) y copie el valor de encabezado de **Location** (Ubicación):</span><span class="sxs-lookup"><span data-stu-id="79104-135">Click the **Headers** tab in the **Response** pane and copy the **Location** header value:</span></span>

![Pestaña Headers (Encabezados) de la consola de Postman](../../tutorials/first-web-api/_static/pmc2.png)

<span data-ttu-id="79104-137">El URI del encabezado de ubicación puede utilizarse para acceder al nuevo elemento.</span><span class="sxs-lookup"><span data-stu-id="79104-137">The Location header URI can be used to access the new item.</span></span>

### <a name="update"></a><span data-ttu-id="79104-138">Actualizar</span><span class="sxs-lookup"><span data-stu-id="79104-138">Update</span></span>

<span data-ttu-id="79104-139">Agregue el siguiente método `Update`:</span><span class="sxs-lookup"><span data-stu-id="79104-139">Add the following `Update` method:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="79104-140">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="79104-140">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="79104-141">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="79104-141">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>
::: moniker-end

<span data-ttu-id="79104-142">`Update` es similar a `Create`, salvo por el hecho de que usa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="79104-142">`Update` is similar to `Create`, except it uses HTTP PUT.</span></span> <span data-ttu-id="79104-143">La respuesta es [204 Sin contenido](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="79104-143">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="79104-144">Según la especificación HTTP, una solicitud PUT requiere que el cliente envíe toda la entidad actualizada, no solo los deltas.</span><span class="sxs-lookup"><span data-stu-id="79104-144">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="79104-145">Para admitir actualizaciones parciales, use HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="79104-145">To support partial updates, use HTTP PATCH.</span></span>

<span data-ttu-id="79104-146">Use Postman para actualizar el nombre de la tarea pendiente a "walk cat":</span><span class="sxs-lookup"><span data-stu-id="79104-146">Use Postman to update the to-do item's name to "walk cat":</span></span>

![Consola de Postman que muestra la respuesta 204 Sin contenido](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="79104-148">Eliminar</span><span class="sxs-lookup"><span data-stu-id="79104-148">Delete</span></span>

<span data-ttu-id="79104-149">Agregue el siguiente método `Delete`:</span><span class="sxs-lookup"><span data-stu-id="79104-149">Add the following `Delete` method:</span></span>

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="79104-150">La respuesta de `Delete` es [204 Sin contenido](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="79104-150">The `Delete` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

<span data-ttu-id="79104-151">Use Postman para eliminar la tarea pendiente:</span><span class="sxs-lookup"><span data-stu-id="79104-151">Use Postman to delete the to-do item:</span></span>

![Consola de Postman que muestra la respuesta 204 Sin contenido](../../tutorials/first-web-api/_static/pmd.png)
