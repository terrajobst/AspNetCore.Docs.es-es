## <a name="implement-the-other-crud-operations"></a>Implementar las otras operaciones CRUD

En las secciones siguientes, se agregan los métodos `Create`, `Update` y `Delete` al controlador.

### <a name="create"></a>Crear

Agregue el siguiente método `Create`:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

El código anterior es un método HTTP POST, según indica el atributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute). El atributo [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) indica a MVC que obtenga el valor de la tarea pendiente del cuerpo de la solicitud HTTP.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

El código anterior es un método HTTP POST, según indica el atributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute). MVC obtiene el valor de la tarea pendiente del cuerpo de la solicitud HTTP.
::: moniker-end

El método `CreatedAtRoute` realiza las acciones siguientes:

* Devuelve una respuesta 201. HTTP 201 es la respuesta estándar para un método HTTP POST que crea un recurso en el servidor.
* Agrega un encabezado de ubicación a la respuesta. El encabezado de ubicación especifica el URI de la tarea pendiente recién creada. Vea [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (10.2.2 201 creada).
* Usa la ruta denominada "GetTodo" para crear la dirección URL. La ruta con nombre "GetTodo" se define en `GetById`:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end

### <a name="use-postman-to-send-a-create-request"></a>Usar Postman para enviar una solicitud de creación

* Inicia la aplicación.
* Abra Postman.

![Consola de Postman](../../tutorials/first-web-api/_static/pmc.png)

* Actualice el número de puerto en la dirección URL de localhost.
* Establezca el método HTTP en *POST*.
* Haga clic en la pestaña **Body** (Cuerpo).
* Seleccione el botón de radio **Raw** (Sin formato).
* Establezca el tipo en *JSON (application/json)*.
* Escriba un cuerpo de solicitud con una tarea pendiente parecida al siguiente JSON:

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* Haga clic en el botón **Send** (Enviar).

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> Si no aparece ninguna respuesta tras hacer clic en **Send** (Enviar), deshabilite la opción **SSL certification verification** (Comprobación de certificación SSL). La encontrará en **File** > **Settings** (Archivo > Configuración). Vuelva a hacer clic en el botón **Send** (Enviar) después de deshabilitar la configuración.
::: moniker-end

Haga clic en la pestaña **Headers** (Encabezados) del panel **Response** (Respuesta) y copie el valor de encabezado de **Location** (Ubicación):

![Pestaña Headers (Encabezados) de la consola de Postman](../../tutorials/first-web-api/_static/pmc2.png)

El URI del encabezado de ubicación puede utilizarse para acceder al nuevo elemento.

### <a name="update"></a>Actualizar

Agregue el siguiente método `Update`:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end

`Update` es similar a `Create`, salvo por el hecho de que usa HTTP PUT. La respuesta es [204 Sin contenido](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Según la especificación HTTP, una solicitud PUT requiere que el cliente envíe toda la entidad actualizada, no solo los deltas. Para admitir actualizaciones parciales, use HTTP PATCH.

Use Postman para actualizar el nombre de la tarea pendiente a "walk cat":

![Consola de Postman que muestra la respuesta 204 Sin contenido](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a>Eliminar

Agregue el siguiente método `Delete`:

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

La respuesta de `Delete` es [204 Sin contenido](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

Use Postman para eliminar la tarea pendiente:

![Consola de Postman que muestra la respuesta 204 Sin contenido](../../tutorials/first-web-api/_static/pmd.png)
