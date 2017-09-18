[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

El código anterior:

* Define una clase de controlador vacía. En las secciones siguientes, agregaremos métodos para implementar la API.
* El constructor usa la [inserción de dependencias](xref:fundamentals/dependency-injection) para insertar el contexto de base de datos (`TodoContext `) en el controlador. El contexto de base de datos se usa en cada uno de los métodos [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) del controlador.
* El constructor agrega un elemento a la base de datos en memoria si no existe ninguno.

## <a name="getting-to-do-items"></a>Obtención de tareas pendientes

Para obtener tareas pendientes, agregue estos métodos a la clase `TodoController`.

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

Estos métodos implementan los dos métodos GET:

* `GET /api/todo`
* `GET /api/todo/{id}`

Esta es una respuesta HTTP de ejemplo para el método `GetAll`:

```
HTTP/1.1 200 OK
   Content-Type: application/json; charset=utf-8
   Server: Microsoft-IIS/10.0
   Date: Thu, 18 Jun 2015 20:51:10 GMT
   Content-Length: 82

   [{"Key":"1", "Name":"Item1","IsComplete":false}]
   ```

Más adelante en el tutorial, mostraré cómo puede ver la respuesta HTTP mediante [Postman](https://www.getpostman.com/) o [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).

### <a name="routing-and-url-paths"></a>Enrutamiento y rutas URL

El atributo `[HttpGet]` especifica un método HTTP GET. La ruta de dirección URL para cada método se construye como sigue:

* Tome la cadena de plantilla en el atributo de ruta del controlador:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* Reemplace "[Controller]" por el nombre del controlador, que es el nombre de clase de controlador sin el sufijo "Controller". En este ejemplo, el nombre de clase de controlador es **Todo**Controller y el nombre de raíz es "todo". El [enrutamiento](xref:mvc/controllers/routing) en ASP.NET Core no distingue entre mayúsculas y minúsculas.
* Si el atributo `[HttpGet]` tiene una plantilla de ruta (como `[HttpGet("/products")]`), anexiónela a la ruta de acceso. En este ejemplo no se usa una plantilla. Para más información, vea [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) (Enrutamiento de atributos con atributos Http[Verb]).

En el método `GetById`:

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

`"{id}"` es una variable de marcador de posición para el identificador del elemento `todo`. Cuando se invoca `GetById`, asigna el valor de "{id}" en la URL al parámetro del método `id`.

`Name = "GetTodo"` crea una ruta con nombre y permite establecer un vínculo a esta ruta en una respuesta HTTP. Explicaré esto con un ejemplo más adelante. Para más información, vea [Routing to Controller Actions](xref:mvc/controllers/routing) (Enrutamiento a acciones del controlador).

### <a name="return-values"></a>Valores devueltos

El método `GetAll` devuelve un objeto `IEnumerable`. MVC serializa automáticamente el objeto a [JSON](http://www.json.org/) y escribe el JSON en el cuerpo del mensaje de respuesta. El código de respuesta para este método es 200, suponiendo que no haya ninguna excepción no controlada. (Las excepciones no controladas se convierten en errores 5xx).

En cambio, el método `GetById` devuelve el tipo más general `IActionResult`, que representa una amplia gama de tipos de valor devuelto. `GetById` tiene dos tipos de valor devueltos distintos:

* Si no hay ningún elemento que coincida con el identificador solicitado, el método devuelve un error 404.  Esto se hace devolviendo `NotFound`.

* En caso contrario, el método devuelve 200 con un cuerpo de respuesta JSON. Esto se hace devolviendo un `ObjectResult`
