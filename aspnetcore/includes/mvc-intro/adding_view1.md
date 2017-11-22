# <a name="adding-a-view-to-an-aspnet-core-mvc-app"></a>Adición de una vista en una aplicación de ASP.NET Core MVC

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

En esta sección, se modificará la clase `HelloWorldController` para usar los archivos de plantilla de vista Razor con el objetivo de encapsular correctamente el proceso de generar respuestas HTML a un cliente.

Para crear un archivo de plantilla de vista se usa Razor. Las plantillas de vista basadas en Razor tienen una extensión de archivo *.cshtml*. Ofrecen una forma elegante de crear un resultado HTML mediante C#.

Actualmente, el método `Index` devuelve una cadena con un mensaje que está codificado de forma rígida en la clase de controlador. En la clase `HelloWorldController`, reemplace el método `Index` por el siguiente código:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_4)]

El código anterior devuelve un objeto `View`. Usa una plantilla de vista para generar una respuesta HTML al explorador. Los métodos de controlador (también conocidos como métodos de acción), como el método `Index` anterior, suelen devolver [IActionResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iactionresult) o una clase derivada de `ActionResult`, en lugar de un tipo como una cadena.
