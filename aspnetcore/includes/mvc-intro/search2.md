<!--
[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](../../tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

El método `Index` anterior:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_1stSearch)]

El método `Index` actualizado con el parámetro `id`:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_SearchID)]

Ahora puede pasar el título de la búsqueda como datos de ruta (un segmento de dirección URL) en lugar de como un valor de cadena de consulta.

![Vista de índice con la palabra Ghost (Fantasma) agregada a la dirección URL y una lista de las películas obtenidas, con dos películas, Ghostbusters y Ghostbusters 2](../../tutorials/first-mvc-app/search/_static/g2.png)

Sin embargo, no se puede esperar que los usuarios modifiquen la dirección URL cada vez que quieran buscar una película. Por lo tanto, ahora deberá agregar la interfaz de usuario con la que podrán filtrar las películas. Si cambió la firma del método `Index` para probar cómo pasar el parámetro `ID` enlazado a una ruta, vuelva a cambiarlo para que tome un parámetro denominado `searchString`:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_1stSearch)]

Abra el archivo *Views/Movies/Index.cshtml* y agregue el marcado `<form>` resaltado a continuación:

[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexForm1.cshtml?highlight=10-16&range=4-21)]

La etiqueta HTML `<form>` usa la [aplicación auxiliar de etiquetas de formulario](../../mvc/views/working-with-forms.md), por lo que cuando se envía el formulario, la cadena de filtro se registra en la acción `Index` del controlador de películas. Guarde los cambios y después pruebe el filtro.

![Vista de índice con la palabra Ghost (Fantasma) escrita en el cuadro de texto del filtro Title (Título)](../../tutorials/first-mvc-app/search/_static/filter.png)

No hay ninguna sobrecarga `[HttpPost]` del método `Index` como cabría esperar. No es necesario, porque el método no cambia el estado de la aplicación, simplemente filtra los datos.

Después, puede agregar el método `[HttpPost] Index` siguiente.

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_SearchPost)]

El parámetro `notUsed` se usa para crear una sobrecarga para el método `Index`. Hablaremos sobre esto más adelante en el tutorial.

Si agrega este método, el invocador de acción coincidiría con el método `[HttpPost] Index`, mientras que el método `[HttpPost] Index` se ejecutaría tal como se muestra en la imagen de abajo.

![Ventana del explorador con la respuesta: From HttpPost Index: filter on ghost (Desde el índice HttpPost: filtrar en Ghost)](../../tutorials/first-mvc-app/search/_static/fo.png)

Sin embargo, aunque agregue esta versión de `[HttpPost]` al método `Index`, hay una limitación en cómo se ha implementado todo esto. Supongamos que quiere marcar una búsqueda en particular o que quiere enviar un vínculo a sus amigos donde puedan hacer clic para ver la misma lista filtrada de películas. Tenga en cuenta que la dirección URL de la solicitud HTTP POST es la misma que la dirección URL de la solicitud GET (localhost:xxxxx/Movies/Index): no hay información de búsqueda en la URL. La información de la cadena de búsqueda se envía al servidor como un [valor de campo de formulario](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data). Puede comprobarlo con las herramientas de desarrollo del explorador o con la excelente [herramienta Fiddler](http://www.telerik.com/fiddler). En la imagen de abajo se muestran las herramientas de desarrollo del explorador Chrome:

![Pestaña Red de las herramientas de desarrollo en Microsoft Edge, que muestra un cuerpo de solicitud con un valor searchString de la palabra Ghost](../../tutorials/first-mvc-app/search/_static/f12_rb.png)

Puede ver el parámetro de búsqueda y el token [XSRF](../../security/anti-request-forgery.md) en el cuerpo de la solicitud. Tenga en cuenta, como se mencionó en el tutorial anterior, que la [aplicación auxiliar de etiquetas de formulario](../../mvc/views/working-with-forms.md) genera un token [XSRF](../../security/anti-request-forgery.md) antifalsificación. Como no se van a modificar datos, no es necesario validar el token con el método del controlador.

El parámetro de búsqueda se encuentra en el cuerpo de solicitud y no en la dirección URL. Por eso no se puede capturar dicha información para marcarla o compartirla con otros usuarios. Para solucionar este problema, especificaremos que la solicitud sea `HTTP GET`.
