## <a name="implement-the-other-crud-operations"></a>Implementar las otras operaciones CRUD

Vamos a agregar los métodos `Create`, `Update` y `Delete` al controlador. Son variaciones de un tema, así que solo mostraré el código y comentaré las diferencias principales. Compile el proyecto después de agregar o cambiar el código.

### <a name="create"></a>Crear

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Se trata de un método HTTP POST, indicado por el atributo [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute). El atributo [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) indica a MVC que obtenga el valor de la tarea pendiente del cuerpo de la solicitud HTTP.

El método `CreatedAtRoute` devuelve una respuesta 201, que es la respuesta estándar para un método HTTP POST que crea un nuevo recurso en el servidor. `CreatedAtRoute` también agrega un encabezado de ubicación a la respuesta. El encabezado de ubicación especifica el URI de la tarea pendiente recién creada. Vea [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (10.2.2 201 creada).

### <a name="use-postman-to-send-a-create-request"></a>Usar Postman para enviar una solicitud de creación

![Consola de Postman](../../tutorials/first-web-api/_static/pmc.png)

* Establezca el método HTTP en `POST`.
* Seleccione el botón de radio **Body** (Cuerpo).
* Seleccione el botón de radio **Raw** (Sin formato).
* Establezca el tipo en JSON.
* En el editor de pares clave-valor, escriba una tarea pendiente como la siguiente: 

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* Seleccione **Send** (Enviar).

* Seleccione la pestaña Headers (Encabezados) en el panel inferior y copie el encabezado **Location** (Ubicación):

![Pestaña Headers (Encabezados) de la consola de Postman](../../tutorials/first-web-api/_static/pmget.png)

Puede usar el URI del encabezado Location (Ubicación) para acceder al recurso que acaba de crear. Recuerde que el método `GetById` creó la ruta denominada `"GetTodo"`:

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

### <a name="update"></a>Actualizar

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

`Update` es similar a `Create`, pero usa HTTP PUT. La respuesta es [204 Sin contenido](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Según la especificación HTTP, una solicitud PUT requiere que el cliente envíe toda la entidad actualizada, no solo los deltas. Para admitir actualizaciones parciales, use HTTP PATCH.

![Consola de Postman que muestra la respuesta 204 Sin contenido](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a>Eliminar

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

La respuesta es [204 Sin contenido](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

![Consola de Postman que muestra la respuesta 204 Sin contenido](../../tutorials/first-web-api/_static/pmd.png)
