Reemplace el contenido de *Controllers/HelloWorldController.cs* con lo siguiente:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_1)]

Cada método `public` en un controlador puede ser invocado como un punto de conexión HTTP. En el ejemplo anterior, ambos métodos devuelven una cadena.  Observe los comentarios delante de cada método.

Un extremo HTTP es una dirección URL que se puede usar como destino en la aplicación web, como por ejemplo `http://localhost:1234/HelloWorld`. Combina el protocolo usado `HTTP`, la ubicación de red del servidor web (incluido el puerto TCP) `localhost:1234` y el URI de destino `HelloWorld`.

El primer comentario dice que se trata de un método [HTTP GET](https://www.w3schools.com/tags/ref_httpmethods.asp) que se invoca mediante la anexión de "/HelloWorld/" a la dirección URL base. El segundo comentario dice que se trata de un método [HTTP GET](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) que se invoca mediante la anexión de "/HelloWorld/Welcome/" a la dirección URL. Más adelante en el tutorial usaremos el motor de scaffolding para generar métodos `HTTP POST`.

Ejecute la aplicación en modo de no depuración y anexione "HelloWorld" a la ruta de acceso en la barra de direcciones. El método `Index` devuelve una cadena.

![Ventana del explorador que muestra una respuesta de la aplicación a "This is my default action" (Esta es mi acción predeterminada)](../../tutorials/first-mvc-app/adding-controller/_static/hell1.png)

MVC invoca las clases del controlador (y los métodos de acción que contienen) en función de la URL entrante. La [lógica de enrutamiento de URL](../../mvc/controllers/routing.md) predeterminada que usa MVC emplea un formato similar al siguiente para determinar qué código se debe invocar:

`/[Controller]/[ActionName]/[Parameters]`

El formato para el enrutamiento se establece en el método `Configure` en *Startup.cs*.

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

Cuando se ejecuta la aplicación y no se suministra ningún segmento de dirección URL, de manera predeterminada se usa el controlador "Home" y el método "Index" especificados en la línea de plantilla resaltada arriba.

El primer segmento de dirección URL determina la clase de controlador que se va a ejecutar. De modo que `localhost:xxxx/HelloWorld` se asigna a la clase `HelloWorldController`. La segunda parte del segmento de dirección URL determina el método de acción en la clase. De modo que `localhost:xxxx/HelloWorld/Index` podría provocar que se ejecute el método `Index` de la clase `HelloWorldController`. Tenga en cuenta que solo es necesario navegar a `localhost:xxxx/HelloWorld` para que se llame al método `Index` de manera predeterminada. Esto es porque `Index` es el método predeterminado al que se llamará en un controlador si no se especifica explícitamente un nombre de método. La tercera parte del segmento de dirección URL (`id`) es para los datos de ruta. Veremos los datos de ruta más adelante en este tutorial.

Vaya a `http://localhost:xxxx/HelloWorld/Welcome`. El método `Welcome` se ejecuta y devuelve la cadena "This is the Welcome action method..." (Este es el método de acción de bienvenida). Para esta dirección URL, el controlador es `HelloWorld` y `Welcome` es el método de acción. Todavía no ha usado el elemento `[Parameters]` de la dirección URL.

![Ventana del explorador que muestra la respuesta de la aplicación "This is the Welcome action method" (Este es el método de acción predeterminado)](../../tutorials/first-mvc-app/adding-controller/_static/welcome.png)

Modifique el código para pasar cierta información del parámetro desde la dirección URL al controlador. Por ejemplo: `/HelloWorld/Welcome?name=Rick&numtimes=4`. Cambie el método `Welcome` para que incluya dos parámetros, como se muestra en el código siguiente. 

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_2)]

El código anterior:

* Usa la característica de parámetro opcional de C# para indicar que el parámetro `numTimes` tiene el valor predeterminado 1 si no se pasa ningún valor para ese parámetro.
* Usa `HtmlEncoder.Default.Encode` para proteger la aplicación de entradas malintencionadas (es decir, JavaScript). 
* Usa [cadenas interpoladas](https://docs.microsoft.com/dotnet/articles/csharp/language-reference/keywords/interpolated-strings).

Ejecute la aplicación y navegue a:

   `http://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

(Reemplace xxxx con el número de puerto). Puede probar valores diferentes para `name` y `numtimes` en la dirección URL. El sistema de [enlace de modelos](../../mvc/models/model-binding.md) de MVC asigna automáticamente los parámetros con nombre de la cadena de consulta en la barra de dirección a los parámetros del método. Vea [Model Binding](../../mvc/models/model-binding.md) (Enlace de modelos) para más información.

![Ventana del explorador que muestra una respuesta de la aplicación a "Hello Rick, NumTimes is: 4" (Hola Rick, NumTimes es: 4)](../../tutorials/first-mvc-app/adding-controller/_static/rick4.png)

En la ilustración anterior, el segmento de dirección URL (`Parameters`) no se usa, y los parámetros `name` y `numTimes` se pasan como [cadenas de consulta](https://wikipedia.org/wiki/Query_string). El `?` (signo de interrogación) en la dirección URL anterior es un separador y le siguen las cadenas de consulta. El carácter `&` separa las cadenas de consulta.

Reemplace el método `Welcome` con el código siguiente:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_3)]

Ejecute la aplicación y escriba la dirección URL siguiente: `http://localhost:xxx/HelloWorld/Welcome/3?name=Rick`

![Ventana del explorador que muestra una respuesta de la aplicación a "Hello Rick, ID: 3" (Hola Rick, Id.: 3)](../../tutorials/first-mvc-app/adding-controller/_static/rick_routedata.png)

Esta vez el tercer segmento de dirección URL coincide con el parámetro de ruta `id`. El método `Welcome` contiene un parámetro `id` que coincide con la plantilla de dirección URL en el método `MapRoute`. El `?` del final (en `id?`) indica que el parámetro `id` es opcional.

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

En estos ejemplos, el controlador ha realizado la parte "VC" de MVC, es decir, el trabajo de vista y de controlador. El controlador devuelve HTML directamente. Por lo general, no es aconsejable que los controles devuelvan HTML directamente, porque resulta muy complicado de programar y mantener. En su lugar, se suele usar un archivo de plantilla de vista de Razor independiente para ayudar a generar la respuesta HTML. Haremos esto en el siguiente tutorial.
