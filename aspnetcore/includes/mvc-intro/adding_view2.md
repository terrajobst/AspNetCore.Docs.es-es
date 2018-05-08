Reemplace el contenido del archivo de vista de Razor *Views/HelloWorld/Index.cshtml* con lo siguiente:

[!code-HTML[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Index.cshtml)]

Navegue a `http://localhost:xxxx/HelloWorld`. El método `Index` en `HelloWorldController` no hizo mucho; ejecutó la instrucción `return View();`, que especificaba que el método debe usar un archivo de plantilla de vista para representar una respuesta al explorador. Como no especificó expresamente el nombre del archivo de plantilla de vista, MVC usó de manera predeterminada el archivo de vista *Index.cshtml* de la carpeta */Views/HelloWorld*. La imagen siguiente muestra la cadena "Hello from our View Template!" (Hola desde nuestra plantilla de vista) codificada de forma rígida en la vista.

![Ventana del explorador](../../tutorials/first-mvc-app/adding-view/_static/hell_template.png)

Si la ventana del explorador es pequeña (por ejemplo en un dispositivo móvil), es conveniente que alterne (pulse) el [botón de navegación de arranque](http://getbootstrap.com/components/#navbar) en la esquina superior derecha para ver los vínculos **Home** (Inicio), **About** (Acerca de) y **Contact** (Contacto).

![Ventana del explorador donde se resalta el botón de navegación de arranque](../../tutorials/first-mvc-app/adding-view/_static/1.png)

## <a name="changing-views-and-layout-pages"></a>Cambiar vistas y páginas de diseño

Pulse los vínculos de menú: **MvcMovie** (Película de MVC), **Home** (Inicio), **About** (Acerca de). Cada página muestra el mismo diseño de menú. El diseño de menú se implementa en el archivo *Views/Shared/_Layout.cshtml*. Abra el archivo *Views/Shared/_Layout.cshtml*.

Las plantillas de [diseño](xref:mvc/views/layout) permiten especificar el diseño del contenedor HTML del sitio en un solo lugar y, después, aplicarlo en varias páginas del sitio. Busque la línea `@RenderBody()`. `RenderBody` es un marcador de posición donde se mostrarán todas las páginas específicas de vista que cree, *encapsuladas* en la página de diseño. Por ejemplo, si selecciona el vínculo **About** (Acerca de), la vista **Views/Home/About.cshtml** se representa dentro del método `RenderBody`.

## <a name="change-the-title-and-menu-link-in-the-layout-file"></a>Cambiar el título y el vínculo del menú en el archivo de diseño

En el elemento de título, cambie `MvcMovie` por `Movie App`. Cambie el texto del delimitador en la plantilla de diseño de `MvcMovie` a `Movie App` y el controlador de `Home` a `Movies` como se resalta aquí:

Nota: La versión ASP.NET Core 2.0 es algo diferente. No contiene `@inject ApplicationInsights` ni `@Html.Raw(JavaScriptSnippet.FullScript)`.

[!code-html[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]

>[!WARNING]
> Aún no hemos implementado el controlador `Movies`, por lo que si hace clic en ese vínculo, obtendrá un error 404 (no encontrado).

Guarde los cambios y pulse en el vínculo **About** (Acerca de). Observe cómo el título de la pestaña del explorador muestra ahora **About - Movie App** (Acerca de - Aplicación de película) en lugar de **About - Mvc Movie** (Acerca de - Aplicación de MVC): 

![Acerca de la pestaña](../../tutorials/first-mvc-app/adding-view/_static/about2.png)

Pulse el vínculo **Contacto** y observe que el texto del título y el delimitador también muestran **Movie App**. Hemos realizado el cambio una vez en la plantilla de diseño y hemos conseguido que todas las páginas del sitio reflejen el nuevo texto de vínculo y el nuevo título.

Examine el archivo *Views/_ViewStart.cshtml*:


```HTML
@{
    Layout = "_Layout";
}
```

El archivo *Views/_ViewStart.cshtml* trae el archivo *Views/Shared/_Layout.cshtml* a cada vista. Puede usar la propiedad `Layout` para establecer una vista de diseño diferente o establecerla en `null` para que no se use ningún archivo de diseño.

Cambie el título de la vista `Index`.

Abra *Views/HelloWorld/Index.cshtml*. Los cambios se pueden hacer en dos lugares:

   * El texto que aparece en el título del explorador.
   * El encabezado secundario (elemento `<h2>`).

Haremos que sean algo diferentes para que pueda ver qué parte del código cambia cada área de la aplicación.


```HTML
@{
    ViewData["Title"] = "Movie List";
}

<h2>My Movie List</h2>

<p>Hello from our View Template!</p>
```

En el código anterior, `ViewData["Title"] = "Movie List";` establece la propiedad `Title` del diccionario `ViewData` en "Movie List" (Lista de películas). La propiedad `Title` se usa en el elemento HTML `<title>` en la página de diseño:


```HTML
<title>@ViewData["Title"] - Movie App</title>
   ```

Guarde el cambio y navegue a `http://localhost:xxxx/HelloWorld`. Tenga en cuenta que el título del explorador, el encabezado principal y los encabezados secundarios han cambiado. (Si no ve los cambios en el explorador, es posible que esté viendo contenido almacenado en caché. Presione Ctrl+F5 en el explorador para forzar que se cargue la respuesta del servidor). El título del explorador se crea con `ViewData["Title"]`, que se definió en la plantilla de vista *Index.cshtml* y el texto "- Movie App" (-Aplicación de película) que se agregó en el archivo de diseño.

Observe también cómo el contenido de la plantilla de vista *Index.cshtml* se fusionó con la plantilla de vista *Views/Shared/_Layout.cshtml* y se envió una única respuesta HTML al explorador. Con las plantillas de diseño es realmente fácil hacer cambios para que se apliquen en todas las páginas de la aplicación. Para saber más, vea [Layout](xref:mvc/views/layout) (Diseño).

![Vista de lista de películas](../../tutorials/first-mvc-app/adding-view/_static/hell3.png)

Nuestra pequeña cantidad de "datos", en este caso, el mensaje "Hello from our View Template!" (Hola desde nuestra plantilla de vista), están codificados de forma rígida. La aplicación de MVC tiene una "V" (vista) y ha obtenido una "C" (controlador), pero todavía no tiene una "M" (modelo).

## <a name="passing-data-from-the-controller-to-the-view"></a>Pasar datos del controlador a la vista

Las acciones del controlador se invocan en respuesta a una solicitud de dirección URL entrante. Una clase de controlador es donde se escribe el código que controla las solicitudes entrantes del explorador. El controlador recupera datos de un origen de datos y decide qué tipo de respuesta devolverá al explorador. Las plantillas de vista se pueden usar desde un controlador para generar y dar formato a una respuesta HTML al explorador.

Los controladores se encargan de proporcionar los datos necesarios para que una plantilla de vista represente una respuesta. Procedimiento recomendado: las plantillas de vista **no** deben realizar lógica de negocios ni interactuar directamente con una base de datos. En su lugar, una plantilla de vista debe funcionar solo con los datos que le proporciona el controlador. Mantener esta "separación de intereses" ayuda a mantener el código limpio, fácil de probar y de mantener.

Actualmente, el método `Welcome` de la clase `HelloWorldController` toma un parámetro `name` y `ID`, y luego obtiene los valores directamente en el explorador. En lugar de que el controlador represente esta respuesta como una cadena, cambie el controlador para que use una plantilla de vista. La plantilla de vista genera una respuesta dinámica, lo que significa que se deben pasar las partes de datos adecuadas desde el controlador a la vista para que se genere la respuesta. Para hacerlo, indique al controlador que coloque los datos dinámicos (parámetros) que necesita la plantilla de vista en un diccionario `ViewData` al que luego pueda obtener acceso la plantilla de vista.

Vuelva al archivo *HelloWorldController.cs* y cambie el método `Welcome` para agregar un valor `Message` y `NumTimes` al diccionario `ViewData`. El diccionario `ViewData` es un objeto dinámico, lo que significa que puede colocar en él todo lo que quiera; el objeto `ViewData` no tiene ninguna propiedad definida hasta que coloca algo dentro de él. El [sistema de enlace de modelos](xref:mvc/models/model-binding) de MVC asigna automáticamente los parámetros con nombre (`name` y `numTimes`) de la cadena de consulta en la barra de dirección a los parámetros del método. El archivo *HelloWorldController.cs* completo tiene este aspecto:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_5)]

El objeto de diccionario `ViewData` contiene datos que se pasarán a la vista. 

Cree una plantilla de vista principal denominada *Views/HelloWorld/Welcome.cshtml*.

Se creará un bucle en la vista *Welcome.cshtml* que muestra "Hello" (Hola) `NumTimes`. Reemplace el contenido de *Views/HelloWorld/Welcome.cshtml* con lo siguiente:

[!code-html[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Welcome.cshtml)]

Guarde los cambios y vaya a esta dirección URL:

`http://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

Los datos se toman de la dirección URL y se pasan al controlador mediante el [enlazador de modelos MVC](xref:mvc/models/model-binding). El controlador empaqueta los datos en un diccionario `ViewData` y pasa ese objeto a la vista. Después, la vista representa los datos como HTML en el explorador.

![Vista About (Acerca de) que muestra una etiqueta Welcome (Bienvenida) y la frase "Hello Rick" (Hola Rick) cuatro veces](../../tutorials/first-mvc-app/adding-view/_static/rick2.png)

En el ejemplo anterior, usamos el diccionario `ViewData` para pasar datos del controlador a una vista. Más adelante en el tutorial usaremos un modelo de vista para pasar datos de un controlador a una vista. El enfoque del modelo de vista que consiste en pasar datos suele ser más preferible que el enfoque de diccionario `ViewData`. Para saber más, vea [ViewModel vs ViewData vs ViewBag vs TempData vs Session in MVC](http://www.mytecbits.com/microsoft/dot-net/viewmodel-viewdata-viewbag-tempdata-mvc) (ViewModel, ViewData, ViewBag, TempData y Session en MVC).

Bueno, todo esto era un tipo de "M" para el modelo, pero no el tipo de base de datos. Vamos a aprovechar lo que hemos aprendido para crear una base de datos de películas.
