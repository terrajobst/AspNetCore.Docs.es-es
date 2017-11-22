## <a name="implement-the-other-crud-operations"></a>Implementar las otras operaciones CRUD

En las secciones siguientes, se agregan los métodos `Create`, `Update` y `Delete` al controlador.

### <a name="create"></a>Crear

Agregue el siguiente método `Create`.

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

El código anterior es un método HTTP POST, indicado por el atributo [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute). El atributo [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) indica a MVC que obtenga el valor de la tarea pendiente del cuerpo de la solicitud HTTP.

El método `CreatedAtRoute` realiza las acciones siguientes:

* Devuelve una respuesta 201. HTTP 201 es la respuesta estándar para un método HTTP POST que crea un recurso en el servidor.
* Agrega un encabezado de ubicación a la respuesta. El encabezado de ubicación especifica el URI de la tarea pendiente recién creada. Vea [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (10.2.2 201 creada).
* Usa la ruta denominada "GetTodo" para crear la dirección URL. La ruta con nombre "GetTodo" se define en `GetById`:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

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

El URI del encabezado de ubicación puede utilizarse para acceder al nuevo elemento.

### <a name="update"></a>Actualizar

Agregue el siguiente método `Update`:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

`Update` es similar a `Create`, pero usa HTTP PUT. La respuesta es [204 Sin contenido](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Según la especificación HTTP, una solicitud PUT requiere que el cliente envíe toda la entidad actualizada, no solo los deltas. Para admitir actualizaciones parciales, use HTTP PATCH.

![Consola de Postman que muestra la respuesta 204 Sin contenido](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a>Eliminar

Agregue el siguiente método `Delete`:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

La respuesta de `Delete` es [204 Sin contenido](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

Probar `Delete`: 

![Consola de Postman que muestra la respuesta 204 Sin contenido](../../tutorials/first-web-api/_static/pmd.png)
