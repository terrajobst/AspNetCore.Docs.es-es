## <a name="call-the-web-api-with-jquery"></a>Llamar a Web API con jQuery

En esta sección, se agrega una página HTML que usa jQuery para llamar a Web API. jQuery inicia la solicitud y actualiza la página con los detalles de la respuesta de la API.

Configure el proyecto para atender archivos estáticos y para permitir la asignación de archivos predeterminada. Esto se logra invocando los métodos de extensión [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) y [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) en *Startup.Configure*. Para más información, vea [Trabajar con archivos estáticos en ASP.NET Core](xref:fundamentals/static-files).

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup2.cs?name=snippet_Configure&highlight=3-4)]

Agregue un archivo HTML denominado *index.html* al directorio *wwwroot* del proyecto. Reemplace el contenido por el siguiente marcado:

[!code-html[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/index.html)]

Agregue un archivo JavaScript denominado *site.js* al directorio *wwwroot* del proyecto. Reemplace el contenido por el siguiente código:

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

Puede que sea necesario realizar un cambio en la configuración de inicio del proyecto de ASP.NET Core para comprobar la página HTML localmente. Abra *launchSettings.json* en el directorio *Properties* del proyecto. Quite la propiedad `launchUrl` para forzar a la aplicación a abrirse en *index.html*, esto es, el archivo predeterminado del proyecto.

Existen varias formas de obtener jQuery. En el fragmento de código anterior, la biblioteca se carga desde una red CDN. Este ejemplo es un ejemplo de CRUD completo de llamada a la API de jQuery. Existen más características en este ejemplo para hacer que la experiencia sea más completa. Aquí verá algunas explicaciones sobre las llamadas a la API.

### <a name="get-a-list-of-to-do-items"></a>Obtener una lista de tareas pendientes

Para obtener una lista de tareas pendientes, envíe una solicitud HTTP GET a */api/todo*.

La función de JQuery [ajax](https://api.jquery.com/jquery.ajax/) envía una solicitud AJAX a la API, que devuelve código JSON que representa un objeto o una matriz. Esta función puede controlar todas las formas de interacción de HTTP enviando una solicitud HTTP a la `url` especificada. `GET` se emite como `type`. La función de devolución de llamada `success` se invoca si la solicitud se realiza correctamente. En la devolución de llamada, el DOM se actualiza con la información de la tarea pendiente.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a>Agregar una tarea pendiente

Para agregar una tarea pendiente, envíe una solicitud HTTP POST a */api/todo*. El cuerpo de la solicitud debe contener un objeto de tarea pendiente. La función [ajax](https://api.jquery.com/jquery.ajax/) usa `POST` para llamar a la API. En las solicitudes `POST` y `PUT`, el cuerpo de la solicitud representa los datos enviados a la API. La API espera un cuerpo de solicitud con formato JSON. Las opciones `accepts` y `contentType` se establecen en `application/json` para clasificar el tipo de medio que se va a recibir y a enviar respectivamente. Los datos se convierten en un objeto JSON usando [`JSON.stringify`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify). Cuando la API devuelve un código de estado correcto, se invoca la función `getData` para actualizar la tabla HTML.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a>Actualizar una tarea pendiente

Actualizar una tarea pendiente es muy parecido a agregarla, ya que ambos procesos se basan en el cuerpo de solicitud. En este caso, la única diferencia real entre ambos es que `url` cambia para reflejar el identificador único del elemento, y `type` es `PUT`.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a>Eliminar una tarea pendiente

Para eliminar una tarea pendiente, hay que establecer `type` de la llamada de AJAX en `DELETE` y especificar el identificador único de la tarea en la dirección URL.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]
