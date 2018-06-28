
## <a name="test-the-app"></a>Prueba de la aplicación

* Ejecute la aplicación y pulse en el vínculo **Mvc Movie** (Película Mvc).
* Pulse **Create New** (Crear nueva) y cree una película.

  ![Crear vista con campos de género, precio, fecha de lanzamiento y título](~/tutorials/first-mvc-app/adding-model/_static/movies.png)

* Es posible no que pueda escribir comas ni puntos decimales en el campo `Price`. Para que la [validación de jQuery](https://jqueryvalidation.org/) sea compatible con configuraciones regionales distintas del inglés que usan una coma (",") en lugar de un punto decimal y formatos de fecha distintos del de Estados Unidos, debe seguir unos pasos para globalizar la aplicación. Para más información, vea [https://github.com/aspnet/Docs/issues/4076](https://github.com/aspnet/Docs/issues/4076) y [Recursos adicionales](#additional-resources). Por ahora, escriba solamente números enteros como 10.

<a name="displayformatdatelocal"></a>

* En algunas configuraciones regionales debe especificar el formato de fecha. Vea el código que aparece resaltado a continuación.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateFormat.cs?name=snippet_1&highlight=2,10)]

Hablaremos sobre `DataAnnotations` más adelante en el tutorial.

Al pulsar en **Crear** el formulario se envía al servidor, donde se guarda la información de la película en una base de datos. La aplicación redirige a la URL */Movies*, donde se muestra la información de la película que acaba de crear.

![Vista de películas que muestra la lista de películas que acaba de crear](~/tutorials/first-mvc-app/adding-model/_static/h.png)

Cree un par de entradas más de película. Compruebe que todos los vínculos **Editar**, **Detalles** y **Eliminar** son funcionales.
