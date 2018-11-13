::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

El código anterior define una clase de controlador de API sin métodos. En las secciones siguientes, se agregan métodos para implementar la API.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

El código anterior:

* Define una clase de controlador de API sin métodos.
* Crea una tarea pendiente cuando `TodoItems` está vacío. No podrá eliminar todas las tareas pendientes porque el constructor crea una si `TodoItems` está vacío.

En las secciones siguientes, se agregan métodos para implementar la API. La clase se anota con un atributo `[ApiController]` para habilitar algunas características muy prácticas. Para más información sobre las características que el atributo habilita, vea [Anotación con ApiControllerAttribute](xref:web-api/index#annotation-with-apicontrollerattribute).

::: moniker-end

El constructor del controlador usa la [inserción de dependencias](xref:fundamentals/dependency-injection) para insertar el contexto de base de datos (`TodoContext`) en el controlador. El contexto de base de datos se usa en cada uno de los métodos [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) del controlador. El constructor agrega un elemento a la base de datos en memoria si no existe ninguno.

## <a name="get-to-do-items"></a>Tareas pendientes

Para obtener tareas pendientes, agregue estos métodos a la clase `TodoController`:

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

::: moniker-end

Estos métodos implementan los dos métodos GET:

* `GET /api/todo`
* `GET /api/todo/{id}`

Esta es una respuesta HTTP de ejemplo del método `GetAll`:

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

Más adelante en el tutorial, veremos cómo se puede ver la respuesta HTTP por medio de [Postman](https://www.getpostman.com/) o [curl](https://curl.haxx.se/docs/manpage.html).

### <a name="routing-and-url-paths"></a>Enrutamiento y rutas URL

El atributo `[HttpGet]` indica un método que responde a una solicitud HTTP GET. La ruta de dirección URL para cada método se construye como sigue:

* Tome la cadena de plantilla en el atributo `Route` del controlador:

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

::: moniker-end

* Reemplace `[controller]` por el nombre del controlador, que por convención es el nombre de clase de controlador sin el sufijo "Controller". En este ejemplo, el nombre de clase de controlador es **Todo**Controller y el nombre de raíz es "todo". El [enrutamiento](xref:mvc/controllers/routing) en ASP.NET Core no distingue entre mayúsculas y minúsculas.
* Si el atributo `[HttpGet]` tiene una plantilla de ruta (como `[HttpGet("/products")]`), anexiónela a la ruta de acceso. En este ejemplo no se usa una plantilla. Para más información, vea [Enrutamiento mediante atributos con atributos Http[Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).

En el siguiente método `GetById`, `"{id}"` es una variable de marcador de posición correspondiente al identificador único de la tarea pendiente. Cuando `GetById` se invoca, asigna el valor `"{id}"` de la dirección URL al parámetro `id` del método.

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

`Name = "GetTodo"` crea una ruta con nombre. Rutas con nombre:

* Permita que la aplicación cree un vínculo HTTP mediante el nombre de ruta.
* Se explican más adelante en el tutorial.

### <a name="return-values"></a>Valores devueltos

El método `GetAll` devuelve una colección de objetos `TodoItem`. MVC serializa automáticamente el objeto a [JSON](https://www.json.org/) y escribe el JSON en el cuerpo del mensaje de respuesta. El código de respuesta para este método es 200, suponiendo que no haya ninguna excepción no controlada. Las excepciones no controladas se convierten en errores 5xx.

::: moniker range="<= aspnetcore-2.0"

En cambio, el método `GetById` devuelve el tipo más general [IActionResult](xref:web-api/action-return-types#iactionresult-type), que representa una amplia gama de tipos de valor devuelto. `GetById` tiene dos tipos de valor devueltos distintos:

* Si no hay ningún elemento que coincida con el identificador solicitado, el método devuelve un error 404. Devolver [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) genera una respuesta HTTP 404.
* En caso contrario, el método devuelve 200 con un cuerpo de respuesta JSON. Devolver [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) genera una respuesta HTTP 200.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

En cambio, el método `GetById` devuelve el tipo [ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type), que representa una amplia gama de tipos de valor devuelto. `GetById` tiene dos tipos de valor devueltos distintos:

* Si no hay ningún elemento que coincida con el identificador solicitado, el método devuelve un error 404. Devolver [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) genera una respuesta HTTP 404.
* En caso contrario, el método devuelve 200 con un cuerpo de respuesta JSON. Devolver `item` genera una respuesta HTTP 200.

::: moniker-end
